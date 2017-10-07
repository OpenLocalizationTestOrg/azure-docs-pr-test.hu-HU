---
title: "aaaImprove oszlopcentrikus index teljesítmény az Azure SQL |} Microsoft Docs"
description: "Csökkentse a, vagy az oszlopcentrikus index tömöríti az egyes sorcsoport sorok hello rendelkezésre álló memória toomaximize hello számának növeléséhez."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 6/2/2017
ms.author: shigu;barbkess
ms.openlocfilehash: 2c5a68435aa200236a2dc8538aa4638b52a59093
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a>Az oszlopcentrikus maximalizálva sorcsoport minősége

Sorcsoport minőségi hello egy sorcsoport sorainak száma határozza meg. Csökkentse a, vagy az oszlopcentrikus index tömöríti az egyes sorcsoport sorok hello rendelkezésre álló memória toomaximize hello számának növeléséhez.  Használja az ezen módszerek tooimprove tömörítési sebességét és a lekérdezési teljesítmény az oszlopcentrikus indexek.

## <a name="why-hello-rowgroup-size-matters"></a>Hello sorcsoport mérete fontossága
Mert oszlopcentrikus index táblázat egyedi rowgroups oszlop szegmenseinek beolvasásával ellenőrzi, maximalizálva hello minden sorcsoport sorainak száma fokozza a lekérdezések teljesítményét. Ha rowgroups nagy mennyiségű sort, adattömörítés javítja, ami azt jelenti, hogy a kevesebb adat tooread lemezről van.

Rowgroups kapcsolatos további információkért lásd: [Oszlopcentrikus indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx).

## <a name="target-size-for-rowgroups"></a>Rowgroups célméretet
A legjobb lekérdezési teljesítmény elérése érdekében a hello célja toomaximize hello kötegenkénti sorok száma az oszlopcentrikus index sorcsoport. Egy sorcsoport legfeljebb 1 048 576 sort is rendelkezhetnek. Annak ellenére toonot rendelkezik hello / sorcsoport sorok maximális számát. Oszloptárindexek a megfelelő teljesítmény elérése érdekében, ha rowgroups legalább 100 000 sort.

## <a name="rowgroups-can-get-trimmed-during-compression"></a>Rowgroups is beolvasása levágja a tömörítés során

A tömeges betöltés vagy oszlopcentrikus index rebuild, során néha nem áll rendelkezésre elegendő memória rendelkezésre álló toocompress minden egyes sorcsoport kijelölt sorok hello. Memóriaprobléma esetén, oszlopcentrikus indexek hello sorcsoport méretek vághatja a, tömörítés hello oszlopcentrikus való sikeres lehet. 

Ha nincs elegendő memória toocompress legalább 10 000 sorokat minden sorcsoport, az SQL Data Warehouse hibát generál.

További információ a tömeges betöltés: [fürtözött oszlopcentrikus index a tömeges betöltés](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).

## <a name="how-toomonitor-rowgroup-quality"></a>Hogyan toomonitor sorcsoport minősége

Nincs a hasznos információk, például a tisztítás rowgroups és hello okát a sorok számát mutatja, ha nem lett díszítésre DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats). Létrehozhat egy hasznos tooquery nézetként következő hello a DMV tooget információk sorcsoport tisztítás.

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[distribution_id]    = nt.[distribution_id]                                          
)
select *
from cte;
```

hello trim_reason_desc közli, hogy hello sorcsoport lett rövidített (trim_reason_desc = NO_TRIM azt jelenti, nem tisztítás történt, és sorcsoport optimális minőségű). hello következő vágással kapcsolatos okok miatt jelzi, hogy hello sorcsoport korai karakterek:
- BULKLOAD: Vágás ezért használatos hello bejövő hello terhelést-sorok kötegét kisebb, mint 1 millió sort foglalnak volna. hello motor tömörített sorcsoportok hoz létre, ha nagyobb, mint 100 000 sort beszúrni (a megakadályozását tooinserting hello különbözeti tárolóba), de beállítása hello vágás OK tooBULKLOAD. Ebben az esetben érdemes megfontolni a kötegelt betöltés ablak tooaccumulate több sort. Ezenkívül újra kiértékelje a particionálási sémát tooensure nincs túl részletes, sorcsoportok partícióhatárok nem terjedhet ki.
- MEMORY_LIMITATION: hello motor toocreate sorcsoportok az 1 millió sort foglalnak, bizonyos mennyiségű működő memória szükséges. Hello munkamenet betöltése a rendelkezésre álló memória értéke kisebb, mint a hello szükséges memória használata, ha a get túl korán rövidített sorcsoportok. hello a következő szakaszok azt ismertetik, hogyan tooestimate memória szükséges, és több memóriát lefoglalni.
- DICTIONARY_SIZE: Vágás ezért azt jelzi, hogy sorcsoport tisztítás történt, mert legalább egy karakterlánc-oszlopnak széles és/vagy nagy számosságot karakterláncok történt. hello szótár mérete MB memória, és ez a korlát elérésekor hello sorcsoport tömörített korlátozott too16. Ha ez a helyzet tapasztal, fontolja meg, hello problematikus oszlop azoknak egy külön táblába.

## <a name="how-tooestimate-memory-requirements"></a>Hogyan tooestimate memóriára vonatkozó követelményeknek.

<!--
tooview an estimate of hello memory requirements toocompress a rowgroup of maximum size into a columnstore index, download and run hello view [dbo.vCS_mon_mem_grant](). This view shows hello size of hello memory grant that a rowgroup requires for compression in toohello columnstore.
-->

hello maximális szükséges memória toocompress egy sorcsoport körülbelül van

- 72 MB +
- \#sorok \* \#oszlopok \* 8 bájt +
- \#sorok \* \#rövid-karakterlánc-oszlopok \* 32 bájt +
- \#hosszú-karakterlánc-oszlopok \* tömörítési 16 MB

Ha rövid-karakterlánc-oszlopok használnak, a karakterláncos adattípusokkal < = 32 bájt és karakterláncos adattípusokkal hosszú-karakterlánc-oszlopok használata > 32 bájtban kifejezve.

Hosszú karakterláncok szöveges tömörítés készült tömörítési módszer tömörített. Ez a tömörítés módszer egy *szótár* toostore szövegminták. hello maximális dictionary mérete 16 MB. Csak egy szótár hello sorcsoport hosszú karakterlánc oszlopainak van.

Részletes ismertető oszlopcentrikus memória követelmények, lásd: a videó [Azure SQL Data Warehouse skálázás: útmutató és konfigurációs](https://myignite.microsoft.com/videos/14822).

## <a name="ways-tooreduce-memory-requirements"></a>Többféleképpen tooreduce memóriára vonatkozó követelményeknek

A következő módszerek tooreduce hello memóriakövetelményei rowgroups tömörítés az oszlopcentrikus indexek hello használata.

### <a name="use-fewer-columns"></a>Oszlopok
Ha lehetséges tervezze meg kevesebb oszlopot hello tábla. Amikor egy sorcsoport hello oszlopcentrikus van tömörített, hello oszlopcentrikus index külön-külön tömöríti minden oszlop szegmensben. Ezért hello toocompress egy sorcsoport oszlopok növekszik hello számának növelése memóriára vonatkozó követelményeknek.


### <a name="use-fewer-string-columns"></a>Oszlopok karakterlánc
String adattípusú oszlopok numerikus-nál több memóriát és dátum adattípus van szükség. tooreduce memóriára vonatkozó követelményeknek, vegye figyelembe, karakterlánc típusú oszlopokra eltávolítása a ténytáblák és abba a kisebb dimenziótáblák.

További memóriára vonatkozó követelményeknek karakterlánc tömörítési:

- Karakterláncos adattípusokkal mentése too32 karakterek 32 további bájt értéke lehet szükség.
- Karakterláncos adattípusokkal legfeljebb 32 karakter hosszú lehet a tömörített szótár módszerekkel.  Minden egyes oszlopának hello sorcsoport tooan további 16 MB toobuild hello szótár fel lehet szükség.

### <a name="avoid-over-partitioning"></a>Kerülje a túlzott particionálás

Oszlopcentrikus indexek létrehozása egy vagy több rowgroups partíciónként. Az SQL Data Warehouse hello partíciók száma nő gyorsan mert hello adatok terjeszt, és minden egyes terjesztési particionálva van. Ha hello tábla túl sok partíciókkal rendelkezik, a nem feltétlenül elég sorok toofill hello rowgroups. sorok hello hiánya nem hoz létre Memóriaterhelést tömörítés során, de toorowgroups, hogy ne használjon a legjobb oszlopcentrikus lekérdezési teljesítmény hello vezet.

Egy másik OK tooavoid túlzott particionálás, a memória esetén a particionált tábla oszlopcentrikus index a sorok betöltése. A betöltés során sok partíciót kap a hello bejövő sorok, a memóriában lévő mindaddig, amíg minden partíció elég tömörített sorok toobe rendelkezik. További memória rendelkező túl sok partíciót hoz létre.

### <a name="simplify-hello-load-query"></a>Hello terhelés lekérdezés egyszerűsítése

hello adatbázis megosztások hello memória biztosítása közötti összes hello operátorok hello lekérdezés lekérdezés. Ha egy terhelés-lekérdezés összetett típusú és illesztések rendelkezik, hello memória tömörítési csökken.

Tervezze meg hello terhelés lekérdezés toofocus csak a hello lekérdezés betöltésekor. Ha toorun átalakítások hello adatokra van szüksége, futtatásukhoz külön hello terhelés lekérdezésből. Például a szakasz hello adatok egy halommemóriában tábla hello átalakítások futtassa, és majd betölteni az előkészítési tábla hello oszlopcentrikus indexet a hello. Is először az hello adatok betöltése, és kövesse a hello MPP rendszer tootransform hello adatokat.

### <a name="adjust-maxdop"></a>MAXDOP beállítása

Minden terjesztési tömöríti rowgroups hello oszlopcentrikus párhuzamosan be, ha egynél több Processzormagok érhető el) eloszlása feladatonként. hello párhuzamossági nagyobb mennyiségű memóriát használ, ami toomemory nyomás és sorcsoport levágási igényel.

tooreduce Memóriaterhelést, hello MAXDOP lekérdezés mutató tooforce hello load műveletet toorun használhatja belül minden terjesztési soros módban.

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-tooallocate-more-memory"></a>Többféleképpen tooallocate további memória

DWU méretének és hello felhasználói erőforrásosztály együtt határozzák meg, mennyi memória érhető el a felhasználó lekérdezése. tooincrease hello memória hello dwu-k számának növelése vagy hello erőforrásosztály növelje a terhelés lekérdezés megadása.

- tooincrease hello dwu-k, lásd: [hogyan méretezhető teljesítmény?](sql-data-warehouse-manage-compute-overview.md#scale-compute)
- toochange hello erőforrásosztály lekérdezéshez, lásd: [módosíthatja a felhasználói erőforrás osztály példa](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).

Például a DWU 100 hello smallrc erőforrásosztály felhasználójának használhatja 100 MB memóriát minden egyes terjesztési. Hello részletekért lásd: [az SQL Data Warehouse párhuzamossági](sql-data-warehouse-develop-concurrency.md).

Tegyük fel, akkor határozza meg, hogy szükséges-e 700 MB memória tooget kiváló minőségű sorcsoport méretű. Ezek a példák azt szemléltetik, hogyan futtathat hello terhelés lekérdezés elegendő memóriával.

- A DWU 1000 és mediumrc, a memóriabeli ideiglenes 800 MB
- Használja a DWU 600 és largerc, a memóriabeli ideiglenes tárat, 800 MB.


## <a name="next-steps"></a>Következő lépések

toofind további módszereket tooimprove teljesítmény az SQL Data Warehouse, lásd: hello [teljesítményének áttekintését](sql-data-warehouse-overview-manage-user-queries.md).

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->

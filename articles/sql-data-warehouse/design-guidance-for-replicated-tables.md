---
title: "Útmutató a aaaDesign replikált táblák - Azure SQL Data Warehouse |} Microsoft Docs"
description: "Tervezésével kapcsolatos ajánlások replikált táblák az Azure SQL Data Warehouse sémában."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/14/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 5d405b8c404c65177b387ba959126839c1cf8799
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a>Útmutató a replikált táblák használata az Azure SQL Data Warehouse tervezése
Ez a cikk tervezése során az SQL Data Warehouse-sémát a replikált táblák ajánlásokat biztosít. A javaslatok tooimprove lekérdezési teljesítmény csökkenhet az adatok mozgása és lekérdezés összetettsége használatához.

> [!NOTE]
> hello replikált tábla funkció jelenleg nyilvános előzetes verziójához. Bizonyos viselkedéseinek tulajdonos toochange.
> 

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk azt feltételezi, hogy az adatok terjesztési és az SQL Data Warehouse adatátviteli – fogalmak megismerése.  További információkért lásd: [adatok elosztott](sql-data-warehouse-distributed-data.md). 

Táblatervezés részeként megérteni a lehető legnagyobb mértékben az adatokat, és hogyan hello adatok le kell kérdezni.  Tegyük fel ezeket a kérdéseket:

- Mekkora hello tábla?   
- Milyen gyakran frissülnek, hello tábla?   
- Van tény-és dimenziótáblák az adatraktárban?   

## <a name="what-is-a-replicated-table"></a>Mi az a replikált tábla?
A replikált tábla rendelkezzen minden számítási csomópont hello tábla elérhető teljes másolata. A tábla replikálása eltávolítja hello kell tootransfer join vagy összesítés előtt a számítási csomópontok között. Mivel hello tábla csak különböző példányainak, replikált táblák legjobban, ha hello tábla mérete kisebb, mint 2 GB tömörített.

hello következő diagramon láthatók a replikált tábla által elérhető minden számítási csomóponton. Az SQL Data Warehouse hello replikált tábla teljesen másolt tooa terjesztési adatbázis minden számítási csomóponton. 

![Replikált tábla](media/guidance-for-using-replicated-tables/replicated-table.png "replikált tábla")  

Replikált táblák munkahelyi csillagséma kis dimenzió táblák esetén. Dimenziótáblák általában, amely megkönnyíti a megvalósítható toostore méretű és különböző példányainak karbantartása. Dimenziók adattárolásra leíró megváltoztató lassan, például a felhasználói nevét és címét és a termék részletei. hello lassan módosítása hello adatok jellege vezet toofewer újraépíti hello replikált tábla. 

Fontolja meg a replikált tábla esetén:

- hello tábla lemez mérete 2 GB-nál kevesebb, függetlenül hello sorok száma. egy tábla toofind hello méretét, hello használható [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) parancs: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`. 
- hello tábla szolgál, amelyek egyébként adatmozgás illesztésekben. Például az illesztés kivonatoló elosztott táblákon adatátvitelt jelölik a szükséges, ha hello csatlakozó oszlopok nem vannak hello azonos terjesztési oszlop. Ha hello kivonatoló elosztott tábla kicsi, fontolja meg a replikált tábla. Ciklikus multiplexelés táblán illesztés adatmozgás igényel. Replikált táblák helyett a legtöbb esetben ciklikus multiplexelés táblák használatát javasoljuk. 


Vegye figyelembe a replikált tábla tooa konvertálása a meglévő elosztott táblából:

- Lekérdezés-használja az adatátviteli műveletek hello adatok tooall hello számítási csomópontok szórási csomagok. hello BroadcastMoveOperation drága, és csökkenti a lekérdezés teljesítményét. tooview az adatátviteli műveletek a lekérdezésterveket, használjon [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).
 
Replikált táblák nem fed fel hello legjobb lekérdezési teljesítmény során:

- hello táblához gyakori beszúrási, frissítési és törlési műveletek. Ezen adatok adatkezelési nyelv működéséhez szükségesek egy újjáépítését hello replikált tábla. Újraépítése gyakran okozhat lassabb teljesítményre.
- hello adatraktár gyakran méretezett. Adatraktár skálázás módosítja a hello számítási csomópontok száma, ami azt eredményezi azok háromszorosa egy Újraépítés.
- hello tábla nagy számú oszlopot tartalmaz, de az műveleteket általában hozzáférni csak kis számú oszlopot. Ebben a forgatókönyvben, ahelyett, hogy a teljes táblázat hello, előfordulhat, hogy hatékonyabb toohash hello tábla terjesztése, majd hozza létre az index a gyakran használt hello oszlopokra. Ha egy lekérdezéshez adatmozgás, az SQL Data Warehouse csak hello helyezi át adatokat kért oszlopok. 



## <a name="use-replicated-tables-with-simple-query-predicates"></a>Replikált táblák használata egyszerű lekérdezés predikátumok
Mielőtt toodistribute válassza vagy a tábla replikálása, gondolja át azt tervezi, hogy hello táblázaton toorun hello lekérdezéstípusok. Amikor csak lehetséges –

- Az egyszerű lekérdezés predikátumok, például egyenlőség vagy egyenlőtlen Lekérdezések replikált táblák használja.
- Az összetett lekérdezések predikátumok, például a hasonló lekérdezések elosztott táblázatot használni, vagy nincs MEGELÉGEDVE.

Processzorigényes lekérdezések legjobb elvégezni, amikor hello munkahelyi oszlik el minden hello számítási csomópont. Például egy táblázat minden egyes sorára számítások futó lekérdezések végezzen jobban, mint a replikált táblák elosztott táblákon. Mivel a replikált tábla rendszer teljes minden számítási csomóponton, processzorigényes replikált táblákon futása hello teljes táblázaton minden számítási csomóponton. hello további számítási is csökkentheti a lekérdezések teljesítményét.

Például ez a lekérdezés tartalmaz egy összetett predikátum.  Az elosztott táblázat helyett egy replikált tábla esetén a szállító gyorsabban futtatja. Ebben a példában a szállító kivonatoló elosztott vagy ciklikus multiplexelés elosztott.

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-tooreplicated-tables"></a>Alakítsa át a létező táblák ciklikus multiplexelés tooreplicated táblák
Ha már rendelkezik ciklikus multiplexelés táblák, javasoljuk a konvertálás tooreplicated táblák azokat a cikkben ismertetett feltételekkel, amelyek megfelelnek. Replikált táblák teljesítmény javítása a ciklikus multiplexelés táblák keresztül, mert azok kiküszöbölik adatmozgás hello szükségességét.  Ciklikus multiplexelés tábla mindig szükséges, adatátvitelt jelölik a táblákra. 

Ez a példa [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) toochange hello DimSalesTerritory tábla tooa replikált tábla. Ez a példa függetlenül attól, hogy DimSalesTerritory kivonatoló elosztott vagy a ciklikus multiplexelési működik.

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

--Create statistics on new table
CREATE STATISTICS [SalesTerritoryKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryKey]);
CREATE STATISTICS [SalesTerritoryAlternateKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryAlternateKey]);
CREATE STATISTICS [SalesTerritoryRegion] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryRegion]);
CREATE STATISTICS [SalesTerritoryCountry] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryCountry]);
CREATE STATISTICS [SalesTerritoryGroup] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryGroup]);

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] too[DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] too[DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a>Ciklikus multiplexelés és a lekérdezési teljesítmény példa replikálása 

A replikált tábla nem szükséges minden adatátvitelt jelölik a táblákra mivel hello teljes tábla már létezik minden számítási csomóponton. Ha hello dimenziótáblák ciklikus multiplexelés elosztott, illesztés átmásolja a terjesztendő hello dimenziótáblában teljes tooeach számítási csomópont. toomove hello adat, hello lekérdezésterv tartalmaz egy BroadcastMoveOperation nevű művelet. Ilyen adatok adatátviteli művelet csökkenti a lekérdezés teljesítményét, és kiiktatja a replikált táblák használata. tooview lekérdezés terv lépései, használja a hello [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) rendszer katalógusnézet használatával derítheti ki. 

Például a következő lekérdezés hello AdventureWorks sémának, hello ` FactInternetSales` táblát kell kivonatoló elosztott. Hello `DimDate` és `DimSalesTerritory` kisebb dimenzió táblák. A lekérdezés által visszaadott, pénzügyi év 2004 hello összes értékesítés Észak-Amerikában:
 
```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
Újból létrehozza `DimDate` és `DimSalesTerritory` ciklikus multiplexelés táblák. Ennek eredményeképpen hello lekérdezés bemutatta, a következő műveletek áthelyezése több szórás rendelkező lekérdezésterv hello: 
 
![Ciklikus lekérdezés terv](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

Újból létrehozza `DimDate` és `DimSalesTerritory` regisztrációja, mivel a replikált táblák és a hello lekérdezés újra futott. hello eredményül kapott lekérdezésterv sokkal rövidebb, és nem rendelkezik a küldi helyezi át.

![Replikált lekérdezésterv](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a>A replikált táblák módosítását a teljesítménnyel kapcsolatos szempontok
Az SQL Data Warehouse egy replikált tábla fölött: hello tábla főverzió valósítja meg. Másolja át hello főverzió tooone terjesztési adatbázis minden számítási csomóponton. Változás történik, amikor az SQL Data Warehouse először frissíti hello fő tábla. Majd minden számítási csomóponton hello táblák egy újjáépítését igényel. A replikált tábla újjáépítését hello tábla tooeach számítási csomópont másolása és majd a hello indexek újraépítése tartalmazza.

Újraépíti szükségesek után:
- Adatok betöltése vagy módosította
- hello adatraktár az méretezett tooa különböző DWU-beállítására
- Tábla definíciójában frissül.

Újraépíti esetén nincs szükség után:
- Szüneteltetési művelete
- A művelet folytatása

hello rebuild történik meg azonnal adatok módosítása után. Ehelyett hello rebuild kiváltásáról hello hello táblából kiválasztja a lekérdezés első alkalommal.  Hello kezdeti select utasítás hello táblából van lépéseket toorebuild hello replikált tábla.  Mivel hello rebuild hello lekérdezésen belül történik, hello hatás toohello kezdeti select utasítás hello tábla hello méretétől függően jelentős lehet.  Ha több replikált táblák egy rebuild igénylő van szó, minden egyes példányra újraépítésekor Feladattervek lépések hello utasításon belül.  toomaintain adatok konzisztenciájának hello hello újjáépítését során replikált tábla kizárólagos zárolást hello tábla lesz végrehajtva.  hello zárolási megakadályozza, hogy a minden hozzáférési toohello tábla hello hello Újraépítés időtartama. 

### <a name="use-indexes-conservatively"></a>Konzervatív módon indexek használata
Standard indexelési eljárások tooreplicated táblák alkalmazni. Az SQL Data Warehouse újraépíti minden replikált tábla index hello rebuild részeként. Indexek csak akkor alkalmazza, ha hello jobb teljesítménye ez fontosabb, mint a hello indexek újraépítése hello költségét.  
 
### <a name="batch-data-loads"></a>Kötegelt adatbetöltés
Ha az adatok betöltését replikált táblák, próbálkozna toominimize újraépíti terhelések kötegelés együtt. Hajtsa végre az összes kötegelni hello terhelések select utasítás futtatása előtt.

Például a terhelés mintát négy forrásból származó adatokat tölt, és meghívja a négy újraépíti. 

- Forrás 1 betöltése.
- SELECT utasítás eseményindítók építse újra az 1.
- Forrás 2 betöltése.
- SELECT utasítás eseményindítók építse újra a 2.
- Forrás 3 betöltése.
- SELECT utasítás eseményindítók építse újra a 3.
- Adatforrás 4 betöltése.
- SELECT utasítás eseményindítók építse újra a 4.

Például a terhelés mintát négy forrásból származó adatokat tölt, de csak hív meg, egy Újraépítés.

- Forrás 1 betöltése.
- Forrás 2 betöltése.
- Forrás 3 betöltése.
- Adatforrás 4 betöltése.
- SELECT utasítás eseményindítók építse újra.


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a>A replikált tábla helyreállítása egy kötegelt betöltés után
tooensure lekérdezés konzisztens végrehajtásának lassúságát, ajánlott kényszerített hello replikált táblák frissítését egy kötegelt betöltés után. Ellenkező esetben hello első lekérdezés hello táblák toorefresh, amely tartalmazza a hello indexek újraépítése kell várnia. Attól függően, hogy hello méretét és az érintett replikált táblák számát hello teljesítményre gyakorolt hatás jelentős lehet.  

Ez a lekérdezés használ hello [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV toolist hello replikált módosított, de nem úgy táblákhoz.

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
egy rebuild tooforce futtassa a következő utasítás minden táblában a kimeneti megelőző hello hello. 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a>Következő lépések 
a replikált tábla toocreate ezekre az utasításokra egyikét használja:

- [(Az Azure SQL Data Warehouse) tábla létrehozása](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [TABLE AS SELECT (Azure SQL Data Warehouse létrehozása](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

Elosztott táblák áttekintését lásd: [táblák elosztott](sql-data-warehouse-tables-distribute.md).




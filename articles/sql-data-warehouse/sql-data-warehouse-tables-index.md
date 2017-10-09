---
title: "az SQL Data Warehouse aaaIndexing táblák |} A Microsoft Azure"
description: "Ismerkedés az Azure SQL Data Warehouse indexelő táblával."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 3e617674-7b62-43ab-9ca2-3f40c41d5a88
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/12/2016
ms.author: shigu;barbkess
ms.openlocfilehash: e614d63c8fb871f2ba388f14576cf9f282d4b818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-tables-in-sql-data-warehouse"></a>Az SQL Data Warehouse táblák indexelő
> [!div class="op_single_selector"]
> * [– Áttekintés][Overview]
> * [Adattípusok][Data Types]
> * [Terjesztése][Distribute]
> * [Index][Index]
> * [Partíció][Partition]
> * [Statisztika][Statistics]
> * [Ideiglenes][Temporary]
> 
> 

Az SQL Data Warehouse beleértve több indexelési lehetőséget kínál [fürtözött oszlopcentrikus indexek][clustered columnstore indexes], [fürtözött indexek és fürtözetlen indexeire] [ clustered indexes and nonclustered indexes].  Ezenkívül azt is lehetőséget is kínál a Nincs index is [halommemória][heap].  Ez a cikk ismertet minden Indextípus hello előnyeit, valamint tippek toogetting hello kívül az indexek legtöbb teljesítmény. Lásd: [table szintaxis létrehozása] [ create table syntax] kapcsolatos további részletek az SQL Data Warehouse tábla toocreate.

## <a name="clustered-columnstore-indexes"></a>Fürtözött oszlopcentrikus indexek
Alapértelmezés szerint az SQL Data Warehouse akkor hozza létre fürtözött oszlopcentrikus index, amikor nincsenek index beállítások adhatók meg egy tábla. Fürtözött oszloptárindexű táblákat mindkét hello legmagasabb szintű adattömörítést, valamint a hello legjobb átfogó lekérdezési teljesítményt nyújtanak.  Fürtözött oszloptárindexű táblákat általában fog outperform fürtözött index vagy halommemória táblát, és általában nagy táblák hello a legjobb választás.  Ezen okok miatt a fürtözött oszlopcentrikus hello legjobb hely toostart akkor, ha biztos abban, hogy hogyan tooindex a táblában.  

a fürtözött oszlopcentrikus tábla toocreate egyszerűen adja meg a FÜRTÖZÖTT OSZLOPCENTRIKUS INDEX hello WITH záradékban, vagy hello WITH záradék hagyja:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Van néhány olyan forgatókönyvet, ahol fürtözött oszloptárindex nem lehet jó választás:

* Oszloptárindexű táblákat támogatja a varchar(max), nvarchar(max) és varbinary(max).  Fontolja meg inkább halmot vagy fürtözött index.
* Lehet, hogy az átmeneti adatok kevésbé hatékony Oszloptárindexű táblákat.  Vegye figyelembe a halommemória és akár még az ideiglenes táblák.
* Kevesebb mint 100 millió sort foglalnak kis táblákban.  Vegye figyelembe a halommemória táblákat.

## <a name="heap-tables"></a>Halommemória táblák
Amikor a rendszer átmenetileg érkezési SQL Data warehouse az adatokat, előfordulhat, hogy halommemória tábla használata teszi gyorsabban hello teljes folyamata.  Ennek az az oka terhelések tooheaps gyorsabb, mint a tooindex táblák és az egyes esetekben hello későbbi olvasási teheti a gyorsítótárból.  Ha tölt be adatokat, akkor további átalakítások hello tábla tooheap táblájának betöltésekor futtatása előtt fog sokkal gyorsabb, mint hello adatok tooa betöltése csak toostage fürtözött oszlopcentrikus tábla. Ezenkívül az adatok tooa betöltése [ideiglenes tábla] [ Temporary] is betölti sokkal gyorsabb, mint egy tábla toopermanent tárolási betöltésekor.  

A kis keresési táblák, kevesebb mint 100 millió sort foglalnak, gyakran halommemória táblázatokkal logika.  Fürt oszloptárindexű táblákat tooachieve optimális tömörítési kezdődik, ha több mint 100 millió sort.

toocreate halommemória tábla, egyszerűen adja meg HALOMMEMÓRIA hello WITH záradékkal:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Fürtözött és fürtözetlen indexeinek
Fürtözött indexek előfordulhat, hogy outperform fürtözött oszloptárindexű táblákat, ha egyetlen sor lekérése gyorsan toobe.  Hol található a egyetlen vagy nagyon kevés sor keresési lekérdezések szükséges tooperformance szélsőséges sebességű, fontolja meg a fürt index vagy másodlagos fürtözetlen index.  hello hátránya toousing fürtözött index, hogy csak olyan lekérdezéseket, amelyek magas szelektív szűrőt használja hello fürtözött index oszlop ki előnyeit.  más oszlopokat is meg lehet egy nem fürtözött index tooimprove szűrő tooother oszlopok hozzá.  Azonban minden index tooa tábla felvett felveszi lemezterület és a feldolgozási idő tooloads.

egy fürtözött index táblázat toocreate egyszerűen adja meg, FÜRTÖZÖTT INDEX hello WITH záradékban:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

tooadd egy nem fürtözött index táblán, egyszerűen használja a következő szintaxist hello:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

## <a name="optimizing-clustered-columnstore-indexes"></a>Fürtözött oszlopcentrikus indexek optimalizálása
Fürtözött oszloptárindexű táblákat történő adatok vannak rendszerezve.  Kritikus tooachieving optimális lekérdezési teljesítmény oszlopcentrikus táblán kiváló minőségű magas szegmens.  Szegmens minőségi mérhető egy tömörített sor csoport sorainak hello száma alapján.  Szegmens minőségi legoptimálisabb, ahol legalább 100K tömörített soronkénti sorok csoportot és arra, hogy a teljesítmény, hello kötegenkénti sorok száma sor csoport megközelítés 1 048 576 sort, amely egy sorcsoport tartalmazhat legtöbb sorok hello.

alább nézet hello hozható létre és használja a rendszer toocompute hello soronkénti átlagos sorok csoportosítása és optimális fürt oszlopcentrikus indexekkel azonosítása.  Ez a nézet hello utolsó oszlopa az indexek, amely lehet használt toorebuild SQL-utasítás hoz létre.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,    COUNT(DISTINCT rg.[partition_number])                    AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,    CEILING    ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Most, hogy a létrehozott hello nézet, a lekérdezés futtatására tooidentify táblák kevesebb mint 100K sorok sor csoportokkal.  Természetesen érdemes lehet tooincrease hello küszöbérték 100 k Ha további optimális szegmens minőségi keres. 

```sql
SELECT    *
FROM    [dbo].[vColumnstoreDensity]
WHERE    COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Hello lekérdezés futtatása után kezdje toolook hello adatait, és az eredményeket elemezni. Ez a táblázat azt ismerteti, milyen toolook esetében a sor csoport elemzés.

| Oszlop | Hogyan toouse ezeket az adatokat |
| --- | --- |
| [table_partition_count] |Ha hello tábla particionálva van, majd előfordulhat, hogy várt toosee magasabb nyissa meg a sorok csoport száma. Mindegyik partíció hello elosztása elméletileg lehet egy megnyitott sor csoport társítva. Ez az elemzéshez kéttényezős. Kis tábla particionálva van sikerült optimalizálhatók a particionálás elemet, mivel ez javíthatják a tömörítési hello eltávolításával. |
| [row_count_total] |Sorok teljes számának hello tábla. A sorok érték toocalculate százalékát használhatja például tömörített hello állapotban. |
| [row_count_per_distribution_MAX] |Ha minden sor egyenletesen ezt az értéket hello cél kötegenkénti sorok száma terjesztési lenne. Hasonlítsa össze a hello compressed_rowgroup_count ezt az értéket. |
| [COMPRESSED_rowgroup_rows] |Oszlopcentrikus formátum hello tábla sorainak száma. |
| [COMPRESSED_rowgroup_rows_AVG] |Ha hello sorok átlagos száma jelentősen kisebb, mint hello maximális száma egy sor csoport sorok, akkor érdemes CTAS, vagy az ALTER INDEX REBUILD toorecompress hello adatok |
| [COMPRESSED_rowgroup_count] |Sorcsoportok oszlopcentrikus formátumban száma. Ez a szám a kapcsolat toohello táblában nagyon magas, akkor azt jelzi, hogy hello oszlopcentrikus sűrűség nem elegendőek. |
| [COMPRESSED_rowgroup_rows_DELETED] |Vannak logikailag törölt sorok oszlopcentrikus formátumban. Ha hello szám magas relatív tootable méretét, fontolja meg a hello partíció újbóli létrehozása vagy újraépítése hello index, fizikailag ezzel eltávolítja azokat. |
| [COMPRESSED_rowgroup_rows_MIN] |Ezzel együtt hello átlagos és maximális oszlopok toounderstand hello értékek tartományán hello sor csoportok az oszloptárindexet. Kevés hello oldalbetöltés küszöbértéke (102,400 igazítva partíció eloszlása) keresztül javasol, hogy hello adatbetöltés rendelkezésre állnak-e optimalizálás. |
| [COMPRESSED_rowgroup_rows_MAX] |Mivel a fenti |
| [OPEN_rowgroup_count] |Nyissa meg sorcsoportok nem rendellenes. Egy tábla eloszlása (60) megnyitott sorcsoport ésszerűen teheti. Túl sok számok között partíciók Adatbetöltési javaslat. Ellenőrizze hello particionálási stratégia toomake róla, hogy ésszerű |
| [OPEN_rowgroup_rows] |Minden egyes sorára csoport lehetnek 1 048 576 sort azt a maximum. Használja a hogyan teljes érték toosee hello nyitott sorcsoportok jelenleg |
| [OPEN_rowgroup_rows_MIN] |A csoportok menü megnyitása jelzi, hogy adatokat trickle betöltését hello táblába, vagy az, hogy a sor csoportba fennmaradó sorok keresztül kiömlött előző terhelés hello. Használjon hello MIN, MAX, AVG oszlopok toosee mennyi adatot az volt a nyitott sorcsoportok. Kis táblák összes hello adatok 100 %-os lehet! Ebben az esetben az ALTER INDEX REBUILD tooforce hello adatok toocolumnstore. |
| [OPEN_rowgroup_rows_MAX] |Mivel a fenti |
| [OPEN_rowgroup_rows_AVG] |Mivel a fenti |
| [CLOSED_rowgroup_rows] |Tekintse meg lezárt hello sor sorok csoportosítása megerősítést ellenőrzése. |
| [CLOSED_rowgroup_count] |lezárt sorcsoportok hello száma alacsony, ha bármelyik egyáltalán nem lehet. Lezárt sorcsoportok konvertált toocompressed rowg roups hello ALTER INDEX használatával lehet... SZERVEZNI a parancsot. Ez azonban nincs szükség. Lezárt csoportok olyan automatikusan átalakított toocolumnstore sorcsoportok hello háttérben "rekord rekordáthelyezőnek" folyamat. |
| [CLOSED_rowgroup_rows_MIN] |Lezárt sorcsoportok kell rendelkeznie a nagyon nagy kitöltés értéket. Ha hello kitöltés sebessége lezárt sor csoport alacsony, majd a hello oszlopcentrikus további elemzésre szükség. |
| [CLOSED_rowgroup_rows_MAX] |Mivel a fenti |
| [CLOSED_rowgroup_rows_AVG] |Mivel a fenti |
| [Rebuild_Index_SQL] |SQL toorebuild oszlopcentrikus indexet a tábla |

## <a name="causes-of-poor-columnstore-index-quality"></a>A gyenge oszlopcentrikus index minőségi okok
Ha a táblák gyenge szegmens minőségi azonosította, célszerű tooidentify hello alapvető okát.  Az alábbiakban a gyenge szegmens quaility egyéb gyakori oka:

1. Memóriaprobléma, ha készült el index
2. Nagy mennyiségű DML-műveletek
3. Kis vagy trickle terhelés műveletek
4. Túl sok partíciót

Ezek a tényezők okozhatják egy oszlopcentrikus index toohave jelentősen kisebb, mint a hello optimális 1 millió sort foglalnak sor csoportonként.  Sorok toogo toohello különbözeti sorcsoport tömörített sor csoport helyett is okozhat. 

### <a name="memory-pressure-when-index-was-built"></a>Memóriaprobléma, ha készült el index
hello kötegenkénti sorok száma tömörített sorcsoport hello sor közvetlenül kapcsolódó toohello szélességét és hello elérhető tooprocess hello sorcsoport memória mennyisége.  Amikor sorok írt toocolumnstore táblák nagynak a Memóriaterhelést, oszlopcentrikus szegmens minőségi romolhat.  Ajánlott eljárás hello ezért toogive hello munkamenetből, ezt a tooyour oszlopcentrikus index táblák hozzáférési tooas mennyi memóriát lehető ír.  Mivel a memória és a párhuzamosság között, jobb foglalási függ hello adatfájl egyes soraiban a tábla, hello DWU-mennyiség, memória kiosztása tooyour rendszer, és feldolgozási hello mennyisége tárolóhely, hello hello útmutatást biztosíthat toohello az munkamenetből, ezt a tooyour adattábla ír.  Ajánlott eljárásként javasoljuk, xlargerc DW300 használata vagy kevésbé largerc használata DW400 tooDW600 és mediumrc DW1000 használata vagy újabb.

### <a name="high-volume-of-dml-operations"></a>Nagy mennyiségű DML-műveletek
Nagyszámú DML-műveletek, frissítése és sorokat törölni hello oszlopcentrikus is elégtelenség bevezetéséhez. Ez különösen igaz egy sor csoport hello sorainak hello többsége módosításakor.

* Egy sor törlését a tömörített sorcsoport csak logikailag jelöli meg hello sor törlése. hello sor marad hello tömörített sor csoport hello partíció vagy tábla újraépítésekor.
* Hello sor tootooan belső sortárindex nevű táblát a különbözeti sorcsoport beszúrni egy sort ad hozzá. szúrja be hello sor nincs konvertált toocolumnstore hello különbözeti sorcsoport megtelt, és a lezártnak jelölt. Sorcsoportok be van zárva, ha eléri maximális kapacitását hello 1 048 576 sort. 
* A logikai törlés és Beszúrás feldolgozása oszlopcentrikus formátumban sor frissítése. sort beszúrni hello tárolhatjuk hello különbözeti tárolójában.

Frissítés kötegelni és 102 400 sorszám igazított terjesztési fog szerepelni, közvetlen toohello oszlopcentrikus formázása hello tömeges küszöbértéket meghaladó műveletek beszúrása. Azonban ha az még akkor is, terjesztési, kellene toobe módosítása több mint 6.144 millió sort foglalnak el az e toooccur egyetlen műveletben. Ha egy adott partícióra sorok számát hello egyeztetve terjesztési nem kisebb, mint 102,400 hello sorok toohello különbözeti tároló kerül, és nem marad, amíg elegendő sorok beszúrását vagy módosított tooclose hello csoport vagy hello sorindex újra lett építve.

### <a name="small-or-trickle-load-operations"></a>Kis vagy trickle terhelés műveletek
Kis tölt be, hogy az SQL Data Warehouse-folyamat is is ismertek, terhelések trickle. Általában képviselnek keresztül a szervezetbe hello rendszer alatt álló adatok közel állandó adatfolyam. Azonban mivel ezen az adatfolyamon már majdnem folyamatos hello kötet sorok nincs különösen nagy. Gyakran hello adata jelentősen közvetlen terhelés toocolumnstore formátuma nem szükséges hello küszöbérték alatti.

Ezekben a helyzetekben gyakran jobb tooland hello adatok először az Azure blob Storage tárolóban, és azt a korábbi tooloading felhalmozhat. Ezzel a technikával gyakran nevezik *micro kötegelés*.

### <a name="too-many-partitions"></a>Túl sok partíciót
Egy másik művelet tooconsider a fürtözött oszloptárindexű táblákat a particionálás hello hatását.  Particionálás, mielőtt az SQL Data Warehouse már osztja fel az adatok 60 adatbázisok.  Az adatok további particionálás osztja.  Ha az adatok particionálása, akkor érdemes tooconsider, amely **minden** partíció kell toohave legalább 1 millió sort toobenefit a fürtözött oszlopcentrikus index.  Ha Ön a tábla particionálása 100 partíciókra, akkor a táblában kell legalább a 6 milliárd toohave sorok toobenefit a fürtözött oszlopcentrikus index (60 terjesztéseket * 100 partíciók * 1 millió sort foglalnak). A 100 partíciós tábla nincs 6 egymilliárd sort, csökkentse a partíciók száma hello, vagy érdemes inkább egy halommemóriában tábla.

A táblák adatokkal lett betöltve, kövesse az alábbi lépéseket tooidentify hello és optimális fürt oszlopcentrikus indexekkel rendelkező táblák újbóli létrehozása.

## <a name="rebuilding-indexes-tooimprove-segment-quality"></a>Indexek tooimprove szegmens minőségi újraépítése
### <a name="step-1-identify-or-create-user-which-uses-hello-right-resource-class"></a>1. lépés: Azonosítására vagy hello jobb erőforrásosztály használó felhasználó létrehozása
Egy gyors tooimmediately szegmens minőségének javítása módja toorebuild hello index.  SQL-nézet fent hello által visszaadott hello adja vissza az ALTER INDEX REBUILD utasítás, amely lehet használt toorebuild az indexek.  Ha az indexek újraépítése, győződjön meg, hogy elegendő memória toohello munkamenetből, ezt a rendszer az index újraépítése osszon ki.  toodo e, növelje hello erőforrásosztály egy olyan felhasználó, amelynek a tábla toohello ajánlott minimális engedélyek toorebuild hello index van.  hello erőforrásosztály hello adatbázis tulajdonosa felhasználó nem lehet módosítani, ezért ha hello rendszeren nem a felhasználó hozott létre, szüksége lesz toodo így először.  hello általunk javasolt minimum xlargerc DW300 használata, vagy kevesebb, largerc használata DW400 tooDW600 és mediumrc DW1000 használata vagy újabb.

Az alábbiakban a példa bemutatja, hogyan van tooallocate további memória tooa felhasználói azok erőforrásosztály növelésével.  További információ az erőforrás-osztályok és hogyan toocreate új felhasználó található a hello [egyidejűségi és munkaterhelés-kezelés] [ Concurrency] cikk.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>2. lépés: A fürtözött oszlopcentrikus indexek újraépítése magasabb erőforrás osztály felhasználóval
Bejelentkezési felhasználóként hello 1. lépésben (pl. LoadUser), amely most már nagyobb erőforrás osztályt használ, és hello ALTER INDEX utasítás végrehajtása.  Győződjön meg arról, hogy a felhasználó rendelkezik-e az ALTER engedéllyel toohello táblák hello index újraépíti amennyiben.  Ezek a példák azt szemléltetik, hogyan toorebuild hello teljes oszlopcentrikus index, vagy hogyan toorebuild egyetlen partícióra. Nagy táblák akkor egyetlen partition egyszerre több gyakorlati toorebuild indexeket.

Másik lehetőségként helyett hello index újraépítése, átmásolhatja hello tábla tooa új tábla használatával [CTAS][CTAS].  Mely legjobb módja? A nagy mennyiségű adatot [CTAS] [ CTAS] általában gyorsabb, mint [ALTER INDEX][ALTER INDEX]. A kisebb mennyiségű adatot [ALTER INDEX] [ ALTER INDEX] könnyebb toouse és nem igényel tooswap hello tábla ki.  Lásd: **CTAS és a partíció váltás indexek újraépítése** alább olvashat, hogyan toorebuild indexeli a CTAS.

```sql
-- Rebuild hello entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Az SQL Data Warehouse az index újraépítése során offline állapotú.  Indexek újraépítésével kapcsolatos további információkért tekintse meg az ALTER INDEX REBUILD című hello [Oszlopcentrikus index töredezettségmentesítési] [ Columnstore Indexes Defragmentation] és hello szintaxis témakör [ALTER INDEX] [ALTER INDEX].

### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>3. lépés: Ellenőrizze, hogy fürtözött oszlopcentrikus szegmens minőség javult
Futtassa újra a műveletet hello lekérdezés tábla meghatározott gyenge a minőségi szegmentálni, és ellenőrzése a szegmens minőség javult.  Ha nem javította szegmens minőségének, lehet, hogy a tábla sorainak hello nagyon nagy.  Az index újraépítésekor a magasabb erőforrásosztály vagy a DWU használata javasolt.

## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>A CTAS és a partíció váltás indexek újraépítése
Ez a példa [CTAS] [ CTAS] és partícióváltás toorebuild tábla partíció. 

```sql
-- Step 1: Select hello partition of data and write it out tooa new table using CTAS
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT hello data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 too [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN hello rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 too [dbo].[FactInternetSales] PARTITION 2;
```

Hozza létre újra a partíciók használatával kapcsolatos további részletekért `CTAS`, lásd: hello [partíció] [ Partition] cikk.

## <a name="next-steps"></a>Következő lépések
toolearn több, lásd: hello cikkeket a [tábla áttekintése][Overview], [tábla adattípusok][Data Types], [táblaterjesztése] [ Distribute], [Tábla particionáló][Partition], [fenntartása a tábla statisztikai adatainak] [ Statistics] és [ Az ideiglenes táblák][Temporary].  További információ az ajánlott eljárások toolearn lásd [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[heap]: https://msdn.microsoft.com/library/hh213609.aspx
[clustered indexes and nonclustered indexes]: https://msdn.microsoft.com/library/ms190457.aspx
[create table syntax]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore Indexes Defragmentation]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[clustered columnstore indexes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->

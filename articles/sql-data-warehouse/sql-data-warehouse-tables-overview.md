---
title: "az SQL Data Warehouse táblák aaaOverview |} Microsoft Docs"
description: "Ismerkedés az Azure SQL Data Warehouse tábláiba."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 2114d9ad-c113-43da-859f-419d72604bdf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 06/29/2016
ms.author: shigu;jrj
ms.openlocfilehash: 4edabcb4b0754bf6c99c2b6b3f0c077749051d9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-tables-in-sql-data-warehouse"></a>Az SQL Data Warehouse táblák áttekintése
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

Ismerkedés az SQL Data Warehouse-táblázatok létrehozása felettébb egyszerű.  Alapszintű hello [CREATE TABLE] [ CREATE TABLE] szintaxisa a következő hello közös szintaxis legnagyobb valószínűséggel már ismeri, a más adatbázisok nem működnek.  egy tábla toocreate, egyszerűen tooname a táblában az oszlopok neve, és az egyes oszlopok adattípusok definiálása.  Ha más adatbázisokban is hozzon létre táblák, ez tisztában tooyou kell kinéznie.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Példa fent hello táblázatot hoz létre az ügyfelek nevű két oszlop, az utónév és Vezetéknév.  Egyes oszlopok VARCHAR(25), hello adatok too25 karakterek korlátozó adattípus van definiálva.  Alapvető attribútumait az egy tábla, valamint más, főleg hello ugyanaz, mint a más adatbázisok vannak.  Az egyes oszlopok megadása és hello az adatok sértetlenségének ellenőrzése.  Indexek i/o csökkentésével tooimprove teljesítmény lehet hozzáadni.  Particionálás felveheti tooimprove teljesítmény Ha toomodify adatok van szükség.

[Átnevezése] [ RENAME] egy SQL Data Warehouse tábla néz ki:

```sql  
RENAME OBJECT Customer tooCustomerOrig; 
 ```

## <a name="distributed-tables"></a>Elosztott táblák
A like SQL Data Warehouse hello elosztott rendszerek által bevezetett új alapvető attribútum **terjesztési oszlop**.  hello terjesztési oszlop túlságosan azok milyen úgy tűnik, hogy hasonló.  Hello oszlop, amely meghatározza, hogyan toodistribute, vagy osztási, az adatok hello színfalak mögött.  Amikor hello terjesztési oszlopok megadása nélkül hozunk létre táblát, hello tábla a rendszer automatikusan továbbítja használatával **ciklikus multiplexelés**.  Ciklikus multiplexelés táblák elegendő bizonyos esetekben lehetnek, terjesztési oszlopok meghatározása csökkenti adatátvitelt jelölik a lekérdezések, így a teljesítmény optimalizálása során.  Olyan esetekben ha kis mennyiségű táblákban tárolt adatokat, válassza a toocreate hello tábla hello **replikálása** terjesztési típust másolja át az adatokat tooeach számítási csomópont, és menti a adatátvitelt jelölik a lekérdezések végrehajtása során. Lásd: [terjesztése egy tábla] [ Distribute] kapcsolatos további toolearn tooselect terjesztési oszlop.

## <a name="indexing-and-partitioning-tables"></a>Indexelő és táblák particionálása
Válnak fejlettebb, az SQL Data Warehouse a, és szeretné, hogy toooptimize teljesítmény, érdemes toolearn Táblatervezés többet.  toolearn több, lásd: hello cikkeket a [tábla adattípusok][Data Types], [terjesztése egy tábla][Distribute], [táblaindexelő] [ Index] és [tábla particionáló][Partition].

## <a name="table-statistics"></a>Tábla statisztikai adatainak
A statisztikákat egy rendkívül fontos toogetting hello legjobb teljesítmény érdekében az SQL Data Warehouse kívül.  Mivel az SQL Data Warehouse automatikusan még nem létrehozása, és statisztikai adatainak frissítése, például előfordulhat, hogy az Azure SQL Database tooexpect származnak, a cikk elolvasása a [statisztika] [ Statistics] hello egyike lehet legfontosabb cikkeket tooensure olvassa el a lekérdezés elérhetővé hello lehető legjobb teljesítményt.

## <a name="temporary-tables"></a>Ideiglenes táblák
Az ideiglenes táblák olyan táblák csak hello ideje alatt a bejelentkezési létezik, és más felhasználók által nem láthatók.  Az ideiglenes táblák lehetnek egy jó módszer tooprevent mások ideiglenes eredmények megtekinthessék és karbantartása hello szükségességét is csökkentheti.  Az ideiglenes táblák is a helyi tároló használatára, mivel egyes műveletek esetében gyorsabb teljesítményt is biztosítanak.  Lásd: hello [ideiglenes tábla] [ Temporary] cikkek az ideiglenes táblák további információt.

## <a name="external-tables"></a>Külső táblák
Külső táblák, más néven a Polybase táblák olyan táblák, amely az SQL Data Warehouse, de az SQL Data Warehouse külső pont toodata kérdezhetők le.  Például létrehozhat egy külső tábla melyik pontok toofiles Azure Blob Storage tárolóban.  Hogyan toocreate és a lekérdezés a külső tábla, tekintse meg a további részletekért [adatok betöltése a Polybase][Load data with Polybase].  

## <a name="unsupported-table-features"></a>A tábla nem támogatott funkciók
Amíg az SQL Data Warehouse tartalmaz hello szolgáltatást tábla más adatbázisok által kínált számos, van néhány funkció, amely még nem támogatottak.  Alább olvashat egy listát néhány hello táblában funkciók még nem támogatottak.

| Nem támogatott funkciók |
| --- |
| Elsődleges kulcs, külső kulcsok, egyedi és a jelölőnégyzet [tábla megkötések][Table Constraints] |
| [Egyedi indexek][Unique Indexes] |
| [A számított oszlopok][Computed Columns] |
| [Ritka oszlopokat][Sparse Columns] |
| [Felhasználó által definiált típusok][User-Defined Types] |
| [Feladatütemezési][Sequence] |
| [Eseményindítók][Triggers] |
| [Az indexelt nézetek][Indexed Views] |
| [Szinonimák][Synonyms] |

## <a name="table-size-queries"></a>Tábla méretét lekérdezések
Egy egyszerű módon tooidentify terület és egy tábla minden hello 60 azokat a terjesztéseket, által felhasznált sorok toouse [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Azonban a DBCC-parancsokkal is kell meglehetősen korlátozása.  Dinamikus felügyeleti nézetekkel (dinamikus felügyeleti nézetek) lehetővé teszi a toosee sokkal részletességi, valamint hello lekérdezési eredmények sokkal nagyobb szabályozható.  Először hozzon létre ebben a nézetben, ez az említett tooby jelen példában számos ebben és egyéb.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Összefoglaló táblázat terület
A lekérdezés hello sorok és a hely által visszaadott tábla.  A táblák, a legnagyobb táblák, és azok csatlakoznak-e időszeletelés, replikált vagy elosztott kivonatoló kiváló lekérdezés toosee.  Az elosztott kivonattáblákkal hello terjesztési oszlop is mutatja.  A legtöbb esetben a legnagyobb kell szerepelnie. a fürtözött oszlopcentrikus indexszel rendelkező elosztott kivonatát.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Tábla hely telepítési típusonként
```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Tábla terület index típus szerint
```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Terjesztési hely összefoglaló
```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Következő lépések
toolearn több, lásd: hello cikkeket a [tábla adattípusok][Data Types], [terjesztése egy tábla][Distribute], [táblaindexelő] [ Index], [Tábla particionáló][Partition], [fenntartása a tábla statisztikai adatainak] [ Statistics] és [ Az ideiglenes táblák][Temporary].  Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Load data with Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Table Constraints]: https://msdn.microsoft.com/library/ms188066.aspx
[Computed Columns]: https://msdn.microsoft.com/library/ms186241.aspx
[Sparse Columns]: https://msdn.microsoft.com/library/cc280604.aspx
[User-Defined Types]: https://msdn.microsoft.com/library/ms131694.aspx
[Sequence]: https://msdn.microsoft.com/library/ff878091.aspx
[Triggers]: https://msdn.microsoft.com/library/ms189799.aspx
[Indexed Views]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyms]: https://msdn.microsoft.com/library/ms177544.aspx
[Unique Indexes]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->

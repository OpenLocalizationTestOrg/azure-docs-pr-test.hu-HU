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
# <a name="overview-of-tables-in-sql-data-warehouse"></a><span data-ttu-id="a7dec-103">Az SQL Data Warehouse táblák áttekintése</span><span class="sxs-lookup"><span data-stu-id="a7dec-103">Overview of tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="a7dec-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="a7dec-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="a7dec-105">[Adattípusok][Data Types]</span><span class="sxs-lookup"><span data-stu-id="a7dec-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="a7dec-106">[Terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="a7dec-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="a7dec-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="a7dec-107">[Index][Index]</span></span>
> * <span data-ttu-id="a7dec-108">[Partíció][Partition]</span><span class="sxs-lookup"><span data-stu-id="a7dec-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="a7dec-109">[Statisztika][Statistics]</span><span class="sxs-lookup"><span data-stu-id="a7dec-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="a7dec-110">[Ideiglenes][Temporary]</span><span class="sxs-lookup"><span data-stu-id="a7dec-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="a7dec-111">Ismerkedés az SQL Data Warehouse-táblázatok létrehozása felettébb egyszerű.</span><span class="sxs-lookup"><span data-stu-id="a7dec-111">Getting started with creating tables in SQL Data Warehouse is simple.</span></span>  <span data-ttu-id="a7dec-112">Alapszintű hello [CREATE TABLE] [ CREATE TABLE] szintaxisa a következő hello közös szintaxis legnagyobb valószínűséggel már ismeri, a más adatbázisok nem működnek.</span><span class="sxs-lookup"><span data-stu-id="a7dec-112">hello basic [CREATE TABLE][CREATE TABLE] syntax follows hello common syntax you are most likely already familiar with from working with other databases.</span></span>  <span data-ttu-id="a7dec-113">egy tábla toocreate, egyszerűen tooname a táblában az oszlopok neve, és az egyes oszlopok adattípusok definiálása.</span><span class="sxs-lookup"><span data-stu-id="a7dec-113">toocreate a table, you simply need tooname your table, name your columns and define data types for each column.</span></span>  <span data-ttu-id="a7dec-114">Ha más adatbázisokban is hozzon létre táblák, ez tisztában tooyou kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="a7dec-114">If you've create tables in other databases, this should look very familiar tooyou.</span></span>

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

<span data-ttu-id="a7dec-115">Példa fent hello táblázatot hoz létre az ügyfelek nevű két oszlop, az utónév és Vezetéknév.</span><span class="sxs-lookup"><span data-stu-id="a7dec-115">hello above example creates a table named Customers with two columns, FirstName and LastName.</span></span>  <span data-ttu-id="a7dec-116">Egyes oszlopok VARCHAR(25), hello adatok too25 karakterek korlátozó adattípus van definiálva.</span><span class="sxs-lookup"><span data-stu-id="a7dec-116">Each column is defined with a data type of VARCHAR(25), which limits hello data too25 characters.</span></span>  <span data-ttu-id="a7dec-117">Alapvető attribútumait az egy tábla, valamint más, főleg hello ugyanaz, mint a más adatbázisok vannak.</span><span class="sxs-lookup"><span data-stu-id="a7dec-117">These fundamental attributes of a table, as well as others, are mostly hello same as other databases.</span></span>  <span data-ttu-id="a7dec-118">Az egyes oszlopok megadása és hello az adatok sértetlenségének ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a7dec-118">Data types are defined for each column and ensure hello integrity of your data.</span></span>  <span data-ttu-id="a7dec-119">Indexek i/o csökkentésével tooimprove teljesítmény lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="a7dec-119">Indexes can be added tooimprove performance by reducing I/O.</span></span>  <span data-ttu-id="a7dec-120">Particionálás felveheti tooimprove teljesítmény Ha toomodify adatok van szükség.</span><span class="sxs-lookup"><span data-stu-id="a7dec-120">Partitioning can be added tooimprove performance when you need toomodify data.</span></span>

<span data-ttu-id="a7dec-121">[Átnevezése] [ RENAME] egy SQL Data Warehouse tábla néz ki:</span><span class="sxs-lookup"><span data-stu-id="a7dec-121">[Renaming][RENAME] a SQL Data Warehouse table looks like this:</span></span>

```sql  
RENAME OBJECT Customer tooCustomerOrig; 
 ```

## <a name="distributed-tables"></a><span data-ttu-id="a7dec-122">Elosztott táblák</span><span class="sxs-lookup"><span data-stu-id="a7dec-122">Distributed tables</span></span>
<span data-ttu-id="a7dec-123">A like SQL Data Warehouse hello elosztott rendszerek által bevezetett új alapvető attribútum **terjesztési oszlop**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-123">A new fundamental attribute introduced by distributed systems like SQL Data Warehouse is hello **distribution column**.</span></span>  <span data-ttu-id="a7dec-124">hello terjesztési oszlop túlságosan azok milyen úgy tűnik, hogy hasonló.</span><span class="sxs-lookup"><span data-stu-id="a7dec-124">hello distribution column is very much what it sounds like.</span></span>  <span data-ttu-id="a7dec-125">Hello oszlop, amely meghatározza, hogyan toodistribute, vagy osztási, az adatok hello színfalak mögött.</span><span class="sxs-lookup"><span data-stu-id="a7dec-125">It is hello column that determines how toodistribute, or divide, your data behind hello scenes.</span></span>  <span data-ttu-id="a7dec-126">Amikor hello terjesztési oszlopok megadása nélkül hozunk létre táblát, hello tábla a rendszer automatikusan továbbítja használatával **ciklikus multiplexelés**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-126">When you create a table without specifying hello distribution column, hello table is automatically distributed using **round robin**.</span></span>  <span data-ttu-id="a7dec-127">Ciklikus multiplexelés táblák elegendő bizonyos esetekben lehetnek, terjesztési oszlopok meghatározása csökkenti adatátvitelt jelölik a lekérdezések, így a teljesítmény optimalizálása során.</span><span class="sxs-lookup"><span data-stu-id="a7dec-127">While round robin tables can be sufficient in some scenarios, defining distribution columns can greatly reduce data movement during queries, thus optimizing performance.</span></span>  <span data-ttu-id="a7dec-128">Olyan esetekben ha kis mennyiségű táblákban tárolt adatokat, válassza a toocreate hello tábla hello **replikálása** terjesztési típust másolja át az adatokat tooeach számítási csomópont, és menti a adatátvitelt jelölik a lekérdezések végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="a7dec-128">In situations where there is a small amount of data in a table, choosing toocreate hello table with hello **replicate** distribution type copies data tooeach compute node and saves data movement at query execution time.</span></span> <span data-ttu-id="a7dec-129">Lásd: [terjesztése egy tábla] [ Distribute] kapcsolatos további toolearn tooselect terjesztési oszlop.</span><span class="sxs-lookup"><span data-stu-id="a7dec-129">See [Distributing a Table][Distribute] toolearn more about how tooselect a distribution column.</span></span>

## <a name="indexing-and-partitioning-tables"></a><span data-ttu-id="a7dec-130">Indexelő és táblák particionálása</span><span class="sxs-lookup"><span data-stu-id="a7dec-130">Indexing and partitioning tables</span></span>
<span data-ttu-id="a7dec-131">Válnak fejlettebb, az SQL Data Warehouse a, és szeretné, hogy toooptimize teljesítmény, érdemes toolearn Táblatervezés többet.</span><span class="sxs-lookup"><span data-stu-id="a7dec-131">As you become more advanced in using SQL Data Warehouse and want toooptimize performance, you'll want toolearn more about Table Design.</span></span>  <span data-ttu-id="a7dec-132">toolearn több, lásd: hello cikkeket a [tábla adattípusok][Data Types], [terjesztése egy tábla][Distribute], [táblaindexelő] [ Index] és [tábla particionáló][Partition].</span><span class="sxs-lookup"><span data-stu-id="a7dec-132">toolearn more, see hello articles on [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index] and  [Partitioning a Table][Partition].</span></span>

## <a name="table-statistics"></a><span data-ttu-id="a7dec-133">Tábla statisztikai adatainak</span><span class="sxs-lookup"><span data-stu-id="a7dec-133">Table statistics</span></span>
<span data-ttu-id="a7dec-134">A statisztikákat egy rendkívül fontos toogetting hello legjobb teljesítmény érdekében az SQL Data Warehouse kívül.</span><span class="sxs-lookup"><span data-stu-id="a7dec-134">Statistics are an extremely important toogetting hello best performance out of your SQL Data Warehouse.</span></span>  <span data-ttu-id="a7dec-135">Mivel az SQL Data Warehouse automatikusan még nem létrehozása, és statisztikai adatainak frissítése, például előfordulhat, hogy az Azure SQL Database tooexpect származnak, a cikk elolvasása a [statisztika] [ Statistics] hello egyike lehet legfontosabb cikkeket tooensure olvassa el a lekérdezés elérhetővé hello lehető legjobb teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="a7dec-135">Since SQL Data Warehouse does not yet automatically create and update statistics for you, like you may have come tooexpect in Azure SQL Database, reading our article on [Statistics][Statistics] might be one of hello most important articles you read tooensure that you get hello best performance from your queries.</span></span>

## <a name="temporary-tables"></a><span data-ttu-id="a7dec-136">Ideiglenes táblák</span><span class="sxs-lookup"><span data-stu-id="a7dec-136">Temporary tables</span></span>
<span data-ttu-id="a7dec-137">Az ideiglenes táblák olyan táblák csak hello ideje alatt a bejelentkezési létezik, és más felhasználók által nem láthatók.</span><span class="sxs-lookup"><span data-stu-id="a7dec-137">Temporary tables are tables which only exist for hello duration of your logon and cannot be seen by other users.</span></span>  <span data-ttu-id="a7dec-138">Az ideiglenes táblák lehetnek egy jó módszer tooprevent mások ideiglenes eredmények megtekinthessék és karbantartása hello szükségességét is csökkentheti.</span><span class="sxs-lookup"><span data-stu-id="a7dec-138">Temporary tables can be a good way tooprevent others from seeing temporary results and also reduce hello need for cleanup.</span></span>  <span data-ttu-id="a7dec-139">Az ideiglenes táblák is a helyi tároló használatára, mivel egyes műveletek esetében gyorsabb teljesítményt is biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="a7dec-139">Since temporary tables also utilize local storage, they can offer faster performance for some operations.</span></span>  <span data-ttu-id="a7dec-140">Lásd: hello [ideiglenes tábla] [ Temporary] cikkek az ideiglenes táblák további információt.</span><span class="sxs-lookup"><span data-stu-id="a7dec-140">See hello [Temporary Table][Temporary] articles for more details about temporary tables.</span></span>

## <a name="external-tables"></a><span data-ttu-id="a7dec-141">Külső táblák</span><span class="sxs-lookup"><span data-stu-id="a7dec-141">External tables</span></span>
<span data-ttu-id="a7dec-142">Külső táblák, más néven a Polybase táblák olyan táblák, amely az SQL Data Warehouse, de az SQL Data Warehouse külső pont toodata kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="a7dec-142">External tables, also known as Polybase tables, are tables which can be queried from SQL Data Warehouse, but point toodata external from SQL Data Warehouse.</span></span>  <span data-ttu-id="a7dec-143">Például létrehozhat egy külső tábla melyik pontok toofiles Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a7dec-143">For example, you can create an external table which points toofiles on Azure Blob Storage.</span></span>  <span data-ttu-id="a7dec-144">Hogyan toocreate és a lekérdezés a külső tábla, tekintse meg a további részletekért [adatok betöltése a Polybase][Load data with Polybase].</span><span class="sxs-lookup"><span data-stu-id="a7dec-144">For more details on how toocreate and query an external table, see [Load data with Polybase][Load data with Polybase].</span></span>  

## <a name="unsupported-table-features"></a><span data-ttu-id="a7dec-145">A tábla nem támogatott funkciók</span><span class="sxs-lookup"><span data-stu-id="a7dec-145">Unsupported table features</span></span>
<span data-ttu-id="a7dec-146">Amíg az SQL Data Warehouse tartalmaz hello szolgáltatást tábla más adatbázisok által kínált számos, van néhány funkció, amely még nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="a7dec-146">While SQL Data Warehouse contains many of hello same table features offered by other databases, there are some features which are not yet supported.</span></span>  <span data-ttu-id="a7dec-147">Alább olvashat egy listát néhány hello táblában funkciók még nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="a7dec-147">Below is a list of some of hello table features which are not yet supported.</span></span>

| <span data-ttu-id="a7dec-148">Nem támogatott funkciók</span><span class="sxs-lookup"><span data-stu-id="a7dec-148">Unsupported features</span></span> |
| --- |
| <span data-ttu-id="a7dec-149">Elsődleges kulcs, külső kulcsok, egyedi és a jelölőnégyzet [tábla megkötések][Table Constraints]</span><span class="sxs-lookup"><span data-stu-id="a7dec-149">Primary key, Foreign keys, Unique and Check [Table Constraints][Table Constraints]</span></span> |
| <span data-ttu-id="a7dec-150">[Egyedi indexek][Unique Indexes]</span><span class="sxs-lookup"><span data-stu-id="a7dec-150">[Unique Indexes][Unique Indexes]</span></span> |
| <span data-ttu-id="a7dec-151">[A számított oszlopok][Computed Columns]</span><span class="sxs-lookup"><span data-stu-id="a7dec-151">[Computed Columns][Computed Columns]</span></span> |
| <span data-ttu-id="a7dec-152">[Ritka oszlopokat][Sparse Columns]</span><span class="sxs-lookup"><span data-stu-id="a7dec-152">[Sparse Columns][Sparse Columns]</span></span> |
| <span data-ttu-id="a7dec-153">[Felhasználó által definiált típusok][User-Defined Types]</span><span class="sxs-lookup"><span data-stu-id="a7dec-153">[User-Defined Types][User-Defined Types]</span></span> |
| <span data-ttu-id="a7dec-154">[Feladatütemezési][Sequence]</span><span class="sxs-lookup"><span data-stu-id="a7dec-154">[Sequence][Sequence]</span></span> |
| <span data-ttu-id="a7dec-155">[Eseményindítók][Triggers]</span><span class="sxs-lookup"><span data-stu-id="a7dec-155">[Triggers][Triggers]</span></span> |
| <span data-ttu-id="a7dec-156">[Az indexelt nézetek][Indexed Views]</span><span class="sxs-lookup"><span data-stu-id="a7dec-156">[Indexed Views][Indexed Views]</span></span> |
| <span data-ttu-id="a7dec-157">[Szinonimák][Synonyms]</span><span class="sxs-lookup"><span data-stu-id="a7dec-157">[Synonyms][Synonyms]</span></span> |

## <a name="table-size-queries"></a><span data-ttu-id="a7dec-158">Tábla méretét lekérdezések</span><span class="sxs-lookup"><span data-stu-id="a7dec-158">Table size queries</span></span>
<span data-ttu-id="a7dec-159">Egy egyszerű módon tooidentify terület és egy tábla minden hello 60 azokat a terjesztéseket, által felhasznált sorok toouse [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].</span><span class="sxs-lookup"><span data-stu-id="a7dec-159">One simple way tooidentify space and rows consumed by a table in each of hello 60 distributions, is toouse [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].</span></span>

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

<span data-ttu-id="a7dec-160">Azonban a DBCC-parancsokkal is kell meglehetősen korlátozása.</span><span class="sxs-lookup"><span data-stu-id="a7dec-160">However, using DBCC commands can be quite limiting.</span></span>  <span data-ttu-id="a7dec-161">Dinamikus felügyeleti nézetekkel (dinamikus felügyeleti nézetek) lehetővé teszi a toosee sokkal részletességi, valamint hello lekérdezési eredmények sokkal nagyobb szabályozható.</span><span class="sxs-lookup"><span data-stu-id="a7dec-161">Dynamic management views (DMVs) will allow you toosee much more detail as well as give you much greater control over hello query results.</span></span>  <span data-ttu-id="a7dec-162">Először hozzon létre ebben a nézetben, ez az említett tooby jelen példában számos ebben és egyéb.</span><span class="sxs-lookup"><span data-stu-id="a7dec-162">Start by creating this view, which will be referred tooby many of our examples in this and other articles.</span></span>

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

### <a name="table-space-summary"></a><span data-ttu-id="a7dec-163">Összefoglaló táblázat terület</span><span class="sxs-lookup"><span data-stu-id="a7dec-163">Table space summary</span></span>
<span data-ttu-id="a7dec-164">A lekérdezés hello sorok és a hely által visszaadott tábla.</span><span class="sxs-lookup"><span data-stu-id="a7dec-164">This query returns hello rows and space by table.</span></span>  <span data-ttu-id="a7dec-165">A táblák, a legnagyobb táblák, és azok csatlakoznak-e időszeletelés, replikált vagy elosztott kivonatoló kiváló lekérdezés toosee.</span><span class="sxs-lookup"><span data-stu-id="a7dec-165">It is a great query toosee which tables are your largest tables and whether they are round robin, replicated or hash distributed.</span></span>  <span data-ttu-id="a7dec-166">Az elosztott kivonattáblákkal hello terjesztési oszlop is mutatja.</span><span class="sxs-lookup"><span data-stu-id="a7dec-166">For hash distributed tables it also shows hello distribution column.</span></span>  <span data-ttu-id="a7dec-167">A legtöbb esetben a legnagyobb kell szerepelnie. a fürtözött oszlopcentrikus indexszel rendelkező elosztott kivonatát.</span><span class="sxs-lookup"><span data-stu-id="a7dec-167">In most cases your largest tables should be hash distributed with a clustered columnstore index.</span></span>

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

### <a name="table-space-by-distribution-type"></a><span data-ttu-id="a7dec-168">Tábla hely telepítési típusonként</span><span class="sxs-lookup"><span data-stu-id="a7dec-168">Table space by distribution type</span></span>
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

### <a name="table-space-by-index-type"></a><span data-ttu-id="a7dec-169">Tábla terület index típus szerint</span><span class="sxs-lookup"><span data-stu-id="a7dec-169">Table space by index type</span></span>
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

### <a name="distribution-space-summary"></a><span data-ttu-id="a7dec-170">Terjesztési hely összefoglaló</span><span class="sxs-lookup"><span data-stu-id="a7dec-170">Distribution space summary</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="a7dec-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a7dec-171">Next steps</span></span>
<span data-ttu-id="a7dec-172">toolearn több, lásd: hello cikkeket a [tábla adattípusok][Data Types], [terjesztése egy tábla][Distribute], [táblaindexelő] [ Index], [Tábla particionáló][Partition], [fenntartása a tábla statisztikai adatainak] [ Statistics] és [ Az ideiglenes táblák][Temporary].</span><span class="sxs-lookup"><span data-stu-id="a7dec-172">toolearn more, see hello articles on [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition], [Maintaining Table Statistics][Statistics] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="a7dec-173">Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="a7dec-173">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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

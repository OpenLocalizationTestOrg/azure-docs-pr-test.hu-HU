---
title: "Az SQL Data Warehouse Táblák particionálása |} Microsoft Docs"
description: "Első lépések az Azure SQL Data Warehouse táblaparticionálást."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 6cef870c-114f-470c-af10-02300c58885d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 3edfd34d368228be32afef48688739639a3b03ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a><span data-ttu-id="f91e7-103">Az SQL Data Warehouse Táblák particionálása</span><span class="sxs-lookup"><span data-stu-id="f91e7-103">Partitioning tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="f91e7-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="f91e7-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="f91e7-105">[Adattípusok][Data Types]</span><span class="sxs-lookup"><span data-stu-id="f91e7-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="f91e7-106">[Terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="f91e7-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="f91e7-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="f91e7-107">[Index][Index]</span></span>
> * <span data-ttu-id="f91e7-108">[Partíció][Partition]</span><span class="sxs-lookup"><span data-stu-id="f91e7-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="f91e7-109">[Statisztika][Statistics]</span><span class="sxs-lookup"><span data-stu-id="f91e7-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="f91e7-110">[Ideiglenes][Temporary]</span><span class="sxs-lookup"><span data-stu-id="f91e7-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="f91e7-111">Particionálás támogatott az SQL Data Warehouse tábla összes olyan; beleértve a fürtözött oszlopcentrikus, a fürtözött index és a halommemória.</span><span class="sxs-lookup"><span data-stu-id="f91e7-111">Partitioning is supported on all SQL Data Warehouse table types; including clustered columnstore, clustered index, and heap.</span></span>  <span data-ttu-id="f91e7-112">Minden terjesztési típuson, beleértve a kivonatoló vagy a ciklikus multiplexelés elosztott a particionálás is támogatott.</span><span class="sxs-lookup"><span data-stu-id="f91e7-112">Partitioning is also supported on all distribution types, including both hash or round robin distributed.</span></span>  <span data-ttu-id="f91e7-113">Particionálás lehetővé teszi, hogy az adatok felosztani a particionálás kisebb csoportokat az adatok és a legtöbb esetben történik, a dátum oszlop.</span><span class="sxs-lookup"><span data-stu-id="f91e7-113">Partitioning enables you to divide your data into smaller groups of data and in most cases, partitioning is done on a date column.</span></span>

## <a name="benefits-of-partitioning"></a><span data-ttu-id="f91e7-114">Particionálás előnyei</span><span class="sxs-lookup"><span data-stu-id="f91e7-114">Benefits of partitioning</span></span>
<span data-ttu-id="f91e7-115">Particionálás adatok karbantartása és lekérdezéseivel kapcsolatos teljesítményt is kihasználhatja.</span><span class="sxs-lookup"><span data-stu-id="f91e7-115">Partitioning can benefit data maintenance and query performance.</span></span>  <span data-ttu-id="f91e7-116">E előnyöket mindegyikét vagy csak egy függ, hogyan történik az adatok betöltése, és hogy mindkét célokra használhatók ugyanarra az oszlopra óta particionálás csak akkor lehet elvégezni egy oszlop.</span><span class="sxs-lookup"><span data-stu-id="f91e7-116">Whether it benefits both or just one is dependent on how data is loaded and whether the same column can be used for both purposes, since partitioning can only be done on one column.</span></span>

### <a name="benefits-to-loads"></a><span data-ttu-id="f91e7-117">Terhelések előnyei</span><span class="sxs-lookup"><span data-stu-id="f91e7-117">Benefits to loads</span></span>
<span data-ttu-id="f91e7-118">Az elsődleges előnye, hogy az SQL Data Warehouse particionálás a hatékonyság és a partíció törlése használatával adatok betöltését, váltás és egyesítése teljesítményének van javítása.</span><span class="sxs-lookup"><span data-stu-id="f91e7-118">The primary benefit of partitioning in SQL Data Warehouse is improve the efficiency and performance of loading data by use of partition deletion, switching and merging.</span></span>  <span data-ttu-id="f91e7-119">A legtöbb esetben adatok particionálása szorosan a sorozatot, amely az adatok betöltése az adatbázishoz kötődő dátum oszlop.</span><span class="sxs-lookup"><span data-stu-id="f91e7-119">In most cases data is partitioned on a date column that is closely tied to the sequence which the data is loaded to the database.</span></span>  <span data-ttu-id="f91e7-120">Egyik legnagyobb előnye az adatok karbantartása partíciókra használatával, a tranzakció naplózási elkerülését.</span><span class="sxs-lookup"><span data-stu-id="f91e7-120">One of the greatest benefits of using partitions to maintain data it the avoidance of transaction logging.</span></span>  <span data-ttu-id="f91e7-121">Egyszerűen beszúrni, frissítése vagy törlése az adatok lehetnek egy kis gondolat és annak érdekében, a legegyszerűbb módszer használatával a betöltési folyamat során particionálás jelentősen javíthatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="f91e7-121">While simply inserting, updating or deleting data can be the most straightforward approach, with a little thought and effort, using partitioning during your load process can substantially improve performance.</span></span>

<span data-ttu-id="f91e7-122">Váltás a partíció segítségével gyorsan távolítsa el vagy cserélje le a szakasz egy tábla.</span><span class="sxs-lookup"><span data-stu-id="f91e7-122">Partition switching can be used to quickly remove or replace a section of a table.</span></span>  <span data-ttu-id="f91e7-123">Azt jelzi, például értékesítési ténytábla tartalmazhat-e az elmúlt 36 hónap csak az adatokat.</span><span class="sxs-lookup"><span data-stu-id="f91e7-123">For example, a sales fact table might contain just data for the past 36 months.</span></span>  <span data-ttu-id="f91e7-124">Minden hónap végén a legrégebbi hónap értékesítési adatokat töröltek, amelyek a tábla.</span><span class="sxs-lookup"><span data-stu-id="f91e7-124">At the end of every month, the oldest month of sales data is deleted from the table.</span></span>  <span data-ttu-id="f91e7-125">Ezek az adatok delete utasítások segítségével az adatok törléséhez a legrégebbi hónaphoz sikerült törölni.</span><span class="sxs-lookup"><span data-stu-id="f91e7-125">This data could be deleted by using a delete statement to delete the data for the oldest month.</span></span>  <span data-ttu-id="f91e7-126">Azonban nagy mennyiségű adat--soronként egy delete utasítás törlése is nagyon hosszú időt vehet igénybe, valamint a veszéllyel nagy tranzakciók, amelyek hosszú ideig tartana visszagörgetése Ha valamilyen hiba.</span><span class="sxs-lookup"><span data-stu-id="f91e7-126">However, deleting a large amount of data row-by-row with a delete statement can take a very long time, as well as create the risk of large transactions which could take a long time to rollback if something goes wrong.</span></span>  <span data-ttu-id="f91e7-127">A több optimális megközelítés egyszerűen törölni az adatok a legrégebbi partíció.</span><span class="sxs-lookup"><span data-stu-id="f91e7-127">A more optimal approach is to simply drop the oldest partition of data.</span></span>  <span data-ttu-id="f91e7-128">Ha az egyes sorok törléséhez órába is telhet, egy teljes partíció törlése sikerült másodpercre.</span><span class="sxs-lookup"><span data-stu-id="f91e7-128">Where deleting the individual rows could take hours, deleting an entire partition could take seconds.</span></span>

### <a name="benefits-to-queries"></a><span data-ttu-id="f91e7-129">Lekérdezések előnyei</span><span class="sxs-lookup"><span data-stu-id="f91e7-129">Benefits to queries</span></span>
<span data-ttu-id="f91e7-130">Particionálás is segítségével javíthatja a lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="f91e7-130">Partitioning can also be used to improve query performance.</span></span>  <span data-ttu-id="f91e7-131">Ha a lekérdezés egy particionált oszlop szűrőt alkalmaz, ez korlátozhatja a csak a megfelelő partíciók az adatokat, elkerülve a teljes táblázat vizsgálat sokkal kisebb részhalmazát esetleg a vizsgálatot.</span><span class="sxs-lookup"><span data-stu-id="f91e7-131">If a query applies a filter on a partitioned column, this can limit the scan to only the qualifying partitions which may be a much smaller subset of the data, avoiding a full table scan.</span></span>  <span data-ttu-id="f91e7-132">Fürtözött oszlopcentrikus indexek bevezetésével a predikátum eltávolítási teljesítménybeli előnyökben kevesebb előnyös, de egyes esetekben lehet lekérdezések előnyt.</span><span class="sxs-lookup"><span data-stu-id="f91e7-132">With the introduction of clustered columnstore indexes, the predicate elimination performance benefits are less beneficial, but in some cases there can be a benefit to queries.</span></span>  <span data-ttu-id="f91e7-133">Például ha az értékesítési ténytábla az értékesítési dátum mező 36 hónapokra particionálva van, akkor ez a szűrő lekérdezi az értékesítés időpontjában kihagyhatja megkeresése, amelyek nem egyeznek a szűrő partíciókat.</span><span class="sxs-lookup"><span data-stu-id="f91e7-133">For example, if the sales fact table is partitioned into 36 months using the sales date field, then queries that filter on the sale date can skip searching in partitions that don’t match the filter.</span></span>

## <a name="partition-sizing-guidance"></a><span data-ttu-id="f91e7-134">Partíció olvasható méretezési útmutató</span><span class="sxs-lookup"><span data-stu-id="f91e7-134">Partition sizing guidance</span></span>
<span data-ttu-id="f91e7-135">Particionálás teljesítmény javításához bizonyos esetekben használható, amíg a tábla létrehozása **túl sok** partíciók hátrányosan befolyásolhatja a teljesítményt bizonyos körülmények között.</span><span class="sxs-lookup"><span data-stu-id="f91e7-135">While partitioning can be used to improve performance some scenarios, creating a table with **too many** partitions can hurt performance under some circumstances.</span></span>  <span data-ttu-id="f91e7-136">A problémák különösen akkor igaz, a fürtözött oszloptárindexű táblákat.</span><span class="sxs-lookup"><span data-stu-id="f91e7-136">These concerns are especially true for clustered columnstore tables.</span></span>  <span data-ttu-id="f91e7-137">Particionálás hasznosak lehetnek, fontos tudni, mikor érdemes használni a particionálás és létrehozásához a partíciók száma.</span><span class="sxs-lookup"><span data-stu-id="f91e7-137">For partitioning to be helpful, it is important to understand when to use partitioning and the number of partitions to create.</span></span>  <span data-ttu-id="f91e7-138">Nincs rögzített gyors szabály, hogy hány partíció található, a túl sok, attól függ, az adatokat, és hogy hány partíciót, amelyek betöltését egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="f91e7-138">There is no hard fast rule as to how many partitions are too many, it depends on your data and how many partitions you are loading to simultaneously.</span></span>  <span data-ttu-id="f91e7-139">De általános tapasztalatok, mint gondolja, hogy 10 egység a 100-as egység partíciók, nem 1000 egység összeadásának.</span><span class="sxs-lookup"><span data-stu-id="f91e7-139">But as a general rule of thumb, think of adding 10s to 100s of partitions, not 1000s.</span></span>

<span data-ttu-id="f91e7-140">A particionálás létrehozásakor **fürtözött oszlopcentrikus** táblák, fontos figyelembe venni, hogy hány sort megnyílik a minden partíció.</span><span class="sxs-lookup"><span data-stu-id="f91e7-140">When creating partitioning on **clustered columnstore** tables, it is important to consider how many rows will land in each partition.</span></span>  <span data-ttu-id="f91e7-141">Az optimális tömörítés és a fürtözött oszloptárindexű táblákat teljesítményétől legalább 1 millió sort foglalnak terjesztési és partíciónként szükséges.</span><span class="sxs-lookup"><span data-stu-id="f91e7-141">For optimal compression and performance of clustered columnstore tables, a minimum of 1 million rows per distribution and partition is needed.</span></span>  <span data-ttu-id="f91e7-142">Mielőtt partíciók jönnek létre, az SQL Data Warehouse már felosztja minden tábla 60 elosztott adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="f91e7-142">Before partitions are created, SQL Data Warehouse already divides each table into 60 distributed databases.</span></span>  <span data-ttu-id="f91e7-143">A particionálás hozzá van a háttérben létrehozott terjesztéseket mellett.</span><span class="sxs-lookup"><span data-stu-id="f91e7-143">Any partitioning added to a table is in addition to the distributions created behind the scenes.</span></span>  <span data-ttu-id="f91e7-144">Ebben a példában használata, ha az értékesítési ténytábla található 36 havi partíciók, és mivel, hogy az SQL Data Warehouse 60 azokat a terjesztéseket, majd az értékesítési ténytábla kell tartalmaznia 60 millió sort foglalnak havonta vagy 2.1 egymilliárd sort minden hónap fel van töltve.</span><span class="sxs-lookup"><span data-stu-id="f91e7-144">Using this example, if the sales fact table contained 36 monthly partitions, and given that SQL Data Warehouse has 60 distributions, then the sales fact table should contain 60 million rows per month, or 2.1 billion rows when all months are populated.</span></span>  <span data-ttu-id="f91e7-145">Ha a tábla Sorszám ajánlott minimális száma jóval kevesebb sort tartalmaz, érdemes lehet kevesebb partíciók annak érdekében, hogy az partíciónként sorok számának növeléséhez.</span><span class="sxs-lookup"><span data-stu-id="f91e7-145">If a table contains significantly less rows than the recommended minimum number of rows per partition, consider using fewer partitions in order to make increase the number of rows per partition.</span></span>  <span data-ttu-id="f91e7-146">Lásd még: a [indexelő] [ Index] cikket, amely tartalmazza az SQL Data Warehouse minőségének fürt oszlopcentrikus indexek futó lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="f91e7-146">Also see the [Indexing][Index] article which includes queries that can be run on SQL Data Warehouse to assess the quality of cluster columnstore indexes.</span></span>

## <a name="syntax-difference-from-sql-server"></a><span data-ttu-id="f91e7-147">Az SQL Server szintaktikai különbség</span><span class="sxs-lookup"><span data-stu-id="f91e7-147">Syntax difference from SQL Server</span></span>
<span data-ttu-id="f91e7-148">SQL Data Warehouse bevezeti a partíciók egy egyszerűsített definíciót, amely az SQL Server kis mértékben eltér.</span><span class="sxs-lookup"><span data-stu-id="f91e7-148">SQL Data Warehouse introduces a simplified definition of partitions which is slightly different from SQL Server.</span></span>  <span data-ttu-id="f91e7-149">Particionálási függvény és sémák nem használják az SQL Data Warehouse szerint az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f91e7-149">Partitioning functions and schemes are not used in SQL Data Warehouse as they are in SQL Server.</span></span>  <span data-ttu-id="f91e7-150">Ehelyett a teendő szüksége a particionált oszlop és a határ pontok azonosítására.</span><span class="sxs-lookup"><span data-stu-id="f91e7-150">Instead, all you need to do is identify partitioned column and the boundary points.</span></span>  <span data-ttu-id="f91e7-151">Bár a particionálás szintaxisát némileg eltérő SQL Server, az alapvető fogalmakat azonosak.</span><span class="sxs-lookup"><span data-stu-id="f91e7-151">While the syntax of partitioning may be slightly different from SQL Server, the basic concepts are the same.</span></span>  <span data-ttu-id="f91e7-152">SQL Server és az SQL Data Warehouse támogatja táblánként, amely címkiosztási partíció lehet egy partícióoszlop.</span><span class="sxs-lookup"><span data-stu-id="f91e7-152">SQL Server and SQL Data Warehouse support one partition column per table, which can be ranged partition.</span></span>  <span data-ttu-id="f91e7-153">Particionálás kapcsolatos további információkért lásd: [particionált táblák és -indexek][Partitioned Tables and Indexes].</span><span class="sxs-lookup"><span data-stu-id="f91e7-153">To learn more about partitioning, see [Partitioned Tables and Indexes][Partitioned Tables and Indexes].</span></span>

<span data-ttu-id="f91e7-154">Az alábbi példában egy SQL Data warehouse particionálva [CREATE TABLE] [ CREATE TABLE] utasítás, particionálja a FactInternetSales táblának OrderDateKey oszlopon:</span><span class="sxs-lookup"><span data-stu-id="f91e7-154">The below example of a SQL Data Warehouse partitioned [CREATE TABLE][CREATE TABLE] statement, partitions the FactInternetSales table on the OrderDateKey column:</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a><span data-ttu-id="f91e7-155">Az SQL Server particionálás áttelepítése</span><span class="sxs-lookup"><span data-stu-id="f91e7-155">Migrating partitioning from SQL Server</span></span>
<span data-ttu-id="f91e7-156">Egyszerűen csak az SQL Data Warehouse áttelepítése az SQL Server partíció definíciók:</span><span class="sxs-lookup"><span data-stu-id="f91e7-156">To migrate SQL Server partition definitions to SQL Data Warehouse simply:</span></span>

* <span data-ttu-id="f91e7-157">Az SQL Server kiküszöbölése [partícióséma][partition scheme].</span><span class="sxs-lookup"><span data-stu-id="f91e7-157">Eliminate the SQL Server [partition scheme][partition scheme].</span></span>
* <span data-ttu-id="f91e7-158">Adja hozzá a [partíciós függvényei] [ partition function] a CREATE TABLE-definíciót.</span><span class="sxs-lookup"><span data-stu-id="f91e7-158">Add the [partition function][partition function] definition to your CREATE TABLE.</span></span>

<span data-ttu-id="f91e7-159">Ha egy particionált táblához telepít egy SQL Server-példány az SQL alatt segítenek kérdezze meg végre lévő egyes partíciók sorok száma.</span><span class="sxs-lookup"><span data-stu-id="f91e7-159">If you are migrating a partitioned table from a SQL Server instance the below SQL can help you to interrogate the number of rows that are in each partition.</span></span>  <span data-ttu-id="f91e7-160">Ne feledje, hogy ha olyan particionálási részletességgel használatban van az SQL Data Warehouse, partíciónként sorok száma csökken, 60 tényezővel.</span><span class="sxs-lookup"><span data-stu-id="f91e7-160">Keep in mind that if the same partitioning granularity is used on SQL Data Warehouse, the number of rows per partition will decrease by a factor of 60.</span></span>  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a><span data-ttu-id="f91e7-161">Terheléskezelés</span><span class="sxs-lookup"><span data-stu-id="f91e7-161">Workload management</span></span>
<span data-ttu-id="f91e7-162">A tábla partíciós döntés számításba a egy végső építőelemre szempont, hogy [munkaterhelés felügyeleti][workload management].</span><span class="sxs-lookup"><span data-stu-id="f91e7-162">One final piece consideration to factor in to the table partition decision is [workload management][workload management].</span></span>  <span data-ttu-id="f91e7-163">Munkaterhelés-kezelés az SQL Data Warehouse az elsősorban a memória és a párhuzamosság management.</span><span class="sxs-lookup"><span data-stu-id="f91e7-163">Workload management in SQL Data Warehouse is primarily the management of memory and concurrency.</span></span>  <span data-ttu-id="f91e7-164">Az SQL Data Warehouse a lekérdezés végrehajtása során minden egyes terjesztési számára lefoglalt maximális memória szabályoz erőforrás osztály.</span><span class="sxs-lookup"><span data-stu-id="f91e7-164">In SQL Data Warehouse the maximum memory allocated to each distribution during query execution is governed resource classes.</span></span>  <span data-ttu-id="f91e7-165">Ideális esetben a partíciók méretezi más tényezőktől, például a fürtözött oszlopcentrikus indexek felépítése memóriaigényét figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="f91e7-165">Ideally your partitions will be sized in consideration of other factors like the memory needs of building clustered columnstore indexes.</span></span>  <span data-ttu-id="f91e7-166">Nagy mértékben fürtözött oszlopcentrikus indexek juttatásra, amikor azok több memóriát foglal le.</span><span class="sxs-lookup"><span data-stu-id="f91e7-166">Clustered columnstore indexes benefit greatly when they are allocated more memory.</span></span>  <span data-ttu-id="f91e7-167">Ezért érdemes győződjön meg arról, hogy a partíció index újraépítése nem függeszteni memória.</span><span class="sxs-lookup"><span data-stu-id="f91e7-167">Therefore, you will want to ensure that a partition index rebuild is not starved of memory.</span></span> <span data-ttu-id="f91e7-168">A lekérdezés számára elérhető memória mennyiségének növelését naplókról smallrc, az alapértelmezett szerepkör egyik szerepkör largerc például lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="f91e7-168">Increasing the amount of memory available to your query can be achieved by switching from the default role, smallrc, to one of the other roles such as largerc.</span></span>

<span data-ttu-id="f91e7-169">A kiosztását) eloszlása feladatonként (memória áll rendelkezésre információ az erőforrás-vezérlő dinamikus felügyeleti nézetekkel lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="f91e7-169">Information on the allocation of memory per distribution is available by querying the resource governor dynamic management views.</span></span> <span data-ttu-id="f91e7-170">A valóságban a memóriabeli ideiglenes lesz kisebb, mint az alábbi ábrán.</span><span class="sxs-lookup"><span data-stu-id="f91e7-170">In reality your memory grant will be less than the figures below.</span></span> <span data-ttu-id="f91e7-171">Azonban ez biztosít, amelyekkel a partíciók felügyeleti műveletek osztályozás útmutatás.</span><span class="sxs-lookup"><span data-stu-id="f91e7-171">However, this provides a level of guidance that you can use when sizing your partitions for data management operations.</span></span>  <span data-ttu-id="f91e7-172">Lehetőleg kerülje a partíciók túl az extra nagy erőforrás osztály memóriaengedély méretezése.</span><span class="sxs-lookup"><span data-stu-id="f91e7-172">Try to avoid sizing your partitions beyond the memory grant provided by the extra large resource class.</span></span> <span data-ttu-id="f91e7-173">Ha ez a szám nagyobb legyen a partíciók futtatja a memóriaterhelése, ami viszont kevésbé optimális tömörítési kockázatát.</span><span class="sxs-lookup"><span data-stu-id="f91e7-173">If your partitions grow beyond this figure you run the risk of memory pressure which in turn leads to less optimal compression.</span></span>

```sql
SELECT  rp.[name]                                AS [pool_name]
,       rp.[max_memory_kb]                        AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                    AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576                AS [mex_memory_gb]
,       rp.[max_memory_percent]                    AS [max_memory_percent]
,       wg.[name]                                AS [group_name]
,       wg.[importance]                            AS [group_importance]
,       wg.[request_max_memory_grant_percent]    AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups    wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools    rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a><span data-ttu-id="f91e7-174">Váltás a partíció</span><span class="sxs-lookup"><span data-stu-id="f91e7-174">Partition switching</span></span>
<span data-ttu-id="f91e7-175">Az SQL Data Warehouse a felosztás egyesítése és váltás partíció támogatja.</span><span class="sxs-lookup"><span data-stu-id="f91e7-175">SQL Data Warehouse supports partition splitting, merging, and switching.</span></span> <span data-ttu-id="f91e7-176">Ezek az excuted használatával a [az ALTER TABLE] [ ALTER TABLE] utasítást.</span><span class="sxs-lookup"><span data-stu-id="f91e7-176">Each of these functions is excuted using the [ALTER TABLE][ALTER TABLE] statement.</span></span>

<span data-ttu-id="f91e7-177">Váltás a partíciók két tábla között gondoskodnia kell arról, hogy a partíciók igazodnak a megfelelő határokon belül, és, hogy megfelel-e a definíciói.</span><span class="sxs-lookup"><span data-stu-id="f91e7-177">To switch partitions between two tables you must ensure that the partitions align on their respective boundaries and that the table definitions match.</span></span> <span data-ttu-id="f91e7-178">Nincsenek elérhető értékek a táblázat kényszerítéséhez ellenőrző korlátozásokat a forrástábla a azonos partícióhatárok, mint a céltábla kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="f91e7-178">As check constraints are not available to enforce the range of values in a table the source table must contain the same partition boundaries as the target table.</span></span> <span data-ttu-id="f91e7-179">Ha nem ez a helyzet, majd a partíció kapcsolójának sikertelen lesz, mivel a partíciós metaadatok nem szinkronizálja.</span><span class="sxs-lookup"><span data-stu-id="f91e7-179">If this is not the case, then the partition switch will fail as the partition metadata will not be synchronized.</span></span>

### <a name="how-to-split-a-partition-that-contains-data"></a><span data-ttu-id="f91e7-180">Hogyan leíró adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f91e7-180">How to split a partition that contains data</span></span>
<span data-ttu-id="f91e7-181">A leghatékonyabb módszer leválasztására már tartalmazza az adatokat egy `CTAS` utasítást.</span><span class="sxs-lookup"><span data-stu-id="f91e7-181">The most efficient method to split a partition that already contains data is to use a `CTAS` statement.</span></span> <span data-ttu-id="f91e7-182">Ha a particionált tábla fürtözött oszlopcentrikus majd a tábla partíciós üresnek kell lennie ahhoz lehessen.</span><span class="sxs-lookup"><span data-stu-id="f91e7-182">If the partitioned table is a clustered columnstore then the table partition must be empty before it can be split.</span></span>

<span data-ttu-id="f91e7-183">Alább egy sor minden partíció tartalmazó minta particionált oszlopcentrikus tábla lesz:</span><span class="sxs-lookup"><span data-stu-id="f91e7-183">Below is a sample partitioned columnstore table containing one row in each partition:</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [!NOTE]
> <span data-ttu-id="f91e7-184">A statisztika-objektum létrehozásával azt győződjön meg arról, hogy a tábla metaadatainak pontosabb.</span><span class="sxs-lookup"><span data-stu-id="f91e7-184">By Creating the statistic object, we ensure that table metadata is more accurate.</span></span> <span data-ttu-id="f91e7-185">Jelenleg nincs megadva statisztikák létrehozása, ha az SQL Data Warehouse alapértelmezett értékeket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="f91e7-185">If we omit creating statistics, then SQL Data Warehouse will use default values.</span></span> <span data-ttu-id="f91e7-186">A statisztika részletes tekintse át [statisztika][statistics].</span><span class="sxs-lookup"><span data-stu-id="f91e7-186">For details on statistics please review [statistics][statistics].</span></span>
> 
> 

<span data-ttu-id="f91e7-187">Azt is majd kereshet a sor számának használatával a `sys.partitions` katalógus megtekintése:</span><span class="sxs-lookup"><span data-stu-id="f91e7-187">We can then query for the row count using the `sys.partitions` catalog view:</span></span>

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

<span data-ttu-id="f91e7-188">Próbálja meg ossza fel ebben a táblázatban, ha azt egy hiba jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="f91e7-188">If we try to split this table, we will get an error:</span></span>

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

<span data-ttu-id="f91e7-189">Üzenet 35346, szint 15 állapot 1, az ALTER PARTITION utasítás sor 44 SPLIT záradékának sikertelen volt, mert a partíció nem üres.</span><span class="sxs-lookup"><span data-stu-id="f91e7-189">Msg 35346, Level 15, State 1, Line 44 SPLIT clause of ALTER PARTITION statement failed because the partition is not empty.</span></span>  <span data-ttu-id="f91e7-190">Csak üres partíciók oszthatók fel oszlopcentrikus indexet a tábla létezik.</span><span class="sxs-lookup"><span data-stu-id="f91e7-190">Only empty partitions can be split in when a columnstore index exists on the table.</span></span> <span data-ttu-id="f91e7-191">Fontolja meg az oszloptárindex letiltását előtt az ALTER PARTITION utasítás kiadása, majd az oszloptárindex újraépítését az ALTER PARTITION utasítás befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="f91e7-191">Consider disabling the columnstore index before issuing the ALTER PARTITION statement, then rebuilding the columnstore index after ALTER PARTITION is complete.</span></span>

<span data-ttu-id="f91e7-192">Azonban használhatjuk `CTAS` ahhoz, hogy az adatok új tábla létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f91e7-192">However, we can use `CTAS` to create a new table to hold our data.</span></span>

```sql
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
FROM    FactInternetSales
WHERE   1=2
;
```

<span data-ttu-id="f91e7-193">A partícióhatárok vannak rendezve, mivel a kapcsoló megengedett.</span><span class="sxs-lookup"><span data-stu-id="f91e7-193">As the partition boundaries are aligned a switch is permitted.</span></span> <span data-ttu-id="f91e7-194">Így marad, hogy a forrástábla egy üres partícióból, azt később fel.</span><span class="sxs-lookup"><span data-stu-id="f91e7-194">This will leave the source table with an empty partition that we can subsequently split.</span></span>

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

<span data-ttu-id="f91e7-195">Ehhez marad csak akkor igazíthatók az adatokat az új partíció határokat a `CTAS` és az adatok ismét a fő tábla kapcsoló</span><span class="sxs-lookup"><span data-stu-id="f91e7-195">All that is left to do is to align our data to the new partition boundaries using `CTAS` and switch our data back in to the main table</span></span>

```sql
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
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

<span data-ttu-id="f91e7-196">Az adatok mozgása befejezése után célszerű a céltábla annak érdekében, hogy tükrözik az adatok a megfelelő partícióikba új terjesztési statisztika frissítése:</span><span class="sxs-lookup"><span data-stu-id="f91e7-196">Once you have completed the movement of the data it is a good idea to refresh the statistics on the target table to ensure they accurately reflect the new distribution of the data in their respective partitions:</span></span>

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a><span data-ttu-id="f91e7-197">A verziókövetési rendszerrel particionálás tábla</span><span class="sxs-lookup"><span data-stu-id="f91e7-197">Table partitioning source control</span></span>
<span data-ttu-id="f91e7-198">A tábla-definíció elkerülése érdekében **Rozsdás** a vezérlő forrásrendszerben érdemes figyelembe venni a következő módon:</span><span class="sxs-lookup"><span data-stu-id="f91e7-198">To avoid your table definition from **rusting** in your source control system you may want to consider the following approach:</span></span>

1. <span data-ttu-id="f91e7-199">Hozzon létre a tábla particionált tábla megegyezik, azonban nincsenek partíció értékek</span><span class="sxs-lookup"><span data-stu-id="f91e7-199">Create the table as a partitioned table but with no partition values</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

1. <span data-ttu-id="f91e7-200">`SPLIT`a tábla a telepítési folyamat részeként:</span><span class="sxs-lookup"><span data-stu-id="f91e7-200">`SPLIT` the table as part of the deployment process:</span></span>

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

<span data-ttu-id="f91e7-201">Ezt a módszert a kódot a verziókövetési statikus marad, és a particionálási tartományhatár-értékek használata engedélyezett a dinamikus; fejlesztik, az adatraktár adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="f91e7-201">With this approach the code in source control remains static and the partitioning boundary values are allowed to be dynamic; evolving with the warehouse over time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f91e7-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f91e7-202">Next steps</span></span>
<span data-ttu-id="f91e7-203">További tudnivalókért tekintse meg a cikkek [tábla áttekintése][Overview], [tábla adattípusok][Data Types], [terjesztése egy tábla] [ Distribute], [Tábla indexelő][Index], [fenntartása a tábla statisztikai adatainak] [ Statistics] és [Az ideiglenes táblák][Temporary].</span><span class="sxs-lookup"><span data-stu-id="f91e7-203">To learn more, see the articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index], [Maintaining Table Statistics][Statistics] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="f91e7-204">Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="f91e7-204">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[workload management]: ./sql-data-warehouse-develop-concurrency.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Partitioned Tables and Indexes]: https://msdn.microsoft.com/library/ms190787.aspx
[ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[partition function]: https://msdn.microsoft.com/library/ms187802.aspx
[partition scheme]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->

---
title: "az SQL Data Warehouse aaaPartitioning táblák |} Microsoft Docs"
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
ms.openlocfilehash: aa63c51562f3e6f83063320860b195e135a721e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a><span data-ttu-id="14596-103">Az SQL Data Warehouse Táblák particionálása</span><span class="sxs-lookup"><span data-stu-id="14596-103">Partitioning tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="14596-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="14596-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="14596-105">[Adattípusok][Data Types]</span><span class="sxs-lookup"><span data-stu-id="14596-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="14596-106">[Terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="14596-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="14596-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="14596-107">[Index][Index]</span></span>
> * <span data-ttu-id="14596-108">[Partíció][Partition]</span><span class="sxs-lookup"><span data-stu-id="14596-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="14596-109">[Statisztika][Statistics]</span><span class="sxs-lookup"><span data-stu-id="14596-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="14596-110">[Ideiglenes][Temporary]</span><span class="sxs-lookup"><span data-stu-id="14596-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="14596-111">Particionálás támogatott az SQL Data Warehouse tábla összes olyan; beleértve a fürtözött oszlopcentrikus, a fürtözött index és a halommemória.</span><span class="sxs-lookup"><span data-stu-id="14596-111">Partitioning is supported on all SQL Data Warehouse table types; including clustered columnstore, clustered index, and heap.</span></span>  <span data-ttu-id="14596-112">Minden terjesztési típuson, beleértve a kivonatoló vagy a ciklikus multiplexelés elosztott a particionálás is támogatott.</span><span class="sxs-lookup"><span data-stu-id="14596-112">Partitioning is also supported on all distribution types, including both hash or round robin distributed.</span></span>  <span data-ttu-id="14596-113">Particionálás lehetővé teszi a dátum oszlop történik az adatok particionálása kisebb csoportokra és a legtöbb esetben az adatok toodivide.</span><span class="sxs-lookup"><span data-stu-id="14596-113">Partitioning enables you toodivide your data into smaller groups of data and in most cases, partitioning is done on a date column.</span></span>

## <a name="benefits-of-partitioning"></a><span data-ttu-id="14596-114">Particionálás előnyei</span><span class="sxs-lookup"><span data-stu-id="14596-114">Benefits of partitioning</span></span>
<span data-ttu-id="14596-115">Particionálás adatok karbantartása és lekérdezéseivel kapcsolatos teljesítményt is kihasználhatja.</span><span class="sxs-lookup"><span data-stu-id="14596-115">Partitioning can benefit data maintenance and query performance.</span></span>  <span data-ttu-id="14596-116">E előnyöket mindegyikét vagy csak egy függ, hogyan történik az adatok betöltése, és hogy hello ugyanarra az oszlopra használhatók mindkét célokra, mivel particionálás csak akkor lehet elvégezni egy oszlop.</span><span class="sxs-lookup"><span data-stu-id="14596-116">Whether it benefits both or just one is dependent on how data is loaded and whether hello same column can be used for both purposes, since partitioning can only be done on one column.</span></span>

### <a name="benefits-tooloads"></a><span data-ttu-id="14596-117">Előnyöket tooloads</span><span class="sxs-lookup"><span data-stu-id="14596-117">Benefits tooloads</span></span>
<span data-ttu-id="14596-118">hello elsődleges előnye, hogy az SQL Data Warehouse particionálás hello hatékonyságát és a partíció törlése használatával az adatok betöltéséhez, váltás és egyesítése teljesítményének van javítása.</span><span class="sxs-lookup"><span data-stu-id="14596-118">hello primary benefit of partitioning in SQL Data Warehouse is improve hello efficiency and performance of loading data by use of partition deletion, switching and merging.</span></span>  <span data-ttu-id="14596-119">A legtöbb esetben adatok particionálása napon oszloptól szorosan toohello feladatütemezési hello adatok betöltött toohello adatbázis társítva.</span><span class="sxs-lookup"><span data-stu-id="14596-119">In most cases data is partitioned on a date column that is closely tied toohello sequence which hello data is loaded toohello database.</span></span>  <span data-ttu-id="14596-120">Az egyik legnagyobb előnyei hello a partíciók toomaintain adatokat a tranzakció naplózási elkerülő hello azt.</span><span class="sxs-lookup"><span data-stu-id="14596-120">One of hello greatest benefits of using partitions toomaintain data it hello avoidance of transaction logging.</span></span>  <span data-ttu-id="14596-121">Egyszerűen beszúrni, frissítése vagy törlése az adatok lehetnek hello legegyszerűbb módszer, egy kis gondolat és erőfeszítést, használja a betöltési folyamat során particionálás jelentősen javíthatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="14596-121">While simply inserting, updating or deleting data can be hello most straightforward approach, with a little thought and effort, using partitioning during your load process can substantially improve performance.</span></span>

<span data-ttu-id="14596-122">Partíció váltás használt tooquickly távolítsa el vagy cserélje le a szakasz egy tábla.</span><span class="sxs-lookup"><span data-stu-id="14596-122">Partition switching can be used tooquickly remove or replace a section of a table.</span></span>  <span data-ttu-id="14596-123">Például egy értékesítési ténytábla tartalmazhat csak az adatokat a hello 36 elmúlt hónap.</span><span class="sxs-lookup"><span data-stu-id="14596-123">For example, a sales fact table might contain just data for hello past 36 months.</span></span>  <span data-ttu-id="14596-124">Minden hónap végén hello hello legrégebbi hónapja ábrázolhatja az értékesítési adatokat töröltek, amelyek hello tábla.</span><span class="sxs-lookup"><span data-stu-id="14596-124">At hello end of every month, hello oldest month of sales data is deleted from hello table.</span></span>  <span data-ttu-id="14596-125">Ezek az adatok használatával egy utasítás toodelete hello adatok hello legrégebbi hónap sikerült törölni.</span><span class="sxs-lookup"><span data-stu-id="14596-125">This data could be deleted by using a delete statement toodelete hello data for hello oldest month.</span></span>  <span data-ttu-id="14596-126">Azonban nagy mennyiségű adat--soronként egy delete utasítás törlése is nagyon hosszú időt vehet igénybe, valamint hello veszéllyel nagy tranzakciók, amelyek egy hosszú ideig toorollback is igénybe vehet, ha valamilyen hiba.</span><span class="sxs-lookup"><span data-stu-id="14596-126">However, deleting a large amount of data row-by-row with a delete statement can take a very long time, as well as create hello risk of large transactions which could take a long time toorollback if something goes wrong.</span></span>  <span data-ttu-id="14596-127">A több optimális megközelítés toosimply eldobási hello legrégebbi partíció adatok.</span><span class="sxs-lookup"><span data-stu-id="14596-127">A more optimal approach is toosimply drop hello oldest partition of data.</span></span>  <span data-ttu-id="14596-128">Ha hello egyedi sorok törléséhez órába is telhet, egy teljes partíció törlése sikerült másodpercre.</span><span class="sxs-lookup"><span data-stu-id="14596-128">Where deleting hello individual rows could take hours, deleting an entire partition could take seconds.</span></span>

### <a name="benefits-tooqueries"></a><span data-ttu-id="14596-129">Előnyöket tooqueries</span><span class="sxs-lookup"><span data-stu-id="14596-129">Benefits tooqueries</span></span>
<span data-ttu-id="14596-130">Particionálás használt tooimprove lekérdezési teljesítmény is lehet.</span><span class="sxs-lookup"><span data-stu-id="14596-130">Partitioning can also be used tooimprove query performance.</span></span>  <span data-ttu-id="14596-131">Ha a lekérdezés egy particionált oszlop szűrőt alkalmaz, ez korlátozhatja hello vizsgálat tooonly hello esetleg hello adatok, elkerülve a teljes táblázat vizsgálat sokkal kisebb részhalmazát partíciók jogosult.</span><span class="sxs-lookup"><span data-stu-id="14596-131">If a query applies a filter on a partitioned column, this can limit hello scan tooonly hello qualifying partitions which may be a much smaller subset of hello data, avoiding a full table scan.</span></span>  <span data-ttu-id="14596-132">A fürtözött oszlopcentrikus indexek hello bevezetése hello predikátum eltávolítási teljesítménybeli előnyökben kevesebb előnyös, de egyes esetekben lehet egy juttatás tooqueries.</span><span class="sxs-lookup"><span data-stu-id="14596-132">With hello introduction of clustered columnstore indexes, hello predicate elimination performance benefits are less beneficial, but in some cases there can be a benefit tooqueries.</span></span>  <span data-ttu-id="14596-133">Például ha hello értékesítési ténytábla hello értékesítési dátum mező 36 hónapokra particionálva van, majd hello pénztári dátum szűrő lekérdezéseket ugorjon hello szűrő nem egyező partíciók megkeresése.</span><span class="sxs-lookup"><span data-stu-id="14596-133">For example, if hello sales fact table is partitioned into 36 months using hello sales date field, then queries that filter on hello sale date can skip searching in partitions that don’t match hello filter.</span></span>

## <a name="partition-sizing-guidance"></a><span data-ttu-id="14596-134">Partíció olvasható méretezési útmutató</span><span class="sxs-lookup"><span data-stu-id="14596-134">Partition sizing guidance</span></span>
<span data-ttu-id="14596-135">Particionálás közben lehet használt tooimprove teljesítmény bizonyos esetekben a tábla létrehozása **túl sok** partíciók hátrányosan befolyásolhatja a teljesítményt bizonyos körülmények között.</span><span class="sxs-lookup"><span data-stu-id="14596-135">While partitioning can be used tooimprove performance some scenarios, creating a table with **too many** partitions can hurt performance under some circumstances.</span></span>  <span data-ttu-id="14596-136">A problémák különösen akkor igaz, a fürtözött oszloptárindexű táblákat.</span><span class="sxs-lookup"><span data-stu-id="14596-136">These concerns are especially true for clustered columnstore tables.</span></span>  <span data-ttu-id="14596-137">Fontos toounderstand esetén particionálás toobe hasznos, ha toouse particionálás és hello száma partíciók toocreate.</span><span class="sxs-lookup"><span data-stu-id="14596-137">For partitioning toobe helpful, it is important toounderstand when toouse partitioning and hello number of partitions toocreate.</span></span>  <span data-ttu-id="14596-138">Nincs toohow nem rögzített gyors szabályát sok partíció található, a túl sok, attól függ, az adatokat, és hány particionálja toosimultaneously tölt be.</span><span class="sxs-lookup"><span data-stu-id="14596-138">There is no hard fast rule as toohow many partitions are too many, it depends on your data and how many partitions you are loading toosimultaneously.</span></span>  <span data-ttu-id="14596-139">Általános tapasztalatok, mint gondolja, hogy 10 egység összeadásának, de a partíciók, nem 1000 egység too100s.</span><span class="sxs-lookup"><span data-stu-id="14596-139">But as a general rule of thumb, think of adding 10s too100s of partitions, not 1000s.</span></span>

<span data-ttu-id="14596-140">A particionálás létrehozásakor **fürtözött oszlopcentrikus** táblák, akkor fontos, hogy hány sort megnyílik a minden partíció tooconsider.</span><span class="sxs-lookup"><span data-stu-id="14596-140">When creating partitioning on **clustered columnstore** tables, it is important tooconsider how many rows will land in each partition.</span></span>  <span data-ttu-id="14596-141">Az optimális tömörítés és a fürtözött oszloptárindexű táblákat teljesítményétől legalább 1 millió sort foglalnak terjesztési és partíciónként szükséges.</span><span class="sxs-lookup"><span data-stu-id="14596-141">For optimal compression and performance of clustered columnstore tables, a minimum of 1 million rows per distribution and partition is needed.</span></span>  <span data-ttu-id="14596-142">Mielőtt partíciók jönnek létre, az SQL Data Warehouse már felosztja minden tábla 60 elosztott adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="14596-142">Before partitions are created, SQL Data Warehouse already divides each table into 60 distributed databases.</span></span>  <span data-ttu-id="14596-143">Bármely particionálási hozzáadott tooa tábla továbbá hello háttérben létrehozott toohello terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="14596-143">Any partitioning added tooa table is in addition toohello distributions created behind hello scenes.</span></span>  <span data-ttu-id="14596-144">Ebben a példában használata, ha hello értékesítési ténytábla 36 havi partíció található, és fényében, hogy az SQL Data Warehouse 60 azokat a terjesztéseket, majd hello értékesítési tény tábla kell tartalmaznia 60 millió sort foglalnak havonta vagy 2.1 egymilliárd sort, ha minden hónap fel van töltve.</span><span class="sxs-lookup"><span data-stu-id="14596-144">Using this example, if hello sales fact table contained 36 monthly partitions, and given that SQL Data Warehouse has 60 distributions, then hello sales fact table should contain 60 million rows per month, or 2.1 billion rows when all months are populated.</span></span>  <span data-ttu-id="14596-145">Ha a tábla lényegesen kevesebb mint hello ajánlott minimális száma sorszám sorokat tartalmaz, érdemes kevesebb partíciók rendelés toomake növekedése hello kötegenkénti sorok száma partíció.</span><span class="sxs-lookup"><span data-stu-id="14596-145">If a table contains significantly less rows than hello recommended minimum number of rows per partition, consider using fewer partitions in order toomake increase hello number of rows per partition.</span></span>  <span data-ttu-id="14596-146">Lásd még: hello [indexelő] [ Index] cikket, amely tartalmazza az SQL Data Warehouse tooassess hello minőségének fürt oszlopcentrikus indexek futó lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="14596-146">Also see hello [Indexing][Index] article which includes queries that can be run on SQL Data Warehouse tooassess hello quality of cluster columnstore indexes.</span></span>

## <a name="syntax-difference-from-sql-server"></a><span data-ttu-id="14596-147">Az SQL Server szintaktikai különbség</span><span class="sxs-lookup"><span data-stu-id="14596-147">Syntax difference from SQL Server</span></span>
<span data-ttu-id="14596-148">SQL Data Warehouse bevezeti a partíciók egy egyszerűsített definíciót, amely az SQL Server kis mértékben eltér.</span><span class="sxs-lookup"><span data-stu-id="14596-148">SQL Data Warehouse introduces a simplified definition of partitions which is slightly different from SQL Server.</span></span>  <span data-ttu-id="14596-149">Particionálási függvény és sémák nem használják az SQL Data Warehouse szerint az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="14596-149">Partitioning functions and schemes are not used in SQL Data Warehouse as they are in SQL Server.</span></span>  <span data-ttu-id="14596-150">Ehelyett toodo szüksége a particionált oszlop és hello határ pontok azonosításához.</span><span class="sxs-lookup"><span data-stu-id="14596-150">Instead, all you need toodo is identify partitioned column and hello boundary points.</span></span>  <span data-ttu-id="14596-151">Bár a particionálás hello szintaxisát némileg eltérő SQL Server, hello főbb fogalmait és kifejezéseit vannak hello azonos.</span><span class="sxs-lookup"><span data-stu-id="14596-151">While hello syntax of partitioning may be slightly different from SQL Server, hello basic concepts are hello same.</span></span>  <span data-ttu-id="14596-152">SQL Server és az SQL Data Warehouse támogatja táblánként, amely címkiosztási partíció lehet egy partícióoszlop.</span><span class="sxs-lookup"><span data-stu-id="14596-152">SQL Server and SQL Data Warehouse support one partition column per table, which can be ranged partition.</span></span>  <span data-ttu-id="14596-153">toolearn particionálás, bővebben lásd: [particionált táblák és -indexek][Partitioned Tables and Indexes].</span><span class="sxs-lookup"><span data-stu-id="14596-153">toolearn more about partitioning, see [Partitioned Tables and Indexes][Partitioned Tables and Indexes].</span></span>

<span data-ttu-id="14596-154">hello az alábbi példában egy SQL Data warehouse particionálva [CREATE TABLE] [ CREATE TABLE] utasítás, partíciók hello FactInternetSales táblának hello OrderDateKey oszlop alapján:</span><span class="sxs-lookup"><span data-stu-id="14596-154">hello below example of a SQL Data Warehouse partitioned [CREATE TABLE][CREATE TABLE] statement, partitions hello FactInternetSales table on hello OrderDateKey column:</span></span>

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

## <a name="migrating-partitioning-from-sql-server"></a><span data-ttu-id="14596-155">Az SQL Server particionálás áttelepítése</span><span class="sxs-lookup"><span data-stu-id="14596-155">Migrating partitioning from SQL Server</span></span>
<span data-ttu-id="14596-156">toomigrate SQL Server egyszerűen partícióazonosító definíciók tooSQL Data warehouse-bA:</span><span class="sxs-lookup"><span data-stu-id="14596-156">toomigrate SQL Server partition definitions tooSQL Data Warehouse simply:</span></span>

* <span data-ttu-id="14596-157">SQL Server hello kiküszöbölése [partícióséma][partition scheme].</span><span class="sxs-lookup"><span data-stu-id="14596-157">Eliminate hello SQL Server [partition scheme][partition scheme].</span></span>
* <span data-ttu-id="14596-158">Adja hozzá a hello [partíciós függvényei] [ partition function] definition tooyour tábla létrehozása.</span><span class="sxs-lookup"><span data-stu-id="14596-158">Add hello [partition function][partition function] definition tooyour CREATE TABLE.</span></span>

<span data-ttu-id="14596-159">Ha egy SQL Server-példány hello alatt SQL telepít egy particionált táblához segítségével is szerepelnek, mindegyik partíció sorok számának toointerrogate hello.</span><span class="sxs-lookup"><span data-stu-id="14596-159">If you are migrating a partitioned table from a SQL Server instance hello below SQL can help you toointerrogate hello number of rows that are in each partition.</span></span>  <span data-ttu-id="14596-160">Ne feledje, hogy ha hello olyan particionálási részletességgel használatban van az SQL Data Warehouse, hello partíciónként sorok száma csökken 60 tényezővel.</span><span class="sxs-lookup"><span data-stu-id="14596-160">Keep in mind that if hello same partitioning granularity is used on SQL Data Warehouse, hello number of rows per partition will decrease by a factor of 60.</span></span>  

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

## <a name="workload-management"></a><span data-ttu-id="14596-161">Terheléskezelés</span><span class="sxs-lookup"><span data-stu-id="14596-161">Workload management</span></span>
<span data-ttu-id="14596-162">Egy utolsó darabja szempont toofactor a toohello tábla partíciós döntési van [munkaterhelés felügyeleti][workload management].</span><span class="sxs-lookup"><span data-stu-id="14596-162">One final piece consideration toofactor in toohello table partition decision is [workload management][workload management].</span></span>  <span data-ttu-id="14596-163">Munkaterhelés-kezelés az SQL Data Warehouse, a memória és a párhuzamosság elsősorban hello kezelése.</span><span class="sxs-lookup"><span data-stu-id="14596-163">Workload management in SQL Data Warehouse is primarily hello management of memory and concurrency.</span></span>  <span data-ttu-id="14596-164">Az SQL Data Warehouse hello tooeach terjesztési lekérdezés végrehajtása közben lefoglalt maximális memória szabályoz erőforrás osztály.</span><span class="sxs-lookup"><span data-stu-id="14596-164">In SQL Data Warehouse hello maximum memory allocated tooeach distribution during query execution is governed resource classes.</span></span>  <span data-ttu-id="14596-165">Ideális esetben a partíciók méretezi más tényezőktől, például a fürtözött oszlopcentrikus indexek felépítése hello memóriaigényét figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="14596-165">Ideally your partitions will be sized in consideration of other factors like hello memory needs of building clustered columnstore indexes.</span></span>  <span data-ttu-id="14596-166">Nagy mértékben fürtözött oszlopcentrikus indexek juttatásra, amikor azok több memóriát foglal le.</span><span class="sxs-lookup"><span data-stu-id="14596-166">Clustered columnstore indexes benefit greatly when they are allocated more memory.</span></span>  <span data-ttu-id="14596-167">Ezért érdemes, amely egy partíció index újraépítése tooensure nem függeszteni memória.</span><span class="sxs-lookup"><span data-stu-id="14596-167">Therefore, you will want tooensure that a partition index rebuild is not starved of memory.</span></span> <span data-ttu-id="14596-168">Más szerepkörök, például largerc hello memóriamennyiség elérhető tooyour lekérdezés elérhető átvált a hello alapértelmezett szerepkör, smallrc, a tooone növelése hello.</span><span class="sxs-lookup"><span data-stu-id="14596-168">Increasing hello amount of memory available tooyour query can be achieved by switching from hello default role, smallrc, tooone of hello other roles such as largerc.</span></span>

<span data-ttu-id="14596-169">Hello kiosztását) eloszlása feladatonként (memória áll rendelkezésre információ hello erőforrás vezérlő dinamikus felügyeleti nézetekkel lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="14596-169">Information on hello allocation of memory per distribution is available by querying hello resource governor dynamic management views.</span></span> <span data-ttu-id="14596-170">A valóságban a memóriabeli ideiglenes kisebb, mint az alábbi ábrán hello lesz.</span><span class="sxs-lookup"><span data-stu-id="14596-170">In reality your memory grant will be less than hello figures below.</span></span> <span data-ttu-id="14596-171">Azonban ez biztosít, amelyekkel a partíciók felügyeleti műveletek osztályozás útmutatás.</span><span class="sxs-lookup"><span data-stu-id="14596-171">However, this provides a level of guidance that you can use when sizing your partitions for data management operations.</span></span>  <span data-ttu-id="14596-172">Próbálja meg a partíciók túl hello extra nagy erőforrásosztály által biztosított hello memóriaengedély méretezése tooavoid.</span><span class="sxs-lookup"><span data-stu-id="14596-172">Try tooavoid sizing your partitions beyond hello memory grant provided by hello extra large resource class.</span></span> <span data-ttu-id="14596-173">Ha ez a szám nagyobb legyen a partíciók futtatnia Memóriaterhelést, ami viszont tooless optimális tömörítési hello kockázatát.</span><span class="sxs-lookup"><span data-stu-id="14596-173">If your partitions grow beyond this figure you run hello risk of memory pressure which in turn leads tooless optimal compression.</span></span>

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

## <a name="partition-switching"></a><span data-ttu-id="14596-174">Váltás a partíció</span><span class="sxs-lookup"><span data-stu-id="14596-174">Partition switching</span></span>
<span data-ttu-id="14596-175">Az SQL Data Warehouse a felosztás egyesítése és váltás partíció támogatja.</span><span class="sxs-lookup"><span data-stu-id="14596-175">SQL Data Warehouse supports partition splitting, merging, and switching.</span></span> <span data-ttu-id="14596-176">Ezek segítségével hello excuted [az ALTER TABLE] [ ALTER TABLE] utasítást.</span><span class="sxs-lookup"><span data-stu-id="14596-176">Each of these functions is excuted using hello [ALTER TABLE][ALTER TABLE] statement.</span></span>

<span data-ttu-id="14596-177">Győződjön meg arról, hogy hello partíciók igazodnak a megfelelő határokon belül, és, hogy megfelel-e hello definíciói két tábla között tooswitch partíciókat.</span><span class="sxs-lookup"><span data-stu-id="14596-177">tooswitch partitions between two tables you must ensure that hello partitions align on their respective boundaries and that hello table definitions match.</span></span> <span data-ttu-id="14596-178">Nem érhetők el ellenőrzési korlátozásokban, egy tábla hello forrástáblában értékek tartományán tooenforce hello tartalmaznia kell hello azonos határok partícióazonosító hello cél táblázatként.</span><span class="sxs-lookup"><span data-stu-id="14596-178">As check constraints are not available tooenforce hello range of values in a table hello source table must contain hello same partition boundaries as hello target table.</span></span> <span data-ttu-id="14596-179">Ha ez nem hello helyzet, majd hello partíció kapcsolójának sikertelen lesz, mivel nem fognak szinkronizálódni hello partíciós metaadatok.</span><span class="sxs-lookup"><span data-stu-id="14596-179">If this is not hello case, then hello partition switch will fail as hello partition metadata will not be synchronized.</span></span>

### <a name="how-toosplit-a-partition-that-contains-data"></a><span data-ttu-id="14596-180">Hogyan toosplit adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="14596-180">How toosplit a partition that contains data</span></span>
<span data-ttu-id="14596-181">hello leghatékonyabb módszer toosplit már tartalmazza az adatokat az toouse egy `CTAS` utasítást.</span><span class="sxs-lookup"><span data-stu-id="14596-181">hello most efficient method toosplit a partition that already contains data is toouse a `CTAS` statement.</span></span> <span data-ttu-id="14596-182">Ha hello particionált tábla fürtözött oszlopcentrikus majd hello tábla partíciós üresnek kell lennie ahhoz, lehessen.</span><span class="sxs-lookup"><span data-stu-id="14596-182">If hello partitioned table is a clustered columnstore then hello table partition must be empty before it can be split.</span></span>

<span data-ttu-id="14596-183">Alább egy sor minden partíció tartalmazó minta particionált oszlopcentrikus tábla lesz:</span><span class="sxs-lookup"><span data-stu-id="14596-183">Below is a sample partitioned columnstore table containing one row in each partition:</span></span>

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
> <span data-ttu-id="14596-184">Creating hello statisztika objektum által jelenleg győződjön meg arról, hogy a tábla metaadatainak pontosabb.</span><span class="sxs-lookup"><span data-stu-id="14596-184">By Creating hello statistic object, we ensure that table metadata is more accurate.</span></span> <span data-ttu-id="14596-185">Jelenleg nincs megadva statisztikák létrehozása, ha az SQL Data Warehouse alapértelmezett értékeket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="14596-185">If we omit creating statistics, then SQL Data Warehouse will use default values.</span></span> <span data-ttu-id="14596-186">A statisztika részletes tekintse át [statisztika][statistics].</span><span class="sxs-lookup"><span data-stu-id="14596-186">For details on statistics please review [statistics][statistics].</span></span>
> 
> 

<span data-ttu-id="14596-187">Azt is majd kereshet hello sorok számát hello segítségével `sys.partitions` katalógus megtekintése:</span><span class="sxs-lookup"><span data-stu-id="14596-187">We can then query for hello row count using hello `sys.partitions` catalog view:</span></span>

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

<span data-ttu-id="14596-188">Ha ez a táblázat azt próbálja toosplit, azt egy hiba jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="14596-188">If we try toosplit this table, we will get an error:</span></span>

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

<span data-ttu-id="14596-189">Üzenet 35346, szint 15 állapot 1, az ALTER PARTITION utasítás sor 44 SPLIT záradékának sikertelen volt, mert hello partíció nem üres.</span><span class="sxs-lookup"><span data-stu-id="14596-189">Msg 35346, Level 15, State 1, Line 44 SPLIT clause of ALTER PARTITION statement failed because hello partition is not empty.</span></span>  <span data-ttu-id="14596-190">Csak üres partíciók oszthatók fel egy oszloptárindex hello tábla létezik.</span><span class="sxs-lookup"><span data-stu-id="14596-190">Only empty partitions can be split in when a columnstore index exists on hello table.</span></span> <span data-ttu-id="14596-191">Vegye figyelembe, hogy hello oszloptárindex letiltását előtt hello az ALTER PARTITION utasítás kiadása, majd hello oszloptárindex újraépítését az ALTER PARTITION utasítás befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="14596-191">Consider disabling hello columnstore index before issuing hello ALTER PARTITION statement, then rebuilding hello columnstore index after ALTER PARTITION is complete.</span></span>

<span data-ttu-id="14596-192">Azonban használhatjuk `CTAS` toocreate egy új tábla toohold az adatok.</span><span class="sxs-lookup"><span data-stu-id="14596-192">However, we can use `CTAS` toocreate a new table toohold our data.</span></span>

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

<span data-ttu-id="14596-193">Hello partícióhatárok vannak rendezve, mivel a kapcsoló megengedett.</span><span class="sxs-lookup"><span data-stu-id="14596-193">As hello partition boundaries are aligned a switch is permitted.</span></span> <span data-ttu-id="14596-194">Így marad, hogy a forrástábla hello egy üres partícióból, azt később fel.</span><span class="sxs-lookup"><span data-stu-id="14596-194">This will leave hello source table with an empty partition that we can subsequently split.</span></span>

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 too FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

<span data-ttu-id="14596-195">Toodo marad csak az adatok toohello új partíció határokat a tooalign `CTAS` és az adatok váltson vissza a toohello főtáblát</span><span class="sxs-lookup"><span data-stu-id="14596-195">All that is left toodo is tooalign our data toohello new partition boundaries using `CTAS` and switch our data back in toohello main table</span></span>

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

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 toodbo.FactInternetSales PARTITION 2;
```

<span data-ttu-id="14596-196">Hello adatok mozgása hello befejezése után egy jó ötlet toorefresh hello statisztika a hello cél tábla tooensure pontosan tükrözik hello új terjesztési hello adatok a megfelelő partícióikba:</span><span class="sxs-lookup"><span data-stu-id="14596-196">Once you have completed hello movement of hello data it is a good idea toorefresh hello statistics on hello target table tooensure they accurately reflect hello new distribution of hello data in their respective partitions:</span></span>

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a><span data-ttu-id="14596-197">A verziókövetési rendszerrel particionálás tábla</span><span class="sxs-lookup"><span data-stu-id="14596-197">Table partitioning source control</span></span>
<span data-ttu-id="14596-198">tooavoid a tábladefiníció a **Rozsdás** a forrásrendszerben vezérlő érdemes lehet a következő megközelítés tooconsider hello:</span><span class="sxs-lookup"><span data-stu-id="14596-198">tooavoid your table definition from **rusting** in your source control system you may want tooconsider hello following approach:</span></span>

1. <span data-ttu-id="14596-199">Hello tábla létrehozása egy particionált táblához megegyezik, azonban nincsenek partíció értékek</span><span class="sxs-lookup"><span data-stu-id="14596-199">Create hello table as a partitioned table but with no partition values</span></span>

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

1. <span data-ttu-id="14596-200">`SPLIT`hello tábla hello központi telepítési folyamat részeként:</span><span class="sxs-lookup"><span data-stu-id="14596-200">`SPLIT` hello table as part of hello deployment process:</span></span>

```sql
-- Create a table containing hello partition boundaries

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

-- Iterate over hello partition boundaries and split hello table

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

<span data-ttu-id="14596-201">Ez a megközelítés hello a verziókövetési kód statikus marad, és a hello particionálási tartományhatár-értékek használata engedélyezett toobe dinamikus; fejlesztik hello adatraktár az idő múlásával.</span><span class="sxs-lookup"><span data-stu-id="14596-201">With this approach hello code in source control remains static and hello partitioning boundary values are allowed toobe dynamic; evolving with hello warehouse over time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14596-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14596-202">Next steps</span></span>
<span data-ttu-id="14596-203">toolearn több, lásd: hello cikkeket a [tábla áttekintése][Overview], [tábla adattípusok][Data Types], [táblaterjesztése] [ Distribute], [Tábla indexelő][Index], [fenntartása a tábla statisztikai adatainak] [ Statistics] és [ Az ideiglenes táblák][Temporary].</span><span class="sxs-lookup"><span data-stu-id="14596-203">toolearn more, see hello articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index], [Maintaining Table Statistics][Statistics] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="14596-204">Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="14596-204">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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

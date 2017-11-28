---
title: "aaaMonitor a számítási feladatok dinamikus felügyeleti nézetek használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toomonitor a számítási feladatok dinamikus felügyeleti nézetek használatával."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 69ecd479-0941-48df-b3d0-cf54c79e6549
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: acccf952d165ccec3de3b4b1c633b18bbbf78077
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-workload-using-dmvs"></a><span data-ttu-id="adbdf-103">Monitor your workload using DMVs</span><span class="sxs-lookup"><span data-stu-id="adbdf-103">Monitor your workload using DMVs</span></span>
<span data-ttu-id="adbdf-104">Ez a cikk ismerteti, hogyan toouse dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) toomonitor a számítási feladatok és vizsgálja meg a lekérdezés végrehajtása az Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="adbdf-104">This article describes how toouse Dynamic Management Views (DMVs) toomonitor your workload and investigate query execution in Azure SQL Data Warehouse.</span></span>

## <a name="permissions"></a><span data-ttu-id="adbdf-105">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="adbdf-105">Permissions</span></span>
<span data-ttu-id="adbdf-106">dinamikus felügyeleti nézetek tooquery hello ebben a cikkben adatbázis állapotának megtekintése vagy a vezérlő engedély szükséges.</span><span class="sxs-lookup"><span data-stu-id="adbdf-106">tooquery hello DMVs in this article, you need either VIEW DATABASE STATE or CONTROL permission.</span></span> <span data-ttu-id="adbdf-107">Általában támogatást nyújtó adatbázis a rendszer előnyben részesített hello engedéllyel, mert az sokkal jobban korlátozó.</span><span class="sxs-lookup"><span data-stu-id="adbdf-107">Usually granting VIEW DATABASE STATE is hello preferred permission as it is much more restrictive.</span></span>

```sql
GRANT VIEW DATABASE STATE toomyuser;
```

## <a name="monitor-connections"></a><span data-ttu-id="adbdf-108">A figyelő kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="adbdf-108">Monitor connections</span></span>
<span data-ttu-id="adbdf-109">Minden bejelentkezések tooSQL adatraktár túl bejelentkezett[sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].</span><span class="sxs-lookup"><span data-stu-id="adbdf-109">All logins tooSQL Data Warehouse are logged too[sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].</span></span>  <span data-ttu-id="adbdf-110">A DMV hello tartalmazza az utolsó 10 000 bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="adbdf-110">This DMV contains hello last 10,000 logins.</span></span>  <span data-ttu-id="adbdf-111">hello session_id hello elsődleges kulcs, és minden egyes új bejelentkezést a egymás után hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="adbdf-111">hello session_id is hello primary key and is assigned sequentially for each new logon.</span></span>

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a><span data-ttu-id="adbdf-112">A figyelő a lekérdezés-végrehajtás</span><span class="sxs-lookup"><span data-stu-id="adbdf-112">Monitor query execution</span></span>
<span data-ttu-id="adbdf-113">Az SQL Data Warehouse végrehajtott összes lekérdezések túl bejelentkezett[sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].</span><span class="sxs-lookup"><span data-stu-id="adbdf-113">All queries executed on SQL Data Warehouse are logged too[sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].</span></span>  <span data-ttu-id="adbdf-114">A DMV hello tartalmazza az utolsó 10 000 végrehajtott lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="adbdf-114">This DMV contains hello last 10,000 queries executed.</span></span>  <span data-ttu-id="adbdf-115">hello request_id egyedi módon azonosítja az egyes lekérdezés és a DMV hello elsődleges kulcsa.</span><span class="sxs-lookup"><span data-stu-id="adbdf-115">hello request_id uniquely identifies each query and is hello primary key for this DMV.</span></span>  <span data-ttu-id="adbdf-116">hello request_id minden új lekérdezés egymás után hozzá van rendelve, és van előtagként QID jelző lekérdezés azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="adbdf-116">hello request_id is assigned sequentially for each new query and is prefixed with QID, which stands for query ID.</span></span>  <span data-ttu-id="adbdf-117">Ez az egy adott session_id DMV lekérdezése jeleníti meg egy adott bejelentkezési összes lekérdezését.</span><span class="sxs-lookup"><span data-stu-id="adbdf-117">Querying this DMV for a given session_id shows all queries for a given logon.</span></span>

> [!NOTE]
> <span data-ttu-id="adbdf-118">Tárolt eljárások több kérelem azonosítókat használjon.</span><span class="sxs-lookup"><span data-stu-id="adbdf-118">Stored procedures use multiple Request IDs.</span></span>  <span data-ttu-id="adbdf-119">Kérelem azonosítók sorrendben vannak hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="adbdf-119">Request IDs are assigned in sequential order.</span></span> 
> 
> 

<span data-ttu-id="adbdf-120">Az alábbiakban lépéseket toofollow tooinvestigate végrehajtási terveket és egy adott lekérdezés idejét.</span><span class="sxs-lookup"><span data-stu-id="adbdf-120">Here are steps toofollow tooinvestigate query execution plans and times for a particular query.</span></span>

### <a name="step-1-identify-hello-query-you-wish-tooinvestigate"></a><span data-ttu-id="adbdf-121">1. lépés: Hello lekérdezés tooinvestigate kívánja azonosítása</span><span class="sxs-lookup"><span data-stu-id="adbdf-121">STEP 1: Identify hello query you wish tooinvestigate</span></span>
```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with hello Label 'My Query'
-- Use brackets when querying hello label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

<span data-ttu-id="adbdf-122">A lekérdezési eredmények megelőző hello **Megjegyzés hello Kérelemazonosító** , hogy szeretné-e tooinvestigate hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="adbdf-122">From hello preceding query results, **note hello Request ID** of hello query that you would like tooinvestigate.</span></span>

<span data-ttu-id="adbdf-123">Hello lévő lekérdezések **felfüggesztett** állapot éppen sorba tooconcurrency korlátok miatt.</span><span class="sxs-lookup"><span data-stu-id="adbdf-123">Queries in hello **Suspended** state are being queued due tooconcurrency limits.</span></span> <span data-ttu-id="adbdf-124">Ezeket a lekérdezéseket is hello sys.dm_pdw_waits vár lekérdezés UserConcurrencyResourceType típussal rendelkező jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="adbdf-124">These queries also appear in hello sys.dm_pdw_waits waits query with a type of UserConcurrencyResourceType.</span></span> <span data-ttu-id="adbdf-125">Lásd: [egyidejűségi és munkaterhelés-kezelés] [ Concurrency and workload management] kapcsolatban további részleteket a feldolgozási korlátok.</span><span class="sxs-lookup"><span data-stu-id="adbdf-125">See [Concurrency and workload management][Concurrency and workload management] for more details on concurrency limits.</span></span> <span data-ttu-id="adbdf-126">Lekérdezéseket is, amíg a többi okai többek között objektum zárolások.</span><span class="sxs-lookup"><span data-stu-id="adbdf-126">Queries can also wait for other reasons such as for object locks.</span></span>  <span data-ttu-id="adbdf-127">Ha a lekérdezés arra vár, hogy egy erőforrást, lásd: [Várakozás erőforrásokra lekérdezések kivizsgálása] [ Investigating queries waiting for resources] további le a cikkben.</span><span class="sxs-lookup"><span data-stu-id="adbdf-127">If your query is waiting for a resource, see [Investigating queries waiting for resources][Investigating queries waiting for resources] further down in this article.</span></span>

<span data-ttu-id="adbdf-128">toosimplify hello keresési hello sys.dm_pdw_exec_requests tábla, használja a lekérdezés [címke] [ LABEL] tooassign hello sys.dm_pdw_exec_requests nézetben kereshetők Megjegyzés tooyour lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="adbdf-128">toosimplify hello lookup of a query in hello sys.dm_pdw_exec_requests table, use [LABEL][LABEL] tooassign a comment tooyour query that can be looked up in hello sys.dm_pdw_exec_requests view.</span></span>

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-hello-query-plan"></a><span data-ttu-id="adbdf-129">2. lépés: Hello lekérdezésterv vizsgálata</span><span class="sxs-lookup"><span data-stu-id="adbdf-129">STEP 2: Investigate hello query plan</span></span>
<span data-ttu-id="adbdf-130">Hello Kérelemazonosító tooretrieve hello lekérdezés elosztott SQL (DSQL)-csomag használata a [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].</span><span class="sxs-lookup"><span data-stu-id="adbdf-130">Use hello Request ID tooretrieve hello query's distributed SQL (DSQL) plan from [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].</span></span>

```sql
-- Find hello distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

<span data-ttu-id="adbdf-131">Egy DSQL terv a vártnál tovább tart, amikor hello ok lehet egy összetett terv sok DSQL lépést vagy hosszú ideig egyetlen lépésben.</span><span class="sxs-lookup"><span data-stu-id="adbdf-131">When a DSQL plan is taking longer than expected, hello cause can be a complex plan with many DSQL steps or just one step taking a long time.</span></span>  <span data-ttu-id="adbdf-132">Ha hello terv több áthelyezési műveletek sok lépést, érdemes a táblázat azokat a terjesztéseket tooreduce adatátvitel optimalizálásához.</span><span class="sxs-lookup"><span data-stu-id="adbdf-132">If hello plan is many steps with several move operations, consider optimizing your table distributions tooreduce data movement.</span></span> <span data-ttu-id="adbdf-133">Hello [tábla terjesztési] [ Table distribution] a cikk azt ismerteti, miért adatokat kell lennie áthelyezett toosolve egy lekérdezést, és néhány terjesztési stratégiák toominimize adatmozgás ismerteti.</span><span class="sxs-lookup"><span data-stu-id="adbdf-133">hello [Table distribution][Table distribution] article explains why data must be moved toosolve a query and explains some distribution strategies toominimize data movement.</span></span>

<span data-ttu-id="adbdf-134">tooinvestigate további részleteit egy lépésben hello *operation_type* hello hosszan futó lekérdezés lépés és megjegyzés hello oszlopa **lépés Index**:</span><span class="sxs-lookup"><span data-stu-id="adbdf-134">tooinvestigate further details about a single step, hello *operation_type* column of hello long-running query step and note hello **Step Index**:</span></span>

* <span data-ttu-id="adbdf-135">Folytassa a 3/a. lépés a **SQL-műveletek**: OnOperation, RemoteOperation, ReturnOperation.</span><span class="sxs-lookup"><span data-stu-id="adbdf-135">Proceed with Step 3a for **SQL operations**: OnOperation, RemoteOperation, ReturnOperation.</span></span>
* <span data-ttu-id="adbdf-136">3b. lépés a folytatásához **adatok mozgási műveletek**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.</span><span class="sxs-lookup"><span data-stu-id="adbdf-136">Proceed with Step 3b for **Data Movement operations**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.</span></span>

### <a name="step-3a-investigate-sql-on-hello-distributed-databases"></a><span data-ttu-id="adbdf-137">3/a. lépés: elosztott hello adatbázisok SQL vizsgálata</span><span class="sxs-lookup"><span data-stu-id="adbdf-137">STEP 3a: Investigate SQL on hello distributed databases</span></span>
<span data-ttu-id="adbdf-138">Hello kérés azonosítója és hello lépés Index tooretrieve adataiból [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], amely ismerteti a végrehajtás hello lekérdezési lépés összes hello elosztott adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="adbdf-138">Use hello Request ID and hello Step Index tooretrieve details from [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], which contains execution information of hello query step on all of hello distributed databases.</span></span>

```sql
-- Find hello distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

<span data-ttu-id="adbdf-139">Hello lekérdezési lépés futtatásakor [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] használt tooretrieve hello SQL Server becsült terv az SQL Server terv gyorsítótár hello lépés egy adott futtató hello is lehet. terjesztési.</span><span class="sxs-lookup"><span data-stu-id="adbdf-139">When hello query step is running, [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] can be used tooretrieve hello SQL Server estimated plan from hello SQL Server plan cache for hello step running on a particular distribution.</span></span>

```sql
-- Find hello SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-hello-distributed-databases"></a><span data-ttu-id="adbdf-140">3b. lépés: elosztott hello adatbázisok adatátvitel vizsgálata</span><span class="sxs-lookup"><span data-stu-id="adbdf-140">STEP 3b: Investigate data movement on hello distributed databases</span></span>
<span data-ttu-id="adbdf-141">Hello kérés azonosítója és hello lépés Index tooretrieve információt egy adatok adatátviteli lépés az egyes terjesztési futó [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].</span><span class="sxs-lookup"><span data-stu-id="adbdf-141">Use hello Request ID and hello Step Index tooretrieve information about a data movement step running on each distribution from [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].</span></span>

```sql
-- Find hello information about all hello workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* <span data-ttu-id="adbdf-142">Ellenőrizze a hello *total_elapsed_time* oszlop toosee, ha egy adott terjesztési adatátvitelt jelölik a többinél jelentősen tovább tart.</span><span class="sxs-lookup"><span data-stu-id="adbdf-142">Check hello *total_elapsed_time* column toosee if a particular distribution is taking significantly longer than others for data movement.</span></span>
* <span data-ttu-id="adbdf-143">Hello hosszan futó terjesztési, ellenőrizze a hello *rows_processed* oszlop toosee jelentősen nagyobb, mint a többire, hogy a terjesztési mozgatásának sorok hello száma esetén.</span><span class="sxs-lookup"><span data-stu-id="adbdf-143">For hello long-running distribution, check hello *rows_processed* column toosee if hello number of rows being moved from that distribution is significantly larger than others.</span></span> <span data-ttu-id="adbdf-144">Ha igen, a mögöttes adatok eltérésére is jelezhet.</span><span class="sxs-lookup"><span data-stu-id="adbdf-144">If so, this may indicate skew of your underlying data.</span></span>

<span data-ttu-id="adbdf-145">Ha hello lekérdezése, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] használt tooretrieve hello SQL Server becsült terv az SQL Server terv gyorsítótárba futó SQL lépés belül egy adott hello hello is lehet. terjesztési.</span><span class="sxs-lookup"><span data-stu-id="adbdf-145">If hello query is running, [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] can be used tooretrieve hello SQL Server estimated plan from hello SQL Server plan cache for hello currently running SQL Step within a particular distribution.</span></span>

```sql
-- Find hello SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a><span data-ttu-id="adbdf-146">A figyelő várakozási lekérdezések</span><span class="sxs-lookup"><span data-stu-id="adbdf-146">Monitor waiting queries</span></span>
<span data-ttu-id="adbdf-147">Ha azt tapasztalja, hogy a lekérdezés nem készít folyamatban, mert egy erőforrás vár, ez egy lekérdezést, amely minden hello erőforrások jeleníti meg egy lekérdezést vár.</span><span class="sxs-lookup"><span data-stu-id="adbdf-147">If you discover that your query is not making progress because it is waiting for a resource, here is a query that shows all hello resources a query is waiting for.</span></span>

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

<span data-ttu-id="adbdf-148">Ha hello lekérdezés aktívan vár a egy másik lekérdezés erőforrásokat, akkor hello állapotban lesz **AcquireResources**.</span><span class="sxs-lookup"><span data-stu-id="adbdf-148">If hello query is actively waiting on resources from another query, then hello state will be **AcquireResources**.</span></span>  <span data-ttu-id="adbdf-149">Ha hello a lekérdezés minden szükséges hello erőforrásokat tartalmaz, akkor hello állapotban lesz **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="adbdf-149">If hello query has all hello required resources, then hello state will be **Granted**.</span></span>

## <a name="monitor-tempdb"></a><span data-ttu-id="adbdf-150">A figyelő a tempdb</span><span class="sxs-lookup"><span data-stu-id="adbdf-150">Monitor tempdb</span></span>
<span data-ttu-id="adbdf-151">A tempdb magas kihasználtsága lehet hello alapvető ok lassú teljesítmény- és kimenő memóriaproblémák léptek fel.</span><span class="sxs-lookup"><span data-stu-id="adbdf-151">High tempdb utilization can be hello root cause for slow performance and out of memory issues.</span></span> <span data-ttu-id="adbdf-152">Először ellenőrizze Ha adatokat eltérésére vagy gyenge minőségű rowgroups rendelkezik, és hello megfelelő műveletet.</span><span class="sxs-lookup"><span data-stu-id="adbdf-152">Please first check if you have data skew or poor quality rowgroups and take hello appropriate actions.</span></span> <span data-ttu-id="adbdf-153">Fontolja meg az adatraktár skálázás, ha a tempdb teljes kapacitásukkal működjenek elérése a lekérdezés végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="adbdf-153">Consider scaling your data warehouse if you find tempdb reaching its limits during query execution.</span></span> <span data-ttu-id="adbdf-154">hello következő ismerteti, hogyan tooidentify tempdb használati lekérdezésenként minden egyes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="adbdf-154">hello following describes how tooidentify tempdb usage per query on each node.</span></span> 

<span data-ttu-id="adbdf-155">A következő nézet tooassociate hello megfelelő csomópont azonosítója sys.dm_pdw_sql_requests hello létrehozása.</span><span class="sxs-lookup"><span data-stu-id="adbdf-155">Create hello following view tooassociate hello appropriate node id for sys.dm_pdw_sql_requests.</span></span> <span data-ttu-id="adbdf-156">Ez akkor tooleverage más csatlakoztatott dinamikus felügyeleti nézetek engedélyezi, és ezek a táblák sys.dm_pdw_sql_requests illesztése.</span><span class="sxs-lookup"><span data-stu-id="adbdf-156">This will enable you tooleverage other pass-through DMVs and join those tables with sys.dm_pdw_sql_requests.</span></span>

```sql
-- sys.dm_pdw_sql_requests with hello correct node id
CREATE VIEW sql_requests AS
(SELECT
       sr.request_id,
       sr.step_index,
       (CASE 
              WHEN (sr.distribution_id = -1 ) THEN 
              (SELECT pdw_node_id FROM sys.dm_pdw_nodes WHERE type = 'CONTROL') 
              ELSE d.pdw_node_id END) AS pdw_node_id,
       sr.distribution_id,
       sr.status,
       sr.error_id,
       sr.start_time,
       sr.end_time,
       sr.total_elapsed_time,
       sr.row_count,
       sr.spid,
       sr.command
FROM sys.pdw_distributions AS d
RIGHT JOIN sys.dm_pdw_sql_requests AS sr ON d.distribution_id = sr.distribution_id)
```
<span data-ttu-id="adbdf-157">Futtassa a következő lekérdezés toomonitor tempdb hello:</span><span class="sxs-lookup"><span data-stu-id="adbdf-157">Run hello following query toomonitor tempdb:</span></span>

```sql
-- Monitor tempdb
SELECT
    sr.request_id,
    ssu.session_id,
    ssu.pdw_node_id,
    sr.command,
    sr.total_elapsed_time,
    es.login_name AS 'LoginName',
    DB_NAME(ssu.database_id) AS 'DatabaseName',
    (es.memory_usage * 8) AS 'MemoryUsage (in KB)',
    (ssu.user_objects_alloc_page_count * 8) AS 'Space Allocated For User Objects (in KB)',
    (ssu.user_objects_dealloc_page_count * 8) AS 'Space Deallocated For User Objects (in KB)',
    (ssu.internal_objects_alloc_page_count * 8) AS 'Space Allocated For Internal Objects (in KB)',
    (ssu.internal_objects_dealloc_page_count * 8) AS 'Space Deallocated For Internal Objects (in KB)',
    CASE es.is_user_process
    WHEN 1 THEN 'User Session'
    WHEN 0 THEN 'System Session'
    END AS 'SessionType',
    es.row_count AS 'RowCount'
FROM sys.dm_pdw_nodes_db_session_space_usage AS ssu
    INNER JOIN sys.dm_pdw_nodes_exec_sessions AS es ON ssu.session_id = es.session_id AND ssu.pdw_node_id = es.pdw_node_id
    INNER JOIN sys.dm_pdw_nodes_exec_connections AS er ON ssu.session_id = er.session_id AND ssu.pdw_node_id = er.pdw_node_id
    INNER JOIN sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa' 
ORDER BY sr.request_id;
```
## <a name="monitor-memory"></a><span data-ttu-id="adbdf-158">A figyelő memória</span><span class="sxs-lookup"><span data-stu-id="adbdf-158">Monitor memory</span></span>

<span data-ttu-id="adbdf-159">Memória lehet hello alapvető ok lassú teljesítmény- és kimenő memóriaproblémák léptek fel.</span><span class="sxs-lookup"><span data-stu-id="adbdf-159">Memory can be hello root cause for slow performance and out of memory issues.</span></span> <span data-ttu-id="adbdf-160">Először ellenőrizze Ha adatokat eltérésére vagy gyenge minőségű rowgroups rendelkezik, és hello megfelelő műveletet.</span><span class="sxs-lookup"><span data-stu-id="adbdf-160">Please first check if you have data skew or poor quality rowgroups and take hello appropriate actions.</span></span> <span data-ttu-id="adbdf-161">Fontolja meg az adatraktár skálázás, ha SQL Server memóriahasználatának teljes kapacitásukkal működjenek elérése a lekérdezés végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="adbdf-161">Consider scaling your data warehouse if you find SQL Server memory usage reaching its limits during query execution.</span></span>

<span data-ttu-id="adbdf-162">a következő lekérdezés hello SQL Server használatát és Memóriaterhelést csomópontonként adja vissza:</span><span class="sxs-lookup"><span data-stu-id="adbdf-162">hello following query returns SQL Server memory usage and memory pressure per node:</span></span> 
```sql
-- Memory consumption
SELECT
  pc1.cntr_value as Curr_Mem_KB, 
  pc1.cntr_value/1024.0 as Curr_Mem_MB,
  (pc1.cntr_value/1048576.0) as Curr_Mem_GB,
  pc2.cntr_value as Max_Mem_KB,
  pc2.cntr_value/1024.0 as Max_Mem_MB,
  (pc2.cntr_value/1048576.0) as Max_Mem_GB,
  pc1.cntr_value * 100.0/pc2.cntr_value AS Memory_Utilization_Percentage,
  pc1.pdw_node_id
FROM
-- pc1: current memory
sys.dm_pdw_nodes_os_performance_counters AS pc1
-- pc2: total memory allowed for this SQL instance
JOIN sys.dm_pdw_nodes_os_performance_counters AS pc2 
ON pc1.object_name = pc2.object_name AND pc1.pdw_node_id = pc2.pdw_node_id
WHERE
pc1.counter_name = 'Total Server Memory (KB)'
AND pc2.counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-size"></a><span data-ttu-id="adbdf-163">A figyelő tranzakciós napló mérete</span><span class="sxs-lookup"><span data-stu-id="adbdf-163">Monitor transaction log size</span></span>
<span data-ttu-id="adbdf-164">hello következő lekérdezés visszaadja hello tranzakciós napló mérete minden terjesztési.</span><span class="sxs-lookup"><span data-stu-id="adbdf-164">hello following query returns hello transaction log size on each distribution.</span></span> <span data-ttu-id="adbdf-165">Győződjön meg arról, ha az adatok eltérésére vagy gyenge minőségű rowgroups rendelkezik, és hello megfelelő műveletet.</span><span class="sxs-lookup"><span data-stu-id="adbdf-165">Please check if you have data skew or poor quality rowgroups and take hello appropriate actions.</span></span> <span data-ttu-id="adbdf-166">Ha egyik hello naplófájlok 160GB közel jár, érdemes a példány vertikális felskálázásával, vagy a tranzakció mérete korlátozza.</span><span class="sxs-lookup"><span data-stu-id="adbdf-166">If one of hello log files is reaching 160GB, you should consider scaling up your instance or limiting your transaction size.</span></span> 
```sql
-- Transaction log size
SELECT
  instance_name as distribution_db,
  cntr_value*1.0/1048576 as log_file_size_used_GB,
  pdw_node_id 
FROM sys.dm_pdw_nodes_os_performance_counters 
WHERE 
instance_name like 'Distribution_%' 
AND counter_name = 'Log File(s) Used Size (KB)'
AND counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-rollback"></a><span data-ttu-id="adbdf-167">A figyelő tranzakciós napló visszaállítása</span><span class="sxs-lookup"><span data-stu-id="adbdf-167">Monitor transaction log rollback</span></span>
<span data-ttu-id="adbdf-168">Ha a lekérdezések nem működnek, vagy egy hosszú ideig tooproceed telik, ellenőrizze és figyelése, ha a tranzakciók visszaállítása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="adbdf-168">If your queries are failing or taking a long time tooproceed, you can check and monitor if you have any transactions rolling back.</span></span>
```sql
-- Monitor rollback
SELECT 
    SUM(CASE WHEN t.database_transaction_next_undo_lsn IS NOT NULL THEN 1 ELSE 0 END),
    t.pdw_node_id,
    nod.[type]
FROM sys.dm_pdw_nodes_tran_database_transactions t
JOIN sys.dm_pdw_nodes nod ON t.pdw_node_id = nod.pdw_node_id
GROUP BY t.pdw_node_id, nod.[type]
```

## <a name="next-steps"></a><span data-ttu-id="adbdf-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="adbdf-169">Next steps</span></span>
<span data-ttu-id="adbdf-170">Lásd: [rendszernézetek] [ System views] dinamikus felügyeleti nézetek olvashat.</span><span class="sxs-lookup"><span data-stu-id="adbdf-170">See [System views][System views] for more information on DMVs.</span></span>
<span data-ttu-id="adbdf-171">Lásd: [gyakorlati tanácsok az SQL Data Warehouse] [ SQL Data Warehouse best practices] ajánlott eljárásokra vonatkozó további információ</span><span class="sxs-lookup"><span data-stu-id="adbdf-171">See [SQL Data Warehouse best practices][SQL Data Warehouse best practices] for more information about best practices</span></span>

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Concurrency and workload management]: ./sql-data-warehouse-develop-concurrency.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx

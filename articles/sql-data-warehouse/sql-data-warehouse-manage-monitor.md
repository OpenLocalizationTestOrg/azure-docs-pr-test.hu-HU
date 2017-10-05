---
title: "A dinamikus felügyeleti nézetek használatával számítási feladat figyeléséhez |} Microsoft Docs"
description: "Útmutató: a dinamikus felügyeleti nézetek használatával számítási feladat figyeléséhez."
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
ms.openlocfilehash: 7ce6c2cdf1e28852da536414533ccdcdaeb437e5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-your-workload-using-dmvs"></a><span data-ttu-id="d93a1-103">Monitor your workload using DMVs</span><span class="sxs-lookup"><span data-stu-id="d93a1-103">Monitor your workload using DMVs</span></span>
<span data-ttu-id="d93a1-104">A cikkből megtudhatja, hogyan lehet a számítási feladat figyeléséhez, és vizsgálja meg a lekérdezés végrehajtása az Azure SQL Data Warehouse dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) segítségével.</span><span class="sxs-lookup"><span data-stu-id="d93a1-104">This article describes how to use Dynamic Management Views (DMVs) to monitor your workload and investigate query execution in Azure SQL Data Warehouse.</span></span>

## <a name="permissions"></a><span data-ttu-id="d93a1-105">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="d93a1-105">Permissions</span></span>
<span data-ttu-id="d93a1-106">Ebben a cikkben a dinamikus felügyeleti nézetek lekérdezéséhez adatbázis állapotának megtekintése vagy a vezérlő engedéllyel kell rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="d93a1-106">To query the DMVs in this article, you need either VIEW DATABASE STATE or CONTROL permission.</span></span> <span data-ttu-id="d93a1-107">Általában támogatást nyújtó ADATBÁZIST a rendszer az előnyben részesített engedély, mert az sokkal jobban korlátozó.</span><span class="sxs-lookup"><span data-stu-id="d93a1-107">Usually granting VIEW DATABASE STATE is the preferred permission as it is much more restrictive.</span></span>

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a><span data-ttu-id="d93a1-108">A figyelő kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="d93a1-108">Monitor connections</span></span>
<span data-ttu-id="d93a1-109">Az SQL Data Warehouse minden bejelentkezések a rendszer naplózza [sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].</span><span class="sxs-lookup"><span data-stu-id="d93a1-109">All logins to SQL Data Warehouse are logged to [sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].</span></span>  <span data-ttu-id="d93a1-110">Ez a DMV az utolsó 10 000 bejelentkezések tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d93a1-110">This DMV contains the last 10,000 logins.</span></span>  <span data-ttu-id="d93a1-111">A session_id az elsődleges kulcs, és minden egyes új bejelentkezést a egymás után hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="d93a1-111">The session_id is the primary key and is assigned sequentially for each new logon.</span></span>

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a><span data-ttu-id="d93a1-112">A figyelő a lekérdezés-végrehajtás</span><span class="sxs-lookup"><span data-stu-id="d93a1-112">Monitor query execution</span></span>
<span data-ttu-id="d93a1-113">Az SQL Data Warehouse végrehajtott összes lekérdezések naplózása [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].</span><span class="sxs-lookup"><span data-stu-id="d93a1-113">All queries executed on SQL Data Warehouse are logged to [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].</span></span>  <span data-ttu-id="d93a1-114">Ez a DMV végrehajtott utolsó 10 000 lekérdezések tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d93a1-114">This DMV contains the last 10,000 queries executed.</span></span>  <span data-ttu-id="d93a1-115">A request_id egyedi módon azonosítja az egyes lekérdezés és a DMV elsődleges kulcsa.</span><span class="sxs-lookup"><span data-stu-id="d93a1-115">The request_id uniquely identifies each query and is the primary key for this DMV.</span></span>  <span data-ttu-id="d93a1-116">A request_id minden új lekérdezés egymás után hozzá van rendelve, és van előtagként QID jelző lekérdezés azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="d93a1-116">The request_id is assigned sequentially for each new query and is prefixed with QID, which stands for query ID.</span></span>  <span data-ttu-id="d93a1-117">Ez az egy adott session_id DMV lekérdezése jeleníti meg egy adott bejelentkezési összes lekérdezését.</span><span class="sxs-lookup"><span data-stu-id="d93a1-117">Querying this DMV for a given session_id shows all queries for a given logon.</span></span>

> [!NOTE]
> <span data-ttu-id="d93a1-118">Tárolt eljárások több kérelem azonosítókat használjon.</span><span class="sxs-lookup"><span data-stu-id="d93a1-118">Stored procedures use multiple Request IDs.</span></span>  <span data-ttu-id="d93a1-119">Kérelem azonosítók sorrendben vannak hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="d93a1-119">Request IDs are assigned in sequential order.</span></span> 
> 
> 

<span data-ttu-id="d93a1-120">Az alábbiakban lépést kell végrehajtania, vizsgálja meg a lekérdezés végrehajtási terveket és egy adott lekérdezés idejét.</span><span class="sxs-lookup"><span data-stu-id="d93a1-120">Here are steps to follow to investigate query execution plans and times for a particular query.</span></span>

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a><span data-ttu-id="d93a1-121">1. lépés: A vizsgálni kívánt lekérdezés azonosítása</span><span class="sxs-lookup"><span data-stu-id="d93a1-121">STEP 1: Identify the query you wish to investigate</span></span>
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

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

<span data-ttu-id="d93a1-122">Az előző lekérdezés eredménybe **jegyezze fel a kérelem azonosítója** a lekérdezés, amely meg szeretné vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="d93a1-122">From the preceding query results, **note the Request ID** of the query that you would like to investigate.</span></span>

<span data-ttu-id="d93a1-123">A lekérdezi a **felfüggesztett** állapot éppen sorba feldolgozási korlátok miatt.</span><span class="sxs-lookup"><span data-stu-id="d93a1-123">Queries in the **Suspended** state are being queued due to concurrency limits.</span></span> <span data-ttu-id="d93a1-124">Ezeket a lekérdezéseket is megjelennek a sys.dm_pdw_waits vár lekérdezés UserConcurrencyResourceType típusú.</span><span class="sxs-lookup"><span data-stu-id="d93a1-124">These queries also appear in the sys.dm_pdw_waits waits query with a type of UserConcurrencyResourceType.</span></span> <span data-ttu-id="d93a1-125">Lásd: [egyidejűségi és munkaterhelés-kezelés] [ Concurrency and workload management] kapcsolatban további részleteket a feldolgozási korlátok.</span><span class="sxs-lookup"><span data-stu-id="d93a1-125">See [Concurrency and workload management][Concurrency and workload management] for more details on concurrency limits.</span></span> <span data-ttu-id="d93a1-126">Lekérdezéseket is, amíg a többi okai többek között objektum zárolások.</span><span class="sxs-lookup"><span data-stu-id="d93a1-126">Queries can also wait for other reasons such as for object locks.</span></span>  <span data-ttu-id="d93a1-127">Ha a lekérdezés arra vár, hogy egy erőforrást, lásd: [Várakozás erőforrásokra lekérdezések kivizsgálása] [ Investigating queries waiting for resources] további le a cikkben.</span><span class="sxs-lookup"><span data-stu-id="d93a1-127">If your query is waiting for a resource, see [Investigating queries waiting for resources][Investigating queries waiting for resources] further down in this article.</span></span>

<span data-ttu-id="d93a1-128">A Keresés a sys.dm_pdw_exec_requests tábla lekérdezés egyszerűsítése érdekében használja a [címke] [ LABEL] Megjegyzés hozzárendelése a lekérdezést, amely a sys.dm_pdw_exec_requests nézetben lehet keresni.</span><span class="sxs-lookup"><span data-stu-id="d93a1-128">To simplify the lookup of a query in the sys.dm_pdw_exec_requests table, use [LABEL][LABEL] to assign a comment to your query that can be looked up in the sys.dm_pdw_exec_requests view.</span></span>

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a><span data-ttu-id="d93a1-129">2. lépés:, Vizsgálja meg a lekérdezésterv</span><span class="sxs-lookup"><span data-stu-id="d93a1-129">STEP 2: Investigate the query plan</span></span>
<span data-ttu-id="d93a1-130">A Kérelemazonosító segítségével beolvasni a lekérdezés elosztott SQL (DSQL) terv a [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].</span><span class="sxs-lookup"><span data-stu-id="d93a1-130">Use the Request ID to retrieve the query's distributed SQL (DSQL) plan from [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].</span></span>

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

<span data-ttu-id="d93a1-131">Egy DSQL terv a vártnál tovább tart, amikor az OK lehet egy összetett terv sok DSQL lépést vagy hosszú ideig egyetlen lépésben.</span><span class="sxs-lookup"><span data-stu-id="d93a1-131">When a DSQL plan is taking longer than expected, the cause can be a complex plan with many DSQL steps or just one step taking a long time.</span></span>  <span data-ttu-id="d93a1-132">Ha a csomag több áthelyezési műveletek sok lépést, érdemes optimalizálása a táblázat azokat a terjesztéseket adatmozgás csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="d93a1-132">If the plan is many steps with several move operations, consider optimizing your table distributions to reduce data movement.</span></span> <span data-ttu-id="d93a1-133">A [tábla terjesztési] [ Table distribution] a cikk azt ismerteti, miért adatok oldja meg a lekérdezés át kell helyezni, és elmagyarázza, néhány terjesztési stratégiák adatmozgás minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="d93a1-133">The [Table distribution][Table distribution] article explains why data must be moved to solve a query and explains some distribution strategies to minimize data movement.</span></span>

<span data-ttu-id="d93a1-134">További egyetlen lépésben adatait kell vizsgálni a *operation_type* oszlopa a hosszan futó lekérdezés lépést, és vegye figyelembe a **lépés Index**:</span><span class="sxs-lookup"><span data-stu-id="d93a1-134">To investigate further details about a single step, the *operation_type* column of the long-running query step and note the **Step Index**:</span></span>

* <span data-ttu-id="d93a1-135">Folytassa a 3/a. lépés a **SQL-műveletek**: OnOperation, RemoteOperation, ReturnOperation.</span><span class="sxs-lookup"><span data-stu-id="d93a1-135">Proceed with Step 3a for **SQL operations**: OnOperation, RemoteOperation, ReturnOperation.</span></span>
* <span data-ttu-id="d93a1-136">3b. lépés a folytatásához **adatok mozgási műveletek**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.</span><span class="sxs-lookup"><span data-stu-id="d93a1-136">Proceed with Step 3b for **Data Movement operations**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.</span></span>

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a><span data-ttu-id="d93a1-137">3/a. lépés: az elosztott adatbázisok SQL vizsgálata</span><span class="sxs-lookup"><span data-stu-id="d93a1-137">STEP 3a: Investigate SQL on the distributed databases</span></span>
<span data-ttu-id="d93a1-138">A kérelem azonosítója és a lépés Index használja az adatok beolvasása [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], amely információkat tartalmaz az végrehajtási lekérdezési lépés az összes elosztott adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="d93a1-138">Use the Request ID and the Step Index to retrieve details from [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], which contains execution information of the query step on all of the distributed databases.</span></span>

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

<span data-ttu-id="d93a1-139">A lekérdezés lépés futtatásakor [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] az SQL Server becsült terv lekérése a lépés egy adott terjesztési futó SQL Server gyorsítótárban használható.</span><span class="sxs-lookup"><span data-stu-id="d93a1-139">When the query step is running, [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] can be used to retrieve the SQL Server estimated plan from the SQL Server plan cache for the step running on a particular distribution.</span></span>

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a><span data-ttu-id="d93a1-140">3b. lépés: az elosztott adatbázisok adatátvitel vizsgálata</span><span class="sxs-lookup"><span data-stu-id="d93a1-140">STEP 3b: Investigate data movement on the distributed databases</span></span>
<span data-ttu-id="d93a1-141">A kérelem azonosítója és a lépés Index használatával egy adatok adatátviteli lépés az egyes terjesztési futó információkat kérjen le [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].</span><span class="sxs-lookup"><span data-stu-id="d93a1-141">Use the Request ID and the Step Index to retrieve information about a data movement step running on each distribution from [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].</span></span>

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* <span data-ttu-id="d93a1-142">Ellenőrizze a *total_elapsed_time* oszlopban tekintheti meg, ha egy adott terjesztési adatátvitelt jelölik a többinél jelentősen tovább tart.</span><span class="sxs-lookup"><span data-stu-id="d93a1-142">Check the *total_elapsed_time* column to see if a particular distribution is taking significantly longer than others for data movement.</span></span>
* <span data-ttu-id="d93a1-143">A hosszan futó terjesztési tekintse meg a *rows_processed* oszlopban tekintheti meg, hogy-e jelentősen nagyobb, mint a többire, hogy a terjesztési mozgatásának sorok száma.</span><span class="sxs-lookup"><span data-stu-id="d93a1-143">For the long-running distribution, check the *rows_processed* column to see if the number of rows being moved from that distribution is significantly larger than others.</span></span> <span data-ttu-id="d93a1-144">Ha igen, a mögöttes adatok eltérésére is jelezhet.</span><span class="sxs-lookup"><span data-stu-id="d93a1-144">If so, this may indicate skew of your underlying data.</span></span>

<span data-ttu-id="d93a1-145">Ha a lekérdezés fut, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] az SQL Server becsült terv lekérdezni az SQL Server gyorsítótárban a jelenleg futó SQL lépés belül egy adott terjesztési használható.</span><span class="sxs-lookup"><span data-stu-id="d93a1-145">If the query is running, [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] can be used to retrieve the SQL Server estimated plan from the SQL Server plan cache for the currently running SQL Step within a particular distribution.</span></span>

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a><span data-ttu-id="d93a1-146">A figyelő várakozási lekérdezések</span><span class="sxs-lookup"><span data-stu-id="d93a1-146">Monitor waiting queries</span></span>
<span data-ttu-id="d93a1-147">Ha azt tapasztalja, hogy a lekérdezés nem készít folyamatban, mert egy erőforrás vár, ez a lekérdezés, amely tartalmazza az összes erőforrást lekérdezést vár.</span><span class="sxs-lookup"><span data-stu-id="d93a1-147">If you discover that your query is not making progress because it is waiting for a resource, here is a query that shows all the resources a query is waiting for.</span></span>

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

<span data-ttu-id="d93a1-148">Ha a lekérdezés aktívan Várakozás egy másik lekérdezés erőforrások, akkor az állapot lesz **AcquireResources**.</span><span class="sxs-lookup"><span data-stu-id="d93a1-148">If the query is actively waiting on resources from another query, then the state will be **AcquireResources**.</span></span>  <span data-ttu-id="d93a1-149">Ha a lekérdezés rendelkezik a szükséges erőforrásokat, akkor az állapot lesz **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="d93a1-149">If the query has all the required resources, then the state will be **Granted**.</span></span>

## <a name="monitor-tempdb"></a><span data-ttu-id="d93a1-150">A figyelő a tempdb</span><span class="sxs-lookup"><span data-stu-id="d93a1-150">Monitor tempdb</span></span>
<span data-ttu-id="d93a1-151">A tempdb magas kihasználtsága lehet az alapvető ok, a lassú teljesítmény és a kimenő memóriaproblémák léptek fel.</span><span class="sxs-lookup"><span data-stu-id="d93a1-151">High tempdb utilization can be the root cause for slow performance and out of memory issues.</span></span> <span data-ttu-id="d93a1-152">Először ellenőrizze Ha adatokat eltérésére vagy gyenge minőségű rowgroups rendelkezik, és hajtsa végre a megfelelő műveleteket.</span><span class="sxs-lookup"><span data-stu-id="d93a1-152">Please first check if you have data skew or poor quality rowgroups and take the appropriate actions.</span></span> <span data-ttu-id="d93a1-153">Fontolja meg az adatraktár skálázás, ha a tempdb teljes kapacitásukkal működjenek elérése a lekérdezés végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="d93a1-153">Consider scaling your data warehouse if you find tempdb reaching its limits during query execution.</span></span> <span data-ttu-id="d93a1-154">A következő útmutató minden egyes csomóponton lekérdezésenként tempdb használatának azonosítása.</span><span class="sxs-lookup"><span data-stu-id="d93a1-154">The following describes how to identify tempdb usage per query on each node.</span></span> 

<span data-ttu-id="d93a1-155">Hozzon létre a következő nézetet társítja a megfelelő csomópont sys.dm_pdw_sql_requests azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="d93a1-155">Create the following view to associate the appropriate node id for sys.dm_pdw_sql_requests.</span></span> <span data-ttu-id="d93a1-156">Ez lehetővé teszi a használhatják fel a többi csatlakoztatott dinamikus felügyeleti nézetek, és ezek a táblák sys.dm_pdw_sql_requests illesztése.</span><span class="sxs-lookup"><span data-stu-id="d93a1-156">This will enable you to leverage other pass-through DMVs and join those tables with sys.dm_pdw_sql_requests.</span></span>

```sql
-- sys.dm_pdw_sql_requests with the correct node id
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
<span data-ttu-id="d93a1-157">Futtassa a következő lekérdezést a tempdb figyelése:</span><span class="sxs-lookup"><span data-stu-id="d93a1-157">Run the following query to monitor tempdb:</span></span>

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
## <a name="monitor-memory"></a><span data-ttu-id="d93a1-158">A figyelő memória</span><span class="sxs-lookup"><span data-stu-id="d93a1-158">Monitor memory</span></span>

<span data-ttu-id="d93a1-159">Memória lehet az alapvető ok, a lassú teljesítmény és a kimenő memóriaproblémák léptek fel.</span><span class="sxs-lookup"><span data-stu-id="d93a1-159">Memory can be the root cause for slow performance and out of memory issues.</span></span> <span data-ttu-id="d93a1-160">Először ellenőrizze Ha adatokat eltérésére vagy gyenge minőségű rowgroups rendelkezik, és hajtsa végre a megfelelő műveleteket.</span><span class="sxs-lookup"><span data-stu-id="d93a1-160">Please first check if you have data skew or poor quality rowgroups and take the appropriate actions.</span></span> <span data-ttu-id="d93a1-161">Fontolja meg az adatraktár skálázás, ha SQL Server memóriahasználatának teljes kapacitásukkal működjenek elérése a lekérdezés végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="d93a1-161">Consider scaling your data warehouse if you find SQL Server memory usage reaching its limits during query execution.</span></span>

<span data-ttu-id="d93a1-162">A következő lekérdezés visszaadja az SQL Server használatát és Memóriaterhelést csomópontonként:</span><span class="sxs-lookup"><span data-stu-id="d93a1-162">The following query returns SQL Server memory usage and memory pressure per node:</span></span>   
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
## <a name="monitor-transaction-log-size"></a><span data-ttu-id="d93a1-163">A figyelő tranzakciós napló mérete</span><span class="sxs-lookup"><span data-stu-id="d93a1-163">Monitor transaction log size</span></span>
<span data-ttu-id="d93a1-164">A következő lekérdezés a tranzakciós napló mérete minden terjesztési adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d93a1-164">The following query returns the transaction log size on each distribution.</span></span> <span data-ttu-id="d93a1-165">Győződjön meg arról, ha az adatok eltérésére vagy gyenge minőségű rowgroups rendelkezik, és hajtsa végre a megfelelő műveleteket.</span><span class="sxs-lookup"><span data-stu-id="d93a1-165">Please check if you have data skew or poor quality rowgroups and take the appropriate actions.</span></span> <span data-ttu-id="d93a1-166">Ha egy, a naplófájlok 160GB közel jár, érdemes a példány vertikális felskálázásával, vagy a tranzakció mérete korlátozza.</span><span class="sxs-lookup"><span data-stu-id="d93a1-166">If one of the log files is reaching 160GB, you should consider scaling up your instance or limiting your transaction size.</span></span> 
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
## <a name="monitor-transaction-log-rollback"></a><span data-ttu-id="d93a1-167">A figyelő tranzakciós napló visszaállítása</span><span class="sxs-lookup"><span data-stu-id="d93a1-167">Monitor transaction log rollback</span></span>
<span data-ttu-id="d93a1-168">Ha a lekérdezések sikertelenek lesznek, vagy folytatja sok időbe telik, ellenőrizze, és figyelése, ha a tranzakciók visszaállítása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="d93a1-168">If your queries are failing or taking a long time to proceed, you can check and monitor if you have any transactions rolling back.</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="d93a1-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d93a1-169">Next steps</span></span>
<span data-ttu-id="d93a1-170">Lásd: [rendszernézetek] [ System views] dinamikus felügyeleti nézetek olvashat.</span><span class="sxs-lookup"><span data-stu-id="d93a1-170">See [System views][System views] for more information on DMVs.</span></span>
<span data-ttu-id="d93a1-171">Lásd: [gyakorlati tanácsok az SQL Data Warehouse] [ SQL Data Warehouse best practices] ajánlott eljárásokra vonatkozó további információ</span><span class="sxs-lookup"><span data-stu-id="d93a1-171">See [SQL Data Warehouse best practices][SQL Data Warehouse best practices] for more information about best practices</span></span>

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

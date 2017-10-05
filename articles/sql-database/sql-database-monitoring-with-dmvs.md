---
title: "Az Azure SQL adatbázis dinamikus felügyeleti nézetekkel figyelése |} Microsoft Docs"
description: "Megtudhatja, hogyan észlelheti és diagnosztizálhatja a gyakori problémák a Microsoft Azure SQL Database figyelése dinamikus felügyeleti nézetek használatával."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: d08f505f-3c62-47d4-bab7-35c9a834b79b
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: d9b007d29e06e672db71b4a8415673f258c3fd89
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a><span data-ttu-id="d143f-103">Az Azure SQL Database felügyelete dinamikus felügyeleti nézetek használatával</span><span class="sxs-lookup"><span data-stu-id="d143f-103">Monitoring Azure SQL Database using dynamic management views</span></span>
<span data-ttu-id="d143f-104">A Microsoft Azure SQL Database lehetővé teszi, hogy a dinamikus felügyeleti nézetekkel teljesítményproblémákat, amely valószínűleg az okozza letiltott vagy hosszan futó lekérdezések, erőforrás-keresztmetszetek, gyenge lekérdezésterveket, és így tovább diagnosztizálásához egy részét.</span><span class="sxs-lookup"><span data-stu-id="d143f-104">Microsoft Azure SQL Database enables a subset of dynamic management views to diagnose performance problems, which might be caused by blocked or long-running queries, resource bottlenecks, poor query plans, and so on.</span></span> <span data-ttu-id="d143f-105">Ez a témakör információkat kapcsolatos gyakori problémák észlelése dinamikus felügyeleti nézetek használatával.</span><span class="sxs-lookup"><span data-stu-id="d143f-105">This topic provides information on how to detect common performance problems by using dynamic management views.</span></span>

<span data-ttu-id="d143f-106">SQL-adatbázis részben három kategóriáinak dinamikus felügyeleti nézetekkel támogatja:</span><span class="sxs-lookup"><span data-stu-id="d143f-106">SQL Database partially supports three categories of dynamic management views:</span></span>

* <span data-ttu-id="d143f-107">Adatbázissal kapcsolatos dinamikus felügyeleti nézetekkel.</span><span class="sxs-lookup"><span data-stu-id="d143f-107">Database-related dynamic management views.</span></span>
* <span data-ttu-id="d143f-108">Végrehajtási kapcsolatos dinamikus felügyeleti nézetekkel.</span><span class="sxs-lookup"><span data-stu-id="d143f-108">Execution-related dynamic management views.</span></span>
* <span data-ttu-id="d143f-109">Tranzakció-hoz kapcsolódó dinamikus felügyeleti nézetek.</span><span class="sxs-lookup"><span data-stu-id="d143f-109">Transaction-related dynamic management views.</span></span>

<span data-ttu-id="d143f-110">A dinamikus felügyeleti nézetekkel részletes információkért lásd: [dinamikus felügyeleti nézetek és függvények (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) az SQL Server Online könyvekben.</span><span class="sxs-lookup"><span data-stu-id="d143f-110">For detailed information on dynamic management views, see [Dynamic Management Views and Functions (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) in SQL Server Books Online.</span></span>

## <a name="permissions"></a><span data-ttu-id="d143f-111">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="d143f-111">Permissions</span></span>
<span data-ttu-id="d143f-112">Az SQL-adatbázis, a dinamikus felügyeleti nézetben lekérdezése szükséges **adatbázis állapotának megtekintése** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="d143f-112">In SQL Database, querying a dynamic management view requires **VIEW DATABASE STATE** permissions.</span></span> <span data-ttu-id="d143f-113">A **adatbázis állapotának megtekintése** engedélyt az aktuális adatbázisban lévő objektumok információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="d143f-113">The **VIEW DATABASE STATE** permission returns information about all objects within the current database.</span></span>
<span data-ttu-id="d143f-114">Megadását a **adatbázis állapotának megtekintése** engedéllyel a megadott adatbázis-felhasználó, futtassa a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="d143f-114">To grant the **VIEW DATABASE STATE** permission to a specific database user, run the following query:</span></span>

```GRANT VIEW DATABASE STATE TO database_user; ```

<span data-ttu-id="d143f-115">A helyszíni SQL Server egy példányát, a dinamikus felügyeleti nézetekkel kiszolgáló állapota adatokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="d143f-115">In an instance of on-premises SQL Server, dynamic management views return server state information.</span></span> <span data-ttu-id="d143f-116">Az SQL-adatbázis akkor csak az aktuális logikai adatbázis vonatkozó információkat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d143f-116">In SQL Database, they return information regarding your current logical database only.</span></span>

## <a name="calculating-database-size"></a><span data-ttu-id="d143f-117">Adatbázisméret kiszámítása</span><span class="sxs-lookup"><span data-stu-id="d143f-117">Calculating database size</span></span>
<span data-ttu-id="d143f-118">A következő lekérdezés az adatbázis (megabájtban) a méretét adja vissza:</span><span class="sxs-lookup"><span data-stu-id="d143f-118">The following query returns the size of your database (in megabytes):</span></span>

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

<span data-ttu-id="d143f-119">A következő lekérdezés az adatbázis méretét (megabájtban) egyéni objektumokat adja vissza:</span><span class="sxs-lookup"><span data-stu-id="d143f-119">The following query returns the size of individual objects (in megabytes) in your database:</span></span>

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a><span data-ttu-id="d143f-120">Kapcsolatok figyelése</span><span class="sxs-lookup"><span data-stu-id="d143f-120">Monitoring connections</span></span>
<span data-ttu-id="d143f-121">Használhatja a [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) nézet az adott Azure SQL adatbázis-kiszolgálót és mindegyik kapcsolat részleteinek fennálló kapcsolatok vonatkozó információk lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="d143f-121">You can use the [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) view to retrieve information about the connections established to a specific Azure SQL Database server and the details of each connection.</span></span> <span data-ttu-id="d143f-122">Emellett a [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) nézet akkor hasznos, ha az összes aktív felhasználói kapcsolatok és belső feladatok adatainak lekérése.</span><span class="sxs-lookup"><span data-stu-id="d143f-122">In addition, the [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) view is helpful when retrieving information about all active user connections and internal tasks.</span></span>
<span data-ttu-id="d143f-123">Az alábbi lekérdezés lekéri az aktuális kapcsolat információkat:</span><span class="sxs-lookup"><span data-stu-id="d143f-123">The following query retrieves information on the current connection:</span></span>

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> <span data-ttu-id="d143f-124">Végrehajtásakor a **sys.dm_exec_requests** és **sys.dm_exec_sessions nézetek**, ha **adatbázis állapotának megtekintése** engedéllyel az adatbázishoz, megjelenik az összes végrehajtás alatt álló munkamenetek az adatbázisban. Ellenkező esetben tekintse meg az aktuális munkamenet.</span><span class="sxs-lookup"><span data-stu-id="d143f-124">When executing the **sys.dm_exec_requests** and **sys.dm_exec_sessions views**, if you have **VIEW DATABASE STATE** permission on the database, you see all executing sessions on the database; otherwise, you see only the current session.</span></span>
> 
> 

## <a name="monitoring-query-performance"></a><span data-ttu-id="d143f-125">Lekérdezés teljesítmény figyelése</span><span class="sxs-lookup"><span data-stu-id="d143f-125">Monitoring query performance</span></span>
<span data-ttu-id="d143f-126">Lassú vagy hosszú ideig futó lekérdezések jelentős rendszererőforrásokat is felhasználhatnak.</span><span class="sxs-lookup"><span data-stu-id="d143f-126">Slow or long running queries can consume significant system resources.</span></span> <span data-ttu-id="d143f-127">Ez a szakasz bemutatja, hogyan kell néhány gyakori lekérdezési teljesítmény problémák észlelése dinamikus felügyeleti nézetek használatával.</span><span class="sxs-lookup"><span data-stu-id="d143f-127">This section demonstrates how to use dynamic management views to detect a few common query performance problems.</span></span> <span data-ttu-id="d143f-128">A hibaelhárításhoz, egy régebbi, de továbbra is hasznos a hivatkozás a [teljesítményproblémák elhárítása az SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) a Microsoft TechNet cikket.</span><span class="sxs-lookup"><span data-stu-id="d143f-128">An older but still helpful reference for troubleshooting, is the [Troubleshooting Performance Problems in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) article on Microsoft TechNet.</span></span>

### <a name="finding-top-n-queries"></a><span data-ttu-id="d143f-129">Legfontosabb N lekérdezések keresése</span><span class="sxs-lookup"><span data-stu-id="d143f-129">Finding top N queries</span></span>
<span data-ttu-id="d143f-130">Az alábbi példa átlagos CPU-idő szerinti sorrendben első öt lekérdezések információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="d143f-130">The following example returns information about the top five queries ranked by average CPU time.</span></span> <span data-ttu-id="d143f-131">Ebben a példában a lekérdezés kivonat szerint a lekérdezések összesíti az, hogy logikailag egyenértékű lekérdezések saját összesítő hálózatierőforrás-fogyasztás szerint vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="d143f-131">This example aggregates the queries according to their query hash, so that logically equivalent queries are grouped by their cumulative resource consumption.</span></span>

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a><span data-ttu-id="d143f-132">Letiltott lekérdezések figyelése</span><span class="sxs-lookup"><span data-stu-id="d143f-132">Monitoring blocked queries</span></span>
<span data-ttu-id="d143f-133">Lassú vagy hosszan futó lekérdezések járulnak hozzá sok erőforrást használ, és letiltott lekérdezések következménye lehet.</span><span class="sxs-lookup"><span data-stu-id="d143f-133">Slow or long-running queries can contribute to excessive resource consumption and be the consequence of blocked queries.</span></span> <span data-ttu-id="d143f-134">A blokkolás okát gyenge alkalmazás tervét, hibás lekérdezésterveket, hasznos indexek, és így tovább hiánya lehet.</span><span class="sxs-lookup"><span data-stu-id="d143f-134">The cause of the blocking can be poor application design, bad query plans, the lack of useful indexes, and so on.</span></span> <span data-ttu-id="d143f-135">A sys.dm_tran_locks nézet segítségével az Azure SQL-adatbázisban az aktuális zárolási tevékenység adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d143f-135">You can use the sys.dm_tran_locks view to get information about the current locking activity in your Azure SQL Database.</span></span> <span data-ttu-id="d143f-136">Például kódját, lásd: [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) az SQL Server Online könyvekben.</span><span class="sxs-lookup"><span data-stu-id="d143f-136">For example code, see [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in SQL Server Books Online.</span></span>

### <a name="monitoring-query-plans"></a><span data-ttu-id="d143f-137">Figyelési lekérdezésterveket</span><span class="sxs-lookup"><span data-stu-id="d143f-137">Monitoring query plans</span></span>
<span data-ttu-id="d143f-138">Egy hatékony lekérdezésterv is növelheti CPU-használat.</span><span class="sxs-lookup"><span data-stu-id="d143f-138">An inefficient query plan also may increase CPU consumption.</span></span> <span data-ttu-id="d143f-139">Az alábbi példában a [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) nézet segítségével meghatározhatja, melyik lekérdezés használja a legtöbb összegző CPU-t.</span><span class="sxs-lookup"><span data-stu-id="d143f-139">The following example uses the [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) view to determine which query uses the most cumulative CPU.</span></span>

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a><span data-ttu-id="d143f-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d143f-140">See also</span></span>
[<span data-ttu-id="d143f-141">SQL-adatbázis bemutatása</span><span class="sxs-lookup"><span data-stu-id="d143f-141">Introduction to SQL Database</span></span>](sql-database-technical-overview.md)


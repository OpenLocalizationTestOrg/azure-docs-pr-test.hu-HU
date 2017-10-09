---
title: "Azure SQL adatbázis használata a dinamikus felügyeleti nézetek aaaMonitoring |} Microsoft Docs"
description: "Megtudhatja, hogyan toodetect és a gyakori problémák diagnosztizálásához dinamikus felügyeleti nézetek toomonitor Microsoft Azure SQL Database segítségével."
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
ms.openlocfilehash: 43d5fe2dd9a38d031e9334f6ad49fce5866e3bec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a><span data-ttu-id="e7e6d-103">Az Azure SQL Database felügyelete dinamikus felügyeleti nézetek használatával</span><span class="sxs-lookup"><span data-stu-id="e7e6d-103">Monitoring Azure SQL Database using dynamic management views</span></span>
<span data-ttu-id="e7e6d-104">A Microsoft Azure SQL Database lehetővé teszi, hogy dinamikus felügyeleti részhalmazának megtekinti toodiagnose teljesítményproblémákat, amely valószínűleg az okozza letiltott vagy hosszan futó lekérdezések, erőforrás-keresztmetszetek, gyenge lekérdezésterveket, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-104">Microsoft Azure SQL Database enables a subset of dynamic management views toodiagnose performance problems, which might be caused by blocked or long-running queries, resource bottlenecks, poor query plans, and so on.</span></span> <span data-ttu-id="e7e6d-105">Ez a témakör bemutatja, hogy miként toodetect közös teljesítményproblémákat dinamikus felügyeleti nézetek használatával.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-105">This topic provides information on how toodetect common performance problems by using dynamic management views.</span></span>

<span data-ttu-id="e7e6d-106">SQL-adatbázis részben három kategóriáinak dinamikus felügyeleti nézetekkel támogatja:</span><span class="sxs-lookup"><span data-stu-id="e7e6d-106">SQL Database partially supports three categories of dynamic management views:</span></span>

* <span data-ttu-id="e7e6d-107">Adatbázissal kapcsolatos dinamikus felügyeleti nézetekkel.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-107">Database-related dynamic management views.</span></span>
* <span data-ttu-id="e7e6d-108">Végrehajtási kapcsolatos dinamikus felügyeleti nézetekkel.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-108">Execution-related dynamic management views.</span></span>
* <span data-ttu-id="e7e6d-109">Tranzakció-hoz kapcsolódó dinamikus felügyeleti nézetek.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-109">Transaction-related dynamic management views.</span></span>

<span data-ttu-id="e7e6d-110">A dinamikus felügyeleti nézetekkel részletes információkért lásd: [dinamikus felügyeleti nézetek és függvények (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) az SQL Server Online könyvekben.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-110">For detailed information on dynamic management views, see [Dynamic Management Views and Functions (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) in SQL Server Books Online.</span></span>

## <a name="permissions"></a><span data-ttu-id="e7e6d-111">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="e7e6d-111">Permissions</span></span>
<span data-ttu-id="e7e6d-112">Az SQL-adatbázis, a dinamikus felügyeleti nézetben lekérdezése szükséges **adatbázis állapotának megtekintése** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-112">In SQL Database, querying a dynamic management view requires **VIEW DATABASE STATE** permissions.</span></span> <span data-ttu-id="e7e6d-113">Hello **adatbázis állapotának megtekintése** engedély hello adatbázisban lévő objektumok információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-113">hello **VIEW DATABASE STATE** permission returns information about all objects within hello current database.</span></span>
<span data-ttu-id="e7e6d-114">toogrant hello **adatbázis állapotának megtekintése** engedély tooa adott adatbázis-felhasználó, futtassa a következő lekérdezés hello:</span><span class="sxs-lookup"><span data-stu-id="e7e6d-114">toogrant hello **VIEW DATABASE STATE** permission tooa specific database user, run hello following query:</span></span>

```GRANT VIEW DATABASE STATE toodatabase_user; ```

<span data-ttu-id="e7e6d-115">A helyszíni SQL Server egy példányát, a dinamikus felügyeleti nézetekkel kiszolgáló állapota adatokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-115">In an instance of on-premises SQL Server, dynamic management views return server state information.</span></span> <span data-ttu-id="e7e6d-116">Az SQL-adatbázis akkor csak az aktuális logikai adatbázis vonatkozó információkat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-116">In SQL Database, they return information regarding your current logical database only.</span></span>

## <a name="calculating-database-size"></a><span data-ttu-id="e7e6d-117">Adatbázisméret kiszámítása</span><span class="sxs-lookup"><span data-stu-id="e7e6d-117">Calculating database size</span></span>
<span data-ttu-id="e7e6d-118">hello következő lekérdezés visszaadja az adatbázis (megabájtban) hello méretét:</span><span class="sxs-lookup"><span data-stu-id="e7e6d-118">hello following query returns hello size of your database (in megabytes):</span></span>

```
-- Calculates hello size of hello database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

<span data-ttu-id="e7e6d-119">hello következő lekérdezés visszaadja hello méretét (megabájtban) egyéni objektumokat az adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="e7e6d-119">hello following query returns hello size of individual objects (in megabytes) in your database:</span></span>

```
-- Calculates hello size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a><span data-ttu-id="e7e6d-120">Kapcsolatok figyelése</span><span class="sxs-lookup"><span data-stu-id="e7e6d-120">Monitoring connections</span></span>
<span data-ttu-id="e7e6d-121">Használhatja a hello [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) hello fennálló kapcsolatok tooa adott Azure SQL Database-kiszolgálóhoz, és minden egyes kapcsolathoz hello részleteit tooretrieve adatainak megtekintése.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-121">You can use hello [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) view tooretrieve information about hello connections established tooa specific Azure SQL Database server and hello details of each connection.</span></span> <span data-ttu-id="e7e6d-122">Ezenkívül hello [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) nézet akkor hasznos, ha az összes aktív felhasználói kapcsolatok és belső feladatok adatainak lekérése.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-122">In addition, hello [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) view is helpful when retrieving information about all active user connections and internal tasks.</span></span>
<span data-ttu-id="e7e6d-123">hello alábbi lekérdezés lekéri hello aktuális kapcsolati információkat:</span><span class="sxs-lookup"><span data-stu-id="e7e6d-123">hello following query retrieves information on hello current connection:</span></span>

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
> <span data-ttu-id="e7e6d-124">Hello végrehajtásakor **sys.dm_exec_requests** és **sys.dm_exec_sessions nézetek**, ha **adatbázis állapotának megtekintése** engedély hello adatbázis, megtekintheti az összes végrehajtása munkamenetek hello adatbázison; Ellenkező esetben látni csak hello aktuális munkamenet.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-124">When executing hello **sys.dm_exec_requests** and **sys.dm_exec_sessions views**, if you have **VIEW DATABASE STATE** permission on hello database, you see all executing sessions on hello database; otherwise, you see only hello current session.</span></span>
> 
> 

## <a name="monitoring-query-performance"></a><span data-ttu-id="e7e6d-125">Lekérdezés teljesítmény figyelése</span><span class="sxs-lookup"><span data-stu-id="e7e6d-125">Monitoring query performance</span></span>
<span data-ttu-id="e7e6d-126">Lassú vagy hosszú ideig futó lekérdezések jelentős rendszererőforrásokat is felhasználhatnak.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-126">Slow or long running queries can consume significant system resources.</span></span> <span data-ttu-id="e7e6d-127">Ez a szakasz bemutatja, hogyan toouse dinamikus felügyeleti nézetek toodetect néhány gyakori lekérdezési teljesítményproblémákat.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-127">This section demonstrates how toouse dynamic management views toodetect a few common query performance problems.</span></span> <span data-ttu-id="e7e6d-128">A hibaelhárításhoz, egy régebbi, de továbbra is hasznos ilyen hivatkozás: hello [teljesítményproblémák elhárítása az SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) a Microsoft TechNet cikket.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-128">An older but still helpful reference for troubleshooting, is hello [Troubleshooting Performance Problems in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) article on Microsoft TechNet.</span></span>

### <a name="finding-top-n-queries"></a><span data-ttu-id="e7e6d-129">Legfontosabb N lekérdezések keresése</span><span class="sxs-lookup"><span data-stu-id="e7e6d-129">Finding top N queries</span></span>
<span data-ttu-id="e7e6d-130">hello alábbi példa információt ad vissza hello felső öt lekérdezések átlagos CPU-idő szerinti sorrendben.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-130">hello following example returns information about hello top five queries ranked by average CPU time.</span></span> <span data-ttu-id="e7e6d-131">Ebben a példában hello lekérdezések megfelelően tootheir lekérdezés kivonatoló összesíti az, hogy logikailag egyenértékű lekérdezések saját összesítő hálózatierőforrás-fogyasztás szerint vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-131">This example aggregates hello queries according tootheir query hash, so that logically equivalent queries are grouped by their cumulative resource consumption.</span></span>

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

### <a name="monitoring-blocked-queries"></a><span data-ttu-id="e7e6d-132">Letiltott lekérdezések figyelése</span><span class="sxs-lookup"><span data-stu-id="e7e6d-132">Monitoring blocked queries</span></span>
<span data-ttu-id="e7e6d-133">Lassú vagy hosszan futó lekérdezések járulnak tooexcessive erőforrás-felhasználás, és letiltott lekérdezések hello következménye lehet.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-133">Slow or long-running queries can contribute tooexcessive resource consumption and be hello consequence of blocked queries.</span></span> <span data-ttu-id="e7e6d-134">hello blokkolás hello oka lehet a gyenge alkalmazás tervét, hibás lekérdezésterveket, hello hasznos indexek hiánya, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-134">hello cause of hello blocking can be poor application design, bad query plans, hello lack of useful indexes, and so on.</span></span> <span data-ttu-id="e7e6d-135">Az Azure SQL Database hello sys.dm_tran_locks tooget adatainak megtekintése hello aktuális zárolási tevékenység használhatja.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-135">You can use hello sys.dm_tran_locks view tooget information about hello current locking activity in your Azure SQL Database.</span></span> <span data-ttu-id="e7e6d-136">Például kódját, lásd: [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) az SQL Server Online könyvekben.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-136">For example code, see [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in SQL Server Books Online.</span></span>

### <a name="monitoring-query-plans"></a><span data-ttu-id="e7e6d-137">Figyelési lekérdezésterveket</span><span class="sxs-lookup"><span data-stu-id="e7e6d-137">Monitoring query plans</span></span>
<span data-ttu-id="e7e6d-138">Egy hatékony lekérdezésterv is növelheti CPU-használat.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-138">An inefficient query plan also may increase CPU consumption.</span></span> <span data-ttu-id="e7e6d-139">hello alábbi példában hello [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) melyik lekérdezés használ hello legtöbb összegző CPU toodetermine megtekintése.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-139">hello following example uses hello [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) view toodetermine which query uses hello most cumulative CPU.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="e7e6d-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e7e6d-140">See also</span></span>
[<span data-ttu-id="e7e6d-141">Bevezetés tooSQL adatbázis</span><span class="sxs-lookup"><span data-stu-id="e7e6d-141">Introduction tooSQL Database</span></span>](sql-database-technical-overview.md)


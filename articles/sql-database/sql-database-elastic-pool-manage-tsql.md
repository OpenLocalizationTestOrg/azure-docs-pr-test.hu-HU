---
title: "T-SQL: Kezelése az Azure SQL Database rugalmas készlet |} Microsoft Docs"
description: "Egy Azure SQL Database rugalmas készlet kezelését a T-SQL használatával."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 4e288e17-bc3e-4255-9fbe-0a2ac0dbd7dd
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 05/27/2016
ms.author: srinia
ms.openlocfilehash: c6b64e4a7fd91283a37a792b294965064d653003
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-manage-an-elastic-pool-with-transact-sql"></a><span data-ttu-id="2da2f-103">Figyelheti és kezelheti a Transact-SQL rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="2da2f-103">Monitor and manage an elastic pool with Transact-SQL</span></span>
<span data-ttu-id="2da2f-104">Ez a témakör bemutatja, hogyan kezelheti méretezhető [rugalmas készletek](sql-database-elastic-pool.md) a Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="2da2f-104">This topic shows you how to manage scalable [elastic pools](sql-database-elastic-pool.md) with Transact-SQL.</span></span>  <span data-ttu-id="2da2f-105">Is létrehozása és kezelése az Azure rugalmas készletek a [Azure-portálon](https://portal.azure.com/), [PowerShell](sql-database-elastic-pool-manage-powershell.md), a REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="2da2f-105">You can also create and manage an Azure elastic pool the [Azure portal](https://portal.azure.com/), [PowerShell](sql-database-elastic-pool-manage-powershell.md), the REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="2da2f-106">Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="2da2f-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>


<span data-ttu-id="2da2f-107">Használja a [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) és [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) létrehozásához és az adatbázisok áthelyezése rugalmas készletek-parancsokat.</span><span class="sxs-lookup"><span data-stu-id="2da2f-107">Use the [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) and [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) commands to create and move databases into and out of elastic pools.</span></span> <span data-ttu-id="2da2f-108">A rugalmas készlet léteznie kell, ezek a parancsok használata előtt.</span><span class="sxs-lookup"><span data-stu-id="2da2f-108">The elastic pool must exist before you can use these commands.</span></span> <span data-ttu-id="2da2f-109">Ezek a parancsok csak adatbázisok vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="2da2f-109">These commands affect only databases.</span></span> <span data-ttu-id="2da2f-110">Az új tárolók létrehozását és a készlet tulajdonságait (például a minimális és maximális edtu-k) beállítását a T-SQL parancsokkal nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="2da2f-110">Creation of new pools and the setting of pool properties (such as min and max eDTUs) cannot be changed with T-SQL commands.</span></span>

## <a name="create-a-pooled-database-in-an-elastic-pool"></a><span data-ttu-id="2da2f-111">A rugalmas készletekben található készletezett-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="2da2f-111">Create a pooled database in an elastic pool</span></span>
<span data-ttu-id="2da2f-112">A CREATE DATABASE paranccsal és a SERVICE_OBJECTIVE lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2da2f-112">Use the CREATE DATABASE command with the SERVICE_OBJECTIVE option.</span></span>   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in an elastic named S3M100.

<span data-ttu-id="2da2f-113">A rugalmas készletekben található összes adatbázis öröklik a szolgáltatási szint a rugalmas készlet (Basic, Standard, Premium).</span><span class="sxs-lookup"><span data-stu-id="2da2f-113">All databases in an elastic pool inherit the service tier of the elastic pool (Basic, Standard, Premium).</span></span> 

## <a name="move-a-database-between-elastic-pools"></a><span data-ttu-id="2da2f-114">Egy adatbázis áthelyezése rugalmas készletek között</span><span class="sxs-lookup"><span data-stu-id="2da2f-114">Move a database between elastic pools</span></span>
<span data-ttu-id="2da2f-115">Az ALTER DATABASE parancs használata a módosítás, és állítsa be a szolgáltatás\_rugalmas OBJEKTÍV beállítást\_készlet.</span><span class="sxs-lookup"><span data-stu-id="2da2f-115">Use the ALTER DATABASE command with the MODIFY and set SERVICE\_OBJECTIVE option as ELASTIC\_POOL.</span></span> <span data-ttu-id="2da2f-116">A cél-készlet neve nevének megadása</span><span class="sxs-lookup"><span data-stu-id="2da2f-116">Set the name to the name of the target pool.</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to an elastic named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="2da2f-117">Egy adatbázis áthelyezése rugalmas készletbe</span><span class="sxs-lookup"><span data-stu-id="2da2f-117">Move a database into an elastic pool</span></span>
<span data-ttu-id="2da2f-118">Az ALTER DATABASE parancs használata a módosítás, és állítsa be a szolgáltatás\_ELASTIC_POOL OBJEKTÍV beállítást.</span><span class="sxs-lookup"><span data-stu-id="2da2f-118">Use the ALTER DATABASE command with the MODIFY and set SERVICE\_OBJECTIVE option as ELASTIC_POOL.</span></span> <span data-ttu-id="2da2f-119">A cél-készlet neve nevének megadása</span><span class="sxs-lookup"><span data-stu-id="2da2f-119">Set the name to the name of the target pool.</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to an elastic named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a><span data-ttu-id="2da2f-120">Egy adatbázis áthelyezése rugalmas készletek kívül</span><span class="sxs-lookup"><span data-stu-id="2da2f-120">Move a database out of an elastic pool</span></span>
<span data-ttu-id="2da2f-121">Az ALTER DATABASE parancs és a SERVICE_OBJECTIVE beállított teljesítmény szintjének (például S0 vagy S1).</span><span class="sxs-lookup"><span data-stu-id="2da2f-121">Use the ALTER DATABASE command and set the SERVICE_OBJECTIVE to one of the performance levels (such as S0 or S1).</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a><span data-ttu-id="2da2f-122">Lista adatbázisok rugalmas készlethez</span><span class="sxs-lookup"><span data-stu-id="2da2f-122">List databases in an elastic pool</span></span>
<span data-ttu-id="2da2f-123">Használja a [sys.database\_szolgáltatás \_célok nézet](https://msdn.microsoft.com/library/mt712619) az adatbázisok rugalmas készlethez listázásához.</span><span class="sxs-lookup"><span data-stu-id="2da2f-123">Use the [sys.database\_service \_objectives view](https://msdn.microsoft.com/library/mt712619) to list all the databases in an elastic pool.</span></span> <span data-ttu-id="2da2f-124">Jelentkezzen be a master adatbázis kérdezze le a nézetet.</span><span class="sxs-lookup"><span data-stu-id="2da2f-124">Log in to the master database to query the view.</span></span>

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-an-elastic-pool"></a><span data-ttu-id="2da2f-125">Erőforrás-használati adatok beolvasása a rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="2da2f-125">Get resource usage data for an elastic pool</span></span>
<span data-ttu-id="2da2f-126">Használja a [sys.elastic\_készlet \_erőforrás \_statisztikák nézet](https://msdn.microsoft.com/library/mt280062.aspx) logikai kiszolgálón rugalmas készletek erőforrás használati statisztikáit vizsgálata.</span><span class="sxs-lookup"><span data-stu-id="2da2f-126">Use the [sys.elastic\_pool \_resource \_stats view](https://msdn.microsoft.com/library/mt280062.aspx) to examine the resource usage statistics of an elastic pool on a logical server.</span></span> <span data-ttu-id="2da2f-127">Jelentkezzen be a master adatbázis kérdezze le a nézetet.</span><span class="sxs-lookup"><span data-stu-id="2da2f-127">Log in to the master database to query the view.</span></span>

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-a-pooled-database"></a><span data-ttu-id="2da2f-128">Erőforrás-használat lekérése egy készletezett adatbázis</span><span class="sxs-lookup"><span data-stu-id="2da2f-128">Get resource usage for a pooled database</span></span>
<span data-ttu-id="2da2f-129">Használja a [sys.dm\_ db\_ erőforrás\_statisztikák nézet](https://msdn.microsoft.com/library/dn800981.aspx) vagy [sys.resource \_statisztikák nézet](https://msdn.microsoft.com/library/dn269979.aspx) megvizsgálhatja a erőforrás kihasználtságának statisztikai adatai, az adatbázis egy rugalmas készlet.</span><span class="sxs-lookup"><span data-stu-id="2da2f-129">Use the [sys.dm\_ db\_ resource\_stats view](https://msdn.microsoft.com/library/dn800981.aspx) or [sys.resource \_stats view](https://msdn.microsoft.com/library/dn269979.aspx) to examine the resource usage statistics of a database in an elastic pool.</span></span> <span data-ttu-id="2da2f-130">Ez a folyamat hasonlít lekérdezése egy adatbázist erőforrás-felhasználása.</span><span class="sxs-lookup"><span data-stu-id="2da2f-130">This process is similar to querying resource usage for a single database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2da2f-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2da2f-131">Next steps</span></span>
<span data-ttu-id="2da2f-132">Miután létrehozta a rugalmas készlethez, a készlet rugalmas adatbázisok rugalmas feladat létrehozása kezelheti.</span><span class="sxs-lookup"><span data-stu-id="2da2f-132">After creating an elastic pool, you can manage elastic databases in the pool by creating elastic jobs.</span></span> <span data-ttu-id="2da2f-133">Rugalmas feladat futó T-SQL-szkriptek használatát a készletben lévő adatbázisok tetszőleges számú megkönnyítése.</span><span class="sxs-lookup"><span data-stu-id="2da2f-133">Elastic jobs facilitate running T-SQL scripts against any number of databases in the pool.</span></span> <span data-ttu-id="2da2f-134">További információkért lásd: [rugalmas adatbázis-feladatok áttekintése](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2da2f-134">For more information, see [Elastic database jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

<span data-ttu-id="2da2f-135">Lásd: [az Azure SQL Database kiterjesztése](sql-database-elastic-scale-introduction.md): rugalmas adatbáziseszközöket használhat a horizontális felskálázás helyezi át az adatokat, lekérdezése vagy tranzakciók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2da2f-135">See [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md): use elastic database tools to scale out, move data, query, or create transactions.</span></span>


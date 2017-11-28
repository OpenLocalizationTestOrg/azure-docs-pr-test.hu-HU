---
title: "T-SQL: Kezelése az Azure SQL Database rugalmas készlet |} Microsoft Docs"
description: "Használja a T-SQL toomanage egy Azure SQL Database rugalmas készlet."
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
ms.openlocfilehash: 666f131b2c88849a1a9ea83381bbc27548e93599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-an-elastic-pool-with-transact-sql"></a><span data-ttu-id="2ec66-103">Figyelheti és kezelheti a Transact-SQL rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="2ec66-103">Monitor and manage an elastic pool with Transact-SQL</span></span>
<span data-ttu-id="2ec66-104">Ez a témakör bemutatja, hogyan méretezhető toomanage [rugalmas készletek](sql-database-elastic-pool.md) a Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="2ec66-104">This topic shows you how toomanage scalable [elastic pools](sql-database-elastic-pool.md) with Transact-SQL.</span></span>  <span data-ttu-id="2ec66-105">Is létrehozása és kezelése az Azure rugalmas készlet hello [Azure-portálon](https://portal.azure.com/), [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="2ec66-105">You can also create and manage an Azure elastic pool hello [Azure portal](https://portal.azure.com/), [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="2ec66-106">Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="2ec66-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>


<span data-ttu-id="2ec66-107">Használjon hello [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) és [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) parancsok toocreate, az áthelyezés adatbázisok esetében bejövő és kimenő rugalmas készletek.</span><span class="sxs-lookup"><span data-stu-id="2ec66-107">Use hello [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) and [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) commands toocreate and move databases into and out of elastic pools.</span></span> <span data-ttu-id="2ec66-108">a rugalmas készlet hello léteznie kell, ezek a parancsok használata előtt.</span><span class="sxs-lookup"><span data-stu-id="2ec66-108">hello elastic pool must exist before you can use these commands.</span></span> <span data-ttu-id="2ec66-109">Ezek a parancsok csak adatbázisok vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="2ec66-109">These commands affect only databases.</span></span> <span data-ttu-id="2ec66-110">Az új tárolók létrehozását és a készlet tulajdonságait (például a minimális és maximális edtu-k) hello beállítása nem lehet módosítani, T-SQL parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="2ec66-110">Creation of new pools and hello setting of pool properties (such as min and max eDTUs) cannot be changed with T-SQL commands.</span></span>

## <a name="create-a-pooled-database-in-an-elastic-pool"></a><span data-ttu-id="2ec66-111">A rugalmas készletekben található készletezett-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ec66-111">Create a pooled database in an elastic pool</span></span>
<span data-ttu-id="2ec66-112">Hello CREATE DATABASE parancs használata hello SERVICE_OBJECTIVE lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2ec66-112">Use hello CREATE DATABASE command with hello SERVICE_OBJECTIVE option.</span></span>   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in an elastic named S3M100.

<span data-ttu-id="2ec66-113">A rugalmas készletekben található összes adatbázis hello szolgáltatási szint rugalmas készlet hello (Basic, Standard, Premium) öröklik.</span><span class="sxs-lookup"><span data-stu-id="2ec66-113">All databases in an elastic pool inherit hello service tier of hello elastic pool (Basic, Standard, Premium).</span></span> 

## <a name="move-a-database-between-elastic-pools"></a><span data-ttu-id="2ec66-114">Egy adatbázis áthelyezése rugalmas készletek között</span><span class="sxs-lookup"><span data-stu-id="2ec66-114">Move a database between elastic pools</span></span>
<span data-ttu-id="2ec66-115">Hello ALTER DATABASE parancs használata hello módosítás, és állítsa be a szolgáltatás\_rugalmas OBJEKTÍV beállítást\_készlet.</span><span class="sxs-lookup"><span data-stu-id="2ec66-115">Use hello ALTER DATABASE command with hello MODIFY and set SERVICE\_OBJECTIVE option as ELASTIC\_POOL.</span></span> <span data-ttu-id="2ec66-116">Hello célkészlettel hello neve toohello nevének beállítása.</span><span class="sxs-lookup"><span data-stu-id="2ec66-116">Set hello name toohello name of hello target pool.</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move hello database named db1 tooan elastic named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="2ec66-117">Egy adatbázis áthelyezése rugalmas készletbe</span><span class="sxs-lookup"><span data-stu-id="2ec66-117">Move a database into an elastic pool</span></span>
<span data-ttu-id="2ec66-118">Hello ALTER DATABASE parancs használata hello módosítás, és állítsa be a szolgáltatás\_ELASTIC_POOL OBJEKTÍV beállítást.</span><span class="sxs-lookup"><span data-stu-id="2ec66-118">Use hello ALTER DATABASE command with hello MODIFY and set SERVICE\_OBJECTIVE option as ELASTIC_POOL.</span></span> <span data-ttu-id="2ec66-119">Hello célkészlettel hello neve toohello nevének beállítása.</span><span class="sxs-lookup"><span data-stu-id="2ec66-119">Set hello name toohello name of hello target pool.</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move hello database named db1 tooan elastic named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a><span data-ttu-id="2ec66-120">Egy adatbázis áthelyezése rugalmas készletek kívül</span><span class="sxs-lookup"><span data-stu-id="2ec66-120">Move a database out of an elastic pool</span></span>
<span data-ttu-id="2ec66-121">Hello ALTER DATABASE paranccsal, és állítsa be a hello SERVICE_OBJECTIVE tooone a hello teljesítményszintek (pl. S0 vagy S1).</span><span class="sxs-lookup"><span data-stu-id="2ec66-121">Use hello ALTER DATABASE command and set hello SERVICE_OBJECTIVE tooone of hello performance levels (such as S0 or S1).</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes hello database into a stand-alone database with hello service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a><span data-ttu-id="2ec66-122">Lista adatbázisok rugalmas készlethez</span><span class="sxs-lookup"><span data-stu-id="2ec66-122">List databases in an elastic pool</span></span>
<span data-ttu-id="2ec66-123">Használjon hello [sys.database\_szolgáltatás \_célok nézet](https://msdn.microsoft.com/library/mt712619) toolist összes hello adatbázisok rugalmas készlethez.</span><span class="sxs-lookup"><span data-stu-id="2ec66-123">Use hello [sys.database\_service \_objectives view](https://msdn.microsoft.com/library/mt712619) toolist all hello databases in an elastic pool.</span></span> <span data-ttu-id="2ec66-124">Jelentkezzen be toohello master adatbázis tooquery hello nézet.</span><span class="sxs-lookup"><span data-stu-id="2ec66-124">Log in toohello master database tooquery hello view.</span></span>

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-an-elastic-pool"></a><span data-ttu-id="2ec66-125">Erőforrás-használati adatok beolvasása a rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="2ec66-125">Get resource usage data for an elastic pool</span></span>
<span data-ttu-id="2ec66-126">Használjon hello [sys.elastic\_készlet \_erőforrás \_statisztikák nézet](https://msdn.microsoft.com/library/mt280062.aspx) tooexamine hello erőforrás kihasználtságának statisztikai adatai egy rugalmas készlet logikai kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="2ec66-126">Use hello [sys.elastic\_pool \_resource \_stats view](https://msdn.microsoft.com/library/mt280062.aspx) tooexamine hello resource usage statistics of an elastic pool on a logical server.</span></span> <span data-ttu-id="2ec66-127">Jelentkezzen be toohello master adatbázis tooquery hello nézet.</span><span class="sxs-lookup"><span data-stu-id="2ec66-127">Log in toohello master database tooquery hello view.</span></span>

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-a-pooled-database"></a><span data-ttu-id="2ec66-128">Erőforrás-használat lekérése egy készletezett adatbázis</span><span class="sxs-lookup"><span data-stu-id="2ec66-128">Get resource usage for a pooled database</span></span>
<span data-ttu-id="2ec66-129">Használjon hello [sys.dm\_ db\_ erőforrás\_statisztikák nézet](https://msdn.microsoft.com/library/dn800981.aspx) vagy [sys.resource \_statisztikák nézet](https://msdn.microsoft.com/library/dn269979.aspx) tooexamine hello erőforrás használati statisztikáit a a rugalmas készletekben található adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2ec66-129">Use hello [sys.dm\_ db\_ resource\_stats view](https://msdn.microsoft.com/library/dn800981.aspx) or [sys.resource \_stats view](https://msdn.microsoft.com/library/dn269979.aspx) tooexamine hello resource usage statistics of a database in an elastic pool.</span></span> <span data-ttu-id="2ec66-130">Ez a folyamat hasonló tooquerying erőforrás-felhasználása egyetlen adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="2ec66-130">This process is similar tooquerying resource usage for a single database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2ec66-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2ec66-131">Next steps</span></span>
<span data-ttu-id="2ec66-132">Miután létrehozta a rugalmas készlethez, hello készletben rugalmas adatbázisok rugalmas feladat létrehozása kezelheti.</span><span class="sxs-lookup"><span data-stu-id="2ec66-132">After creating an elastic pool, you can manage elastic databases in hello pool by creating elastic jobs.</span></span> <span data-ttu-id="2ec66-133">A rugalmas feladatok lehetővé teszi futó T-SQL-szkriptek használatát tetszőleges számú adatbázishoz hello készletben.</span><span class="sxs-lookup"><span data-stu-id="2ec66-133">Elastic jobs facilitate running T-SQL scripts against any number of databases in hello pool.</span></span> <span data-ttu-id="2ec66-134">További információkért lásd: [rugalmas adatbázis-feladatok áttekintése](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2ec66-134">For more information, see [Elastic database jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

<span data-ttu-id="2ec66-135">Lásd: [az Azure SQL Database kiterjesztése](sql-database-elastic-scale-introduction.md): rugalmas adatbázis eszközök tooscale kimenő használnak, akkor az adatok áthelyezése, lekérdezése vagy tranzakciók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2ec66-135">See [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md): use elastic database tools tooscale out, move data, query, or create transactions.</span></span>


---
title: "aaaPause, folytatásához méretezése a T-SQL Azure SQL Data warehouse |} Microsoft Docs"
description: "Transact-SQL (T-SQL) feladatok tooscale kibővített teljesítmény dwu-k beállításával. Költségeket takaríthat vissza csúcsidőszakon kívüli időszakokban."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: a970d939-2adf-4856-8a78-d4fe8ab2cceb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 03/30/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 84c6868acb673221d8853319ac9a05bb98b2b7c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a><span data-ttu-id="b6da8-104">Számítási teljesítményt az Azure SQL Data Warehouse (T-SQL) kezelése</span><span class="sxs-lookup"><span data-stu-id="b6da8-104">Manage compute power in Azure SQL Data Warehouse (T-SQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b6da8-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b6da8-105">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="b6da8-106">Portál</span><span class="sxs-lookup"><span data-stu-id="b6da8-106">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="b6da8-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b6da8-107">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="b6da8-108">REST</span><span class="sxs-lookup"><span data-stu-id="b6da8-108">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="b6da8-109">TSQL</span><span class="sxs-lookup"><span data-stu-id="b6da8-109">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a><span data-ttu-id="b6da8-110">Aktuális DWU beállításainak megjelenítése</span><span class="sxs-lookup"><span data-stu-id="b6da8-110">View current DWU settings</span></span>
<span data-ttu-id="b6da8-111">tooview hello jelenlegi DWU beállításai az adatbázisok esetében:</span><span class="sxs-lookup"><span data-stu-id="b6da8-111">tooview hello current DWU settings for your databases:</span></span>

1. <span data-ttu-id="b6da8-112">Nyissa meg az SQL Server Object Explorert a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="b6da8-112">Open SQL Server Object Explorer in Visual Studio.</span></span>
2. <span data-ttu-id="b6da8-113">Csatlakozás hello logikai SQL Database-kiszolgálóhoz társított toohello főadatbázis.</span><span class="sxs-lookup"><span data-stu-id="b6da8-113">Connect toohello master database associated with hello logical SQL Database server.</span></span>
3. <span data-ttu-id="b6da8-114">Válassza ki a hello sys.database_service_objectives dinamikus kezelési nézetet.</span><span class="sxs-lookup"><span data-stu-id="b6da8-114">Select from hello sys.database_service_objectives dynamic management view.</span></span> <span data-ttu-id="b6da8-115">Például:</span><span class="sxs-lookup"><span data-stu-id="b6da8-115">Here is an example:</span></span> 

```sql
SELECT
    db.name [Database]
,   ds.edition [Edition]
,   ds.service_objective [Service Objective]
FROM
    sys.database_service_objectives ds
JOIN
    sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a><span data-ttu-id="b6da8-116">Skála számítási</span><span class="sxs-lookup"><span data-stu-id="b6da8-116">Scale compute</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="b6da8-117">toochange hello dwu-k számát:</span><span class="sxs-lookup"><span data-stu-id="b6da8-117">toochange hello DWUs:</span></span>

1. <span data-ttu-id="b6da8-118">Toohello master adatbázis az SQL Database logikai kiszolgálóhoz csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="b6da8-118">Connect toohello master database associated with your logical SQL Database server.</span></span>
2. <span data-ttu-id="b6da8-119">Használjon hello [ALTER DATABASE] [ ALTER DATABASE] TSQL utasítást.</span><span class="sxs-lookup"><span data-stu-id="b6da8-119">Use hello [ALTER DATABASE][ALTER DATABASE] TSQL statement.</span></span> <span data-ttu-id="b6da8-120">hello alábbi mintakód hello szolgáltatási szint célkitűzésének tooDW1000 hello adatbázis MySQLDW.</span><span class="sxs-lookup"><span data-stu-id="b6da8-120">hello following example sets hello service level objective tooDW1000 for hello database MySQLDW.</span></span> 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state-and-operation-progress"></a><span data-ttu-id="b6da8-121">Ellenőrizze az adatbázis állapotát és a művelet folyamatban van</span><span class="sxs-lookup"><span data-stu-id="b6da8-121">Check database state and operation progress</span></span>

1. <span data-ttu-id="b6da8-122">Toohello master adatbázis az SQL Database logikai kiszolgálóhoz csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="b6da8-122">Connect toohello master database associated with your logical SQL Database server.</span></span>
2. <span data-ttu-id="b6da8-123">Küldje el a lekérdezés toocheck adatbázis állapota</span><span class="sxs-lookup"><span data-stu-id="b6da8-123">Submit query toocheck database state</span></span>

```sql
SELECT *
FROM
sys.databases
```

3. <span data-ttu-id="b6da8-124">Küldje el a művelet toocheck állapotának lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b6da8-124">Submit query toocheck status of operation</span></span>

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```

<span data-ttu-id="b6da8-125">A DMV fog tevékenység információt nyújt az SQL Data Warehouse hello művelet és hello állapotának hello művelet, amely befejeződött vagy IN_PROGRESS fog lehet például a különböző felügyeleti műveleteihez.</span><span class="sxs-lookup"><span data-stu-id="b6da8-125">This DMV will return information about various management operations on your SQL Data Warehouse such as hello operation and hello state of hello operation, which will either be IN_PROGRESS or COMPLETED.</span></span>



<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="b6da8-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6da8-126">Next steps</span></span>
<span data-ttu-id="b6da8-127">Más felügyeleti feladatokat [kezelése-áttekintés][Management overview].</span><span class="sxs-lookup"><span data-stu-id="b6da8-127">For other management tasks, see [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute power overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

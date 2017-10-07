---
title: "aaaPause, folytathatja, az Azure SQL Data Warehouse REST méretezést |} Microsoft Docs"
description: "Számítási teljesítményt az SQL Data Warehouse REST, a T-SQL és a PowerShell segítségével kezelheti."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: 21de7337-9356-49bb-a6eb-06c1beeba2c4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 07/25/2017
ms.author: elbutter
ms.openlocfilehash: fc867febb118fb5c86c2637a41b232076021b95d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a><span data-ttu-id="b3608-103">Számítási teljesítményt az Azure SQL Data Warehouse (REST) kezelése</span><span class="sxs-lookup"><span data-stu-id="b3608-103">Manage compute power in Azure SQL Data Warehouse (REST)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b3608-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b3608-104">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="b3608-105">Portál</span><span class="sxs-lookup"><span data-stu-id="b3608-105">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="b3608-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b3608-106">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="b3608-107">REST</span><span class="sxs-lookup"><span data-stu-id="b3608-107">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="b3608-108">TSQL</span><span class="sxs-lookup"><span data-stu-id="b3608-108">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a><span data-ttu-id="b3608-109">Skála számítási teljesítmény</span><span class="sxs-lookup"><span data-stu-id="b3608-109">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="b3608-110">toochange hello dwu-k, használja a hello [létrehozás vagy frissítés adatbázis] [ Create or Update Database] REST API-t.</span><span class="sxs-lookup"><span data-stu-id="b3608-110">toochange hello DWUs, use hello [Create or Update Database][Create or Update Database] REST API.</span></span> <span data-ttu-id="b3608-111">hello alábbi mintakód hello szolgáltatási szint célkitűzésének tooDW1000 hello adatbázis MySQLDW, amely üzemelteti a MyServer kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="b3608-111">hello following example sets hello service level objective tooDW1000 for hello database MySQLDW which is hosted on server MyServer.</span></span> <span data-ttu-id="b3608-112">hello kiszolgáló ResourceGroup1 egy Azure erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="b3608-112">hello server is in an Azure resource group named ResourceGroup1.</span></span>

```
PATCH https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="b3608-113">Felfüggesztés számítási</span><span class="sxs-lookup"><span data-stu-id="b3608-113">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="b3608-114">egy adatbázis toopause hello használata [szünet adatbázis] [ Pause Database] REST API-t.</span><span class="sxs-lookup"><span data-stu-id="b3608-114">toopause a database, use hello [Pause Database][Pause Database] REST API.</span></span> <span data-ttu-id="b3608-115">hello alábbi példa felfüggesztése kiszolgalo01 nevű kiszolgálón található Database02 nevű adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b3608-115">hello following example pauses a database named Database02 hosted on a server named Server01.</span></span> <span data-ttu-id="b3608-116">hello kiszolgáló ResourceGroup1 egy Azure erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="b3608-116">hello server is in an Azure resource group named ResourceGroup1.</span></span>

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="b3608-117">Folytatás számítási</span><span class="sxs-lookup"><span data-stu-id="b3608-117">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="b3608-118">egy adatbázis toostart hello használata [folytatása adatbázis] [ Resume Database] REST API-t.</span><span class="sxs-lookup"><span data-stu-id="b3608-118">toostart a database, use hello [Resume Database][Resume Database] REST API.</span></span> <span data-ttu-id="b3608-119">hello következő példában elindul kiszolgalo01 nevű kiszolgálón található Database02 nevű adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b3608-119">hello following example starts a database named Database02 hosted on a server named Server01.</span></span> <span data-ttu-id="b3608-120">hello kiszolgáló ResourceGroup1 egy Azure erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="b3608-120">hello server is in an Azure resource group named ResourceGroup1.</span></span> 

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a><span data-ttu-id="b3608-121">Ellenőrizze az adatbázis állapota</span><span class="sxs-lookup"><span data-stu-id="b3608-121">Check database state</span></span>

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="b3608-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3608-122">Next steps</span></span>
<span data-ttu-id="b3608-123">Más felügyeleti feladatokat [kezelése-áttekintés][Management overview].</span><span class="sxs-lookup"><span data-stu-id="b3608-123">For other management tasks, see [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Pause Database]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Resume Database]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Create or Update Database]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

---
title: "aaaManage számítási teljesítményt az Azure SQL Data Warehouse (PowerShell) |} Microsoft Docs"
description: "PowerShell feladatok toomanage számítási teljesítményt. Skála dwu-k beállításával számítási erőforrásokat. Vagy, és sablonok felfüggesztése és folytatása a számítási erőforrások toosave költségeket."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 8354a3c1-4e04-4809-933f-db414a8c74dc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 8b379d4cf89570649767f6896d2c630d4f1111d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a><span data-ttu-id="381ae-105">Az Azure SQL Data Warehouse (PowerShell) a számítási teljesítmény kezelése</span><span class="sxs-lookup"><span data-stu-id="381ae-105">Manage compute power in Azure SQL Data Warehouse (PowerShell)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="381ae-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="381ae-106">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="381ae-107">Portál</span><span class="sxs-lookup"><span data-stu-id="381ae-107">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="381ae-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="381ae-108">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="381ae-109">REST</span><span class="sxs-lookup"><span data-stu-id="381ae-109">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="381ae-110">TSQL</span><span class="sxs-lookup"><span data-stu-id="381ae-110">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="before-you-begin"></a><span data-ttu-id="381ae-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="381ae-111">Before you begin</span></span>
### <a name="install-hello-latest-version-of-azure-powershell"></a><span data-ttu-id="381ae-112">Hello Azure PowerShell legújabb verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="381ae-112">Install hello latest version of Azure PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="381ae-113">az SQL Data Warehouse szolgáltatással Azure PowerShell toouse, kell Azure PowerShell 1.0.3-as vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="381ae-113">toouse Azure PowerShell with SQL Data Warehouse, you need Azure PowerShell version 1.0.3 or greater.</span></span>  <span data-ttu-id="381ae-114">tooverify jelenlegi verziójával paranccsal hello **Get-Module - ListAvailable-Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="381ae-114">tooverify your current version run hello command **Get-Module -ListAvailable -Name Azure**.</span></span> <span data-ttu-id="381ae-115">Telepítheti a legújabb verziót hello [Microsoft Webplatform-telepítő][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="381ae-115">You can install hello latest version from [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="381ae-116">További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt][How tooinstall and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="381ae-116">For more information, see [How tooinstall and configure Azure PowerShell][How tooinstall and configure Azure PowerShell].</span></span>
>
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a><span data-ttu-id="381ae-117">Ismerkedés az Azure PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="381ae-117">Get started with Azure PowerShell cmdlets</span></span>
<span data-ttu-id="381ae-118">tooget lépések:</span><span class="sxs-lookup"><span data-stu-id="381ae-118">tooget started:</span></span>

1. <span data-ttu-id="381ae-119">Nyissa meg az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="381ae-119">Open Azure PowerShell.</span></span>
2. <span data-ttu-id="381ae-120">Hello PowerShell-parancssorba ezek a parancsok toosign toohello Azure Resource Manager futnak, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="381ae-120">At hello PowerShell prompt, run these commands toosign in toohello Azure Resource Manager and select your subscription.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a><span data-ttu-id="381ae-121">Skála számítási teljesítmény</span><span class="sxs-lookup"><span data-stu-id="381ae-121">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="381ae-122">toochange hello dwu-k, használja a hello [Set-AzureRmSqlDatabase] [ Set-AzureRmSqlDatabase] PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="381ae-122">toochange hello DWUs, use hello [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase] PowerShell cmdlet.</span></span> <span data-ttu-id="381ae-123">hello alábbi mintakód hello szolgáltatási szint célkitűzésének tooDW1000 hello adatbázis MySQLDW, amely üzemelteti a MyServer kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="381ae-123">hello following example sets hello service level objective tooDW1000 for hello database MySQLDW which is hosted on server MyServer.</span></span>

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="381ae-124">Felfüggesztés számítási</span><span class="sxs-lookup"><span data-stu-id="381ae-124">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="381ae-125">egy adatbázis toopause hello használata [Suspend-AzureRmSqlDatabase] [ Suspend-AzureRmSqlDatabase] parancsmag.</span><span class="sxs-lookup"><span data-stu-id="381ae-125">toopause a database, use hello [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase] cmdlet.</span></span> <span data-ttu-id="381ae-126">hello alábbi példa felfüggesztése kiszolgalo01 nevű kiszolgálón található Database02 nevű adatbázis.</span><span class="sxs-lookup"><span data-stu-id="381ae-126">hello following example pauses a database named Database02 hosted on a server named Server01.</span></span> <span data-ttu-id="381ae-127">hello kiszolgáló ResourceGroup1 egy Azure erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="381ae-127">hello server is in an Azure resource group named ResourceGroup1.</span></span>

> [!NOTE]
> <span data-ttu-id="381ae-128">Figyelje meg, hogy ha a kiszolgáló foo.database.windows.net, nem használható "foo" - kiszolgálónév hello hello PowerShell-parancsmagok a.</span><span class="sxs-lookup"><span data-stu-id="381ae-128">Note that if your server is foo.database.windows.net, use "foo" as hello -ServerName in hello PowerShell cmdlets.</span></span>
>
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
<span data-ttu-id="381ae-129">A módosítást, a következő példa hello adatbázis lekéri a hello $database objektumba.</span><span class="sxs-lookup"><span data-stu-id="381ae-129">A variation, this next example retrieves hello database into hello $database object.</span></span> <span data-ttu-id="381ae-130">Az ezt követően átadja hello objektum túl[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="381ae-130">It then pipes hello object too[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span></span> <span data-ttu-id="381ae-131">hello eredmények hello objektum resultDatabase vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="381ae-131">hello results are stored in hello object resultDatabase.</span></span> <span data-ttu-id="381ae-132">hello utolsó parancs hello eredményeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="381ae-132">hello final command shows hello results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="381ae-133">Folytatás számítási</span><span class="sxs-lookup"><span data-stu-id="381ae-133">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="381ae-134">egy adatbázis toostart hello használata [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] parancsmag.</span><span class="sxs-lookup"><span data-stu-id="381ae-134">toostart a database, use hello [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] cmdlet.</span></span> <span data-ttu-id="381ae-135">hello következő példában elindul kiszolgalo01 nevű kiszolgálón található Database02 nevű adatbázis.</span><span class="sxs-lookup"><span data-stu-id="381ae-135">hello following example starts a database named Database02 hosted on a server named Server01.</span></span> <span data-ttu-id="381ae-136">hello kiszolgáló ResourceGroup1 egy Azure erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="381ae-136">hello server is in an Azure resource group named ResourceGroup1.</span></span>

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

<span data-ttu-id="381ae-137">A módosítást, a következő példa hello adatbázis lekéri a hello $database objektumba.</span><span class="sxs-lookup"><span data-stu-id="381ae-137">A variation, this next example retrieves hello database into hello $database object.</span></span> <span data-ttu-id="381ae-138">Az ezt követően átadja hello objektum túl[Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] és $resultDatabase hello eredmények tárolja.</span><span class="sxs-lookup"><span data-stu-id="381ae-138">It then pipes hello object too[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] and stores hello results in $resultDatabase.</span></span> <span data-ttu-id="381ae-139">hello utolsó parancs hello eredményeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="381ae-139">hello final command shows hello results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state"></a><span data-ttu-id="381ae-140">Ellenőrizze az adatbázis állapota</span><span class="sxs-lookup"><span data-stu-id="381ae-140">Check database state</span></span>

<span data-ttu-id="381ae-141">Ahogy fent példák hello, egy használható [Get-AzureRmSqlDatabase] [ Get-AzureRmSqlDatabase] parancsmag tooget információk egy adatbázist, ezáltal a hello állapotát, de is, amelynek argumentuma toouse ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="381ae-141">As shown in hello above examples, one can use [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase] cmdlet tooget information on a database, thereby checking hello status, but also toouse as an argument.</span></span> 

```powershell
Get-AzureRmSqlDatabase [-ResourceGroupName] <String> [-ServerName] <String> [[-DatabaseName] <String>]
 [-InformationAction <ActionPreference>] [-InformationVariable <String>] [-Confirm] [-WhatIf]
 [<CommonParameters>]
```

<span data-ttu-id="381ae-142">Amely eredményt valamit meg</span><span class="sxs-lookup"><span data-stu-id="381ae-142">Which will result in something like</span></span> 

```powershell   
ResourceGroupName             : nytrg
ServerName                    : nytsvr
DatabaseName                  : nytdb
Location                      : West US
DatabaseId                    : 86461aae-8e3d-4ded-9389-ac9d4bc69bbb
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1General_CP1CI_AS
CatalogCollation              :
MaxSizeBytes                  : 32212254720
Status                        : Online
CreationDate                  : 10/26/2016 4:33:14 PM
CurrentServiceObjectiveId     : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
CurrentServiceObjectiveName   : System2
RequestedServiceObjectiveId   : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           : 1/1/0001 12:00:00 AM
```

<span data-ttu-id="381ae-143">Ha ezután ellenőrizheti toosee hello *állapot* hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="381ae-143">Where you can then check toosee hello *Status* of hello database.</span></span> <span data-ttu-id="381ae-144">Ebben az esetben láthatja, hogy az adatbázis online állapotban.</span><span class="sxs-lookup"><span data-stu-id="381ae-144">In this case, you can see that this database is online.</span></span> 

<span data-ttu-id="381ae-145">Ez a parancs futtatásakor vagy Online, felfüggesztése, folytatása, méretezés és felfüggesztett állapot értéket kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="381ae-145">When you run this command, you should receive a Status value of either Online, Pausing, Resuming, Scaling, and Paused.</span></span>

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="381ae-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="381ae-146">Next steps</span></span>
<span data-ttu-id="381ae-147">Más felügyeleti feladatokat [kezelése-áttekintés][Management overview].</span><span class="sxs-lookup"><span data-stu-id="381ae-147">For other management tasks, see [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Get-AzureRmSqlDatabase]: /powershell/servicemanagement/azure.sqldatabase/v1.6.1/get-azuresqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/

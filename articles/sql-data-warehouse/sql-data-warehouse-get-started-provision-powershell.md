---
title: "az SQL Data Warehouse aaaCreate PowerShell használatával |} Microsoft Docs"
description: "SQL Data Warehouse létrehozása PowerShell használatával"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 97434863-7938-4129-8949-5a119f5949e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: d8af29ec285a11285785ab5474e4dfc8c36bc3ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-sql-data-warehouse-using-powershell"></a><span data-ttu-id="fd8b3-103">SQL Data Warehouse létrehozása PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="fd8b3-103">Create SQL Data Warehouse using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fd8b3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fd8b3-104">Azure Portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="fd8b3-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="fd8b3-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="fd8b3-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fd8b3-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="fd8b3-107">Ez a cikk bemutatja, hogyan toocreate egy SQL Data Warehouse PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-107">This article shows you how toocreate a SQL Data Warehouse using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd8b3-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fd8b3-108">Prerequisites</span></span>
<span data-ttu-id="fd8b3-109">tooget elindult, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="fd8b3-109">tooget started, you need:</span></span>

* <span data-ttu-id="fd8b3-110">**Azure-fiók**: keresse fel [Azure ingyenes próbaverzió] [ Azure Free Trial] vagy [MSDN Azure-Krediteket] [ MSDN Azure Credits] toocreate fiókkal.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] toocreate an account.</span></span>
* <span data-ttu-id="fd8b3-111">**Azure SQL-kiszolgáló**: lásd: [Azure SQL-adatbázis létrehozása az Azure portál hello] [ Create an Azure SQL database in hello Azure Portal] vagy [Azure SQL-adatbázis létrehozása a PowerShell használatával] [ Create an Azure SQL database with PowerShell] további részleteket.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-111">**Azure SQL server**:  See [Create an Azure SQL database in hello Azure Portal][Create an Azure SQL database in hello Azure Portal] or [Create an Azure SQL database with PowerShell][Create an Azure SQL database with PowerShell] for more details.</span></span>
* <span data-ttu-id="fd8b3-112">**Erőforráscsoport**: hello azonos erőforrás csoport és az Azure SQL server, vagy tekintse meg használjon [hogyan toocreate erőforráscsoport](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fd8b3-112">**Resource group**: Either use hello same resource group as your Azure SQL server or see [how toocreate a resource group](../azure-resource-manager/resource-group-portal.md).</span></span>
* <span data-ttu-id="fd8b3-113">**PowerShell 1.0.3-as vagy újabb verzió**: A verziószámot a **Get-Module -ListAvailable -Name Azure** futtatásával ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-113">**PowerShell version 1.0.3 or greater**:  You can check your version by running **Get-Module -ListAvailable -Name Azure**.</span></span>  <span data-ttu-id="fd8b3-114">a legújabb verzió hello telepíthető [Microsoft Webplatform-telepítő][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="fd8b3-114">hello latest version can be installed from [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="fd8b3-115">Hello legújabb verzió telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt][How tooinstall and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="fd8b3-115">For more information on installing hello latest version, see [How tooinstall and configure Azure PowerShell][How tooinstall and configure Azure PowerShell].</span></span>

> [!NOTE]
> <span data-ttu-id="fd8b3-116">A SQL Data Warehouse létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-116">Creating a SQL Data Warehouse may result in a new billable service.</span></span>  <span data-ttu-id="fd8b3-117">További információ a díjszabásról: [Az SQL Data Warehouse díjszabása][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="fd8b3-117">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details on pricing.</span></span>
>
>

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="fd8b3-118">SQL Data Warehouse létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd8b3-118">Create a SQL Data Warehouse</span></span>
1. <span data-ttu-id="fd8b3-119">Nyissa meg a Windows PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-119">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="fd8b3-120">Ez a parancsmag toologin tooAzure erőforrás-kezelő futtatni.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-120">Run this cmdlet toologin tooAzure Resource Manager.</span></span>

    ```Powershell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="fd8b3-121">Válassza ki a jelenlegi munkamenethez használni kívánt toouse hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-121">Select hello subscription you want toouse for your current session.</span></span>

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. <span data-ttu-id="fd8b3-122">Hozza létre az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-122">Create database.</span></span> <span data-ttu-id="fd8b3-123">Ebben a példában a "mynewsqldw", objektív szolgáltatásiszint "DW400", "sqldwserver1", amely "mywesteuroperesgp1" nevű erőforráscsoportban hello elnevezésű toohello server nevű adatbázist hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-123">This example creates a database named "mynewsqldw", with service objective level "DW400", toohello server named "sqldwserver1", which is in hello resource group named "mywesteuroperesgp1".</span></span>

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

<span data-ttu-id="fd8b3-124">A szükséges paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="fd8b3-124">Required Parameters are:</span></span>

* <span data-ttu-id="fd8b3-125">**RequestedServiceObjectiveName**: hello mennyisége [DWU] [ DWU] kért.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-125">**RequestedServiceObjectiveName**: hello amount of [DWU][DWU] you are requesting.</span></span>  <span data-ttu-id="fd8b3-126">A támogatott értékek a következők: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 és DW6000.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-126">Supported values are: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000, and DW6000.</span></span>
* <span data-ttu-id="fd8b3-127">**DatabaseName**: hello SQL Data Warehouse létrehozása hello nevét.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-127">**DatabaseName**: hello name of hello SQL Data Warehouse that you are creating.</span></span>
* <span data-ttu-id="fd8b3-128">**Kiszolgálónév**: hello hello kiszolgáló nevét, amely a létrehozásához használ (12-es kell lennie).</span><span class="sxs-lookup"><span data-stu-id="fd8b3-128">**ServerName**: hello name of hello server that you are using for creation (must be V12).</span></span>
* <span data-ttu-id="fd8b3-129">**ResourceGroupName**: A használt erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-129">**ResourceGroupName**: Resource group you are using.</span></span>  <span data-ttu-id="fd8b3-130">elérhető erőforráscsoportok toofind előfizetése használja a Get-azureresource parancsot.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-130">toofind available resource groups in your subscription use Get-AzureResource.</span></span>
* <span data-ttu-id="fd8b3-131">**Edition**: kell lennie "DataWarehouse" toocreate SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-131">**Edition**: Must be "DataWarehouse" toocreate a SQL Data Warehouse.</span></span>

<span data-ttu-id="fd8b3-132">A választható paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="fd8b3-132">Optional Parameters are:</span></span>

* <span data-ttu-id="fd8b3-133">**%{Collationname/**: hello alapértelmezett rendezését, ha nincs megadva az SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-133">**CollationName**: hello default collation if not specified is SQL_Latin1_General_CP1_CI_AS.</span></span>  <span data-ttu-id="fd8b3-134">Az adatbázisok rendezése nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-134">Collation cannot be changed on a database.</span></span>
* <span data-ttu-id="fd8b3-135">**MaxSizeBytes**: hello alapértelmezett maximális adatbázis mérete 10 GB-os.</span><span class="sxs-lookup"><span data-stu-id="fd8b3-135">**MaxSizeBytes**: hello default max size of a database is 10 GB.</span></span>

<span data-ttu-id="fd8b3-136">További hello paraméter lehetőségekről további információkért lásd: [New-AzureRmSqlDatabase] [ New-AzureRmSqlDatabase] és [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].</span><span class="sxs-lookup"><span data-stu-id="fd8b3-136">For more details on hello parameter options, see [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] and [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd8b3-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd8b3-137">Next steps</span></span>
<span data-ttu-id="fd8b3-138">Miután befejezte az SQL Data Warehouse kiépítése azt szeretné, hogy tootry [mintaadatokat] [ loading sample data] vagy részleteket tudhat meg túl[fejlesztése] [ develop] , [betölteni][load], vagy [áttelepítése][migrate].</span><span class="sxs-lookup"><span data-stu-id="fd8b3-138">After your SQL Data Warehouse has finished provisioning you may want tootry [loading sample data][loading sample data] or check out how too[develop][develop], [load][load], or [migrate][migrate].</span></span>

<span data-ttu-id="fd8b3-139">Ha szeretné használni az toomanage SQL Data Warehouse programozott módon, tekintse meg a hogyan toouse [PowerShell-parancsmagok és a REST API-k][PowerShell cmdlets and REST APIs].</span><span class="sxs-lookup"><span data-stu-id="fd8b3-139">If you're interested in more on how toomanage SQL Data Warehouse programmatically, check out our article on how toouse [PowerShell cmdlets and REST APIs][PowerShell cmdlets and REST APIs].</span></span>

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-load-with-bcp.md
[loading sample data]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlets and REST APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[how toocreate a SQL Data Warehouse from hello Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Create an Azure SQL database in hello Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-get-started-powershell.md
[how toocreate a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

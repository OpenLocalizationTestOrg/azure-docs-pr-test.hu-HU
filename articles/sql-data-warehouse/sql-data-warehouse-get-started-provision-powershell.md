---
title: "SQL Data Warehouse létrehozása PowerShell használatával | Microsoft Docs"
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
ms.openlocfilehash: a763f1c600c1a3f37cb565a8eb7db3c3f27dcf75
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-sql-data-warehouse-using-powershell"></a><span data-ttu-id="027fc-103">SQL Data Warehouse létrehozása PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="027fc-103">Create SQL Data Warehouse using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="027fc-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="027fc-104">Azure Portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="027fc-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="027fc-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="027fc-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="027fc-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="027fc-107">Ebből a cikkből megtudhatja, hogyan hozható létre az SQL Data Warehouse a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="027fc-107">This article shows you how to create a SQL Data Warehouse using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="027fc-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="027fc-108">Prerequisites</span></span>
<span data-ttu-id="027fc-109">A kezdéshez a következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="027fc-109">To get started, you need:</span></span>

* <span data-ttu-id="027fc-110">**Azure-fiók**: A fiók létrehozásával kapcsolatban lásd: [Ingyenes Azure-fiók létrehozása][Azure Free Trial] vagy [Havi Azure-kredit a Visual Studio-előfizetőknek][MSDN Azure Credits].</span><span class="sxs-lookup"><span data-stu-id="027fc-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] to create an account.</span></span>
* <span data-ttu-id="027fc-111">**Azure SQL Server**: Az [Azure SQL Database-adatbázis létrehozása az Azure Portal használatával][Create an Azure SQL database in the Azure Portal] vagy az [Azure SQL Database-adatbázis létrehozása a PowerShell használatával][Create an Azure SQL database with PowerShell] című cikkekben talál további információt.</span><span class="sxs-lookup"><span data-stu-id="027fc-111">**Azure SQL server**:  See [Create an Azure SQL database in the Azure Portal][Create an Azure SQL database in the Azure Portal] or [Create an Azure SQL database with PowerShell][Create an Azure SQL database with PowerShell] for more details.</span></span>
* <span data-ttu-id="027fc-112">**Erőforráscsoport**: Használja ugyanazt az erőforráscsoportot, mint az Azure SQL-kiszolgáló, vagy tekintse át az [erőforráscsoportok létrehozásával foglalkozó](../azure-resource-manager/resource-group-portal.md) cikket.</span><span class="sxs-lookup"><span data-stu-id="027fc-112">**Resource group**: Either use the same resource group as your Azure SQL server or see [how to create a resource group](../azure-resource-manager/resource-group-portal.md).</span></span>
* <span data-ttu-id="027fc-113">**PowerShell 1.0.3-as vagy újabb verzió**: A verziószámot a **Get-Module -ListAvailable -Name Azure** futtatásával ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="027fc-113">**PowerShell version 1.0.3 or greater**:  You can check your version by running **Get-Module -ListAvailable -Name Azure**.</span></span>  <span data-ttu-id="027fc-114">A legújabb verziót a [Microsoft Webplatform-telepítőből][Microsoft Web Platform Installer] telepítheti.</span><span class="sxs-lookup"><span data-stu-id="027fc-114">The latest version can be installed from [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="027fc-115">A legújabb verzió telepítésével kapcsolatban lásd: [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell] (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="027fc-115">For more information on installing the latest version, see [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell].</span></span>

> [!NOTE]
> <span data-ttu-id="027fc-116">A SQL Data Warehouse létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="027fc-116">Creating a SQL Data Warehouse may result in a new billable service.</span></span>  <span data-ttu-id="027fc-117">További információ a díjszabásról: [Az SQL Data Warehouse díjszabása][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="027fc-117">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details on pricing.</span></span>
>
>

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="027fc-118">SQL Data Warehouse létrehozása</span><span class="sxs-lookup"><span data-stu-id="027fc-118">Create a SQL Data Warehouse</span></span>
1. <span data-ttu-id="027fc-119">Nyissa meg a Windows PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="027fc-119">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="027fc-120">A parancsmag futtatásával jelentkezzen be az Azure Resource Managerbe.</span><span class="sxs-lookup"><span data-stu-id="027fc-120">Run this cmdlet to login to Azure Resource Manager.</span></span>

    ```Powershell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="027fc-121">Válassza ki a jelenlegi munkamenethez használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="027fc-121">Select the subscription you want to use for your current session.</span></span>

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. <span data-ttu-id="027fc-122">Hozza létre az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="027fc-122">Create database.</span></span> <span data-ttu-id="027fc-123">Ebben a példában egy adatbázist hozunk létre „mynewsqldw” néven, „DW400” szintű szolgáltatási céllal az „sqldwserver1” elnevezésű kiszolgálóra, amely a „mywesteuroperesgp1” nevű erőforráscsoportban található.</span><span class="sxs-lookup"><span data-stu-id="027fc-123">This example creates a database named "mynewsqldw", with service objective level "DW400", to the server named "sqldwserver1", which is in the resource group named "mywesteuroperesgp1".</span></span>

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

<span data-ttu-id="027fc-124">A szükséges paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="027fc-124">Required Parameters are:</span></span>

* <span data-ttu-id="027fc-125">**RequestedServiceObjectiveName**: A kért [DWU][DWU]-mennyiség.</span><span class="sxs-lookup"><span data-stu-id="027fc-125">**RequestedServiceObjectiveName**: The amount of [DWU][DWU] you are requesting.</span></span>  <span data-ttu-id="027fc-126">A támogatott értékek a következők: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 és DW6000.</span><span class="sxs-lookup"><span data-stu-id="027fc-126">Supported values are: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000, and DW6000.</span></span>
* <span data-ttu-id="027fc-127">**DatabaseName**: A létrehozandó SQL Data Warehouse neve.</span><span class="sxs-lookup"><span data-stu-id="027fc-127">**DatabaseName**: The name of the SQL Data Warehouse that you are creating.</span></span>
* <span data-ttu-id="027fc-128">**ServerName**: A létrehozás során használt kiszolgáló neve (V12-nek kell lennie).</span><span class="sxs-lookup"><span data-stu-id="027fc-128">**ServerName**: The name of the server that you are using for creation (must be V12).</span></span>
* <span data-ttu-id="027fc-129">**ResourceGroupName**: A használt erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="027fc-129">**ResourceGroupName**: Resource group you are using.</span></span>  <span data-ttu-id="027fc-130">Az előfizetésben elérhető erőforráscsoportok kereséséhez használja a Get-AzureResource parancsot.</span><span class="sxs-lookup"><span data-stu-id="027fc-130">To find available resource groups in your subscription use Get-AzureResource.</span></span>
* <span data-ttu-id="027fc-131">**Edition**: Egy SQL Data Warehouse létrehozásához a „DataWarehouse” értéket kell megadni.</span><span class="sxs-lookup"><span data-stu-id="027fc-131">**Edition**: Must be "DataWarehouse" to create a SQL Data Warehouse.</span></span>

<span data-ttu-id="027fc-132">A választható paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="027fc-132">Optional Parameters are:</span></span>

* <span data-ttu-id="027fc-133">**CollationName**: Ha nincs megadva, az alapértelmezett rendezés: SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="027fc-133">**CollationName**: The default collation if not specified is SQL_Latin1_General_CP1_CI_AS.</span></span>  <span data-ttu-id="027fc-134">Az adatbázisok rendezése nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="027fc-134">Collation cannot be changed on a database.</span></span>
* <span data-ttu-id="027fc-135">**MaxSizeBytes**: Alapértelmezés szerint az adatbázisok maximális mérete 10 GB.</span><span class="sxs-lookup"><span data-stu-id="027fc-135">**MaxSizeBytes**: The default max size of a database is 10 GB.</span></span>

<span data-ttu-id="027fc-136">A paraméterbeállításokkal kapcsolatos további információk: [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] és [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)] (Adatbázis létrehozása (Azure SQL Data Warehouse)).</span><span class="sxs-lookup"><span data-stu-id="027fc-136">For more details on the parameter options, see [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] and [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].</span></span>

## <a name="next-steps"></a><span data-ttu-id="027fc-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="027fc-137">Next steps</span></span>
<span data-ttu-id="027fc-138">Miután az SQL Data Warehouse kiépítése befejeződött, [mintaadatokat tölthet be][loading sample data], vagy további részleteket tudhat meg a [fejlesztés][develop], [betöltés][load] vagy [áttelepítés][migrate] mikéntjéről.</span><span class="sxs-lookup"><span data-stu-id="027fc-138">After your SQL Data Warehouse has finished provisioning you may want to try [loading sample data][loading sample data] or check out how to [develop][develop], [load][load], or [migrate][migrate].</span></span>

<span data-ttu-id="027fc-139">Ha érdekli az SQL Data Warehouse programozott módon való kezelése, tekintse meg a [PowerShell-parancsmagok és a REST API-k][PowerShell cmdlets and REST APIs] használatával kapcsolatos cikkünket.</span><span class="sxs-lookup"><span data-stu-id="027fc-139">If you're interested in more on how to manage SQL Data Warehouse programmatically, check out our article on how to use [PowerShell cmdlets and REST APIs][PowerShell cmdlets and REST APIs].</span></span>

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-load-with-bcp.md
[loading sample data]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlets and REST APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Create an Azure SQL database in the Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-get-started-powershell.md
[how to create a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

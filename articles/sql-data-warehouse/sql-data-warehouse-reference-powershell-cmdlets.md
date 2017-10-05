---
title: Az Azure SQL Data Warehouse PowerShell-parancsmagjai
description: "A felső PowerShell-parancsmagok az Azure SQL Data Warehouse figyeléséről, valamint szüneteltetéséről és folytatásáról adatbázis található."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 6f0d5772-f05f-4cc8-9749-4adb153dfd50
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: ce3e11587c2e0cb92923868a4f26d7f59c7ef4ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a><span data-ttu-id="f430e-103">PowerShell-parancsmagok és a REST API-k, az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f430e-103">PowerShell cmdlets and REST APIs for SQL Data Warehouse</span></span>
<span data-ttu-id="f430e-104">Az SQL Data Warehouse számos feladat Azure PowerShell-parancsmagokkal vagy a REST API-k kezelhetők.</span><span class="sxs-lookup"><span data-stu-id="f430e-104">Many SQL Data Warehouse administration tasks can be managed using either Azure PowerShell cmdlets or REST APIs.</span></span>  <span data-ttu-id="f430e-105">Az alábbiakban néhány olyan PowerShell-parancsok használata az SQL Data Warehouse a gyakori feladatok automatizálására.</span><span class="sxs-lookup"><span data-stu-id="f430e-105">Below are some examples of how to use PowerShell commands to automate common tasks in your SQL Data Warehouse.</span></span>  <span data-ttu-id="f430e-106">Egyes jó REST, tekintse meg a cikk [kezelése a REST-méretezhetőség][Manage scalability with REST].</span><span class="sxs-lookup"><span data-stu-id="f430e-106">For some good REST examples, see the article [Manage scalability with REST][Manage scalability with REST].</span></span>

> [!NOTE]
> <span data-ttu-id="f430e-107">Az SQL Data Warehouse szolgáltatással Azure PowerShell használatához szüksége Azure PowerShell 1.0.3-as vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="f430e-107">In order to use Azure PowerShell with SQL Data Warehouse, you need Azure PowerShell version 1.0.3 or greater.</span></span>  <span data-ttu-id="f430e-108">A verzió futtatásával ellenőrizheti **Get-Module - ListAvailable-Name Azure**.</span><span class="sxs-lookup"><span data-stu-id="f430e-108">You can check your version by running **Get-Module -ListAvailable -Name Azure**.</span></span>  <span data-ttu-id="f430e-109">A legújabb verzió telepíthető [Microsoft Webplatform-telepítő][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="f430e-109">The latest version can be installed from  [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="f430e-110">A legújabb verzió telepítésével kapcsolatban lásd: [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell] (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="f430e-110">For more information on installing the latest version, see [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell].</span></span>
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a><span data-ttu-id="f430e-111">Ismerkedés az Azure PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="f430e-111">Get started with Azure PowerShell cmdlets</span></span>
1. <span data-ttu-id="f430e-112">Nyissa meg a Windows PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="f430e-112">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="f430e-113">A PowerShell-parancssorba, futtassa az alábbi parancsokat az Azure Resource Managerrel történő bejelentkezéshez, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="f430e-113">At the PowerShell prompt, run these commands to sign in to the Azure Resource Manager and select your subscription.</span></span>
   
    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a><span data-ttu-id="f430e-114">Felfüggesztés SQL adatok adatraktár – példa</span><span class="sxs-lookup"><span data-stu-id="f430e-114">Pause SQL Data Warehouse Example</span></span>
<span data-ttu-id="f430e-115">A "Kiszolgalo01." nevű kiszolgáló által üzemeltetett "Database02" nevű adatbázis felfüggesztése</span><span class="sxs-lookup"><span data-stu-id="f430e-115">Pause a database named "Database02" hosted on a server named "Server01."</span></span>  <span data-ttu-id="f430e-116">A kiszolgáló van egy Azure erőforráscsoport neve "ResourceGroup1."</span><span class="sxs-lookup"><span data-stu-id="f430e-116">The server is in an Azure resource group named "ResourceGroup1."</span></span>

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
<span data-ttu-id="f430e-117">Módosítás, ez a példa kiszolgálókészletéhez a lekérdezett objektum [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="f430e-117">A variation, this example pipes the retrieved object to [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].</span></span>  <span data-ttu-id="f430e-118">Ennek eredményeképpen az adatbázis fel van függesztve.</span><span class="sxs-lookup"><span data-stu-id="f430e-118">As a result, the database is paused.</span></span> <span data-ttu-id="f430e-119">A záró parancs az eredményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f430e-119">The final command shows the results.</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a><span data-ttu-id="f430e-120">Indítsa el az SQL Data Warehouse – példa</span><span class="sxs-lookup"><span data-stu-id="f430e-120">Start SQL Data Warehouse Example</span></span>
<span data-ttu-id="f430e-121">A "Kiszolgalo01." nevű kiszolgáló által üzemeltetett "Database02" nevű adatbázis folytatása</span><span class="sxs-lookup"><span data-stu-id="f430e-121">Resume operation of a database named "Database02" hosted on a server named "Server01."</span></span> <span data-ttu-id="f430e-122">A kiszolgálón lévő "ResourceGroup1." nevű erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="f430e-122">The server is contained in a resource group named "ResourceGroup1."</span></span>

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

<span data-ttu-id="f430e-123">Módosítás, ez a példa lekéri "Database02" néven "Kiszolgalo01", "ResourceGroup1." nevű erőforráscsoportban található nevű kiszolgálóról adatbázis</span><span class="sxs-lookup"><span data-stu-id="f430e-123">A variation, this example retrieves a database named "Database02" from a server named "Server01" that is contained in a resource group named "ResourceGroup1."</span></span> <span data-ttu-id="f430e-124">A lekérdezett objektum kiszolgálókészletéhez azt [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].</span><span class="sxs-lookup"><span data-stu-id="f430e-124">It pipes the retrieved object to [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].</span></span>

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> <span data-ttu-id="f430e-125">Figyelje meg, hogy ha a kiszolgáló foo.database.windows.net, "foo" használják a PowerShell-parancsmagok - kiszolgálónév.</span><span class="sxs-lookup"><span data-stu-id="f430e-125">Note that if your server is foo.database.windows.net, use "foo" as the -ServerName in the PowerShell cmdlets.</span></span>
> 
> 

## <a name="other-supported-powershell-cmdlets"></a><span data-ttu-id="f430e-126">PowerShell-parancsmagok támogatott</span><span class="sxs-lookup"><span data-stu-id="f430e-126">Other supported PowerShell cmdlets</span></span>
<span data-ttu-id="f430e-127">Azure SQL Data Warehouse PowerShell-parancsmagok használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="f430e-127">These PowerShell cmdlets are supported with Azure SQL Data Warehouse.</span></span>

* <span data-ttu-id="f430e-128">[Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="f430e-128">[Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="f430e-129">[Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]</span><span class="sxs-lookup"><span data-stu-id="f430e-129">[Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]</span></span>
* <span data-ttu-id="f430e-130">[Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]</span><span class="sxs-lookup"><span data-stu-id="f430e-130">[Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]</span></span>
* <span data-ttu-id="f430e-131">[Új-AzureRmSqlDatabase][New-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="f430e-131">[New-AzureRmSqlDatabase][New-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="f430e-132">[Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="f430e-132">[Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="f430e-133">[Visszaállítás-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="f430e-133">[Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="f430e-134">[RESUME-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="f430e-134">[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="f430e-135">[SELECT-AzureRmSubscription][Select-AzureRmSubscription]</span><span class="sxs-lookup"><span data-stu-id="f430e-135">[Select-AzureRmSubscription][Select-AzureRmSubscription]</span></span>
* <span data-ttu-id="f430e-136">[Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="f430e-136">[Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]</span></span>
* <span data-ttu-id="f430e-137">[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]</span><span class="sxs-lookup"><span data-stu-id="f430e-137">[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]</span></span>

## <a name="next-steps"></a><span data-ttu-id="f430e-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f430e-138">Next steps</span></span>
<span data-ttu-id="f430e-139">További PowerShell című részben talál példákat:</span><span class="sxs-lookup"><span data-stu-id="f430e-139">For more PowerShell examples, see:</span></span>

* <span data-ttu-id="f430e-140">[PowerShell-lel SQL Data Warehouse létrehozása][Create a SQL Data Warehouse using PowerShell]</span><span class="sxs-lookup"><span data-stu-id="f430e-140">[Create a SQL Data Warehouse using PowerShell][Create a SQL Data Warehouse using PowerShell]</span></span>
* <span data-ttu-id="f430e-141">[Adatbázis visszaállítása][Database restore]</span><span class="sxs-lookup"><span data-stu-id="f430e-141">[Database restore][Database restore]</span></span>

<span data-ttu-id="f430e-142">Egyéb feladatokat, melyekhez a PowerShell segítségével automatizálható, lásd: [Azure SQL adatbázis parancsmagok][Azure SQL Database Cmdlets].</span><span class="sxs-lookup"><span data-stu-id="f430e-142">For other tasks which can be automated with PowerShell, see [Azure SQL Database Cmdlets][Azure SQL Database Cmdlets].</span></span> <span data-ttu-id="f430e-143">Vegye figyelembe, hogy nem minden Azure SQL Database-parancsmagok az Azure SQL Data Warehouse támogatottak.</span><span class="sxs-lookup"><span data-stu-id="f430e-143">Note that not all Azure SQL Database cmdlets are supported for Azure SQL Data Warehouse.</span></span>  <span data-ttu-id="f430e-144">Automatizálható a többi feladatok listájáért lásd: [műveletek az Azure SQL-adatbázisok][Operations for Azure SQL Databases].</span><span class="sxs-lookup"><span data-stu-id="f430e-144">For a list of tasks which can be automated with REST, see [Operations for Azure SQL Databases][Operations for Azure SQL Databases].</span></span>

<!--Image references-->

<!--Article references-->
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Operations for Azure SQL Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Remove-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps

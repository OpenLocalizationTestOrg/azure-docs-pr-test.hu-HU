---
title: "PowerShell példa-visszaállítási-biztonsági mentés-Azure SQL-adatbázis |} Microsoft Docs"
description: "Az Azure PowerShell-példa parancsfájl georedundáns biztonsági másolatból Azure SQL-adatbázis visszaállítása"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: ae1d0c828ae1e7e1e7e07dcc7d6157187a3859d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-restore-an-azure-sql-database-from-backups"></a><span data-ttu-id="57efa-103">Azure SQL-adatbázis visszaállítása biztonsági másolatból a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="57efa-103">Use PowerShell to restore an Azure SQL database from backups</span></span>

<span data-ttu-id="57efa-104">A PowerShell-mintaparancsfájl Azure SQL-adatbázis visszaállítása egy georedundáns biztonsági másolatból, törölt Azure SQL-adatbázis visszaállítása a legfrissebb biztonsági másolat, és visszaállítja az Azure SQL-adatbázis adott időben.</span><span class="sxs-lookup"><span data-stu-id="57efa-104">This PowerShell script example restores an Azure SQL database from a geo-redundant backup, restores a deleted Azure SQL database to its latest backup, and restores an Azure SQL database to a specific point in time.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="57efa-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="57efa-105">Sample script</span></span>

<span data-ttu-id="57efa-106">[!code-powershell[fő](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "SQL-adatbázis létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="57efa-106">[!code-powershell[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="57efa-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="57efa-107">Clean up deployment</span></span>

<span data-ttu-id="57efa-108">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="57efa-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="57efa-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="57efa-109">Script explanation</span></span>

<span data-ttu-id="57efa-110">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="57efa-110">This script uses the following commands.</span></span> <span data-ttu-id="57efa-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="57efa-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="57efa-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="57efa-112">Command</span></span> | <span data-ttu-id="57efa-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="57efa-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="57efa-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="57efa-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="57efa-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="57efa-115">Creates a resource group in which all resources are stored.</span></span> | [<span data-ttu-id="57efa-116">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="57efa-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="57efa-117">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="57efa-117">Creates a logical server that hosts a database or elastic pool.</span></span> | 
| [<span data-ttu-id="57efa-118">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="57efa-118">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="57efa-119">Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="57efa-119">Creates a database in a logical server as a single or a pooled database.</span></span> |
[<span data-ttu-id="57efa-120">Get-AzureRmSqlDatabaseGeoBackup</span><span class="sxs-lookup"><span data-stu-id="57efa-120">Get-AzureRmSqlDatabaseGeoBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | <span data-ttu-id="57efa-121">Egy adatbázis georedundáns biztonsági másolatot kap.</span><span class="sxs-lookup"><span data-stu-id="57efa-121">Gets a geo-redundant backup of a database.</span></span> |
| [<span data-ttu-id="57efa-122">Visszaállítás-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="57efa-122">Restore-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | <span data-ttu-id="57efa-123">SQL-adatbázis visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="57efa-123">Restores a SQL database.</span></span> |
|[<span data-ttu-id="57efa-124">Remove-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="57efa-124">Remove-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | <span data-ttu-id="57efa-125">Eltávolítja az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="57efa-125">Removes an Azure SQL database.</span></span> |
| [<span data-ttu-id="57efa-126">Get-AzureRmSqlDeletedDatabaseBackup</span><span class="sxs-lookup"><span data-stu-id="57efa-126">Get-AzureRmSqlDeletedDatabaseBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | <span data-ttu-id="57efa-127">Lekérdezi a törölt adatbázisok, amelyek is helyreállíthatja.</span><span class="sxs-lookup"><span data-stu-id="57efa-127">Gets a deleted database that you can restore.</span></span> |
| [<span data-ttu-id="57efa-128">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="57efa-128">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="57efa-129">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="57efa-129">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="57efa-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="57efa-130">Next steps</span></span>

<span data-ttu-id="57efa-131">Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="57efa-131">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="57efa-132">További SQL Database PowerShell parancsfájl minták megtalálhatók a [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="57efa-132">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

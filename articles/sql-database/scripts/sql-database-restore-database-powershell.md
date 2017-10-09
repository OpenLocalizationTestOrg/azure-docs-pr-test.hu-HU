---
title: "aaaPowerShell példa-visszaállítási-biztonsági mentés-Azure SQL-adatbázis |} Microsoft Docs"
description: "Az Azure PowerShell például parancsfájl-toorestore georedundáns biztonsági másolatból Azure SQL-adatbázis"
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
ms.openlocfilehash: 68becb89e8a8680aa2efc3de8ad947e674c5fc35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toorestore-an-azure-sql-database-from-backups"></a><span data-ttu-id="a0fcb-103">Használjon PowerShell toorestore Azure SQL-adatbázis biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="a0fcb-103">Use PowerShell toorestore an Azure SQL database from backups</span></span>

<span data-ttu-id="a0fcb-104">A PowerShell-mintaparancsfájl egy Azure SQL-adatbázis visszaállítása egy georedundáns biztonsági másolatból, visszaállítása egy törölt Azure SQL adatbázis tooits legfrissebb biztonsági másolat és az Azure SQL adatbázis tooa adott pontja visszaállítja az időben.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-104">This PowerShell script example restores an Azure SQL database from a geo-redundant backup, restores a deleted Azure SQL database tooits latest backup, and restores an Azure SQL database tooa specific point in time.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="a0fcb-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="a0fcb-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="a0fcb-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="a0fcb-106">Clean up deployment</span></span>

<span data-ttu-id="a0fcb-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="a0fcb-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="a0fcb-108">Script explanation</span></span>

<span data-ttu-id="a0fcb-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-109">This script uses hello following commands.</span></span> <span data-ttu-id="a0fcb-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a0fcb-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="a0fcb-111">Command</span></span> | <span data-ttu-id="a0fcb-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a0fcb-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a0fcb-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a0fcb-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="a0fcb-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-114">Creates a resource group in which all resources are stored.</span></span> | [<span data-ttu-id="a0fcb-115">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="a0fcb-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="a0fcb-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-116">Creates a logical server that hosts a database or elastic pool.</span></span> | 
| [<span data-ttu-id="a0fcb-117">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="a0fcb-117">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="a0fcb-118">Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-118">Creates a database in a logical server as a single or a pooled database.</span></span> |
[<span data-ttu-id="a0fcb-119">Get-AzureRmSqlDatabaseGeoBackup</span><span class="sxs-lookup"><span data-stu-id="a0fcb-119">Get-AzureRmSqlDatabaseGeoBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | <span data-ttu-id="a0fcb-120">Egy adatbázis georedundáns biztonsági másolatot kap.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-120">Gets a geo-redundant backup of a database.</span></span> |
| [<span data-ttu-id="a0fcb-121">Visszaállítás-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="a0fcb-121">Restore-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | <span data-ttu-id="a0fcb-122">SQL-adatbázis visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-122">Restores a SQL database.</span></span> |
|[<span data-ttu-id="a0fcb-123">Remove-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="a0fcb-123">Remove-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | <span data-ttu-id="a0fcb-124">Eltávolítja az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-124">Removes an Azure SQL database.</span></span> |
| [<span data-ttu-id="a0fcb-125">Get-AzureRmSqlDeletedDatabaseBackup</span><span class="sxs-lookup"><span data-stu-id="a0fcb-125">Get-AzureRmSqlDeletedDatabaseBackup</span></span>](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | <span data-ttu-id="a0fcb-126">Lekérdezi a törölt adatbázisok, amelyek is helyreállíthatja.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-126">Gets a deleted database that you can restore.</span></span> |
| [<span data-ttu-id="a0fcb-127">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a0fcb-127">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="a0fcb-128">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="a0fcb-128">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a0fcb-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0fcb-129">Next steps</span></span>

<span data-ttu-id="a0fcb-130">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a0fcb-130">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="a0fcb-131">További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a0fcb-131">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

---
title: "aaaPowerShell példa-import-bacpac fájl – az Azure SQL database |} Microsoft Docs"
description: "Az Azure PowerShell-példa parancsfájl tooimport egy bacpac csempére az SQL-adatbázisba"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: b85fca1c7fd52037d74254980469f9f53906448e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooimport-a-bacpac-file-into-an-azure-sql-database"></a><span data-ttu-id="30f8b-103">Az Azure SQL adatbázishoz PowerShell tooimport bacpac fájl használata</span><span class="sxs-lookup"><span data-stu-id="30f8b-103">Use PowerShell tooimport a bacpac file into an Azure SQL database</span></span>

<span data-ttu-id="30f8b-104">A PowerShell-mintaparancsfájl importálja az adatbázist egy **bacpac** fájl az Azure SQL adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="30f8b-104">This PowerShell script example imports a database from a **bacpac** file into an Azure SQL database.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="30f8b-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="30f8b-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=18-19 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="30f8b-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="30f8b-106">Clean up deployment</span></span>

<span data-ttu-id="30f8b-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="30f8b-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="30f8b-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="30f8b-108">Script explanation</span></span>

<span data-ttu-id="30f8b-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="30f8b-109">This script uses hello following commands.</span></span> <span data-ttu-id="30f8b-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="30f8b-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="30f8b-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="30f8b-111">Command</span></span> | <span data-ttu-id="30f8b-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="30f8b-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="30f8b-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="30f8b-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="30f8b-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="30f8b-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="30f8b-115">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="30f8b-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="30f8b-116">Hogy a gazdagépek hello SQL Database logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="30f8b-116">Creates a logical server that hosts hello SQL Database.</span></span> |
| [<span data-ttu-id="30f8b-117">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="30f8b-117">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="30f8b-118">Létrehoz egy tűzfal szabály tooallow hozzáférés tooall SQL-adatbázisok hello megadott IP-címtartomány a hello kiszolgálóján.</span><span class="sxs-lookup"><span data-stu-id="30f8b-118">Creates a firewall rule tooallow access tooall SQL Databases on hello server from hello entered IP address range.</span></span> |
| [<span data-ttu-id="30f8b-119">Új AzureRmSqlDatabaseImport</span><span class="sxs-lookup"><span data-stu-id="30f8b-119">New-AzureRmSqlDatabaseImport</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) | <span data-ttu-id="30f8b-120">Importálja a .bacpac fájlba, és hozzon létre egy új adatbázist hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="30f8b-120">Imports a .bacpac file and create a new database on hello server.</span></span> |
| [<span data-ttu-id="30f8b-121">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="30f8b-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="30f8b-122">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="30f8b-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="30f8b-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="30f8b-123">Next steps</span></span>

<span data-ttu-id="30f8b-124">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="30f8b-124">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="30f8b-125">További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="30f8b-125">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

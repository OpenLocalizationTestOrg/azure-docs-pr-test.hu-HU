---
title: "PowerShell – példa-import-bacpac fájl – az Azure SQL database |} Microsoft Docs"
description: "Az Azure PowerShell-példa parancsfájl SQL-adatbázis bacpac mozaik importálása"
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
ms.openlocfilehash: 20e1d4c195314d6e828e1dac6afae8be11805b42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-import-a-bacpac-file-into-an-azure-sql-database"></a><span data-ttu-id="1ef8e-103">Azure SQL-adatbázis bacpac a fájl importálásához a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1ef8e-103">Use PowerShell to import a bacpac file into an Azure SQL database</span></span>

<span data-ttu-id="1ef8e-104">A PowerShell-mintaparancsfájl importálja az adatbázist egy **bacpac** fájl az Azure SQL adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="1ef8e-104">This PowerShell script example imports a database from a **bacpac** file into an Azure SQL database.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="1ef8e-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="1ef8e-105">Sample script</span></span>

<span data-ttu-id="1ef8e-106">[!code-powershell[fő](../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=18-19 "SQL-adatbázis létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="1ef8e-106">[!code-powershell[main](../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=18-19 "Create SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="1ef8e-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1ef8e-107">Clean up deployment</span></span>

<span data-ttu-id="1ef8e-108">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="1ef8e-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="1ef8e-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="1ef8e-109">Script explanation</span></span>

<span data-ttu-id="1ef8e-110">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="1ef8e-110">This script uses the following commands.</span></span> <span data-ttu-id="1ef8e-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="1ef8e-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="1ef8e-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="1ef8e-112">Command</span></span> | <span data-ttu-id="1ef8e-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1ef8e-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1ef8e-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1ef8e-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="1ef8e-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1ef8e-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1ef8e-116">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="1ef8e-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="1ef8e-117">Az SQL-adatbázist futtató logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1ef8e-117">Creates a logical server that hosts the SQL Database.</span></span> |
| [<span data-ttu-id="1ef8e-118">New-AzureRmSqlServerFirewallRule</span><span class="sxs-lookup"><span data-stu-id="1ef8e-118">New-AzureRmSqlServerFirewallRule</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | <span data-ttu-id="1ef8e-119">Engedélyezi a kiszolgálón lévő összes SQL-adatbázis elérését a megadott IP-címtartomány egy tűzfalszabályt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1ef8e-119">Creates a firewall rule to allow access to all SQL Databases on the server from the entered IP address range.</span></span> |
| [<span data-ttu-id="1ef8e-120">Új AzureRmSqlDatabaseImport</span><span class="sxs-lookup"><span data-stu-id="1ef8e-120">New-AzureRmSqlDatabaseImport</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) | <span data-ttu-id="1ef8e-121">Importálja a .bacpac fájlba, és hozzon létre egy új adatbázist a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="1ef8e-121">Imports a .bacpac file and create a new database on the server.</span></span> |
| [<span data-ttu-id="1ef8e-122">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1ef8e-122">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="1ef8e-123">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="1ef8e-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1ef8e-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ef8e-124">Next steps</span></span>

<span data-ttu-id="1ef8e-125">Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1ef8e-125">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="1ef8e-126">További SQL Database PowerShell parancsfájl minták megtalálhatók a [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="1ef8e-126">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

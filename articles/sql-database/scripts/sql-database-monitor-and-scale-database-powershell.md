---
title: "PowerShell-példa-figyelő-skálázási-egyetlen Azure SQL-adatbázis |} Microsoft Docs"
description: "Az Azure PowerShell-példa parancsfájl figyelését, valamint egy Azure SQL-adatbázis méretezése"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 5d08335f4b1d6c5c6a120cbfb86ab2c62b79bb9f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-monitor-and-scale-a-single-sql-database"></a><span data-ttu-id="2e22c-103">PowerShell segítségével figyelheti, és egy SQL-adatbázis méretezése</span><span class="sxs-lookup"><span data-stu-id="2e22c-103">Use PowerShell to monitor and scale a single SQL database</span></span>

<span data-ttu-id="2e22c-104">A PowerShell-mintaparancsfájl figyeli a metrikák egy adatbázis méretezi a magasabb teljesítményszintre és egy riasztási szabályt hoz létre a Teljesítményelemzési értékek egyike.</span><span class="sxs-lookup"><span data-stu-id="2e22c-104">This PowerShell script example monitors the performance metrics of a database, scales it to a higher performance level, and creates an alert rule on one of the performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="2e22c-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="2e22c-105">Sample script</span></span>

<span data-ttu-id="2e22c-106">[!code-powershell[fő](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "figyelő és a skála egyetlen SQL-adatbázis")]</span><span class="sxs-lookup"><span data-stu-id="2e22c-106">[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "Monitor and scale single SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="2e22c-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="2e22c-107">Clean up deployment</span></span>

<span data-ttu-id="2e22c-108">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="2e22c-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="2e22c-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="2e22c-109">Script explanation</span></span>

<span data-ttu-id="2e22c-110">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="2e22c-110">This script uses the following commands.</span></span> <span data-ttu-id="2e22c-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="2e22c-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="2e22c-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="2e22c-112">Command</span></span> | <span data-ttu-id="2e22c-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="2e22c-113">Notes</span></span> |
|---|---|
 [<span data-ttu-id="2e22c-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2e22c-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="2e22c-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2e22c-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2e22c-116">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="2e22c-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="2e22c-117">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2e22c-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="2e22c-118">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="2e22c-118">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="2e22c-119">Az az adatbázis méretének használati információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2e22c-119">Shows the size usage information for the database.</span></span>|
| [<span data-ttu-id="2e22c-120">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="2e22c-120">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="2e22c-121">Frissíti az adatbázis-tulajdonságai, vagy azokat, kívüli vagy rugalmas készletek között adatbázis helyezi.</span><span class="sxs-lookup"><span data-stu-id="2e22c-121">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="2e22c-122">Adja hozzá AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="2e22c-122">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="2e22c-123">Beállítja az automatikusan figyelése a jövőben a dtu-k riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="2e22c-123">Sets an alert rule to automatically monitor DTUs in the future.</span></span> |
| [<span data-ttu-id="2e22c-124">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2e22c-124">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="2e22c-125">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="2e22c-125">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="2e22c-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e22c-126">Next steps</span></span>

<span data-ttu-id="2e22c-127">Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2e22c-127">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="2e22c-128">További SQL Database PowerShell parancsfájl minták megtalálhatók a [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2e22c-128">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

---
title: "aaaPowerShell példa-figyelő-skálázási-SQL rugalmas készlet-Azure SQL Database |} Microsoft Docs"
description: "Az Azure PowerShell-példa toomonitor parancsfájlt, és a rugalmas SQL-készletet, az Azure SQL-adatbázis méretezése"
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
ms.openlocfilehash: 149a45174ccb8072ea21753364196c7f98fd4101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomonitor-and-scale-a-sql-elastic-pool-in-azure-sql-database"></a><span data-ttu-id="0cade-103">PowerShell toomonitor használja, és a rugalmas SQL-készletet, az Azure SQL-adatbázis méretezése</span><span class="sxs-lookup"><span data-stu-id="0cade-103">Use PowerShell toomonitor and scale a SQL elastic pool in Azure SQL Database</span></span>

<span data-ttu-id="0cade-104">A PowerShell-mintaparancsfájl figyelők hello teljesítménymutatók egy rugalmas készlet tooa magasabb teljesítményszintre méretezi azt, és hello teljesítménymutatók egyik egy riasztási szabályt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0cade-104">This PowerShell script example monitors hello performance metrics of an elastic pool, scales it tooa higher performance level, and creates an alert rule on one of hello performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="0cade-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="0cade-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="0cade-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0cade-106">Clean up deployment</span></span>

<span data-ttu-id="0cade-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="0cade-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="0cade-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="0cade-108">Script explanation</span></span>

<span data-ttu-id="0cade-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="0cade-109">This script uses hello following commands.</span></span> <span data-ttu-id="0cade-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="0cade-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="0cade-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="0cade-111">Command</span></span> | <span data-ttu-id="0cade-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="0cade-112">Notes</span></span> |
|---|---|
 [<span data-ttu-id="0cade-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="0cade-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="0cade-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0cade-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0cade-115">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="0cade-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="0cade-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0cade-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="0cade-117">Új-AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="0cade-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="0cade-118">Létrehoz egy logikai kiszolgáló belül rugalmas készlethez.</span><span class="sxs-lookup"><span data-stu-id="0cade-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="0cade-119">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="0cade-119">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="0cade-120">Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="0cade-120">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="0cade-121">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="0cade-121">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="0cade-122">Hello hello adatbázis mérete használati információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0cade-122">Shows hello size usage information for hello database.</span></span>|
| [<span data-ttu-id="0cade-123">Adja hozzá AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="0cade-123">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="0cade-124">A metrika-alapú riasztási szabály frissítése vagy hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="0cade-124">Adds or updates a metric-based alert rule.</span></span> |
| [<span data-ttu-id="0cade-125">Set-AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="0cade-125">Set-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) | <span data-ttu-id="0cade-126">Frissítés a rugalmas készlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="0cade-126">Updates elastic pool properties</span></span> |
| [<span data-ttu-id="0cade-127">Adja hozzá AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="0cade-127">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="0cade-128">Beállítja egy riasztási szabály tooautomatically figyelő dtu-k hello jövőbeli.</span><span class="sxs-lookup"><span data-stu-id="0cade-128">Sets an alert rule tooautomatically monitor DTUs in hello future.</span></span> |
| [<span data-ttu-id="0cade-129">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="0cade-129">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="0cade-130">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="0cade-130">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="0cade-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0cade-131">Next steps</span></span>

<span data-ttu-id="0cade-132">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0cade-132">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="0cade-133">További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="0cade-133">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

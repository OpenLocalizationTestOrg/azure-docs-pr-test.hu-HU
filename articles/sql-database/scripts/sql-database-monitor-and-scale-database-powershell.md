---
title: "aaaPowerShell példa-figyelő-skálázási-egyetlen Azure SQL adatbázis |} Microsoft Docs"
description: "Az Azure PowerShell-példa parancsfájl toomonitor és a skála egy Azure SQL-adatbázis"
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
ms.openlocfilehash: bd8f880fb47b1360ae4962d2b039faa742de258e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomonitor-and-scale-a-single-sql-database"></a><span data-ttu-id="00480-103">PowerShell toomonitor használja, és egy SQL-adatbázis méretezése</span><span class="sxs-lookup"><span data-stu-id="00480-103">Use PowerShell toomonitor and scale a single SQL database</span></span>

<span data-ttu-id="00480-104">Figyelők hello teljesítménymutatók adatbázis, a PowerShell-mintaparancsfájl méretezi, magasabb teljesítményszintre tooa, és egyik hello teljesítménymutatók egy riasztási szabályt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="00480-104">This PowerShell script example monitors hello performance metrics of a database, scales it tooa higher performance level, and creates an alert rule on one of hello performance metrics.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="00480-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="00480-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="00480-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="00480-106">Clean up deployment</span></span>

<span data-ttu-id="00480-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="00480-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="00480-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="00480-108">Script explanation</span></span>

<span data-ttu-id="00480-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="00480-109">This script uses hello following commands.</span></span> <span data-ttu-id="00480-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="00480-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="00480-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="00480-111">Command</span></span> | <span data-ttu-id="00480-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="00480-112">Notes</span></span> |
|---|---|
 [<span data-ttu-id="00480-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="00480-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="00480-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="00480-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="00480-115">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="00480-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="00480-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="00480-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="00480-117">Get-AzureRmMetric</span><span class="sxs-lookup"><span data-stu-id="00480-117">Get-AzureRmMetric</span></span>](/powershell/module/azurerm.insights/get-azurermmetric) | <span data-ttu-id="00480-118">Hello hello adatbázis mérete használati információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="00480-118">Shows hello size usage information for hello database.</span></span>|
| [<span data-ttu-id="00480-119">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="00480-119">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="00480-120">Frissíti az adatbázis-tulajdonságai, vagy azokat, kívüli vagy rugalmas készletek között adatbázis helyezi.</span><span class="sxs-lookup"><span data-stu-id="00480-120">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="00480-121">Adja hozzá AzureRMMetricAlertRule</span><span class="sxs-lookup"><span data-stu-id="00480-121">Add-AzureRMMetricAlertRule</span></span>](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | <span data-ttu-id="00480-122">Beállítja egy riasztási szabály tooautomatically figyelő dtu-k hello jövőbeli.</span><span class="sxs-lookup"><span data-stu-id="00480-122">Sets an alert rule tooautomatically monitor DTUs in hello future.</span></span> |
| [<span data-ttu-id="00480-123">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="00480-123">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="00480-124">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="00480-124">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="00480-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00480-125">Next steps</span></span>

<span data-ttu-id="00480-126">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="00480-126">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="00480-127">További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="00480-127">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

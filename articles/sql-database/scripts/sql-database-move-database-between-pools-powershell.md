---
title: "aaaPowerShell példa-áthelyezési Azure SQL adatbázis-SQL rugalmas készlet |} Microsoft Docs"
description: "Az Azure PowerShell például parancsfájl-toomove SQL-adatbázis közötti rugalmas készletek PowerShell használatával"
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
ms.openlocfilehash: 501e82ce93a31264d0625fb0243b4e44dcb2d007
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-elastic-pools-and-move-databases-between-elastic-pools"></a><span data-ttu-id="fb4ad-103">PowerShell toocreate rugalmas készletek használatára, majd az adatbázisok áthelyezése rugalmas készletek között</span><span class="sxs-lookup"><span data-stu-id="fb4ad-103">Use PowerShell toocreate elastic pools and move databases between elastic pools</span></span>

<span data-ttu-id="fb4ad-104">A PowerShell-mintaparancsfájl hoz létre két rugalmas készletek és egy adatbázis egy rugalmas készletből áthelyezi egy másik rugalmas készlet, és majd adatbázis kimozdul egy rugalmas készlet tooa egyetlen adatbázis teljesítményszintjét.</span><span class="sxs-lookup"><span data-stu-id="fb4ad-104">This PowerShell script example creates two elastic pools and moves a database from one elastic pool into another elastic pool, and then moves a database out of an elastic pool tooa single database performance level.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="fb4ad-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="fb4ad-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/move-database-between-pools-and-standalone/move-database-between-pools-and-standalone.ps1?highlight=17-18 "Move database between pools")]

## <a name="clean-up-deployment"></a><span data-ttu-id="fb4ad-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="fb4ad-106">Clean up deployment</span></span>

<span data-ttu-id="fb4ad-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="fb4ad-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="fb4ad-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="fb4ad-108">Script explanation</span></span>

<span data-ttu-id="fb4ad-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="fb4ad-109">This script uses hello following commands.</span></span> <span data-ttu-id="fb4ad-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="fb4ad-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="fb4ad-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="fb4ad-111">Command</span></span> | <span data-ttu-id="fb4ad-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fb4ad-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="fb4ad-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="fb4ad-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="fb4ad-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fb4ad-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="fb4ad-115">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="fb4ad-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="fb4ad-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fb4ad-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="fb4ad-117">Új-AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="fb4ad-117">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="fb4ad-118">Létrehoz egy logikai kiszolgáló belül rugalmas készlethez.</span><span class="sxs-lookup"><span data-stu-id="fb4ad-118">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="fb4ad-119">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="fb4ad-119">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="fb4ad-120">Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="fb4ad-120">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="fb4ad-121">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="fb4ad-121">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="fb4ad-122">Frissíti az adatbázis-tulajdonságai, vagy azokat, kívüli vagy rugalmas készletek között adatbázis helyezi.</span><span class="sxs-lookup"><span data-stu-id="fb4ad-122">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="fb4ad-123">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="fb4ad-123">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="fb4ad-124">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="fb4ad-124">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="fb4ad-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb4ad-125">Next steps</span></span>

<span data-ttu-id="fb4ad-126">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fb4ad-126">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="fb4ad-127">További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="fb4ad-127">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

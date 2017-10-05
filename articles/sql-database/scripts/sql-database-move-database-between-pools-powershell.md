---
title: "PowerShell – példa-áthelyezési Azure SQL adatbázis-SQL rugalmas készlet |} Microsoft Docs"
description: "Az Azure PowerShell-példa parancsprogram SQL-adatbázis áthelyezése rugalmas készletek PowerShell használatával"
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
ms.openlocfilehash: 58f14dc668f25f17e93fcaf30f72b15a46d71484
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-create-elastic-pools-and-move-databases-between-elastic-pools"></a><span data-ttu-id="9d5ff-103">PowerShell segítségével hozza létre a rugalmas készletek, és adatbázisok áthelyezése rugalmas készletek között</span><span class="sxs-lookup"><span data-stu-id="9d5ff-103">Use PowerShell to create elastic pools and move databases between elastic pools</span></span>

<span data-ttu-id="9d5ff-104">A PowerShell-mintaparancsfájl hoz létre két rugalmas készletek és egy adatbázis egy rugalmas készletből áthelyezi egy másik rugalmas készlet, és majd kimozdul rugalmas készletek egy önálló adatbázis teljesítményszintjét adatbázis.</span><span class="sxs-lookup"><span data-stu-id="9d5ff-104">This PowerShell script example creates two elastic pools and moves a database from one elastic pool into another elastic pool, and then moves a database out of an elastic pool to a single database performance level.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="9d5ff-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="9d5ff-105">Sample script</span></span>

<span data-ttu-id="9d5ff-106">[!code-powershell[fő](../../../powershell_scripts/sql-database/move-database-between-pools-and-standalone/move-database-between-pools-and-standalone.ps1?highlight=17-18 "adatbázis áthelyezése készletek között")]</span><span class="sxs-lookup"><span data-stu-id="9d5ff-106">[!code-powershell[main](../../../powershell_scripts/sql-database/move-database-between-pools-and-standalone/move-database-between-pools-and-standalone.ps1?highlight=17-18 "Move database between pools")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="9d5ff-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9d5ff-107">Clean up deployment</span></span>

<span data-ttu-id="9d5ff-108">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="9d5ff-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="9d5ff-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="9d5ff-109">Script explanation</span></span>

<span data-ttu-id="9d5ff-110">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="9d5ff-110">This script uses the following commands.</span></span> <span data-ttu-id="9d5ff-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="9d5ff-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="9d5ff-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="9d5ff-112">Command</span></span> | <span data-ttu-id="9d5ff-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9d5ff-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9d5ff-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="9d5ff-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="9d5ff-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9d5ff-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9d5ff-116">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="9d5ff-116">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="9d5ff-117">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9d5ff-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="9d5ff-118">Új-AzureRmSqlElasticPool</span><span class="sxs-lookup"><span data-stu-id="9d5ff-118">New-AzureRmSqlElasticPool</span></span>](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | <span data-ttu-id="9d5ff-119">Létrehoz egy logikai kiszolgáló belül rugalmas készlethez.</span><span class="sxs-lookup"><span data-stu-id="9d5ff-119">Creates an elastic pool within a logical server.</span></span> |
| [<span data-ttu-id="9d5ff-120">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="9d5ff-120">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="9d5ff-121">Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="9d5ff-121">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="9d5ff-122">Set-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="9d5ff-122">Set-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabase) | <span data-ttu-id="9d5ff-123">Frissíti az adatbázis-tulajdonságai, vagy azokat, kívüli vagy rugalmas készletek között adatbázis helyezi.</span><span class="sxs-lookup"><span data-stu-id="9d5ff-123">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="9d5ff-124">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="9d5ff-124">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="9d5ff-125">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="9d5ff-125">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="9d5ff-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d5ff-126">Next steps</span></span>

<span data-ttu-id="9d5ff-127">Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9d5ff-127">For more information on the Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="9d5ff-128">További SQL Database PowerShell parancsfájl minták megtalálhatók a [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9d5ff-128">Additional SQL Database PowerShell script samples can be found in the [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

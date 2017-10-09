---
title: "aaaPowerShell példa naplózás threat detection-Azure SQL Database |} Microsoft Docs"
description: "Azure PowerShell példa parancsfájl tooconfigure naplózás & threat detection Azure SQL adatbázis"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 97e057ac6efe5e730404ae796bc01e7e5c70df35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-sql-database-auditing-and-threat-detection"></a><span data-ttu-id="08973-103">PowerShell tooconfigure SQL-adatbázis naplózásának és a fenyegetések észlelésére használata</span><span class="sxs-lookup"><span data-stu-id="08973-103">Use PowerShell tooconfigure SQL Database auditing and threat detection</span></span>

<span data-ttu-id="08973-104">A PowerShell-mintaparancsfájl konfigurálja az SQL-adatbázis naplózásának és a fenyegetések észlelésére.</span><span class="sxs-lookup"><span data-stu-id="08973-104">This PowerShell script example configures SQL Database auditing and threat detection.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="08973-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="08973-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/database-auditing-and-threat-detection/database-auditing-and-threat-detection.ps1?highlight=13-14 "Configure auditing and threat detection")]

## <a name="clean-up-deployment"></a><span data-ttu-id="08973-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="08973-106">Clean up deployment</span></span>

<span data-ttu-id="08973-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="08973-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="08973-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="08973-108">Script explanation</span></span>

<span data-ttu-id="08973-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="08973-109">This script uses hello following commands.</span></span> <span data-ttu-id="08973-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="08973-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="08973-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="08973-111">Command</span></span> | <span data-ttu-id="08973-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="08973-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="08973-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="08973-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="08973-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="08973-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="08973-115">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="08973-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="08973-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="08973-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="08973-117">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="08973-117">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="08973-118">Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="08973-118">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="08973-119">Új-AzureRmStorageAccount</span><span class="sxs-lookup"><span data-stu-id="08973-119">New-AzureRmStorageAccount</span></span>](/powershell/module/azurerm.storage/new-azurermstorageaccount) | <span data-ttu-id="08973-120">Létrehoz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="08973-120">Creates a Storage account.</span></span> |
| [<span data-ttu-id="08973-121">Set-AzureRmSqlDatabaseAuditingPolicy</span><span class="sxs-lookup"><span data-stu-id="08973-121">Set-AzureRmSqlDatabaseAuditingPolicy</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabaseauditingpolicy) | <span data-ttu-id="08973-122">Beállítja a hello naplózási házirend-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="08973-122">Sets hello auditing policy for a database.</span></span> |
| [<span data-ttu-id="08973-123">Set-AzureRmSqlDatabaseThreatDetectionPolicy</span><span class="sxs-lookup"><span data-stu-id="08973-123">Set-AzureRmSqlDatabaseThreatDetectionPolicy</span></span>](/powershell/module/azurerm.sql/set-azurermsqldatabasethreatdetectionpolicy) | <span data-ttu-id="08973-124">Egy adatbázis egy fenyegetés szabályzat beállítása.</span><span class="sxs-lookup"><span data-stu-id="08973-124">Sets a threat detection policy on a database.</span></span> |
| [<span data-ttu-id="08973-125">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="08973-125">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="08973-126">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="08973-126">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="08973-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="08973-127">Next steps</span></span>

<span data-ttu-id="08973-128">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="08973-128">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="08973-129">További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="08973-129">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

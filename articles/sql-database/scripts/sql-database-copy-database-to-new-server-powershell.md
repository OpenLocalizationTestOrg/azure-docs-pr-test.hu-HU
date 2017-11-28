---
title: "aaaPowerShell példa-másolási-Azure SQL adatbázis új kiszolgáló |} Microsoft Docs"
description: "Az Azure PowerShell például parancsfájl-toocopy SQL adatbázis tooa új kiszolgáló"
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
ms.openlocfilehash: c08f993bd75913481b1d534857ac057263e1d02b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocopy-a-sql-database-tooa-new-server"></a><span data-ttu-id="a2c50-103">PowerShell toocopy SQL adatbázis tooa új kiszolgáló használata</span><span class="sxs-lookup"><span data-stu-id="a2c50-103">Use PowerShell toocopy a SQL database tooa new server</span></span>

<span data-ttu-id="a2c50-104">A PowerShell-mintaparancsfájl egy új kiszolgálót a meglévő adatbázis másolatának létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a2c50-104">This PowerShell script example creates a copy of an existing database in a new server.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="copy-a-database-tooa-new-server"></a><span data-ttu-id="a2c50-105">Adatbázis-tooa új kiszolgálót másolása</span><span class="sxs-lookup"><span data-stu-id="a2c50-105">Copy a database tooa new server</span></span>

[!code-powershell[main](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=18-21 "Copy database toonew server")]

## <a name="clean-up-deployment"></a><span data-ttu-id="a2c50-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="a2c50-106">Clean up deployment</span></span>

<span data-ttu-id="a2c50-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a2c50-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="a2c50-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="a2c50-108">Script explanation</span></span>

<span data-ttu-id="a2c50-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="a2c50-109">This script uses hello following commands.</span></span> <span data-ttu-id="a2c50-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="a2c50-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a2c50-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="a2c50-111">Command</span></span> | <span data-ttu-id="a2c50-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a2c50-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a2c50-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a2c50-113">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="a2c50-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a2c50-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a2c50-115">Új AzureRmSqlServer</span><span class="sxs-lookup"><span data-stu-id="a2c50-115">New-AzureRmSqlServer</span></span>](/powershell/module/azurerm.sql/new-azurermsqlserver) | <span data-ttu-id="a2c50-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a2c50-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="a2c50-117">New-AzureRmSqlDatabase</span><span class="sxs-lookup"><span data-stu-id="a2c50-117">New-AzureRmSqlDatabase</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabase) | <span data-ttu-id="a2c50-118">Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="a2c50-118">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="a2c50-119">Új AzureRmSqlDatabaseCopy</span><span class="sxs-lookup"><span data-stu-id="a2c50-119">New-AzureRmSqlDatabaseCopy</span></span>](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) | <span data-ttu-id="a2c50-120">Egy adatbázis használó hello pillanatkép hello a jelenlegi időpontnál másolatát hozza létre.</span><span class="sxs-lookup"><span data-stu-id="a2c50-120">Creates a copy of a database that uses hello snapshot at hello current time.</span></span> |
| [<span data-ttu-id="a2c50-121">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="a2c50-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="a2c50-122">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="a2c50-122">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="a2c50-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a2c50-123">Next steps</span></span>

<span data-ttu-id="a2c50-124">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a2c50-124">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="a2c50-125">További SQL Database PowerShell parancsfájl minták hello található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a2c50-125">Additional SQL Database PowerShell script samples can be found in hello [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>

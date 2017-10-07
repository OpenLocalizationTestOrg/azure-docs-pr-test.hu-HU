---
title: "PowerShell parancsfájl-hozzon létre egy Azure Cosmos DB tűzfalon aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - tűzfal létrehozása az Azure Cosmos DB rendszerhez"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 00dd2dd847c7ed0e35f5555c2b87b90977f137f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-firewall-using-powershell"></a><span data-ttu-id="7bf6b-103">Azure Cosmos DB: Hozzon létre egy tűzfal PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7bf6b-103">Azure Cosmos DB: Create a firewall using PowerShell</span></span>

<span data-ttu-id="7bf6b-104">A PowerShell-parancsfájlpélda létrehoz egy tűzfal bármilyen Azure Cosmos DB API-fiók.</span><span class="sxs-lookup"><span data-stu-id="7bf6b-104">This sample PowerShell script creates a firewall for any kind of Azure Cosmos DB API account.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="7bf6b-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="7bf6b-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/cosmosdb/create-firewall/create-firewall.ps1?highlight=35-36,39-43 "Create a firewall for Azure Cosmos DB")]

## <a name="clean-up-deployment"></a><span data-ttu-id="7bf6b-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="7bf6b-106">Clean up deployment</span></span>

<span data-ttu-id="7bf6b-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="7bf6b-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="7bf6b-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="7bf6b-108">Script explanation</span></span>

<span data-ttu-id="7bf6b-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="7bf6b-109">This script uses hello following commands.</span></span> <span data-ttu-id="7bf6b-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="7bf6b-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="7bf6b-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="7bf6b-111">Command</span></span> | <span data-ttu-id="7bf6b-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7bf6b-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7bf6b-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="7bf6b-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="7bf6b-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7bf6b-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7bf6b-115">Új AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="7bf6b-115">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="7bf6b-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7bf6b-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="7bf6b-117">Set-AzureRMResource</span><span class="sxs-lookup"><span data-stu-id="7bf6b-117">Set-AzureRMResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="7bf6b-118">Hello adatbázisfiók módosítja.</span><span class="sxs-lookup"><span data-stu-id="7bf6b-118">Modifies hello database account.</span></span> |
| [<span data-ttu-id="7bf6b-119">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="7bf6b-119">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="7bf6b-120">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="7bf6b-120">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="7bf6b-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7bf6b-121">Next steps</span></span>

<span data-ttu-id="7bf6b-122">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="7bf6b-122">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="7bf6b-123">További Azure Cosmos DB PowerShell-parancsfájl példák találhatók hello [Azure Cosmos DB PowerShell-parancsfájlok](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7bf6b-123">Additional Azure Cosmos DB PowerShell script samples can be found in hello [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>

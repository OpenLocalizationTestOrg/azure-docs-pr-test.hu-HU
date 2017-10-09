---
title: "PowerShell parancsfájl-Azure Cosmos DB feladatátvételi házirend létrehozása aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - Azure Cosmos DB feladatátvételi házirend létrehozása"
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
ms.openlocfilehash: 750127385164cd16837b6e29c506d2b44146a629
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-cosmos-db-failover-policy-for-high-availability-using-powershell"></a><span data-ttu-id="91211-103">A PowerShell használatával magas rendelkezésre állású Azure Cosmos DB feladatátvételi házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="91211-103">Create an Azure Cosmos DB failover policy for high availability using PowerShell</span></span>

<span data-ttu-id="91211-104">A PowerShell-parancsfájlpélda a magas rendelkezésre állású egy feladatátvételi házirendhez tartozó Azure Cosmos DB hoz létre.</span><span class="sxs-lookup"><span data-stu-id="91211-104">This sample PowerShell script creates a failover policy for high availability for Azure Cosmos DB.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="91211-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="91211-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/cosmosdb/modify-failover-priority/modify-failover-priority.ps1?highlight=36-39,42-47 "Create an Azure Cosmos DB DocumentDB API account")]

## <a name="clean-up-deployment"></a><span data-ttu-id="91211-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="91211-106">Clean up deployment</span></span>

<span data-ttu-id="91211-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="91211-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="91211-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="91211-108">Script explanation</span></span>

<span data-ttu-id="91211-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="91211-109">This script uses hello following commands.</span></span> <span data-ttu-id="91211-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="91211-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="91211-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="91211-111">Command</span></span> | <span data-ttu-id="91211-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="91211-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="91211-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="91211-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="91211-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="91211-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="91211-115">Új AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="91211-115">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="91211-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="91211-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="91211-117">Invoke-AzureRmResourceAction</span><span class="sxs-lookup"><span data-stu-id="91211-117">Invoke-AzureRmResourceAction</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | <span data-ttu-id="91211-118">Egy műveletet hello Azure CosmosDB fiók hív meg.</span><span class="sxs-lookup"><span data-stu-id="91211-118">Invokes an action on hello Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="91211-119">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="91211-119">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="91211-120">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="91211-120">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="91211-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91211-121">Next steps</span></span>

<span data-ttu-id="91211-122">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="91211-122">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="91211-123">További Azure Cosmos DB PowerShell-parancsfájl példák találhatók hello [Azure Cosmos DB PowerShell-parancsfájlok](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="91211-123">Additional Azure Cosmos DB PowerShell script samples can be found in hello [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>

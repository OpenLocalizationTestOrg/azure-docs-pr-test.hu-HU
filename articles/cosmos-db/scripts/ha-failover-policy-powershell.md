---
title: "Az Azure PowerShell parancsfájl-Azure Cosmos DB feladatátvételi szabályzat létrehozása |} Microsoft Docs"
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
ms.openlocfilehash: 16da3cd543ccbb7fe346261f91d2e9a3ceaf3a8b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-cosmos-db-failover-policy-for-high-availability-using-powershell"></a><span data-ttu-id="4ad6d-103">A PowerShell használatával magas rendelkezésre állású Azure Cosmos DB feladatátvételi házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ad6d-103">Create an Azure Cosmos DB failover policy for high availability using PowerShell</span></span>

<span data-ttu-id="4ad6d-104">A PowerShell-parancsfájlpélda a magas rendelkezésre állású egy feladatátvételi házirendhez tartozó Azure Cosmos DB hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4ad6d-104">This sample PowerShell script creates a failover policy for high availability for Azure Cosmos DB.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="4ad6d-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="4ad6d-105">Sample script</span></span>

<span data-ttu-id="4ad6d-106">[!code-powershell[fő](../../../powershell_scripts/cosmosdb/modify-failover-priority/modify-failover-priority.ps1?highlight=36-39,42-47 "Azure Cosmos DB DocumentDB API-fiók létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="4ad6d-106">[!code-powershell[main](../../../powershell_scripts/cosmosdb/modify-failover-priority/modify-failover-priority.ps1?highlight=36-39,42-47 "Create an Azure Cosmos DB DocumentDB API account")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="4ad6d-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="4ad6d-107">Clean up deployment</span></span>

<span data-ttu-id="4ad6d-108">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="4ad6d-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="4ad6d-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="4ad6d-109">Script explanation</span></span>

<span data-ttu-id="4ad6d-110">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="4ad6d-110">This script uses the following commands.</span></span> <span data-ttu-id="4ad6d-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="4ad6d-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="4ad6d-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="4ad6d-112">Command</span></span> | <span data-ttu-id="4ad6d-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4ad6d-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4ad6d-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4ad6d-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="4ad6d-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4ad6d-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4ad6d-116">Új AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="4ad6d-116">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="4ad6d-117">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4ad6d-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="4ad6d-118">Invoke-AzureRmResourceAction</span><span class="sxs-lookup"><span data-stu-id="4ad6d-118">Invoke-AzureRmResourceAction</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | <span data-ttu-id="4ad6d-119">Meghívja az Azure CosmosDB fiók egy műveletet.</span><span class="sxs-lookup"><span data-stu-id="4ad6d-119">Invokes an action on the Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="4ad6d-120">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4ad6d-120">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="4ad6d-121">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="4ad6d-121">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="4ad6d-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4ad6d-122">Next steps</span></span>

<span data-ttu-id="4ad6d-123">Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="4ad6d-123">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="4ad6d-124">További Azure Cosmos DB PowerShell-parancsfájl példák találhatók a [Azure Cosmos DB PowerShell-parancsfájlok](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4ad6d-124">Additional Azure Cosmos DB PowerShell script samples can be found in the [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>
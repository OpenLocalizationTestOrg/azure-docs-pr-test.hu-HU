---
title: "Az Azure PowerShell parancsfájl-Azure Cosmos DB DocumentDB API-fiók létrehozása |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - Azure Cosmos DB DocumentDB API-fiók létrehozása"
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
ms.openlocfilehash: 9b54236ce3446fe1c6a2a30b31f6d91ad43a92d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-create-a-documentdb-api-account-using-powershell"></a><span data-ttu-id="d3350-103">Az Azure Cosmos DB: PowerShell-lel DocumentDB API-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3350-103">Azure Cosmos DB: Create a DocumentDB API account using PowerShell</span></span>

<span data-ttu-id="d3350-104">A PowerShell-parancsfájlpélda létrehoz egy Azure Cosmos DB API-fiókot.</span><span class="sxs-lookup"><span data-stu-id="d3350-104">This sample PowerShell script creates an Azure Cosmos DB API account.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="d3350-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d3350-105">Sample script</span></span>

<span data-ttu-id="d3350-106">[!code-powershell[fő](../../../powershell_scripts/cosmosdb/create-and-configure-database/create-and-configure-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "Azure Cosmos DB-fiók létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="d3350-106">[!code-powershell[main](../../../powershell_scripts/cosmosdb/create-and-configure-database/create-and-configure-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "Create an Azure Cosmos DB account")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d3350-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d3350-107">Clean up deployment</span></span>

<span data-ttu-id="d3350-108">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d3350-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="d3350-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d3350-109">Script explanation</span></span>

<span data-ttu-id="d3350-110">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="d3350-110">This script uses the following commands.</span></span> <span data-ttu-id="d3350-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="d3350-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d3350-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="d3350-112">Command</span></span> | <span data-ttu-id="d3350-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d3350-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d3350-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d3350-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="d3350-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d3350-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d3350-116">Új AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="d3350-116">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="d3350-117">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d3350-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="d3350-118">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d3350-118">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="d3350-119">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="d3350-119">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="d3350-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3350-120">Next steps</span></span>

<span data-ttu-id="d3350-121">Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="d3350-121">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="d3350-122">További Azure Cosmos DB PowerShell-parancsfájl példák találhatók a [Azure Cosmos DB PowerShell-parancsfájlok](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d3350-122">Additional Azure Cosmos DB PowerShell script samples can be found in the [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>
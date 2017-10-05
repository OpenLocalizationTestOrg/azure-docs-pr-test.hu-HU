---
title: "Az Azure PowerShell parancsfájl-Get Azure Cosmos DB kapcsolati karakterlánc MongoDB-alkalmazásokhoz |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - Get Azure Cosmos DB kapcsolati karakterlánc MongoDB-alkalmazásokhoz"
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
ms.openlocfilehash: e44e35cc7d11db48cd82e470ce8226b3c8cc220a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-an-azure-cosmos-db-connection-string-for-mongodb-apps-using-powershell"></a><span data-ttu-id="2a50b-103">Egy Azure Cosmos DB kapcsolati karakterlánc beolvasása MongoDB alkalmazások PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2a50b-103">Get an Azure Cosmos DB connection string for MongoDB apps using PowerShell</span></span>

<span data-ttu-id="2a50b-104">Ez a minta lekérdezi egy PowerShell-lel MongoDB-alkalmazások Azure Cosmos DB kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2a50b-104">This sample gets an Azure Cosmos DB connection string for MongoDB apps using PowerShell.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="2a50b-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="2a50b-105">Sample script</span></span>

<span data-ttu-id="2a50b-106">[!code-powershell[fő](../../../powershell_scripts/cosmosdb/get-mongodb-connection-string/get-mongodb-connection-string.ps1?highlight=37-41 "Azure Cosmos DB fiókból a MongoDB-kapcsolati karakterlánc beolvasása")]</span><span class="sxs-lookup"><span data-stu-id="2a50b-106">[!code-powershell[main](../../../powershell_scripts/cosmosdb/get-mongodb-connection-string/get-mongodb-connection-string.ps1?highlight=37-41 "Get the MongoDB connection string from an Azure Cosmos DB account")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="2a50b-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="2a50b-107">Clean up deployment</span></span>

<span data-ttu-id="2a50b-108">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="2a50b-108">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="2a50b-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="2a50b-109">Script explanation</span></span>

<span data-ttu-id="2a50b-110">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="2a50b-110">This script uses the following commands.</span></span> <span data-ttu-id="2a50b-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="2a50b-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="2a50b-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="2a50b-112">Command</span></span> | <span data-ttu-id="2a50b-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="2a50b-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2a50b-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2a50b-114">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="2a50b-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2a50b-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2a50b-116">Új AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="2a50b-116">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="2a50b-117">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2a50b-117">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="2a50b-118">Invoke-AzureRmResourceAction</span><span class="sxs-lookup"><span data-stu-id="2a50b-118">Invoke-AzureRmResourceAction</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | <span data-ttu-id="2a50b-119">Meghívja az Azure CosmosDB fiók egy műveletet.</span><span class="sxs-lookup"><span data-stu-id="2a50b-119">Invokes an action on the Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="2a50b-120">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2a50b-120">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="2a50b-121">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="2a50b-121">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="2a50b-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a50b-122">Next steps</span></span>

<span data-ttu-id="2a50b-123">Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="2a50b-123">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="2a50b-124">További Azure Cosmos DB PowerShell-parancsfájl példák találhatók a [Azure Cosmos DB PowerShell-parancsfájlok](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2a50b-124">Additional Azure Cosmos DB PowerShell script samples can be found in the [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>
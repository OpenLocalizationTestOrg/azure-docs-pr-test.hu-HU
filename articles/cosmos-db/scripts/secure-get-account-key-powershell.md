---
title: "PowerShell parancsfájl-Get-fiók aaaAzure kulcsok cosmosdb a(z) |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - Get kulcsait a cosmosdb"
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
ms.openlocfilehash: 9ccee3085dc4fa6507d43e4a220dd5fc32889a9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-account-keys-for-azure-cosmos-db-using-powershell"></a><span data-ttu-id="23973-103">Kulcsait lekérése Azure Cosmos DB PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="23973-103">Get account keys for Azure Cosmos DB using PowerShell</span></span>

<span data-ttu-id="23973-104">Ez a minta Azure Cosmos DB fiók bármilyen kulcsait lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="23973-104">This sample gets account keys for any kind of Azure Cosmos DB account.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a><span data-ttu-id="23973-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="23973-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/cosmosdb/get-account-keys/get-account-keys.ps1?highlight=36-40 "Get hello keys for an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a><span data-ttu-id="23973-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="23973-106">Clean up deployment</span></span>

<span data-ttu-id="23973-107">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="23973-107">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="23973-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="23973-108">Script explanation</span></span>

<span data-ttu-id="23973-109">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="23973-109">This script uses hello following commands.</span></span> <span data-ttu-id="23973-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="23973-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="23973-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="23973-111">Command</span></span> | <span data-ttu-id="23973-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="23973-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="23973-113">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="23973-113">New-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | <span data-ttu-id="23973-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="23973-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="23973-115">Új AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="23973-115">New-AzureRmResource</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | <span data-ttu-id="23973-116">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="23973-116">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="23973-117">Invoke-AzureRmResourceAction</span><span class="sxs-lookup"><span data-stu-id="23973-117">Invoke-AzureRmResourceAction</span></span>](https://docs.microsoft.com/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-3.8.0) | <span data-ttu-id="23973-118">Egy műveletet hello Azure CosmosDB fiók hív meg.</span><span class="sxs-lookup"><span data-stu-id="23973-118">Invokes an action on hello Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="23973-119">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="23973-119">Remove-AzureRmResourceGroup</span></span>](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | <span data-ttu-id="23973-120">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="23973-120">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="23973-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="23973-121">Next steps</span></span>

<span data-ttu-id="23973-122">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/).</span><span class="sxs-lookup"><span data-stu-id="23973-122">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/).</span></span>

<span data-ttu-id="23973-123">További Azure Cosmos DB PowerShell-parancsfájl példák találhatók hello [Azure Cosmos DB PowerShell-parancsfájlok](../powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="23973-123">Additional Azure Cosmos DB PowerShell script samples can be found in hello [Azure Cosmos DB PowerShell scripts](../powershell-samples.md).</span></span>

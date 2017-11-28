---
title: "aaaAzure CLI parancsfájl-hozzon létre egy Azure Cosmos DB DocumentDB API-fiók, adatbázis és gyűjtemény |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – egy Azure Cosmos DB DocumentDB API-fiók, adatbázis és gyűjtemény létrehozása"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/06/2017
ms.author: mimig
ms.openlocfilehash: 53919a849e04fa69680219e51c0289b9f2affe07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-an-documentdb-api-account-using-cli"></a><span data-ttu-id="40426-103">Az Azure Cosmos DB: Parancssori felület használatával DocumentDB API-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="40426-103">Azure Cosmos DB: Create an DocumentDB API account using CLI</span></span>

<span data-ttu-id="40426-104">A parancsfájlpéldát CLI Azure Cosmos DB DocumentDB API fiók, adatbázis és gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="40426-104">This sample CLI script creates an Azure Cosmos DB DocumentDB API account, database, and collection.</span></span>  

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="40426-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="40426-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="40426-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="40426-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="40426-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="40426-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="40426-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="40426-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-account-database/create-cosmosdb-account-database.sh?highlight=15-35 "Create an Azure Cosmos DB DocumentDB API account, database, and collection")]

## <a name="clean-up-deployment"></a><span data-ttu-id="40426-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="40426-109">Clean up deployment</span></span>

<span data-ttu-id="40426-110">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="40426-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="40426-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="40426-111">Script explanation</span></span>

<span data-ttu-id="40426-112">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="40426-112">This script uses hello following commands.</span></span> <span data-ttu-id="40426-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="40426-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="40426-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="40426-114">Command</span></span> | <span data-ttu-id="40426-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="40426-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="40426-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="40426-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="40426-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="40426-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="40426-118">az cosmosdb létrehozása</span><span class="sxs-lookup"><span data-stu-id="40426-118">az cosmosdb create</span></span>](/cli/azure/cosmosdb#create) | <span data-ttu-id="40426-119">Azure Cosmos DB fiókot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="40426-119">Creates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="40426-120">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="40426-120">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="40426-121">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="40426-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="40426-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="40426-122">Next steps</span></span>

<span data-ttu-id="40426-123">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="40426-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="40426-124">További Azure Cosmos DB CLI parancsfájl minták hello található [Azure Cosmos DB CLI dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="40426-124">Additional Azure Cosmos DB CLI script samples can be found in hello [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>

---
title: "Az Azure CLI parancsfájl-Azure Cosmos DB MongoDB API-fiók létrehozása, az adatbázis és a gyűjtemény |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – egy Azure Cosmos DB MongoDB API-fiók, adatbázis és gyűjtemény létrehozása"
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
ms.date: 06/02/2017
ms.author: mimig
ms.openlocfilehash: b0bf637db90cfcb987ad43ed34cb8065d28b0fcf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-create-an-mongodb-api-account-using-the-azure-cli"></a><span data-ttu-id="e31ac-103">Az Azure Cosmos DB: Az Azure parancssori felület használatával MongoDB API-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e31ac-103">Azure Cosmos DB: Create an MongoDB API account using the Azure CLI</span></span>

<span data-ttu-id="e31ac-104">A parancsfájlpéldát CLI Azure Cosmos DB MongoDB API fiók, adatbázis és gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="e31ac-104">This sample CLI script creates an Azure Cosmos DB MongoDB API account, database, and collection.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e31ac-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="e31ac-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="e31ac-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="e31ac-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="e31ac-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e31ac-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="e31ac-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="e31ac-108">Sample script</span></span>

<span data-ttu-id="e31ac-109">[!code-azurecli-interactive[fő](../../../cli_scripts/cosmosdb/create-cosmosdb-mongodb-account/create-cosmosdb-mongodb-account.sh?highlight=15-35 "egy Azure Cosmos DB MongoDB API-fiók, adatbázis és gyűjtemény létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="e31ac-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-mongodb-account/create-cosmosdb-mongodb-account.sh?highlight=15-35 "Create an Azure Cosmos DB MongoDB API account, database, and collection")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="e31ac-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="e31ac-110">Clean up deployment</span></span>

<span data-ttu-id="e31ac-111">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="e31ac-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="e31ac-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="e31ac-112">Script explanation</span></span>

<span data-ttu-id="e31ac-113">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="e31ac-113">This script uses the following commands.</span></span> <span data-ttu-id="e31ac-114">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="e31ac-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="e31ac-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="e31ac-115">Command</span></span> | <span data-ttu-id="e31ac-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e31ac-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e31ac-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="e31ac-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="e31ac-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e31ac-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e31ac-119">az cosmosdb létrehozása</span><span class="sxs-lookup"><span data-stu-id="e31ac-119">az cosmosdb create</span></span>](/cli/azure/cosmosdb#create) | <span data-ttu-id="e31ac-120">Azure Cosmos DB fiókot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e31ac-120">Creates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="e31ac-121">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="e31ac-121">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="e31ac-122">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="e31ac-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e31ac-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e31ac-123">Next steps</span></span>

<span data-ttu-id="e31ac-124">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e31ac-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e31ac-125">További Azure Cosmos DB CLI parancsfájl minták megtalálhatók a [Azure Cosmos DB CLI dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e31ac-125">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>

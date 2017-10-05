---
title: "Az Azure CLI parancsfájl-létrehozása az Azure Cosmos DB tűzfal |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - tűzfal létrehozása az Azure Cosmos DB rendszerhez"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: sammvcple
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: mimig
ms.openlocfilehash: 51f61901e1b1615e48582690dea701a01a56ebca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-create-a-firewall-using-the-azure-cli"></a><span data-ttu-id="a83d9-103">Az Azure Cosmos DB: Hozzon létre egy tűzfal, az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="a83d9-103">Azure Cosmos DB: Create a firewall using the Azure CLI</span></span>

<span data-ttu-id="a83d9-104">Ez a parancsfájlpélda CLI Azure Cosmos DB fiók bármilyen egy tűzfal-házirendet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a83d9-104">This sample CLI script creates a firewall policy for any kind of Azure Cosmos DB account.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a83d9-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="a83d9-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a83d9-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="a83d9-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="a83d9-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a83d9-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="a83d9-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="a83d9-108">Sample script</span></span>

<span data-ttu-id="a83d9-109">[!code-azurecli-interactive[fő](../../../cli_scripts/cosmosdb/secure-cosmosdb-create-firewall/secure-cosmosdb-create-firewall.sh?highlight=38-42 "hozzon létre egy Azure Cosmos DB tűzfal")]</span><span class="sxs-lookup"><span data-stu-id="a83d9-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-create-firewall/secure-cosmosdb-create-firewall.sh?highlight=38-42 "Create an Azure Cosmos DB firewall")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="a83d9-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="a83d9-110">Clean up deployment</span></span>

<span data-ttu-id="a83d9-111">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a83d9-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="a83d9-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="a83d9-112">Script explanation</span></span>

<span data-ttu-id="a83d9-113">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="a83d9-113">This script uses the following commands.</span></span> <span data-ttu-id="a83d9-114">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="a83d9-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="a83d9-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="a83d9-115">Command</span></span> | <span data-ttu-id="a83d9-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a83d9-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a83d9-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="a83d9-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="a83d9-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a83d9-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a83d9-119">az cosmosdb létrehozása</span><span class="sxs-lookup"><span data-stu-id="a83d9-119">az cosmosdb create</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#create) | <span data-ttu-id="a83d9-120">Azure CosmosDB fiókot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a83d9-120">Creates an Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="a83d9-121">az cosmosdb frissítés</span><span class="sxs-lookup"><span data-stu-id="a83d9-121">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="a83d9-122">Frissíti a tűzfal beállításait adja meg Azure CosmosDB fiókot.</span><span class="sxs-lookup"><span data-stu-id="a83d9-122">Updates an Azure CosmosDB account to include firewall settings.</span></span> |
| [<span data-ttu-id="a83d9-123">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="a83d9-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="a83d9-124">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="a83d9-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a83d9-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a83d9-125">Next steps</span></span>

<span data-ttu-id="a83d9-126">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a83d9-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a83d9-127">További Azure Cosmos DB CLI parancsfájl minták megtalálhatók a [Azure Cosmos DB CLI dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a83d9-127">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>

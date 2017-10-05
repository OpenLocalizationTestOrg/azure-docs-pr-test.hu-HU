---
title: "Az Azure CLI parancsfájl méretű Azure Cosmos DB tároló átviteli |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - skálázási Azure Cosmos DB contianer átviteli sebesség"
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
ms.openlocfilehash: f08733cd4074c7144b20a0592522423e729e6f1d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="scale-azure-cosmos-db-container-throughput-using-the-azure-cli"></a><span data-ttu-id="82ea9-103">Scale Azure Cosmos DB tároló átviteli az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="82ea9-103">Scale Azure Cosmos DB container throughput using the Azure CLI</span></span>

<span data-ttu-id="82ea9-104">Ez a minta arányosan tároló átviteli bármilyen Azure Cosmos DB tároló.</span><span class="sxs-lookup"><span data-stu-id="82ea9-104">This sample scales container throughput for any kind of Azure Cosmos DB container.</span></span>  

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="82ea9-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="82ea9-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="82ea9-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="82ea9-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="82ea9-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="82ea9-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="82ea9-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="82ea9-108">Sample script</span></span>

<span data-ttu-id="82ea9-109">[!code-azurecli-interactive[fő](../../../cli_scripts/cosmosdb/scale-cosmosdb-throughput/scale-cosmosdb-throughput.sh?highlight=40-46 "Scale Azure Cosmos DB átviteli sebesség")]</span><span class="sxs-lookup"><span data-stu-id="82ea9-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/scale-cosmosdb-throughput/scale-cosmosdb-throughput.sh?highlight=40-46 "Scale Azure Cosmos DB throughput")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="82ea9-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="82ea9-110">Clean up deployment</span></span>

<span data-ttu-id="82ea9-111">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="82ea9-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="82ea9-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="82ea9-112">Script explanation</span></span>

<span data-ttu-id="82ea9-113">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="82ea9-113">This script uses the following commands.</span></span> <span data-ttu-id="82ea9-114">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="82ea9-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="82ea9-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="82ea9-115">Command</span></span> | <span data-ttu-id="82ea9-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="82ea9-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="82ea9-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="82ea9-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="82ea9-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="82ea9-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="82ea9-119">az cosmosdb frissítés</span><span class="sxs-lookup"><span data-stu-id="82ea9-119">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="82ea9-120">Egy Azure Cosmos DB fiók frissíti.</span><span class="sxs-lookup"><span data-stu-id="82ea9-120">Updates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="82ea9-121">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="82ea9-121">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="82ea9-122">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="82ea9-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="82ea9-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82ea9-123">Next steps</span></span>

<span data-ttu-id="82ea9-124">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="82ea9-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="82ea9-125">További Azure Cosmos DB CLI parancsfájl minták megtalálhatók a [Azure Cosmos DB CLI dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="82ea9-125">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>

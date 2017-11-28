---
title: "parancssori felület parancsfájl-házirendet hozhat létre feladatátvevő magas rendelkezésre állású aaaAzure |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - magas rendelkezésre állású feladatátvétel házirend létrehozása"
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
ms.openlocfilehash: 9076f4ef23fceb4208c934c57ac6899f0b58ffd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-failover-policy-for-high-availability-using-hello-azure-cli"></a><span data-ttu-id="d998b-103">A magas rendelkezésre állású hello Azure parancssori felület használatával feladatátvételi házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="d998b-103">Create a failover policy for high availability using hello Azure CLI</span></span>

<span data-ttu-id="d998b-104">Ez a parancsfájlpélda CLI Azure Cosmos DB fiókot hoz létre, és ezután konfigurálja a magas rendelkezésre állás érdekében.</span><span class="sxs-lookup"><span data-stu-id="d998b-104">This sample CLI script creates an Azure Cosmos DB account, and then configures it for high availability.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d998b-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d998b-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d998b-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="d998b-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="d998b-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d998b-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="d998b-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d998b-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/high-availability-cosmosdb-configure-failover/high-availability-cosmosdb-configure-failover.sh?highlight=23-27 "Create an Azure Cosmos DB failover policy")]

## <a name="clean-up-deployment"></a><span data-ttu-id="d998b-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d998b-109">Clean up deployment</span></span>

<span data-ttu-id="d998b-110">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d998b-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d998b-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d998b-111">Script explanation</span></span>

<span data-ttu-id="d998b-112">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="d998b-112">This script uses hello following commands.</span></span> <span data-ttu-id="d998b-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="d998b-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d998b-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="d998b-114">Command</span></span> | <span data-ttu-id="d998b-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d998b-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d998b-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="d998b-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="d998b-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d998b-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d998b-118">az cosmosdb létrehozása</span><span class="sxs-lookup"><span data-stu-id="d998b-118">az cosmosdb create</span></span>](/cli/azure/sql/server#create) | <span data-ttu-id="d998b-119">Azure Cosmos DB fiókot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d998b-119">Creates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="d998b-120">az cosmosdb frissítés</span><span class="sxs-lookup"><span data-stu-id="d998b-120">az cosmosdb update</span></span>](/cli/azure/cosmosdb#update) | <span data-ttu-id="d998b-121">Azure Cosmos DB fiók frissíti.</span><span class="sxs-lookup"><span data-stu-id="d998b-121">Updates Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="d998b-122">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="d998b-122">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="d998b-123">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="d998b-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d998b-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d998b-124">Next steps</span></span>

<span data-ttu-id="d998b-125">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d998b-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d998b-126">További Azure Cosmos DB CLI parancsfájl minták hello található [Azure Cosmos DB CLI dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d998b-126">Additional Azure Cosmos DB CLI script samples can be found in hello [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>

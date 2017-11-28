---
title: "parancssori felület parancsfájl-létrehozása az Azure Cosmos DB tűzfal aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: d4bee4f37906033c96826b9662d2ba396325c792
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-firewall-using-hello-azure-cli"></a><span data-ttu-id="39b7a-103">Az Azure Cosmos DB: Hozzon létre egy tűzfal hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="39b7a-103">Azure Cosmos DB: Create a firewall using hello Azure CLI</span></span>

<span data-ttu-id="39b7a-104">Ez a parancsfájlpélda CLI Azure Cosmos DB fiók bármilyen egy tűzfal-házirendet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="39b7a-104">This sample CLI script creates a firewall policy for any kind of Azure Cosmos DB account.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="39b7a-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="39b7a-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="39b7a-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="39b7a-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="39b7a-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="39b7a-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="39b7a-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="39b7a-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-create-firewall/secure-cosmosdb-create-firewall.sh?highlight=38-42 "Create an Azure Cosmos DB firewall")]

## <a name="clean-up-deployment"></a><span data-ttu-id="39b7a-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="39b7a-109">Clean up deployment</span></span>

<span data-ttu-id="39b7a-110">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="39b7a-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="39b7a-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="39b7a-111">Script explanation</span></span>

<span data-ttu-id="39b7a-112">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="39b7a-112">This script uses hello following commands.</span></span> <span data-ttu-id="39b7a-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="39b7a-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="39b7a-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="39b7a-114">Command</span></span> | <span data-ttu-id="39b7a-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="39b7a-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="39b7a-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="39b7a-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="39b7a-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="39b7a-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="39b7a-118">az cosmosdb létrehozása</span><span class="sxs-lookup"><span data-stu-id="39b7a-118">az cosmosdb create</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#create) | <span data-ttu-id="39b7a-119">Azure CosmosDB fiókot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="39b7a-119">Creates an Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="39b7a-120">az cosmosdb frissítés</span><span class="sxs-lookup"><span data-stu-id="39b7a-120">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="39b7a-121">Egy Azure CosmosDB fiók tooinclude tűzfal beállításait a frissítések.</span><span class="sxs-lookup"><span data-stu-id="39b7a-121">Updates an Azure CosmosDB account tooinclude firewall settings.</span></span> |
| [<span data-ttu-id="39b7a-122">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="39b7a-122">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="39b7a-123">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="39b7a-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="39b7a-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="39b7a-124">Next steps</span></span>

<span data-ttu-id="39b7a-125">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="39b7a-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="39b7a-126">További Azure Cosmos DB CLI parancsfájl minták hello található [Azure Cosmos DB CLI dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="39b7a-126">Additional Azure Cosmos DB CLI script samples can be found in hello [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>

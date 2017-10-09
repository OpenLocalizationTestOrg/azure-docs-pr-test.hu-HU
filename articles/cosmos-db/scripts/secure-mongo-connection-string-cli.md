---
title: "aaaAzure CLI parancsfájl-Get Azure Cosmos DB kapcsolati karakterlánc MongoDB-alkalmazásokhoz |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - Get Azure Cosmos DB kapcsolati karakterlánc MongoDB-alkalmazásokhoz"
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
ms.openlocfilehash: 9b0a4bf020039c9bf9554b4199a279622c15a15d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-an-azure-cosmos-db-connection-string-for-mongodb-apps-using-hello-azure-cli"></a><span data-ttu-id="9116e-103">Egy Azure Cosmos DB kapcsolati karakterlánc beolvasása MongoDB alkalmazások hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="9116e-103">Get an Azure Cosmos DB connection string for MongoDB apps using hello Azure CLI</span></span>

<span data-ttu-id="9116e-104">Ez a minta lekérdezi egy Azure Cosmos DB kapcsolati karakterlánc hello Azure CLI-t használó MongoDB-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="9116e-104">This sample gets an Azure Cosmos DB connection string for MongoDB apps using hello Azure CLI.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="9116e-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="9116e-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="9116e-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="9116e-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="9116e-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9116e-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="9116e-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="9116e-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-get-mongodb-connection-string/secure-cosmosdb-get-mongodb-connection-string.sh?highlight=36-39 "Get Azure Cosmos DB connection string for MongoDB apps")]

## <a name="clean-up-deployment"></a><span data-ttu-id="9116e-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9116e-109">Clean up deployment</span></span>

<span data-ttu-id="9116e-110">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="9116e-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="9116e-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="9116e-111">Script explanation</span></span>

<span data-ttu-id="9116e-112">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="9116e-112">This script uses hello following commands.</span></span> <span data-ttu-id="9116e-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="9116e-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9116e-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="9116e-114">Command</span></span> | <span data-ttu-id="9116e-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9116e-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9116e-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="9116e-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="9116e-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9116e-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9116e-118">az cosmosdb frissítés</span><span class="sxs-lookup"><span data-stu-id="9116e-118">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="9116e-119">Egy Azure Cosmos DB fiók frissíti.</span><span class="sxs-lookup"><span data-stu-id="9116e-119">Updates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="9116e-120">az cosmosdb lista-kapcsolat-karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9116e-120">az cosmosdb list-connection-strings</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#list-connection-strings) | <span data-ttu-id="9116e-121">Hello fiók hello kapcsolati karakterlánc beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9116e-121">Gets hello connection string for hello account.</span></span>|
| [<span data-ttu-id="9116e-122">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="9116e-122">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="9116e-123">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="9116e-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9116e-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9116e-124">Next steps</span></span>

<span data-ttu-id="9116e-125">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9116e-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="9116e-126">További Azure Cosmos DB CLI parancsfájl minták hello található [Azure Cosmos DB CLI dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9116e-126">Additional Azure Cosmos DB CLI script samples can be found in hello [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>

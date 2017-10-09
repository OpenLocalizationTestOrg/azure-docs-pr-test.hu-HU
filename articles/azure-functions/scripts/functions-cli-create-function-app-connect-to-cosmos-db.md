---
title: "egy Azure-függvény, amely a tooan Azure Cosmos DB aaaCreate |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - tooan Azure Cosmos DB kapcsolódó Azure-függvény létrehozása"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: 0fbc1ebec2dfd480e0cf3ca64f9febcec8af9a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-that-connects-tooan-azure-cosmos-db"></a><span data-ttu-id="f3b1d-103">Egy Azure-függvény, amely a tooan Azure Cosmos DB létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3b1d-103">Create an Azure Function that connects tooan Azure Cosmos DB</span></span>

<span data-ttu-id="f3b1d-104">Ez a parancsfájlpélda hoz létre egy Azure-függvény alkalmazást, és tooan Azure Cosmos DB adatbázishoz kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="f3b1d-104">This sample script creates an Azure Function App and connects tooan Azure Cosmos DB database.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f3b1d-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="f3b1d-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="f3b1d-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="f3b1d-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="f3b1d-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f3b1d-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="f3b1d-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="f3b1d-108">Sample script</span></span>

<span data-ttu-id="f3b1d-109">Ez a minta létrehoz egy Azure-függvény alkalmazást, és hozzáadja egy Cosmos DB végpont és a hozzáférési kulcs tooapp beállításait.</span><span class="sxs-lookup"><span data-stu-id="f3b1d-109">This sample creates an Azure Function app and adds a Cosmos DB endpoint and access key tooapp settings.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects tooan Azure Cosmos DB")]

## <a name="clean-up-deployment"></a><span data-ttu-id="f3b1d-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f3b1d-110">Clean up deployment</span></span>

<span data-ttu-id="f3b1d-111">Hello parancsfájl minta futtatása után hello kövesse parancs lehet használt tooremove hello erőforráscsoportban, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="f3b1d-111">After hello script sample has been run, hello follow command can be used tooremove hello resource group and all related resources.</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="f3b1d-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="f3b1d-112">Script explanation</span></span>

<span data-ttu-id="f3b1d-113">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="f3b1d-113">This script uses hello following commands.</span></span> <span data-ttu-id="f3b1d-114">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="f3b1d-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f3b1d-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="f3b1d-115">Command</span></span> | <span data-ttu-id="f3b1d-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f3b1d-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f3b1d-117">az bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f3b1d-117">az login</span></span>](https://docs.microsoft.com/cli/azure/#login) | <span data-ttu-id="f3b1d-118">Bejelentkezési tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f3b1d-118">Login tooAzure.</span></span> |
| [<span data-ttu-id="f3b1d-119">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3b1d-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f3b1d-120">Hozzon létre egy erőforráscsoportot, amelynek a helye</span><span class="sxs-lookup"><span data-stu-id="f3b1d-120">Create a resource group with location</span></span> |
| [<span data-ttu-id="f3b1d-121">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3b1d-121">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account) | <span data-ttu-id="f3b1d-122">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="f3b1d-122">Create a storage account</span></span> |
| [<span data-ttu-id="f3b1d-123">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3b1d-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="f3b1d-124">Hozzon létre egy új funkció alkalmazást</span><span class="sxs-lookup"><span data-stu-id="f3b1d-124">Create a new function app</span></span> |
| [<span data-ttu-id="f3b1d-125">az documentdb létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3b1d-125">az documentdb create</span></span>](https://docs.microsoft.com/cli/azure/documentdb#create) | <span data-ttu-id="f3b1d-126">A documentdb-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3b1d-126">Create documentdb database</span></span> |
| [<span data-ttu-id="f3b1d-127">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="f3b1d-127">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="f3b1d-128">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f3b1d-128">Clean up</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f3b1d-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3b1d-129">Next steps</span></span>

<span data-ttu-id="f3b1d-130">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f3b1d-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f3b1d-131">További Azure Functions CLI parancsfájl minták hello található [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f3b1d-131">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>





---
title: "Egy Azure Cosmos DB kapcsolódó Azure-függvény létrehozása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták –, amely egy Azure Cosmos DB csatlakozik Azure-függvény létrehozása"
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
ms.openlocfilehash: ba7e934f71824493f29b001cea6dd1c567ef3414
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a><span data-ttu-id="cc122-103">Hozzon létre egy Azure-függvény, amely egy Azure Cosmos DB csatlakozik</span><span class="sxs-lookup"><span data-stu-id="cc122-103">Create an Azure Function that connects to an Azure Cosmos DB</span></span>

<span data-ttu-id="cc122-104">Ez a parancsfájlpélda hoz létre egy Azure-függvény alkalmazást, és egy Azure Cosmos DB adatbázishoz kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="cc122-104">This sample script creates an Azure Function App and connects to an Azure Cosmos DB database.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="cc122-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="cc122-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="cc122-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="cc122-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="cc122-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="cc122-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="cc122-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="cc122-108">Sample script</span></span>

<span data-ttu-id="cc122-109">Ez a minta egy Azure-függvény alkalmazást hoz létre, és Alkalmazásbeállítások ad hozzá egy Cosmos DB végpont és a hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="cc122-109">This sample creates an Azure Function app and adds a Cosmos DB endpoint and access key to app settings.</span></span>

<span data-ttu-id="cc122-110">[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh ", amely egy Azure Cosmos DB csatlakozik Azure-függvény létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="cc122-110">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="cc122-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="cc122-111">Clean up deployment</span></span>

<span data-ttu-id="cc122-112">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cc122-112">After the script sample has been run, the follow command can be used to remove the resource group and all related resources.</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="cc122-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="cc122-113">Script explanation</span></span>

<span data-ttu-id="cc122-114">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="cc122-114">This script uses the following commands.</span></span> <span data-ttu-id="cc122-115">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="cc122-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="cc122-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="cc122-116">Command</span></span> | <span data-ttu-id="cc122-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cc122-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cc122-118">az bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cc122-118">az login</span></span>](https://docs.microsoft.com/cli/azure/#login) | <span data-ttu-id="cc122-119">Bejelentkezés az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="cc122-119">Login to Azure.</span></span> |
| [<span data-ttu-id="cc122-120">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc122-120">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="cc122-121">Hozzon létre egy erőforráscsoportot, amelynek a helye</span><span class="sxs-lookup"><span data-stu-id="cc122-121">Create a resource group with location</span></span> |
| [<span data-ttu-id="cc122-122">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc122-122">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account) | <span data-ttu-id="cc122-123">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="cc122-123">Create a storage account</span></span> |
| [<span data-ttu-id="cc122-124">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc122-124">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="cc122-125">Hozzon létre egy új funkció alkalmazást</span><span class="sxs-lookup"><span data-stu-id="cc122-125">Create a new function app</span></span> |
| [<span data-ttu-id="cc122-126">az documentdb létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc122-126">az documentdb create</span></span>](https://docs.microsoft.com/cli/azure/documentdb#create) | <span data-ttu-id="cc122-127">A documentdb-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc122-127">Create documentdb database</span></span> |
| [<span data-ttu-id="cc122-128">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="cc122-128">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="cc122-129">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="cc122-129">Clean up</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cc122-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc122-130">Next steps</span></span>

<span data-ttu-id="cc122-131">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cc122-131">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="cc122-132">További Azure Functions CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="cc122-132">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>





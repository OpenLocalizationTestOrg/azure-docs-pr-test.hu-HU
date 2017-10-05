---
title: "Egy Azure Storage kapcsolódó Azure-függvény létrehozása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták –, amely kapcsolódik az Azure Storage Azure-függvény létrehozása"
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
ms.openlocfilehash: 36dbc2c181c9991a27163e3194800f63c6c0e01e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a><span data-ttu-id="c62f8-103">Függvény alkalmazás integrálja az Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="c62f8-103">Integrate Function App into Azure Storage Account</span></span>

<span data-ttu-id="c62f8-104">Ez a parancsfájlpélda hoz létre, a függvény alkalmazás és a Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="c62f8-104">This sample script creates a Function App and Storage Account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c62f8-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="c62f8-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c62f8-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="c62f8-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="c62f8-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c62f8-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="c62f8-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="c62f8-108">Sample script</span></span>

<span data-ttu-id="c62f8-109">Ez a minta egy Azure-függvény alkalmazást hoz létre, és hozzáadja a tárolási kapcsolati karakterlánc Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="c62f8-109">This sample creates an Azure Function app and adds the storage connection string to an app setting.</span></span>

<span data-ttu-id="c62f8-110">[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "függvény alkalmazás integrálja az Azure Storage-fiók")]</span><span class="sxs-lookup"><span data-stu-id="c62f8-110">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]</span></span>


## <a name="clean-up-deployment"></a><span data-ttu-id="c62f8-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c62f8-111">Clean up deployment</span></span>

<span data-ttu-id="c62f8-112">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, az App Service-alkalmazást, és minden kapcsolódó erőforrások:</span><span class="sxs-lookup"><span data-stu-id="c62f8-112">After the script sample has been run, the following command can be used to remove the resource group, App Service app, and all related resources:</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="c62f8-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="c62f8-113">Script explanation</span></span>

<span data-ttu-id="c62f8-114">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="c62f8-114">This script uses the following commands.</span></span> <span data-ttu-id="c62f8-115">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="c62f8-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c62f8-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="c62f8-116">Command</span></span> | <span data-ttu-id="c62f8-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c62f8-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c62f8-118">az bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c62f8-118">az login</span></span>](https://docs.microsoft.com/cli/azure/#login) | <span data-ttu-id="c62f8-119">Bejelentkezés az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="c62f8-119">Login to Azure.</span></span> |
| [<span data-ttu-id="c62f8-120">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="c62f8-120">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c62f8-121">Hozzon létre egy erőforráscsoportot, amelynek a helye</span><span class="sxs-lookup"><span data-stu-id="c62f8-121">Create a resource group with location</span></span> |
| [<span data-ttu-id="c62f8-122">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c62f8-122">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account) | <span data-ttu-id="c62f8-123">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="c62f8-123">Create a storage account</span></span> |
| [<span data-ttu-id="c62f8-124">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="c62f8-124">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="c62f8-125">Hozzon létre egy új funkció alkalmazást</span><span class="sxs-lookup"><span data-stu-id="c62f8-125">Create a new function app</span></span> |
| [<span data-ttu-id="c62f8-126">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="c62f8-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="c62f8-127">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c62f8-127">Clean up</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c62f8-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c62f8-128">Next steps</span></span>

<span data-ttu-id="c62f8-129">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c62f8-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c62f8-130">További Azure Functions CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c62f8-130">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>

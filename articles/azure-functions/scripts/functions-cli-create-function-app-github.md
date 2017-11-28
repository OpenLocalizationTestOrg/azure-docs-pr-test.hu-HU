---
title: "Függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: d67e85f91c80efe464fceb1105243bedfba83a0f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a><span data-ttu-id="b4fae-103">Függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot</span><span class="sxs-lookup"><span data-stu-id="b4fae-103">Create a function app and deploy function code from GitHub</span></span>

<span data-ttu-id="b4fae-104">Ez a parancsfájlpélda hoz létre, a függvény alkalmazás használatával az [fogyasztás terv](../functions-scale.md#consumption-plan) kapcsolódó erőforrásokkal, és telepíti a funkciókódot a nyilvános GitHub-adattár (folyamatos üzembe helyezés) nélkül.</span><span class="sxs-lookup"><span data-stu-id="b4fae-104">This sample script creates a function app using the [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and deploys your function code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="b4fae-105">A Githubból funkciókódot folyamatos kézbesítését, olvassa el a [függvény-alkalmazás létrehozása, és folyamatosan telepítése a Githubból](functions-cli-create-function-app-github-continuous.md)</span><span class="sxs-lookup"><span data-stu-id="b4fae-105">For continuous delivery of function code from GitHub, read [Create a function app and continuously deploy from GitHub](functions-cli-create-function-app-github-continuous.md)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b4fae-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="b4fae-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="b4fae-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="b4fae-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="b4fae-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b4fae-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="b4fae-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="b4fae-109">Sample script</span></span>

<span data-ttu-id="b4fae-110">Ez a minta egy Azure-függvény alkalmazás létrehozza, és telepíti a Githubról funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="b4fae-110">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

<span data-ttu-id="b4fae-111">[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "a Githubról telepítés függvény-alkalmazás létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="b4fae-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="b4fae-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="b4fae-112">Script explanation</span></span>

<span data-ttu-id="b4fae-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="b4fae-113">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="b4fae-114">Ezt a parancsfájlt az alábbi parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="b4fae-114">This script uses the following commands:</span></span>

| <span data-ttu-id="b4fae-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="b4fae-115">Command</span></span> | <span data-ttu-id="b4fae-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b4fae-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b4fae-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4fae-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="b4fae-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b4fae-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b4fae-119">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4fae-119">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="b4fae-120">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b4fae-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="b4fae-121">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4fae-121">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) | <span data-ttu-id="b4fae-122">Létrehoz egy Azure függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b4fae-122">Creates an Azure Function app.</span></span> |
| [<span data-ttu-id="b4fae-123">az App Service web verziókezelő konfiguráció</span><span class="sxs-lookup"><span data-stu-id="b4fae-123">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="b4fae-124">Egy függvény app társítja a Git vagy Mercurial tárházba.</span><span class="sxs-lookup"><span data-stu-id="b4fae-124">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b4fae-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4fae-125">Next steps</span></span>

<span data-ttu-id="b4fae-126">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b4fae-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b4fae-127">További Azure Functions CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b4fae-127">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>

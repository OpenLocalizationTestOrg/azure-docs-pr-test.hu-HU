---
title: "egy függvény App aaaCreate és központi telepítése a Githubból funkciókódot |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 026886f11909149db695d9a52d0aa37f109f64e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a><span data-ttu-id="4bd15-103">Függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot</span><span class="sxs-lookup"><span data-stu-id="4bd15-103">Create a function app and deploy function code from GitHub</span></span>

<span data-ttu-id="4bd15-104">Ez a parancsfájlpélda hoz létre egy függvény app hello segítségével [fogyasztás terv](../functions-scale.md#consumption-plan) kapcsolódó erőforrásokkal, és telepíti a funkciókódot a nyilvános GitHub-adattár (folyamatos üzembe helyezés) nélkül.</span><span class="sxs-lookup"><span data-stu-id="4bd15-104">This sample script creates a function app using hello [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and deploys your function code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="4bd15-105">A Githubból funkciókódot folyamatos kézbesítését, olvassa el a [függvény-alkalmazás létrehozása, és folyamatosan telepítése a Githubból](functions-cli-create-function-app-github-continuous.md)</span><span class="sxs-lookup"><span data-stu-id="4bd15-105">For continuous delivery of function code from GitHub, read [Create a function app and continuously deploy from GitHub](functions-cli-create-function-app-github-continuous.md)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4bd15-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="4bd15-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="4bd15-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="4bd15-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="4bd15-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4bd15-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="4bd15-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="4bd15-109">Sample script</span></span>

<span data-ttu-id="4bd15-110">Ez a minta egy Azure-függvény alkalmazás létrehozza, és telepíti a Githubról funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="4bd15-110">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="4bd15-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="4bd15-111">Script explanation</span></span>

<span data-ttu-id="4bd15-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="4bd15-112">Each command in hello table links toocommand specific documentation.</span></span> <span data-ttu-id="4bd15-113">A parancsfájl a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="4bd15-113">This script uses hello following commands:</span></span>

| <span data-ttu-id="4bd15-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="4bd15-114">Command</span></span> | <span data-ttu-id="4bd15-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4bd15-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4bd15-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="4bd15-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4bd15-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4bd15-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4bd15-118">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="4bd15-118">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="4bd15-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4bd15-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="4bd15-120">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="4bd15-120">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) | <span data-ttu-id="4bd15-121">Létrehoz egy Azure függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4bd15-121">Creates an Azure Function app.</span></span> |
| [<span data-ttu-id="4bd15-122">az App Service web verziókezelő konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4bd15-122">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="4bd15-123">Egy függvény app társítja a Git vagy Mercurial tárházba.</span><span class="sxs-lookup"><span data-stu-id="4bd15-123">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4bd15-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4bd15-124">Next steps</span></span>

<span data-ttu-id="4bd15-125">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4bd15-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4bd15-126">További Azure Functions CLI parancsfájl minták hello található [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4bd15-126">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>

---
title: "Függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot |} Microsoft Docs"
description: "Függvény-alkalmazás létrehozása és központi telepítése a Githubból funkciókódot"
services: functions
ms.service: functions
keywords: 
ms.devlang: azurecli
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.custom: mvc
ms.openlocfilehash: 67eb41d89328ab57741c419d8b718e19b947dab1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-app-service"></a><span data-ttu-id="36096-103">Hozzon létre egy App Service</span><span class="sxs-lookup"><span data-stu-id="36096-103">Create an App Service</span></span>

<span data-ttu-id="36096-104">Ez a parancsfájlpélda hoz létre, a függvény alkalmazás használatával az [fogyasztás terv](../functions-scale.md#consumption-plan) kapcsolódó erőforrásokkal, és folyamatosan telepíti a funkciókódot a GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="36096-104">This sample script creates a function app using the [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and continuously deploys your function code from a GitHub repository.</span></span> <span data-ttu-id="36096-105">Ez a példa lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="36096-105">In this sample, you need:</span></span>

* <span data-ttu-id="36096-106">GitHub-tárházban funkciók kóddal, a rendszergazdai jogosultsággal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="36096-106">A GitHub repository with functions code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="36096-107">A [személyes hozzáférési jogkivonat (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) a GitHub-fiókjában.</span><span class="sxs-lookup"><span data-stu-id="36096-107">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="36096-108">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="36096-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="36096-109">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="36096-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="36096-110">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="36096-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="36096-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="36096-111">Sample script</span></span>

<span data-ttu-id="36096-112">Ez a minta egy Azure-függvény alkalmazás létrehozza, és telepíti a Githubról funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="36096-112">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

<span data-ttu-id="36096-113">[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure szolgáltatás")]</span><span class="sxs-lookup"><span data-stu-id="36096-113">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="36096-114">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="36096-114">Script explanation</span></span>

<span data-ttu-id="36096-115">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="36096-115">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="36096-116">Ezt a parancsfájlt használja a következő:</span><span class="sxs-lookup"><span data-stu-id="36096-116">This script uses the following:</span></span>

| <span data-ttu-id="36096-117">Parancs</span><span class="sxs-lookup"><span data-stu-id="36096-117">Command</span></span> | <span data-ttu-id="36096-118">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="36096-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="36096-119">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="36096-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="36096-120">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="36096-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="36096-121">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="36096-121">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="36096-122">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="36096-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="36096-123">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="36096-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [<span data-ttu-id="36096-124">az App Service web verziókezelő konfiguráció</span><span class="sxs-lookup"><span data-stu-id="36096-124">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="36096-125">Egy függvény app társítja a Git vagy Mercurial tárházba.</span><span class="sxs-lookup"><span data-stu-id="36096-125">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="36096-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36096-126">Next steps</span></span>

<span data-ttu-id="36096-127">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="36096-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="36096-128">További Azure Functions CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="36096-128">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>

---
title: "egy függvény App aaaCreate és központi telepítése a Githubból funkciókódot |} Microsoft Docs"
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
ms.openlocfilehash: 4d44204b899b32af464260db51ed98bcf00eb2bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-app-service"></a><span data-ttu-id="7081c-103">Hozzon létre egy App Service</span><span class="sxs-lookup"><span data-stu-id="7081c-103">Create an App Service</span></span>

<span data-ttu-id="7081c-104">Ez a parancsfájlpélda hoz létre egy függvény app hello segítségével [fogyasztás terv](../functions-scale.md#consumption-plan) kapcsolódó erőforrásokkal, és folyamatosan telepíti a funkciókódot a GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="7081c-104">This sample script creates a function app using hello [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and continuously deploys your function code from a GitHub repository.</span></span> <span data-ttu-id="7081c-105">Ez a példa lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="7081c-105">In this sample, you need:</span></span>

* <span data-ttu-id="7081c-106">GitHub-tárházban funkciók kóddal, a rendszergazdai jogosultsággal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7081c-106">A GitHub repository with functions code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="7081c-107">A [személyes hozzáférési jogkivonat (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) a GitHub-fiókjában.</span><span class="sxs-lookup"><span data-stu-id="7081c-107">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="7081c-108">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="7081c-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="7081c-109">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="7081c-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="7081c-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7081c-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="7081c-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="7081c-111">Sample script</span></span>

<span data-ttu-id="7081c-112">Ez a minta egy Azure-függvény alkalmazás létrehozza, és telepíti a Githubról funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="7081c-112">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="7081c-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="7081c-113">Script explanation</span></span>

<span data-ttu-id="7081c-114">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="7081c-114">Each command in hello table links toocommand specific documentation.</span></span> <span data-ttu-id="7081c-115">A parancsfájl a következő hello:</span><span class="sxs-lookup"><span data-stu-id="7081c-115">This script uses hello following:</span></span>

| <span data-ttu-id="7081c-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="7081c-116">Command</span></span> | <span data-ttu-id="7081c-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7081c-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7081c-118">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="7081c-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="7081c-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7081c-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7081c-120">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="7081c-120">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="7081c-121">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7081c-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="7081c-122">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="7081c-122">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [<span data-ttu-id="7081c-123">az App Service web verziókezelő konfiguráció</span><span class="sxs-lookup"><span data-stu-id="7081c-123">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="7081c-124">Egy függvény app társítja a Git vagy Mercurial tárházba.</span><span class="sxs-lookup"><span data-stu-id="7081c-124">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7081c-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7081c-125">Next steps</span></span>

<span data-ttu-id="7081c-126">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7081c-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="7081c-127">További Azure Functions CLI parancsfájl minták hello található [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7081c-127">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>

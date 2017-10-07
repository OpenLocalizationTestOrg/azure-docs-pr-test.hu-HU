---
title: "aaaAzure CLI parancsfájl minta - webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0205c991-0989-4ca3-bb41-237dcc964460
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6adb06a35ceea8ea64723c9887c25c50f046e280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="ddf01-103">Webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról</span><span class="sxs-lookup"><span data-stu-id="ddf01-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="ddf01-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és beállítja egy GitHub-tárházban folyamatos üzembe helyezést.</span><span class="sxs-lookup"><span data-stu-id="ddf01-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="ddf01-105">Folyamatos üzembe helyezés nélkül GitHub telepítését: [webalkalmazás létrehozása és központi telepítése a Githubból kód](app-service-cli-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="ddf01-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-cli-deploy-github.md).</span></span> <span data-ttu-id="ddf01-106">Ez a példa szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="ddf01-106">In this sample, you will need:</span></span>

* <span data-ttu-id="ddf01-107">GitHub-tárházban alkalmazás kóddal, a rendszergazdai jogosultsággal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="ddf01-107">A GitHub repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="ddf01-108">A [személyes hozzáférési jogkivonat (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) a GitHub-fiókjában.</span><span class="sxs-lookup"><span data-stu-id="ddf01-108">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ddf01-109">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="ddf01-109">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ddf01-110">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="ddf01-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ddf01-111">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ddf01-111">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ddf01-112">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="ddf01-112">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="ddf01-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="ddf01-113">Script explanation</span></span>

<span data-ttu-id="ddf01-114">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="ddf01-114">This script uses hello following commands.</span></span> <span data-ttu-id="ddf01-115">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="ddf01-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ddf01-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="ddf01-116">Command</span></span> | <span data-ttu-id="ddf01-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ddf01-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ddf01-118">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ddf01-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ddf01-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ddf01-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ddf01-120">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="ddf01-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="ddf01-121">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ddf01-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="ddf01-122">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="ddf01-122">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="ddf01-123">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ddf01-123">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="ddf01-124">az webalkalmazás központi telepítési forrás konfigurációs</span><span class="sxs-lookup"><span data-stu-id="ddf01-124">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="ddf01-125">Azure-webalkalmazás társít egy Mercurial vagy a Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="ddf01-125">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="ddf01-126">az alkalmazás kulcs Tallózás</span><span class="sxs-lookup"><span data-stu-id="ddf01-126">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="ddf01-127">Azure-webalkalmazás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="ddf01-127">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ddf01-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ddf01-128">Next steps</span></span>

<span data-ttu-id="ddf01-129">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ddf01-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ddf01-130">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ddf01-130">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

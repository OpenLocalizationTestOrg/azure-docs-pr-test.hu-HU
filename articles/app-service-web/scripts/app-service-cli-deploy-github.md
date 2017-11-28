---
title: "aaaAzure CLI parancsfájl minta - webalkalmazás létrehozása a Githubról telepítés |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - webalkalmazás létrehozása a Githubról telepítés"
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
ms.tgt_pltfrm: sample
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: eb7231aa5c6a7e23d76885107e733008382f7487
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-deployment-from-github"></a><span data-ttu-id="4268e-103">Hozzon létre egy webalkalmazást telepítése a Githubból</span><span class="sxs-lookup"><span data-stu-id="4268e-103">Create a web app with deployment from GitHub</span></span>

<span data-ttu-id="4268e-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és majd központilag telepíti a webes alkalmazás kód egy nyilvános GitHub-adattár (folyamatos üzembe helyezés) nélkül.</span><span class="sxs-lookup"><span data-stu-id="4268e-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="4268e-105">Lásd a GitHub-telepítéshez olyan folyamatos üzembe helyezés, [webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról](app-service-cli-continuous-deployment-github.md).</span><span class="sxs-lookup"><span data-stu-id="4268e-105">For GitHub deployment with continuous deployment, see [Create a web app with continuous deployment from GitHub](app-service-cli-continuous-deployment-github.md).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4268e-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="4268e-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="4268e-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="4268e-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="4268e-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4268e-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="4268e-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="4268e-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github/deploy-github.sh?highlight=3 "Create a web app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="4268e-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="4268e-110">Script explanation</span></span> 

<span data-ttu-id="4268e-111">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="4268e-111">This script uses hello following commands.</span></span> <span data-ttu-id="4268e-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="4268e-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4268e-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="4268e-113">Command</span></span> | <span data-ttu-id="4268e-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4268e-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4268e-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="4268e-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4268e-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4268e-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4268e-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="4268e-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="4268e-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4268e-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="4268e-119">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="4268e-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="4268e-120">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4268e-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="4268e-121">az webalkalmazás központi telepítési forrás konfigurációs</span><span class="sxs-lookup"><span data-stu-id="4268e-121">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="4268e-122">Azure-webalkalmazás társít egy Mercurial vagy a Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="4268e-122">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="4268e-123">az alkalmazás kulcs Tallózás</span><span class="sxs-lookup"><span data-stu-id="4268e-123">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="4268e-124">Azure-webalkalmazás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="4268e-124">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4268e-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4268e-125">Next steps</span></span>

<span data-ttu-id="4268e-126">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4268e-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4268e-127">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4268e-127">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

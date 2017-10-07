---
title: "aaaAzure CLI parancsfájl minta - webalkalmazás létrehozása és központi telepítése átmeneti környezet kód tooa |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - webalkalmazás létrehozása és központi telepítése átmeneti környezet kód tooa"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 2b995dcd-e471-4355-9fda-00babcdb156e
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: cd07f5eda31041effd7b7334f5ecc04e6c1a0514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-tooa-staging-environment"></a><span data-ttu-id="150c5-103">Webalkalmazás létrehozása és központi telepítése átmeneti környezet kód tooa</span><span class="sxs-lookup"><span data-stu-id="150c5-103">Create a web app and deploy code tooa staging environment</span></span>

<span data-ttu-id="150c5-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service egy további üzembe helyezési pont "tesztelés" nevezik, és majd központilag telepíti egy minta app toohello "staging" tárolóhely.</span><span class="sxs-lookup"><span data-stu-id="150c5-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app toohello "staging" slot.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="150c5-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="150c5-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="150c5-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="150c5-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="150c5-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="150c5-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="150c5-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="150c5-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Create a web app and deploy code tooa staging environment")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="150c5-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="150c5-109">Script explanation</span></span>

<span data-ttu-id="150c5-110">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="150c5-110">This script uses hello following commands.</span></span> <span data-ttu-id="150c5-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="150c5-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="150c5-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="150c5-112">Command</span></span> | <span data-ttu-id="150c5-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="150c5-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="150c5-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="150c5-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="150c5-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="150c5-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="150c5-116">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="150c5-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="150c5-117">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="150c5-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="150c5-118">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="150c5-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="150c5-119">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="150c5-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="150c5-120">az webalkalmazás üzembe helyezési tárhely létrehozása</span><span class="sxs-lookup"><span data-stu-id="150c5-120">az webapp deployment slot create</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#create) | <span data-ttu-id="150c5-121">Egy üzembe helyezési tárhely létrehozása.</span><span class="sxs-lookup"><span data-stu-id="150c5-121">Create a deployment slot.</span></span> |
| [<span data-ttu-id="150c5-122">az webalkalmazás központi telepítési forrás konfigurációs</span><span class="sxs-lookup"><span data-stu-id="150c5-122">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="150c5-123">Azure-webalkalmazás társít egy Mercurial vagy a Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="150c5-123">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="150c5-124">az alkalmazás kulcs Tallózás</span><span class="sxs-lookup"><span data-stu-id="150c5-124">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="150c5-125">Azure-webalkalmazás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="150c5-125">Open an Azure web app in a browser.</span></span> |
| [<span data-ttu-id="150c5-126">az webalkalmazás központi telepítési tárolóhelycsere</span><span class="sxs-lookup"><span data-stu-id="150c5-126">az webapp deployment slot swap</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#swap) | <span data-ttu-id="150c5-127">A megadott üzembe helyezési pont felcserélése éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="150c5-127">Swap a specified deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="150c5-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="150c5-128">Next steps</span></span>

<span data-ttu-id="150c5-129">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="150c5-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="150c5-130">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="150c5-130">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

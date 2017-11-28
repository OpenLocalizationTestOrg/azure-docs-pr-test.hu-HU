---
title: "aaaAzure CLI parancsfájl minta - webalkalmazás létrehozása a folyamatos üzembe helyezés, a Visual Studio Team Services |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - webalkalmazás létrehozása a folyamatos üzembe helyezés, a Visual Studio Team Services"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: f8d0c2645ec5311296ca9b2df20f97e77bab2283
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a><span data-ttu-id="a7722-103">A folyamatos üzembe helyezés, a Visual Studio Team Services-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7722-103">Create a web app with continuous deployment from Visual Studio Team Services</span></span>

<span data-ttu-id="a7722-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és beállítja a Visual Studio Team Services tárházból folyamatos üzembe helyezést.</span><span class="sxs-lookup"><span data-stu-id="a7722-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a Visual Studio Team Services repository.</span></span> <span data-ttu-id="a7722-105">Az ebben a példában a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="a7722-105">For this sample, you will need:</span></span>

* <span data-ttu-id="a7722-106">A Visual Studio Team Services tárház alkalmazás kóddal, a rendszergazdai jogosultsággal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="a7722-106">A Visual Studio Team Services repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="a7722-107">A [személyes hozzáférési jogkivonat (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) a Visual Studio Team Services-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="a7722-107">A [Personal Access Token (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) for your Visual Studio Team Services account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a7722-108">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a7722-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a7722-109">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="a7722-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a7722-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a7722-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="a7722-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="a7722-111">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from Visual Studio Team Services")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="a7722-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="a7722-112">Script explanation</span></span>

<span data-ttu-id="a7722-113">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="a7722-113">This script uses hello following commands.</span></span> <span data-ttu-id="a7722-114">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="a7722-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a7722-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="a7722-115">Command</span></span> | <span data-ttu-id="a7722-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a7722-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a7722-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7722-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="a7722-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a7722-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a7722-119">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7722-119">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="a7722-120">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a7722-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="a7722-121">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7722-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="a7722-122">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a7722-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="a7722-123">az webalkalmazás központi telepítési forrás konfigurációs</span><span class="sxs-lookup"><span data-stu-id="a7722-123">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="a7722-124">Azure-webalkalmazás társít egy Mercurial vagy a Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="a7722-124">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="a7722-125">az alkalmazás kulcs Tallózás</span><span class="sxs-lookup"><span data-stu-id="a7722-125">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="a7722-126">Azure-webalkalmazás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="a7722-126">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a7722-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a7722-127">Next steps</span></span>

<span data-ttu-id="a7722-128">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a7722-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a7722-129">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a7722-129">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

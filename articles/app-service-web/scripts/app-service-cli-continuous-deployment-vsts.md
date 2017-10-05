---
title: "Az Azure CLI-parancsfájlt minta - webalkalmazás létrehozása a folyamatos üzembe helyezés, a Visual Studio Team Services |} Microsoft Docs"
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
ms.openlocfilehash: 2b983616757ca3c4226c12876f5fd4c285067318
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a><span data-ttu-id="c25b1-103">A folyamatos üzembe helyezés, a Visual Studio Team Services-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c25b1-103">Create a web app with continuous deployment from Visual Studio Team Services</span></span>

<span data-ttu-id="c25b1-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és beállítja a Visual Studio Team Services tárházból folyamatos üzembe helyezést.</span><span class="sxs-lookup"><span data-stu-id="c25b1-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a Visual Studio Team Services repository.</span></span> <span data-ttu-id="c25b1-105">Az ebben a példában a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="c25b1-105">For this sample, you will need:</span></span>

* <span data-ttu-id="c25b1-106">A Visual Studio Team Services tárház alkalmazás kóddal, a rendszergazdai jogosultsággal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="c25b1-106">A Visual Studio Team Services repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="c25b1-107">A [személyes hozzáférési jogkivonat (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) a Visual Studio Team Services-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="c25b1-107">A [Personal Access Token (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) for your Visual Studio Team Services account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c25b1-108">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="c25b1-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c25b1-109">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="c25b1-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="c25b1-110">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c25b1-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="c25b1-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="c25b1-111">Sample script</span></span>

<span data-ttu-id="c25b1-112">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "a folyamatos üzembe helyezés, a Visual Studio Team Services-webalkalmazás létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="c25b1-112">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from Visual Studio Team Services")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="c25b1-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="c25b1-113">Script explanation</span></span>

<span data-ttu-id="c25b1-114">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="c25b1-114">This script uses the following commands.</span></span> <span data-ttu-id="c25b1-115">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="c25b1-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c25b1-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="c25b1-116">Command</span></span> | <span data-ttu-id="c25b1-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c25b1-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c25b1-118">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="c25b1-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c25b1-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c25b1-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c25b1-120">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="c25b1-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="c25b1-121">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c25b1-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="c25b1-122">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="c25b1-122">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="c25b1-123">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c25b1-123">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="c25b1-124">az webalkalmazás központi telepítési forrás konfigurációs</span><span class="sxs-lookup"><span data-stu-id="c25b1-124">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="c25b1-125">Azure-webalkalmazás társít egy Mercurial vagy a Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="c25b1-125">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="c25b1-126">az alkalmazás kulcs Tallózás</span><span class="sxs-lookup"><span data-stu-id="c25b1-126">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="c25b1-127">Azure-webalkalmazás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="c25b1-127">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c25b1-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c25b1-128">Next steps</span></span>

<span data-ttu-id="c25b1-129">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c25b1-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c25b1-130">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c25b1-130">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

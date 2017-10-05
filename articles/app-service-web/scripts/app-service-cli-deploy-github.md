---
title: "Az Azure CLI-parancsfájlt minta - webalkalmazás létrehozása a Githubról telepítés |} Microsoft Docs"
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
ms.openlocfilehash: 61e9d65319cecf3ea4e9152ebdf1035566aad74c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-with-deployment-from-github"></a><span data-ttu-id="2c5fc-103">Hozzon létre egy webalkalmazást telepítése a Githubból</span><span class="sxs-lookup"><span data-stu-id="2c5fc-103">Create a web app with deployment from GitHub</span></span>

<span data-ttu-id="2c5fc-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és majd központilag telepíti a webes alkalmazás kód egy nyilvános GitHub-adattár (folyamatos üzembe helyezés) nélkül.</span><span class="sxs-lookup"><span data-stu-id="2c5fc-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="2c5fc-105">Lásd a GitHub-telepítéshez olyan folyamatos üzembe helyezés, [webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról](app-service-cli-continuous-deployment-github.md).</span><span class="sxs-lookup"><span data-stu-id="2c5fc-105">For GitHub deployment with continuous deployment, see [Create a web app with continuous deployment from GitHub](app-service-cli-continuous-deployment-github.md).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="2c5fc-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="2c5fc-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="2c5fc-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="2c5fc-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="2c5fc-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2c5fc-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="2c5fc-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="2c5fc-109">Sample script</span></span>

<span data-ttu-id="2c5fc-110">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/deploy-github/deploy-github.sh?highlight=3 "webalkalmazás létrehozása a Githubról telepítés")]</span><span class="sxs-lookup"><span data-stu-id="2c5fc-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github/deploy-github.sh?highlight=3 "Create a web app with deployment from GitHub")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="2c5fc-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="2c5fc-111">Script explanation</span></span> 

<span data-ttu-id="2c5fc-112">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="2c5fc-112">This script uses the following commands.</span></span> <span data-ttu-id="2c5fc-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="2c5fc-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="2c5fc-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="2c5fc-114">Command</span></span> | <span data-ttu-id="2c5fc-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="2c5fc-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2c5fc-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c5fc-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="2c5fc-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2c5fc-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2c5fc-118">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c5fc-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="2c5fc-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2c5fc-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="2c5fc-120">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c5fc-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="2c5fc-121">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2c5fc-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="2c5fc-122">az webalkalmazás központi telepítési forrás konfigurációs</span><span class="sxs-lookup"><span data-stu-id="2c5fc-122">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="2c5fc-123">Azure-webalkalmazás társít egy Mercurial vagy a Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="2c5fc-123">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="2c5fc-124">az alkalmazás kulcs Tallózás</span><span class="sxs-lookup"><span data-stu-id="2c5fc-124">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="2c5fc-125">Azure-webalkalmazás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="2c5fc-125">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2c5fc-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2c5fc-126">Next steps</span></span>

<span data-ttu-id="2c5fc-127">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2c5fc-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="2c5fc-128">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2c5fc-128">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

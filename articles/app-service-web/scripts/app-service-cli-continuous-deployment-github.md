---
title: "Az Azure CLI-parancsfájlt minta - webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról |} Microsoft Docs"
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
ms.openlocfilehash: a12085a7a8146c22d6b079381542d4fe3a8e6e87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="b069b-103">Webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról</span><span class="sxs-lookup"><span data-stu-id="b069b-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="b069b-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és beállítja egy GitHub-tárházban folyamatos üzembe helyezést.</span><span class="sxs-lookup"><span data-stu-id="b069b-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="b069b-105">Folyamatos üzembe helyezés nélkül GitHub telepítését: [webalkalmazás létrehozása és központi telepítése a Githubból kód](app-service-cli-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="b069b-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-cli-deploy-github.md).</span></span> <span data-ttu-id="b069b-106">Ez a példa szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="b069b-106">In this sample, you will need:</span></span>

* <span data-ttu-id="b069b-107">GitHub-tárházban alkalmazás kóddal, a rendszergazdai jogosultsággal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="b069b-107">A GitHub repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="b069b-108">A [személyes hozzáférési jogkivonat (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) a GitHub-fiókjában.</span><span class="sxs-lookup"><span data-stu-id="b069b-108">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b069b-109">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="b069b-109">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="b069b-110">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="b069b-110">Run `az --version` to find the version.</span></span> <span data-ttu-id="b069b-111">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b069b-111">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="b069b-112">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="b069b-112">Sample script</span></span>

<span data-ttu-id="b069b-113">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról")]</span><span class="sxs-lookup"><span data-stu-id="b069b-113">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from GitHub")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="b069b-114">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="b069b-114">Script explanation</span></span>

<span data-ttu-id="b069b-115">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="b069b-115">This script uses the following commands.</span></span> <span data-ttu-id="b069b-116">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="b069b-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="b069b-117">Parancs</span><span class="sxs-lookup"><span data-stu-id="b069b-117">Command</span></span> | <span data-ttu-id="b069b-118">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b069b-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b069b-119">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="b069b-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="b069b-120">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b069b-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b069b-121">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="b069b-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="b069b-122">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b069b-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="b069b-123">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="b069b-123">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="b069b-124">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b069b-124">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="b069b-125">az webalkalmazás központi telepítési forrás konfigurációs</span><span class="sxs-lookup"><span data-stu-id="b069b-125">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="b069b-126">Azure-webalkalmazás társít egy Mercurial vagy a Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="b069b-126">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="b069b-127">az alkalmazás kulcs Tallózás</span><span class="sxs-lookup"><span data-stu-id="b069b-127">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="b069b-128">Azure-webalkalmazás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="b069b-128">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b069b-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b069b-129">Next steps</span></span>

<span data-ttu-id="b069b-130">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b069b-130">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b069b-131">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b069b-131">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

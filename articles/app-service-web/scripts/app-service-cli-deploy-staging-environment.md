---
title: "Az Azure CLI parancsfájl minta - webalkalmazás létrehozása és kód telepítése az átmeneti környezetben való használatra |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - webalkalmazás létrehozása és kód telepítése az átmeneti környezetben való használatra"
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
ms.openlocfilehash: d586b50258c32e44f55859aad0a89475e9e4d2eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a><span data-ttu-id="0942f-103">Webalkalmazás létrehozása, majd kód telepítése az átmeneti környezetben való használatra</span><span class="sxs-lookup"><span data-stu-id="0942f-103">Create a web app and deploy code to a staging environment</span></span>

<span data-ttu-id="0942f-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service egy további üzembe helyezési pont "tesztelés" nevű, és ezután telepíti egy mintaalkalmazást az "előkészítési" pont.</span><span class="sxs-lookup"><span data-stu-id="0942f-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app to the "staging" slot.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0942f-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="0942f-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="0942f-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="0942f-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="0942f-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0942f-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="0942f-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="0942f-108">Sample script</span></span>

<span data-ttu-id="0942f-109">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "webalkalmazás létrehozása, majd kód telepítése az átmeneti környezetben való használatra")]</span><span class="sxs-lookup"><span data-stu-id="0942f-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Create a web app and deploy code to a staging environment")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="0942f-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="0942f-110">Script explanation</span></span>

<span data-ttu-id="0942f-111">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="0942f-111">This script uses the following commands.</span></span> <span data-ttu-id="0942f-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="0942f-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="0942f-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="0942f-113">Command</span></span> | <span data-ttu-id="0942f-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="0942f-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0942f-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0942f-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="0942f-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0942f-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0942f-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="0942f-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="0942f-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0942f-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="0942f-119">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="0942f-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="0942f-120">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0942f-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="0942f-121">az webalkalmazás üzembe helyezési tárhely létrehozása</span><span class="sxs-lookup"><span data-stu-id="0942f-121">az webapp deployment slot create</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#create) | <span data-ttu-id="0942f-122">Egy üzembe helyezési tárhely létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0942f-122">Create a deployment slot.</span></span> |
| [<span data-ttu-id="0942f-123">az webalkalmazás központi telepítési forrás konfigurációs</span><span class="sxs-lookup"><span data-stu-id="0942f-123">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="0942f-124">Azure-webalkalmazás társít egy Mercurial vagy a Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="0942f-124">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="0942f-125">az alkalmazás kulcs Tallózás</span><span class="sxs-lookup"><span data-stu-id="0942f-125">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="0942f-126">Azure-webalkalmazás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="0942f-126">Open an Azure web app in a browser.</span></span> |
| [<span data-ttu-id="0942f-127">az webalkalmazás központi telepítési tárolóhelycsere</span><span class="sxs-lookup"><span data-stu-id="0942f-127">az webapp deployment slot swap</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#swap) | <span data-ttu-id="0942f-128">A megadott üzembe helyezési pont felcserélése éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="0942f-128">Swap a specified deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0942f-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0942f-129">Next steps</span></span>

<span data-ttu-id="0942f-130">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0942f-130">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="0942f-131">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="0942f-131">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

---
title: "Azure CLI parancsfájl minta - webalkalmazás létrehozása és központi telepítése a kódot egy helyi Git-tárház |} Microsoft Docs"
description: "Azure CLI parancsfájl minta - webalkalmazás létrehozása és központi telepítése egy helyi Git-tárház kódot"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 048f98aa-f708-44cb-9b9e-953f67dc6da8
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 50d69ac48438920ce59808ee79809235d8330b14
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a><span data-ttu-id="1aa6a-103">A webalkalmazás létrehozása és telepítése a kódot egy helyi Git-tárházat</span><span class="sxs-lookup"><span data-stu-id="1aa6a-103">Create a web app and deploy code from a local Git repository</span></span>

<span data-ttu-id="1aa6a-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és ezután telepíti a webes alkalmazás kódja egy helyi Git-tárházban.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code in a local Git repository.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="1aa6a-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="1aa6a-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="1aa6a-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1aa6a-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="1aa6a-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="1aa6a-108">Sample script</span></span>

<span data-ttu-id="1aa6a-109">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "egy webalkalmazás létrehozása és telepítése a kódot egy helyi Git-tárházat")]</span><span class="sxs-lookup"><span data-stu-id="1aa6a-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "Create a web app and deploy code from a local Git repository")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="1aa6a-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="1aa6a-110">Script explanation</span></span>

<span data-ttu-id="1aa6a-111">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-111">This script uses the following commands.</span></span> <span data-ttu-id="1aa6a-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="1aa6a-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="1aa6a-113">Command</span></span> | <span data-ttu-id="1aa6a-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1aa6a-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1aa6a-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="1aa6a-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="1aa6a-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1aa6a-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="1aa6a-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="1aa6a-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="1aa6a-119">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="1aa6a-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="1aa6a-120">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="1aa6a-121">beállítva az az webalkalmazás üzembe helyező felhasználó</span><span class="sxs-lookup"><span data-stu-id="1aa6a-121">az webapp deployment user set</span></span>](https://review.docs.microsoft.com/cli/azure/webapp/deployment/user#set) | <span data-ttu-id="1aa6a-122">A fiók szintű üzembe helyezési hitelesítő adatok beállítása az App Service.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-122">Sets the account-level deployment credentials for App Service.</span></span> |
| [<span data-ttu-id="1aa6a-123">az webalkalmazás központi telepítési forrás config-helyi – git</span><span class="sxs-lookup"><span data-stu-id="1aa6a-123">az webapp deployment source config-local-git</span></span>](https://review.docs.microsoft.com/cli/azure/webapp/deployment/source#config-local-git) | <span data-ttu-id="1aa6a-124">Létrehoz egy helyi Git-tárház forrás vezérlő konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-124">Creates a source control configuration for a local Git repository.</span></span> |
| [<span data-ttu-id="1aa6a-125">az alkalmazás kulcs Tallózás</span><span class="sxs-lookup"><span data-stu-id="1aa6a-125">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="1aa6a-126">Azure-webalkalmazás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="1aa6a-126">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1aa6a-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1aa6a-127">Next steps</span></span>

<span data-ttu-id="1aa6a-128">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1aa6a-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="1aa6a-129">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="1aa6a-129">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

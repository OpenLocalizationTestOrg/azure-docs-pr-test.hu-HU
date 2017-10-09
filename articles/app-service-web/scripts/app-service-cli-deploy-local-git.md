---
title: "aaaAzure CLI parancsfájl minta - webalkalmazás létrehozása és központi telepítése a kódot egy helyi Git-tárház |} Microsoft Docs"
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
ms.openlocfilehash: 5ad75394c40025d8941282eabeaf34c19c72ee1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a><span data-ttu-id="394ec-103">A webalkalmazás létrehozása és telepítése a kódot egy helyi Git-tárházat</span><span class="sxs-lookup"><span data-stu-id="394ec-103">Create a web app and deploy code from a local Git repository</span></span>

<span data-ttu-id="394ec-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és ezután telepíti a webes alkalmazás kódja egy helyi Git-tárházban.</span><span class="sxs-lookup"><span data-stu-id="394ec-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code in a local Git repository.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="394ec-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="394ec-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="394ec-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="394ec-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="394ec-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="394ec-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="394ec-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="394ec-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "Create a web app and deploy code from a local Git repository")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="394ec-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="394ec-109">Script explanation</span></span>

<span data-ttu-id="394ec-110">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="394ec-110">This script uses hello following commands.</span></span> <span data-ttu-id="394ec-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="394ec-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="394ec-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="394ec-112">Command</span></span> | <span data-ttu-id="394ec-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="394ec-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="394ec-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="394ec-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="394ec-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="394ec-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="394ec-116">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="394ec-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="394ec-117">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="394ec-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="394ec-118">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="394ec-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="394ec-119">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="394ec-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="394ec-120">beállítva az az webalkalmazás üzembe helyező felhasználó</span><span class="sxs-lookup"><span data-stu-id="394ec-120">az webapp deployment user set</span></span>](https://review.docs.microsoft.com/cli/azure/webapp/deployment/user#set) | <span data-ttu-id="394ec-121">App Service hello fiók szintű üzembe helyezési hitelesítő adatok beállítása.</span><span class="sxs-lookup"><span data-stu-id="394ec-121">Sets hello account-level deployment credentials for App Service.</span></span> |
| [<span data-ttu-id="394ec-122">az webalkalmazás központi telepítési forrás config-helyi – git</span><span class="sxs-lookup"><span data-stu-id="394ec-122">az webapp deployment source config-local-git</span></span>](https://review.docs.microsoft.com/cli/azure/webapp/deployment/source#config-local-git) | <span data-ttu-id="394ec-123">Létrehoz egy helyi Git-tárház forrás vezérlő konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="394ec-123">Creates a source control configuration for a local Git repository.</span></span> |
| [<span data-ttu-id="394ec-124">az alkalmazás kulcs Tallózás</span><span class="sxs-lookup"><span data-stu-id="394ec-124">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="394ec-125">Azure-webalkalmazás megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="394ec-125">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="394ec-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="394ec-126">Next steps</span></span>

<span data-ttu-id="394ec-127">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="394ec-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="394ec-128">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="394ec-128">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

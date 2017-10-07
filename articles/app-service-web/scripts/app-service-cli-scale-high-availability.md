---
title: "aaaAzure CLI parancsfájl minta - webalkalmazás világszerte egy nagy-availabilty architektúrával méretezése |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - skálázási világszerte egy nagy-availabilty architektúrával webes alkalmazás"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4033a50-0e05-4505-8ce8-c876204b2acc
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b72fbccd7f2aaab58e4b4721e14dca14146c7c72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="bd5cd-103">A webalkalmazás skálázása világszerte egy magas rendelkezésre állású architektúra</span><span class="sxs-lookup"><span data-stu-id="bd5cd-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="bd5cd-104">Ebben a forgatókönyvben létre fog hozni egy erőforráscsoport, két app service-csomagokról, két webes alkalmazásokat, egy traffic manager-profil, illetve két traffic manager-végpont.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="bd5cd-105">Hello a gyakorlatban befejezése után egy magas rendelkezésre állású fog architektúra, amely lehetővé teszi a megoldással a globális hello legkisebb hálózati késést alapuló webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-105">Once hello exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on hello lowest network latency.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="bd5cd-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="bd5cd-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="bd5cd-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="bd5cd-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="bd5cd-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="bd5cd-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "Geographic Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="bd5cd-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="bd5cd-110">Script explanation</span></span>

<span data-ttu-id="bd5cd-111">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a webes alkalmazás, a traffic manager-profil hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-111">This script uses hello following commands toocreate a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="bd5cd-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="bd5cd-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="bd5cd-113">Command</span></span> | <span data-ttu-id="bd5cd-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="bd5cd-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bd5cd-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd5cd-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="bd5cd-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bd5cd-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd5cd-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="bd5cd-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-118">Creates an App Service plan.</span></span> <span data-ttu-id="bd5cd-119">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="bd5cd-120">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd5cd-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="bd5cd-121">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="bd5cd-122">az hálózati forgalmat-manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd5cd-122">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="bd5cd-123">Az Azure Traffic Manager-profil létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-123">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="bd5cd-124">az hálózati forgalmat-manager-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd5cd-124">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="bd5cd-125">A végpont tooan Azure Traffic Manager-profil hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="bd5cd-125">Adds a endpoint tooan Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bd5cd-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bd5cd-126">Next steps</span></span>

<span data-ttu-id="bd5cd-127">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bd5cd-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="bd5cd-128">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bd5cd-128">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

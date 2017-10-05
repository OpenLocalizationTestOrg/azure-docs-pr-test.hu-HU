---
title: "Az Azure CLI parancsfájl minta - skálázási világszerte egy nagy-availabilty architektúrával webalkalmazás |} Microsoft Docs"
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
ms.openlocfilehash: c368bdc48f197ff5b491d1796d85abfd339051a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="70adf-103">A webalkalmazás skálázása világszerte egy magas rendelkezésre állású architektúra</span><span class="sxs-lookup"><span data-stu-id="70adf-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="70adf-104">Ebben a forgatókönyvben létre fog hozni egy erőforráscsoport, két app service-csomagokról, két webes alkalmazásokat, egy traffic manager-profil, illetve két traffic manager-végpont.</span><span class="sxs-lookup"><span data-stu-id="70adf-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="70adf-105">A gyakorlatban befejezése után egy magas rendelkezésre állású fog architektúra, amely lehetővé teszi, hogy globális alapján a legkisebb hálózati késést a webalkalmazás rendelkezésre állását biztosítja.</span><span class="sxs-lookup"><span data-stu-id="70adf-105">Once the exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on the lowest network latency.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="70adf-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="70adf-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="70adf-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="70adf-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="70adf-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="70adf-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="70adf-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="70adf-109">Sample script</span></span>

<span data-ttu-id="70adf-110">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "földrajzi méretezési")]</span><span class="sxs-lookup"><span data-stu-id="70adf-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "Geographic Scale")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="70adf-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="70adf-111">Script explanation</span></span>

<span data-ttu-id="70adf-112">A parancsfájl a következő parancsokat egy erőforráscsoport, webalkalmazás, traffic manager-profil és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="70adf-112">This script uses the following commands to create a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="70adf-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="70adf-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="70adf-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="70adf-114">Command</span></span> | <span data-ttu-id="70adf-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="70adf-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="70adf-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="70adf-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="70adf-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="70adf-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="70adf-118">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="70adf-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="70adf-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="70adf-119">Creates an App Service plan.</span></span> <span data-ttu-id="70adf-120">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="70adf-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="70adf-121">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="70adf-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="70adf-122">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="70adf-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="70adf-123">az hálózati forgalmat-manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="70adf-123">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="70adf-124">Az Azure Traffic Manager-profil létrehozása.</span><span class="sxs-lookup"><span data-stu-id="70adf-124">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="70adf-125">az hálózati forgalmat-manager-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="70adf-125">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="70adf-126">Az Azure Traffic Manager-profilt ad hozzá egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="70adf-126">Adds a endpoint to an Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="70adf-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="70adf-127">Next steps</span></span>

<span data-ttu-id="70adf-128">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="70adf-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="70adf-129">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="70adf-129">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>

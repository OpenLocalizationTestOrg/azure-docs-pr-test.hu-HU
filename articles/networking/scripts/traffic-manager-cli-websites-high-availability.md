---
title: "parancssori felület parancsfájl minta - irányíthatja a forgalmat a magas rendelkezésre állás, az alkalmazások aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - irányíthatja a forgalmat a magas rendelkezésre állású alkalmazások"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: tysonn
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 2142c8bbec1dffc2f12b5500df142a429393a145
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="7fb7c-103">Irányíthatja a forgalmat a magas rendelkezésre állású alkalmazások</span><span class="sxs-lookup"><span data-stu-id="7fb7c-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="7fb7c-104">Ezt a parancsfájlt hoz létre egy erőforráscsoportot, két app service-csomagokról, két webes alkalmazásokat, egy traffic manager-profil és két traffic manager-végpont.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="7fb7c-105">A TRAFFIC Manager forgalom toohello alkalmazás egy régió tartozik, hello elsődleges régió, valamint toohello másodlagos régióba arra utasítja, amikor az elsődleges régióban hello hello alkalmazás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-105">Traffic Manager directs traffic toohello application in one region as hello primary region, and toohello secondary region when hello application in hello primary region is unavailable.</span></span> <span data-ttu-id="7fb7c-106">Hello-parancsprogram végrehajtása előtt meg kell változtatnia hello MyWebApp, MyWebAppL1 és MyWebAppL2 toounique értékek értékei egész Azure.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-106">Before executing hello script, you must change hello MyWebApp, MyWebAppL1 and MyWebAppL2 values toounique values across Azure.</span></span> <span data-ttu-id="7fb7c-107">Hello parancsprogram futtatása után a hello URL-cím mywebapp.trafficmanager.net hello elsődleges régióban hello app végezheti el.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-107">After running hello script, you can access hello app in hello primary region with hello URL mywebapp.trafficmanager.net.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="7fb7c-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="7fb7c-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a><span data-ttu-id="7fb7c-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="7fb7c-109">Clean up deployment</span></span> 

<span data-ttu-id="7fb7c-110">Hello parancsfájl minta futtatása után a hello kövesse parancs lehet használt tooremove hello erőforráscsoport, az App Service alkalmazás és minden kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-110">After hello script sample has been run, hello follow command can be used tooremove hello resource group, App Service app, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a><span data-ttu-id="7fb7c-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="7fb7c-111">Script explanation</span></span>

<span data-ttu-id="7fb7c-112">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a webes alkalmazás, a traffic manager-profil hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-112">This script uses hello following commands toocreate a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="7fb7c-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="7fb7c-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="7fb7c-114">Command</span></span> | <span data-ttu-id="7fb7c-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7fb7c-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7fb7c-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fb7c-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="7fb7c-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7fb7c-118">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fb7c-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="7fb7c-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-119">Creates an App Service plan.</span></span> <span data-ttu-id="7fb7c-120">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="7fb7c-121">az App Service webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fb7c-121">az appservice web create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#create) | <span data-ttu-id="7fb7c-122">Létrehoz egy Azure webalkalmazás az App Service-csomag hello belül.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-122">Creates an Azure web app within hello App Service plan.</span></span> |
| [<span data-ttu-id="7fb7c-123">az hálózati forgalmat-manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fb7c-123">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="7fb7c-124">Az Azure Traffic Manager-profil létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-124">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="7fb7c-125">az hálózati forgalmat-manager-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fb7c-125">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="7fb7c-126">A végpont tooan Azure Traffic Manager-profil hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="7fb7c-126">Adds a endpoint tooan Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7fb7c-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7fb7c-127">Next steps</span></span>

<span data-ttu-id="7fb7c-128">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7fb7c-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="7fb7c-129">További App Service CLI parancsfájl minták hello található [Azure Networking dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7fb7c-129">Additional App Service CLI script samples can be found in hello [Azure Networking documentation](../cli-samples.md).</span></span>

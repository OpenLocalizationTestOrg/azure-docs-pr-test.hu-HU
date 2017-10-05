---
title: "Az Azure CLI parancsfájl minta - irányíthatja a forgalmat a magas rendelkezésre állású alkalmazások |} Microsoft Docs"
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
ms.openlocfilehash: 0593d063a4935d02aae124d83b62b11e37aa3c33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="4d457-103">Irányíthatja a forgalmat a magas rendelkezésre állású alkalmazások</span><span class="sxs-lookup"><span data-stu-id="4d457-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="4d457-104">Ezt a parancsfájlt hoz létre egy erőforráscsoportot, két app service-csomagokról, két webes alkalmazásokat, egy traffic manager-profil és két traffic manager-végpont.</span><span class="sxs-lookup"><span data-stu-id="4d457-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="4d457-105">A TRAFFIC Manager irányítja a forgalmat a alkalmazását egy régió tartozik, az elsődleges régióban, és a másodlagos régióban, ha az elsődleges régióban az alkalmazás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="4d457-105">Traffic Manager directs traffic to the application in one region as the primary region, and to the secondary region when the application in the primary region is unavailable.</span></span> <span data-ttu-id="4d457-106">A parancsprogram végrehajtása előtt módosítania kell a MyWebApp, MyWebAppL1 és MyWebAppL2 értékek egyedi értékeket az egész Azure.</span><span class="sxs-lookup"><span data-stu-id="4d457-106">Before executing the script, you must change the MyWebApp, MyWebAppL1 and MyWebAppL2 values to unique values across Azure.</span></span> <span data-ttu-id="4d457-107">A parancsfájl futtatása után érheti el az alkalmazást az URL-cím mywebapp.trafficmanager.net az elsődleges régióban.</span><span class="sxs-lookup"><span data-stu-id="4d457-107">After running the script, you can access the app in the primary region with the URL mywebapp.trafficmanager.net.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4d457-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="4d457-108">Sample script</span></span>

<span data-ttu-id="4d457-109">[!code-azurecli-interactive[fő](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "forgalmat a magas rendelkezésre állás")]</span><span class="sxs-lookup"><span data-stu-id="4d457-109">[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]</span></span>


## <a name="clean-up-deployment"></a><span data-ttu-id="4d457-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="4d457-110">Clean up deployment</span></span> 

<span data-ttu-id="4d457-111">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, az App Service-alkalmazást, és minden kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4d457-111">After the script sample has been run, the follow command can be used to remove the resource group, App Service app, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a><span data-ttu-id="4d457-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="4d457-112">Script explanation</span></span>

<span data-ttu-id="4d457-113">A parancsfájl a következő parancsokat egy erőforráscsoport, webalkalmazás, traffic manager-profil és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4d457-113">This script uses the following commands to create a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="4d457-114">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="4d457-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="4d457-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="4d457-115">Command</span></span> | <span data-ttu-id="4d457-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4d457-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4d457-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d457-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4d457-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4d457-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4d457-119">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d457-119">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="4d457-120">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4d457-120">Creates an App Service plan.</span></span> <span data-ttu-id="4d457-121">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="4d457-121">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="4d457-122">az App Service webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d457-122">az appservice web create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#create) | <span data-ttu-id="4d457-123">Létrehoz egy Azure webalkalmazás az App Service-csomag belül.</span><span class="sxs-lookup"><span data-stu-id="4d457-123">Creates an Azure web app within the App Service plan.</span></span> |
| [<span data-ttu-id="4d457-124">az hálózati forgalmat-manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d457-124">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="4d457-125">Az Azure Traffic Manager-profil létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4d457-125">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="4d457-126">az hálózati forgalmat-manager-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d457-126">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="4d457-127">Az Azure Traffic Manager-profilt ad hozzá egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="4d457-127">Adds a endpoint to an Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4d457-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d457-128">Next steps</span></span>

<span data-ttu-id="4d457-129">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4d457-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4d457-130">További App Service CLI parancsfájl minták megtalálhatók a [Azure Networking dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4d457-130">Additional App Service CLI script samples can be found in the [Azure Networking documentation](../cli-samples.md).</span></span>

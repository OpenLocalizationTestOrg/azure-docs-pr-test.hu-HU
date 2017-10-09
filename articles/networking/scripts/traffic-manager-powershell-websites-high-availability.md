---
title: "PowerShell parancsfájl minta - irányíthatja a forgalmat a magas rendelkezésre állás, az alkalmazások aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - irányíthatja a forgalmat a magas rendelkezésre állású alkalmazások"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: georgewallace
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 11d15780403b4ed79e85d7b3495bc5d674bfdaee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="14ae9-103">Irányíthatja a forgalmat a magas rendelkezésre állású alkalmazások</span><span class="sxs-lookup"><span data-stu-id="14ae9-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="14ae9-104">Ezt a parancsfájlt hoz létre egy erőforráscsoportot, két app service-csomagokról, két webes alkalmazásokat, egy traffic manager-profil és két traffic manager-végpont.</span><span class="sxs-lookup"><span data-stu-id="14ae9-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="14ae9-105">A TRAFFIC Manager forgalom toohello alkalmazás egy régió tartozik, hello elsődleges régió, valamint toohello másodlagos régióba arra utasítja, amikor az elsődleges régióban hello hello alkalmazás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="14ae9-105">Traffic Manager directs traffic toohello application in one region as hello primary region, and toohello secondary region when hello application in hello primary region is unavailable.</span></span> <span data-ttu-id="14ae9-106">Hello-parancsprogram végrehajtása előtt meg kell változtatnia hello MyWebApp, MyWebAppL1 és MyWebAppL2 toounique értékek értékei egész Azure.</span><span class="sxs-lookup"><span data-stu-id="14ae9-106">Before executing hello script, you must change hello MyWebApp, MyWebAppL1 and MyWebAppL2 values toounique values across Azure.</span></span> <span data-ttu-id="14ae9-107">Hello parancsprogram futtatása után a hello URL-cím mywebapp.trafficmanager.net hello elsődleges régióban hello app végezheti el.</span><span class="sxs-lookup"><span data-stu-id="14ae9-107">After running hello script, you can access hello app in hello primary region with hello URL mywebapp.trafficmanager.net.</span></span>

<span data-ttu-id="14ae9-108">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="14ae9-108">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="14ae9-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="14ae9-109">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


<span data-ttu-id="14ae9-110">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="14ae9-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a><span data-ttu-id="14ae9-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="14ae9-111">Script explanation</span></span>

<span data-ttu-id="14ae9-112">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a webes alkalmazás, a traffic manager-profil hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="14ae9-112">This script uses hello following commands toocreate a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="14ae9-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="14ae9-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="14ae9-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="14ae9-114">Command</span></span> | <span data-ttu-id="14ae9-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="14ae9-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="14ae9-116">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="14ae9-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | <span data-ttu-id="14ae9-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="14ae9-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="14ae9-118">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="14ae9-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="14ae9-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="14ae9-119">Creates an App Service plan.</span></span> <span data-ttu-id="14ae9-120">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="14ae9-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="14ae9-121">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="14ae9-121">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="14ae9-122">Létrehoz egy Azure webalkalmazás az App Service-csomag hello belül.</span><span class="sxs-lookup"><span data-stu-id="14ae9-122">Creates an Azure web app within hello App Service plan.</span></span> |
| [<span data-ttu-id="14ae9-123">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="14ae9-123">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/new-azurermresource) | <span data-ttu-id="14ae9-124">Létrehoz egy Azure webalkalmazás az App Service-csomag hello belül.</span><span class="sxs-lookup"><span data-stu-id="14ae9-124">Creates an Azure web app within hello App Service plan.</span></span> |
| [<span data-ttu-id="14ae9-125">Új AzureRmTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="14ae9-125">New-AzureRmTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="14ae9-126">Az Azure Traffic Manager-profil létrehozása.</span><span class="sxs-lookup"><span data-stu-id="14ae9-126">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="14ae9-127">Új AzureRmTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="14ae9-127">New-AzureRmTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="14ae9-128">A végpont tooan Azure Traffic Manager-profil hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="14ae9-128">Adds a endpoint tooan Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="14ae9-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14ae9-129">Next steps</span></span>

<span data-ttu-id="14ae9-130">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="14ae9-130">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="14ae9-131">További hálózati PowerShell parancsfájl-mintában hello található [Azure hálózati áttekintés dokumentáció](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="14ae9-131">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>

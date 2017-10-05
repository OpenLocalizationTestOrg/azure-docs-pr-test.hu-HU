---
title: "Az Azure PowerShell-parancsfájl minta - irányíthatja a forgalmat a magas rendelkezésre állású alkalmazások |} Microsoft Docs"
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
ms.openlocfilehash: 2f0ac4fd1779661aab04bafb217e64af5d619a2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="ed170-103">Irányíthatja a forgalmat a magas rendelkezésre állású alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ed170-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="ed170-104">Ezt a parancsfájlt hoz létre egy erőforráscsoportot, két app service-csomagokról, két webes alkalmazásokat, egy traffic manager-profil és két traffic manager-végpont.</span><span class="sxs-lookup"><span data-stu-id="ed170-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="ed170-105">A TRAFFIC Manager irányítja a forgalmat a alkalmazását egy régió tartozik, az elsődleges régióban, és a másodlagos régióban, ha az elsődleges régióban az alkalmazás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ed170-105">Traffic Manager directs traffic to the application in one region as the primary region, and to the secondary region when the application in the primary region is unavailable.</span></span> <span data-ttu-id="ed170-106">A parancsprogram végrehajtása előtt módosítania kell a MyWebApp, MyWebAppL1 és MyWebAppL2 értékek egyedi értékeket az egész Azure.</span><span class="sxs-lookup"><span data-stu-id="ed170-106">Before executing the script, you must change the MyWebApp, MyWebAppL1 and MyWebAppL2 values to unique values across Azure.</span></span> <span data-ttu-id="ed170-107">A parancsfájl futtatása után érheti el az alkalmazást az URL-cím mywebapp.trafficmanager.net az elsődleges régióban.</span><span class="sxs-lookup"><span data-stu-id="ed170-107">After running the script, you can access the app in the primary region with the URL mywebapp.trafficmanager.net.</span></span>

<span data-ttu-id="ed170-108">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), majd futtassa a `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="ed170-108">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="ed170-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="ed170-109">Sample script</span></span>

<span data-ttu-id="ed170-110">[!code-powershell[fő](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "forgalmat a magas rendelkezésre állás")]</span><span class="sxs-lookup"><span data-stu-id="ed170-110">[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]</span></span>


<span data-ttu-id="ed170-111">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="ed170-111">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a><span data-ttu-id="ed170-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="ed170-112">Script explanation</span></span>

<span data-ttu-id="ed170-113">A parancsfájl a következő parancsokat egy erőforráscsoport, webalkalmazás, traffic manager-profil és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ed170-113">This script uses the following commands to create a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="ed170-114">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="ed170-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="ed170-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="ed170-115">Command</span></span> | <span data-ttu-id="ed170-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ed170-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ed170-117">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="ed170-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | <span data-ttu-id="ed170-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ed170-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ed170-119">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="ed170-119">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="ed170-120">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ed170-120">Creates an App Service plan.</span></span> <span data-ttu-id="ed170-121">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="ed170-121">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="ed170-122">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="ed170-122">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="ed170-123">Létrehoz egy Azure webalkalmazás az App Service-csomag belül.</span><span class="sxs-lookup"><span data-stu-id="ed170-123">Creates an Azure web app within the App Service plan.</span></span> |
| [<span data-ttu-id="ed170-124">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="ed170-124">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/new-azurermresource) | <span data-ttu-id="ed170-125">Létrehoz egy Azure webalkalmazás az App Service-csomag belül.</span><span class="sxs-lookup"><span data-stu-id="ed170-125">Creates an Azure web app within the App Service plan.</span></span> |
| [<span data-ttu-id="ed170-126">Új AzureRmTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="ed170-126">New-AzureRmTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="ed170-127">Az Azure Traffic Manager-profil létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ed170-127">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="ed170-128">Új AzureRmTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="ed170-128">New-AzureRmTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="ed170-129">Az Azure Traffic Manager-profilt ad hozzá egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="ed170-129">Adds a endpoint to an Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ed170-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ed170-130">Next steps</span></span>

<span data-ttu-id="ed170-131">Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ed170-131">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="ed170-132">További hálózati PowerShell parancsfájl-példák találhatók a [Azure hálózati áttekintés dokumentáció](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ed170-132">Additional networking PowerShell script samples can be found in the [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
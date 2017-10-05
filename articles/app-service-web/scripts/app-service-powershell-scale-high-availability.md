---
title: "Az Azure PowerShell-parancsfájl minta - skálázási világszerte egy magas rendelkezésre állású architektúrával webalkalmazás |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - skálázási világszerte egy magas rendelkezésre állású architektúrával webes alkalmazás"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 470f0129-1efe-462c-a029-5c66e04158a8
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 9acd1cf4d1a5705811c4dedc545505ec0ac55fc7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="dc5ca-103">A webalkalmazás skálázása világszerte egy magas rendelkezésre állású architektúra</span><span class="sxs-lookup"><span data-stu-id="dc5ca-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="dc5ca-104">Ebben a forgatókönyvben létre fog hozni egy erőforráscsoport, két app service-csomagokról, két webes alkalmazásokat, egy traffic manager-profil, illetve két traffic manager-végpont.</span><span class="sxs-lookup"><span data-stu-id="dc5ca-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="dc5ca-105">A gyakorlatban befejezése után egy magas rendelkezésre állású fog architektúra, amely lehetővé teszi, hogy globális alapján a legkisebb hálózati késést a webalkalmazás rendelkezésre állását biztosítja.</span><span class="sxs-lookup"><span data-stu-id="dc5ca-105">Once the exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on the lowest network latency.</span></span>

<span data-ttu-id="dc5ca-106">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="dc5ca-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="dc5ca-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="dc5ca-107">Sample script</span></span>

<span data-ttu-id="dc5ca-108">[!code-powershell[fő](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "egy magas rendelkezésre állású architektúrával világszerte a webalkalmazás skálázása")]</span><span class="sxs-lookup"><span data-stu-id="dc5ca-108">[!code-powershell[main](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "Scale a web app worldwide with a high-availability architecture")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="dc5ca-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="dc5ca-109">Clean up deployment</span></span> 

<span data-ttu-id="dc5ca-110">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="dc5ca-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="dc5ca-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="dc5ca-111">Script explanation</span></span>

<span data-ttu-id="dc5ca-112">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="dc5ca-112">This script uses the following commands.</span></span> <span data-ttu-id="dc5ca-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="dc5ca-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="dc5ca-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="dc5ca-114">Command</span></span> | <span data-ttu-id="dc5ca-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="dc5ca-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="dc5ca-116">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="dc5ca-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="dc5ca-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="dc5ca-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="dc5ca-118">Új AzureRMTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="dc5ca-118">New-AzureRMTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="dc5ca-119">Traffic Manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="dc5ca-119">Creates a Traffic Manager profile.</span></span> |
| [<span data-ttu-id="dc5ca-120">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="dc5ca-120">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="dc5ca-121">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="dc5ca-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="dc5ca-122">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="dc5ca-122">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="dc5ca-123">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dc5ca-123">Creates a web app.</span></span> |
| [<span data-ttu-id="dc5ca-124">Új AzureRMTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="dc5ca-124">New-AzureRMTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="dc5ca-125">Traffic Manager-profil egy végpontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="dc5ca-125">Creates an endpoint in a Traffic Manager profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="dc5ca-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dc5ca-126">Next steps</span></span>

<span data-ttu-id="dc5ca-127">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dc5ca-127">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="dc5ca-128">Azure App Service Web Apps további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="dc5ca-128">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>

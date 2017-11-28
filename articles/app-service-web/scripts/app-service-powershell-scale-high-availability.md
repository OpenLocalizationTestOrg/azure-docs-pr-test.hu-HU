---
title: "PowerShell parancsfájl minta - aaaAzure egy webalkalmazás világszerte egy magas rendelkezésre állású architektúrával skálázása |} Microsoft Docs"
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
ms.openlocfilehash: 1fcda23250efe4966d63c5dfa744b76c26f3762a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="95c49-103">A webalkalmazás skálázása világszerte egy magas rendelkezésre állású architektúra</span><span class="sxs-lookup"><span data-stu-id="95c49-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="95c49-104">Ebben a forgatókönyvben létre fog hozni egy erőforráscsoport, két app service-csomagokról, két webes alkalmazásokat, egy traffic manager-profil, illetve két traffic manager-végpont.</span><span class="sxs-lookup"><span data-stu-id="95c49-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="95c49-105">Hello a gyakorlatban befejezése után egy magas rendelkezésre állású fog architektúra, amely lehetővé teszi a megoldással a globális hello legkisebb hálózati késést alapuló webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="95c49-105">Once hello exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on hello lowest network latency.</span></span>

<span data-ttu-id="95c49-106">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="95c49-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="95c49-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="95c49-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "Scale a web app worldwide with a high-availability architecture")]

## <a name="clean-up-deployment"></a><span data-ttu-id="95c49-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="95c49-108">Clean up deployment</span></span> 

<span data-ttu-id="95c49-109">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="95c49-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="95c49-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="95c49-110">Script explanation</span></span>

<span data-ttu-id="95c49-111">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="95c49-111">This script uses hello following commands.</span></span> <span data-ttu-id="95c49-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="95c49-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="95c49-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="95c49-113">Command</span></span> | <span data-ttu-id="95c49-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="95c49-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="95c49-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="95c49-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="95c49-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="95c49-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="95c49-117">Új AzureRMTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="95c49-117">New-AzureRMTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="95c49-118">Traffic Manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="95c49-118">Creates a Traffic Manager profile.</span></span> |
| [<span data-ttu-id="95c49-119">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="95c49-119">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="95c49-120">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="95c49-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="95c49-121">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="95c49-121">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="95c49-122">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="95c49-122">Creates a web app.</span></span> |
| [<span data-ttu-id="95c49-123">Új AzureRMTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="95c49-123">New-AzureRMTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="95c49-124">Traffic Manager-profil egy végpontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="95c49-124">Creates an endpoint in a Traffic Manager profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="95c49-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="95c49-125">Next steps</span></span>

<span data-ttu-id="95c49-126">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="95c49-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="95c49-127">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="95c49-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>

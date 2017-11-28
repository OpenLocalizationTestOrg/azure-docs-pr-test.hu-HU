---
title: "Az Azure PowerShell-parancsfájl minta - hozzárendelése egy egyéni tartományt |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - hozzárendelése egy egyéni tartományt"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 356f5af9-f62e-411c-8b24-deba05214103
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6d25fe8098848fc69470c77e3200bee554c1f875
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="assign-a-custom-domain-to-a-web-app"></a><span data-ttu-id="01226-103">Rendelje hozzá az egyéni tartománynév a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="01226-103">Assign a custom domain to a web app</span></span>

<span data-ttu-id="01226-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és majd leképezi `www.<yourdomain>` rá.</span><span class="sxs-lookup"><span data-stu-id="01226-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` to it.</span></span> 

<span data-ttu-id="01226-105">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="01226-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span> <span data-ttu-id="01226-106">Is hogy hozzáféréssel kell rendelkeznie a tartományregisztráló DNS-konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="01226-106">Also, you need to have access to your domain registrar's DNS configuration page.</span></span>

## <a name="sample-script"></a><span data-ttu-id="01226-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="01226-107">Sample script</span></span>

<span data-ttu-id="01226-108">[!code-powershell[fő](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "rendelje hozzá az egyéni tartománynév a webes alkalmazás")]</span><span class="sxs-lookup"><span data-stu-id="01226-108">[!code-powershell[main](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "Assign a custom domain to a web app")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="01226-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="01226-109">Clean up deployment</span></span> 

<span data-ttu-id="01226-110">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="01226-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="01226-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="01226-111">Script explanation</span></span>

<span data-ttu-id="01226-112">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="01226-112">This script uses the following commands.</span></span> <span data-ttu-id="01226-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="01226-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="01226-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="01226-114">Command</span></span> | <span data-ttu-id="01226-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="01226-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="01226-116">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="01226-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="01226-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="01226-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="01226-118">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="01226-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="01226-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="01226-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="01226-120">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="01226-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="01226-121">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="01226-121">Creates a web app.</span></span> |
| [<span data-ttu-id="01226-122">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="01226-122">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="01226-123">Módosítja egy App Service-csomag a tarifacsomag-váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="01226-123">Modifies an App Service plan to change its pricing tier.</span></span> |
| [<span data-ttu-id="01226-124">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="01226-124">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="01226-125">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="01226-125">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="01226-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="01226-126">Next steps</span></span>

<span data-ttu-id="01226-127">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="01226-127">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="01226-128">Azure App Service Web Apps további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="01226-128">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>

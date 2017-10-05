---
title: "Az Azure PowerShell-parancsfájl minta - webalkalmazás létrehozása és kód telepítése az átmeneti környezetben való használatra |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - webalkalmazás létrehozása és kód telepítése az átmeneti környezetben való használatra"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 27cf0680-c3a9-4a58-9f71-6dec09f6b874
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 55adc13350eb0f4711efa3c901f6e4e7755dfb27
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a><span data-ttu-id="3dc2a-103">Webalkalmazás létrehozása, majd kód telepítése az átmeneti környezetben való használatra</span><span class="sxs-lookup"><span data-stu-id="3dc2a-103">Create a web app and deploy code to a staging environment</span></span>

<span data-ttu-id="3dc2a-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service egy további üzembe helyezési pont "tesztelés" nevű, és ezután telepíti egy mintaalkalmazást az "előkészítési" pont.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app to the "staging" slot.</span></span>

<span data-ttu-id="3dc2a-105">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="3dc2a-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="3dc2a-106">Sample script</span></span>

<span data-ttu-id="3dc2a-107">[!code-powershell[fő](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "webalkalmazás létrehozása, majd kód telepítése az átmeneti környezetben való használatra")]</span><span class="sxs-lookup"><span data-stu-id="3dc2a-107">[!code-powershell[main](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "Create a web app and deploy code to a staging environment")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="3dc2a-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="3dc2a-108">Clean up deployment</span></span> 

<span data-ttu-id="3dc2a-109">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-109">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="3dc2a-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="3dc2a-110">Script explanation</span></span>

<span data-ttu-id="3dc2a-111">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-111">This script uses the following commands.</span></span> <span data-ttu-id="3dc2a-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="3dc2a-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="3dc2a-113">Command</span></span> | <span data-ttu-id="3dc2a-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="3dc2a-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3dc2a-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="3dc2a-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="3dc2a-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3dc2a-117">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="3dc2a-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="3dc2a-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="3dc2a-119">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="3dc2a-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="3dc2a-120">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-120">Creates a web app.</span></span> |
| [<span data-ttu-id="3dc2a-121">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="3dc2a-121">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="3dc2a-122">Módosítja egy App Service-csomag a tarifacsomag-váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-122">Modifies an App Service plan to change its pricing tier.</span></span> |
| [<span data-ttu-id="3dc2a-123">Új AzureRmWebAppSlot</span><span class="sxs-lookup"><span data-stu-id="3dc2a-123">New-AzureRmWebAppSlot</span></span>](/powershell/module/azurerm.websites/new-azurermwebappslot) | <span data-ttu-id="3dc2a-124">Létrehoz egy üzembe helyezési pont egy webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-124">Creates a deployment slot for a web app.</span></span> |
| [<span data-ttu-id="3dc2a-125">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="3dc2a-125">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="3dc2a-126">Módosítja egy erőforrás egy erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-126">Modifies a resource in a resource group.</span></span> |
| [<span data-ttu-id="3dc2a-127">Lapozófájl-kapacitás-AzureRmWebAppSlot</span><span class="sxs-lookup"><span data-stu-id="3dc2a-127">Swap-AzureRmWebAppSlot</span></span>](/powershell/module/azurerm.websites/swap-azurermwebappslot) | <span data-ttu-id="3dc2a-128">A webes alkalmazás üzembe helyezési pont cseréje éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="3dc2a-128">Swaps a web app's deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3dc2a-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3dc2a-129">Next steps</span></span>

<span data-ttu-id="3dc2a-130">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3dc2a-130">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="3dc2a-131">Azure App Service Web Apps további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="3dc2a-131">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>

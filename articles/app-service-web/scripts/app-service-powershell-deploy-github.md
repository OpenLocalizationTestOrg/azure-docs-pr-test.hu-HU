---
title: "Az Azure PowerShell-parancsfájl minta - webalkalmazás létrehozása és központi telepítése a Githubból kód |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - webalkalmazás létrehozása és központi telepítése a Githubból kódot"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 1f7fc21dc12c334f5d347ae9918bd62945bede64
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-and-deploy-code-from-github"></a><span data-ttu-id="1cea6-103">A webalkalmazás létrehozása és telepítése a Githubból kód</span><span class="sxs-lookup"><span data-stu-id="1cea6-103">Create a web app and deploy code from GitHub</span></span>

<span data-ttu-id="1cea6-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és majd központilag telepíti a webes alkalmazás kód egy nyilvános GitHub-adattár (folyamatos üzembe helyezés) nélkül.</span><span class="sxs-lookup"><span data-stu-id="1cea6-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="1cea6-105">Lásd a GitHub-telepítéshez olyan folyamatos üzembe helyezés, [webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról](app-service-powershell-continuous-deployment-github.md).</span><span class="sxs-lookup"><span data-stu-id="1cea6-105">For GitHub deployment with continuous deployment, see [Create a web app with continuous deployment from GitHub](app-service-powershell-continuous-deployment-github.md).</span></span>

<span data-ttu-id="1cea6-106">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1cea6-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="1cea6-107">Is kell a webes alkalmazás kódot tartalmazó GitHub-tárházban mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="1cea6-107">Also, you need a link to GitHub repository that contains the web app code.</span></span>

## <a name="sample-script"></a><span data-ttu-id="1cea6-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="1cea6-108">Sample script</span></span>

<span data-ttu-id="1cea6-109">[!code-powershell[fő](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "webalkalmazás létrehozása és központi telepítése a Githubból kódot")]</span><span class="sxs-lookup"><span data-stu-id="1cea6-109">[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "Create a web app and deploy code from GitHub")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="1cea6-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1cea6-110">Clean up deployment</span></span> 

<span data-ttu-id="1cea6-111">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="1cea6-111">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="1cea6-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="1cea6-112">Script explanation</span></span>

<span data-ttu-id="1cea6-113">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="1cea6-113">This script uses the following commands.</span></span> <span data-ttu-id="1cea6-114">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="1cea6-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="1cea6-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="1cea6-115">Command</span></span> | <span data-ttu-id="1cea6-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1cea6-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1cea6-117">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1cea6-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="1cea6-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1cea6-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1cea6-119">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="1cea6-119">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="1cea6-120">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1cea6-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="1cea6-121">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="1cea6-121">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="1cea6-122">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1cea6-122">Creates a web app.</span></span> |
| [<span data-ttu-id="1cea6-123">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="1cea6-123">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="1cea6-124">Módosítja egy erőforrás egy erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="1cea6-124">Modifies a resource in a resource group.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1cea6-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1cea6-125">Next steps</span></span>

<span data-ttu-id="1cea6-126">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1cea6-126">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="1cea6-127">Azure App Service Web Apps további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="1cea6-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>

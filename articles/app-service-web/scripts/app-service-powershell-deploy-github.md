---
title: "aaaAzure PowerShell parancsfájl minta - webalkalmazás létrehozása és központi telepítése a Githubból kód |} Microsoft Docs"
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
ms.openlocfilehash: 9a28f9cb01b71c86fa0a3f1d0a6761fc3d45d43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-github"></a><span data-ttu-id="b5ca6-103">A webalkalmazás létrehozása és telepítése a Githubból kód</span><span class="sxs-lookup"><span data-stu-id="b5ca6-103">Create a web app and deploy code from GitHub</span></span>

<span data-ttu-id="b5ca6-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és majd központilag telepíti a webes alkalmazás kód egy nyilvános GitHub-adattár (folyamatos üzembe helyezés) nélkül.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="b5ca6-105">Lásd a GitHub-telepítéshez olyan folyamatos üzembe helyezés, [webalkalmazás létrehozása a folyamatos üzembe helyezés a Githubról](app-service-powershell-continuous-deployment-github.md).</span><span class="sxs-lookup"><span data-stu-id="b5ca6-105">For GitHub deployment with continuous deployment, see [Create a web app with continuous deployment from GitHub](app-service-powershell-continuous-deployment-github.md).</span></span>

<span data-ttu-id="b5ca6-106">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b5ca6-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="b5ca6-107">Is kell a hivatkozás tooGitHub tárház hello web app kódot tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-107">Also, you need a link tooGitHub repository that contains hello web app code.</span></span>

## <a name="sample-script"></a><span data-ttu-id="b5ca6-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="b5ca6-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "Create a web app and deploy code from GitHub")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b5ca6-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b5ca6-109">Clean up deployment</span></span> 

<span data-ttu-id="b5ca6-110">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-110">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="b5ca6-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="b5ca6-111">Script explanation</span></span>

<span data-ttu-id="b5ca6-112">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-112">This script uses hello following commands.</span></span> <span data-ttu-id="b5ca6-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b5ca6-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="b5ca6-114">Command</span></span> | <span data-ttu-id="b5ca6-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b5ca6-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b5ca6-116">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b5ca6-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="b5ca6-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b5ca6-118">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="b5ca6-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="b5ca6-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="b5ca6-120">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="b5ca6-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="b5ca6-121">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-121">Creates a web app.</span></span> |
| [<span data-ttu-id="b5ca6-122">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="b5ca6-122">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="b5ca6-123">Módosítja egy erőforrás egy erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-123">Modifies a resource in a resource group.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b5ca6-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b5ca6-124">Next steps</span></span>

<span data-ttu-id="b5ca6-125">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b5ca6-125">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="b5ca6-126">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b5ca6-126">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>

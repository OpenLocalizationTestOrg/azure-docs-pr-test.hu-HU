---
title: "aaaAzure PowerShell parancsfájl minta - webalkalmazás létrehozása és központi telepítése átmeneti környezet kód tooa |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - webalkalmazás létrehozása és központi telepítése átmeneti környezet kód tooa"
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
ms.openlocfilehash: 5c74b962955770637173f1fd4f49342fec54ae3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-tooa-staging-environment"></a><span data-ttu-id="5bd47-103">Webalkalmazás létrehozása és központi telepítése átmeneti környezet kód tooa</span><span class="sxs-lookup"><span data-stu-id="5bd47-103">Create a web app and deploy code tooa staging environment</span></span>

<span data-ttu-id="5bd47-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service egy további üzembe helyezési pont "tesztelés" nevezik, és majd központilag telepíti egy minta app toohello "staging" tárolóhely.</span><span class="sxs-lookup"><span data-stu-id="5bd47-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app toohello "staging" slot.</span></span>

<span data-ttu-id="5bd47-105">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="5bd47-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="5bd47-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="5bd47-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "Create a web app and deploy code tooa staging environment")]

## <a name="clean-up-deployment"></a><span data-ttu-id="5bd47-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5bd47-107">Clean up deployment</span></span> 

<span data-ttu-id="5bd47-108">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="5bd47-108">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="5bd47-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="5bd47-109">Script explanation</span></span>

<span data-ttu-id="5bd47-110">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="5bd47-110">This script uses hello following commands.</span></span> <span data-ttu-id="5bd47-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5bd47-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5bd47-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="5bd47-112">Command</span></span> | <span data-ttu-id="5bd47-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5bd47-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5bd47-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="5bd47-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="5bd47-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5bd47-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5bd47-116">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="5bd47-116">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="5bd47-117">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5bd47-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="5bd47-118">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="5bd47-118">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="5bd47-119">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5bd47-119">Creates a web app.</span></span> |
| [<span data-ttu-id="5bd47-120">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="5bd47-120">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="5bd47-121">Az App Service-csomag toochange a tarifacsomag módosítása.</span><span class="sxs-lookup"><span data-stu-id="5bd47-121">Modifies an App Service plan toochange its pricing tier.</span></span> |
| [<span data-ttu-id="5bd47-122">Új AzureRmWebAppSlot</span><span class="sxs-lookup"><span data-stu-id="5bd47-122">New-AzureRmWebAppSlot</span></span>](/powershell/module/azurerm.websites/new-azurermwebappslot) | <span data-ttu-id="5bd47-123">Létrehoz egy üzembe helyezési pont egy webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5bd47-123">Creates a deployment slot for a web app.</span></span> |
| [<span data-ttu-id="5bd47-124">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="5bd47-124">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/set-azurermresource) | <span data-ttu-id="5bd47-125">Módosítja egy erőforrás egy erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="5bd47-125">Modifies a resource in a resource group.</span></span> |
| [<span data-ttu-id="5bd47-126">Lapozófájl-kapacitás-AzureRmWebAppSlot</span><span class="sxs-lookup"><span data-stu-id="5bd47-126">Swap-AzureRmWebAppSlot</span></span>](/powershell/module/azurerm.websites/swap-azurermwebappslot) | <span data-ttu-id="5bd47-127">A webes alkalmazás üzembe helyezési pont cseréje éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5bd47-127">Swaps a web app's deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5bd47-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5bd47-128">Next steps</span></span>

<span data-ttu-id="5bd47-129">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5bd47-129">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="5bd47-130">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5bd47-130">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>

---
title: "aaaAzure PowerShell parancsfájl minta - hozzárendelése egyéni tartományhoz tooa webalkalmazás |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - hozzárendelése egyéni tartományhoz tooa webes alkalmazás"
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
ms.openlocfilehash: 10224e800588019626ef25cbba4a926096779920
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-custom-domain-tooa-web-app"></a><span data-ttu-id="5a321-103">Rendelje hozzá az egyéni tartomány tooa webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5a321-103">Assign a custom domain tooa web app</span></span>

<span data-ttu-id="5a321-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és majd leképezi `www.<yourdomain>` tooit.</span><span class="sxs-lookup"><span data-stu-id="5a321-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` tooit.</span></span> 

<span data-ttu-id="5a321-105">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="5a321-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> <span data-ttu-id="5a321-106">Is kell toohave hozzáférés tooyour tartomány regisztráló DNS-konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="5a321-106">Also, you need toohave access tooyour domain registrar's DNS configuration page.</span></span>

## <a name="sample-script"></a><span data-ttu-id="5a321-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="5a321-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "Assign a custom domain tooa web app")]

## <a name="clean-up-deployment"></a><span data-ttu-id="5a321-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5a321-108">Clean up deployment</span></span> 

<span data-ttu-id="5a321-109">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="5a321-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="5a321-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="5a321-110">Script explanation</span></span>

<span data-ttu-id="5a321-111">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="5a321-111">This script uses hello following commands.</span></span> <span data-ttu-id="5a321-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5a321-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5a321-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="5a321-113">Command</span></span> | <span data-ttu-id="5a321-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5a321-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5a321-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="5a321-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="5a321-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5a321-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5a321-117">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="5a321-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="5a321-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5a321-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="5a321-119">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="5a321-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="5a321-120">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5a321-120">Creates a web app.</span></span> |
| [<span data-ttu-id="5a321-121">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="5a321-121">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="5a321-122">Az App Service-csomag toochange a tarifacsomag módosítása.</span><span class="sxs-lookup"><span data-stu-id="5a321-122">Modifies an App Service plan toochange its pricing tier.</span></span> |
| [<span data-ttu-id="5a321-123">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="5a321-123">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="5a321-124">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="5a321-124">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5a321-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a321-125">Next steps</span></span>

<span data-ttu-id="5a321-126">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5a321-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="5a321-127">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5a321-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>

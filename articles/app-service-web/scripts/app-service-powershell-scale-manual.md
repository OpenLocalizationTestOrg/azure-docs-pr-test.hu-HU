---
title: "PowerShell parancsfájl minta - aaaAzure manuálisan a webalkalmazás skálázása |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - webalkalmazás skálázása manuálisan"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: de5d4285-9c7d-4735-a695-288264047375
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c749031fbe6c6bcbb25395387b4f32b2ba75cef4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="6e37d-103">A webalkalmazás skálázása manuálisan</span><span class="sxs-lookup"><span data-stu-id="6e37d-103">Scale a web app manually</span></span>

<span data-ttu-id="6e37d-104">Ebben a forgatókönyvben egy erőforráscsoport, toocreate megtanulhatja app service-csomag és a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6e37d-104">In this scenario you will learn toocreate a resource group, app service plan and web app.</span></span> <span data-ttu-id="6e37d-105">Ezután a hello App Service-csomag egy egypéldányos toomultiple példányokból lesz skálázva.</span><span class="sxs-lookup"><span data-stu-id="6e37d-105">You will then scale hello App Service Plan from a single instance toomultiple instances.</span></span>

<span data-ttu-id="6e37d-106">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="6e37d-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="6e37d-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="6e37d-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/scale-manual/scale-manual.ps1 "Scale a web app manually")]

## <a name="clean-up-deployment"></a><span data-ttu-id="6e37d-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6e37d-108">Clean up deployment</span></span> 

<span data-ttu-id="6e37d-109">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="6e37d-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="6e37d-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="6e37d-110">Script explanation</span></span>

<span data-ttu-id="6e37d-111">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="6e37d-111">This script uses hello following commands.</span></span> <span data-ttu-id="6e37d-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6e37d-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="6e37d-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="6e37d-113">Command</span></span> | <span data-ttu-id="6e37d-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6e37d-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6e37d-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="6e37d-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="6e37d-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6e37d-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6e37d-117">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="6e37d-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="6e37d-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6e37d-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="6e37d-119">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="6e37d-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="6e37d-120">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6e37d-120">Creates a web app.</span></span> |
| [<span data-ttu-id="6e37d-121">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="6e37d-121">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="6e37d-122">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="6e37d-122">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6e37d-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e37d-123">Next steps</span></span>

<span data-ttu-id="6e37d-124">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6e37d-124">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="6e37d-125">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="6e37d-125">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
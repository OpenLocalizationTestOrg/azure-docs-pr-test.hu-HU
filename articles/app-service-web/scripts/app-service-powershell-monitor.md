---
title: "PowerShell parancsfájl minta - aaaAzure figyelése a webes alkalmazás a webkiszolgáló naplóinak |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - figyelő a webes alkalmazás a webkiszolgáló naplóinak"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5805d7cd-9e56-4eba-bd85-75b013690ff5
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: efaae1e19f5153e33d1f5d5decadb9f6c4649f8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a><span data-ttu-id="addaf-103">A webkiszolgáló naplóinak webes alkalmazás figyelése</span><span class="sxs-lookup"><span data-stu-id="addaf-103">Monitor a web app with web server logs</span></span>

<span data-ttu-id="addaf-104">Ebben a forgatókönyvben létrehoz egy erőforráscsoport, az app service-csomag, a web app és hello web app tooenable webkiszolgáló naplóinak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="addaf-104">In this scenario you will create a resource group, app service plan, web app and configure hello web app tooenable web server logs.</span></span> <span data-ttu-id="addaf-105">Tekintse át a hello naplófájlokban majd letölti.</span><span class="sxs-lookup"><span data-stu-id="addaf-105">You will then download hello log files for review.</span></span>

<span data-ttu-id="addaf-106">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="addaf-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="addaf-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="addaf-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/monitor-with-logs/monitor-with-logs.ps1 "Monitor a web app with web server logs")]

## <a name="clean-up-deployment"></a><span data-ttu-id="addaf-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="addaf-108">Clean up deployment</span></span> 

<span data-ttu-id="addaf-109">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="addaf-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="addaf-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="addaf-110">Script explanation</span></span>

<span data-ttu-id="addaf-111">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="addaf-111">This script uses hello following commands.</span></span> <span data-ttu-id="addaf-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="addaf-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="addaf-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="addaf-113">Command</span></span> | <span data-ttu-id="addaf-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="addaf-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="addaf-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="addaf-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="addaf-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="addaf-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="addaf-117">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="addaf-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="addaf-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="addaf-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="addaf-119">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="addaf-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="addaf-120">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="addaf-120">Creates a web app.</span></span> |
| [<span data-ttu-id="addaf-121">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="addaf-121">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="addaf-122">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="addaf-122">Modifies a web app's configuration.</span></span> |
| [<span data-ttu-id="addaf-123">Get-AzureRMWebAppMetrics</span><span class="sxs-lookup"><span data-stu-id="addaf-123">Get-AzureRMWebAppMetrics</span></span>](/powershell/module/azurerm.websites/get-azurermwebappmetrics) | <span data-ttu-id="addaf-124">A webes alkalmazás metrikák beolvasása.</span><span class="sxs-lookup"><span data-stu-id="addaf-124">Gets a web app's metrics.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="addaf-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="addaf-125">Next steps</span></span>

<span data-ttu-id="addaf-126">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="addaf-126">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="addaf-127">Azure App Service Web Apps további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="addaf-127">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>

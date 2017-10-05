---
title: "Az Azure PowerShell-parancsfájl minta - figyelő a webes alkalmazás a webkiszolgáló naplóinak |} Microsoft Docs"
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
ms.openlocfilehash: 34a3dd318cb9896342fce870922ecd113b3ed08d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a><span data-ttu-id="d8e70-103">A webkiszolgáló naplóinak webes alkalmazás figyelése</span><span class="sxs-lookup"><span data-stu-id="d8e70-103">Monitor a web app with web server logs</span></span>

<span data-ttu-id="d8e70-104">Ebben a forgatókönyvben létrehoz egy erőforráscsoport, az app service-csomag, a webes alkalmazás, és konfigurálja a webalkalmazás a webkiszolgáló naplóinak engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d8e70-104">In this scenario you will create a resource group, app service plan, web app and configure the web app to enable web server logs.</span></span> <span data-ttu-id="d8e70-105">Tekintse át a naplófájlokat, majd letölti azokat.</span><span class="sxs-lookup"><span data-stu-id="d8e70-105">You will then download the log files for review.</span></span>

<span data-ttu-id="d8e70-106">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview), majd futtassa a `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="d8e70-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="d8e70-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d8e70-107">Sample script</span></span>

<span data-ttu-id="d8e70-108">[!code-powershell[fő](../../../powershell_scripts/app-service/monitor-with-logs/monitor-with-logs.ps1 "a webkiszolgáló naplóinak webes alkalmazás figyelése")]</span><span class="sxs-lookup"><span data-stu-id="d8e70-108">[!code-powershell[main](../../../powershell_scripts/app-service/monitor-with-logs/monitor-with-logs.ps1 "Monitor a web app with web server logs")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d8e70-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d8e70-109">Clean up deployment</span></span> 

<span data-ttu-id="d8e70-110">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a web app és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="d8e70-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="d8e70-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d8e70-111">Script explanation</span></span>

<span data-ttu-id="d8e70-112">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="d8e70-112">This script uses the following commands.</span></span> <span data-ttu-id="d8e70-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="d8e70-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d8e70-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="d8e70-114">Command</span></span> | <span data-ttu-id="d8e70-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d8e70-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d8e70-116">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d8e70-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="d8e70-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d8e70-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d8e70-118">Új AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="d8e70-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="d8e70-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d8e70-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="d8e70-120">Új AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="d8e70-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="d8e70-121">Létrehoz egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d8e70-121">Creates a web app.</span></span> |
| [<span data-ttu-id="d8e70-122">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="d8e70-122">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="d8e70-123">A webalkalmazás konfigurációs módosítja.</span><span class="sxs-lookup"><span data-stu-id="d8e70-123">Modifies a web app's configuration.</span></span> |
| [<span data-ttu-id="d8e70-124">Get-AzureRMWebAppMetrics</span><span class="sxs-lookup"><span data-stu-id="d8e70-124">Get-AzureRMWebAppMetrics</span></span>](/powershell/module/azurerm.websites/get-azurermwebappmetrics) | <span data-ttu-id="d8e70-125">A webes alkalmazás metrikák beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d8e70-125">Gets a web app's metrics.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d8e70-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d8e70-126">Next steps</span></span>

<span data-ttu-id="d8e70-127">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d8e70-127">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="d8e70-128">Azure App Service Web Apps további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d8e70-128">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>

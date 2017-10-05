---
title: "Az Azure PowerShell-parancsfájl minta - frissítés a Service Fabric-alkalmazás |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - frissítés a Service Fabric-alkalmazás."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 454849f82ddb23ddb9d71459f86e3cf5a1589254
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upgrade-a-service-fabric-application"></a><span data-ttu-id="61c63-103">A Service Fabric-alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="61c63-103">Upgrade a Service Fabric application</span></span>

<span data-ttu-id="61c63-104">Ez a parancsfájlpélda futó Service Fabric-alkalmazás példány 1.3.0 verzióra frissíti.</span><span class="sxs-lookup"><span data-stu-id="61c63-104">This sample script upgrades a running Service Fabric application instance to version 1.3.0.</span></span> <span data-ttu-id="61c63-105">A parancsfájl másolja az új alkalmazáscsomagot a fürt lemezképtárolóhoz, az alkalmazástípus regisztrálása, a figyelt frissítés elindul, és folyamatosan ellenőrzi a frissítési állapot, amíg nem fejeződik a frissítés, vagy visszaállítja a.</span><span class="sxs-lookup"><span data-stu-id="61c63-105">The script copies the new application package to the cluster image store, registers the application type, starts a monitored upgrade, and continuously checks the upgrade status until the upgrade completes or rolls back.</span></span> <span data-ttu-id="61c63-106">A paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="61c63-106">Customize the parameters as needed.</span></span> 

<span data-ttu-id="61c63-107">Szükség esetén telepítse a Service Fabric PowerShell-modulját és a [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="61c63-107">If needed, install the Service Fabric PowerShell module with the [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="61c63-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="61c63-108">Sample script</span></span>

<span data-ttu-id="61c63-109">[!code-powershell[fő](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "alkalmazás frissítése")]</span><span class="sxs-lookup"><span data-stu-id="61c63-109">[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="61c63-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="61c63-110">Script explanation</span></span>

<span data-ttu-id="61c63-111">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="61c63-111">This script uses the following commands.</span></span> <span data-ttu-id="61c63-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="61c63-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="61c63-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="61c63-113">Command</span></span> | <span data-ttu-id="61c63-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="61c63-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="61c63-115">Get-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="61c63-115">Get-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="61c63-116">Lekérdezi a Service Fabric-fürt összes alkalmazást vagy egy adott alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="61c63-116">Gets all the applications in the Service Fabric cluster or a specific application.</span></span>  |
| [<span data-ttu-id="61c63-117">Get-ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="61c63-117">Get-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="61c63-118">A Service Fabric-alkalmazás frissítés állapotát olvassa be.</span><span class="sxs-lookup"><span data-stu-id="61c63-118">Gets the status of a Service Fabric application upgrade.</span></span> |
| [<span data-ttu-id="61c63-119">Get-ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="61c63-119">Get-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="61c63-120">A Service Fabric a Service Fabric-fürt regisztrált típusok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="61c63-120">Gets the Service Fabric application types registered on the Service Fabric cluster.</span></span> |
| [<span data-ttu-id="61c63-121">ServiceFabricApplicationType regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="61c63-121">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="61c63-122">A Service Fabric alkalmazástípus regisztrációjának törlése.</span><span class="sxs-lookup"><span data-stu-id="61c63-122">Unregisters a Service Fabric application type.</span></span>  |
| [<span data-ttu-id="61c63-123">Másolás-ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="61c63-123">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="61c63-124">A Service Fabric alkalmazáscsomagok másolja az image store.</span><span class="sxs-lookup"><span data-stu-id="61c63-124">Copies a Service Fabric application package to the image store.</span></span>  |
| [<span data-ttu-id="61c63-125">Register-ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="61c63-125">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="61c63-126">Regisztrálja a Service Fabric-alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="61c63-126">Registers a Service Fabric application type.</span></span> |
| [<span data-ttu-id="61c63-127">Start-ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="61c63-127">Start-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="61c63-128">A Service Fabric-alkalmazás a megadott alkalmazás típusa verzióra frissíti.</span><span class="sxs-lookup"><span data-stu-id="61c63-128">Upgrades a Service Fabric application to the specified application type version.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="61c63-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61c63-129">Next steps</span></span>

<span data-ttu-id="61c63-130">További információ a Service Fabric PowerShell-modul: [Azure PowerShell dokumentációs](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="61c63-130">For more information on the Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="61c63-131">Azure Service Fabric további Powershell-példák találhatók a [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="61c63-131">Additional Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>

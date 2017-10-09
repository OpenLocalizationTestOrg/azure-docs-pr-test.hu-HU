---
title: "PowerShell parancsfájl minta - aaaAzure frissítése a Service Fabric-alkalmazás |} Microsoft Docs"
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
ms.openlocfilehash: 4f4777607bd6b35a76029e09ddb441006565d4cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-service-fabric-application"></a><span data-ttu-id="e0731-103">A Service Fabric-alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="e0731-103">Upgrade a Service Fabric application</span></span>

<span data-ttu-id="e0731-104">Ez a parancsfájlpélda egy futó Service Fabric application példány tooversion 1.3.0 frissíti.</span><span class="sxs-lookup"><span data-stu-id="e0731-104">This sample script upgrades a running Service Fabric application instance tooversion 1.3.0.</span></span> <span data-ttu-id="e0731-105">hello parancsfájl hello új alkalmazás csomag toohello fürt lemezképtárolóhoz másolja, hello alkalmazástípus regisztrálása, a figyelt frissítés kezdődik, és folyamatosan ellenőrzi az hello frissítési állapot, amíg hello frissítése befejeződik, vagy visszaállítja a.</span><span class="sxs-lookup"><span data-stu-id="e0731-105">hello script copies hello new application package toohello cluster image store, registers hello application type, starts a monitored upgrade, and continuously checks hello upgrade status until hello upgrade completes or rolls back.</span></span> <span data-ttu-id="e0731-106">Hello paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="e0731-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="e0731-107">Szükség esetén – hello Service Fabric PowerShell-modul telepítése a hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e0731-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="e0731-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="e0731-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a><span data-ttu-id="e0731-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="e0731-109">Script explanation</span></span>

<span data-ttu-id="e0731-110">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="e0731-110">This script uses hello following commands.</span></span> <span data-ttu-id="e0731-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="e0731-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="e0731-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="e0731-112">Command</span></span> | <span data-ttu-id="e0731-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e0731-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e0731-114">Get-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="e0731-114">Get-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="e0731-115">Lekérdezi a hello Service Fabric-fürt összes hello alkalmazást vagy egy adott alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="e0731-115">Gets all hello applications in hello Service Fabric cluster or a specific application.</span></span>  |
| [<span data-ttu-id="e0731-116">Get-ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="e0731-116">Get-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="e0731-117">A Service Fabric-alkalmazás frissítés hello állapotának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e0731-117">Gets hello status of a Service Fabric application upgrade.</span></span> |
| [<span data-ttu-id="e0731-118">Get-ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="e0731-118">Get-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="e0731-119">Lekérdezi a Service Fabric típusok hello hello Service Fabric-fürt regisztrált.</span><span class="sxs-lookup"><span data-stu-id="e0731-119">Gets hello Service Fabric application types registered on hello Service Fabric cluster.</span></span> |
| [<span data-ttu-id="e0731-120">ServiceFabricApplicationType regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="e0731-120">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="e0731-121">A Service Fabric alkalmazástípus regisztrációjának törlése.</span><span class="sxs-lookup"><span data-stu-id="e0731-121">Unregisters a Service Fabric application type.</span></span>  |
| [<span data-ttu-id="e0731-122">Másolás-ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="e0731-122">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="e0731-123">Másolja a Service Fabric application toohello lemezképet tárolja.</span><span class="sxs-lookup"><span data-stu-id="e0731-123">Copies a Service Fabric application package toohello image store.</span></span>  |
| [<span data-ttu-id="e0731-124">Register-ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="e0731-124">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="e0731-125">Regisztrálja a Service Fabric-alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="e0731-125">Registers a Service Fabric application type.</span></span> |
| [<span data-ttu-id="e0731-126">Start-ServiceFabricApplicationUpgrade</span><span class="sxs-lookup"><span data-stu-id="e0731-126">Start-ServiceFabricApplicationUpgrade</span></span>](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | <span data-ttu-id="e0731-127">A Service Fabric toohello megadott alkalmazás alkalmazástípus verziója frissíti.</span><span class="sxs-lookup"><span data-stu-id="e0731-127">Upgrades a Service Fabric application toohello specified application type version.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="e0731-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0731-128">Next steps</span></span>

<span data-ttu-id="e0731-129">A Service Fabric PowerShell-modul hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="e0731-129">For more information on hello Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="e0731-130">Azure Service Fabric további Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e0731-130">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>

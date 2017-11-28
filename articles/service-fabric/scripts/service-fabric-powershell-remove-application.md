---
title: "PowerShell parancsfájl minta - fürtök törlése alkalmazás aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - alkalmazás a Service Fabric-fürt eltávolítása."
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
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 3fe2082c2fbeffbff1622f206021d4d907197d19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="610eb-103">Alkalmazások eltávolítása a Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="610eb-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="610eb-104">Ez a parancsfájlpélda futó Service Fabric-alkalmazás példány törlése megszünteti az alkalmazástípus és -verzió hello fürtről és hello alkalmazáscsomag törlése hello fürt lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="610eb-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from hello cluster, and deletes hello application package from hello cluster image store.</span></span>  <span data-ttu-id="610eb-105">Törlése hello alkalmazáspéldány fut az adott alkalmazáshoz kapcsolódó szolgáltatáspéldány összes hello is törli.</span><span class="sxs-lookup"><span data-stu-id="610eb-105">Deleting hello application instance also deletes all hello running service instances associated with that application.</span></span> <span data-ttu-id="610eb-106">Hello paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="610eb-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="610eb-107">Szükség esetén – hello Service Fabric PowerShell-modul telepítése a hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="610eb-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="610eb-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="610eb-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a><span data-ttu-id="610eb-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="610eb-109">Script explanation</span></span>

<span data-ttu-id="610eb-110">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="610eb-110">This script uses hello following commands.</span></span> <span data-ttu-id="610eb-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="610eb-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="610eb-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="610eb-112">Command</span></span> | <span data-ttu-id="610eb-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="610eb-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="610eb-114">Remove-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="610eb-114">Remove-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | <span data-ttu-id="610eb-115">Eltávolítja egy futó Service Fabric-alkalmazás példány hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="610eb-115">Removes a running Service Fabric application instance from hello cluster.</span></span>  |
| [<span data-ttu-id="610eb-116">ServiceFabricApplicationType regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="610eb-116">Unregister-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | <span data-ttu-id="610eb-117">Megszünteti a Service Fabric alkalmazástípus és -verzió hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="610eb-117">Unregisters a Service Fabric application type and version from hello cluster.</span></span> |
| [<span data-ttu-id="610eb-118">Remove-ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="610eb-118">Remove-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="610eb-119">A Service Fabric alkalmazáscsomag eltávolítása hello lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="610eb-119">Removes a Service Fabric application package from hello image store.</span></span>|

## <a name="next-steps"></a><span data-ttu-id="610eb-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="610eb-120">Next steps</span></span>

<span data-ttu-id="610eb-121">A Service Fabric PowerShell-modul hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="610eb-121">For more information on hello Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="610eb-122">Azure Service Fabric további Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="610eb-122">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>

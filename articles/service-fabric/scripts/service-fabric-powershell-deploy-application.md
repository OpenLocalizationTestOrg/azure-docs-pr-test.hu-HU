---
title: "PowerShell parancsfájl minta - aaaAzure alkalmazás tooa fürt központi telepítése |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - alkalmazás tooa Service Fabric-fürt üzembe helyezése."
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
ms.openlocfilehash: b417c9908c72f016e930c43ff2d13e0cc5451f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a><span data-ttu-id="13372-103">Alkalmazás tooa Service Fabric-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="13372-103">Deploy an application tooa Service Fabric cluster</span></span>

<span data-ttu-id="13372-104">Ez a parancsfájlpélda másolja át az alkalmazás csomag tooa fürt lemezképtárolóhoz, hello alkalmazástípus regisztrálása hello fürt, és létrehoz egy alkalmazáspéldányt hello alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="13372-104">This sample script copies an application package tooa cluster image store, registers hello application type in hello cluster, and creates an application instance from hello application type.</span></span>  <span data-ttu-id="13372-105">Ha az alapértelmezett szolgáltatások hello célalkalmazás típusa hello alkalmazás jegyzékében meghatározott, akkor ezek a szolgáltatások jelenleg jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="13372-105">If any default services were defined in hello application manifest of hello target application type, then those services are created at this time.</span></span> <span data-ttu-id="13372-106">Hello paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="13372-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="13372-107">Szükség esetén – hello Service Fabric PowerShell-modul telepítése a hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="13372-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="13372-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="13372-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a><span data-ttu-id="13372-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="13372-109">Clean up deployment</span></span> 

<span data-ttu-id="13372-110">Hello parancsfájl minta futtatása után a parancsfájl hello [alkalmazás eltávolítása](service-fabric-powershell-remove-application.md) is használt tooremove hello alkalmazáspéldány kell hello alkalmazástípus regisztrációjának törlése és hello alkalmazáscsomag törlése hello lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="13372-110">After hello script sample has been run, hello script in [Remove an application](service-fabric-powershell-remove-application.md) can be used tooremove hello application instance, unregister hello application type, and delete hello application package from hello image store.</span></span>

## <a name="script-explanation"></a><span data-ttu-id="13372-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="13372-111">Script explanation</span></span>

<span data-ttu-id="13372-112">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="13372-112">This script uses hello following commands.</span></span> <span data-ttu-id="13372-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="13372-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="13372-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="13372-114">Command</span></span> | <span data-ttu-id="13372-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="13372-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="13372-116">Másolás-ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="13372-116">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="13372-117">Másolja az alkalmazás csomag toohello fürt lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="13372-117">Copy an application package toohello cluster image store.</span></span>  |
|[<span data-ttu-id="13372-118">Register-ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="13372-118">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| <span data-ttu-id="13372-119">Regisztrálja az alkalmazástípus és -verzió hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="13372-119">Registers an application type and version on hello cluster.</span></span> |
|[<span data-ttu-id="13372-120">Új ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="13372-120">New-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| <span data-ttu-id="13372-121">Alkalmazást hoz létre a regisztrált alkalmazáshoz típusból.</span><span class="sxs-lookup"><span data-stu-id="13372-121">Creates an application from a registered application type.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="13372-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13372-122">Next steps</span></span>

<span data-ttu-id="13372-123">A Service Fabric PowerShell-modul hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="13372-123">For more information on hello Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="13372-124">Azure Service Fabric további Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="13372-124">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>

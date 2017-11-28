---
title: "Az Azure PowerShell-parancsfájl minta - fürtre alkalmazás központi telepítése |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - alkalmazás üzembe helyezése a Service Fabric-fürt."
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
ms.openlocfilehash: 2863823205dbd70f63948ecd4af8898220fe1ff8
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a><span data-ttu-id="21e51-103">A Service Fabric-fürt alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="21e51-103">Deploy an application to a Service Fabric cluster</span></span>

<span data-ttu-id="21e51-104">Ez a parancsfájlpélda másolja egy alkalmazáscsomagot a fürt lemezképtárolóhoz, az alkalmazástípus regisztrálása a fürtben, és létrehoz egy alkalmazáspéldányt az alkalmazástípus.</span><span class="sxs-lookup"><span data-stu-id="21e51-104">This sample script copies an application package to a cluster image store, registers the application type in the cluster, and creates an application instance from the application type.</span></span>  <span data-ttu-id="21e51-105">Ha a célalkalmazás típusa, az alkalmazás jegyzékében meghatározott alapértelmezett szolgáltatások, akkor ezek a szolgáltatások jelenleg jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="21e51-105">If any default services were defined in the application manifest of the target application type, then those services are created at this time.</span></span> <span data-ttu-id="21e51-106">A paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="21e51-106">Customize the parameters as needed.</span></span> 

<span data-ttu-id="21e51-107">Szükség esetén telepítse a Service Fabric PowerShell-modulját és a [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="21e51-107">If needed, install the Service Fabric PowerShell module with the [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="21e51-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="21e51-108">Sample script</span></span>

<span data-ttu-id="21e51-109">[!code-powershell[fő](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "fürtre alkalmazás központi telepítése")]</span><span class="sxs-lookup"><span data-stu-id="21e51-109">[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application to a cluster")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="21e51-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="21e51-110">Clean up deployment</span></span> 

<span data-ttu-id="21e51-111">A parancsfájl-minta futtatása után, a parancsfájl [alkalmazás eltávolítása](service-fabric-powershell-remove-application.md) az alkalmazáspéldány eltávolítása az alkalmazástípus regisztrációjának törlése és az alkalmazáscsomag törlése az image store használható.</span><span class="sxs-lookup"><span data-stu-id="21e51-111">After the script sample has been run, the script in [Remove an application](service-fabric-powershell-remove-application.md) can be used to remove the application instance, unregister the application type, and delete the application package from the image store.</span></span>

## <a name="script-explanation"></a><span data-ttu-id="21e51-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="21e51-112">Script explanation</span></span>

<span data-ttu-id="21e51-113">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="21e51-113">This script uses the following commands.</span></span> <span data-ttu-id="21e51-114">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="21e51-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="21e51-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="21e51-115">Command</span></span> | <span data-ttu-id="21e51-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="21e51-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="21e51-117">Másolás-ServiceFabricApplicationPackage</span><span class="sxs-lookup"><span data-stu-id="21e51-117">Copy-ServiceFabricApplicationPackage</span></span>](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | <span data-ttu-id="21e51-118">Másolja egy alkalmazáscsomagot a fürt lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="21e51-118">Copy an application package to the cluster image store.</span></span>  |
|[<span data-ttu-id="21e51-119">Register-ServiceFabricApplicationType</span><span class="sxs-lookup"><span data-stu-id="21e51-119">Register-ServiceFabricApplicationType</span></span>](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| <span data-ttu-id="21e51-120">Regisztrálja az alkalmazástípus és -verzió a fürtön.</span><span class="sxs-lookup"><span data-stu-id="21e51-120">Registers an application type and version on the cluster.</span></span> |
|[<span data-ttu-id="21e51-121">Új ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="21e51-121">New-ServiceFabricApplication</span></span>](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| <span data-ttu-id="21e51-122">Alkalmazást hoz létre a regisztrált alkalmazáshoz típusból.</span><span class="sxs-lookup"><span data-stu-id="21e51-122">Creates an application from a registered application type.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="21e51-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21e51-123">Next steps</span></span>

<span data-ttu-id="21e51-124">További információ a Service Fabric PowerShell-modul: [Azure PowerShell dokumentációs](/powershell/azure/service-fabric/?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="21e51-124">For more information on the Service Fabric PowerShell module, see [Azure PowerShell documentation](/powershell/azure/service-fabric/?view=azureservicefabricps).</span></span>

<span data-ttu-id="21e51-125">Azure Service Fabric további Powershell-példák találhatók a [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="21e51-125">Additional Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>

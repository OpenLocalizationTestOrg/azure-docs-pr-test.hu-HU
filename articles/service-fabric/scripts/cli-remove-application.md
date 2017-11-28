---
title: "Az Azure Service Fabric CLI parancsfájl eltávolítása minta"
description: "Alkalmazások eltávolítása egy Azure Service Fabric-fürt, az Azure Service Fabric parancssori felület használatával"
services: service-fabric
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: d86f195d2c37a71e476c5ba4eec040dd46931d23
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="7ca8a-103">Alkalmazások eltávolítása a Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="7ca8a-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="7ca8a-104">Ez a parancsfájlpélda futó Service Fabric-alkalmazás példány törlése, regisztrációjának törlése az alkalmazástípus és -verzió a fürtből.</span><span class="sxs-lookup"><span data-stu-id="7ca8a-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from the cluster.</span></span>  <span data-ttu-id="7ca8a-105">Az alkalmazáspéldány törlésekor is törlődnek a futó szolgáltatás az adott alkalmazáshoz tartozó példányok.</span><span class="sxs-lookup"><span data-stu-id="7ca8a-105">Deleting the application instance also deletes all the running service instances associated with that application.</span></span> <span data-ttu-id="7ca8a-106">Ezt követően az alkalmazásfájlokat az image store törlődnek.</span><span class="sxs-lookup"><span data-stu-id="7ca8a-106">Next, the application files are deleted from the image store.</span></span> 

<span data-ttu-id="7ca8a-107">Szükség esetén telepítse a [Service Fabric CLI](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7ca8a-107">If needed, install the [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="7ca8a-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="7ca8a-108">Sample script</span></span>

<span data-ttu-id="7ca8a-109">[!code-sh[fő](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "alkalmazás eltávolítása egy fürtről")]</span><span class="sxs-lookup"><span data-stu-id="7ca8a-109">[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ca8a-110">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ca8a-110">Next steps</span></span>

<span data-ttu-id="7ca8a-111">További információkért lásd: a [Service Fabric CLI dokumentáció](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7ca8a-111">For more information, see the [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="7ca8a-112">Azure Service Fabric további Service Fabric CLI-példák találhatók a [Service Fabric CLI minták](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7ca8a-112">Additional Service Fabric CLI samples for Azure Service Fabric can be found in the [Service Fabric CLI samples](../samples-cli.md).</span></span>

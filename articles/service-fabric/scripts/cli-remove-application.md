---
title: "Service Fabric CLI parancsfájl eltávolítása minta aaaAzure"
description: "Alkalmazások eltávolítása egy Azure Service Fabric-fürt hello Azure Service Fabric parancssori felület használatával"
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
ms.openlocfilehash: 3ccefd4a04c5b7af71a2f959e11da6e402f25881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="8604e-103">Alkalmazások eltávolítása a Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="8604e-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="8604e-104">Ez a parancsfájlpélda futó Service Fabric-alkalmazás példány törli, az alkalmazástípus és -verzió hello fürtből regisztrációjának törlése.</span><span class="sxs-lookup"><span data-stu-id="8604e-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from hello cluster.</span></span>  <span data-ttu-id="8604e-105">Törlése hello alkalmazáspéldány fut az adott alkalmazáshoz kapcsolódó szolgáltatáspéldány összes hello is törli.</span><span class="sxs-lookup"><span data-stu-id="8604e-105">Deleting hello application instance also deletes all hello running service instances associated with that application.</span></span> <span data-ttu-id="8604e-106">A következő hello alkalmazásfájlok hello lemezképtárolóhoz lesznek törölve.</span><span class="sxs-lookup"><span data-stu-id="8604e-106">Next, hello application files are deleted from hello image store.</span></span> 

<span data-ttu-id="8604e-107">Szükség esetén telepítse hello [Service Fabric CLI](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8604e-107">If needed, install hello [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="8604e-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="8604e-108">Sample script</span></span>

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a><span data-ttu-id="8604e-109">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8604e-109">Next steps</span></span>

<span data-ttu-id="8604e-110">További információkért lásd: hello [Service Fabric CLI dokumentáció](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8604e-110">For more information, see hello [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="8604e-111">Azure Service Fabric további Service Fabric CLI-példák találhatók hello [Service Fabric CLI minták](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8604e-111">Additional Service Fabric CLI samples for Azure Service Fabric can be found in hello [Service Fabric CLI samples](../samples-cli.md).</span></span>

---
title: "aaaAzure tároló példányok és tároló Vezénylési"
description: "Azure-tároló példányok és tároló orchestrators együttműködését ismertetése"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 69a39edc6f14d885c1ac300990ed1399002ccfee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances-and-container-orchestrators"></a><span data-ttu-id="eef7a-103">Azure-példányokon tároló és a tároló orchestrators</span><span class="sxs-lookup"><span data-stu-id="eef7a-103">Azure Container Instances and container orchestrators</span></span>

<span data-ttu-id="eef7a-104">Kis méret és alkalmazás tájolás, miatt tárolók alkalmas mikroszolgáltatási-alapú architektúra és gyors kézbesítési környezetben.</span><span class="sxs-lookup"><span data-stu-id="eef7a-104">Because of their small size and application orientation, containers are well suited for agile delivery environments and microservice-based architectures.</span></span> <span data-ttu-id="eef7a-105">hello feladat automatizálása és kezelése a tárolók és azok működésmódját nagy számú nevezik *vezénylési*.</span><span class="sxs-lookup"><span data-stu-id="eef7a-105">hello task of automating and managing a large number of containers and how they interact is known as *orchestration*.</span></span> <span data-ttu-id="eef7a-106">Népszerű tároló orchestrators például Kubernetes, a DC/OS és a Docker Swarm, amelyek mindegyike hello elérhető [Azure Tárolószolgáltatás](https://docs.microsoft.com/azure/container-service/).</span><span class="sxs-lookup"><span data-stu-id="eef7a-106">Popular container orchestrators include Kubernetes, DC/OS, and Docker Swarm, all of which are available in hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span></span>

<span data-ttu-id="eef7a-107">Azure tároló példányok néhány alapvető képességek vezénylési platformokról ütemezés hello biztosít, de nem terjed ki, hogy ezek a platformok adja meg, és ténylegesen lehet őket a kiegészítő hello nagyobb értékű szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="eef7a-107">Azure Container Instances provides some of hello basic scheduling capabilities of orchestration platforms, but it does not cover hello higher-value services that those platforms provide and can in fact be complementary with them.</span></span> <span data-ttu-id="eef7a-108">Ez a cikk ismerteti a hello hatókör Azure tároló példányok kezeli, és hogyan teljes tároló orchestrators lehet használni.</span><span class="sxs-lookup"><span data-stu-id="eef7a-108">This article describes hello scope of what Azure Container Instances handles and how full container orchestrators might interact with it.</span></span>

## <a name="traditional-orchestration"></a><span data-ttu-id="eef7a-109">Hagyományos vezénylési</span><span class="sxs-lookup"><span data-stu-id="eef7a-109">Traditional orchestration</span></span>

<span data-ttu-id="eef7a-110">vezénylési hello szabványos definícióját a következő feladatok hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="eef7a-110">hello standard definition of orchestration includes hello following tasks:</span></span>

- <span data-ttu-id="eef7a-111">**Ütemezés**: egy tároló lemezképet és az erőforrás kérelmet kap, található megfelelő gép mely toorun hello tárolójához.</span><span class="sxs-lookup"><span data-stu-id="eef7a-111">**Scheduling**: Given a container image and a resource request, find a suitable machine on which toorun hello container.</span></span>
- <span data-ttu-id="eef7a-112">**Affinitás/Anti-affinity**: Adja meg, hogy tárolók készlete kell közelben egymástól (teljesítmény) vagy fusson elég távolságra (a rendelkezésre állás érdekében).</span><span class="sxs-lookup"><span data-stu-id="eef7a-112">**Affinity/Anti-affinity**: Specify that a set of containers should run nearby each other (for performance) or sufficiently far apart (for availability).</span></span>
- <span data-ttu-id="eef7a-113">**Állapotfigyelés**: nézze meg a tároló hibák, és automatikusan ütemezze újra őket.</span><span class="sxs-lookup"><span data-stu-id="eef7a-113">**Health monitoring**: Watch for container failures and automatically reschedule them.</span></span>
- <span data-ttu-id="eef7a-114">**Feladatátvételi**: nyomon követjük, hogy mi minden számítógépen fut, és ütemezze újra a sikertelen gépek toohealthy csomópontjáról tárolók.</span><span class="sxs-lookup"><span data-stu-id="eef7a-114">**Failover**: Keep track of what is running on each machine and reschedule containers from failed machines toohealthy nodes.</span></span>
- <span data-ttu-id="eef7a-115">**Skálázás**: hozzáadása vagy eltávolítása a tároló példányok toomatch igény szerint manuálisan vagy automatikusan.</span><span class="sxs-lookup"><span data-stu-id="eef7a-115">**Scaling**: Add or remove container instances toomatch demand, either manually or automatically.</span></span>
- <span data-ttu-id="eef7a-116">**Hálózati**: Adjon meg egy átmeneti területre hálózatot a tárolók toocommunicate összehangolása a különböző több gazdagép gépnek.</span><span class="sxs-lookup"><span data-stu-id="eef7a-116">**Networking**: Provide an overlay network for coordinating containers toocommunicate across multiple host machines.</span></span>
- <span data-ttu-id="eef7a-117">**A szolgáltatásészlelés**: tárolók toolocate egymással automatikusan engedélyezni akár gazdagépeken között, és módosítsa az IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="eef7a-117">**Service discovery**: Enable containers toolocate each other automatically even as they move between host machines and change IP addresses.</span></span>
- <span data-ttu-id="eef7a-118">**Alkalmazásfrissítések koordinált**: tároló frissítések tooavoid alkalmazás állásidő kezeléséhez, és engedélyezze a visszaállítás, ha valamilyen hiba.</span><span class="sxs-lookup"><span data-stu-id="eef7a-118">**Coordinated application upgrades**: Manage container upgrades tooavoid application down time and enable rollback if something goes wrong.</span></span>

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a><span data-ttu-id="eef7a-119">Vezénylési Azure tároló osztályt: réteges megközelítésének</span><span class="sxs-lookup"><span data-stu-id="eef7a-119">Orchestration with Azure Container Instances: A layered approach</span></span>

<span data-ttu-id="eef7a-120">Azure-példányokon tároló lehetővé teszi, hogy a réteges megközelítésének tooorchestration összes hello ütemezés megadása, és felügyeleti képességek szükséges toorun egyetlen tárolót, miközben lehetővé teszi az orchestrator a platformok toomanage több tároló feladatok utasítást.</span><span class="sxs-lookup"><span data-stu-id="eef7a-120">Azure Container Instances enables a layered approach tooorchestration, providing all of hello scheduling and management capabilities required toorun a single container, while allowing orchestrator platforms toomanage multi-container tasks on top of it.</span></span>

<span data-ttu-id="eef7a-121">Összes alapjául szolgáló infrastruktúra tároló példányok hello Azure kezeli, mert az orchestrator platform nem kell egy megfelelő gazdagépet a mely toorun egyetlen tárolót keresése a saját magát tooconcern.</span><span class="sxs-lookup"><span data-stu-id="eef7a-121">Because all of hello underlying infrastructure for Container Instances is managed by Azure, an orchestrator platform does not need tooconcern itself with finding an appropriate host machine on which toorun a single container.</span></span> <span data-ttu-id="eef7a-122">hello felhő hello rugalmassága biztosítja, hogy egy mindig elérhető legyen.</span><span class="sxs-lookup"><span data-stu-id="eef7a-122">hello elasticity of hello cloud ensures that one is always available.</span></span> <span data-ttu-id="eef7a-123">Ehelyett hello orchestrator hello feladatok, amelyek megkönnyítik a több tároló-architektúrák, beleértve a méretezés és koordinált frissítéseinek hello fejlesztésének összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="eef7a-123">Instead, hello orchestrator can focus on hello tasks that simplify hello development of multi-container architectures, including scaling and coordinated upgrades.</span></span>



## <a name="potential-scenarios"></a><span data-ttu-id="eef7a-124">A lehetséges forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="eef7a-124">Potential scenarios</span></span>

<span data-ttu-id="eef7a-125">Azure-tároló példányaival orchestrator integrációs pedig továbbra is születő tervezzük, hogy néhány különböző környezetekben előfordulhat, hogy megjelenni:</span><span class="sxs-lookup"><span data-stu-id="eef7a-125">While orchestrator integration with Azure Container Instances is still nascent, we anticipate that a few different environments may emerge:</span></span>

### <a name="orchestration-of-container-instances-exclusively"></a><span data-ttu-id="eef7a-126">Vezénylési tároló példányok kizárólag</span><span class="sxs-lookup"><span data-stu-id="eef7a-126">Orchestration of Container Instances exclusively</span></span>

<span data-ttu-id="eef7a-127">Mivel gyorsan start, illetve kiszámlázni hello által a második, kizárólag a alapú környezet Azure tároló példányok kínál hello leggyorsabb módon tooget elindult és a munkaterhelések magas változó toodeal.</span><span class="sxs-lookup"><span data-stu-id="eef7a-127">Because they start quickly and bill by hello second, an environment based exclusively on Azure Container Instances offers hello fastest way tooget started and toodeal with highly variable workloads.</span></span>

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a><span data-ttu-id="eef7a-128">Tároló-példányok és a virtuális gépek a tárolók</span><span class="sxs-lookup"><span data-stu-id="eef7a-128">Combination of Container Instances and Containers in Virtual Machines</span></span>

<span data-ttu-id="eef7a-129">Hosszú futású stabil, tárolók dedikált virtuális gépek fürtben futó munkaterhelések jellemzően lesz olcsóbbak futó hello azonos tárolók tároló osztályt.</span><span class="sxs-lookup"><span data-stu-id="eef7a-129">For long-running, stable workloads, orchestrating containers in a cluster of dedicated virtual machines will typically be cheaper than running hello same containers with Container Instances.</span></span> <span data-ttu-id="eef7a-130">Azonban a tároló példányok kiválóan alkalmas gyorsan kibontásával, és a teljes kapacitásának toodeal való használata során váratlan vagy rövid élettartamú igényeiben jelentkező szerződő kínálnak.</span><span class="sxs-lookup"><span data-stu-id="eef7a-130">However, Container Instances offer a great solution for quickly expanding and contracting your overall capacity toodeal with unexpected or short-lived spikes in usage.</span></span> <span data-ttu-id="eef7a-131">Ahelyett, hogy a fürtben lévő virtuális gépek száma hello kiterjesztése, akkor telepíti ezeket a gépeket alakzatot további tárolókat, hello orchestrator is egyszerűen ütemezése hello tároló példányok használatával további tárolókat, és törölje őket, ha azok nem már szükséges.</span><span class="sxs-lookup"><span data-stu-id="eef7a-131">Rather than scaling out hello number of virtual machines in your cluster, then deploying additional containers onto those machines, hello orchestrator can simply schedule hello additional containers using Container Instances and delete them when they're no longer needed.</span></span>

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a><span data-ttu-id="eef7a-132">Példa: Kubernetes Azure tároló példányok összekötő</span><span class="sxs-lookup"><span data-stu-id="eef7a-132">Sample implementation: Azure Container Instances Connector for Kubernetes</span></span>

<span data-ttu-id="eef7a-133">Hogyan tároló vezénylési platformok integrálhatják az Azure tároló osztályt toodemonstrate, azt elindította épület egy [Kubernetes minta összekötő][aci-connector-k8s].</span><span class="sxs-lookup"><span data-stu-id="eef7a-133">toodemonstrate how container orchestration platforms can integrate with Azure Container Instances, we have started building a [sample connector for Kubernetes][aci-connector-k8s].</span></span> 

<span data-ttu-id="eef7a-134">összekötő hello a Kubernetes utánozza hello [kubelet] [ kubelet-doc] korlátlan kapacitás csomópontként regisztrálása és hello létrehozása terjesztéséhez [három munkaállomás-csoporttal] [ pod-doc] tároló csoportokként Azure tároló példányát.</span><span class="sxs-lookup"><span data-stu-id="eef7a-134">hello connector for Kubernetes mimics hello [kubelet][kubelet-doc] by registering as a node with unlimited capacity and dispatching hello creation of [pods][pod-doc] as container groups in Azure Container Instances.</span></span> 

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

<span data-ttu-id="eef7a-135">Összekötők a többi orchestrators épülhet, hasonlóképpen platform primitívek toocombine hello power hello orchestrator API hello sebességű és egyszerű Azure tároló példányát tárolók kezelése a integrálva.</span><span class="sxs-lookup"><span data-stu-id="eef7a-135">Connectors for other orchestrators could be built that similarly integrated with platform primitives toocombine hello power of hello orchestrator API with hello speed and simplicity of managing containers in Azure Container Instances.</span></span>

> [!WARNING]
> <span data-ttu-id="eef7a-136">hello ACI összekötő Kubernetes érték *kísérleti* és éles környezetben nem használható.</span><span class="sxs-lookup"><span data-stu-id="eef7a-136">hello ACI connector for Kubernetes is *experimental* and should not be used in production.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eef7a-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eef7a-137">Next steps</span></span>

<span data-ttu-id="eef7a-138">Az első tároló létrehozása Azure-tároló osztályt hello segítségével [– első lépések útmutató](container-instances-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="eef7a-138">Create your first container with Azure Container Instances using hello [quick start guide](container-instances-quickstart.md).</span></span>

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/azure/aci-connector-k8s
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
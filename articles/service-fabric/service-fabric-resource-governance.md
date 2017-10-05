---
title: "Azure Service Fabric erőforrás-szabályozás megvalósításához a tárolók és a szolgáltatások |} Microsoft Docs"
description: "Az Azure Service Fabric teszi erőforrás határértékeken belül vagy kívül tárolók futó szolgáltatásokhoz."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 88d44953ad83f9e7401fd087a39842e4a3790124
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="resource-governance"></a><span data-ttu-id="1df59-103">Erőforrás-irányítás</span><span class="sxs-lookup"><span data-stu-id="1df59-103">Resource governance</span></span> 

<span data-ttu-id="1df59-104">A csomópont vagy a fürt több szolgáltatást futtat, esetén lehetséges, hogy egy szolgáltatás előfordulhat, hogy több erőforrást starving egyéb szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="1df59-104">When running multiple services on the same node or cluster, it is possible that one service might consume more resources starving other services.</span></span> <span data-ttu-id="1df59-105">Ez a probléma a zajos szomszédos probléma nevezzük.</span><span class="sxs-lookup"><span data-stu-id="1df59-105">This problem is referred to as the noisy-neighbor problem.</span></span> <span data-ttu-id="1df59-106">A Service Fabric lehetővé teszi, hogy a fejlesztő adhatja meg a fenntartásokat és határon belül az egyes erőforrások biztosítása és az erőforrás-használat is korlátozza.</span><span class="sxs-lookup"><span data-stu-id="1df59-106">Service Fabric allows the developer to specify reservations and limits per service to guarantee resources and also limit its resource usage.</span></span> 

## <a name="resource-governance-metrics"></a><span data-ttu-id="1df59-107">Erőforrás-irányítás metrikák</span><span class="sxs-lookup"><span data-stu-id="1df59-107">Resource governance metrics</span></span> 

<span data-ttu-id="1df59-108">Erőforrás-irányítás támogatott a Service Fabric / [szolgáltatáscsomag](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="1df59-108">Resource governance is supported in Service Fabric per [Service Package](service-fabric-application-model.md).</span></span> <span data-ttu-id="1df59-109">A Service-csomagra hozzárendelt erőforrások tovább oszthatók kód csomagok között.</span><span class="sxs-lookup"><span data-stu-id="1df59-109">The resources that are assigned to Service Package can be further divided between code packages.</span></span> <span data-ttu-id="1df59-110">A megadott erőforrás-korlátok is jelentheti a Foglalás erőforrást.</span><span class="sxs-lookup"><span data-stu-id="1df59-110">The resource limits specified also mean the reservation of the resources.</span></span> <span data-ttu-id="1df59-111">A Service Fabric támogatja a Processzor és memória megadó használatával két beépített szolgáltatás csomagonként [metrikák](service-fabric-cluster-resource-manager-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="1df59-111">Service Fabric supports specifying CPU and Memory per service package, using two built-in [metrics](service-fabric-cluster-resource-manager-metrics.md):</span></span>

* <span data-ttu-id="1df59-112">Processzor (metrika neve `ServiceFabric:/_CpuCores`): alapszintű, a gazdagépen rendelkezésre álló logikai alapszintű, és az összes csomópont összes mag van súlyozott azonos.</span><span class="sxs-lookup"><span data-stu-id="1df59-112">CPU (metric name `ServiceFabric:/_CpuCores`): A core is a logical core that is available on the host machine, and all cores across all nodes are weighted the same.</span></span>
* <span data-ttu-id="1df59-113">Memória (metrika neve `ServiceFabric:/_MemoryInMB`): memória megabájtban van kifejezve, és hozzárendeli őket a gépen rendelkezésre álló fizikai memória.</span><span class="sxs-lookup"><span data-stu-id="1df59-113">Memory (metric name `ServiceFabric:/_MemoryInMB`): Memory is expressed in megabytes, and it maps to physical memory that is available on the machine.</span></span>

<span data-ttu-id="1df59-114">Csak az ideiglenes foglalási garanciák vannak megadott - futásidejű elutasítja a rendelkezésre álló erőforrások számát új service-csomagok megnyitása.</span><span class="sxs-lookup"><span data-stu-id="1df59-114">Only soft reservation guarantees are provided - the runtime rejects opening of new service packages available resources are exceeded.</span></span> <span data-ttu-id="1df59-115">Azonban a csomópont egy másik végrehajtható vagy tároló helyezkedik el, ha, amely az eredeti foglalási garanciák megsértő is.</span><span class="sxs-lookup"><span data-stu-id="1df59-115">However, if another executable or container is placed on the node, that may violate the original reservation guarantees.</span></span>

<span data-ttu-id="1df59-116">A két metrikákat a [fürt erőforrás-kezelő](service-fabric-cluster-resource-manager-cluster-description.md) követi nyomon a fürt teljes kapacitás, a terhelést a fürt mindegyik csomópontján, és a fürterőforrások maradt.</span><span class="sxs-lookup"><span data-stu-id="1df59-116">For these two metrics, the [Cluster Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) tracks total cluster capacity, the load on each node in the cluster, and, remaining resources in the cluster.</span></span> <span data-ttu-id="1df59-117">Két metrikákat felhasználói vagy egyéni metrika és minden meglévő szolgáltatása velük használható:</span><span class="sxs-lookup"><span data-stu-id="1df59-117">These two metrics are equivalent to any other user or custom metric and all existing features can be used with them:</span></span>
* <span data-ttu-id="1df59-118">Fürt lehet [elosztott terhelésű](service-fabric-cluster-resource-manager-balancing.md) megfelelően a két metrikák (alapértelmezés).</span><span class="sxs-lookup"><span data-stu-id="1df59-118">Cluster can be [balanced](service-fabric-cluster-resource-manager-balancing.md) according to these two metrics (default behavior).</span></span>
* <span data-ttu-id="1df59-119">Fürt lehet [töredezettségmentesíteni](service-fabric-cluster-resource-manager-defragmentation-metrics.md) megfelelően két metrikákat.</span><span class="sxs-lookup"><span data-stu-id="1df59-119">Cluster can be [defragmented](service-fabric-cluster-resource-manager-defragmentation-metrics.md) according to these two metrics.</span></span>
* <span data-ttu-id="1df59-120">Ha [fürt leíró](service-fabric-cluster-resource-manager-cluster-description.md), pufferelt kapacitás állíthat be két metrikákat.</span><span class="sxs-lookup"><span data-stu-id="1df59-120">When [describing a cluster](service-fabric-cluster-resource-manager-cluster-description.md), buffered capacity can be set for these two metrics.</span></span>

<span data-ttu-id="1df59-121">[Dinamikus terheléselosztó jelentéskészítési](service-fabric-cluster-resource-manager-metrics.md) nem támogatott a következő metrikák tekintetében, és betölti a fenti metrikák létrehozáskor vannak meghatározva.</span><span class="sxs-lookup"><span data-stu-id="1df59-121">[Dynamic load reporting](service-fabric-cluster-resource-manager-metrics.md) is not supported for these metrics, and loads for these metrics are defined at creation time.</span></span>

## <a name="cluster-set-up-for-enabling-resource-governance"></a><span data-ttu-id="1df59-122">A fürt set feliratkozott erőforrás irányítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1df59-122">Cluster set up for enabling resource governance</span></span>

<span data-ttu-id="1df59-123">Kapacitás definiálni kell manuálisan a fürt minden csomópont típus az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1df59-123">Capacity should be defined manually in each node type in the cluster as follows:</span></span>

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
<span data-ttu-id="1df59-124">Csak a felhasználó-szolgáltatásokra, és nem a rendszer szolgáltatások erőforrás irányítás engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="1df59-124">Resource governance is allowed only on user services and not on any system services.</span></span> <span data-ttu-id="1df59-125">Kapacitás, néhány maggal és memória megadásakor kell kell balra nem lefoglalt-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="1df59-125">When specifying capacity, some cores and memory must be left unallocated for system services.</span></span> <span data-ttu-id="1df59-126">Az optimális teljesítmény érdekében az alábbi beállítást is be kell kapcsolni a fürtjegyzékben:</span><span class="sxs-lookup"><span data-stu-id="1df59-126">For optimal performance, the following setting should also be turned on in the cluster manifest:</span></span> 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a><span data-ttu-id="1df59-127">Adja meg az erőforrás-irányítás</span><span class="sxs-lookup"><span data-stu-id="1df59-127">Specifying resource governance</span></span> 

<span data-ttu-id="1df59-128">Erőforrás-irányítás határértékeken vannak megadva az alkalmazásjegyzékben (ServiceManifestImport szakaszát), a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="1df59-128">Resource governance limits are specified in the application manifest (ServiceManifestImport section) as shown in the following example:</span></span>

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has the number of CPU cores defined, but doesn't have the MemoryInMB defined.
  In this case, Service Fabric will sum the limits on code packages and uses the sum as 
  the overall ServicePackage limit.
  -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1000" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1000" />
    </Policies>
  </ServiceManifestImport>
```
  
<span data-ttu-id="1df59-129">Ebben a példában a szolgáltatáscsomagot ServicePackageA egy alapvető lekérdezi a csomópontokon, ahol el van helyezve.</span><span class="sxs-lookup"><span data-stu-id="1df59-129">In this example, service package ServicePackageA gets one core on the nodes where it is placed.</span></span> <span data-ttu-id="1df59-130">A szolgáltatás csomagban (CodeA1 és CodeA2) két kód csomagok, és adja meg, mindkét a `CpuShares` paraméter.</span><span class="sxs-lookup"><span data-stu-id="1df59-130">This service package contains two code packages (CodeA1 and CodeA2), and both specify the `CpuShares` parameter.</span></span> <span data-ttu-id="1df59-131">CpuShares 512:256 aránya a core osztja a két kód csomagok között.</span><span class="sxs-lookup"><span data-stu-id="1df59-131">The proportion of CpuShares 512:256  divides the core across the two code packages.</span></span> <span data-ttu-id="1df59-132">Így ebben a példában CodeA1 beolvasása, amely alapszintű, és CodeA2 lekérdezi egyharmad részére alapszintű (és ugyanazt a soft-garancia lefoglalása).</span><span class="sxs-lookup"><span data-stu-id="1df59-132">Thus, in this example, CodeA1 gets two-thirds of a core, and  CodeA2 gets one-third of a core (and a soft-guarantee reservation of the same).</span></span> <span data-ttu-id="1df59-133">Abban az esetben, amikor CpuShares nincsenek megadva a kód csomagokat, a Service Fabric osztja a magok egyaránt közöttük.</span><span class="sxs-lookup"><span data-stu-id="1df59-133">In case when CpuShares are not specified for code packages, Service Fabric divides the cores equally among them.</span></span>

<span data-ttu-id="1df59-134">Memóriakorlátokat olyan abszolút, úgy, hogy mindkét kód csomag legfeljebb 1024 MB memória (és ugyanazt a soft-garancia lefoglalása).</span><span class="sxs-lookup"><span data-stu-id="1df59-134">Memory limits are absolute, so both code packages are limited to 1024 MB of memory (and a soft-guarantee reservation of the same).</span></span> <span data-ttu-id="1df59-135">A kódcsomagok (tárolók vagy folyamatok) nem tudnak ennél a korlátnál több memóriát lefoglalni, és ennek megkísérlése memóriahiány miatti kivételt eredményez.</span><span class="sxs-lookup"><span data-stu-id="1df59-135">Code packages (containers or processes) are not able to allocate more memory than this limit, and attempting to do so results in an out-of-memory exception.</span></span> <span data-ttu-id="1df59-136">Az erőforráskorlát érvényesítéséhez a szolgáltatáscsomagokban lévő minden kódcsomaghoz memóriakorlátokat kell meghatároznia.</span><span class="sxs-lookup"><span data-stu-id="1df59-136">For resource limit enforcement to work, all code packages within a service package should have memory limits specified.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1df59-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1df59-137">Next steps</span></span>
* <span data-ttu-id="1df59-138">További tudnivalók fürt erőforrás-kezelő, olvassa el ezt [cikk](service-fabric-cluster-resource-manager-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1df59-138">To learn more about Cluster Resource Manager, read this [article](service-fabric-cluster-resource-manager-introduction.md).</span></span>
* <span data-ttu-id="1df59-139">További információt alkalmazásmodell, szolgáltatáscsomagok, kód csomagok és hogyan replikák hozzárendelését őket olvassa el ezt [cikk](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="1df59-139">To learn more about application model, service packages, code packages and how replicas map to them read this [article](service-fabric-application-model.md).</span></span>

---
title: "Service Fabric erőforrás irányítás tárolók és a szolgáltatások aaaAzure |} Microsoft Docs"
description: "Az Azure Service Fabric toospecify erőforrás-határértékeken belül vagy kívül tárolók futó szolgáltatások segítségével."
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
ms.openlocfilehash: 34e368211d98ff6b5b294c9c8b3af5ca30eeb20c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-governance"></a><span data-ttu-id="7cdde-103">Erőforrás-irányítás</span><span class="sxs-lookup"><span data-stu-id="7cdde-103">Resource governance</span></span> 

<span data-ttu-id="7cdde-104">Több szolgáltatás futó hello ugyanazon csomópont vagy fürthöz, esetén lehetséges, hogy egy szolgáltatás előfordulhat, hogy több erőforrást starving egyéb szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="7cdde-104">When running multiple services on hello same node or cluster, it is possible that one service might consume more resources starving other services.</span></span> <span data-ttu-id="7cdde-105">Ez a probléma nem hivatkozott tooas hello zajos szomszédos probléma.</span><span class="sxs-lookup"><span data-stu-id="7cdde-105">This problem is referred tooas hello noisy-neighbor problem.</span></span> <span data-ttu-id="7cdde-106">A Service Fabric hello fejlesztői toospecify fenntartásokat és határon belül az egyes szolgáltatási tooguarantee erőforrások lehetővé teszi, és is az az erőforrás-használatát korlátozása.</span><span class="sxs-lookup"><span data-stu-id="7cdde-106">Service Fabric allows hello developer toospecify reservations and limits per service tooguarantee resources and also limit its resource usage.</span></span> 

## <a name="resource-governance-metrics"></a><span data-ttu-id="7cdde-107">Erőforrás-irányítás metrikák</span><span class="sxs-lookup"><span data-stu-id="7cdde-107">Resource governance metrics</span></span> 

<span data-ttu-id="7cdde-108">Erőforrás-irányítás támogatott a Service Fabric / [szolgáltatáscsomag](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="7cdde-108">Resource governance is supported in Service Fabric per [Service Package](service-fabric-application-model.md).</span></span> <span data-ttu-id="7cdde-109">hello erőforrásokhoz rendelt tooService csomag tovább oszthatók kód csomagok között.</span><span class="sxs-lookup"><span data-stu-id="7cdde-109">hello resources that are assigned tooService Package can be further divided between code packages.</span></span> <span data-ttu-id="7cdde-110">a megadott erőforrás-korlátok hello is jelentheti hello hello erőforrások lefoglalása.</span><span class="sxs-lookup"><span data-stu-id="7cdde-110">hello resource limits specified also mean hello reservation of hello resources.</span></span> <span data-ttu-id="7cdde-111">A Service Fabric támogatja a Processzor és memória megadó használatával két beépített szolgáltatás csomagonként [metrikák](service-fabric-cluster-resource-manager-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="7cdde-111">Service Fabric supports specifying CPU and Memory per service package, using two built-in [metrics](service-fabric-cluster-resource-manager-metrics.md):</span></span>

* <span data-ttu-id="7cdde-112">Processzor (metrika neve `ServiceFabric:/_CpuCores`): alapszintű logikai alapszintű hello gazdaszámítógépen elérhető, és az összes csomópont összes mag van súlyozott hello azonos.</span><span class="sxs-lookup"><span data-stu-id="7cdde-112">CPU (metric name `ServiceFabric:/_CpuCores`): A core is a logical core that is available on hello host machine, and all cores across all nodes are weighted hello same.</span></span>
* <span data-ttu-id="7cdde-113">Memória (metrika neve `ServiceFabric:/_MemoryInMB`): memória megabájtban van kifejezve, és hello számítógépen legyen toophysical memóriát rendel hozzá.</span><span class="sxs-lookup"><span data-stu-id="7cdde-113">Memory (metric name `ServiceFabric:/_MemoryInMB`): Memory is expressed in megabytes, and it maps toophysical memory that is available on hello machine.</span></span>

<span data-ttu-id="7cdde-114">Csak ideiglenes foglalási garanciák találhatók - hello futásidejű elutasítja a csomagok rendelkezésre álló erőforrások túllépése új szolgáltatás megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="7cdde-114">Only soft reservation guarantees are provided - hello runtime rejects opening of new service packages available resources are exceeded.</span></span> <span data-ttu-id="7cdde-115">Azonban egy másik végrehajtható vagy tároló hello csomóponton kerül, ha, előfordulhat, hogy megsértik hello eredeti foglalási garanciák.</span><span class="sxs-lookup"><span data-stu-id="7cdde-115">However, if another executable or container is placed on hello node, that may violate hello original reservation guarantees.</span></span>

<span data-ttu-id="7cdde-116">A két metrikák hello [fürt erőforrás-kezelő](service-fabric-cluster-resource-manager-cluster-description.md) követi nyomon a fürt teljes kapacitás, hello terhelés hello fürt mindegyik csomópontján, és fennmaradó hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="7cdde-116">For these two metrics, hello [Cluster Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) tracks total cluster capacity, hello load on each node in hello cluster, and, remaining resources in hello cluster.</span></span> <span data-ttu-id="7cdde-117">A két metrikák egyenértékű tooany más felhasználó vagy az egyéni metrika, és minden meglévő szolgáltatása velük használható:</span><span class="sxs-lookup"><span data-stu-id="7cdde-117">These two metrics are equivalent tooany other user or custom metric and all existing features can be used with them:</span></span>
* <span data-ttu-id="7cdde-118">Fürt lehet [elosztott terhelésű](service-fabric-cluster-resource-manager-balancing.md) szerint toothese két metrikák (alapértelmezés).</span><span class="sxs-lookup"><span data-stu-id="7cdde-118">Cluster can be [balanced](service-fabric-cluster-resource-manager-balancing.md) according toothese two metrics (default behavior).</span></span>
* <span data-ttu-id="7cdde-119">Fürt lehet [töredezettségmentesíteni](service-fabric-cluster-resource-manager-defragmentation-metrics.md) toothese két mérőszámok alapján történik.</span><span class="sxs-lookup"><span data-stu-id="7cdde-119">Cluster can be [defragmented](service-fabric-cluster-resource-manager-defragmentation-metrics.md) according toothese two metrics.</span></span>
* <span data-ttu-id="7cdde-120">Ha [fürt leíró](service-fabric-cluster-resource-manager-cluster-description.md), pufferelt kapacitás állíthat be két metrikákat.</span><span class="sxs-lookup"><span data-stu-id="7cdde-120">When [describing a cluster](service-fabric-cluster-resource-manager-cluster-description.md), buffered capacity can be set for these two metrics.</span></span>

<span data-ttu-id="7cdde-121">[Dinamikus terheléselosztó jelentéskészítési](service-fabric-cluster-resource-manager-metrics.md) nem támogatott a következő metrikák tekintetében, és betölti a fenti metrikák létrehozáskor vannak meghatározva.</span><span class="sxs-lookup"><span data-stu-id="7cdde-121">[Dynamic load reporting](service-fabric-cluster-resource-manager-metrics.md) is not supported for these metrics, and loads for these metrics are defined at creation time.</span></span>

## <a name="cluster-set-up-for-enabling-resource-governance"></a><span data-ttu-id="7cdde-122">A fürt set feliratkozott erőforrás irányítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7cdde-122">Cluster set up for enabling resource governance</span></span>

<span data-ttu-id="7cdde-123">Kapacitás definiálni kell manuálisan az egyes csomóponttípusokban hello fürt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="7cdde-123">Capacity should be defined manually in each node type in hello cluster as follows:</span></span>

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
<span data-ttu-id="7cdde-124">Csak a felhasználó-szolgáltatásokra, és nem a rendszer szolgáltatások erőforrás irányítás engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="7cdde-124">Resource governance is allowed only on user services and not on any system services.</span></span> <span data-ttu-id="7cdde-125">Kapacitás, néhány maggal és memória megadásakor kell kell balra nem lefoglalt-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="7cdde-125">When specifying capacity, some cores and memory must be left unallocated for system services.</span></span> <span data-ttu-id="7cdde-126">Az optimális teljesítmény érdekében a következő beállítás hello is be kell kapcsolni a fürtjegyzékben hello:</span><span class="sxs-lookup"><span data-stu-id="7cdde-126">For optimal performance, hello following setting should also be turned on in hello cluster manifest:</span></span> 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a><span data-ttu-id="7cdde-127">Adja meg az erőforrás-irányítás</span><span class="sxs-lookup"><span data-stu-id="7cdde-127">Specifying resource governance</span></span> 

<span data-ttu-id="7cdde-128">Erőforrás-irányítás határértékeken hello alkalmazásjegyzékben (ServiceManifestImport szakaszát) vannak megadva, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="7cdde-128">Resource governance limits are specified in hello application manifest (ServiceManifestImport section) as shown in hello following example:</span></span>

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has hello number of CPU cores defined, but doesn't have hello MemoryInMB defined.
  In this case, Service Fabric will sum hello limits on code packages and uses hello sum as 
  hello overall ServicePackage limit.
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
  
<span data-ttu-id="7cdde-129">Ebben a példában a service-csomag ServicePackageA egy alapvető lekérdezi a hello csomóponton, ahol el van helyezve.</span><span class="sxs-lookup"><span data-stu-id="7cdde-129">In this example, service package ServicePackageA gets one core on hello nodes where it is placed.</span></span> <span data-ttu-id="7cdde-130">A szolgáltatás csomagban (CodeA1 és CodeA2) két kód csomagok, és mindkét adja meg a hello `CpuShares` paraméter.</span><span class="sxs-lookup"><span data-stu-id="7cdde-130">This service package contains two code packages (CodeA1 and CodeA2), and both specify hello `CpuShares` parameter.</span></span> <span data-ttu-id="7cdde-131">hello hányadát CpuShares 512:256 hello core osztja hello két kód csomagok között.</span><span class="sxs-lookup"><span data-stu-id="7cdde-131">hello proportion of CpuShares 512:256  divides hello core across hello two code packages.</span></span> <span data-ttu-id="7cdde-132">Emiatt ebben a példában CodeA1 egy mag, amely lekérdezi és CodeA2 lekérdezi az alapszintű egyharmad (és soft-garancia foglalást a hello ugyanaz).</span><span class="sxs-lookup"><span data-stu-id="7cdde-132">Thus, in this example, CodeA1 gets two-thirds of a core, and  CodeA2 gets one-third of a core (and a soft-guarantee reservation of hello same).</span></span> <span data-ttu-id="7cdde-133">Abban az esetben, ha CpuShares kód csomagok esetében nincs megadva, a Service Fabric osztja hello magok egyaránt közöttük.</span><span class="sxs-lookup"><span data-stu-id="7cdde-133">In case when CpuShares are not specified for code packages, Service Fabric divides hello cores equally among them.</span></span>

<span data-ttu-id="7cdde-134">Memóriakorlátokat úgy, hogy mindkét kód csomag korlátozott too1024 absolute rendszer MB memória (és a soft-garancia lefoglalása hello ugyanaz).</span><span class="sxs-lookup"><span data-stu-id="7cdde-134">Memory limits are absolute, so both code packages are limited too1024 MB of memory (and a soft-guarantee reservation of hello same).</span></span> <span data-ttu-id="7cdde-135">Kód csomagok (tárolók és folyamatok) olyan nem tud tooallocate toodo kísérlet, és ezt a határt több memóriával, kevés a memória kivétel eredményez.</span><span class="sxs-lookup"><span data-stu-id="7cdde-135">Code packages (containers or processes) are not able tooallocate more memory than this limit, and attempting toodo so results in an out-of-memory exception.</span></span> <span data-ttu-id="7cdde-136">A service-csomag összes kódot csomagok erőforrás korlátját kényszerítési toowork, a megadott memóriakorlátokat kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7cdde-136">For resource limit enforcement toowork, all code packages within a service package should have memory limits specified.</span></span>


## <a name="next-steps"></a><span data-ttu-id="7cdde-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7cdde-137">Next steps</span></span>
* <span data-ttu-id="7cdde-138">több kapcsolatos erőforrás-kezelő toolearn olvassa el ezt [cikk](service-fabric-cluster-resource-manager-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7cdde-138">toolearn more about Cluster Resource Manager, read this [article](service-fabric-cluster-resource-manager-introduction.md).</span></span>
* <span data-ttu-id="7cdde-139">További információ az alkalmazásmodell, szolgáltatáscsomagok, kód csomagok, és hogyan replikák leképezése toothem toolearn olvassa el ezt [cikk](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="7cdde-139">toolearn more about application model, service packages, code packages and how replicas map toothem read this [article](service-fabric-application-model.md).</span></span>

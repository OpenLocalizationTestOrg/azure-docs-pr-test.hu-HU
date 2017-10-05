---
title: "Service Fabric fürt erőforrás-kezelő: a mozgás költsége |} Microsoft Docs"
description: "A mozgás költsége Service Fabric-szolgáltatások áttekintése"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f022f258-7bc0-4db4-aa85-8c6c8344da32
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5de07c259d1d327d0211338c2911804445dd6b60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="service-movement-cost"></a><span data-ttu-id="18071-103">A mozgás költsége szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="18071-103">Service movement cost</span></span>
<span data-ttu-id="18071-104">A Service Fabric fürt erőforrás-kezelő úgy ítéli meg, amikor megpróbálja megállapítani, hogy milyen módosítások, hogy a fürt tényezővel a költségét és ezeket a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="18071-104">A factor that the Service Fabric Cluster Resource Manager considers when trying to determine what changes to make to a cluster is the cost of those changes.</span></span> <span data-ttu-id="18071-105">A "költség" fogalmát ellen, hogy mekkora a fürt növelhető a forog ki.</span><span class="sxs-lookup"><span data-stu-id="18071-105">The notion of "cost" is traded off against how much the cluster can be improved.</span></span> <span data-ttu-id="18071-106">Költség Beleszámítja a terheléselosztás, töredezettségmentesítés és egyéb követelmények szolgáltatások áthelyezésekor.</span><span class="sxs-lookup"><span data-stu-id="18071-106">Cost is factored in when moving services for balancing, defragmentation, and other requirements.</span></span> <span data-ttu-id="18071-107">A cél, hogy legalább zavaró vagy drága módon követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="18071-107">The goal is to meet the requirements in the least disruptive or expensive way.</span></span> 

<span data-ttu-id="18071-108">Helyezze át a szolgáltatások költségek CPU-idő, és a hálózati sávszélesség minimális.</span><span class="sxs-lookup"><span data-stu-id="18071-108">Moving services costs CPU time and network bandwidth at a minimum.</span></span> <span data-ttu-id="18071-109">Állapotalapú szolgáltatások esetén igényel azon szolgáltatásokat, ezzel további memóriát és lemez állapotának másolása.</span><span class="sxs-lookup"><span data-stu-id="18071-109">For stateful services, it requires copying the state of those services, consuming additional memory and disk.</span></span> <span data-ttu-id="18071-110">Győződjön meg arról, hogy a fürt erőforrásait feleslegesen nem töltött megoldások, hogy az Azure Service Fabric fürt erőforrás-kezelő a költségek minimalizálása segít.</span><span class="sxs-lookup"><span data-stu-id="18071-110">Minimizing the cost of solutions that the Azure Service Fabric Cluster Resource Manager comes up with helps ensure that the cluster's resources aren't spent unnecessarily.</span></span> <span data-ttu-id="18071-111">Azonban még nem szeretnénk figyelmen kívül hagyása megoldások, amelyek jelentősen javíthatják a a fürt-erőforrások elosztását.</span><span class="sxs-lookup"><span data-stu-id="18071-111">However, you also don’t want to ignore solutions that would significantly improve the allocation of resources in the cluster.</span></span>

<span data-ttu-id="18071-112">A fürt erőforrás-kezelő rendelkezik kétféleképpen számítástechnikai költségeket, és korlátozza azokat próbál meg a fürt kezelése közben.</span><span class="sxs-lookup"><span data-stu-id="18071-112">The Cluster Resource Manager has two ways of computing costs and limiting them while it tries to manage the cluster.</span></span> <span data-ttu-id="18071-113">Az első eljárás egyszerűen számolása minden elindított áthelyezésre is volna.</span><span class="sxs-lookup"><span data-stu-id="18071-113">The first mechanism is simply counting every move that it would make.</span></span> <span data-ttu-id="18071-114">Ha a két megoldás akkor jönnek létre, az azonos kapcsolatos egyenleg (pontszám), akkor a fürt erőforrás-kezelő inkább egy, a legalacsonyabb költség (a kurzor teljes száma).</span><span class="sxs-lookup"><span data-stu-id="18071-114">If two solutions are generated with about the same balance (score), then the Cluster Resource Manager prefers the one with the lowest cost (total number of moves).</span></span>

<span data-ttu-id="18071-115">Ezt a stratégiát akkor is működik.</span><span class="sxs-lookup"><span data-stu-id="18071-115">This strategy works well.</span></span> <span data-ttu-id="18071-116">De az alapértelmezett vagy statikus, nem valószínű, hogy az összes lépés egyenlőek bármely összetett rendszerben.</span><span class="sxs-lookup"><span data-stu-id="18071-116">But as with default or static loads, it's unlikely in any complex system that all moves are equal.</span></span> <span data-ttu-id="18071-117">Néhány valószínűleg sokkal drága lehet.</span><span class="sxs-lookup"><span data-stu-id="18071-117">Some are likely to be much more expensive.</span></span>

## <a name="setting-move-costs"></a><span data-ttu-id="18071-118">A beállítás költségek áthelyezése</span><span class="sxs-lookup"><span data-stu-id="18071-118">Setting Move Costs</span></span> 
<span data-ttu-id="18071-119">Az alapértelmezett áthelyezése költség szolgáltatás létrehozásakor adhatók meg:</span><span class="sxs-lookup"><span data-stu-id="18071-119">You can specify the default move cost for a service when it is created:</span></span>

<span data-ttu-id="18071-120">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="18071-120">PowerShell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

<span data-ttu-id="18071-121">C#:</span><span class="sxs-lookup"><span data-stu-id="18071-121">C#:</span></span> 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up the rest of the ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

<span data-ttu-id="18071-122">Adja meg vagy frissítés MoveCost dinamikusan a szolgáltatás a szolgáltatás létrehozása után is:</span><span class="sxs-lookup"><span data-stu-id="18071-122">You can also specify or update MoveCost dynamically for a service after the service has been created:</span></span> 

<span data-ttu-id="18071-123">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="18071-123">PowerShell:</span></span> 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

<span data-ttu-id="18071-124">C#:</span><span class="sxs-lookup"><span data-stu-id="18071-124">C#:</span></span>

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a><span data-ttu-id="18071-125">Dinamikusan adja meg a replika alapon áthelyezés költség</span><span class="sxs-lookup"><span data-stu-id="18071-125">Dynamically specifying move cost on a per-replica basis</span></span>

<span data-ttu-id="18071-126">Az előző kódtöredékek segédanyagokra MoveCost megadásának egyszerre a teljes szolgáltatás kívül a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="18071-126">The preceding snippets are all for specifying MoveCost for a whole service at once from outside the service itself.</span></span> <span data-ttu-id="18071-127">Azonban áthelyezése költsége leghasznosabb kell, ha az élettartamot időbeli változásainak egy adott szolgáltatás objektum áthelyezése költségét.</span><span class="sxs-lookup"><span data-stu-id="18071-127">However, move cost is most useful is when the move cost of a specific service object changes over its lifespan.</span></span> <span data-ttu-id="18071-128">A szolgáltatásoknak valószínűleg a legjobb meghatározni, hogy hogyan költséges azok helyezze át egy adott idő alatt, mert nincs a jelentés a saját egyéni költség áthelyezése futásidőben szolgáltatásainak API.</span><span class="sxs-lookup"><span data-stu-id="18071-128">Since the services themselves probably have the best idea of how costly they are to move a given time, there's an API for services to report their own individual move cost during runtime.</span></span> 

<span data-ttu-id="18071-129">C#:</span><span class="sxs-lookup"><span data-stu-id="18071-129">C#:</span></span>

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a><span data-ttu-id="18071-130">Áthelyezési költség hatása</span><span class="sxs-lookup"><span data-stu-id="18071-130">Impact of move cost</span></span>
<span data-ttu-id="18071-131">MoveCost négy szintje van: nulla, alacsony, közepes és magas.</span><span class="sxs-lookup"><span data-stu-id="18071-131">MoveCost has four levels: Zero, Low, Medium, and High.</span></span> <span data-ttu-id="18071-132">MoveCosts vannak egymáshoz, kivéve a nulla.</span><span class="sxs-lookup"><span data-stu-id="18071-132">MoveCosts are relative to each other, except for Zero.</span></span> <span data-ttu-id="18071-133">Nulla áthelyezés költség azt jelenti, hogy a mozgás szabad, nem kell csökkenti a pontszám megoldás.</span><span class="sxs-lookup"><span data-stu-id="18071-133">Zero move cost means that movement is free and should not count against the score of the solution.</span></span> <span data-ttu-id="18071-134">A magas does költsége move beállítást *nem* biztosítja, hogy a replika egy helyen tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="18071-134">Setting your move cost to High does *not* guarantee that the replica stays in one place.</span></span>

<span data-ttu-id="18071-135"><center>
![Helyezze át a költség tényezőként adatátviteli replikák kiválasztása][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="18071-135"><center>
![Move cost as a factor in selecting replicas for movement][Image1]
</center></span></span>

<span data-ttu-id="18071-136">Keresse meg a a megoldást, amely átfogó zavartalanul okozza, és közben továbbra is érkező egyenértékű egyenleg eléréséhez legegyszerűbb MoveCost segítségével.</span><span class="sxs-lookup"><span data-stu-id="18071-136">MoveCost helps you find the solutions that cause the least disruption overall and are easiest to achieve while still arriving at equivalent balance.</span></span> <span data-ttu-id="18071-137">A szolgáltatás fogalmát költség lehet számos elemet viszonyítva.</span><span class="sxs-lookup"><span data-stu-id="18071-137">A service’s notion of cost can be relative to many things.</span></span> <span data-ttu-id="18071-138">A leggyakoribb tényezők az áthelyezés költsége kiszámításakor a következők:</span><span class="sxs-lookup"><span data-stu-id="18071-138">The most common factors in calculating your move cost are:</span></span>

- <span data-ttu-id="18071-139">Állam vagy olyan adatok, amelyek a szolgáltatás áthelyezése mennyisége.</span><span class="sxs-lookup"><span data-stu-id="18071-139">The amount of state or data that the service has to move.</span></span>
- <span data-ttu-id="18071-140">Ügyfelek leválasztása a költségét.</span><span class="sxs-lookup"><span data-stu-id="18071-140">The cost of disconnection of clients.</span></span> <span data-ttu-id="18071-141">Helyezze át az elsődleges replika általában költségesebb, mint a áthelyezi egy másodlagos másodpéldány költségét.</span><span class="sxs-lookup"><span data-stu-id="18071-141">Moving a primary replica is usually more costly than the cost of moving a secondary replica.</span></span>
- <span data-ttu-id="18071-142">Az üzenetsoroktól művelet megszakítása költsége.</span><span class="sxs-lookup"><span data-stu-id="18071-142">The cost of interrupting an in-flight operation.</span></span> <span data-ttu-id="18071-143">Bizonyos műveletek, az adatok tárolásához szint, és egy ügyfél hívást végre műveletek esetében költséges.</span><span class="sxs-lookup"><span data-stu-id="18071-143">Some operations at the data store level or operations performed in response to a client call are costly.</span></span> <span data-ttu-id="18071-144">Egy bizonyos mértékig után nem kívánja azokat le, ha nincs.</span><span class="sxs-lookup"><span data-stu-id="18071-144">After a certain point, you don’t want to stop them if you don’t have to.</span></span> <span data-ttu-id="18071-145">Ezért amíg a művelet van folyamatban, növelheti a move költségek csökkentése érdekében, hogy ez a szolgáltatás-objektum.</span><span class="sxs-lookup"><span data-stu-id="18071-145">So while the operation is going on, you increase the move cost of this service object to reduce the likelihood that it moves.</span></span> <span data-ttu-id="18071-146">Amikor a műveletet hajtja végre, a költség be visszaállítása a szokásos módon folytatódik.</span><span class="sxs-lookup"><span data-stu-id="18071-146">When the operation is done, you set the cost back to normal.</span></span>

## <a name="enabling-move-cost-in-your-cluster"></a><span data-ttu-id="18071-147">Áthelyezési költség, a fürt engedélyezése</span><span class="sxs-lookup"><span data-stu-id="18071-147">Enabling move cost in your cluster</span></span>
<span data-ttu-id="18071-148">Ahhoz, hogy a részletesebb MoveCosts figyelembe kell venni MoveCost a fürtben engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="18071-148">In order for the more granular MoveCosts to be taken into account, MoveCost must be enabled in your cluster.</span></span> <span data-ttu-id="18071-149">Ha ez a beállítás számbavételi helyezi át az alapértelmezett mód MoveCost kiszámításához, és MoveCost jelentéseket a rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="18071-149">Without this setting, the default mode of counting moves is used for calculating MoveCost, and MoveCost reports are ignored.</span></span>


<span data-ttu-id="18071-150">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="18071-150">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

<span data-ttu-id="18071-151">az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:</span><span class="sxs-lookup"><span data-stu-id="18071-151">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "UseMoveCostReports",
          "value": "true"
      }
    ]
  }
]
```

## <a name="next-steps"></a><span data-ttu-id="18071-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18071-152">Next steps</span></span>
- <span data-ttu-id="18071-153">Service Fabric fürt erőforrás-kezelő metrikákat használ a használat és a kapacitás, a fürt kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="18071-153">Service Fabric Cluster Resource Manger uses metrics to manage consumption and capacity in the cluster.</span></span> <span data-ttu-id="18071-154">A metrikák és konfigurálásuk módját kapcsolatos további tudnivalókért tekintse meg [hálózatierőforrás-fogyasztás kezelése és a metrikák a Service Fabric terheléselosztási](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="18071-154">To learn more about metrics and how to configure them, check out [Managing resource consumption and load in Service Fabric with metrics](service-fabric-cluster-resource-manager-metrics.md).</span></span>
- <span data-ttu-id="18071-155">Hogyan kezeli a fürt erőforrás-kezelő, és elosztja a terhelést a fürt kapcsolatos információkért tekintse meg [a Service Fabric-fürt terheléselosztási](service-fabric-cluster-resource-manager-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="18071-155">To learn about how the Cluster Resource Manager manages and balances load in the cluster, check out [Balancing your Service Fabric cluster](service-fabric-cluster-resource-manager-balancing.md).</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png

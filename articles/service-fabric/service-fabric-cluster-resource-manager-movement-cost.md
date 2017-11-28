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
ms.openlocfilehash: 65d4ac73efffcf7b25b1e95da6f9012a9238cb75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-movement-cost"></a><span data-ttu-id="252a8-103">A mozgás költsége szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="252a8-103">Service movement cost</span></span>
<span data-ttu-id="252a8-104">Service Fabric fürt erőforrás-kezelő hello tényezővel úgy ítéli meg, milyen módosítások toomake tooa fürt ezeket a módosításokat hello költsége toodetermine tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="252a8-104">A factor that hello Service Fabric Cluster Resource Manager considers when trying toodetermine what changes toomake tooa cluster is hello cost of those changes.</span></span> <span data-ttu-id="252a8-105">"költség" Hello fogalmát elleni mennyi hello fürt javítható ki forog.</span><span class="sxs-lookup"><span data-stu-id="252a8-105">hello notion of "cost" is traded off against how much hello cluster can be improved.</span></span> <span data-ttu-id="252a8-106">Költség Beleszámítja a terheléselosztás, töredezettségmentesítés és egyéb követelmények szolgáltatások áthelyezésekor.</span><span class="sxs-lookup"><span data-stu-id="252a8-106">Cost is factored in when moving services for balancing, defragmentation, and other requirements.</span></span> <span data-ttu-id="252a8-107">hello célja toomeet hello követelményeinek hello legalább zavaró vagy drága módon.</span><span class="sxs-lookup"><span data-stu-id="252a8-107">hello goal is toomeet hello requirements in hello least disruptive or expensive way.</span></span> 

<span data-ttu-id="252a8-108">Helyezze át a szolgáltatások költségek CPU-idő, és a hálózati sávszélesség minimális.</span><span class="sxs-lookup"><span data-stu-id="252a8-108">Moving services costs CPU time and network bandwidth at a minimum.</span></span> <span data-ttu-id="252a8-109">Állapotalapú szolgáltatások esetén ehhez szükséges szolgáltatások, ezzel további memóriát és lemez hello állapot másolása.</span><span class="sxs-lookup"><span data-stu-id="252a8-109">For stateful services, it requires copying hello state of those services, consuming additional memory and disk.</span></span> <span data-ttu-id="252a8-110">Minimalizálja a megoldások hello költségét, hogy az Azure Service Fabric-fürt erőforrás-kezelő felmerül hello segítségével győződjön meg arról, hogy hello fürt erőforrásait feleslegesen nem fordított.</span><span class="sxs-lookup"><span data-stu-id="252a8-110">Minimizing hello cost of solutions that hello Azure Service Fabric Cluster Resource Manager comes up with helps ensure that hello cluster's resources aren't spent unnecessarily.</span></span> <span data-ttu-id="252a8-111">Azonban nem szeretnénk tooignore megoldások, amelyek jelentősen javíthatják a hello fürterőforrások hello kiosztásáért.</span><span class="sxs-lookup"><span data-stu-id="252a8-111">However, you also don’t want tooignore solutions that would significantly improve hello allocation of resources in hello cluster.</span></span>

<span data-ttu-id="252a8-112">hello fürt erőforrás-kezelő számítástechnikai költségeket, és korlátozza azokat, amíg toomanage hello fürt megkísérli két lehetősége van.</span><span class="sxs-lookup"><span data-stu-id="252a8-112">hello Cluster Resource Manager has two ways of computing costs and limiting them while it tries toomanage hello cluster.</span></span> <span data-ttu-id="252a8-113">hello első mechanizmus egyszerűen számolása minden elindított áthelyezésre is volna.</span><span class="sxs-lookup"><span data-stu-id="252a8-113">hello first mechanism is simply counting every move that it would make.</span></span> <span data-ttu-id="252a8-114">Ha két megoldások akkor jönnek létre a hello kapcsolatos azonos egyenleg (pontszám), majd hello fürt erőforrás-kezelő egyik hello inkább a hello legalacsonyabb költség (a kurzor teljes száma).</span><span class="sxs-lookup"><span data-stu-id="252a8-114">If two solutions are generated with about hello same balance (score), then hello Cluster Resource Manager prefers hello one with hello lowest cost (total number of moves).</span></span>

<span data-ttu-id="252a8-115">Ezt a stratégiát akkor is működik.</span><span class="sxs-lookup"><span data-stu-id="252a8-115">This strategy works well.</span></span> <span data-ttu-id="252a8-116">De az alapértelmezett vagy statikus, nem valószínű, hogy az összes lépés egyenlőek bármely összetett rendszerben.</span><span class="sxs-lookup"><span data-stu-id="252a8-116">But as with default or static loads, it's unlikely in any complex system that all moves are equal.</span></span> <span data-ttu-id="252a8-117">Valószínűleg toobe sokkal több hátránnyal között.</span><span class="sxs-lookup"><span data-stu-id="252a8-117">Some are likely toobe much more expensive.</span></span>

## <a name="setting-move-costs"></a><span data-ttu-id="252a8-118">A beállítás költségek áthelyezése</span><span class="sxs-lookup"><span data-stu-id="252a8-118">Setting Move Costs</span></span> 
<span data-ttu-id="252a8-119">Hello alapértelmezett áthelyezés költség szolgáltatás létrehozásakor adhatók meg:</span><span class="sxs-lookup"><span data-stu-id="252a8-119">You can specify hello default move cost for a service when it is created:</span></span>

<span data-ttu-id="252a8-120">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="252a8-120">PowerShell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

<span data-ttu-id="252a8-121">C#:</span><span class="sxs-lookup"><span data-stu-id="252a8-121">C#:</span></span> 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up hello rest of hello ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

<span data-ttu-id="252a8-122">Adja meg, vagy frissíteni MoveCost dinamikusan egy szolgáltatáshoz hello szolgáltatás létrehozása után is:</span><span class="sxs-lookup"><span data-stu-id="252a8-122">You can also specify or update MoveCost dynamically for a service after hello service has been created:</span></span> 

<span data-ttu-id="252a8-123">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="252a8-123">PowerShell:</span></span> 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

<span data-ttu-id="252a8-124">C#:</span><span class="sxs-lookup"><span data-stu-id="252a8-124">C#:</span></span>

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a><span data-ttu-id="252a8-125">Dinamikusan adja meg a replika alapon áthelyezés költség</span><span class="sxs-lookup"><span data-stu-id="252a8-125">Dynamically specifying move cost on a per-replica basis</span></span>

<span data-ttu-id="252a8-126">hello előző kódtöredékek segédanyagokra egyszerre egy külső hello szolgáltatás teljes kiszolgálásra MoveCost meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="252a8-126">hello preceding snippets are all for specifying MoveCost for a whole service at once from outside hello service itself.</span></span> <span data-ttu-id="252a8-127">Azonban áthelyezése költsége leghasznosabb kell, ha az élettartamot időbeli változásainak hello áthelyezés költségét, egy adott objektumot.</span><span class="sxs-lookup"><span data-stu-id="252a8-127">However, move cost is most useful is when hello move cost of a specific service object changes over its lifespan.</span></span> <span data-ttu-id="252a8-128">Óta hello valószínűleg szolgáltatások maguk hello legjobb meghatározni, hogy hogyan költséges azok toomove egy adott idő alatt van, és van egy API-t, a szolgáltatások tooreport saját egyedi áthelyezés futásidőben költsége.</span><span class="sxs-lookup"><span data-stu-id="252a8-128">Since hello services themselves probably have hello best idea of how costly they are toomove a given time, there's an API for services tooreport their own individual move cost during runtime.</span></span> 

<span data-ttu-id="252a8-129">C#:</span><span class="sxs-lookup"><span data-stu-id="252a8-129">C#:</span></span>

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a><span data-ttu-id="252a8-130">Áthelyezési költség hatása</span><span class="sxs-lookup"><span data-stu-id="252a8-130">Impact of move cost</span></span>
<span data-ttu-id="252a8-131">MoveCost négy szintje van: nulla, alacsony, közepes és magas.</span><span class="sxs-lookup"><span data-stu-id="252a8-131">MoveCost has four levels: Zero, Low, Medium, and High.</span></span> <span data-ttu-id="252a8-132">MoveCosts más, kivéve a nulla relatív tooeach.</span><span class="sxs-lookup"><span data-stu-id="252a8-132">MoveCosts are relative tooeach other, except for Zero.</span></span> <span data-ttu-id="252a8-133">Nulla áthelyezés költség azt jelenti, hogy a mozgás szabad, nem kell csökkenti hello pontszám hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="252a8-133">Zero move cost means that movement is free and should not count against hello score of hello solution.</span></span> <span data-ttu-id="252a8-134">A beállítás a költségeket, tooHigh áthelyezés does *nem* garantálja, hogy hello replika egy helyen tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="252a8-134">Setting your move cost tooHigh does *not* guarantee that hello replica stays in one place.</span></span>

<span data-ttu-id="252a8-135"><center>
![Helyezze át a költség tényezőként adatátviteli replikák kiválasztása][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="252a8-135"><center>
![Move cost as a factor in selecting replicas for movement][Image1]
</center></span></span>

<span data-ttu-id="252a8-136">MoveCost segít, hogy OK zavartalanul teljes hello és legegyszerűbb tooachieve közben továbbra is egyenértékű egyenleg bejövő hello megoldások keresése.</span><span class="sxs-lookup"><span data-stu-id="252a8-136">MoveCost helps you find hello solutions that cause hello least disruption overall and are easiest tooachieve while still arriving at equivalent balance.</span></span> <span data-ttu-id="252a8-137">A szolgáltatás fogalmát költség relatív toomany dolog lehet.</span><span class="sxs-lookup"><span data-stu-id="252a8-137">A service’s notion of cost can be relative toomany things.</span></span> <span data-ttu-id="252a8-138">hello leggyakoribb tényezők az áthelyezés költsége kiszámításakor a következők:</span><span class="sxs-lookup"><span data-stu-id="252a8-138">hello most common factors in calculating your move cost are:</span></span>

- <span data-ttu-id="252a8-139">hello az állapot vagy a hello szolgáltatás meglétének toomove adatok mennyisége.</span><span class="sxs-lookup"><span data-stu-id="252a8-139">hello amount of state or data that hello service has toomove.</span></span>
- <span data-ttu-id="252a8-140">ügyfelek leválasztása hello költségét.</span><span class="sxs-lookup"><span data-stu-id="252a8-140">hello cost of disconnection of clients.</span></span> <span data-ttu-id="252a8-141">Helyezze át az elsődleges replika általában költségesebb, mint egy másodlagos másodpéldány hello költségeit.</span><span class="sxs-lookup"><span data-stu-id="252a8-141">Moving a primary replica is usually more costly than hello cost of moving a secondary replica.</span></span>
- <span data-ttu-id="252a8-142">az üzenetsoroktól művelet megszakítása hello költségét.</span><span class="sxs-lookup"><span data-stu-id="252a8-142">hello cost of interrupting an in-flight operation.</span></span> <span data-ttu-id="252a8-143">Bizonyos műveleteket hello adatok tárolásához szint vagy válasz tooa ügyfél hívásban végrehajtott műveletek költséges.</span><span class="sxs-lookup"><span data-stu-id="252a8-143">Some operations at hello data store level or operations performed in response tooa client call are costly.</span></span> <span data-ttu-id="252a8-144">Egy bizonyos mértékig után nem szeretné, hogy toostop őket, ha nem kell.</span><span class="sxs-lookup"><span data-stu-id="252a8-144">After a certain point, you don’t want toostop them if you don’t have to.</span></span> <span data-ttu-id="252a8-145">Ezért amíg hello művelet van folyamatban, növelheti hello áthelyezés költségét, a szolgáltatás objektum tooreduce hello kevésbé valószínű, hogy áthelyezi azt.</span><span class="sxs-lookup"><span data-stu-id="252a8-145">So while hello operation is going on, you increase hello move cost of this service object tooreduce hello likelihood that it moves.</span></span> <span data-ttu-id="252a8-146">Amikor hello műveletet hajtja végre, hello költség hátsó toonormal be.</span><span class="sxs-lookup"><span data-stu-id="252a8-146">When hello operation is done, you set hello cost back toonormal.</span></span>

## <a name="enabling-move-cost-in-your-cluster"></a><span data-ttu-id="252a8-147">Áthelyezési költség, a fürt engedélyezése</span><span class="sxs-lookup"><span data-stu-id="252a8-147">Enabling move cost in your cluster</span></span>
<span data-ttu-id="252a8-148">Ahhoz, hogy hello részletesebb MoveCosts toobe figyelembe venni, MoveCost engedélyezni kell a fürtben.</span><span class="sxs-lookup"><span data-stu-id="252a8-148">In order for hello more granular MoveCosts toobe taken into account, MoveCost must be enabled in your cluster.</span></span> <span data-ttu-id="252a8-149">Ha ez a beállítás hello alapértelmezett mód a kurzor számbavételi MoveCost kiszámításához és MoveCost jelentéseket a rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="252a8-149">Without this setting, hello default mode of counting moves is used for calculating MoveCost, and MoveCost reports are ignored.</span></span>


<span data-ttu-id="252a8-150">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="252a8-150">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

<span data-ttu-id="252a8-151">az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:</span><span class="sxs-lookup"><span data-stu-id="252a8-151">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="252a8-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="252a8-152">Next steps</span></span>
- <span data-ttu-id="252a8-153">Service Fabric fürt erőforrás-kezelő a metrikák toomanage használat és a kapacitás hello fürt használja.</span><span class="sxs-lookup"><span data-stu-id="252a8-153">Service Fabric Cluster Resource Manger uses metrics toomanage consumption and capacity in hello cluster.</span></span> <span data-ttu-id="252a8-154">További információk a metrikák toolearn és hogyan tooconfigure őket, tekintse meg [hálózatierőforrás-fogyasztás kezelése és a metrikák a Service Fabric terheléselosztási](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="252a8-154">toolearn more about metrics and how tooconfigure them, check out [Managing resource consumption and load in Service Fabric with metrics](service-fabric-cluster-resource-manager-metrics.md).</span></span>
- <span data-ttu-id="252a8-155">toolearn arról, hogyan hello fürt erőforrás-kezelő felügyeli, és elosztja a fürt hello terhelését, tekintse meg [a Service Fabric-fürt terheléselosztási](service-fabric-cluster-resource-manager-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="252a8-155">toolearn about how hello Cluster Resource Manager manages and balances load in hello cluster, check out [Balancing your Service Fabric cluster](service-fabric-cluster-resource-manager-balancing.md).</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png

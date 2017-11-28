---
title: "aaaBalance az Azure Service Fabric-fürt |} Microsoft Docs"
description: "Egy bevezető toobalancing hello Service Fabric fürt erőforrás-kezelő a fürt."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5f7ad2f5cf4cfb3751a860f5293b03d2d5266d99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="balancing-your-service-fabric-cluster"></a><span data-ttu-id="65bd3-103">A service fabric-fürt terheléselosztás</span><span class="sxs-lookup"><span data-stu-id="65bd3-103">Balancing your service fabric cluster</span></span>
<span data-ttu-id="65bd3-104">Service Fabric fürt erőforrás-kezelő hello támogatja a dinamikus terheléselosztó módosításokat, a reakciójú tooadditions vagy a csomópontok vagy szolgáltatások eltávolítására.</span><span class="sxs-lookup"><span data-stu-id="65bd3-104">hello Service Fabric Cluster Resource Manager supports dynamic load changes, reacting tooadditions or removals of nodes or services.</span></span> <span data-ttu-id="65bd3-105">Is automatikusan javítja a megkötés megsértésének, és proaktív újra egyensúlyba hozza a hello fürt.</span><span class="sxs-lookup"><span data-stu-id="65bd3-105">It also automatically corrects constraint violations, and proactively rebalances hello cluster.</span></span> <span data-ttu-id="65bd3-106">De gyakoriságát. Ezek a műveletek készít, és mi váltja ki őket?</span><span class="sxs-lookup"><span data-stu-id="65bd3-106">But how often are these actions taken, and what triggers them?</span></span>

<span data-ttu-id="65bd3-107">Három különböző kategóriába sorolhatók munkahelyi adott hello fürt erőforrás-kezelő hajt végre.</span><span class="sxs-lookup"><span data-stu-id="65bd3-107">There are three different categories of work that hello Cluster Resource Manager performs.</span></span> <span data-ttu-id="65bd3-108">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="65bd3-108">They are:</span></span>

1. <span data-ttu-id="65bd3-109">Elhelyezését – ebben a szakaszban foglalkozik bármely állapot-nyilvántartó replikák vagy hiányzó állapotmentes példányok.</span><span class="sxs-lookup"><span data-stu-id="65bd3-109">Placement – this stage deals with placing any stateful replicas or stateless instances that are missing.</span></span> <span data-ttu-id="65bd3-110">Elhelyezési tartalmazza az új szolgáltatásokat és a kezelési állapot-nyilvántartó replikák vagy állapotmentes példányokat, amelyek nem tudták.</span><span class="sxs-lookup"><span data-stu-id="65bd3-110">Placement includes both new services and handling stateful replicas or stateless instances that have failed.</span></span> <span data-ttu-id="65bd3-111">Itt törlése és a replikák és példányok eldobása történik.</span><span class="sxs-lookup"><span data-stu-id="65bd3-111">Deleting and dropping replicas or instances are handled here.</span></span>
2. <span data-ttu-id="65bd3-112">Korlátozás ellenőrzi – ebben a fázisban ellenőrzi, és kijavítja a megsértésének hello különböző placement Constraints korlátozásokat (szabály) hello rendszeren belül.</span><span class="sxs-lookup"><span data-stu-id="65bd3-112">Constraint Checks – this stage checks for and corrects violations of hello different placement constraints (rules) within hello system.</span></span> <span data-ttu-id="65bd3-113">Szabályok Példák többek között a győződjön meg arról, hogy a csomópontok nem: keresztül kapacitás és, hogy teljesülnek-e a szolgáltatás egy elhelyezési korlátozás.</span><span class="sxs-lookup"><span data-stu-id="65bd3-113">Examples of rules are things like ensuring that nodes are not over capacity and that a service’s placement constraints are met.</span></span>
3. <span data-ttu-id="65bd3-114">Terheléselosztás – ebben a fázisban ellenőrzi, hogy toosee Ha újraelosztás szükséges konfigurált hello alapján szükséges különböző metrikák elosztás szintjét.</span><span class="sxs-lookup"><span data-stu-id="65bd3-114">Balancing – this stage checks toosee if rebalancing is necessary based on hello configured desired level of balance for different metrics.</span></span> <span data-ttu-id="65bd3-115">Ha így kísérel meg toofind hello az elrendezést a fürt, amely több elosztott terhelésű.</span><span class="sxs-lookup"><span data-stu-id="65bd3-115">If so it attempts toofind an arrangement in hello cluster that is more balanced.</span></span>

## <a name="configuring-cluster-resource-manager-timers"></a><span data-ttu-id="65bd3-116">Erőforrás-kezelő fürt időzítők konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65bd3-116">Configuring Cluster Resource Manager Timers</span></span>
<span data-ttu-id="65bd3-117">vezérlők körüli terheléselosztás első készlete hello olyan időzítőket.</span><span class="sxs-lookup"><span data-stu-id="65bd3-117">hello first set of controls around balancing are a set of timers.</span></span> <span data-ttu-id="65bd3-118">Ezek időzítők szabályozására, milyen gyakran hello fürt erőforrás-kezelő hello fürt megvizsgálja és korrekciós műveleteket hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="65bd3-118">These timers govern how often hello Cluster Resource Manager examines hello cluster and takes corrective actions.</span></span>

<span data-ttu-id="65bd3-119">Egy másik számlálót gyakoriságát szabályzó egyes fürt erőforrás-kezelő tehet javításokat hello különböző típusaival vezérli.</span><span class="sxs-lookup"><span data-stu-id="65bd3-119">Each of these different types of corrections hello Cluster Resource Manager can make is controlled by a different timer that governs its frequency.</span></span> <span data-ttu-id="65bd3-120">Minden egyes időzítő következik be, amikor hello feladata ütemezve.</span><span class="sxs-lookup"><span data-stu-id="65bd3-120">When each timer fires, hello task is scheduled.</span></span> <span data-ttu-id="65bd3-121">Alapértelmezés szerint a Resource Manager hello:</span><span class="sxs-lookup"><span data-stu-id="65bd3-121">By default hello Resource Manager:</span></span>

* <span data-ttu-id="65bd3-122">megvizsgálja az állapotát, és alkalmazza a frissítéseket (például, hogy a csomópont nem működik a rögzítés) minden 1/10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="65bd3-122">scans its state and applies updates (like recording that a node is down) every 1/10th of a second</span></span>
* <span data-ttu-id="65bd3-123">hello elhelyezési ellenőrzést jelzőjének beállítása</span><span class="sxs-lookup"><span data-stu-id="65bd3-123">sets hello placement check flag</span></span> 
* <span data-ttu-id="65bd3-124">minden második hello megkötés ellenőrzés jelzőjének beállítása</span><span class="sxs-lookup"><span data-stu-id="65bd3-124">sets hello constraint check flag every second</span></span>
* <span data-ttu-id="65bd3-125">terheléselosztás jelző öt másodpercenként hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="65bd3-125">sets hello balancing flag every five seconds.</span></span>

<span data-ttu-id="65bd3-126">Ezek időzítők szabályozó hello konfigurációs többek között az alábbi:</span><span class="sxs-lookup"><span data-stu-id="65bd3-126">Examples of hello configuration governing these timers are below:</span></span>

<span data-ttu-id="65bd3-127">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="65bd3-127">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

<span data-ttu-id="65bd3-128">az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:</span><span class="sxs-lookup"><span data-stu-id="65bd3-128">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PLBRefreshGap",
          "value": "0.10"
      },
      {
          "name": "MinPlacementInterval",
          "value": "1.0"
      },
      {
          "name": "MinConstraintCheckInterval",
          "value": "1.0"
      },
      {
          "name": "MinLoadBalancingInterval",
          "value": "5.0"
      }
    ]
  }
]
```

<span data-ttu-id="65bd3-129">Ma hello fürt erőforrás-kezelő csak műveletek egyikét hajtja végre ezek egyszerre, egymás után.</span><span class="sxs-lookup"><span data-stu-id="65bd3-129">Today hello Cluster Resource Manager only performs one of these actions at a time, sequentially.</span></span> <span data-ttu-id="65bd3-130">Ezért azt toothese időzítők intervallumként"minimális" utal, és hello beolvasása hajt végre, amikor hello időzítők lépjen ki a jelzőként"beállítás" műveleteket.</span><span class="sxs-lookup"><span data-stu-id="65bd3-130">This is why we refer toothese timers as “minimum intervals” and hello actions that get taken when hello timers go off as "setting flags".</span></span> <span data-ttu-id="65bd3-131">Például a hello fürt erőforrás-kezelő gondoskodik függőben lévő kérelmek toocreate szolgáltatások hello fürt terheléselosztás előtt.</span><span class="sxs-lookup"><span data-stu-id="65bd3-131">For example, hello Cluster Resource Manager takes care of pending requests toocreate services before balancing hello cluster.</span></span> <span data-ttu-id="65bd3-132">Ahogy látja, időszakonkénti hello alapértelmezett megadott, hello fürt erőforrás-kezelő keres semmit az igényeinek toodo gyakran.</span><span class="sxs-lookup"><span data-stu-id="65bd3-132">As you can see by hello default time intervals specified, hello Cluster Resource Manager scans for anything it needs toodo frequently.</span></span> <span data-ttu-id="65bd3-133">Általában ez azt jelenti, hogy minden lépése során végrehajtott módosítások hello készlete kis.</span><span class="sxs-lookup"><span data-stu-id="65bd3-133">Normally this means that hello set of changes made during each step is small.</span></span> <span data-ttu-id="65bd3-134">Kis változtatások gyakran lehetővé teszi a fürt erőforrás-kezelő toobe hello rugalmas amikor dolgokat okozhat hello fürt.</span><span class="sxs-lookup"><span data-stu-id="65bd3-134">Making small changes frequently allows hello Cluster Resource Manager toobe responsive when things happen in hello cluster.</span></span> <span data-ttu-id="65bd3-135">hello alapértelmezett időzítők adjon meg néhány kötegelés óta hello számos azonos típusú eseményeket általában toooccur egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="65bd3-135">hello default timers provide some batching since many of hello same types of events tend toooccur simultaneously.</span></span> 

<span data-ttu-id="65bd3-136">Például ha csomópontok nem végezhetnek úgy teljes tartalék tartományok egyszerre.</span><span class="sxs-lookup"><span data-stu-id="65bd3-136">For example, when nodes fail they can do so entire fault domains at a time.</span></span> <span data-ttu-id="65bd3-137">Minden ilyen hibához során hello következő állapotban vannak rögzített frissítés után hello *PLBRefreshGap*.</span><span class="sxs-lookup"><span data-stu-id="65bd3-137">All these failures are captured during hello next state update after hello *PLBRefreshGap*.</span></span> <span data-ttu-id="65bd3-138">hello határozzák meg hello elhelyezési korlátozás ellenőrzése a következő és terheléselosztási futtatása során.</span><span class="sxs-lookup"><span data-stu-id="65bd3-138">hello corrections are determined during hello following placement, constraint check, and balancing runs.</span></span> <span data-ttu-id="65bd3-139">Alapértelmezett hello által fürt erőforrás-kezelő van nem keresztül hello fürt módosítások órányi vizsgálatát, majd próbálkozzon tooaddress összes módosítás egyszerre.</span><span class="sxs-lookup"><span data-stu-id="65bd3-139">By default hello Cluster Resource Manager is not scanning through hours of changes in hello cluster and trying tooaddress all changes at once.</span></span> <span data-ttu-id="65bd3-140">Ezzel a forgalom toobursts vezet.</span><span class="sxs-lookup"><span data-stu-id="65bd3-140">Doing so would lead toobursts of churn.</span></span>

<span data-ttu-id="65bd3-141">Fürt erőforrás-kezelő hello kell néhány további információt toodetermine Ha hello fürt imbalanced.</span><span class="sxs-lookup"><span data-stu-id="65bd3-141">hello Cluster Resource Manager also needs some additional information toodetermine if hello cluster imbalanced.</span></span> <span data-ttu-id="65bd3-142">Az adott konfigurációs két darabjai tudunk: *BalancingThresholds* és *ActivityThresholds*.</span><span class="sxs-lookup"><span data-stu-id="65bd3-142">For that we have two other pieces of configuration: *BalancingThresholds* and *ActivityThresholds*.</span></span>

## <a name="balancing-thresholds"></a><span data-ttu-id="65bd3-143">Terheléselosztás küszöbértékek</span><span class="sxs-lookup"><span data-stu-id="65bd3-143">Balancing thresholds</span></span>
<span data-ttu-id="65bd3-144">A terheléselosztás küszöbértéke hello fő vezérlő újraelosztás indítására.</span><span class="sxs-lookup"><span data-stu-id="65bd3-144">A Balancing Threshold is hello main control for triggering rebalancing.</span></span> <span data-ttu-id="65bd3-145">hello terheléselosztási a metrika küszöbértéke egy _arány_.</span><span class="sxs-lookup"><span data-stu-id="65bd3-145">hello Balancing Threshold for a metric is a _ratio_.</span></span> <span data-ttu-id="65bd3-146">Ha hello terhelés olyan metrikajelentés hello a legtöbb hello mennyisége terhelés legalább betöltött hello osztva csomópont csomópont meghaladja-e az adott metrika *BalancingThreshold*, akkor hello fürt imbalanced.</span><span class="sxs-lookup"><span data-stu-id="65bd3-146">If hello load for a metric on hello most loaded node divided by hello amount of load on hello least loaded node exceeds that metric's *BalancingThreshold*, then hello cluster is imbalanced.</span></span> <span data-ttu-id="65bd3-147">Ennek eredményeképpen a terheléselosztás kiváltott hello fürt erőforrás-kezelő ellenőrzi tovább idő hello.</span><span class="sxs-lookup"><span data-stu-id="65bd3-147">As a result balancing is triggered hello next time hello Cluster Resource Manager checks.</span></span> <span data-ttu-id="65bd3-148">Hello *MinLoadBalancingInterval* időzítő határozza meg, milyen gyakran hello fürt erőforrás-kezelő ellenőrizni kell, hogy ha újraelosztás szükség.</span><span class="sxs-lookup"><span data-stu-id="65bd3-148">hello *MinLoadBalancingInterval* timer defines how often hello Cluster Resource Manager should check if rebalancing is necessary.</span></span> <span data-ttu-id="65bd3-149">Ellenőrzése nem jelenti azt, hogy bármi Baja.</span><span class="sxs-lookup"><span data-stu-id="65bd3-149">Checking doesn't mean that anything happens.</span></span> 

<span data-ttu-id="65bd3-150">Terheléselosztási küszöbértékek meghatározott metrika alapon hello fürt definíciójának részeként.</span><span class="sxs-lookup"><span data-stu-id="65bd3-150">Balancing Thresholds are defined on a per-metric basis as a part of hello cluster definition.</span></span> <span data-ttu-id="65bd3-151">A metrikák további információkért tekintse meg [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="65bd3-151">For more information on metrics, check out [this article](service-fabric-cluster-resource-manager-metrics.md).</span></span>

<span data-ttu-id="65bd3-152">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="65bd3-152">ClusterManifest.xml</span></span>

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

<span data-ttu-id="65bd3-153">az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:</span><span class="sxs-lookup"><span data-stu-id="65bd3-153">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "MetricBalancingThresholds",
    "parameters": [
      {
          "name": "MetricName1",
          "value": "2"
      },
      {
          "name": "MetricName2",
          "value": "3.5"
      }
    ]
  }
]
```

<span data-ttu-id="65bd3-154"><center>
![Terheléselosztási küszöbérték – példa][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="65bd3-154"><center>
![Balancing Threshold Example][Image1]
</center></span></span>

<span data-ttu-id="65bd3-155">Ebben a példában minden szolgáltatás van fel néhány metrika egységárát.</span><span class="sxs-lookup"><span data-stu-id="65bd3-155">In this example, each service is consuming one unit of some metric.</span></span> <span data-ttu-id="65bd3-156">Hello felső példában egy csomópont maximális terhelése hello öt pedig hello legalább két.</span><span class="sxs-lookup"><span data-stu-id="65bd3-156">In hello top example, hello maximum load on a node is five and hello minimum is two.</span></span> <span data-ttu-id="65bd3-157">Tegyük fel, hogy ez a metrika küszöbértéke terheléselosztás hello három.</span><span class="sxs-lookup"><span data-stu-id="65bd3-157">Let’s say that hello balancing threshold for this metric is three.</span></span> <span data-ttu-id="65bd3-158">Mivel a hello arány hello fürt 5/2 = 2.5 és, amely kisebb, mint a hello megadott küszöbérték három, hello fürt terheléselosztás kiegyensúlyozott.</span><span class="sxs-lookup"><span data-stu-id="65bd3-158">Since hello ratio in hello cluster is 5/2 = 2.5 and that is less than hello specified balancing threshold of three, hello cluster is balanced.</span></span> <span data-ttu-id="65bd3-159">Nincs terheléselosztás akkor lesz kiváltva, ha a fürt erőforrás-kezelő hello ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="65bd3-159">No balancing is triggered when hello Cluster Resource Manager checks.</span></span>

<span data-ttu-id="65bd3-160">Hello alsó példában egy csomópont maximális terhelése hello: 10, pedig hello legalább két, ami öt aránya.</span><span class="sxs-lookup"><span data-stu-id="65bd3-160">In hello bottom example, hello maximum load on a node is 10, while hello minimum is two, resulting in a ratio of five.</span></span> <span data-ttu-id="65bd3-161">Öt értéke nagyobb, mint az adott metrika három terheléselosztási küszöbérték kijelölt hello.</span><span class="sxs-lookup"><span data-stu-id="65bd3-161">Five is greater than hello designated balancing threshold of three for that metric.</span></span> <span data-ttu-id="65bd3-162">Ennek eredményeképpen a rebalancing futtató lesz ütemezett tovább idő hello terheléselosztás időzítő következik be.</span><span class="sxs-lookup"><span data-stu-id="65bd3-162">As a result, a rebalancing run will be scheduled next time hello balancing timer fires.</span></span> <span data-ttu-id="65bd3-163">Ilyen helyzetben néhány terhelés általában elosztott tooNode3.</span><span class="sxs-lookup"><span data-stu-id="65bd3-163">In a situation like this some load is usually distributed tooNode3.</span></span> <span data-ttu-id="65bd3-164">Hello Service Fabric fürt erőforrás-kezelő nem használja a mohó megközelítést, mert néhány terhelés kell lenniük elosztott tooNode2.</span><span class="sxs-lookup"><span data-stu-id="65bd3-164">Because hello Service Fabric Cluster Resource Manager doesn't use a greedy approach, some load could also be distributed tooNode2.</span></span> 

<span data-ttu-id="65bd3-165"><center>
![Terheléselosztási küszöbérték példa műveletek][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="65bd3-165"><center>
![Balancing Threshold Example Actions][Image2]
</center></span></span>

> [!NOTE]
> <span data-ttu-id="65bd3-166">Két különböző stratégiák a terhelést a fürt kezelése "Terheléselosztás" kezeli.</span><span class="sxs-lookup"><span data-stu-id="65bd3-166">"Balancing" handles two different strategies for managing load in your cluster.</span></span> <span data-ttu-id="65bd3-167">Erőforrás-kezelő fürt által használt hello hello alapértelmezett stratégiát toodistribute terhelés hello fürt hello csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="65bd3-167">hello default strategy that hello Cluster Resource Manager uses is toodistribute load across hello nodes in hello cluster.</span></span> <span data-ttu-id="65bd3-168">hello más stratégia van [töredezettségmentesítés](service-fabric-cluster-resource-manager-defragmentation-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="65bd3-168">hello other strategy is [defragmentation](service-fabric-cluster-resource-manager-defragmentation-metrics.md).</span></span> <span data-ttu-id="65bd3-169">Töredezettségmentesítéssel során hello azonos terheléselosztás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="65bd3-169">Defragmentation is performed during hello same balancing run.</span></span> <span data-ttu-id="65bd3-170">hello terheléselosztás és a lemeztöredezettség-mentesítés stratégiák használhat a különböző metrikák belül hello ugyanabban a fürtben.</span><span class="sxs-lookup"><span data-stu-id="65bd3-170">hello balancing and defragmentation strategies can be used for different metrics within hello same cluster.</span></span> <span data-ttu-id="65bd3-171">Egy szolgáltatás terheléselosztási és töredezettségmentesítési metrikák is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="65bd3-171">A service can have both balancing and defragmentation metrics.</span></span> <span data-ttu-id="65bd3-172">Töredezettségmentesítés metrikáihoz hello hello aránya hello fürt eseményindítók újraelosztás, ha betölti _alatt_ hello terheléselosztás küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="65bd3-172">For defragmentation metrics, hello ratio of hello loads in hello cluster triggers rebalancing when it is _below_ hello balancing threshold.</span></span> 
>

<span data-ttu-id="65bd3-173">Hello terheléselosztás küszöbérték alatti első célja nem egy explicit.</span><span class="sxs-lookup"><span data-stu-id="65bd3-173">Getting below hello balancing threshold is not an explicit goal.</span></span> <span data-ttu-id="65bd3-174">Terheléselosztási küszöbértékek vannak csak egy *eseményindító*.</span><span class="sxs-lookup"><span data-stu-id="65bd3-174">Balancing Thresholds are just a *trigger*.</span></span> <span data-ttu-id="65bd3-175">Terheléselosztás fut, hello fürt erőforrás-kezelő határozza meg azt is ellenőrizze, milyen fejlesztéseket ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="65bd3-175">When balancing runs, hello Cluster Resource Manager determines what improvements it can make, if any.</span></span> <span data-ttu-id="65bd3-176">Most, mert egy terheléselosztási keresési van kezdődött el nem jelenti azt semmit helyezi át.</span><span class="sxs-lookup"><span data-stu-id="65bd3-176">Just because a balancing search is kicked off doesn't mean anything moves.</span></span> <span data-ttu-id="65bd3-177">Egyes esetekben hello fürtre imbalanced, de túl korlátozott toocorrect.</span><span class="sxs-lookup"><span data-stu-id="65bd3-177">Sometimes hello cluster is imbalanced but too constrained toocorrect.</span></span> <span data-ttu-id="65bd3-178">Azt is megteheti, hello fejlesztései igényel, amelyek túl áthelyezések [költséges](service-fabric-cluster-resource-manager-movement-cost.md)).</span><span class="sxs-lookup"><span data-stu-id="65bd3-178">Alternatively, hello improvements require movements that are too [costly](service-fabric-cluster-resource-manager-movement-cost.md)).</span></span>

## <a name="activity-thresholds"></a><span data-ttu-id="65bd3-179">Tevékenység küszöbértékek</span><span class="sxs-lookup"><span data-stu-id="65bd3-179">Activity thresholds</span></span>
<span data-ttu-id="65bd3-180">Egyes esetekben, annak ellenére, hogy csomópontok viszonylag imbalanced, hello *teljes* hello fürt mennyisége pedig kevés.</span><span class="sxs-lookup"><span data-stu-id="65bd3-180">Sometimes, although nodes are relatively imbalanced, hello *total* amount of load in hello cluster is low.</span></span> <span data-ttu-id="65bd3-181">betöltési hello hiánya lehet egy átmeneti dip, vagy mert hello fürt új, és csak az első bootstrapped.</span><span class="sxs-lookup"><span data-stu-id="65bd3-181">hello lack of load could be a transient dip, or because hello cluster is new and just getting bootstrapped.</span></span> <span data-ttu-id="65bd3-182">Mindkét esetben kívánt terheléselosztási hello fürt, mert kevés toobe szerzett toospend idő.</span><span class="sxs-lookup"><span data-stu-id="65bd3-182">In either case, you may not want toospend time balancing hello cluster because there’s little toobe gained.</span></span> <span data-ttu-id="65bd3-183">Ha hello fürt mentek keresztül, terheléselosztás, ehhez töltött hálózati és számítási erőforrások toomove dolgot körül anélkül, hogy minden nagy *abszolút* különbség.</span><span class="sxs-lookup"><span data-stu-id="65bd3-183">If hello cluster underwent balancing, you’d spend network and compute resources toomove things around without making any large *absolute* difference.</span></span> <span data-ttu-id="65bd3-184">felesleges tooavoid helyezi, nincs egy másik vezérlő tevékenység küszöbértékek néven ismert.</span><span class="sxs-lookup"><span data-stu-id="65bd3-184">tooavoid unnecessary moves, there’s another control known as Activity Thresholds.</span></span> <span data-ttu-id="65bd3-185">Tevékenység küszöbértékek lehetővé teszi bizonyos abszolút alsó határa tevékenység toospecify.</span><span class="sxs-lookup"><span data-stu-id="65bd3-185">Activity Thresholds allows you toospecify some absolute lower bound for activity.</span></span> <span data-ttu-id="65bd3-186">Ha nem a csomópont a küszöbérték feletti, terheléselosztás nem indított, még akkor is, ha hello terheléselosztás küszöbérték elérése.</span><span class="sxs-lookup"><span data-stu-id="65bd3-186">If no node is over this threshold, balancing isn't triggered even if hello Balancing Threshold is met.</span></span>

<span data-ttu-id="65bd3-187">Tegyük fel, azt megőrizze-e a három, ez a mérőszám a terheléselosztás küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="65bd3-187">Let’s say that we retain our Balancing Threshold of three for this metric.</span></span> <span data-ttu-id="65bd3-188">Tételezzük is fel kell egy tevékenység 1536 küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="65bd3-188">Let's also say we have an Activity Threshold of 1536.</span></span> <span data-ttu-id="65bd3-189">Hello első esetben imbalanced / hello hello fürt pedig küszöbérték terheléselosztás nincs egyetlen csomópont megfelel-e tevékenység a küszöbérték, a történik, ezért nem.</span><span class="sxs-lookup"><span data-stu-id="65bd3-189">In hello first case, while hello cluster is imbalanced per hello Balancing Threshold there's no node meets that Activity Threshold, so nothing happens.</span></span> <span data-ttu-id="65bd3-190">Hello alsó példában csomópont1 hello tevékenység küszöbérték alatt van.</span><span class="sxs-lookup"><span data-stu-id="65bd3-190">In hello bottom example, Node1 is over hello Activity Threshold.</span></span> <span data-ttu-id="65bd3-191">Mivel mindkét hello terheléselosztás küszöbérték és hello tevékenység küszöbérték hello mérőszám túllépése, terheléselosztás van ütemezve.</span><span class="sxs-lookup"><span data-stu-id="65bd3-191">Since both hello Balancing Threshold and hello Activity Threshold for hello metric are exceeded, balancing is scheduled.</span></span> <span data-ttu-id="65bd3-192">Tegyük fel vizsgáljuk meg a következő diagram hello:</span><span class="sxs-lookup"><span data-stu-id="65bd3-192">As an example, let's look at hello following diagram:</span></span> 

<span data-ttu-id="65bd3-193"><center>
![Tevékenység küszöbérték – példa][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="65bd3-193"><center>
![Activity Threshold Example][Image3]
</center></span></span>

<span data-ttu-id="65bd3-194">Terheléselosztás küszöbértékeket, mint például tevékenység küszöbértékek meghatározott /-metrika keresztül hello fürt definíciója:</span><span class="sxs-lookup"><span data-stu-id="65bd3-194">Just like Balancing Thresholds, Activity Thresholds are defined per-metric via hello cluster definition:</span></span>

<span data-ttu-id="65bd3-195">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="65bd3-195">ClusterManifest.xml</span></span>

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

<span data-ttu-id="65bd3-196">az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:</span><span class="sxs-lookup"><span data-stu-id="65bd3-196">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "MetricActivityThresholds",
    "parameters": [
      {
          "name": "Memory",
          "value": "1536"
      }
    ]
  }
]
```

<span data-ttu-id="65bd3-197">Terheléselosztás és a tevékenység küszöbértékek, mind a feltételekhez tooa adott metrika - terheléselosztás akkor váltódik ki, csak akkor, ha mindkét hello terheléselosztás küszöbérték és tevékenység küszöbérték elérése esetén a hello azonos metrikát.</span><span class="sxs-lookup"><span data-stu-id="65bd3-197">Balancing and activity thresholds are both tied tooa specific metric - balancing is triggered only if both hello Balancing Threshold and Activity Threshold is exceeded for hello same metric.</span></span>

## <a name="balancing-services-together"></a><span data-ttu-id="65bd3-198">Terheléselosztás együtt szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="65bd3-198">Balancing services together</span></span>
<span data-ttu-id="65bd3-199">E hello fürt imbalanced-e vagy sem-e a fürtre vonatkozó döntés.</span><span class="sxs-lookup"><span data-stu-id="65bd3-199">Whether hello cluster is imbalanced or not is a cluster-wide decision.</span></span> <span data-ttu-id="65bd3-200">Áthelyezése azonban hello módja azt érhetők el az javítsa ki a szolgáltatás replikákat és a példányt körüli.</span><span class="sxs-lookup"><span data-stu-id="65bd3-200">However, hello way we go about fixing it is moving individual service replicas and instances around.</span></span> <span data-ttu-id="65bd3-201">Ez így érthető, ugye?</span><span class="sxs-lookup"><span data-stu-id="65bd3-201">This makes sense, right?</span></span> <span data-ttu-id="65bd3-202">Memória az egyik csomópont van halmozott, ha több replikák és példányok sikerült azonosítása tooit.</span><span class="sxs-lookup"><span data-stu-id="65bd3-202">If memory is stacked up on one node, multiple replicas or instances could be contributing tooit.</span></span> <span data-ttu-id="65bd3-203">Rögzítési hello egyensúlyhiány áthelyezése hello állapot-nyilvántartó replikák vagy állapotmentes példányok hello imbalanced metrikus igényel.</span><span class="sxs-lookup"><span data-stu-id="65bd3-203">Fixing hello imbalance could require moving any of hello stateful replicas or stateless instances that use hello imbalanced metric.</span></span>

<span data-ttu-id="65bd3-204">Alkalmanként, ha egy szolgáltatás, amelynek maga imbalanced nem lekérdezi áthelyezése (ne feledje hello vitafórum a helyi és globális súlyozza korábban).</span><span class="sxs-lookup"><span data-stu-id="65bd3-204">Occasionally though, a service that wasn’t itself imbalanced gets moved (remember hello discussion of local and global weights earlier).</span></span> <span data-ttu-id="65bd3-205">Miért kellene szolgáltatás első helye, amikor, hogy a szolgáltatás metrikák kiegyensúlyozott volt?</span><span class="sxs-lookup"><span data-stu-id="65bd3-205">Why would a service get moved when all that service’s metrics were balanced?</span></span> <span data-ttu-id="65bd3-206">Nézzük meg, például:</span><span class="sxs-lookup"><span data-stu-id="65bd3-206">Let’s see an example:</span></span>

- <span data-ttu-id="65bd3-207">Tegyük fel, négy szolgáltatások, Service1, Service2, Service3 és Service4 van.</span><span class="sxs-lookup"><span data-stu-id="65bd3-207">Let's say there are four services, Service1, Service2, Service3, and Service4.</span></span> 
- <span data-ttu-id="65bd3-208">A jelentés metrikák Metric1 és Metric2 service1.</span><span class="sxs-lookup"><span data-stu-id="65bd3-208">Service1 reports metrics Metric1 and Metric2.</span></span> 
- <span data-ttu-id="65bd3-209">A jelentés metrikák Metric2 és Metric3 Service2.</span><span class="sxs-lookup"><span data-stu-id="65bd3-209">Service2 reports metrics Metric2 and Metric3.</span></span> 
- <span data-ttu-id="65bd3-210">A jelentés metrikák Metric3 és Metric4 Service3.</span><span class="sxs-lookup"><span data-stu-id="65bd3-210">Service3 reports metrics Metric3 and Metric4.</span></span>
- <span data-ttu-id="65bd3-211">A jelentés metrika Metric99 Service4.</span><span class="sxs-lookup"><span data-stu-id="65bd3-211">Service4 reports metric Metric99.</span></span> 

<span data-ttu-id="65bd3-212">Minden bizonnyal láthatja, ahol az oktatóanyagban módosítjuk itt: a lánc!</span><span class="sxs-lookup"><span data-stu-id="65bd3-212">Surely you can see where we’re going here: There's a chain!</span></span> <span data-ttu-id="65bd3-213">Valóban nincsenek négy független szolgáltatások, a kapcsolódó három szolgáltatást, a másik önállóan ki van kapcsolva van.</span><span class="sxs-lookup"><span data-stu-id="65bd3-213">We don’t really have four independent services, we have three services that are related and one that is off on its own.</span></span>

<span data-ttu-id="65bd3-214"><center>
![Terheléselosztás együtt szolgáltatások][Image4]
</center></span><span class="sxs-lookup"><span data-stu-id="65bd3-214"><center>
![Balancing Services Together][Image4]
</center></span></span>

<span data-ttu-id="65bd3-215">A tanúsítványlánc miatt is lehet, hogy metrikák 1-4 egyenlőtlenség okozhat replikák és példányok tooservices tartozó 1-3 toomove körül.</span><span class="sxs-lookup"><span data-stu-id="65bd3-215">Because of this chain, it's possible that an imbalance in metrics 1-4 can cause replicas or instances belonging tooservices 1-3 toomove around.</span></span> <span data-ttu-id="65bd3-216">Is tudjuk, hogy egyenlőtlenség metrikák 1, 2 vagy 3 típusú áthelyezések nem okozhat Service4.</span><span class="sxs-lookup"><span data-stu-id="65bd3-216">We also know that an imbalance in Metrics 1, 2, or 3 can't cause movements in Service4.</span></span> <span data-ttu-id="65bd3-217">Volna az egyetlen pont óta hello replikák áthelyezése vagy azonban körüli tooService4 tartozó példány nincs feltétlenül tooimpact hello egyenlege metrikák 1 – 3.</span><span class="sxs-lookup"><span data-stu-id="65bd3-217">There would be no point since moving hello replicas or instances belonging tooService4 around can do absolutely nothing tooimpact hello balance of Metrics 1-3.</span></span>

<span data-ttu-id="65bd3-218">hello fürt erőforrás-kezelő automatikusan ki, milyen szolgáltatások kapcsolatos adatok.</span><span class="sxs-lookup"><span data-stu-id="65bd3-218">hello Cluster Resource Manager automatically figures out what services are related.</span></span> <span data-ttu-id="65bd3-219">Hozzáadása, eltávolítása vagy módosítása hello metrikák szolgáltatások hatással lehet a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="65bd3-219">Adding, removing, or changing hello metrics for services can impact their relationships.</span></span> <span data-ttu-id="65bd3-220">Például közötti terheléselosztás Service2 két kísérletei lehetett frissített tooremove Metric2.</span><span class="sxs-lookup"><span data-stu-id="65bd3-220">For example, between two runs of balancing Service2 may have been updated tooremove Metric2.</span></span> <span data-ttu-id="65bd3-221">Ez megszünteti hello lánc Service1 és Service2 között.</span><span class="sxs-lookup"><span data-stu-id="65bd3-221">This breaks hello chain between Service1 and Service2.</span></span> <span data-ttu-id="65bd3-222">Most helyett a kapcsolódó szolgáltatások két csoportok nincsenek három:</span><span class="sxs-lookup"><span data-stu-id="65bd3-222">Now instead of two groups of related services, there are three:</span></span>

<span data-ttu-id="65bd3-223"><center>
![Terheléselosztás együtt szolgáltatások][Image5]
</center></span><span class="sxs-lookup"><span data-stu-id="65bd3-223"><center>
![Balancing Services Together][Image5]
</center></span></span>

## <a name="next-steps"></a><span data-ttu-id="65bd3-224">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65bd3-224">Next steps</span></span>
* <span data-ttu-id="65bd3-225">Adatok gyűjtése le hogyan kezeli a Service Fabric fürt erőforrás-kezelő hello használat és a kapacitás hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="65bd3-225">Metrics are how hello Service Fabric Cluster Resource Manger manages consumption and capacity in hello cluster.</span></span> <span data-ttu-id="65bd3-226">További információk a metrikák toolearn, valamint hogyan tooconfigure őket, [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="65bd3-226">toolearn more about metrics and how tooconfigure them, check out [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>
* <span data-ttu-id="65bd3-227">A mozgás költsége az egyik módja az, hogy egyes szolgáltatások-e többinél drágább toomove toohello fürt erőforrás-kezelő jelzés.</span><span class="sxs-lookup"><span data-stu-id="65bd3-227">Movement Cost is one way of signaling toohello Cluster Resource Manager that certain services are more expensive toomove than others.</span></span> <span data-ttu-id="65bd3-228">A mozgás költsége kapcsolatban bővebben lásd túl[Ez a cikk](service-fabric-cluster-resource-manager-movement-cost.md)</span><span class="sxs-lookup"><span data-stu-id="65bd3-228">For more about movement cost, refer too[this article](service-fabric-cluster-resource-manager-movement-cost.md)</span></span>
* <span data-ttu-id="65bd3-229">hello fürt erőforrás-kezelő számos szabályozások konfigurálhat a forgalom le tooslow hello fürt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="65bd3-229">hello Cluster Resource Manager has several throttles that you can configure tooslow down churn in hello cluster.</span></span> <span data-ttu-id="65bd3-230">Nem fontosságúak normál esetben szükséges, de ha szüksége van rájuk megismerheti azokat [Itt](service-fabric-cluster-resource-manager-advanced-throttling.md)</span><span class="sxs-lookup"><span data-stu-id="65bd3-230">They're not normally necessary, but if you need them you can learn about them [here](service-fabric-cluster-resource-manager-advanced-throttling.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png

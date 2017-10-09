---
title: "Alkalmazáscsoportok Fabric-fürt erőforráskezelő - aaaService |} Microsoft Docs"
description: "Hello alkalmazáscsoport hello Service Fabric fürt erőforrás-kezelő funkcióinak áttekintése"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: b4f068862d962b53a0b3ea813b89bb13ee395681
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapplication-groups"></a><span data-ttu-id="cd3d2-103">Bevezetés tooApplication csoportok</span><span class="sxs-lookup"><span data-stu-id="cd3d2-103">Introduction tooApplication Groups</span></span>
<span data-ttu-id="cd3d2-104">A Service Fabric-fürt erőforrás-kezelő általában felügyel fürterőforrások hello terhelés terjednek (keresztül képviselt [metrikák](service-fabric-cluster-resource-manager-metrics.md)) egyenletesen teljes hello fürt.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-104">Service Fabric's Cluster Resource Manager typically manages cluster resources by spreading hello load (represented via [Metrics](service-fabric-cluster-resource-manager-metrics.md)) evenly throughout hello cluster.</span></span> <span data-ttu-id="cd3d2-105">Kezeli a Service Fabric hello kapacitása hello és hello fürtön hello csomópontja segítségével egész [kapacitás](service-fabric-cluster-resource-manager-cluster-description.md).</span><span class="sxs-lookup"><span data-stu-id="cd3d2-105">Service Fabric manages hello capacity of hello nodes in hello cluster and hello cluster as a whole via [capacity](service-fabric-cluster-resource-manager-cluster-description.md).</span></span> <span data-ttu-id="cd3d2-106">Metrikák és a kapacitás működik nagy számos különféle munkaterheléshez azonban néha további követelmények is betöltheti különböző háló alkalmazáspéldányok használó mintákat.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-106">Metrics and capacity work great for many workloads, but patterns that make heavy use of different Service Fabric Application Instances sometimes bring in additional requirements.</span></span> <span data-ttu-id="cd3d2-107">Például előfordulhat, hogy szeretné:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-107">For example you may want to:</span></span>

- <span data-ttu-id="cd3d2-108">Tartalékkapacitás néhány hello fürt hello csomópontján hello szolgáltatások néhány elnevezett alkalmazáspéldány belül</span><span class="sxs-lookup"><span data-stu-id="cd3d2-108">Reserve some capacity on hello nodes in hello cluster for hello services within some named application instance</span></span>
- <span data-ttu-id="cd3d2-109">Hello csomópontok hello szolgáltatások belül egy elnevezett alkalmazáspéldány rendszeren futó (nem terjednek őket hello fürtözési keresztül) teljes száma</span><span class="sxs-lookup"><span data-stu-id="cd3d2-109">Limit hello total number of nodes that hello services within a named application instance run on (instead of spreading them out over hello entire cluster)</span></span>
- <span data-ttu-id="cd3d2-110">Kapacitások példányon hello elnevezett alkalmazás maga határozza meg a toolimit hello számát, azon belül hello szolgáltatások szolgáltatások vagy a teljes erőforrás-felhasználás</span><span class="sxs-lookup"><span data-stu-id="cd3d2-110">Define capacities on hello named application instance itself toolimit hello number of services or total resource consumption of hello services inside it</span></span>

<span data-ttu-id="cd3d2-111">toomeet ezeknek a követelményeknek, a Service Fabric fürt erőforrás-kezelő hello alkalmazáscsoportok nevezett szolgáltatással támogatja.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-111">toomeet these requirements, hello Service Fabric Cluster Resource Manager supports a feature called Application Groups.</span></span>

## <a name="limiting-hello-maximum-number-of-nodes"></a><span data-ttu-id="cd3d2-112">Korlátozó hello csomópontok maximális száma</span><span class="sxs-lookup"><span data-stu-id="cd3d2-112">Limiting hello maximum number of nodes</span></span>
<span data-ttu-id="cd3d2-113">Alkalmazás kapacitás hello legegyszerűbb használati eset akkor, ha egy alkalmazáspéldányt kell toobe korlátozott tooa egyes csomópontok maximális száma.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-113">hello simplest use case for Application capacity is when an application instance needs toobe limited tooa certain maximum number of nodes.</span></span> <span data-ttu-id="cd3d2-114">Ez összesíti az adott alkalmazáspéldány alakzatot gépek meghatározott számú belül minden szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-114">This consolidates all services within that application instance onto a set number of machines.</span></span> <span data-ttu-id="cd3d2-115">Összevonás akkor hasznos, ha próbál toopredict vagy fizikai erőforrások felhasználását cap által adott elnevezett alkalmazáspéldány hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-115">Consolidation is useful when you're trying toopredict or cap physical resource use by hello services within that named application instance.</span></span> 

<span data-ttu-id="cd3d2-116">hello következő kép bemutatja egy alkalmazáspéldány rendelkező és anélküli egy meghatározott csomópontok maximális száma:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-116">hello following image shows an application instance with and without a maximum number of nodes defined:</span></span>

<span data-ttu-id="cd3d2-117"><center>
![Az alkalmazáspéldány meghatározása csomópontok maximális száma][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="cd3d2-117"><center>
![Application Instance Defining Maximum Number of Nodes][Image1]
</center></span></span>

<span data-ttu-id="cd3d2-118">Hello bal példában hello alkalmazásnál nincs definiálva csomópontok maximális számát, és nem rendelkezik, mindhárom szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-118">In hello left example, hello application doesn’t have a maximum number of nodes defined, and it has three services.</span></span> <span data-ttu-id="cd3d2-119">hello fürt erőforrás-kezelő van elosztva el az összes replika hat elérhető csomópontok tooachieve hello legjobb egyensúlyt hello fürt (hello alapértelmezés).</span><span class="sxs-lookup"><span data-stu-id="cd3d2-119">hello Cluster Resource Manager has spread out all replicas across six available nodes tooachieve hello best balance in hello cluster (hello default behavior).</span></span> <span data-ttu-id="cd3d2-120">Hello jobb példában látható hello ugyanahhoz az alkalmazáshoz, csak korlátozott toothree csomópontok.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-120">In hello right example, we see hello same application limited toothree nodes.</span></span>

<span data-ttu-id="cd3d2-121">Ez a viselkedés vezérlő hello paraméter MaximumNodes nevezik.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-121">hello parameter that controls this behavior is called MaximumNodes.</span></span> <span data-ttu-id="cd3d2-122">Ez a paraméter alkalmazás létrehozása során be, vagy egy alkalmazáspéldányt, amelyen már futott frissítve.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-122">This parameter can be set during application creation, or updated for an application instance that was already running.</span></span>

<span data-ttu-id="cd3d2-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cd3d2-123">Powershell</span></span>

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

<span data-ttu-id="cd3d2-124">C#</span><span class="sxs-lookup"><span data-stu-id="cd3d2-124">C#</span></span>

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
await fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
await fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

```

<span data-ttu-id="cd3d2-125">Hello meg csomópontok, belül hello fürt erőforrás-kezelő nem garantálja, mely szolgáltatás objektumok együtt elhelyezett vagy csomópontok használt beolvasása.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-125">Within hello set of nodes, hello Cluster Resource Manager doesn't guarantee which service objects get placed together or which nodes get used.</span></span>

## <a name="application-metrics-load-and-capacity"></a><span data-ttu-id="cd3d2-126">Alkalmazás metrikákat, a betöltés és a kapacitás</span><span class="sxs-lookup"><span data-stu-id="cd3d2-126">Application Metrics, Load, and Capacity</span></span>
<span data-ttu-id="cd3d2-127">Alkalmazáscsoportok is lehetővé teszik a megadott elnevezett alkalmazáspéldány és az adott alkalmazáspéldány kapacitás e metrikáihoz társított toodefine metrikákat.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-127">Application Groups also allow you toodefine metrics associated with a given named application instance, and that application instance's capacity for those metrics.</span></span> <span data-ttu-id="cd3d2-128">Alkalmazás metrikák lehetővé teszik tootrack tartalék és korlát hello hálózatierőforrás-fogyasztás hello szolgáltatások adott alkalmazáspéldány belül.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-128">Application metrics allow you tootrack, reserve, and limit hello resource consumption of hello services inside that application instance.</span></span>

<span data-ttu-id="cd3d2-129">Minden egyes alkalmazásmetrika vonatkoznak-e két érték beállítható:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-129">For each application metric, there are two values that can be set:</span></span>

- <span data-ttu-id="cd3d2-130">**Összes alkalmazás kapacitás** – ezt a beállítást jelöli egy adott metrika hello alkalmazás hello teljes kapacitását.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-130">**Total Application Capacity** – This setting represents hello total capacity of hello application for a particular metric.</span></span> <span data-ttu-id="cd3d2-131">hello fürt erőforrás-kezelő nem engedélyezi a új szolgáltatások, amelyek teljes terhelés tooexceed ezt az értéket az alkalmazáspéldány belül hello létrehozását.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-131">hello Cluster Resource Manager disallows hello creation of any new services within this application instance that would cause total load tooexceed this value.</span></span> <span data-ttu-id="cd3d2-132">Például tételezzük fel hello alkalmazáspéldány 10 kapacitású kellett, és már a terheléselosztási öt.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-132">For example, let's say hello application instance had a capacity of 10 and already had load of five.</span></span> <span data-ttu-id="cd3d2-133">lenne egy teljes alapértelmezett betöltési 10 hello szolgáltatás létrehozása le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-133">hello creation of a service with a total default load of 10 would be disallowed.</span></span>
- <span data-ttu-id="cd3d2-134">**Maximális csomópont-kapacitás** – ezzel a beállítással hello maximális teljes terhelés hello alkalmazás egyetlen csomópontján.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-134">**Maximum Node Capacity** – This setting specifies hello maximum total load for hello application on a single node.</span></span> <span data-ttu-id="cd3d2-135">Betöltése a kapacitás keresztül kerül, ha a fürt erőforrás-kezelő hello replikák tooother csomópontok helyez át, hogy hello terhelés csökkenése.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-135">If load goes over this capacity, hello Cluster Resource Manager moves replicas tooother nodes so that hello load decreases.</span></span>


<span data-ttu-id="cd3d2-136">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-136">Powershell:</span></span>

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

<span data-ttu-id="cd3d2-137">C#:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-137">C#:</span></span>

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;
appMetric.MaximumNodeCapacity = 100;
ad.Metrics.Add(appMetric);
await fc.ApplicationManager.CreateApplicationAsync(ad);
```

## <a name="reserving-capacity"></a><span data-ttu-id="cd3d2-138">Lefoglal kapacitást</span><span class="sxs-lookup"><span data-stu-id="cd3d2-138">Reserving Capacity</span></span>
<span data-ttu-id="cd3d2-139">Alkalmazáscsoportok egy másik gyakori felhasználási, hogy az erőforrások hello fürt tooensure egy adott alkalmazáspéldány számára vannak fenntartva.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-139">Another common use for application groups is tooensure that resources within hello cluster are reserved for a given application instance.</span></span> <span data-ttu-id="cd3d2-140">hello terület mindig van fenntartva, hello alkalmazáspéldány létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-140">hello space is always reserved when hello application instance is created.</span></span>

<span data-ttu-id="cd3d2-141">Hello fürt lemezterület lefoglalása a hello alkalmazás azonnal történik akkor is, ha:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-141">Reserving space in hello cluster for hello application happens immediately even when:</span></span>
- <span data-ttu-id="cd3d2-142">hello alkalmazáspéldány létrejött, de még nem rendelkezik hozzá tartozó szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cd3d2-142">hello application instance is created but doesn't have any services within it yet</span></span>
- <span data-ttu-id="cd3d2-143">hello száma belül hello példány alkalmazásmódosítás minden egyes szolgáltatásai</span><span class="sxs-lookup"><span data-stu-id="cd3d2-143">hello number of services within hello application instance changes every time</span></span> 
- <span data-ttu-id="cd3d2-144">hello szolgáltatások léteznek, de nem hello erőforrások felhasználása</span><span class="sxs-lookup"><span data-stu-id="cd3d2-144">hello services exist but aren't consuming hello resources</span></span> 

<span data-ttu-id="cd3d2-145">Erőforrások lefoglalása egy alkalmazáspéldányt megadása szükséges, két további paraméterek megadása: *MinimumNodes* és *NodeReservationCapacity*</span><span class="sxs-lookup"><span data-stu-id="cd3d2-145">Reserving resources for an application instance requires specifying two additional parameters: *MinimumNodes* and *NodeReservationCapacity*</span></span>

- <span data-ttu-id="cd3d2-146">**MinimumNodes** -hello minimális számát határozza meg, hogy az alkalmazás hello csomópontok példány futtasson-e.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-146">**MinimumNodes** - Defines hello minimum number of nodes that hello application instance should run on.</span></span>  
- <span data-ttu-id="cd3d2-147">**NodeReservationCapacity** – Ez a beállítás akkor / hello alkalmazás metrikát.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-147">**NodeReservationCapacity** - This setting is per metric for hello application.</span></span> <span data-ttu-id="cd3d2-148">hello értéke, hogy minden csomóponton, amennyiben ez az alkalmazás futtatásához a szolgáltatások hello hello alkalmazás számára fenntartott metrika hello mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-148">hello value is hello amount of that metric reserved for hello application on any node where that hello services in that application run.</span></span>

<span data-ttu-id="cd3d2-149">Kombinálásával **MinimumNodes** és **NodeReservationCapacity** hello alkalmazás hello fürtön belüli minimális terhelés foglalás garantálja.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-149">Combining **MinimumNodes** and **NodeReservationCapacity** guarantees a minimum load reservation for hello application within hello cluster.</span></span> <span data-ttu-id="cd3d2-150">Ha kevesebb fennmaradó hello kapacitás fürtön, mint hello teljes Foglalás szükséges, akkor hello alkalmazás létrehozása sikertelen.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-150">If there's less remaining capacity in hello cluster than hello total reservation required, creation of hello application fails.</span></span> 

<span data-ttu-id="cd3d2-151">Nézzük kapacitásfoglalás példát:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-151">Let's look at an example of capacity reservation:</span></span>

<span data-ttu-id="cd3d2-152"><center>
![Az alkalmazáspéldányok lefoglalt kapacitás meghatározása][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="cd3d2-152"><center>
![Application Instances Defining Reserved Capacity][Image2]
</center></span></span>

<span data-ttu-id="cd3d2-153">Hello bal példában alkalmazások nem rendelkeznek egyetlen meghatározott alkalmazás kapacitás.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-153">In hello left example, applications do not have any Application Capacity defined.</span></span> <span data-ttu-id="cd3d2-154">Erőforrás-kezelő fürt hello kiegyensúlyozza mindent toonormal szabályok szerint.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-154">hello Cluster Resource Manager balances everything according toonormal rules.</span></span>

<span data-ttu-id="cd3d2-155">A jobb oldali hello hello példában tegyük fel, hogy a következő beállítások hello Alkalmaz1 hozták létre:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-155">In hello example on hello right, let's say that Application1 was created with hello following settings:</span></span>

- <span data-ttu-id="cd3d2-156">MinimumNodes tootwo beállítása</span><span class="sxs-lookup"><span data-stu-id="cd3d2-156">MinimumNodes set tootwo</span></span>
- <span data-ttu-id="cd3d2-157">Egy alkalmazás definiálva mértéket</span><span class="sxs-lookup"><span data-stu-id="cd3d2-157">An application Metric defined with</span></span>
  - <span data-ttu-id="cd3d2-158">20 NodeReservationCapacity</span><span class="sxs-lookup"><span data-stu-id="cd3d2-158">NodeReservationCapacity of 20</span></span>

<span data-ttu-id="cd3d2-159">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cd3d2-159">Powershell</span></span>

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

<span data-ttu-id="cd3d2-160">C#</span><span class="sxs-lookup"><span data-stu-id="cd3d2-160">C#</span></span>

 ``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MinimumNodes = 2;

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.NodeReservationCapacity = 20;

ad.Metrics.Add(appMetric);

await fc.ApplicationManager.CreateApplicationAsync(ad);
```

<span data-ttu-id="cd3d2-161">A Service Fabric két csomópont kapacitást foglal a Alkalmaz1, és nem lehetővé teheti a Alkalmaz2 nevű almappát tooconsume szolgáltatását, hogy akkor sem, ha nem a hello szolgáltatások belül Alkalmaz1 használnak, hello időpontban.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-161">Service Fabric reserves capacity on two nodes for Application1, and doesn't allow services from Application2 tooconsume that capacity even if there are no load is being consumed by hello services inside Application1 at hello time.</span></span> <span data-ttu-id="cd3d2-162">A fenntartott alkalmazás kapacitás fel, és csökkenti a fennmaradó ezen a csomóponton, és a hello fürtön belül hello tekinthető.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-162">This reserved application capacity is considered consumed  and counts against hello remaining capacity on that node and within hello cluster.</span></span>  <span data-ttu-id="cd3d2-163">hello foglalás fürt kapacitás azonnal fennmaradó hello kell vonni, azonban hello szolgáltatás számára fenntartott fogyasztás kell vonni egy adott csomópont hello kapacitás csak akkor, ha legalább egy objektum el van helyezve az.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-163">hello reservation is deducted from hello remaining cluster capacity immediately, however hello reserved consumption is deducted from hello capacity of a specific node only when at least one service object is placed on it.</span></span> <span data-ttu-id="cd3d2-164">A későbbi foglalás lehetővé teszi, hogy a rugalmasság és erőforrás-kihasználást óta erőforrások csak szükség esetén csomópontján vannak lefoglalva.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-164">This later reservation allows for flexibility and better resource utilization since resources are only reserved on nodes when needed.</span></span>

## <a name="obtaining-hello-application-load-information"></a><span data-ttu-id="cd3d2-165">Hello alkalmazás terhelés információk beszerzése</span><span class="sxs-lookup"><span data-stu-id="cd3d2-165">Obtaining hello application load information</span></span>
<span data-ttu-id="cd3d2-166">Minden egyes alkalmazáshoz, amely rendelkezik egy alkalmazás egy vagy több metrikák meghatározott kapacitás hello összesített betöltése a hozzá tartozó szolgáltatások replikái által jelentett hello információt szerezhet be.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-166">For each application that has an Application Capacity defined for one or more metrics you can obtain hello information about hello aggregate load reported by replicas of its services.</span></span>

<span data-ttu-id="cd3d2-167">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-167">Powershell:</span></span>

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1
```

<span data-ttu-id="cd3d2-168">C#</span><span class="sxs-lookup"><span data-stu-id="cd3d2-168">C#</span></span>

``` csharp
var v = await fc.QueryManager.GetApplicationLoadInformationAsync("fabric:/MyApplication1");
var metrics = v.ApplicationLoadMetricInformation;
foreach (ApplicationLoadMetricInformation metric in metrics)
{
    Console.WriteLine(metric.ApplicationCapacity);  //total capacity for this metric in this application instance
    Console.WriteLine(metric.ReservationCapacity);  //reserved capacity for this metric in this application instance
    Console.WriteLine(metric.ApplicationLoad);  //current load for this metric in this application instance
}
```

<span data-ttu-id="cd3d2-169">hello ApplicationLoad lekérdezés hello hello az alkalmazáshoz megadott alkalmazás kapacitás alapvető adatait adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-169">hello ApplicationLoad query returns hello basic information about Application Capacity that was specified for hello application.</span></span> <span data-ttu-id="cd3d2-170">Ezen információk közé tartozik a hello minimális csomópontok és a maximális csomópontszám adatai, és hello alkalmazás van jelenleg elfoglaló hello számát.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-170">This information includes hello Minimum Nodes and Maximum Nodes info, and hello number that hello application is currently occupying.</span></span> <span data-ttu-id="cd3d2-171">Minden alkalmazás terhelési metrika kapcsolatos adatokat is tartalmaz többek között:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-171">It also includes information about each application load metric, including:</span></span>

* <span data-ttu-id="cd3d2-172">Metrika neve: Hello metrika neve.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-172">Metric Name: Name of hello metric.</span></span>
* <span data-ttu-id="cd3d2-173">Foglalási kapacitás: Az alkalmazás fenntartott hello fürt fürt kapacitást.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-173">Reservation Capacity: Cluster Capacity that is reserved in hello cluster for this Application.</span></span>
* <span data-ttu-id="cd3d2-174">Alkalmazásterhelés: Az alkalmazás gyermek replikák teljes terhelése.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-174">Application Load: Total Load of this Application’s child replicas.</span></span>
* <span data-ttu-id="cd3d2-175">Alkalmazás kapacitás: Maximálisan engedélyezett alkalmazás terhelés érték.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-175">Application Capacity: Maximum permitted value of Application Load.</span></span>

## <a name="removing-application-capacity"></a><span data-ttu-id="cd3d2-176">Alkalmazás kapacitás eltávolítása</span><span class="sxs-lookup"><span data-stu-id="cd3d2-176">Removing Application Capacity</span></span>
<span data-ttu-id="cd3d2-177">Miután hello alkalmazás kapacitás paraméterei vannak beállítva, az alkalmazás, akkor lehet eltávolítani frissítés alkalmazás API-k vagy PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-177">Once hello Application Capacity parameters are set for an application, they can be removed using Update Application APIs or PowerShell cmdlets.</span></span> <span data-ttu-id="cd3d2-178">Példa:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-178">For example:</span></span>

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

<span data-ttu-id="cd3d2-179">Ez a parancs eltávolítja minden alkalmazás kapacitás paraméterek hello alkalmazáspéldányt.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-179">This command removes all Application capacity management parameters from hello application instance.</span></span> <span data-ttu-id="cd3d2-180">Ez magában foglalja MinimumNodes, MaximumNodes és hello alkalmazás metrikákat, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-180">This includes MinimumNodes, MaximumNodes, and hello Application's metrics, if any.</span></span> <span data-ttu-id="cd3d2-181">hello parancs hello hatása az azonnali.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-181">hello effect of hello command is immediate.</span></span> <span data-ttu-id="cd3d2-182">Ez a parancs befejezése után a fürt erőforrás-kezelő hello az alkalmazások kezelésére hello alapértelmezett viselkedést alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-182">After this command completes, hello Cluster Resource Manager uses hello default behavior for managing applications.</span></span> <span data-ttu-id="cd3d2-183">Alkalmazás kapacitás paraméter adható újra keresztül `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-183">Application Capacity parameters can be specified again via `Update-ServiceFabricApplication`/`System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.</span></span>

### <a name="restrictions-on-application-capacity"></a><span data-ttu-id="cd3d2-184">Az alkalmazás kapacitás korlátozások</span><span class="sxs-lookup"><span data-stu-id="cd3d2-184">Restrictions on Application Capacity</span></span>
<span data-ttu-id="cd3d2-185">Nincsenek több korlátozások figyelembe kell venni a alkalmazás kapacitás paramétereket.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-185">There are several restrictions on Application Capacity parameters that must be respected.</span></span> <span data-ttu-id="cd3d2-186">Érvényesítési hiba esetén nem kerül sor.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-186">If there are validation errors no changes take place.</span></span>

- <span data-ttu-id="cd3d2-187">Minden egész paraméterek nem negatív számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-187">All integer parameters must be non-negative numbers.</span></span>
- <span data-ttu-id="cd3d2-188">MinimumNodes soha nem lehet nagyobb, mint a maximumnodes értéke.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-188">MinimumNodes must never be greater than MaximumNodes.</span></span>
- <span data-ttu-id="cd3d2-189">Ha a terhelési metrika kapacitások vannak meghatározva, majd kell követniük ezeket a szabályokat:</span><span class="sxs-lookup"><span data-stu-id="cd3d2-189">If capacities for a load metric are defined, then they must follow these rules:</span></span>
  - <span data-ttu-id="cd3d2-190">Csomópont foglalási kapacitás nem lehet nagyobb, mint a maximális csomópont-kapacitás.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-190">Node Reservation Capacity must not be greater than Maximum Node Capacity.</span></span> <span data-ttu-id="cd3d2-191">Például nem hello kapacitás hello mérőszám "CPU" hello csomópont tootwo egységeken korlátozza, és próbálja tooreserve három egységek minden egyes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-191">For example, you cannot limit hello capacity for hello metric “CPU” on hello node tootwo units and try tooreserve three units on each node.</span></span>
  - <span data-ttu-id="cd3d2-192">Ha maximumnodes értéke meg van adva, majd hello termék MaximumNodes és a maximális csomópont-kapacitás nem lehet nagyobb, mint a teljes alkalmazás kapacitás.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-192">If MaximumNodes is specified, then hello product of MaximumNodes and Maximum Node Capacity must not be greater than Total Application Capacity.</span></span> <span data-ttu-id="cd3d2-193">Például tételezzük fel hello maximális csomópont-kapacitás a terhelési metrika "CPU" tooeight van beállítva.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-193">For example, let's say hello Maximum Node Capacity for load metric “CPU” is set tooeight.</span></span> <span data-ttu-id="cd3d2-194">Tételezzük is fel hogy hello too10 csomópontok maximális száma.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-194">Let's also say you set hello Maximum Nodes too10.</span></span> <span data-ttu-id="cd3d2-195">Teljes alkalmazás kapacitás ebben az esetben a terhelési metrika 80-as nagyobbnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-195">In this case, Total Application Capacity must be greater than 80 for this load metric.</span></span>

<span data-ttu-id="cd3d2-196">hello korlátozások érvényesek lesznek, mind az alkalmazás létrehozása és a frissítések során.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-196">hello restrictions are enforced both during application creation and updates.</span></span>

## <a name="how-not-toouse-application-capacity"></a><span data-ttu-id="cd3d2-197">Hogyan toouse alkalmazás kapacitása</span><span class="sxs-lookup"><span data-stu-id="cd3d2-197">How not toouse Application Capacity</span></span>
- <span data-ttu-id="cd3d2-198">Ne próbáljon toouse hello alkalmazáscsoport szolgáltatások tooconstrain hello alkalmazás tooa _adott_ csomópontok részhalmaza.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-198">Do not try toouse hello Application Group features tooconstrain hello application tooa _specific_ subset of nodes.</span></span> <span data-ttu-id="cd3d2-199">Más szóval megadhatja, hogy hello alkalmazás fut, legfeljebb öt csomópont, de nem mely adott öt hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-199">In other words, you can specify that hello application runs on at most five nodes, but not which specific five nodes in hello cluster.</span></span> <span data-ttu-id="cd3d2-200">Egy alkalmazás toospecific korlátozási csomópontok elérhető szolgáltatások placement Constraints korlátozásokat alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-200">Constraining an application toospecific nodes can be achieved using placement constraints for services.</span></span>
- <span data-ttu-id="cd3d2-201">Ne próbáljon toouse hello alkalmazás kapacitás tooensure, hogy a hello kerülnek, az alkalmazás két szolgáltatását hello azonos csomópontok.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-201">Do not try toouse hello Application Capacity tooensure that two services from hello same application are placed on hello same nodes.</span></span> <span data-ttu-id="cd3d2-202">Ehelyett használja [affinitás](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) vagy [placement Constraints korlátozásokat](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).</span><span class="sxs-lookup"><span data-stu-id="cd3d2-202">Instead use [affinity](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) or [placement constraints](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd3d2-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd3d2-203">Next steps</span></span>
- <span data-ttu-id="cd3d2-204">A szolgáltatások konfigurálásáról [további információ a szolgáltatások konfigurálása](service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="cd3d2-204">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)</span></span>
- <span data-ttu-id="cd3d2-205">toofind meg arról, hogyan hello fürt erőforrás-kezelő felügyeli, és elosztja a terhelést hello fürtben, tekintse meg hello a cikk a [terheléselosztás](service-fabric-cluster-resource-manager-balancing.md)</span><span class="sxs-lookup"><span data-stu-id="cd3d2-205">toofind out about how hello Cluster Resource Manager manages and balances load in hello cluster, check out hello article on [balancing load](service-fabric-cluster-resource-manager-balancing.md)</span></span>
- <span data-ttu-id="cd3d2-206">Indítsa el a hello elejétől és [egy bevezető toohello Service Fabric fürt erőforrás-kezelő beolvasása](service-fabric-cluster-resource-manager-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="cd3d2-206">Start from hello beginning and [get an Introduction toohello Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)</span></span>
- <span data-ttu-id="cd3d2-207">További tájékoztatást a metrikák működése általában olvassa a [Service Fabric terheléselosztási metrikák](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="cd3d2-207">For more information on how metrics work generally, read up on [Service Fabric Load Metrics](service-fabric-cluster-resource-manager-metrics.md)</span></span>
- <span data-ttu-id="cd3d2-208">hello fürt erőforrás-kezelő számos lehetőséget leíró hello fürt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="cd3d2-208">hello Cluster Resource Manager has many options for describing hello cluster.</span></span> <span data-ttu-id="cd3d2-209">toofind további információk a őket, tekintse meg a cikk a [leíró a Service Fabric-fürt](service-fabric-cluster-resource-manager-cluster-description.md)</span><span class="sxs-lookup"><span data-stu-id="cd3d2-209">toofind out more about them, check out this article on [describing a Service Fabric cluster](service-fabric-cluster-resource-manager-cluster-description.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png

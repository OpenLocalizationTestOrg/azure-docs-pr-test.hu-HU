---
title: "aaaHow tooview Azure Service Fabric entitásainak összesített állapotát |} Microsoft Docs"
description: "Leírja, hogyan tooquery, megtekintheti, és Azure Service Fabric entitások összesített állapotát, állapotlekérdezések és általános lekérdezések kiértékelése."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: fa34c52d-3a74-4b90-b045-ad67afa43fe5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: add810551cac26d2b4ff81b57d94ddd780c2cc2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-service-fabric-health-reports"></a><span data-ttu-id="f2192-103">A Service Fabric rendszerállapot-jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="f2192-103">View Service Fabric health reports</span></span>
<span data-ttu-id="f2192-104">Az Azure Service Fabric vezet be a [állapotmodell](service-fabric-health-introduction.md) figyelés az állapotfigyelő entitások mely rendszerösszetevők és watchdogs a hajthatja végre helyi feltételek, amelyek jelentés.</span><span class="sxs-lookup"><span data-stu-id="f2192-104">Azure Service Fabric introduces a [health model](service-fabric-health-introduction.md) with health entities on which system components and watchdogs can report local conditions that they are monitoring.</span></span> <span data-ttu-id="f2192-105">Hello [a health Store adatbázisban](service-fabric-health-introduction.md#health-store) összes rendszerállapot-adatok toodetermine összefoglalja, hogy kifogástalan-e entitások.</span><span class="sxs-lookup"><span data-stu-id="f2192-105">hello [health store](service-fabric-health-introduction.md#health-store) aggregates all health data toodetermine whether entities are healthy.</span></span>

<span data-ttu-id="f2192-106">hello fürt automatikusan töltődik hello rendszerösszetevők által küldött állapotjelentések.</span><span class="sxs-lookup"><span data-stu-id="f2192-106">hello cluster is automatically populated with health reports sent by hello system components.</span></span> <span data-ttu-id="f2192-107">További információ: [használata rendszerállapot-jelentéseket tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).</span><span class="sxs-lookup"><span data-stu-id="f2192-107">Read more at [Use system health reports tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).</span></span>

<span data-ttu-id="f2192-108">A Service Fabric hello entitások több módon tooget hello összesített állapotát tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="f2192-108">Service Fabric provides multiple ways tooget hello aggregated health of hello entities:</span></span>

* <span data-ttu-id="f2192-109">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) vagy más képi megjelenítés eszközök</span><span class="sxs-lookup"><span data-stu-id="f2192-109">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) or other visualization tools</span></span>
* <span data-ttu-id="f2192-110">Állapotlekérdezések száma (a PowerShell, API vagy REST)</span><span class="sxs-lookup"><span data-stu-id="f2192-110">Health queries (through PowerShell, API, or REST)</span></span>
* <span data-ttu-id="f2192-111">Általános lekérdezi az állapotfigyelő egyik hello tulajdonságaként (a PowerShell, API vagy REST) rendelkező entitásokat, amely visszatérési listája</span><span class="sxs-lookup"><span data-stu-id="f2192-111">General queries that return a list of entities that have health as one of hello properties (through PowerShell, API, or REST)</span></span>

<span data-ttu-id="f2192-112">Ezeket a beállításokat, most toodemonstrate egy helyi fürt használatára öt csomópontot és hello [fabric: / WordCount alkalmazás](http://aka.ms/servicefabric-wordcountapp).</span><span class="sxs-lookup"><span data-stu-id="f2192-112">toodemonstrate these options, let's use a local cluster with five nodes and hello [fabric:/WordCount application](http://aka.ms/servicefabric-wordcountapp).</span></span> <span data-ttu-id="f2192-113">Hello **fabric: / WordCount** két alapértelmezett szolgáltatások, a következő típusú állapot-nyilvántartó szolgáltatást tartalmazó alkalmazás `WordCountServiceType`, és egy állapotmentes szolgáltatások típusú `WordCountWebServiceType`.</span><span class="sxs-lookup"><span data-stu-id="f2192-113">hello **fabric:/WordCount** application contains two default services, a stateful service of type `WordCountServiceType`, and a stateless service of type `WordCountWebServiceType`.</span></span> <span data-ttu-id="f2192-114">Hello módosítottam `ApplicationManifest.xml` toorequire hét cél replikák hello állapotalapú szolgáltatás és egy partíciót.</span><span class="sxs-lookup"><span data-stu-id="f2192-114">I changed hello `ApplicationManifest.xml` toorequire seven target replicas for hello stateful service and one partition.</span></span> <span data-ttu-id="f2192-115">Mivel hello fürt csak az öt csomóponttal, hello rendszerösszetevők jelentés figyelmeztetés hello szolgáltatás partíció mert kevesebb hello cél.</span><span class="sxs-lookup"><span data-stu-id="f2192-115">Because there are only five nodes in hello cluster, hello system components report a warning on hello service partition because it is below hello target count.</span></span>

```xml
<Service Name="WordCountService">
<<<<<<< HEAD
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="3">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
=======
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
>>>>>>> 5e84dbdd8e45a5d6b36f435a550b7433b873bf11
</Service>
```

## <a name="health-in-service-fabric-explorer"></a><span data-ttu-id="f2192-116">A Service Fabric Explorerben állapota</span><span class="sxs-lookup"><span data-stu-id="f2192-116">Health in Service Fabric Explorer</span></span>
<span data-ttu-id="f2192-117">Hello fürt vizuális áttekintést nyújt a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="f2192-117">Service Fabric Explorer provides a visual view of hello cluster.</span></span> <span data-ttu-id="f2192-118">Az alábbi hello kép láthatja, hogy:</span><span class="sxs-lookup"><span data-stu-id="f2192-118">In hello image below, you can see that:</span></span>

* <span data-ttu-id="f2192-119">alkalmazás hello **fabric: / WordCount** piros (a hiba), mert egy hibaesemény által jelentett **MyWatchdog** hello tulajdonság **rendelkezésre állási**.</span><span class="sxs-lookup"><span data-stu-id="f2192-119">hello application **fabric:/WordCount** is red (in error) because it has an error event reported by **MyWatchdog** for hello property **Availability**.</span></span>
* <span data-ttu-id="f2192-120">A szolgáltatások egyik **fabric: / WordCount/WordCountService** sárga (figyelmeztetés) a.</span><span class="sxs-lookup"><span data-stu-id="f2192-120">One of its services, **fabric:/WordCount/WordCountService** is yellow (in warning).</span></span> <span data-ttu-id="f2192-121">hét replikák hello szolgáltatás van konfigurálva, és hello fürt az öt csomóponttal rendelkezik, így a két repicas nem helyezhető el.</span><span class="sxs-lookup"><span data-stu-id="f2192-121">hello service is configured with seven replicas and hello cluster has five nodes, so two repicas can't be placed.</span></span> <span data-ttu-id="f2192-122">Bár ez nem látható itt, hello szolgáltatás partíciója sárga miatt a rendszer jelentése `System.FM` arról, hogy `Partition is below target replica or instance count`.</span><span class="sxs-lookup"><span data-stu-id="f2192-122">Although it's not shown here, hello service partition is yellow because of a system report from `System.FM` saying that `Partition is below target replica or instance count`.</span></span> <span data-ttu-id="f2192-123">hello sárga partíció eseményindítók hello sárga szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f2192-123">hello yellow partition triggers hello yellow service.</span></span>
* <span data-ttu-id="f2192-124">hello fürtre piros piros hello alkalmazás miatt.</span><span class="sxs-lookup"><span data-stu-id="f2192-124">hello cluster is red because of hello red application.</span></span>

<span data-ttu-id="f2192-125">hello értékelésének alapértelmezett házirendeket hello fürtjegyzékben és az alkalmazás jegyzékében használ.</span><span class="sxs-lookup"><span data-stu-id="f2192-125">hello evaluation uses default policies from hello cluster manifest and application manifest.</span></span> <span data-ttu-id="f2192-126">Szigorú házirendek, és nem legyenek tűrni minden hiba.</span><span class="sxs-lookup"><span data-stu-id="f2192-126">They are strict policies and do not tolerate any failure.</span></span>

<span data-ttu-id="f2192-127">A Service Fabric Explorerrel hello fürt megtekintése:</span><span class="sxs-lookup"><span data-stu-id="f2192-127">View of hello cluster with Service Fabric Explorer:</span></span>

![A Service Fabric Explorerrel hello fürt ábrázolása.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> <span data-ttu-id="f2192-129">Tudjon meg többet az [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="f2192-129">Read more about [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
>
>

## <a name="health-queries"></a><span data-ttu-id="f2192-130">Állapotlekérdezések száma</span><span class="sxs-lookup"><span data-stu-id="f2192-130">Health queries</span></span>
<span data-ttu-id="f2192-131">A Service Fabric állapotlekérdezések mutatja az egyes támogatott hello [entitástípusok](service-fabric-health-introduction.md#health-entities-and-hierarchy).</span><span class="sxs-lookup"><span data-stu-id="f2192-131">Service Fabric exposes health queries for each of hello supported [entity types](service-fabric-health-introduction.md#health-entities-and-hierarchy).</span></span> <span data-ttu-id="f2192-132">API-t, a módszerekkel hello keresztül hozzáférhetők [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell-parancsmagok és a többi.</span><span class="sxs-lookup"><span data-stu-id="f2192-132">They can be accessed through hello API, using methods on [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell cmdlets, and REST.</span></span> <span data-ttu-id="f2192-133">Ezeket a lekérdezéseket információval teljes állapotát figyelhesse hello entitás: hello összesíti az állapotokat, entitás állapotával kapcsolatos események, gyermek állapotokat (ha alkalmazható), a nem megfelelő értékelések (ha hello entitás állapota nem kifogástalan) és gyermekek állapotstatisztika (Ha érvényes).</span><span class="sxs-lookup"><span data-stu-id="f2192-133">These queries return complete health information about hello entity: hello aggregated health state, entity health events, child health states (when applicable), unhealthy evaluations (when hello entity is not healthy), and children health statistics (when applicable).</span></span>

> [!NOTE]
> <span data-ttu-id="f2192-134">Teljes mértékben a telepítéskor hello a health Store adatbázisban egy egészségügyi entitás érték érkezett vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-134">A health entity is returned when it is fully populated in hello health store.</span></span> <span data-ttu-id="f2192-135">hello entitás aktív (nem törölte a) és egy jelentést.</span><span class="sxs-lookup"><span data-stu-id="f2192-135">hello entity must be active (not deleted) and have a system report.</span></span> <span data-ttu-id="f2192-136">A szülő entitások hello hierarchia lánc rendszer jelentések is rendelkeznie kell.</span><span class="sxs-lookup"><span data-stu-id="f2192-136">Its parent entities on hello hierarchy chain must also have system reports.</span></span> <span data-ttu-id="f2192-137">Ha ezek a feltételek nem teljesülnek, a hello állapotfigyelő visszatérési lekérdezi egy [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) rendelkező [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` , amely bemutatja, hogy miért hello entitás nem adott vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-137">If any of these conditions are not satisfied, hello health queries return a [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) with [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` that shows why hello entity is not returned.</span></span>
>
>

<span data-ttu-id="f2192-138">hello állapotlekérdezések kell eltelnie a hello entitás azonosító, amely hello entitástípus függ.</span><span class="sxs-lookup"><span data-stu-id="f2192-138">hello health queries must pass in hello entity identifier, which depends on hello entity type.</span></span> <span data-ttu-id="f2192-139">hello lekérdezések választható állapotfigyelő házirend paraméterek fogadja el.</span><span class="sxs-lookup"><span data-stu-id="f2192-139">hello queries accept optional health policy parameters.</span></span> <span data-ttu-id="f2192-140">Ha nincsenek házirendek meg van adva, hello [állapotházirendeket](service-fabric-health-introduction.md#health-policies) hello fürt vagy az alkalmazás jegyzékfájlból használt értékelésre.</span><span class="sxs-lookup"><span data-stu-id="f2192-140">If no health policies are specified, hello [health policies](service-fabric-health-introduction.md#health-policies) from hello cluster or application manifest are used for evaluation.</span></span> <span data-ttu-id="f2192-141">Ha hello jegyzékfájlokat nem tartalmazzák a következő definícióját: állapotházirendeket, hello alapértelmezett állapotházirendeket értékelésre használja.</span><span class="sxs-lookup"><span data-stu-id="f2192-141">If hello manifests don't contain a definition for health policies, hello default health policies are used for evaluation.</span></span> <span data-ttu-id="f2192-142">hello alapértelmezett állapotházirendeket nem működését az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="f2192-142">hello default health policies do not tolerate any failures.</span></span> <span data-ttu-id="f2192-143">hello lekérdezéseket is fogadja el a szűrők csak részleges gyermekek visszaküldésére használatos, vagy események – hello néhányat a meglévők közül, figyelembe vegyék hello a megadott szűrők.</span><span class="sxs-lookup"><span data-stu-id="f2192-143">hello queries also accept filters for returning only partial children or events--hello ones that respect hello specified filters.</span></span> <span data-ttu-id="f2192-144">Egy másik szűrő lehetővé teszi, hogy hello gyermekek statisztika kivételével.</span><span class="sxs-lookup"><span data-stu-id="f2192-144">Another filter allows excluding hello children statistics.</span></span>

> [!NOTE]
> <span data-ttu-id="f2192-145">hello kimeneti alkalmazza a rendszer hello kiszolgáló oldalán, így csökken a hello üzenet válasz mérete.</span><span class="sxs-lookup"><span data-stu-id="f2192-145">hello output filters are applied on hello server side, so hello message reply size is reduced.</span></span> <span data-ttu-id="f2192-146">Hello kimeneti szűrők toolimit hello adatokat adott vissza, nem pedig szűrőket alkalmazhat hello ügyféloldalon használata javasolt.</span><span class="sxs-lookup"><span data-stu-id="f2192-146">We recommended that you use hello output filters toolimit hello data returned, rather than apply filters on hello client side.</span></span>
>
>

<span data-ttu-id="f2192-147">Egy entitás állapotát tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="f2192-147">An entity's health contains:</span></span>

* <span data-ttu-id="f2192-148">hello hello entitás állapotát összesíti.</span><span class="sxs-lookup"><span data-stu-id="f2192-148">hello aggregated health state of hello entity.</span></span> <span data-ttu-id="f2192-149">Hello a health Store adatbázisban entitás állapotjelentések száma, a gyermek állapotokat (ha alkalmazható) és az állapotházirendeket alapján számítja ki.</span><span class="sxs-lookup"><span data-stu-id="f2192-149">Computed by hello health store based on entity health reports, child health states (when applicable), and health policies.</span></span> <span data-ttu-id="f2192-150">Tudjon meg többet az [entitás állapotának kiértékelését](service-fabric-health-introduction.md#health-evaluation).</span><span class="sxs-lookup"><span data-stu-id="f2192-150">Read more about [entity health evaluation](service-fabric-health-introduction.md#health-evaluation).</span></span>  
* <span data-ttu-id="f2192-151">hello állapotával kapcsolatos események hello entitás.</span><span class="sxs-lookup"><span data-stu-id="f2192-151">hello health events on hello entity.</span></span>
* <span data-ttu-id="f2192-152">Minden gyermeknek hello entitások, amelyeken gyermekek állapotának hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="f2192-152">hello collection of health states of all children for hello entities that can have children.</span></span> <span data-ttu-id="f2192-153">hello állapotok entitás azonosítók tartalmazhat, és hello összesített állapotát.</span><span class="sxs-lookup"><span data-stu-id="f2192-153">hello health states contain entity identifiers and hello aggregated health state.</span></span> <span data-ttu-id="f2192-154">tooget teljes állapotát figyelhesse a gyermek hello lekérdezés állapotfigyelő hívás hello gyermek entitástípus számára pedig hello gyermek azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f2192-154">tooget complete health for a child, call hello query health for hello child entity type and pass in hello child identifier.</span></span>
* <span data-ttu-id="f2192-155">hello adott pont toohello jelenti, hogy a nem megfelelő értékelések hello entitás hello állapota elindul, ha hello entitás nem működik megfelelően.</span><span class="sxs-lookup"><span data-stu-id="f2192-155">hello unhealthy evaluations that point toohello report that triggered hello state of hello entity, if hello entity is not healthy.</span></span> <span data-ttu-id="f2192-156">hello értékelések rekurzív hello gyermekek állapotfigyelő értékelések aktuális állapot kiváltó tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="f2192-156">hello evaluations are recursive, containing hello children health evaluations that triggered current health state.</span></span> <span data-ttu-id="f2192-157">Például egy figyelő elleni replika hibát jelzett.</span><span class="sxs-lookup"><span data-stu-id="f2192-157">For example, a watchdog reported an error against a replica.</span></span> <span data-ttu-id="f2192-158">hello alkalmazás állapotának miatt tooan sérült szolgáltatás; a nem megfelelő próbaverzióként jeleníti meg. hello szolgáltatás állapota nem megfelelő miatt tooa partíció hibás; hello partíció nem kifogástalan miatt tooa replika hibás; hello a replika állapota nem megfelelő jelentés toohello figyelő hiba miatt.</span><span class="sxs-lookup"><span data-stu-id="f2192-158">hello application health shows an unhealthy evaluation due tooan unhealthy service; hello service is unhealthy due tooa partition in error; hello partition is unhealthy due tooa replica in error; hello replica is unhealthy due toohello watchdog error health report.</span></span>
* <span data-ttu-id="f2192-159">hello állapotstatisztika gyermekkel hello entitások minden gyermek típusú.</span><span class="sxs-lookup"><span data-stu-id="f2192-159">hello health statistics for all children types of hello entities that have children.</span></span> <span data-ttu-id="f2192-160">Például a fürt állapota látható hello teljes száma, az alkalmazások, szolgáltatások, partíciók, a replikákat, és entitások hello fürt telepíteni.</span><span class="sxs-lookup"><span data-stu-id="f2192-160">For example, cluster health shows hello total number of applications, services, partitions, replicas, and deployed entities in hello cluster.</span></span> <span data-ttu-id="f2192-161">Szolgáltatás állapotát jeleníti meg a partíciók száma hello és replikák hello alatt megadott szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f2192-161">Service health shows hello total number of partitions and replicas under hello specified service.</span></span>

## <a name="get-cluster-health"></a><span data-ttu-id="f2192-162">Fürt állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="f2192-162">Get cluster health</span></span>
<span data-ttu-id="f2192-163">Értéket ad vissza hello hello fürt entitás állapotát, és tartalmazza az alkalmazások és a csomópontok (gyermekek hello fürt) hello állapotokat.</span><span class="sxs-lookup"><span data-stu-id="f2192-163">Returns hello health of hello cluster entity and contains hello health states of applications and nodes (children of hello cluster).</span></span> <span data-ttu-id="f2192-164">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="f2192-164">Input:</span></span>

* <span data-ttu-id="f2192-165">[Választható] hello fürt állapotházirend használt tooevaluate hello csomópontokat és hello fürthöz kapcsolódó események.</span><span class="sxs-lookup"><span data-stu-id="f2192-165">[Optional] hello cluster health policy used tooevaluate hello nodes and hello cluster events.</span></span>
* <span data-ttu-id="f2192-166">[Választható] hello alkalmazás állapotának házirend-hozzárendelés, az hello állapotházirendeket toooverride hello alkalmazás-jegyzékfájl házirendek használható.</span><span class="sxs-lookup"><span data-stu-id="f2192-166">[Optional] hello application health policy map, with hello health policies used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="f2192-167">[Választható] Események, a csomópontok és az alkalmazásokat, amelyek adja meg, mely tételek szűrők érdeklő, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák).</span><span class="sxs-lookup"><span data-stu-id="f2192-167">[Optional] Filters for events, nodes, and applications that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="f2192-168">Összes esemény, csomópontokat és alkalmazások úgy legyenek használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="f2192-168">All events, nodes, and applications are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="f2192-169">[Választható] Szűrés tooexclude állapotstatisztika.</span><span class="sxs-lookup"><span data-stu-id="f2192-169">[Optional] Filter tooexclude health statistics.</span></span>
* <span data-ttu-id="f2192-170">[Választható] Szűrés tooinclude fabric: / rendszer állapotfigyelő statisztikákat hello állapotstatisztika.</span><span class="sxs-lookup"><span data-stu-id="f2192-170">[Optional] Filter tooinclude fabric:/System health statistics in hello health statistics.</span></span> <span data-ttu-id="f2192-171">Csak érvényes hello állapotstatisztika nem dimenziónevek kizárásakor.</span><span class="sxs-lookup"><span data-stu-id="f2192-171">Only applicable when hello health statistics are not excluded.</span></span> <span data-ttu-id="f2192-172">Alapértelmezés szerint a hello egészségügyi statisztikák csak felhasználói alkalmazások és hello rendszeralkalmazás statisztika tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f2192-172">By default, hello health statistics include only statistics for user applications and not hello System application.</span></span>

### <a name="api"></a><span data-ttu-id="f2192-173">API</span><span class="sxs-lookup"><span data-stu-id="f2192-173">API</span></span>
<span data-ttu-id="f2192-174">tooget fürt állapotát, és hozzon létre egy `FabricClient` és hívás hello [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) metódust a **HealthManager**.</span><span class="sxs-lookup"><span data-stu-id="f2192-174">tooget cluster health, create a `FabricClient` and call hello [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) method on its **HealthManager**.</span></span>

<span data-ttu-id="f2192-175">hello következő hívás lekérdezi hello fürt állapotát:</span><span class="sxs-lookup"><span data-stu-id="f2192-175">hello following call gets hello cluster health:</span></span>

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

<span data-ttu-id="f2192-176">hello alábbira hello fürt állapotfigyelő lekérdezi az egyéni fürterőforrás állapotát házirend használatával és szűri a csomópontok és alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="f2192-176">hello following code gets hello cluster health by using a custom cluster health policy and filters for nodes and applications.</span></span> <span data-ttu-id="f2192-177">Meghatározza, hogy a hello állapotstatisztika hello háló tartalmaz: / System statisztika.</span><span class="sxs-lookup"><span data-stu-id="f2192-177">It specifies that hello health statistics include hello fabric:/System statistics.</span></span> <span data-ttu-id="f2192-178">Létrehozza [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), hello bemeneti adatokat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="f2192-178">It creates [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), which contains hello input information.</span></span>

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var healthStatisticsFilter = new ClusterHealthStatisticsFilter()
{
    ExcludeHealthStatistics = false,
    IncludeSystemApplicationHealthStatistics = true
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
    HealthStatisticsFilter = healthStatisticsFilter
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="f2192-179">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2192-179">PowerShell</span></span>
<span data-ttu-id="f2192-180">hello parancsmag tooget hello fürt állapota [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth).</span><span class="sxs-lookup"><span data-stu-id="f2192-180">hello cmdlet tooget hello cluster health is [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth).</span></span> <span data-ttu-id="f2192-181">Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f2192-181">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="f2192-182">hello hello fürt állapota az öt csomóponttal, hello rendszeralkalmazás és fabric: / WordCount leírtak szerint konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f2192-182">hello state of hello cluster is five nodes, hello system application, and fabric:/WordCount configured as described.</span></span>

<span data-ttu-id="f2192-183">a következő parancsmag hello lekérdezi a fürt állapotának alapértelmezett házirendek használatával.</span><span class="sxs-lookup"><span data-stu-id="f2192-183">hello following cmdlet gets cluster health by using default health policies.</span></span> <span data-ttu-id="f2192-184">hello összesített állapota figyelmet igényel, mert hello fabric: / WordCount alkalmazás figyelmeztetés is van.</span><span class="sxs-lookup"><span data-stu-id="f2192-184">hello aggregated health state is warning, because hello fabric:/WordCount application is in warning.</span></span> <span data-ttu-id="f2192-185">Vegye figyelembe, hogyan hello sérült értékelések hello feltételek hello összesített állapotát kiváltó részletekkel szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="f2192-185">Note how hello unhealthy evaluations provide details on hello conditions that triggered hello aggregated health.</span></span>

```xml
PS D:\ServiceFabric> Get-ServiceFabricClusterHealth


AggregatedHealthState   : Warning
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                          
                          
NodeHealthStates        : 
                          NodeName              : _Node_4
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_0
                          AggregatedHealthState : Ok
                          
ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok
                          
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning
                          
HealthEvents            : None
HealthStatistics        : 
                          Node                  : 5 Ok, 0 Warning, 0 Error
                          Replica               : 6 Ok, 0 Warning, 0 Error
                          Partition             : 1 Ok, 1 Warning, 0 Error
                          Service               : 1 Ok, 1 Warning, 0 Error
                          DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                          DeployedApplication   : 5 Ok, 0 Warning, 0 Error
                          Application           : 0 Ok, 1 Warning, 0 Error
```

<span data-ttu-id="f2192-186">hello következő PowerShell-parancsmag lekéri hello fürt hello állapotát egy egyéni alkalmazás-házirend használatával.</span><span class="sxs-lookup"><span data-stu-id="f2192-186">hello following PowerShell cmdlet gets hello health of hello cluster by using a custom application policy.</span></span> <span data-ttu-id="f2192-187">Eredmények tooget alkalmazások és a hiba vagy figyelmeztetés csomópontjára szűri.</span><span class="sxs-lookup"><span data-stu-id="f2192-187">It filters results tooget only applications and nodes in error or warning.</span></span> <span data-ttu-id="f2192-188">Ennek eredményeképpen a csomópontok nem ad vissza, mivel ezek az összes kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="f2192-188">As a result, no nodes are returned, as they are all healthy.</span></span> <span data-ttu-id="f2192-189">Csak hello fabric: / WordCount alkalmazás tiszteletben tartja hello alkalmazások szűrő.</span><span class="sxs-lookup"><span data-stu-id="f2192-189">Only hello fabric:/WordCount application respects hello applications filter.</span></span> <span data-ttu-id="f2192-190">Mivel a hello egyéni házirend hello a háló hibaként tooconsider figyelmeztetések határozza meg: / WordCount alkalmazás hello alkalmazás hasonlóan hiba történik, és így hello fürt.</span><span class="sxs-lookup"><span data-stu-id="f2192-190">Because hello custom policy specifies tooconsider warnings as errors for hello fabric:/WordCount application, hello application is evaluated as in error, and so is hello cluster.</span></span>

```powershell
PS D:\ServiceFabric> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics


AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                          
                          
NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error
                          
HealthEvents            : None
```

### <a name="rest"></a><span data-ttu-id="f2192-191">REST</span><span class="sxs-lookup"><span data-stu-id="f2192-191">REST</span></span>
<span data-ttu-id="f2192-192">Kaphat a fürt health használata az egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.</span><span class="sxs-lookup"><span data-stu-id="f2192-192">You can get cluster health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-node-health"></a><span data-ttu-id="f2192-193">Csomópont állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="f2192-193">Get node health</span></span>
<span data-ttu-id="f2192-194">Értéket ad vissza hello egy csomópont entitás állapotát, és hello állapotával kapcsolatos események hello csomóponton jelentett tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f2192-194">Returns hello health of a node entity and contains hello health events reported on hello node.</span></span> <span data-ttu-id="f2192-195">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="f2192-195">Input:</span></span>

* <span data-ttu-id="f2192-196">[Szükséges] hello csomópont nevét, amely azonosítja a hello csomópont.</span><span class="sxs-lookup"><span data-stu-id="f2192-196">[Required] hello node name that identifies hello node.</span></span>
* <span data-ttu-id="f2192-197">[Választható] hello fürt állapotfigyelő házirend-beállítások használt tooevaluate állapotát.</span><span class="sxs-lookup"><span data-stu-id="f2192-197">[Optional] hello cluster health policy settings used tooevaluate health.</span></span>
* <span data-ttu-id="f2192-198">[Választható] Adja meg, mely tételek események szűrők iránt, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák).</span><span class="sxs-lookup"><span data-stu-id="f2192-198">[Optional] Filters for events that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="f2192-199">Összes esemény használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="f2192-199">All events are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>

### <a name="api"></a><span data-ttu-id="f2192-200">API</span><span class="sxs-lookup"><span data-stu-id="f2192-200">API</span></span>
<span data-ttu-id="f2192-201">tooget csomópont állapotát keresztül hello API, hozzon létre egy `FabricClient` és hívás hello [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) a HealthManager metódust.</span><span class="sxs-lookup"><span data-stu-id="f2192-201">tooget node health through hello API, create a `FabricClient` and call hello [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) method on its HealthManager.</span></span>

<span data-ttu-id="f2192-202">hello következő kód jogosultságot kap hello megadott csomópontnév hello csomópont állapotát:</span><span class="sxs-lookup"><span data-stu-id="f2192-202">hello following code gets hello node health for hello specified node name:</span></span>

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

<span data-ttu-id="f2192-203">hello következő kód jogosultságot kap hello csomópont állapotát az hello adott csomópont neve és fázisok eseményszűrő és egyéni házirend használatával [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):</span><span class="sxs-lookup"><span data-stu-id="f2192-203">hello following code gets hello node health for hello specified node name and passes in events filter and custom policy through [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):</span></span>

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="f2192-204">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2192-204">PowerShell</span></span>
<span data-ttu-id="f2192-205">hello parancsmag tooget hello csomópont állapota [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth).</span><span class="sxs-lookup"><span data-stu-id="f2192-205">hello cmdlet tooget hello node health is [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth).</span></span> <span data-ttu-id="f2192-206">Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f2192-206">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>
<span data-ttu-id="f2192-207">a következő parancsmag hello alapértelmezett házirendek használatával lekérdezi a hello csomópont állapotát:</span><span class="sxs-lookup"><span data-stu-id="f2192-207">hello following cmdlet gets hello node health by using default health policies:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 7/13/2017 4:39:23 PM
                        ReceivedAt            : 7/13/2017 4:40:47 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 4:40:47 PM, LastWarning = 1/1/0001 12:00:00 AM
```

<span data-ttu-id="f2192-208">hello következő parancsmag lekéri az összes csomópont állapotát hello hello fürt:</span><span class="sxs-lookup"><span data-stu-id="f2192-208">hello following cmdlet gets hello health of all nodes in hello cluster:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_4                     Ok
_Node_3                     Ok
_Node_2                     Ok
_Node_1                     Ok
_Node_0                     Ok
```

### <a name="rest"></a><span data-ttu-id="f2192-209">REST</span><span class="sxs-lookup"><span data-stu-id="f2192-209">REST</span></span>
<span data-ttu-id="f2192-210">Kaphat a csomópont állapotát egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.</span><span class="sxs-lookup"><span data-stu-id="f2192-210">You can get node health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-application-health"></a><span data-ttu-id="f2192-211">Alkalmazás állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="f2192-211">Get application health</span></span>
<span data-ttu-id="f2192-212">Az alkalmazás entitás értéket ad vissza a hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="f2192-212">Returns hello health of an application entity.</span></span> <span data-ttu-id="f2192-213">Hello állapotának hello telepített alkalmazás és szolgáltatás gyermekeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f2192-213">It contains hello health states of hello deployed application and service children.</span></span> <span data-ttu-id="f2192-214">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="f2192-214">Input:</span></span>

* <span data-ttu-id="f2192-215">[Szükséges] hello alkalmazásnév (URI), amely azonosítja a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f2192-215">[Required] hello application name (URI) that identifies hello application.</span></span>
* <span data-ttu-id="f2192-216">[Választható] hello az alkalmazás állapotházirendje toooverride hello application manifest házirendeket használt.</span><span class="sxs-lookup"><span data-stu-id="f2192-216">[Optional] hello application health policy used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="f2192-217">[Választható] Események, szolgáltatások, és adja meg, mely tételek üzembe helyezett alkalmazások szűrők érdeklő, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák).</span><span class="sxs-lookup"><span data-stu-id="f2192-217">[Optional] Filters for events, services, and deployed applications that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="f2192-218">Minden események, a szolgáltatások és a központilag telepített alkalmazások is használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="f2192-218">All events, services, and deployed applications are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="f2192-219">[Választható] Szűrés tooexclude hello állapotstatisztika.</span><span class="sxs-lookup"><span data-stu-id="f2192-219">[Optional] Filter tooexclude hello health statistics.</span></span> <span data-ttu-id="f2192-220">Ha nincs megadva, hello állapotstatisztika közé tartozik a hello ok, figyelmeztetés és hibák száma az összes alkalmazás számára: szolgáltatások, partíciók, replikák alkalmazások telepítését, és központilag telepített szervizcsomagok.</span><span class="sxs-lookup"><span data-stu-id="f2192-220">If not specified, hello health statistics include hello ok, warning, and error count for all application children: services, partitions, replicas, deployed applications, and deployed service packages.</span></span>

### <a name="api"></a><span data-ttu-id="f2192-221">API</span><span class="sxs-lookup"><span data-stu-id="f2192-221">API</span></span>
<span data-ttu-id="f2192-222">tooget alkalmazás állapotának, hozzon létre egy `FabricClient` és hívás hello [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) a HealthManager metódust.</span><span class="sxs-lookup"><span data-stu-id="f2192-222">tooget application health, create a `FabricClient` and call hello [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) method on its HealthManager.</span></span>

<span data-ttu-id="f2192-223">hello következő kód jogosultságot kap hello alkalmazás állapotának hello megadott neve (URI):</span><span class="sxs-lookup"><span data-stu-id="f2192-223">hello following code gets hello application health for hello specified application name (URI):</span></span>

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

<span data-ttu-id="f2192-224">hello következő kód jogosultságot kap hello alkalmazás állapotának hello megadott neve (URI), szűrők és a megadott egyéni házirendek [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="f2192-224">hello following code gets hello application health for hello specified application name (URI), with filters and custom policies specified via [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).</span></span>

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="f2192-225">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2192-225">PowerShell</span></span>
<span data-ttu-id="f2192-226">hello parancsmag tooget hello alkalmazás üzemállapota [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="f2192-226">hello cmdlet tooget hello application health is [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps).</span></span> <span data-ttu-id="f2192-227">Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f2192-227">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="f2192-228">hello következő parancsmag visszaadja hello hello állapotának **fabric: / WordCount** alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="f2192-228">hello following cmdlet returns hello health of hello **fabric:/WordCount** application:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok
                                  
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning
                                  
DeployedApplicationHealthStates : 
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok
                                  
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/13/2017 5:57:05 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
                                  
HealthStatistics                : 
                                  Replica               : 6 Ok, 0 Warning, 0 Error
                                  Partition             : 1 Ok, 1 Warning, 0 Error
                                  Service               : 1 Ok, 1 Warning, 0 Error
                                  DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                                  DeployedApplication   : 5 Ok, 0 Warning, 0 Error
```

<span data-ttu-id="f2192-229">a következő PowerShell-parancsmag cserél az egyéni házirendek hello.</span><span class="sxs-lookup"><span data-stu-id="f2192-229">hello following PowerShell cmdlet passes in custom policies.</span></span> <span data-ttu-id="f2192-230">Szűrők is gyermekek és események.</span><span class="sxs-lookup"><span data-stu-id="f2192-230">It also filters children and events.</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error
                                  
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a><span data-ttu-id="f2192-231">REST</span><span class="sxs-lookup"><span data-stu-id="f2192-231">REST</span></span>
<span data-ttu-id="f2192-232">Beszerezheti az alkalmazás állapotának egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.</span><span class="sxs-lookup"><span data-stu-id="f2192-232">You can get application health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-service-health"></a><span data-ttu-id="f2192-233">Szolgáltatás állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="f2192-233">Get service health</span></span>
<span data-ttu-id="f2192-234">Egy szolgáltatás entitás hello állapotát értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-234">Returns hello health of a service entity.</span></span> <span data-ttu-id="f2192-235">Hello partíció állapotokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f2192-235">It contains hello partition health states.</span></span> <span data-ttu-id="f2192-236">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="f2192-236">Input:</span></span>

* <span data-ttu-id="f2192-237">[Szükséges] hello szolgáltatás neve (URI), amely azonosítja a hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f2192-237">[Required] hello service name (URI) that identifies hello service.</span></span>
* <span data-ttu-id="f2192-238">[Választható] hello az alkalmazás állapotházirendje használt toooverride hello alkalmazás-jegyzékfájl házirend.</span><span class="sxs-lookup"><span data-stu-id="f2192-238">[Optional] hello application health policy used toooverride hello application manifest policy.</span></span>
* <span data-ttu-id="f2192-239">[Választható] Az események és adja meg, mely tételek partícióknak szűrők érdeklő, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák).</span><span class="sxs-lookup"><span data-stu-id="f2192-239">[Optional] Filters for events and partitions that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="f2192-240">Események és a partíciók olyan használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="f2192-240">All events and partitions are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="f2192-241">[Választható] Szűrés tooexclude állapotstatisztika.</span><span class="sxs-lookup"><span data-stu-id="f2192-241">[Optional] Filter tooexclude health statistics.</span></span> <span data-ttu-id="f2192-242">Ha nincs megadva, egészségügyi statisztika megjelenítése hello hello ok, figyelmeztetés és hiba száma az összes partíció és replikák hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f2192-242">If not specified, hello health statistics show hello ok, warning, and error count for all partitions and replicas of hello service.</span></span>

### <a name="api"></a><span data-ttu-id="f2192-243">API</span><span class="sxs-lookup"><span data-stu-id="f2192-243">API</span></span>
<span data-ttu-id="f2192-244">tooget szolgáltatásának állapota keresztül hello API, hozzon létre egy `FabricClient` és hívás hello [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) a HealthManager metódust.</span><span class="sxs-lookup"><span data-stu-id="f2192-244">tooget service health through hello API, create a `FabricClient` and call hello [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) method on its HealthManager.</span></span>

<span data-ttu-id="f2192-245">hello alábbi példa lekérdezi hello Állapotfigyelő szolgáltatás a megadott szolgáltatásnév (URI):</span><span class="sxs-lookup"><span data-stu-id="f2192-245">hello following example gets hello health of a service with specified service name (URI):</span></span>

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

<span data-ttu-id="f2192-246">hello következő kód jogosultságot kap hello szolgáltatás állapotát a hello megadott szolgáltatásnév (URI), szűrők és egyéb egyéni házirend használatával [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):</span><span class="sxs-lookup"><span data-stu-id="f2192-246">hello following code gets hello service health for hello specified service name (URI), specifying filters and custom policy via [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):</span></span>

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="f2192-247">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2192-247">PowerShell</span></span>
<span data-ttu-id="f2192-248">hello parancsmag tooget hello szolgáltatás állapota [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth).</span><span class="sxs-lookup"><span data-stu-id="f2192-248">hello cmdlet tooget hello service health is [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth).</span></span> <span data-ttu-id="f2192-249">Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f2192-249">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="f2192-250">a következő parancsmag hello alapértelmezett házirendek használatával lekérdezi a hello szolgáltatás állapota:</span><span class="sxs-lookup"><span data-stu-id="f2192-250">hello following cmdlet gets hello service health by using default health policies:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                        
                        Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                        
                            Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
PartitionHealthStates : 
                        PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                        AggregatedHealthState : Warning
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 15
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
                        Partition             : 0 Ok, 1 Warning, 0 Error
```

### <a name="rest"></a><span data-ttu-id="f2192-251">REST</span><span class="sxs-lookup"><span data-stu-id="f2192-251">REST</span></span>
<span data-ttu-id="f2192-252">Kaphat a szolgáltatás állapotát a egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.</span><span class="sxs-lookup"><span data-stu-id="f2192-252">You can get service health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-partition-health"></a><span data-ttu-id="f2192-253">Partíció állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="f2192-253">Get partition health</span></span>
<span data-ttu-id="f2192-254">Egy partíció entitás hello állapotát értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-254">Returns hello health of a partition entity.</span></span> <span data-ttu-id="f2192-255">Hello replika állapotokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f2192-255">It contains hello replica health states.</span></span> <span data-ttu-id="f2192-256">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="f2192-256">Input:</span></span>

* <span data-ttu-id="f2192-257">[Szükséges] hello partíció Azonosítót (GUID), amely azonosítja a hello partíció.</span><span class="sxs-lookup"><span data-stu-id="f2192-257">[Required] hello partition ID (GUID) that identifies hello partition.</span></span>
* <span data-ttu-id="f2192-258">[Választható] hello az alkalmazás állapotházirendje használt toooverride hello alkalmazás-jegyzékfájl házirend.</span><span class="sxs-lookup"><span data-stu-id="f2192-258">[Optional] hello application health policy used toooverride hello application manifest policy.</span></span>
* <span data-ttu-id="f2192-259">[Választható] Az események és adja meg, mely tételek replikák szűrők érdeklő, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák).</span><span class="sxs-lookup"><span data-stu-id="f2192-259">[Optional] Filters for events and replicas that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="f2192-260">Események és a replikák használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="f2192-260">All events and replicas are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="f2192-261">[Választható] Szűrés tooexclude állapotstatisztika.</span><span class="sxs-lookup"><span data-stu-id="f2192-261">[Optional] Filter tooexclude health statistics.</span></span> <span data-ttu-id="f2192-262">Ha nincs megadva, hello állapotstatisztika megjelenítése a rendszer hány replikák ok, figyelmeztetés és hiba állapotok.</span><span class="sxs-lookup"><span data-stu-id="f2192-262">If not specified, hello health statistics show how many replicas are in ok, warning, and error states.</span></span>

### <a name="api"></a><span data-ttu-id="f2192-263">API</span><span class="sxs-lookup"><span data-stu-id="f2192-263">API</span></span>
<span data-ttu-id="f2192-264">tooget partíció állapotfigyelő keresztül hello API, hozzon létre egy `FabricClient` és hívás hello [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) a HealthManager metódust.</span><span class="sxs-lookup"><span data-stu-id="f2192-264">tooget partition health through hello API, create a `FabricClient` and call hello [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) method on its HealthManager.</span></span> <span data-ttu-id="f2192-265">Választható paraméterek: toospecify, hozzon létre [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="f2192-265">toospecify optional parameters, create [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).</span></span>

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a><span data-ttu-id="f2192-266">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2192-266">PowerShell</span></span>
<span data-ttu-id="f2192-267">hello parancsmag tooget hello partíció állapota [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth).</span><span class="sxs-lookup"><span data-stu-id="f2192-267">hello cmdlet tooget hello partition health is [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth).</span></span> <span data-ttu-id="f2192-268">Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f2192-268">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="f2192-269">hello következő parancsmag lekéri hello állapotát minden partíciója hello **fabric: / WordCount/WordCountService** szolgáltatás és a szűrők replika állapotokat ki:</span><span class="sxs-lookup"><span data-stu-id="f2192-269">hello following cmdlet gets hello health for all partitions of hello **fabric:/WordCount/WordCountService** service and filters out replica health states:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 72
                        SentAt                : 7/13/2017 5:57:29 PM
                        ReceivedAt            : 7/13/2017 5:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/P RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/S RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 7/13/2017 5:57:48 PM, LastError = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131444445174851664
                        SentAt                : 7/13/2017 6:35:17 PM
                        ReceivedAt            : 7/13/2017 6:35:18 PM
                        TTL                   : 00:01:05
                        Description           : hello Load Balancer was unable toofind a placement for one or more of hello Service's Replicas:
                        Secondary replica could not be placed due toohello following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/13/2017 5:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a><span data-ttu-id="f2192-270">REST</span><span class="sxs-lookup"><span data-stu-id="f2192-270">REST</span></span>
<span data-ttu-id="f2192-271">Kaphat a partíció health használata az egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.</span><span class="sxs-lookup"><span data-stu-id="f2192-271">You can get partition health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-replica-health"></a><span data-ttu-id="f2192-272">A replika állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="f2192-272">Get replica health</span></span>
<span data-ttu-id="f2192-273">Állapotalapú szolgáltatási replika vagy állapotmentes szolgáltatáspéldány hello állapotának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f2192-273">Returns hello health of a stateful service replica or a stateless service instance.</span></span> <span data-ttu-id="f2192-274">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="f2192-274">Input:</span></span>

* <span data-ttu-id="f2192-275">[Szükséges] hello Azonosítót (GUID) és a replika Partícióazonosító hello replika azonosító.</span><span class="sxs-lookup"><span data-stu-id="f2192-275">[Required] hello partition ID (GUID) and replica ID that identifies hello replica.</span></span>
* <span data-ttu-id="f2192-276">[Választható] hello alkalmazás állapotának házirend paramétereket toooverride hello alkalmazás-jegyzékfájl házirendek használni.</span><span class="sxs-lookup"><span data-stu-id="f2192-276">[Optional] hello application health policy parameters used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="f2192-277">[Választható] Adja meg, mely tételek események szűrők iránt, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák).</span><span class="sxs-lookup"><span data-stu-id="f2192-277">[Optional] Filters for events that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="f2192-278">Összes esemény használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="f2192-278">All events are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>

### <a name="api"></a><span data-ttu-id="f2192-279">API</span><span class="sxs-lookup"><span data-stu-id="f2192-279">API</span></span>
<span data-ttu-id="f2192-280">tooget hello replika állapotfigyelő keresztül hello API, hozzon létre egy `FabricClient` és hívás hello [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) a HealthManager metódust.</span><span class="sxs-lookup"><span data-stu-id="f2192-280">tooget hello replica health through hello API, create a `FabricClient` and call hello [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) method on its HealthManager.</span></span> <span data-ttu-id="f2192-281">speciális paramétereket használja toospecify [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="f2192-281">toospecify advanced parameters, use [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).</span></span>

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a><span data-ttu-id="f2192-282">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2192-282">PowerShell</span></span>
<span data-ttu-id="f2192-283">hello parancsmag tooget hello replika állapota [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth).</span><span class="sxs-lookup"><span data-stu-id="f2192-283">hello cmdlet tooget hello replica health is [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth).</span></span> <span data-ttu-id="f2192-284">Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f2192-284">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="f2192-285">hello következő parancsmag lekéri hello elsődleges replika hello szolgáltatás összes partíciók hello állapotát:</span><span class="sxs-lookup"><span data-stu-id="f2192-285">hello following cmdlet gets hello health of hello primary replica for all partitions of hello service:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a><span data-ttu-id="f2192-286">REST</span><span class="sxs-lookup"><span data-stu-id="f2192-286">REST</span></span>
<span data-ttu-id="f2192-287">Kaphat a replika health használata az egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.</span><span class="sxs-lookup"><span data-stu-id="f2192-287">You can get replica health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-deployed-application-health"></a><span data-ttu-id="f2192-288">Telepített alkalmazás állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="f2192-288">Get deployed application health</span></span>
<span data-ttu-id="f2192-289">Egy csomópont entitás központi telepítésű alkalmazás hello állapotát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-289">Returns hello health of an application deployed on a node entity.</span></span> <span data-ttu-id="f2192-290">Telepített hello szolgáltatás csomag állapotokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f2192-290">It contains hello deployed service package health states.</span></span> <span data-ttu-id="f2192-291">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="f2192-291">Input:</span></span>

* <span data-ttu-id="f2192-292">[Szükséges] hello alkalmazásnév (URI) és a csomópont neve (karakterlánc) azonosító hello telepített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f2192-292">[Required] hello application name (URI) and node name (string) that identify hello deployed application.</span></span>
* <span data-ttu-id="f2192-293">[Választható] hello az alkalmazás állapotházirendje toooverride hello application manifest házirendeket használt.</span><span class="sxs-lookup"><span data-stu-id="f2192-293">[Optional] hello application health policy used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="f2192-294">[Választható] Az események és adja meg, mely tételek telepített szervizcsomagok szűrők érdeklő, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák).</span><span class="sxs-lookup"><span data-stu-id="f2192-294">[Optional] Filters for events and deployed service packages that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="f2192-295">Események és a telepített szervizcsomagok használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="f2192-295">All events and deployed service packages are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="f2192-296">[Választható] Szűrés tooexclude állapotstatisztika.</span><span class="sxs-lookup"><span data-stu-id="f2192-296">[Optional] Filter tooexclude health statistics.</span></span> <span data-ttu-id="f2192-297">Ha nincs megadva, hello állapotstatisztika telepített szervizcsomagok hello száma ok, figyelmeztetés és hiba állapotának megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="f2192-297">If not specified, hello health statistics show hello number of deployed service packages in ok, warning, and error health states.</span></span>

### <a name="api"></a><span data-ttu-id="f2192-298">API</span><span class="sxs-lookup"><span data-stu-id="f2192-298">API</span></span>
<span data-ttu-id="f2192-299">egy csomóponton keresztül hello API-t központi telepítésű alkalmazás állapotának tooget hello hozzon létre egy `FabricClient` és hívás hello [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) a HealthManager metódust.</span><span class="sxs-lookup"><span data-stu-id="f2192-299">tooget hello health of an application deployed on a node through hello API, create a `FabricClient` and call hello [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) method on its HealthManager.</span></span> <span data-ttu-id="f2192-300">Választható paraméterek: toospecify, használjon [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="f2192-300">toospecify optional parameters, use [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).</span></span>

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a><span data-ttu-id="f2192-301">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2192-301">PowerShell</span></span>
<span data-ttu-id="f2192-302">hello parancsmag tooget hello telepített alkalmazás állapotának van [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="f2192-302">hello cmdlet tooget hello deployed application health is [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps).</span></span> <span data-ttu-id="f2192-303">Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f2192-303">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="f2192-304">futtassa, ha egy alkalmazás központi telepítése, toofind [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) és hello pillantást telepített alkalmazás gyermekei.</span><span class="sxs-lookup"><span data-stu-id="f2192-304">toofind out where an application is deployed, run [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) and look at hello deployed application children.</span></span>

<span data-ttu-id="f2192-305">hello következő parancsmag lekéri hello hello állapotának **fabric: / WordCount** a központilag telepített alkalmazás **_Node_2**.</span><span class="sxs-lookup"><span data-stu-id="f2192-305">hello following cmdlet gets hello health of hello **fabric:/WordCount** application deployed on **_Node_2**.</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_0


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_0
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
                                     ServiceManifestName   : WordCountWebServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131444422261848308
                                     SentAt                : 7/13/2017 5:57:06 PM
                                     ReceivedAt            : 7/13/2017 5:57:17 PM
                                     TTL                   : Infinite
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/13/2017 5:57:17 PM, LastWarning = 1/1/0001 12:00:00 AM
                                     
HealthStatistics                   : 
                                     DeployedServicePackage : 2 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a><span data-ttu-id="f2192-306">REST</span><span class="sxs-lookup"><span data-stu-id="f2192-306">REST</span></span>
<span data-ttu-id="f2192-307">Kaphat a telepített alkalmazás health használata az egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.</span><span class="sxs-lookup"><span data-stu-id="f2192-307">You can get deployed application health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-deployed-service-package-health"></a><span data-ttu-id="f2192-308">Telepített szolgáltatások csomag állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="f2192-308">Get deployed service package health</span></span>
<span data-ttu-id="f2192-309">Értéket ad vissza egy telepített szolgáltatást csomag entitás állapotának hello.</span><span class="sxs-lookup"><span data-stu-id="f2192-309">Returns hello health of a deployed service package entity.</span></span> <span data-ttu-id="f2192-310">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="f2192-310">Input:</span></span>

* <span data-ttu-id="f2192-311">[Szükséges] hello alkalmazásnév (URI), a csomópont neve (karakterlánc) és szolgáltatás jegyzékfájljának nevét (karakterlánc) azonosító hello telepített service-csomag.</span><span class="sxs-lookup"><span data-stu-id="f2192-311">[Required] hello application name (URI), node name (string), and service manifest name (string) that identify hello deployed service package.</span></span>
* <span data-ttu-id="f2192-312">[Választható] hello az alkalmazás állapotházirendje használt toooverride hello alkalmazás-jegyzékfájl házirend.</span><span class="sxs-lookup"><span data-stu-id="f2192-312">[Optional] hello application health policy used toooverride hello application manifest policy.</span></span>
* <span data-ttu-id="f2192-313">[Választható] Adja meg, mely tételek események szűrők iránt, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák).</span><span class="sxs-lookup"><span data-stu-id="f2192-313">[Optional] Filters for events that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="f2192-314">Összes esemény használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="f2192-314">All events are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>

### <a name="api"></a><span data-ttu-id="f2192-315">API</span><span class="sxs-lookup"><span data-stu-id="f2192-315">API</span></span>
<span data-ttu-id="f2192-316">tooget hello állapotát egy telepített szervizcsomag keresztül hello API, hozzon létre egy `FabricClient` és hívás hello [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) a HealthManager metódust.</span><span class="sxs-lookup"><span data-stu-id="f2192-316">tooget hello health of a deployed service package through hello API, create a `FabricClient` and call hello [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) method on its HealthManager.</span></span> <span data-ttu-id="f2192-317">Választható paraméterek: toospecify, használjon [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="f2192-317">toospecify optional parameters, use [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).</span></span>

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a><span data-ttu-id="f2192-318">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2192-318">PowerShell</span></span>
<span data-ttu-id="f2192-319">hello parancsmag tooget telepített hello szolgáltatás csomag állapota van [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth).</span><span class="sxs-lookup"><span data-stu-id="f2192-319">hello cmdlet tooget hello deployed service package health is [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth).</span></span> <span data-ttu-id="f2192-320">Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f2192-320">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="f2192-321">Ha egy alkalmazás központi telepítése, toosee futtatása [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) , és tekintse meg a hello központilag telepített alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="f2192-321">toosee where an application is deployed, run [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) and look at hello deployed applications.</span></span> <span data-ttu-id="f2192-322">service-csomagok jelenleg egy alkalmazásban keresse meg hello toosee telepített szolgáltatás csomag gyermekek hello [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) kimeneti.</span><span class="sxs-lookup"><span data-stu-id="f2192-322">toosee which service packages are in an application, look at hello deployed service package children in hello [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) output.</span></span>

<span data-ttu-id="f2192-323">hello következő parancsmag lekéri hello hello állapotának **WordCountServicePkg** hello a service-csomag **fabric: / WordCount** a központilag telepített alkalmazás **_Node_2**.</span><span class="sxs-lookup"><span data-stu-id="f2192-323">hello following cmdlet gets hello health of hello **WordCountServicePkg** service package of hello **fabric:/WordCount** application deployed on **_Node_2**.</span></span> <span data-ttu-id="f2192-324">hello entitásnak van **System.Hosting** sikeres service-csomag és a belépési pont aktiválása és sikeres szolgáltatástípus regisztráció a jelentésekre.</span><span class="sxs-lookup"><span data-stu-id="f2192-324">hello entity has **System.Hosting** reports for successful service-package and entry-point activation, and successful service-type registration.</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_2
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131444422267693359
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131444422267903345
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131444422272458374
                             SentAt                : 7/13/2017 5:57:07 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a><span data-ttu-id="f2192-325">REST</span><span class="sxs-lookup"><span data-stu-id="f2192-325">REST</span></span>
<span data-ttu-id="f2192-326">Kaphat a telepített szolgáltatások csomag health használata az egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.</span><span class="sxs-lookup"><span data-stu-id="f2192-326">You can get deployed service package health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="health-chunk-queries"></a><span data-ttu-id="f2192-327">Adatrészlet állapotlekérdezések száma</span><span class="sxs-lookup"><span data-stu-id="f2192-327">Health chunk queries</span></span>
<span data-ttu-id="f2192-328">hello adatrészlet állapotlekérdezések több szintű fürt gyermekek (rekurzív), egy bemeneti szűrőket adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-328">hello health chunk queries can return multi-level cluster children (recursively), per input filters.</span></span> <span data-ttu-id="f2192-329">Támogatja a speciális szűrők, amelyek lehetővé teszik a magas fokú rugalmasságot biztosít a hello gyermekek kiválasztása toobe adott vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-329">It supports advanced filters that allow a lot of flexibility in choosing hello children toobe returned.</span></span> <span data-ttu-id="f2192-330">hello szűrőket adhat meg a gyermekek hello egyedi azonosítója vagy más csoportazonosítókhoz és/vagy állapotokat.</span><span class="sxs-lookup"><span data-stu-id="f2192-330">hello filters can specify children by hello unique identifier or by other group identifiers and/or health states.</span></span> <span data-ttu-id="f2192-331">Alapértelmezés szerint nincsenek gyermekei tartoznak, mint első szintű gyermekek mindig tartalmazó megakadályozását toohealth parancsokat.</span><span class="sxs-lookup"><span data-stu-id="f2192-331">By default, no children are included, as opposed toohealth commands that always include first-level children.</span></span>

<span data-ttu-id="f2192-332">Hello [állapotlekérdezések](service-fabric-view-entities-aggregated-health.md#health-queries) hello gyermekeinek visszatérési csak az első szintű megadott szükséges szűrők egy entitást.</span><span class="sxs-lookup"><span data-stu-id="f2192-332">hello [health queries](service-fabric-view-entities-aggregated-health.md#health-queries) return only first-level children of hello specified entity per required filters.</span></span> <span data-ttu-id="f2192-333">hello gyermekek tooget hello gyermekei, meg kell hívnia további rendszerállapot API-k minden egyes entitásnál iránt.</span><span class="sxs-lookup"><span data-stu-id="f2192-333">tooget hello children of hello children, you must call additional health APIs for each entity of interest.</span></span> <span data-ttu-id="f2192-334">Hasonlóképpen konkrét személyek tooget hello állapotát, meg kell hívnia egy rendszerállapot API minden kívánt entitás.</span><span class="sxs-lookup"><span data-stu-id="f2192-334">Similarly, tooget hello health of specific entities, you must call one health API for each desired entity.</span></span> <span data-ttu-id="f2192-335">hello adatrészlet lekérdezés egyéb szűrési lehetővé teszi toorequest több elem egyik fontos egy lekérdezést, minimalizálja a hello üzenet mérete és hello üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="f2192-335">hello chunk query advanced filtering allows you toorequest multiple items of interest in one query, minimizing hello message size and hello number of messages.</span></span>

<span data-ttu-id="f2192-336">hello hello adatrészlet lekérdezés értéke, hogy kaphat állapota további fürt entitásokat (vélhetően fürt entitásokhoz kezdve a kötelező gyökérszintű) egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="f2192-336">hello value of hello chunk query is that you can get health state for more cluster entities (potentially all cluster entities starting at required root) in one call.</span></span> <span data-ttu-id="f2192-337">Összetett állapot lekérdezés is express, mint:</span><span class="sxs-lookup"><span data-stu-id="f2192-337">You can express complex health query such as:</span></span>

* <span data-ttu-id="f2192-338">Visszatérési csak alkalmazások hiba, és ezeket az alkalmazásokat tartalmaznak minden figyelmeztetés vagy hiba.</span><span class="sxs-lookup"><span data-stu-id="f2192-338">Return only applications in error, and for those applications include all services in warning or error.</span></span> <span data-ttu-id="f2192-339">A kiadott szolgáltatások tartalmazza az összes partíció.</span><span class="sxs-lookup"><span data-stu-id="f2192-339">For returned services, include all partitions.</span></span>
* <span data-ttu-id="f2192-340">A név által meghatározott négy alkalmazások csak hello állapotának visszaadása.</span><span class="sxs-lookup"><span data-stu-id="f2192-340">Return only hello health of four applications, specified by their names.</span></span>
* <span data-ttu-id="f2192-341">A kívánt alkalmazáshoz típusú alkalmazások csak hello állapotának visszaadása.</span><span class="sxs-lookup"><span data-stu-id="f2192-341">Return only hello health of applications of a desired application type.</span></span>
* <span data-ttu-id="f2192-342">Térjen vissza az összes üzembe helyezett entitások csomóponton.</span><span class="sxs-lookup"><span data-stu-id="f2192-342">Return all deployed entities on a node.</span></span> <span data-ttu-id="f2192-343">Összes alkalmazás, hello megadott csomóponton összes üzembe helyezett alkalmazás és minden telepített hello szolgáltatás csomagok ezen a csomóponton adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-343">Returns all applications, all deployed applications on hello specified node and all hello deployed service packages on that node.</span></span>
* <span data-ttu-id="f2192-344">Hiba található összes replika visszaadása.</span><span class="sxs-lookup"><span data-stu-id="f2192-344">Return all replicas in error.</span></span> <span data-ttu-id="f2192-345">Alkalmazások, szolgáltatások, partíciók és csak a replikák hiba adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-345">Returns all applications, services, partitions, and only replicas in error.</span></span>
* <span data-ttu-id="f2192-346">Térjen vissza az összes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f2192-346">Return all applications.</span></span> <span data-ttu-id="f2192-347">Az egy megadott szolgáltatás tartalmazza az összes partíció.</span><span class="sxs-lookup"><span data-stu-id="f2192-347">For a specified service, include all partitions.</span></span>

<span data-ttu-id="f2192-348">Hello állapotfigyelő adatrészlet lekérdezés jelenleg ki van téve, csak a hello fürt entitás.</span><span class="sxs-lookup"><span data-stu-id="f2192-348">Currently, hello health chunk query is exposed only for hello cluster entity.</span></span> <span data-ttu-id="f2192-349">Azt jelzi, hogy a fürt állapotfigyelő adattömb, amely tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="f2192-349">It returns a cluster health chunk, which contains:</span></span>

* <span data-ttu-id="f2192-350">hello fürt állapotát összesíti.</span><span class="sxs-lookup"><span data-stu-id="f2192-350">hello cluster aggregated health state.</span></span>
* <span data-ttu-id="f2192-351">hello egészségügyi állapot adatrészlet csomópontlista, figyelembe vegyék bemeneti szűrők.</span><span class="sxs-lookup"><span data-stu-id="f2192-351">hello health state chunk list of nodes that respect input filters.</span></span>
* <span data-ttu-id="f2192-352">hello egészségügyi állapot adatrészlet alkalmazások listáját tiszteletben tartják bemeneti szűrők.</span><span class="sxs-lookup"><span data-stu-id="f2192-352">hello health state chunk list of applications that respect input filters.</span></span> <span data-ttu-id="f2192-353">Egyes alkalmazás állapotának állapot adattömbök adatrészlet listáját összes szolgáltatások tiszteletben bemeneti szűrők és adatrészlet hello szűrők tiszteletben összes üzembe helyezett alkalmazások listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f2192-353">Each application health state chunk contains a chunk list with all services that respect input filters and a chunk list with all deployed applications that respect hello filters.</span></span> <span data-ttu-id="f2192-354">A szolgáltatások és telepített alkalmazások hello gyermekek esetében azonos.</span><span class="sxs-lookup"><span data-stu-id="f2192-354">Same for hello children of services and deployed applications.</span></span> <span data-ttu-id="f2192-355">Ezzel a módszerrel hello fürt összes entitásának potenciálisan visszaadható kérésre, hierarchikus.</span><span class="sxs-lookup"><span data-stu-id="f2192-355">This way, all entities in hello cluster can be potentially returned if requested, in a hierarchical fashion.</span></span>

### <a name="cluster-health-chunk-query"></a><span data-ttu-id="f2192-356">Fürt állapotfigyelő adatrészlet lekérdezés</span><span class="sxs-lookup"><span data-stu-id="f2192-356">Cluster health chunk query</span></span>
<span data-ttu-id="f2192-357">Beolvasása hello hello fürt entitás állapotát, és hello hierarchikus egészségügyi állapot adattömbök szükséges gyermekek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f2192-357">Returns hello health of hello cluster entity and contains hello hierarchical health state chunks of required children.</span></span> <span data-ttu-id="f2192-358">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="f2192-358">Input:</span></span>

* <span data-ttu-id="f2192-359">[Választható] hello fürt állapotházirend használt tooevaluate hello csomópontokat és hello fürthöz kapcsolódó események.</span><span class="sxs-lookup"><span data-stu-id="f2192-359">[Optional] hello cluster health policy used tooevaluate hello nodes and hello cluster events.</span></span>
* <span data-ttu-id="f2192-360">[Választható] hello alkalmazás állapotának házirend-hozzárendelés, az hello állapotházirendeket toooverride hello alkalmazás-jegyzékfájl házirendek használható.</span><span class="sxs-lookup"><span data-stu-id="f2192-360">[Optional] hello application health policy map, with hello health policies used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="f2192-361">[Választható] Csomópont-és alkalmazásokat, amelyek adja meg, mely tételek érdeklő, és vissza kell adni a hello eredmény.</span><span class="sxs-lookup"><span data-stu-id="f2192-361">[Optional] Filters for nodes and applications that specify which entries are of interest and should be returned in hello result.</span></span> <span data-ttu-id="f2192-362">hello szűrők adott tooan entitás/csoport az entitások vagy azon a szinten alkalmazható tooall entitásokat.</span><span class="sxs-lookup"><span data-stu-id="f2192-362">hello filters are specific tooan entity/group of entities or are applicable tooall entities at that level.</span></span> <span data-ttu-id="f2192-363">szűrők hello listáját egy általános szűrő és/vagy az adott azonosítók toofine-felbontása entitások hello lekérdezés által visszaadott szűrők tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="f2192-363">hello list of filters can contain one general filter and/or filters for specific identifiers toofine-grain entities returned by hello query.</span></span> <span data-ttu-id="f2192-364">Ha üres, hello gyermeke nem lehet megjeleníteni alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="f2192-364">If empty, hello children are not returned by default.</span></span>
  <span data-ttu-id="f2192-365">További információk: hello szűrők [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) és [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter).</span><span class="sxs-lookup"><span data-stu-id="f2192-365">Read more about hello filters at [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) and [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter).</span></span> <span data-ttu-id="f2192-366">hello alkalmazás szűrők is rekurzív módon speciális szűrők megadása a gyermekek számára.</span><span class="sxs-lookup"><span data-stu-id="f2192-366">hello application filters can recursively specify advanced filters for children.</span></span>

<span data-ttu-id="f2192-367">hello adatrészlet eredmény hello szűrők tiszteletben hello alárendelt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f2192-367">hello chunk result includes hello children that respect hello filters.</span></span>

<span data-ttu-id="f2192-368">Jelenleg hello adatrészlet lekérdezés nem ad vissza a nem megfelelő értékelések vagy entitás események.</span><span class="sxs-lookup"><span data-stu-id="f2192-368">Currently, hello chunk query does not return unhealthy evaluations or entity events.</span></span> <span data-ttu-id="f2192-369">További adatokat hello meglévő fürt állapotának lekérdezés nyerhetők.</span><span class="sxs-lookup"><span data-stu-id="f2192-369">That extra information can be obtained using hello existing cluster health query.</span></span>

### <a name="api"></a><span data-ttu-id="f2192-370">API</span><span class="sxs-lookup"><span data-stu-id="f2192-370">API</span></span>
<span data-ttu-id="f2192-371">tooget fürt állapotfigyelő darabos, hozzon létre egy `FabricClient` és hívás hello [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) metódust a **HealthManager**.</span><span class="sxs-lookup"><span data-stu-id="f2192-371">tooget cluster health chunk, create a `FabricClient` and call hello [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) method on its **HealthManager**.</span></span> <span data-ttu-id="f2192-372">Adhasson [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) toodescribe házirendek és a speciális szűrők.</span><span class="sxs-lookup"><span data-stu-id="f2192-372">You can pass in [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) toodescribe health policies and advanced filters.</span></span>

<span data-ttu-id="f2192-373">hello következő kód jogosultságot kap fürt állapotfigyelő adatrészlet speciális szűrők.</span><span class="sxs-lookup"><span data-stu-id="f2192-373">hello following code gets cluster health chunk with advanced filters.</span></span>

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except hello ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="f2192-374">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2192-374">PowerShell</span></span>
<span data-ttu-id="f2192-375">hello parancsmag tooget hello fürt állapota [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk).</span><span class="sxs-lookup"><span data-stu-id="f2192-375">hello cmdlet tooget hello cluster health is [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk).</span></span> <span data-ttu-id="f2192-376">Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f2192-376">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="f2192-377">hello következő kód jogosultságot kap csomópontok csak akkor, ha egy konkrét csomóponton, amely mindig vissza kell adni az kivételével hiba vannak.</span><span class="sxs-lookup"><span data-stu-id="f2192-377">hello following code gets nodes only if they are in Error except for a specific node, which should always be returned.</span></span>

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in hello cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters


HealthState                  : Warning
NodeHealthStateChunks        : 
                               TotalCount            : 1
                               
                               NodeName              : _Node_1
                               HealthState           : Ok
                               
ApplicationHealthStateChunks : None
```

<span data-ttu-id="f2192-378">a következő parancsmag hello lekérdezi a fürt adattömbök alkalmazás szűrők.</span><span class="sxs-lookup"><span data-stu-id="f2192-378">hello following cmdlet gets cluster chunk with application filters.</span></span>

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 1
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks : 
                                TotalCount            : 1
                               
                                ServiceName           : fabric:/WordCount/WordCountService
                                HealthState           : Error
                                PartitionHealthStateChunks : 
                                    TotalCount            : 1
                               
                                    PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                                    HealthState           : Error
                                    ReplicaHealthStateChunks : 
                                        TotalCount            : 5
                               
                                        ReplicaOrInstanceId   : 131444422293118720
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293118721
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113678
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113679
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422260002646
                                        HealthState           : Error
```

<span data-ttu-id="f2192-379">hello következő parancsmag entitásokat ad vissza összes üzembe helyezett egy csomóponton.</span><span class="sxs-lookup"><span data-stu-id="f2192-379">hello following cmdlet returns all deployed entities on a node.</span></span>

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 2
                               
                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : FAS
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
                               
                               
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : WordCountServicePkg
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
```

### <a name="rest"></a><span data-ttu-id="f2192-380">REST</span><span class="sxs-lookup"><span data-stu-id="f2192-380">REST</span></span>
<span data-ttu-id="f2192-381">Kaphat a fürt állapotának rendszer egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) házirendek és a speciális szűrők hello törzsében leírt foglal magában.</span><span class="sxs-lookup"><span data-stu-id="f2192-381">You can get cluster health chunk with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) that includes health policies and advanced filters described in hello body.</span></span>

## <a name="general-queries"></a><span data-ttu-id="f2192-382">Általános lekérdezések</span><span class="sxs-lookup"><span data-stu-id="f2192-382">General queries</span></span>
<span data-ttu-id="f2192-383">Általános lekérdezésekben Service Fabric entitások, megadott típusú listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-383">General queries return a list of Service Fabric entities of a specified type.</span></span> <span data-ttu-id="f2192-384">Hello API keresztül ki vannak téve (keresztül hello metódusai **FabricClient.QueryManager**), PowerShell-parancsmagok és a többi.</span><span class="sxs-lookup"><span data-stu-id="f2192-384">They are exposed through hello API (via hello methods on **FabricClient.QueryManager**), PowerShell cmdlets, and REST.</span></span> <span data-ttu-id="f2192-385">Ezeket a lekérdezéseket a segédlekérdezések több összetevőkből összesíteni.</span><span class="sxs-lookup"><span data-stu-id="f2192-385">These queries aggregate subqueries from multiple components.</span></span> <span data-ttu-id="f2192-386">Hello őket egyik [a health Store adatbázisban](service-fabric-health-introduction.md#health-store), hello tölti fel, amelyek összesített állapota minden egyes lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="f2192-386">One of them is hello [health store](service-fabric-health-introduction.md#health-store), which populates hello aggregated health state for each query result.</span></span>  

> [!NOTE]
> <span data-ttu-id="f2192-387">Általános lekérdezések hello entitás állapotát összesíti hello vissza, és nem tartalmaz gazdag egészségügyi adatokat.</span><span class="sxs-lookup"><span data-stu-id="f2192-387">General queries return hello aggregated health state of hello entity and do not contain rich health data.</span></span> <span data-ttu-id="f2192-388">Entitás állapota nem kifogástalan, ha nyomon lehet követni az egészségügyi lekérdezések tooget a egészségügyi adatok, beleértve az események, a gyermek állapotokat és a nem megfelelő értékelések.</span><span class="sxs-lookup"><span data-stu-id="f2192-388">If an entity is not healthy, you can follow up with health queries tooget all its health information, including events, child health states, and unhealthy evaluations.</span></span>
>
>

<span data-ttu-id="f2192-389">Általános lekérdezések vissza egy entitás ismeretlen állapot, akkor lehetséges, hogy hello a health Store adatbázisban nem rendelkezik teljes hello entitás adatait.</span><span class="sxs-lookup"><span data-stu-id="f2192-389">If general queries return an unknown health state for an entity, it's possible that hello health store doesn't have complete data about hello entity.</span></span> <span data-ttu-id="f2192-390">Akkor is, hogy a segédlekérdezés toohello a health Store adatbázisban nem volt sikeres (például egy kommunikációs hiba vagy hello a health Store adatbázisban halmozódni volt).</span><span class="sxs-lookup"><span data-stu-id="f2192-390">It's also possible that a subquery toohello health store wasn't successful (for example, there was a communication error, or hello health store was throttled).</span></span> <span data-ttu-id="f2192-391">Hello entitás állapotának lekérdezés nyomon követéséhez.</span><span class="sxs-lookup"><span data-stu-id="f2192-391">Follow up with a health query for hello entity.</span></span> <span data-ttu-id="f2192-392">Hello allekérdezés hibát jelzett a átmeneti, például a hálózati problémák, a követési lekérdezés esetleg sikeresek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="f2192-392">If hello subquery encountered transient errors, such as network issues, this follow-up query may succeed.</span></span> <span data-ttu-id="f2192-393">Azt is kaphat további részleteket a hello health Store adatbázisból kapcsolatos miért hello entitás nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="f2192-393">It may also give you more details from hello health store about why hello entity is not exposed.</span></span>

<span data-ttu-id="f2192-394">lekérdezések hello **HealthState** a szervezetek a következők:</span><span class="sxs-lookup"><span data-stu-id="f2192-394">hello queries that contain **HealthState** for entities are:</span></span>

* <span data-ttu-id="f2192-395">Csomópontlista: hello lista csomópontok (lapozható) hello fürt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-395">Node list: Returns hello list nodes in hello cluster (paged).</span></span>
  * <span data-ttu-id="f2192-396">Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)</span><span class="sxs-lookup"><span data-stu-id="f2192-396">API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)</span></span>
  * <span data-ttu-id="f2192-397">PowerShell: Get-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="f2192-397">PowerShell: Get-ServiceFabricNode</span></span>
* <span data-ttu-id="f2192-398">Alkalmazáslista: hello fürt (lapozható) alkalmazások hello listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-398">Application list: Returns hello list of applications in hello cluster (paged).</span></span>
  * <span data-ttu-id="f2192-399">Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)</span><span class="sxs-lookup"><span data-stu-id="f2192-399">API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)</span></span>
  * <span data-ttu-id="f2192-400">PowerShell: Get-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="f2192-400">PowerShell: Get-ServiceFabricApplication</span></span>
* <span data-ttu-id="f2192-401">Lista: egy alkalmazás (lapozható) szolgáltatások hello listája értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-401">Service list: Returns hello list of services in an application (paged).</span></span>
  * <span data-ttu-id="f2192-402">Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)</span><span class="sxs-lookup"><span data-stu-id="f2192-402">API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)</span></span>
  * <span data-ttu-id="f2192-403">PowerShell: Get-ServiceFabricService</span><span class="sxs-lookup"><span data-stu-id="f2192-403">PowerShell: Get-ServiceFabricService</span></span>
* <span data-ttu-id="f2192-404">Partition listában: (lapozható) szolgáltatás a partíciók hello listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-404">Partition list: Returns hello list of partitions in a service (paged).</span></span>
  * <span data-ttu-id="f2192-405">Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)</span><span class="sxs-lookup"><span data-stu-id="f2192-405">API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)</span></span>
  * <span data-ttu-id="f2192-406">PowerShell: Get-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="f2192-406">PowerShell: Get-ServiceFabricPartition</span></span>
* <span data-ttu-id="f2192-407">Listáján: egy partíció (lapozható) replikák hello listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-407">Replica list: Returns hello list of replicas in a partition (paged).</span></span>
  * <span data-ttu-id="f2192-408">Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)</span><span class="sxs-lookup"><span data-stu-id="f2192-408">API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)</span></span>
  * <span data-ttu-id="f2192-409">PowerShell: Get-ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="f2192-409">PowerShell: Get-ServiceFabricReplica</span></span>
* <span data-ttu-id="f2192-410">Telepített alkalmazások listája: csomóponton központilag telepített alkalmazások hello listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f2192-410">Deployed application list: Returns hello list of deployed applications on a node.</span></span>
  * <span data-ttu-id="f2192-411">Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)</span><span class="sxs-lookup"><span data-stu-id="f2192-411">API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)</span></span>
  * <span data-ttu-id="f2192-412">PowerShell: Get-ServiceFabricDeployedApplication</span><span class="sxs-lookup"><span data-stu-id="f2192-412">PowerShell: Get-ServiceFabricDeployedApplication</span></span>
* <span data-ttu-id="f2192-413">Telepített szolgáltatás csomaglista: beolvasása hello listát a szolgáltatás egy telepített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f2192-413">Deployed service package list: Returns hello list of service packages in a deployed application.</span></span>
  * <span data-ttu-id="f2192-414">Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)</span><span class="sxs-lookup"><span data-stu-id="f2192-414">API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)</span></span>
  * <span data-ttu-id="f2192-415">PowerShell: Get-ServiceFabricDeployedApplication</span><span class="sxs-lookup"><span data-stu-id="f2192-415">PowerShell: Get-ServiceFabricDeployedApplication</span></span>

> [!NOTE]
> <span data-ttu-id="f2192-416">Lapozható eredmények hello lekérdezések némelyike visszaadása.</span><span class="sxs-lookup"><span data-stu-id="f2192-416">Some of hello queries return paged results.</span></span> <span data-ttu-id="f2192-417">hello ezeket a lekérdezéseket a return származtatva listáját [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1).</span><span class="sxs-lookup"><span data-stu-id="f2192-417">hello return of these queries is a list derived from [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1).</span></span> <span data-ttu-id="f2192-418">Hello eredmények nem férnek el egy üzenetet, ha csak egy lap ad vissza, és egy ContinuationToken, amely nyomon követi, ahol számbavételi leállt.</span><span class="sxs-lookup"><span data-stu-id="f2192-418">If hello results do not fit a message, only a page is returned and a ContinuationToken that tracks where enumeration stopped.</span></span> <span data-ttu-id="f2192-419">Továbbra is toocall hello azonos lekérdezés számára pedig hello folytatási kód hello előző lekérdezés tooget következő eredménybe.</span><span class="sxs-lookup"><span data-stu-id="f2192-419">Continue toocall hello same query and pass in hello continuation token from hello previous query tooget next results.</span></span>
>
>

### <a name="examples"></a><span data-ttu-id="f2192-420">Példák</span><span class="sxs-lookup"><span data-stu-id="f2192-420">Examples</span></span>
<span data-ttu-id="f2192-421">hello következő kód jogosultságot kap a nem megfelelő alkalmazások hello hello fürt:</span><span class="sxs-lookup"><span data-stu-id="f2192-421">hello following code gets hello unhealthy applications in hello cluster:</span></span>

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

<span data-ttu-id="f2192-422">hello következő parancsmag lekéri hello alkalmazás részletei hello fabric: / WordCount alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f2192-422">hello following cmdlet gets hello application details for hello fabric:/WordCount application.</span></span> <span data-ttu-id="f2192-423">Láthatja, hogy állapot figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="f2192-423">Notice that health state is at warning.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

<span data-ttu-id="f2192-424">hello következő parancsmag lekéri hiba állapotot hello szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="f2192-424">hello following cmdlet gets hello services with a health state of error:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Error"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Error
```

## <a name="cluster-and-application-upgrades"></a><span data-ttu-id="f2192-425">Fürt- és frissítések</span><span class="sxs-lookup"><span data-stu-id="f2192-425">Cluster and application upgrades</span></span>
<span data-ttu-id="f2192-426">Hello fürt és az alkalmazás egy figyelt frissítés során a Service Fabric ellenőrzi, hogy minden továbbra is kifogástalan állapotát tooensure.</span><span class="sxs-lookup"><span data-stu-id="f2192-426">During a monitored upgrade of hello cluster and application, Service Fabric checks health tooensure that everything remains healthy.</span></span> <span data-ttu-id="f2192-427">Ha egy entitás nem kifogástalan, mint a konfigurált házirendek kiértékelése, hello frissítés alkalmazva frissítés rendszerre jellemző házirendek toodetermine hello tovább művelet.</span><span class="sxs-lookup"><span data-stu-id="f2192-427">If an entity is unhealthy as evaluated by using configured health policies, hello upgrade applies upgrade-specific policies toodetermine hello next action.</span></span> <span data-ttu-id="f2192-428">hello frissítés szüneteltetett tooallow felhasználói beavatkozást (például a hiba kijavítása, vagy házirendek módosítása), vagy az lehet, hogy automatikusan állítsa vissza toohello helyes verziót.</span><span class="sxs-lookup"><span data-stu-id="f2192-428">hello upgrade may be paused tooallow user interaction (such as fixing error conditions or changing policies), or it may automatically roll back toohello previous good version.</span></span>

<span data-ttu-id="f2192-429">Során egy *fürt* frissítési, hogy megkaphassa hello fürt frissítési állapot.</span><span class="sxs-lookup"><span data-stu-id="f2192-429">During a *cluster* upgrade, you can get hello cluster upgrade status.</span></span> <span data-ttu-id="f2192-430">hello frissítésének állapota nem kifogástalan értékelések, mely pont toowhat állapota nem megfelelő hello fürt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f2192-430">hello upgrade status includes unhealthy evaluations, which point toowhat is unhealthy in hello cluster.</span></span> <span data-ttu-id="f2192-431">Ha hello frissítés toohealth problémák miatt vissza lesz állítva, hello frissítési állapot emlékszik hello utolsó sérült okok miatt.</span><span class="sxs-lookup"><span data-stu-id="f2192-431">If hello upgrade is rolled back due toohealth issues, hello upgrade status remembers hello last unhealthy reasons.</span></span> <span data-ttu-id="f2192-432">Az információk segítségével a rendszergazdák kivizsgálni, mi a probléma után hello frissítés visszaállításra, vagy leállt.</span><span class="sxs-lookup"><span data-stu-id="f2192-432">This information can help administrators investigate what went wrong after hello upgrade rolled back or stopped.</span></span>

<span data-ttu-id="f2192-433">Ehhez hasonlóan során egy *alkalmazás* frissítés, bármely sérült értékelések található hello alkalmazás frissítési állapot.</span><span class="sxs-lookup"><span data-stu-id="f2192-433">Similarly, during an *application* upgrade, any unhealthy evaluations are contained in hello application upgrade status.</span></span>

<span data-ttu-id="f2192-434">hello következő állapotát jeleníti meg hello alkalmazás frissítési a módosított fabric: / WordCount alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f2192-434">hello following shows hello application upgrade status for a modified fabric:/WordCount application.</span></span> <span data-ttu-id="f2192-435">A figyelő hibát jelzett az egyik a replikáit.</span><span class="sxs-lookup"><span data-stu-id="f2192-435">A watchdog reported an error on one of its replicas.</span></span> <span data-ttu-id="f2192-436">hello frissítése folyamatban van, mert hello egészségügyi ellenőrzések nem veszik.</span><span class="sxs-lookup"><span data-stu-id="f2192-436">hello upgrade is rolling back because hello health checks are not respected.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2017 5:23:26 PM
FailureTimestampUtc           : 4/21/2017 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

<span data-ttu-id="f2192-437">További információk a hello [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="f2192-437">Read more about hello [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

## <a name="use-health-evaluations-tootroubleshoot"></a><span data-ttu-id="f2192-438">Állapotfigyelő értékelések tootroubleshoot használata</span><span class="sxs-lookup"><span data-stu-id="f2192-438">Use health evaluations tootroubleshoot</span></span>
<span data-ttu-id="f2192-439">Ha probléma van a hello fürt vagy egy alkalmazás, hello fürt vagy az alkalmazás állapotának toopinpoint nézze meg mi.</span><span class="sxs-lookup"><span data-stu-id="f2192-439">Whenever there is an issue with hello cluster or an application, look at hello cluster or application health toopinpoint what is wrong.</span></span> <span data-ttu-id="f2192-440">a nem megfelelő értékelések hello milyen kiváltott hello aktuális állapota nem kifogástalan részleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="f2192-440">hello unhealthy evaluations provide details about what triggered hello current unhealthy state.</span></span> <span data-ttu-id="f2192-441">Ha szeretné, részletezhető le nem megfelelő állapotú gyermekfigyelőket entitások tooidentify hello alapvető okát.</span><span class="sxs-lookup"><span data-stu-id="f2192-441">If you need to, you can drill down into unhealthy child entities tooidentify hello root cause.</span></span>

<span data-ttu-id="f2192-442">Vegye figyelembe például az alkalmazás nem megfelelő, mert a replikáit egyik egy esetleges hibajelentésben való megjelenítéshez.</span><span class="sxs-lookup"><span data-stu-id="f2192-442">For example, consider an application unhealthy because there is an error report on one of its replicas.</span></span> <span data-ttu-id="f2192-443">hello következő Powershell-parancsmag jeleníti meg a nem megfelelő értékelések hello:</span><span class="sxs-lookup"><span data-stu-id="f2192-443">hello following Powershell cmdlet shows hello unhealthy evaluations:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount -EventsFilter None -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.
                                  
                                        Unhealthy replica: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', ReplicaOrInstanceId='131444422260002646', AggregatedHealthState='Error'.
                                  
                                            Error event: SourceId='MyWatchdog', Property='Memory'.
                                  
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

<span data-ttu-id="f2192-444">Vessen egy pillantást hello replika tooget további információt:</span><span class="sxs-lookup"><span data-stu-id="f2192-444">You can look at hello replica tooget more information:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricReplicaHealth -ReplicaOrInstanceId 131444422260002646 -PartitionId af2e3e44-a8f8-45ac-9f31-4093eb897600


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Error
UnhealthyEvaluations  : 
                        Error event: SourceId='MyWatchdog', Property='Memory'.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
                        SourceId              : MyWatchdog
                        Property              : Memory
                        HealthState           : Error
                        SequenceNumber        : 131444451657749403
                        SentAt                : 7/13/2017 6:46:05 PM
                        ReceivedAt            : 7/13/2017 6:46:05 PM
                        TTL                   : Infinite
                        Description           : 
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 7/13/2017 6:46:05 PM, LastOk = 1/1/0001 12:00:00 AM
```

> [!NOTE]
> <span data-ttu-id="f2192-445">hello sérült értékelések megjelenítése hello első OK hello entitás kiértékelése toocurrent állapotát.</span><span class="sxs-lookup"><span data-stu-id="f2192-445">hello unhealthy evaluations show hello first reason hello entity is evaluated toocurrent health state.</span></span> <span data-ttu-id="f2192-446">Előfordulhat, hogy több más kiváltó események ebben az állapotban, de nem kell megjelenő hello értékelések.</span><span class="sxs-lookup"><span data-stu-id="f2192-446">There may be multiple other events that trigger this state, but they are not be reflected in hello evaluations.</span></span> <span data-ttu-id="f2192-447">tooget hello állapotfigyelő entitások toofigure ki minden hello sérült jelentések hello fürt a részletezés olvashat.</span><span class="sxs-lookup"><span data-stu-id="f2192-447">tooget more information, drill down into hello health entities toofigure out all hello unhealthy reports in hello cluster.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="f2192-448">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2192-448">Next steps</span></span>
[<span data-ttu-id="f2192-449">Használja a rendszer állapotát jelentések tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="f2192-449">Use system health reports tootroubleshoot</span></span>](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[<span data-ttu-id="f2192-450">Adja hozzá az egyéni Service Fabric állapotjelentések száma</span><span class="sxs-lookup"><span data-stu-id="f2192-450">Add custom Service Fabric health reports</span></span>](service-fabric-report-health.md)

[<span data-ttu-id="f2192-451">Hogyan tooreport és ellenőrzés szolgáltatás állapotát</span><span class="sxs-lookup"><span data-stu-id="f2192-451">How tooreport and check service health</span></span>](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[<span data-ttu-id="f2192-452">Figyelése és diagnosztizálása helyileg szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f2192-452">Monitor and diagnose services locally</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="f2192-453">A Service Fabric-alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="f2192-453">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

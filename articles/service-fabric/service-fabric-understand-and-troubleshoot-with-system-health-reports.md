---
title: "Hibaelhárítás a rendszerállapot-jelentések |} Microsoft Docs"
description: "A rendszerállapot-jelentések küldött hibaelhárítási fürt vagy az alkalmazással kapcsolatos problémák Azure Service Fabric-összetevők és használatát ismerteti."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 52574ea7-eb37-47e0-a20a-101539177625
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: 54e20146b2f1e0ca6153b66319be70c6f7c2fb59
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-system-health-reports-to-troubleshoot"></a><span data-ttu-id="fe34d-103">Rendszerállapot-jelentések használata a hibaelhárítás során</span><span class="sxs-lookup"><span data-stu-id="fe34d-103">Use system health reports to troubleshoot</span></span>
<span data-ttu-id="fe34d-104">Az Azure Service Fabric összetevői jelentést nem a fürt összes entitás.</span><span class="sxs-lookup"><span data-stu-id="fe34d-104">Azure Service Fabric components report out of the box on all entities in the cluster.</span></span> <span data-ttu-id="fe34d-105">A [a health Store adatbázisban](service-fabric-health-introduction.md#health-store) hoz létre, és törli a rendszer-jelentéseken alapuló entitásokat.</span><span class="sxs-lookup"><span data-stu-id="fe34d-105">The [health store](service-fabric-health-introduction.md#health-store) creates and deletes entities based on the system reports.</span></span> <span data-ttu-id="fe34d-106">Azt is rendszerezi azokat a hierarchiában, amely rögzíti az entitás interakciókat.</span><span class="sxs-lookup"><span data-stu-id="fe34d-106">It also organizes them in a hierarchy that captures entity interactions.</span></span>

> [!NOTE]
> <span data-ttu-id="fe34d-107">Állapotfigyelő kapcsolatos fogalmak megismerése, olvassa el: [Service Fabric állapotmodell](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fe34d-107">To understand health-related concepts, read more at [Service Fabric health model](service-fabric-health-introduction.md).</span></span>
> 
> 

<span data-ttu-id="fe34d-108">Rendszerállapot-jelentések adja meg a fürt és az alkalmazás funkciói és keresztül állapotát jelző problémák láthatósága.</span><span class="sxs-lookup"><span data-stu-id="fe34d-108">System health reports provide visibility into cluster and application functionality and flag issues through health.</span></span> <span data-ttu-id="fe34d-109">Az alkalmazások és szolgáltatások rendszerállapot-jelentések győződjön meg arról, hogy az entitások vannak megvalósítva, és a Service Fabric szempontjából megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="fe34d-109">For applications and services, system health reports verify that entities are implemented and are behaving correctly from the Service Fabric perspective.</span></span> <span data-ttu-id="fe34d-110">A jelentések nem biztosít semmilyen állapotfigyelés az üzleti logika, a szolgáltatás vagy a lefagyott folyamatok észlelése.</span><span class="sxs-lookup"><span data-stu-id="fe34d-110">The reports do not provide any health monitoring of the business logic of the service or detection of hung processes.</span></span> <span data-ttu-id="fe34d-111">Felhasználó szolgáltatások állapotfigyelő üzemállapot-adatait a logika vonatkozó információkkal.</span><span class="sxs-lookup"><span data-stu-id="fe34d-111">User services can enrich the health data with information specific to their logic.</span></span>

> [!NOTE]
> <span data-ttu-id="fe34d-112">Watchdogs állapotjelentések láthatók csak *után* rendszerösszetevőn entitás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fe34d-112">Watchdogs health reports are visible only *after* the system components create an entity.</span></span> <span data-ttu-id="fe34d-113">A törölt egy entitás a health Store adatbázisban automatikusan törli a vele társított összes rendszerállapot-jelentések.</span><span class="sxs-lookup"><span data-stu-id="fe34d-113">When an entity is deleted, the health store automatically deletes all health reports associated with it.</span></span> <span data-ttu-id="fe34d-114">Ugyanez vonatkozik az entitás új példányának létrehozásakor (például egy új állapot-nyilvántartó megőrzött replika szolgáltatáspéldány jön létre).</span><span class="sxs-lookup"><span data-stu-id="fe34d-114">The same is true when a new instance of the entity is created (for example, a new stateful persisted service replica instance is created).</span></span> <span data-ttu-id="fe34d-115">A régi példányhoz társított összes jelentések törlése, és a-tárolóból törölni.</span><span class="sxs-lookup"><span data-stu-id="fe34d-115">All reports associated with the old instance are deleted and cleaned up from the store.</span></span>
> 
> 

<span data-ttu-id="fe34d-116">A rendszer az összetevő jelent azonosítják a forrás, amely kezdődik-e a "**System.**"</span><span class="sxs-lookup"><span data-stu-id="fe34d-116">The system component reports are identified by the source, which starts with the "**System.**"</span></span> <span data-ttu-id="fe34d-117">előtag.</span><span class="sxs-lookup"><span data-stu-id="fe34d-117">prefix.</span></span> <span data-ttu-id="fe34d-118">Watchdogs nem használhatja ugyanazt az előtagot az eseményforrásokat, érvénytelen paramétereket tartalmazó jelentések utasítja el.</span><span class="sxs-lookup"><span data-stu-id="fe34d-118">Watchdogs can't use the same prefix for their sources, as reports with invalid parameters are rejected.</span></span>
<span data-ttu-id="fe34d-119">Vizsgáljuk meg bizonyos rendszer jelentéseket megtudhatja, hogy mi elindítja azokat és a lehetséges problémákat jelölnek javítása.</span><span class="sxs-lookup"><span data-stu-id="fe34d-119">Let's look at some system reports to understand what triggers them and how to correct the possible issues they represent.</span></span>

> [!NOTE]
> <span data-ttu-id="fe34d-120">A Service Fabric továbbra is fennáll, a feltételek iránt, amelyek javítják a láthatósága, mi történik a fürt- és a jelentések hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="fe34d-120">Service Fabric continues to add reports on conditions of interest that improve visibility into what is happening in the cluster and application.</span></span> <span data-ttu-id="fe34d-121">Meglévő jelentések is fejleszthető a további részleteket a probléma gyorsabb megoldása érdekében.</span><span class="sxs-lookup"><span data-stu-id="fe34d-121">Existing reports can also be enhanced with more details to help troubleshoot the problem faster.</span></span>
> 
> 

## <a name="cluster-system-health-reports"></a><span data-ttu-id="fe34d-122">Fürt rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="fe34d-122">Cluster system health reports</span></span>
<span data-ttu-id="fe34d-123">A fürt állapotának entitás a health Store adatbázisban automatikusan jön létre.</span><span class="sxs-lookup"><span data-stu-id="fe34d-123">The cluster health entity is created automatically in the health store.</span></span> <span data-ttu-id="fe34d-124">Ha minden megfelelően működik, a rendszer a jelentés nem tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fe34d-124">If everything works properly, it doesn't have a system report.</span></span>

### <a name="neighborhood-loss"></a><span data-ttu-id="fe34d-125">Hálózatok adatvesztés</span><span class="sxs-lookup"><span data-stu-id="fe34d-125">Neighborhood loss</span></span>
<span data-ttu-id="fe34d-126">**System.Federation** egy hibát jelez, amikor azt észleli, hogy a helyek adatvesztés.</span><span class="sxs-lookup"><span data-stu-id="fe34d-126">**System.Federation** reports an error when it detects a neighborhood loss.</span></span> <span data-ttu-id="fe34d-127">A jelentés az egyes csomópontokon, és a csomópont-azonosító szerepel a tulajdonságnév.</span><span class="sxs-lookup"><span data-stu-id="fe34d-127">The report is from individual nodes, and the node ID is included in the property name.</span></span> <span data-ttu-id="fe34d-128">Ha egy alkalmazás a teljes Service Fabric gyűrű elvesznek, általában számíthat két esemény (mindkét oldalán, a résnek jelentés).</span><span class="sxs-lookup"><span data-stu-id="fe34d-128">If one neighborhood is lost in the entire Service Fabric ring, you can typically expect two events (both sides of the gap report).</span></span> <span data-ttu-id="fe34d-129">További környékeken elvesznek, ha nincsenek további események.</span><span class="sxs-lookup"><span data-stu-id="fe34d-129">If more neighborhoods are lost, there are more events.</span></span>

<span data-ttu-id="fe34d-130">A jelentés az élettartamnak adja meg a globális bérleti idejét.</span><span class="sxs-lookup"><span data-stu-id="fe34d-130">The report specifies the global lease timeout as the time to live.</span></span> <span data-ttu-id="fe34d-131">A jelentés minden fele a TTL időtartama Újraküldte mindaddig, amíg a feltétel aktív marad.</span><span class="sxs-lookup"><span data-stu-id="fe34d-131">The report is resent every half of the TTL duration for as long as the condition remains active.</span></span> <span data-ttu-id="fe34d-132">Az esemény lejáratot követően automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="fe34d-132">The event is automatically removed when it expires.</span></span> <span data-ttu-id="fe34d-133">Távolítsa el, ha lejárt a viselkedés garantálja az, hogy a jelentés törlődnek a a health Store adatbázisból megfelelően, még akkor is, ha a jelentéskészítési csomópont nem működik.</span><span class="sxs-lookup"><span data-stu-id="fe34d-133">Remove when expired behavior ensures that the report is cleaned up from the health store correctly, even if the reporting node is down.</span></span>

* <span data-ttu-id="fe34d-134">**SourceId**: System.Federation</span><span class="sxs-lookup"><span data-stu-id="fe34d-134">**SourceId**: System.Federation</span></span>
* <span data-ttu-id="fe34d-135">**Tulajdonság**: kezdődik **hálózatok** és csomópont adatai</span><span class="sxs-lookup"><span data-stu-id="fe34d-135">**Property**: Starts with **Neighborhood** and includes node information</span></span>
* <span data-ttu-id="fe34d-136">**További lépések**: vizsgálja meg, miért a helyek nem vesztek el (például ellenőrizze a fürt csomópontjai közötti kommunikáció).</span><span class="sxs-lookup"><span data-stu-id="fe34d-136">**Next steps**: Investigate why the neighborhood is lost (for example, check the communication between cluster nodes).</span></span>

## <a name="node-system-health-reports"></a><span data-ttu-id="fe34d-137">Csomópont rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="fe34d-137">Node system health reports</span></span>
<span data-ttu-id="fe34d-138">**System.FM**, a szolgáltatót, amely a fürtcsomópontokon kapcsolatos információt, amely jelenti, hogy a Feladatátvevőfürt-kezelő szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fe34d-138">**System.FM**, which represents the Failover Manager service, is the authority that manages information about cluster nodes.</span></span> <span data-ttu-id="fe34d-139">Minden csomópont állapotát megjelenítő System.FM egy jelentést kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="fe34d-139">Each node should have one report from System.FM showing its state.</span></span> <span data-ttu-id="fe34d-140">A csomópont entitások törlődnek, amikor a rendszer eltávolítja a csomópont állapota (lásd: [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span><span class="sxs-lookup"><span data-stu-id="fe34d-140">The node entities are removed when the node state is removed (see [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span></span>

### <a name="node-updown"></a><span data-ttu-id="fe34d-141">A csomópont fel/le</span><span class="sxs-lookup"><span data-stu-id="fe34d-141">Node up/down</span></span>
<span data-ttu-id="fe34d-142">Amikor a csomópont csatlakozik a (is működik, és) gyűrű a OK System.FM jelentés.</span><span class="sxs-lookup"><span data-stu-id="fe34d-142">System.FM reports as OK when the node joins the ring (it's up and running).</span></span> <span data-ttu-id="fe34d-143">Az egy hibát jelez, amikor a csomópont a gyűrűben indulását (szolgáltatás leállt, vagy frissítés, vagy egyszerűen mert sikertelen volt).</span><span class="sxs-lookup"><span data-stu-id="fe34d-143">It reports an error when the node departs the ring (it's down, either for upgrading or simply because it has failed).</span></span> <span data-ttu-id="fe34d-144">A rendszerállapot-hierarchia által a health Store adatbázisban a korrelációs System.FM csomópont jelentések a telepített entitások hajt végre műveletet.</span><span class="sxs-lookup"><span data-stu-id="fe34d-144">The health hierarchy built by the health store takes action on deployed entities in correlation with System.FM node reports.</span></span> <span data-ttu-id="fe34d-145">Úgy ítéli meg, a csomópont minden telepített entitás virtuális szülője.</span><span class="sxs-lookup"><span data-stu-id="fe34d-145">It considers the node a virtual parent of all deployed entities.</span></span> <span data-ttu-id="fe34d-146">Ezen a csomóponton telepített entitások közzétéve az lekérdezésekben, ha a csomópont állapotúként jelentette-e System.FM entitások társított példányt az azonos példánnyal rendelkező által.</span><span class="sxs-lookup"><span data-stu-id="fe34d-146">The deployed entities on that node are exposed through queries if the node is reported as up by System.FM, with the same instance as the instance associated with the entities.</span></span> <span data-ttu-id="fe34d-147">Amikor System.FM jelzi, hogy a csomópont nem működik, vagy újra (egy új példány), a health Store adatbázisban automatikusan megtisztítja a telepített entitás, amely csak a lefelé csomópontján vagy a csomópont előző példány létezhet.</span><span class="sxs-lookup"><span data-stu-id="fe34d-147">When System.FM reports that the node is down or restarted (a new instance), the health store automatically cleans up the deployed entities that can exist only on the down node or on the previous instance of the node.</span></span>

* <span data-ttu-id="fe34d-148">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="fe34d-148">**SourceId**: System.FM</span></span>
* <span data-ttu-id="fe34d-149">**Tulajdonság**: állapota</span><span class="sxs-lookup"><span data-stu-id="fe34d-149">**Property**: State</span></span>
* <span data-ttu-id="fe34d-150">**További lépések**: a csomópont frissítés szempontjából nem működik, ha kellene tartoznia hozzá biztonsági után frissítették.</span><span class="sxs-lookup"><span data-stu-id="fe34d-150">**Next steps**: If the node is down for an upgrade, it should come back up once it has been upgraded.</span></span> <span data-ttu-id="fe34d-151">Ebben az esetben állapotát kell váltson vissza az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="fe34d-151">In this case, the health state should switch back to OK.</span></span> <span data-ttu-id="fe34d-152">A csomópont nem térjen vissza, vagy nem sikerül, ha a probléma további vizsgálat van szüksége.</span><span class="sxs-lookup"><span data-stu-id="fe34d-152">If the node doesn't come back or it fails, the problem needs more investigation.</span></span>

<span data-ttu-id="fe34d-153">A következő példa bemutatja a csomópont OK állapotot System.FM esemény:</span><span class="sxs-lookup"><span data-stu-id="fe34d-153">The following example shows the System.FM event with a health state of OK for node up:</span></span>

```powershell
PS C:\> Get-ServiceFabricNodeHealth  _Node_0

NodeName              : _Node_0
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 8
                        SentAt                : 7/14/2017 4:54:51 PM
                        ReceivedAt            : 7/14/2017 4:55:14 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```


### <a name="certificate-expiration"></a><span data-ttu-id="fe34d-154">Tanúsítvány lejárta</span><span class="sxs-lookup"><span data-stu-id="fe34d-154">Certificate expiration</span></span>
<span data-ttu-id="fe34d-155">**System.FabricNode** jelentések figyelmeztetés, ha a csomópont által használt tanúsítványok lejáró.</span><span class="sxs-lookup"><span data-stu-id="fe34d-155">**System.FabricNode** reports a warning when certificates used by the node are near expiration.</span></span> <span data-ttu-id="fe34d-156">Három tanúsítvány csomópontonként: **Certificate_cluster**, **Certificate_server**, és **Certificate_default_client**.</span><span class="sxs-lookup"><span data-stu-id="fe34d-156">There are three certificates per node: **Certificate_cluster**, **Certificate_server**, and **Certificate_default_client**.</span></span> <span data-ttu-id="fe34d-157">Legalább két héttel a lejárati esetén a jelentés állapota rendben.</span><span class="sxs-lookup"><span data-stu-id="fe34d-157">When the expiration is at least two weeks away, the report health state is OK.</span></span> <span data-ttu-id="fe34d-158">A lejárati két héten belül van, a jelentés típusa esetén figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="fe34d-158">When the expiration is within two weeks, the report type is a warning.</span></span> <span data-ttu-id="fe34d-159">Ezek az események TTL végtelen, és amikor a csomópont elhagyja a fürt eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="fe34d-159">TTL of these events is infinite, and they are removed when a node leaves the cluster.</span></span>

* <span data-ttu-id="fe34d-160">**SourceId**: System.FabricNode</span><span class="sxs-lookup"><span data-stu-id="fe34d-160">**SourceId**: System.FabricNode</span></span>
* <span data-ttu-id="fe34d-161">**Tulajdonság**: kezdődik **tanúsítvány** és a tanúsítvány típusát további információt tartalmaz</span><span class="sxs-lookup"><span data-stu-id="fe34d-161">**Property**: Starts with **Certificate** and contains more information about the certificate type</span></span>
* <span data-ttu-id="fe34d-162">**További lépések**: frissítse a tanúsítványokat, ha azok lejáró.</span><span class="sxs-lookup"><span data-stu-id="fe34d-162">**Next steps**: Update the certificates if they are near expiration.</span></span>

### <a name="load-capacity-violation"></a><span data-ttu-id="fe34d-163">Betöltési kapacitás megsértése</span><span class="sxs-lookup"><span data-stu-id="fe34d-163">Load capacity violation</span></span>
<span data-ttu-id="fe34d-164">A Service Fabric terheléselosztó hiányára amikor azt észleli, hogy a csomópont-kapacitás megsértése.</span><span class="sxs-lookup"><span data-stu-id="fe34d-164">The Service Fabric Load Balancer reports a warning when it detects a node capacity violation.</span></span>

* <span data-ttu-id="fe34d-165">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="fe34d-165">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="fe34d-166">**Tulajdonság**: kezdődik **kapacitás**</span><span class="sxs-lookup"><span data-stu-id="fe34d-166">**Property**: Starts with **Capacity**</span></span>
* <span data-ttu-id="fe34d-167">**További lépések**: biztosított metrikákat és olvassa el az aktuális kapacitása a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="fe34d-167">**Next steps**: Check provided metrics and view the current capacity on the node.</span></span>

## <a name="application-system-health-reports"></a><span data-ttu-id="fe34d-168">Alkalmazás rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="fe34d-168">Application system health reports</span></span>
<span data-ttu-id="fe34d-169">**System.CM**, amely jelenti, hogy a kezelő szolgáltatás a szolgáltató, amely felügyeli az alkalmazással kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="fe34d-169">**System.CM**, which represents the Cluster Manager service, is the authority that manages information about an application.</span></span>

### <a name="state"></a><span data-ttu-id="fe34d-170">Állapot</span><span class="sxs-lookup"><span data-stu-id="fe34d-170">State</span></span>
<span data-ttu-id="fe34d-171">System.CM jelzi az OK gombra az alkalmazás létrehozásakor vagy frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="fe34d-171">System.CM reports as OK when the application has been created or updated.</span></span> <span data-ttu-id="fe34d-172">Az tájékoztatja a health Store adatbázisban az alkalmazás törlésekor, hogy az áruházból el kell távolítani.</span><span class="sxs-lookup"><span data-stu-id="fe34d-172">It informs the health store when the application has been deleted, so that it can be removed from store.</span></span>

* <span data-ttu-id="fe34d-173">**SourceId**: System.CM</span><span class="sxs-lookup"><span data-stu-id="fe34d-173">**SourceId**: System.CM</span></span>
* <span data-ttu-id="fe34d-174">**Tulajdonság**: állapota</span><span class="sxs-lookup"><span data-stu-id="fe34d-174">**Property**: State</span></span>
* <span data-ttu-id="fe34d-175">**További lépések**: Ha az alkalmazás létrehozott vagy frissített, a kezelő állapotjelentése tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="fe34d-175">**Next steps**: If the application has been created or updated, it should include the Cluster Manager health report.</span></span> <span data-ttu-id="fe34d-176">Ellenkező esetben ellenőrizze a lekérdezés kiállításával az alkalmazás állapota (például a PowerShell-parancsmag **Get-ServiceFabricApplication - ApplicationName *applicationName***).</span><span class="sxs-lookup"><span data-stu-id="fe34d-176">Otherwise, check the state of the application by issuing a query (for example, the PowerShell cmdlet **Get-ServiceFabricApplication -ApplicationName *applicationName***).</span></span>

<span data-ttu-id="fe34d-177">Az alábbi példában látható az esemény a **fabric: / WordCount** alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="fe34d-177">The following example shows the state event on the **fabric:/WordCount** application:</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/14/2017 4:55:10 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="service-system-health-reports"></a><span data-ttu-id="fe34d-178">Szolgáltatás rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="fe34d-178">Service system health reports</span></span>
<span data-ttu-id="fe34d-179">**System.FM**, amely jelenti, hogy a Feladatátvevőfürt-kezelő szolgáltatás, amely kezeli a szolgáltatással kapcsolatban további információt a szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="fe34d-179">**System.FM**, which represents the Failover Manager service, is the authority that manages information about services.</span></span>

### <a name="state"></a><span data-ttu-id="fe34d-180">Állapot</span><span class="sxs-lookup"><span data-stu-id="fe34d-180">State</span></span>
<span data-ttu-id="fe34d-181">System.FM jelzi az OK gombra a szolgáltatás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="fe34d-181">System.FM reports as OK when the service has been created.</span></span> <span data-ttu-id="fe34d-182">Ez törli az entitás a health Store adatbázisban a szolgáltatás törlésekor.</span><span class="sxs-lookup"><span data-stu-id="fe34d-182">It deletes the entity from the health store when the service has been deleted.</span></span>

* <span data-ttu-id="fe34d-183">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="fe34d-183">**SourceId**: System.FM</span></span>
* <span data-ttu-id="fe34d-184">**Tulajdonság**: állapota</span><span class="sxs-lookup"><span data-stu-id="fe34d-184">**Property**: State</span></span>

<span data-ttu-id="fe34d-185">A következő példa bemutatja az esemény a szolgáltatás **fabric: / WordCount/wordcountwebservice verziója**:</span><span class="sxs-lookup"><span data-stu-id="fe34d-185">The following example shows the state event on the service **fabric:/WordCount/WordCountWebService**:</span></span>

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountWebService -ExcludeHealthStatistics


ServiceName           : fabric:/WordCount/WordCountWebService
AggregatedHealthState : Ok
PartitionHealthStates : 
                        PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
                        AggregatedHealthState : Ok
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 14
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="service-correlation-error"></a><span data-ttu-id="fe34d-186">Korrelációs hibája</span><span class="sxs-lookup"><span data-stu-id="fe34d-186">Service correlation error</span></span>
<span data-ttu-id="fe34d-187">**System.PLB** egy hibát jelez, amikor azt észleli, hogy egy szolgáltatás, amellyel egyeztetés szükséges egy másik szolgáltatás frissítése létrehoz egy affinitási láncot.</span><span class="sxs-lookup"><span data-stu-id="fe34d-187">**System.PLB** reports an error when it detects that updating a service to be correlated with another service creates an affinity chain.</span></span> <span data-ttu-id="fe34d-188">A jelentés sikeres frissítés történik, ha nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="fe34d-188">The report is cleared when successful update happens.</span></span>

* <span data-ttu-id="fe34d-189">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="fe34d-189">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="fe34d-190">**Tulajdonság**: ServiceDescription objektumot</span><span class="sxs-lookup"><span data-stu-id="fe34d-190">**Property**: ServiceDescription</span></span>
* <span data-ttu-id="fe34d-191">**További lépések**: Ellenőrizze a kapcsolódó szolgáltatások ismertetése.</span><span class="sxs-lookup"><span data-stu-id="fe34d-191">**Next steps**: Check the correlated service descriptions.</span></span>

## <a name="partition-system-health-reports"></a><span data-ttu-id="fe34d-192">Partíció rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="fe34d-192">Partition system health reports</span></span>
<span data-ttu-id="fe34d-193">**System.FM**, a szolgáltatót, amely a szolgáltatáspartíciók kapcsolatos információt, amely jelenti, hogy a Feladatátvevőfürt-kezelő szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fe34d-193">**System.FM**, which represents the Failover Manager service, is the authority that manages information about service partitions.</span></span>

### <a name="state"></a><span data-ttu-id="fe34d-194">Állapot</span><span class="sxs-lookup"><span data-stu-id="fe34d-194">State</span></span>
<span data-ttu-id="fe34d-195">System.FM jelzi az OK gombra, ha a partíció létrejött, és megfelelő állapotban.</span><span class="sxs-lookup"><span data-stu-id="fe34d-195">System.FM reports as OK when the partition has been created and is healthy.</span></span> <span data-ttu-id="fe34d-196">Ez törli az entitás a health Store adatbázisban a partíció törlése.</span><span class="sxs-lookup"><span data-stu-id="fe34d-196">It deletes the entity from the health store when the partition is deleted.</span></span>

<span data-ttu-id="fe34d-197">A partíció nem éri el a replikakészlet minimális száma, ha hiba jelenti.</span><span class="sxs-lookup"><span data-stu-id="fe34d-197">If the partition is below the minimum replica count, it reports an error.</span></span> <span data-ttu-id="fe34d-198">Ha a partíció nincs kevesebb a minimális replika, de kevesebb a cél replika, a rendszer figyelmeztetést jelenti.</span><span class="sxs-lookup"><span data-stu-id="fe34d-198">If the partition is not below the minimum replica count, but it is below the target replica count, it reports a warning.</span></span> <span data-ttu-id="fe34d-199">Ha a partíció kvórumveszteségben van, System.FM jelent hibát.</span><span class="sxs-lookup"><span data-stu-id="fe34d-199">If the partition is in quorum loss, System.FM reports an error.</span></span>

<span data-ttu-id="fe34d-200">Egyéb fontos események tartalmazhat egy figyelmeztetés, ha az újrakonfigurálás hosszabb időt vesz igénybe a vártnál, és amikor a vártnál hosszabb ideig tart a build.</span><span class="sxs-lookup"><span data-stu-id="fe34d-200">Other important events include a warning when the reconfiguration takes longer than expected and when the build takes longer than expected.</span></span> <span data-ttu-id="fe34d-201">A várt a build és újrakonfigurálását konfigurálható: a szolgáltatás forgatókönyvek alapján alkalommal.</span><span class="sxs-lookup"><span data-stu-id="fe34d-201">The expected times for the build and reconfiguration are configurable based on service scenarios.</span></span> <span data-ttu-id="fe34d-202">Például egy terabájt állapot, például az SQL-adatbázis, a szolgáltatás-e a build tovább tart, mint egy szolgáltatás állapotát kis mennyiségű számára.</span><span class="sxs-lookup"><span data-stu-id="fe34d-202">For example, if a service has a terabyte of state, such as SQL Database, the build takes longer than for a service with a small amount of state.</span></span>

* <span data-ttu-id="fe34d-203">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="fe34d-203">**SourceId**: System.FM</span></span>
* <span data-ttu-id="fe34d-204">**Tulajdonság**: állapota</span><span class="sxs-lookup"><span data-stu-id="fe34d-204">**Property**: State</span></span>
* <span data-ttu-id="fe34d-205">**További lépések**: Ha a rendszerállapot nem OK, úgy is, hogy egyes másodpéldányok nem létrehozott, megnyitott, vagy a tartományvezérlővé való előléptetés elsődleges vagy másodlagos megfelelően.</span><span class="sxs-lookup"><span data-stu-id="fe34d-205">**Next steps**: If the health state is not OK, it's possible that some replicas have not been created, opened, or promoted to primary or secondary correctly.</span></span> <span data-ttu-id="fe34d-206">Sok esetben okozza-e a Megnyitás vagy szerepkör módosítása végrehajtása szolgáltatási hiba.</span><span class="sxs-lookup"><span data-stu-id="fe34d-206">In many instances, the root cause is a service bug in the open or change-role implementation.</span></span>

<span data-ttu-id="fe34d-207">A következő példa bemutatja a megfelelő partíció:</span><span class="sxs-lookup"><span data-stu-id="fe34d-207">The following example shows a healthy partition:</span></span>

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountWebService | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics -ReplicasFilter None

PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 70
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

<span data-ttu-id="fe34d-208">A következő példa bemutatja, javasoljuk, hogy kevesebb cél replika állapotát.</span><span class="sxs-lookup"><span data-stu-id="fe34d-208">The following example shows the health of a partition that is below target replica count.</span></span> <span data-ttu-id="fe34d-209">A következő lépés az, hogy beolvasása a partíció leírása, amely bemutatja, hogyan van konfigurálva: **MinReplicaSetSize** három és **TargetReplicaSetSize** hét.</span><span class="sxs-lookup"><span data-stu-id="fe34d-209">The next step is to get the partition description, which shows how it is configured: **MinReplicaSetSize** is three and **TargetReplicaSetSize** is seven.</span></span> <span data-ttu-id="fe34d-210">Majd kérje le a fürt a csomópontok számát: öt.</span><span class="sxs-lookup"><span data-stu-id="fe34d-210">Then get the number of nodes in the cluster: five.</span></span> <span data-ttu-id="fe34d-211">Ebben az esetben két replika nem kell elhelyezni, mert a cél replikák száma nagyobb, mint az elérhető csomópontok számát.</span><span class="sxs-lookup"><span data-stu-id="fe34d-211">So in this case, two replicas can't be placed because the target number of replicas is higher than the number of nodes available.</span></span>

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None -ExcludeHealthStatistics


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 123
                        SentAt                : 7/14/2017 4:55:39 PM
                        ReceivedAt            : 7/14/2017 4:55:44 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/S RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/P RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:55:44 PM, LastOk = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131445250939703027
                        SentAt                : 7/14/2017 4:58:13 PM
                        ReceivedAt            : 7/14/2017 4:58:14 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
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
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:56:14 PM, LastOk = 1/1/0001 12:00:00 AM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | select MinReplicaSetSize,TargetReplicaSetSize

MinReplicaSetSize TargetReplicaSetSize
----------------- --------------------
                2                    7                        

PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a><span data-ttu-id="fe34d-212">A replika korlátozássértést</span><span class="sxs-lookup"><span data-stu-id="fe34d-212">Replica constraint violation</span></span>
<span data-ttu-id="fe34d-213">**System.PLB** jelentések figyelmeztetés, ha egy replika korlátozássértést észlelt, és nem helyezhető el az összes partíció replika.</span><span class="sxs-lookup"><span data-stu-id="fe34d-213">**System.PLB** reports a warning if it detects a replica constraint violation and can't place all partition replicas.</span></span> <span data-ttu-id="fe34d-214">A jelentés-részletek megjelenítése milyen korlátozások és tulajdonságok megakadályozza a replika elhelyezésének.</span><span class="sxs-lookup"><span data-stu-id="fe34d-214">The report details show which constraints and properties prevent the replica placement.</span></span>

* <span data-ttu-id="fe34d-215">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="fe34d-215">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="fe34d-216">**Tulajdonság**: kezdődik **ReplicaConstraintViolation**</span><span class="sxs-lookup"><span data-stu-id="fe34d-216">**Property**: Starts with **ReplicaConstraintViolation**</span></span>

## <a name="replica-system-health-reports"></a><span data-ttu-id="fe34d-217">A replika rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="fe34d-217">Replica system health reports</span></span>
<span data-ttu-id="fe34d-218">**System.RA**, amely jelenti, hogy a reconfiguration agent összetevő a replika állapota az szolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="fe34d-218">**System.RA**, which represents the reconfiguration agent component, is the authority for the replica state.</span></span>

### <a name="state"></a><span data-ttu-id="fe34d-219">Állapot</span><span class="sxs-lookup"><span data-stu-id="fe34d-219">State</span></span>
<span data-ttu-id="fe34d-220">**System.RA** jelentések OK, a replika létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="fe34d-220">**System.RA** reports OK when the replica has been created.</span></span>

* <span data-ttu-id="fe34d-221">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="fe34d-221">**SourceId**: System.RA</span></span>
* <span data-ttu-id="fe34d-222">**Tulajdonság**: állapota</span><span class="sxs-lookup"><span data-stu-id="fe34d-222">**Property**: State</span></span>

<span data-ttu-id="fe34d-223">A következő példa bemutatja a megfelelő replika:</span><span class="sxs-lookup"><span data-stu-id="fe34d-223">The following example shows a healthy replica:</span></span>

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422293118721
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131445248920273536
                        SentAt                : 7/14/2017 4:54:52 PM
                        ReceivedAt            : 7/14/2017 4:55:13 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_0
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:13 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="replica-open-status"></a><span data-ttu-id="fe34d-224">Nyissa meg a replika állapota</span><span class="sxs-lookup"><span data-stu-id="fe34d-224">Replica open status</span></span>
<span data-ttu-id="fe34d-225">A jelentés leírása a kezdési idő (egyezményes világidő) az API-hívás metódus meghívásakor.</span><span class="sxs-lookup"><span data-stu-id="fe34d-225">The description of this health report contains the start time (Coordinated Universal Time) when the API call was invoked.</span></span>

<span data-ttu-id="fe34d-226">**System.RA** jelenti a figyelmeztetést, ha a replika megnyitása tovább tart, mint a beállított időtartam (alapértelmezés: 30 perc).</span><span class="sxs-lookup"><span data-stu-id="fe34d-226">**System.RA** reports a warning if the replica open takes longer than the configured period (default: 30 minutes).</span></span> <span data-ttu-id="fe34d-227">Ha az API-t a szolgáltatás rendelkezésre állása befolyásolja, a jelentés kiadott sokkal gyorsabb (egy konfigurálható időszakban, az alapértelmezett érték 30 másodperc).</span><span class="sxs-lookup"><span data-stu-id="fe34d-227">If the API impacts service availability, the report is issued much faster (a configurable interval, with a default of 30 seconds).</span></span> <span data-ttu-id="fe34d-228">Mért idő magában foglalja a replikátor nyitó és a szolgáltatás megnyitása igénybe vett idő.</span><span class="sxs-lookup"><span data-stu-id="fe34d-228">The time measured includes the time taken for the replicator open and the service open.</span></span> <span data-ttu-id="fe34d-229">Ha a megnyitási elvégzi a OK vált a tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fe34d-229">The property changes to OK if the open completes.</span></span>

* <span data-ttu-id="fe34d-230">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="fe34d-230">**SourceId**: System.RA</span></span>
* <span data-ttu-id="fe34d-231">**Tulajdonság**: **ReplicaOpenStatus**</span><span class="sxs-lookup"><span data-stu-id="fe34d-231">**Property**: **ReplicaOpenStatus**</span></span>
* <span data-ttu-id="fe34d-232">**További lépések**: Ha a rendszerállapot nem OK, vizsgálja meg, miért nyissa meg a replikát a vártnál hosszabb időbe telik.</span><span class="sxs-lookup"><span data-stu-id="fe34d-232">**Next steps**: If the health state is not OK, investigate why the replica open takes longer than expected.</span></span>

### <a name="slow-service-api-call"></a><span data-ttu-id="fe34d-233">Lassú service API-hívás</span><span class="sxs-lookup"><span data-stu-id="fe34d-233">Slow service API call</span></span>
<span data-ttu-id="fe34d-234">**System.RAP** és **System.Replicator** jelentést egy figyelmeztetés, ha a szolgáltatás felhasználói kód hívása tovább tart, mint a beállított időn.</span><span class="sxs-lookup"><span data-stu-id="fe34d-234">**System.RAP** and **System.Replicator** report a warning if a call to the user service code takes longer than the configured time.</span></span> <span data-ttu-id="fe34d-235">A figyelmeztetés nincs bejelölve, a hívás befejezését.</span><span class="sxs-lookup"><span data-stu-id="fe34d-235">The warning is cleared when the call completes.</span></span>

* <span data-ttu-id="fe34d-236">**SourceId**: System.RAP vagy System.Replicator</span><span class="sxs-lookup"><span data-stu-id="fe34d-236">**SourceId**: System.RAP or System.Replicator</span></span>
* <span data-ttu-id="fe34d-237">**Tulajdonság**: a lassú API neve.</span><span class="sxs-lookup"><span data-stu-id="fe34d-237">**Property**: The name of the slow API.</span></span> <span data-ttu-id="fe34d-238">A leírás nyújt további információt az idő, az API-t a függőben lévő lett.</span><span class="sxs-lookup"><span data-stu-id="fe34d-238">The description provides more details about the time the API has been pending.</span></span>
* <span data-ttu-id="fe34d-239">**További lépések**: vizsgálja meg, miért vártnál hosszabb ideig tart a hívást.</span><span class="sxs-lookup"><span data-stu-id="fe34d-239">**Next steps**: Investigate why the call takes longer than expected.</span></span>

<span data-ttu-id="fe34d-240">Az alábbi példában egy partíciót a kvórum elvesztése, és a vizsgálat lépéseket annak mérje fel, hogy miért történik.</span><span class="sxs-lookup"><span data-stu-id="fe34d-240">The following example shows a partition in quorum loss, and the investigation steps done to figure out why.</span></span> <span data-ttu-id="fe34d-241">A replikák egyike figyelmeztetési állapot, hogy az állapotát.</span><span class="sxs-lookup"><span data-stu-id="fe34d-241">One of the replicas has a warning health state, so you get its health.</span></span> <span data-ttu-id="fe34d-242">Azt mutatja, hogy a szolgáltatási művelet hosszabb időt vesz igénybe, mint a System.RAP által jelentett eseményt várt.</span><span class="sxs-lookup"><span data-stu-id="fe34d-242">It shows that the service operation takes longer than expected, an event reported by System.RAP.</span></span> <span data-ttu-id="fe34d-243">Ezek az információk fogadását követően a következő lépéssel nézze meg a szolgáltatáskód hibáit, és vizsgálja meg a hiba.</span><span class="sxs-lookup"><span data-stu-id="fe34d-243">After this information is received, the next step is to look at the service code and investigate there.</span></span> <span data-ttu-id="fe34d-244">Ebben az esetben a **RunAsync** az állapotalapú szolgáltatásból végrehajtása nem kezelt kivételt jelez.</span><span class="sxs-lookup"><span data-stu-id="fe34d-244">For this case, the **RunAsync** implementation of the stateful service throws an unhandled exception.</span></span> <span data-ttu-id="fe34d-245">A replikák vannak újrahasznosítása, ezért nem láthatók a figyelmeztetési állapot a replikákat.</span><span class="sxs-lookup"><span data-stu-id="fe34d-245">The replicas are recycling, so you may not see any replicas in the warning state.</span></span> <span data-ttu-id="fe34d-246">Ismételje meg a kezdeti állapotát, és keresse meg a másodpéldány-azonosító az eltérések</span><span class="sxs-lookup"><span data-stu-id="fe34d-246">You can retry getting the health state and look for any differences in the replica ID.</span></span> <span data-ttu-id="fe34d-247">Bizonyos esetekben a próbálkozások tudhatja meg keresik.</span><span class="sxs-lookup"><span data-stu-id="fe34d-247">In certain cases, the retries can give you clues.</span></span>

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 3
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

<span data-ttu-id="fe34d-248">A hibás alkalmazást a hibakereső indításakor a diagnosztikai események windows megjelenítése RunAsync a kivétel:</span><span class="sxs-lookup"><span data-stu-id="fe34d-248">When you start the faulty application under the debugger, the diagnostic events windows show the exception thrown from RunAsync:</span></span>

![A Visual Studio 2015 diagnosztikai események: RunAsync hiba a fabric: / HelloWorldStatefulApplication.][1]

<span data-ttu-id="fe34d-250">A Visual Studio 2015 diagnosztikai események: RunAsync hiba történt a **fabric: / HelloWorldStatefulApplication**.</span><span class="sxs-lookup"><span data-stu-id="fe34d-250">Visual Studio 2015 diagnostic events: RunAsync failure in **fabric:/HelloWorldStatefulApplication**.</span></span>

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a><span data-ttu-id="fe34d-251">Replikációs sor megtelt</span><span class="sxs-lookup"><span data-stu-id="fe34d-251">Replication queue full</span></span>
<span data-ttu-id="fe34d-252">**System.Replicator** jelentések figyelmeztetés, ha a replikációs sor megtelt.</span><span class="sxs-lookup"><span data-stu-id="fe34d-252">**System.Replicator** reports a warning when the replication queue is full.</span></span> <span data-ttu-id="fe34d-253">Az elsődleges replikációs várólista általában megtelik, mert egy vagy több másodlagos replikák lassúak múlva nyugtázza a műveletek.</span><span class="sxs-lookup"><span data-stu-id="fe34d-253">On the primary, replication queue usually becomes full because one or more secondary replicas are slow to acknowledge operations.</span></span> <span data-ttu-id="fe34d-254">A másodlagos Ez általában történik, ha a szolgáltatás lassú műveletek alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="fe34d-254">On the secondary, this usually happens when the service is slow to apply the operations.</span></span> <span data-ttu-id="fe34d-255">A figyelmeztetés nincs bejelölve, ha a várólista már nem teljes.</span><span class="sxs-lookup"><span data-stu-id="fe34d-255">The warning is cleared when the queue is no longer full.</span></span>

* <span data-ttu-id="fe34d-256">**SourceId**: System.Replicator</span><span class="sxs-lookup"><span data-stu-id="fe34d-256">**SourceId**: System.Replicator</span></span>
* <span data-ttu-id="fe34d-257">**Tulajdonság**: **PrimaryReplicationQueueStatus** vagy **SecondaryReplicationQueueStatus**, attól függően, a replika szerepkör</span><span class="sxs-lookup"><span data-stu-id="fe34d-257">**Property**: **PrimaryReplicationQueueStatus** or **SecondaryReplicationQueueStatus**, depending on the replica role</span></span>

### <a name="slow-naming-operations"></a><span data-ttu-id="fe34d-258">Lassú elnevezési műveletek</span><span class="sxs-lookup"><span data-stu-id="fe34d-258">Slow Naming operations</span></span>
<span data-ttu-id="fe34d-259">**System.NamingService** jelentések állapotát az elsődleges replikán, amikor egy elnevezési művelet elfogadható hosszabb időbe telik.</span><span class="sxs-lookup"><span data-stu-id="fe34d-259">**System.NamingService** reports health on its primary replica when a Naming operation takes longer than acceptable.</span></span> <span data-ttu-id="fe34d-260">Példák elnevezési műveletek [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) vagy [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span><span class="sxs-lookup"><span data-stu-id="fe34d-260">Examples of Naming operations are [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) or [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span></span> <span data-ttu-id="fe34d-261">Több metódust alatt található FabricClient, például a [módszereket szolgáltatás](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) vagy [tulajdonság módszereket](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span><span class="sxs-lookup"><span data-stu-id="fe34d-261">More methods can be found under FabricClient, for example under [service management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) or [property management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span></span>

> [!NOTE]
> <span data-ttu-id="fe34d-262">A Naming service szolgáltatásnevek megszünteti a fürt helyre, és lehetővé teszi a felhasználók kezeléséhez a szolgáltatás nevét és tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="fe34d-262">The Naming service resolves service names to a location in the cluster and enables users to manage service names and properties.</span></span> <span data-ttu-id="fe34d-263">A Service Fabric particionálva szolgáltatás megőrzött.</span><span class="sxs-lookup"><span data-stu-id="fe34d-263">It is a Service Fabric partitioned persisted service.</span></span> <span data-ttu-id="fe34d-264">A Authority Owner, amely tartalmazza az összes Service Fabric-neveket és szolgáltatásokra vonatkozó metaadatok egyik partíciója jelöli.</span><span class="sxs-lookup"><span data-stu-id="fe34d-264">One of the partitions represents the Authority Owner, which contains metadata about all Service Fabric names and services.</span></span> <span data-ttu-id="fe34d-265">A Service Fabric-nevek különböző partíciók nevű Name Owner partíciók, így a szolgáltatás nem bővíthető van leképezve.</span><span class="sxs-lookup"><span data-stu-id="fe34d-265">The Service Fabric names are mapped to different partitions, called Name Owner partitions, so the service is extensible.</span></span> <span data-ttu-id="fe34d-266">Tudjon meg többet az [Naming service](service-fabric-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="fe34d-266">Read more about [Naming service](service-fabric-architecture.md).</span></span>
> 
> 

<span data-ttu-id="fe34d-267">Ha egy elnevezési művelet a vártnál hosszabb időt vesz igénybe, a művelet megjelölt figyelmeztetés jelentést a a *a Naming service partíció a művelet ellátó elsődleges replika*.</span><span class="sxs-lookup"><span data-stu-id="fe34d-267">When a Naming operation takes longer than expected, the operation is flagged with a Warning report on the *primary replica of the Naming service partition that serves the operation*.</span></span> <span data-ttu-id="fe34d-268">Ha a művelet sikeresen befejeződött, a figyelmeztetés nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="fe34d-268">If the operation completes successfully, the Warning is cleared.</span></span> <span data-ttu-id="fe34d-269">Ha a művelet egy hibával befejeződött, a jelentés tartalmazza a hiba részleteit.</span><span class="sxs-lookup"><span data-stu-id="fe34d-269">If the operation completes with an error, the health report includes details about the error.</span></span>

* <span data-ttu-id="fe34d-270">**SourceId**: System.NamingService</span><span class="sxs-lookup"><span data-stu-id="fe34d-270">**SourceId**: System.NamingService</span></span>
* <span data-ttu-id="fe34d-271">**Tulajdonság**: előtaggal kezdődik **Duration_** és azonosítja a lassú művelet és a Service Fabric nevét, amelyen a művelet történik.</span><span class="sxs-lookup"><span data-stu-id="fe34d-271">**Property**: Starts with prefix **Duration_** and identifies the slow operation and the Service Fabric name on which the operation is applied.</span></span> <span data-ttu-id="fe34d-272">Például ha a szolgáltatás létrehozása a háló nevét: / MyApp/MyService túl sokáig tart, Duration_AOCreateService.fabric:/MyApp/MyService tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fe34d-272">For example, if create service at name fabric:/MyApp/MyService takes too long, the property is Duration_AOCreateService.fabric:/MyApp/MyService.</span></span> <span data-ttu-id="fe34d-273">AO szerepe a Naming ezt a nevet és a művelet a mutat.</span><span class="sxs-lookup"><span data-stu-id="fe34d-273">AO points to the role of the Naming partition for this name and operation.</span></span>
* <span data-ttu-id="fe34d-274">**További lépések**: Miért nem sikerül a Naming művelet ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="fe34d-274">**Next steps**: Check why the Naming operation fails.</span></span> <span data-ttu-id="fe34d-275">Minden műveletet lehet különböző alapvető okait.</span><span class="sxs-lookup"><span data-stu-id="fe34d-275">Each operation can have different root causes.</span></span> <span data-ttu-id="fe34d-276">Például törlése egy csomópont elakadt előfordulhat, hogy a szolgáltatás, mert alkalmazásgazda tartja összeomló szolgáltatáskód felhasználói hibája miatt a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="fe34d-276">For example, delete service may be stuck on a node because the application host keeps crashing on a node due to a user bug in the service code.</span></span>

<span data-ttu-id="fe34d-277">A következő példa bemutatja a szolgáltatás létrehozási művelet.</span><span class="sxs-lookup"><span data-stu-id="fe34d-277">The following example shows a create service operation.</span></span> <span data-ttu-id="fe34d-278">A művelet a megadott időtartamnál hosszabb ideig tartott.</span><span class="sxs-lookup"><span data-stu-id="fe34d-278">The operation took longer than the configured duration.</span></span> <span data-ttu-id="fe34d-279">AO újrapróbálkozik, és elküldi a munkahelyi nem.</span><span class="sxs-lookup"><span data-stu-id="fe34d-279">AO retries and sends work to NO.</span></span> <span data-ttu-id="fe34d-280">NEM az utolsó művelet időtúllépéssel fejeződött be.</span><span class="sxs-lookup"><span data-stu-id="fe34d-280">NO completed the last operation with Timeout.</span></span> <span data-ttu-id="fe34d-281">Ebben az esetben a azonos replika nem elsődleges, mind a AO, és nincs szerepkör.</span><span class="sxs-lookup"><span data-stu-id="fe34d-281">In this case, the same replica is primary for both the AO and NO roles.</span></span>

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a><span data-ttu-id="fe34d-282">DeployedApplication rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="fe34d-282">DeployedApplication system health reports</span></span>
<span data-ttu-id="fe34d-283">**System.Hosting** a szervezet az üzembe helyezett entitásokra.</span><span class="sxs-lookup"><span data-stu-id="fe34d-283">**System.Hosting** is the authority on deployed entities.</span></span>

### <a name="activation"></a><span data-ttu-id="fe34d-284">az aktiválás</span><span class="sxs-lookup"><span data-stu-id="fe34d-284">Activation</span></span>
<span data-ttu-id="fe34d-285">System.Hosting jelzi az OK gombra, ha egy alkalmazás aktiválása megtörtént a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="fe34d-285">System.Hosting reports as OK when an application has been successfully activated on the node.</span></span> <span data-ttu-id="fe34d-286">Ellenkező esetben azt egy hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="fe34d-286">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="fe34d-287">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="fe34d-287">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="fe34d-288">**Tulajdonság**: aktiválási, beleértve a bevezetés verzióját</span><span class="sxs-lookup"><span data-stu-id="fe34d-288">**Property**: Activation, including the rollout version</span></span>
* <span data-ttu-id="fe34d-289">**További lépések**: Ha az alkalmazás állapota nem megfelelő, vizsgálja meg, miért nem sikerült az aktiválást.</span><span class="sxs-lookup"><span data-stu-id="fe34d-289">**Next steps**: If the application is unhealthy, investigate why the activation failed.</span></span>

<span data-ttu-id="fe34d-290">A következő példa bemutatja a sikeres aktiválási:</span><span class="sxs-lookup"><span data-stu-id="fe34d-290">The following example shows successful activation:</span></span>

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ExcludeHealthStatistics

ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_1
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131445249083836329
                                     SentAt                : 7/14/2017 4:55:08 PM
                                     ReceivedAt            : 7/14/2017 4:55:14 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a><span data-ttu-id="fe34d-291">Letöltés</span><span class="sxs-lookup"><span data-stu-id="fe34d-291">Download</span></span>
<span data-ttu-id="fe34d-292">**System.Hosting** egy hibát jelez, ha az alkalmazás csomag letöltése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="fe34d-292">**System.Hosting** reports an error if the application package download fails.</span></span>

* <span data-ttu-id="fe34d-293">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="fe34d-293">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="fe34d-294">**Tulajdonság**:  **letöltése:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="fe34d-294">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="fe34d-295">**További lépések**: vizsgálja meg, miért nem sikerült a letöltés a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="fe34d-295">**Next steps**: Investigate why the download failed on the node.</span></span>

## <a name="deployedservicepackage-system-health-reports"></a><span data-ttu-id="fe34d-296">DeployedServicePackage rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="fe34d-296">DeployedServicePackage system health reports</span></span>
<span data-ttu-id="fe34d-297">**System.Hosting** a szervezet az üzembe helyezett entitásokra.</span><span class="sxs-lookup"><span data-stu-id="fe34d-297">**System.Hosting** is the authority on deployed entities.</span></span>

### <a name="service-package-activation"></a><span data-ttu-id="fe34d-298">Szolgáltatás az alkalmazáscsomag aktiválása</span><span class="sxs-lookup"><span data-stu-id="fe34d-298">Service package activation</span></span>
<span data-ttu-id="fe34d-299">System.Hosting jelentések OK gombra, ha a csomóponton a szolgáltatás az alkalmazáscsomag aktiválása sikeres.</span><span class="sxs-lookup"><span data-stu-id="fe34d-299">System.Hosting reports as OK if the service package activation on the node is successful.</span></span> <span data-ttu-id="fe34d-300">Ellenkező esetben azt egy hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="fe34d-300">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="fe34d-301">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="fe34d-301">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="fe34d-302">**Tulajdonság**: aktiválás</span><span class="sxs-lookup"><span data-stu-id="fe34d-302">**Property**: Activation</span></span>
* <span data-ttu-id="fe34d-303">**További lépések**: vizsgálja meg, miért nem sikerült az aktiválást.</span><span class="sxs-lookup"><span data-stu-id="fe34d-303">**Next steps**: Investigate why the activation failed.</span></span>

### <a name="code-package-activation"></a><span data-ttu-id="fe34d-304">Kód az alkalmazáscsomag aktiválása</span><span class="sxs-lookup"><span data-stu-id="fe34d-304">Code package activation</span></span>
<span data-ttu-id="fe34d-305">**System.Hosting** Ha az aktiválás sikeres kód csomagonként OK jelenti.</span><span class="sxs-lookup"><span data-stu-id="fe34d-305">**System.Hosting** reports as OK for each code package if the activation is successful.</span></span> <span data-ttu-id="fe34d-306">Ha az aktiválás sikertelen, akkor hiányára konfigurált módon.</span><span class="sxs-lookup"><span data-stu-id="fe34d-306">If the activation fails, it reports a warning as configured.</span></span> <span data-ttu-id="fe34d-307">Ha **CodePackage** nem tudja aktiválni vagy nagyobb, mint a beállított hibával leáll **CodePackageHealthErrorThreshold**, üzemeltető egy hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="fe34d-307">If **CodePackage** fails to activate or terminates with an error greater than the configured **CodePackageHealthErrorThreshold**, hosting reports an error.</span></span> <span data-ttu-id="fe34d-308">A service-csomag kód több csomagot tartalmaz, az aktiválási jelentés minden egyes jön létre.</span><span class="sxs-lookup"><span data-stu-id="fe34d-308">If a service package contains multiple code packages, an activation report is generated for each one.</span></span>

* <span data-ttu-id="fe34d-309">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="fe34d-309">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="fe34d-310">**Tulajdonság**: az előtag- **CodePackageActivation** és nevét, valamint a kódcsomag a belépési pontot tartalmaz  **CodePackageActivation:*CodePackageName* :*SetupEntryPoint/EntryPoint*** (például **CodePackageActivation:Code:SetupEntryPoint**)</span><span class="sxs-lookup"><span data-stu-id="fe34d-310">**Property**: Uses the prefix **CodePackageActivation** and contains the name of the code package and the entry point as **CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint*** (for example, **CodePackageActivation:Code:SetupEntryPoint**)</span></span>

### <a name="service-type-registration"></a><span data-ttu-id="fe34d-311">Szolgáltatási típus regisztrációs</span><span class="sxs-lookup"><span data-stu-id="fe34d-311">Service type registration</span></span>
<span data-ttu-id="fe34d-312">**System.Hosting** OK jelent, ha a szolgáltatás típusának regisztrálása sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="fe34d-312">**System.Hosting** reports as OK if the service type has been registered successfully.</span></span> <span data-ttu-id="fe34d-313">Azt egy hibát jelez, ha a regisztráció nem volt meg időben (használatával konfigurált **ServiceTypeRegistrationTimeout**).</span><span class="sxs-lookup"><span data-stu-id="fe34d-313">It reports an error if the registration wasn't done in time (as configured by using **ServiceTypeRegistrationTimeout**).</span></span> <span data-ttu-id="fe34d-314">A futtatókörnyezet le van zárva, ha a szolgáltatás típusa a csomópont regisztrációját, és üzemeltetési hiányára.</span><span class="sxs-lookup"><span data-stu-id="fe34d-314">If the runtime is closed, the service type is unregistered from the node and Hosting reports a warning.</span></span>

* <span data-ttu-id="fe34d-315">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="fe34d-315">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="fe34d-316">**Tulajdonság**: az előtag- **ServiceTypeRegistration** és a szolgáltatás környezettípus nevét tartalmazza (például **ServiceTypeRegistration:FileStoreServiceType**)</span><span class="sxs-lookup"><span data-stu-id="fe34d-316">**Property**: Uses the prefix **ServiceTypeRegistration** and contains the service type name (for example, **ServiceTypeRegistration:FileStoreServiceType**)</span></span>

<span data-ttu-id="fe34d-317">A következő példa bemutatja a megfelelő telepített szervizcsomag:</span><span class="sxs-lookup"><span data-stu-id="fe34d-317">The following example shows a healthy deployed service package:</span></span>

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_1
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131445249084026346
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131445249084306362
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131445249088096842
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a><span data-ttu-id="fe34d-318">Letöltés</span><span class="sxs-lookup"><span data-stu-id="fe34d-318">Download</span></span>
<span data-ttu-id="fe34d-319">**System.Hosting** egy hibát jelez, ha a service-csomag letöltése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="fe34d-319">**System.Hosting** reports an error if the service package download fails.</span></span>

* <span data-ttu-id="fe34d-320">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="fe34d-320">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="fe34d-321">**Tulajdonság**:  **letöltése:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="fe34d-321">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="fe34d-322">**További lépések**: vizsgálja meg, miért nem sikerült a letöltés a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="fe34d-322">**Next steps**: Investigate why the download failed on the node.</span></span>

### <a name="upgrade-validation"></a><span data-ttu-id="fe34d-323">Frissítésének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="fe34d-323">Upgrade validation</span></span>
<span data-ttu-id="fe34d-324">**System.Hosting** egy hibát jelez, ha a frissítés során az érvényesítés meghiúsul, vagy ha a frissítés sikertelen lesz a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="fe34d-324">**System.Hosting** reports an error if validation during the upgrade fails or if the upgrade fails on the node.</span></span>

* <span data-ttu-id="fe34d-325">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="fe34d-325">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="fe34d-326">**Tulajdonság**: az előtag- **FabricUpgradeValidation** , és tartalmazza a frissített verzió</span><span class="sxs-lookup"><span data-stu-id="fe34d-326">**Property**: Uses the prefix **FabricUpgradeValidation** and contains the upgrade version</span></span>
* <span data-ttu-id="fe34d-327">**Leírás**: mutat, a következő hiba</span><span class="sxs-lookup"><span data-stu-id="fe34d-327">**Description**: Points to the error encountered</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe34d-328">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe34d-328">Next steps</span></span>
[<span data-ttu-id="fe34d-329">A Service Fabric rendszerállapot-jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="fe34d-329">View Service Fabric health reports</span></span>](service-fabric-view-entities-aggregated-health.md)

[<span data-ttu-id="fe34d-330">Jelentés és a szolgáltatás állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="fe34d-330">How to report and check service health</span></span>](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[<span data-ttu-id="fe34d-331">Figyelése és diagnosztizálása helyileg szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="fe34d-331">Monitor and diagnose services locally</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="fe34d-332">A Service Fabric-alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="fe34d-332">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)


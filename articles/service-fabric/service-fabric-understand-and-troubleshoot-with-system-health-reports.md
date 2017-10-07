---
title: "a rendszerállapot-jelentések aaaTroubleshoot |} Microsoft Docs"
description: "Hibaelhárítási fürt vagy az alkalmazással kapcsolatos problémák Azure Service Fabric-összetevők és a használati által küldött hello állapotjelentések ismerteti."
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
ms.openlocfilehash: c77a6cdd0440ce5d354cd8760f40151f674a3529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-system-health-reports-tootroubleshoot"></a><span data-ttu-id="2ff6c-103">Használja a rendszer állapotát jelentések tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="2ff6c-103">Use system health reports tootroubleshoot</span></span>
<span data-ttu-id="2ff6c-104">Az Azure Service Fabric összetevői jelentés hello kezdő verzióról hello fürt valamennyi entitást.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-104">Azure Service Fabric components report out of hello box on all entities in hello cluster.</span></span> <span data-ttu-id="2ff6c-105">Hello [a health Store adatbázisban](service-fabric-health-introduction.md#health-store) hoz létre, és törli az entitások hello rendszer jelentések alapján.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-105">hello [health store](service-fabric-health-introduction.md#health-store) creates and deletes entities based on hello system reports.</span></span> <span data-ttu-id="2ff6c-106">Azt is rendszerezi azokat a hierarchiában, amely rögzíti az entitás interakciókat.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-106">It also organizes them in a hierarchy that captures entity interactions.</span></span>

> [!NOTE]
> <span data-ttu-id="2ff6c-107">toounderstand állapotfigyelő kapcsolatos fogalmak, további információ: [Service Fabric állapotmodell](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-107">toounderstand health-related concepts, read more at [Service Fabric health model](service-fabric-health-introduction.md).</span></span>
> 
> 

<span data-ttu-id="2ff6c-108">Rendszerállapot-jelentések adja meg a fürt és az alkalmazás funkciói és keresztül állapotát jelző problémák láthatósága.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-108">System health reports provide visibility into cluster and application functionality and flag issues through health.</span></span> <span data-ttu-id="2ff6c-109">Az alkalmazások és szolgáltatások rendszerállapot-jelentések győződjön meg arról, hogy az entitások vannak megvalósítva, és a Service Fabric perspektíva hello megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-109">For applications and services, system health reports verify that entities are implemented and are behaving correctly from hello Service Fabric perspective.</span></span> <span data-ttu-id="2ff6c-110">hello jelentések nem biztosít semmilyen állapotfigyelés hello üzleti logika hello szolgáltatást vagy a lefagyott folyamatok észlelése.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-110">hello reports do not provide any health monitoring of hello business logic of hello service or detection of hung processes.</span></span> <span data-ttu-id="2ff6c-111">Felhasználó szolgáltatások állapotfigyelő hello állapotadatok információt adott tootheir logikával.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-111">User services can enrich hello health data with information specific tootheir logic.</span></span>

> [!NOTE]
> <span data-ttu-id="2ff6c-112">Watchdogs állapotjelentések láthatók csak *után* hello rendszerösszetevők entitás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-112">Watchdogs health reports are visible only *after* hello system components create an entity.</span></span> <span data-ttu-id="2ff6c-113">Egy entitás törlésekor a hello a health Store adatbázisban automatikusan törli a hozzá társított összes rendszerállapot-jelentések.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-113">When an entity is deleted, hello health store automatically deletes all health reports associated with it.</span></span> <span data-ttu-id="2ff6c-114">hello is igaz hello entitás új példányának létrehozásakor (például egy új állapot-nyilvántartó megőrzött replika szolgáltatáspéldány jön létre).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-114">hello same is true when a new instance of hello entity is created (for example, a new stateful persisted service replica instance is created).</span></span> <span data-ttu-id="2ff6c-115">Régi hello-példányhoz társított összes jelentések törlése és hello-tárolóból törölni.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-115">All reports associated with hello old instance are deleted and cleaned up from hello store.</span></span>
> 
> 

<span data-ttu-id="2ff6c-116">hello rendszer összetevő jelentések azonosítva hello forrás, amely hello kezdődik "**System.**"</span><span class="sxs-lookup"><span data-stu-id="2ff6c-116">hello system component reports are identified by hello source, which starts with hello "**System.**"</span></span> <span data-ttu-id="2ff6c-117">előtag.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-117">prefix.</span></span> <span data-ttu-id="2ff6c-118">Watchdogs nem használhatja ugyanazt a források előtag hello érvénytelen paramétereket tartalmazó jelentések utasítja el.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-118">Watchdogs can't use hello same prefix for their sources, as reports with invalid parameters are rejected.</span></span>
<span data-ttu-id="2ff6c-119">Most néhány rendszer pillantást jelentés toounderstand mi elindítja azokat, és hogyan toocorrect hello lehetséges problémákat jelölnek.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-119">Let's look at some system reports toounderstand what triggers them and how toocorrect hello possible issues they represent.</span></span>

> [!NOTE]
> <span data-ttu-id="2ff6c-120">A Service Fabric továbbra is fennáll, a feltételek egyik fontos, hogy mi történik a hello fürt- és láthatósága javítása tooadd jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-120">Service Fabric continues tooadd reports on conditions of interest that improve visibility into what is happening in hello cluster and application.</span></span> <span data-ttu-id="2ff6c-121">Meglévő jelentések is fejleszthető további részletekkel toohelp hello probléma elhárításához gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-121">Existing reports can also be enhanced with more details toohelp troubleshoot hello problem faster.</span></span>
> 
> 

## <a name="cluster-system-health-reports"></a><span data-ttu-id="2ff6c-122">Fürt rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="2ff6c-122">Cluster system health reports</span></span>
<span data-ttu-id="2ff6c-123">hello fürt állapotfigyelő entitás hello a health Store adatbázisban automatikusan jön létre.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-123">hello cluster health entity is created automatically in hello health store.</span></span> <span data-ttu-id="2ff6c-124">Ha minden megfelelően működik, a rendszer a jelentés nem tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-124">If everything works properly, it doesn't have a system report.</span></span>

### <a name="neighborhood-loss"></a><span data-ttu-id="2ff6c-125">Hálózatok adatvesztés</span><span class="sxs-lookup"><span data-stu-id="2ff6c-125">Neighborhood loss</span></span>
<span data-ttu-id="2ff6c-126">**System.Federation** egy hibát jelez, amikor azt észleli, hogy a helyek adatvesztés.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-126">**System.Federation** reports an error when it detects a neighborhood loss.</span></span> <span data-ttu-id="2ff6c-127">hello jelentés az egyes csomópontokon, és hello tulajdonság neve tartalmazza a hello csomópont-Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-127">hello report is from individual nodes, and hello node ID is included in hello property name.</span></span> <span data-ttu-id="2ff6c-128">Ha egy helyek hello a Service Fabric gyűrűben teljes elveszik, általában számíthat két esemény (mindkét végének az hello résnek jelentés).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-128">If one neighborhood is lost in hello entire Service Fabric ring, you can typically expect two events (both sides of hello gap report).</span></span> <span data-ttu-id="2ff6c-129">További környékeken elvesznek, ha nincsenek további események.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-129">If more neighborhoods are lost, there are more events.</span></span>

<span data-ttu-id="2ff6c-130">hello jelentés hello globális bérleti idejét toolive hello időt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-130">hello report specifies hello global lease timeout as hello time toolive.</span></span> <span data-ttu-id="2ff6c-131">hello jelentés minden hello TTL időtartama felében Újraküldte mindaddig, amíg hello feltétel aktív marad.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-131">hello report is resent every half of hello TTL duration for as long as hello condition remains active.</span></span> <span data-ttu-id="2ff6c-132">hello esemény automatikusan törlődik, amikor ez megtörténik.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-132">hello event is automatically removed when it expires.</span></span> <span data-ttu-id="2ff6c-133">Távolítsa el, ha lejárt a viselkedés garantálja az, hogy hello jelentés törlődnek a hello health store megfelelően, még akkor is, ha hello jelentéskészítési csomópont nem működik.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-133">Remove when expired behavior ensures that hello report is cleaned up from hello health store correctly, even if hello reporting node is down.</span></span>

* <span data-ttu-id="2ff6c-134">**SourceId**: System.Federation</span><span class="sxs-lookup"><span data-stu-id="2ff6c-134">**SourceId**: System.Federation</span></span>
* <span data-ttu-id="2ff6c-135">**Tulajdonság**: kezdődik **hálózatok** és csomópont adatai</span><span class="sxs-lookup"><span data-stu-id="2ff6c-135">**Property**: Starts with **Neighborhood** and includes node information</span></span>
* <span data-ttu-id="2ff6c-136">**További lépések**: vizsgálja meg, miért hello hálózatok elvész (például ellenőrizze hello kommunikációs fürt csomópontjai között).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-136">**Next steps**: Investigate why hello neighborhood is lost (for example, check hello communication between cluster nodes).</span></span>

## <a name="node-system-health-reports"></a><span data-ttu-id="2ff6c-137">Csomópont rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="2ff6c-137">Node system health reports</span></span>
<span data-ttu-id="2ff6c-138">**System.FM**, amely hello Feladatátvevőfürt-kezelő szolgáltatás jelöli, a hello hatóság gondoskodik a fürtcsomópontok kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-138">**System.FM**, which represents hello Failover Manager service, is hello authority that manages information about cluster nodes.</span></span> <span data-ttu-id="2ff6c-139">Minden csomópont állapotát megjelenítő System.FM egy jelentést kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-139">Each node should have one report from System.FM showing its state.</span></span> <span data-ttu-id="2ff6c-140">hello csomópont entitások hello csomópont állapota eltávolításakor törlődnek (lásd: [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-140">hello node entities are removed when hello node state is removed (see [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).</span></span>

### <a name="node-updown"></a><span data-ttu-id="2ff6c-141">A csomópont fel/le</span><span class="sxs-lookup"><span data-stu-id="2ff6c-141">Node up/down</span></span>
<span data-ttu-id="2ff6c-142">System.FM jelentések OK, amikor hello csomópont csatlakozik hello ring (már fut).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-142">System.FM reports as OK when hello node joins hello ring (it's up and running).</span></span> <span data-ttu-id="2ff6c-143">Az egy hibát jelez, amikor hello csomópont indulását hello ring (szolgáltatás leállt, vagy frissítés, vagy egyszerűen mert sikertelen volt).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-143">It reports an error when hello node departs hello ring (it's down, either for upgrading or simply because it has failed).</span></span> <span data-ttu-id="2ff6c-144">hello állapotfigyelő hierarchia által hello a health Store adatbázisban a korrelációs System.FM csomópont jelentések a telepített entitások hajt végre műveletet.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-144">hello health hierarchy built by hello health store takes action on deployed entities in correlation with System.FM node reports.</span></span> <span data-ttu-id="2ff6c-145">Figyelembe vételével hello csomópont összes telepített entitás virtuális szülője.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-145">It considers hello node a virtual parent of all deployed entities.</span></span> <span data-ttu-id="2ff6c-146">Ezen a csomóponton telepített hello entitások lekérdezések keresztül közzétett hello csomópont készként való mentése System.FM, amelyet a hello azonos példány hello entitások társított hello példányt.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-146">hello deployed entities on that node are exposed through queries if hello node is reported as up by System.FM, with hello same instance as hello instance associated with hello entities.</span></span> <span data-ttu-id="2ff6c-147">Amikor System.FM hello csomóponton nem működik, vagy újra (egy új példány) jelez, hello a health Store adatbázisban automatikusan törli a szükségtelenné létezhet csak hello csomópont le vagy hello hello csomópont korábbi példányán telepítve hello entitások.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-147">When System.FM reports that hello node is down or restarted (a new instance), hello health store automatically cleans up hello deployed entities that can exist only on hello down node or on hello previous instance of hello node.</span></span>

* <span data-ttu-id="2ff6c-148">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="2ff6c-148">**SourceId**: System.FM</span></span>
* <span data-ttu-id="2ff6c-149">**Tulajdonság**: állapota</span><span class="sxs-lookup"><span data-stu-id="2ff6c-149">**Property**: State</span></span>
* <span data-ttu-id="2ff6c-150">**További lépések**: hello csomópont frissítés szempontjából nem működik, ha kellene tartoznia hozzá biztonsági után frissítették.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-150">**Next steps**: If hello node is down for an upgrade, it should come back up once it has been upgraded.</span></span> <span data-ttu-id="2ff6c-151">Ebben az esetben hello állapota hátsó tooOK kell váltani.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-151">In this case, hello health state should switch back tooOK.</span></span> <span data-ttu-id="2ff6c-152">Hello csomópont nem térjen vissza, vagy nem sikerül, ha a probléma hello kell további vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-152">If hello node doesn't come back or it fails, hello problem needs more investigation.</span></span>

<span data-ttu-id="2ff6c-153">hello alábbi példa bemutatja a csomópont OK állapotot hello System.FM esemény:</span><span class="sxs-lookup"><span data-stu-id="2ff6c-153">hello following example shows hello System.FM event with a health state of OK for node up:</span></span>

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


### <a name="certificate-expiration"></a><span data-ttu-id="2ff6c-154">Tanúsítvány lejárta</span><span class="sxs-lookup"><span data-stu-id="2ff6c-154">Certificate expiration</span></span>
<span data-ttu-id="2ff6c-155">**System.FabricNode** Ha hello csomópont által használt tanúsítványok lejáró hiányára.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-155">**System.FabricNode** reports a warning when certificates used by hello node are near expiration.</span></span> <span data-ttu-id="2ff6c-156">Három tanúsítvány csomópontonként: **Certificate_cluster**, **Certificate_server**, és **Certificate_default_client**.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-156">There are three certificates per node: **Certificate_cluster**, **Certificate_server**, and **Certificate_default_client**.</span></span> <span data-ttu-id="2ff6c-157">Legalább két héttel hello lejárati esetén hello jelentés állapota rendben.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-157">When hello expiration is at least two weeks away, hello report health state is OK.</span></span> <span data-ttu-id="2ff6c-158">Ha hello lejárati két héten belül, hello jelentés típus: figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-158">When hello expiration is within two weeks, hello report type is a warning.</span></span> <span data-ttu-id="2ff6c-159">Ezek az események TTL végtelen, és egy csomópont elhagyásakor hello fürt eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-159">TTL of these events is infinite, and they are removed when a node leaves hello cluster.</span></span>

* <span data-ttu-id="2ff6c-160">**SourceId**: System.FabricNode</span><span class="sxs-lookup"><span data-stu-id="2ff6c-160">**SourceId**: System.FabricNode</span></span>
* <span data-ttu-id="2ff6c-161">**Tulajdonság**: kezdődik **tanúsítvány** és hello tanúsítványtípust további információt tartalmaz</span><span class="sxs-lookup"><span data-stu-id="2ff6c-161">**Property**: Starts with **Certificate** and contains more information about hello certificate type</span></span>
* <span data-ttu-id="2ff6c-162">**További lépések**: hello tanúsítványok frissítése, ha azok lejáró.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-162">**Next steps**: Update hello certificates if they are near expiration.</span></span>

### <a name="load-capacity-violation"></a><span data-ttu-id="2ff6c-163">Betöltési kapacitás megsértése</span><span class="sxs-lookup"><span data-stu-id="2ff6c-163">Load capacity violation</span></span>
<span data-ttu-id="2ff6c-164">Service Fabric terheléselosztó hello hiányára amikor azt észleli, hogy a csomópont-kapacitás megsértése.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-164">hello Service Fabric Load Balancer reports a warning when it detects a node capacity violation.</span></span>

* <span data-ttu-id="2ff6c-165">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="2ff6c-165">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="2ff6c-166">**Tulajdonság**: kezdődik **kapacitás**</span><span class="sxs-lookup"><span data-stu-id="2ff6c-166">**Property**: Starts with **Capacity**</span></span>
* <span data-ttu-id="2ff6c-167">**További lépések**: Ellenőrizze a megadott metrikák és a nézet hello aktuális kapacitás hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-167">**Next steps**: Check provided metrics and view hello current capacity on hello node.</span></span>

## <a name="application-system-health-reports"></a><span data-ttu-id="2ff6c-168">Alkalmazás rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="2ff6c-168">Application system health reports</span></span>
<span data-ttu-id="2ff6c-169">**System.CM**, amely hello kezelő szolgáltatást jelenti, hello szervezet, amely felügyeli az alkalmazással kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-169">**System.CM**, which represents hello Cluster Manager service, is hello authority that manages information about an application.</span></span>

### <a name="state"></a><span data-ttu-id="2ff6c-170">Állapot</span><span class="sxs-lookup"><span data-stu-id="2ff6c-170">State</span></span>
<span data-ttu-id="2ff6c-171">System.CM jelzi az OK gombra hello alkalmazás létrehozásakor vagy frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-171">System.CM reports as OK when hello application has been created or updated.</span></span> <span data-ttu-id="2ff6c-172">Az tájékoztatja hello a health Store adatbázisban hello alkalmazás törlésekor, hogy az áruházból el kell távolítani.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-172">It informs hello health store when hello application has been deleted, so that it can be removed from store.</span></span>

* <span data-ttu-id="2ff6c-173">**SourceId**: System.CM</span><span class="sxs-lookup"><span data-stu-id="2ff6c-173">**SourceId**: System.CM</span></span>
* <span data-ttu-id="2ff6c-174">**Tulajdonság**: állapota</span><span class="sxs-lookup"><span data-stu-id="2ff6c-174">**Property**: State</span></span>
* <span data-ttu-id="2ff6c-175">**További lépések**: hello alkalmazás létrehozás vagy frissítés, ha hello kezelő állapotjelentést tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-175">**Next steps**: If hello application has been created or updated, it should include hello Cluster Manager health report.</span></span> <span data-ttu-id="2ff6c-176">Ellenkező esetben ellenőrizze a lekérdezés kiállításával hello alkalmazás hello állapota (például a PowerShell-parancsmag hello **Get-ServiceFabricApplication - ApplicationName *applicationName***).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-176">Otherwise, check hello state of hello application by issuing a query (for example, hello PowerShell cmdlet **Get-ServiceFabricApplication -ApplicationName *applicationName***).</span></span>

<span data-ttu-id="2ff6c-177">hello alábbi példa látható hello állapot esemény hello **fabric: / WordCount** alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="2ff6c-177">hello following example shows hello state event on hello **fabric:/WordCount** application:</span></span>

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

## <a name="service-system-health-reports"></a><span data-ttu-id="2ff6c-178">Szolgáltatás rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="2ff6c-178">Service system health reports</span></span>
<span data-ttu-id="2ff6c-179">**System.FM**, amely jelöli hello Feladatátvevőfürt-kezelő szolgáltatás, a hello szolgáltatót, amely kezeli a szolgáltatással kapcsolatban további információt.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-179">**System.FM**, which represents hello Failover Manager service, is hello authority that manages information about services.</span></span>

### <a name="state"></a><span data-ttu-id="2ff6c-180">Állapot</span><span class="sxs-lookup"><span data-stu-id="2ff6c-180">State</span></span>
<span data-ttu-id="2ff6c-181">System.FM jelzi az OK gombra hello szolgáltatás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-181">System.FM reports as OK when hello service has been created.</span></span> <span data-ttu-id="2ff6c-182">Hello entitás a hello health store törli a hello szolgáltatás törlésekor.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-182">It deletes hello entity from hello health store when hello service has been deleted.</span></span>

* <span data-ttu-id="2ff6c-183">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="2ff6c-183">**SourceId**: System.FM</span></span>
* <span data-ttu-id="2ff6c-184">**Tulajdonság**: állapota</span><span class="sxs-lookup"><span data-stu-id="2ff6c-184">**Property**: State</span></span>

<span data-ttu-id="2ff6c-185">hello alábbi példa azt mutatja hello állapot esemény hello szolgáltatásban **fabric: / WordCount/wordcountwebservice verziója**:</span><span class="sxs-lookup"><span data-stu-id="2ff6c-185">hello following example shows hello state event on hello service **fabric:/WordCount/WordCountWebService**:</span></span>

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

### <a name="service-correlation-error"></a><span data-ttu-id="2ff6c-186">Korrelációs hibája</span><span class="sxs-lookup"><span data-stu-id="2ff6c-186">Service correlation error</span></span>
<span data-ttu-id="2ff6c-187">**System.PLB** egy hibát jelez, amikor azt észleli, hogy a szolgáltatás toobe szorosan összefügg az egy másik szolgáltatás frissítése létrehoz egy affinitási láncot.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-187">**System.PLB** reports an error when it detects that updating a service toobe correlated with another service creates an affinity chain.</span></span> <span data-ttu-id="2ff6c-188">hello jelentés sikeres frissítés történik, ha nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-188">hello report is cleared when successful update happens.</span></span>

* <span data-ttu-id="2ff6c-189">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="2ff6c-189">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="2ff6c-190">**Tulajdonság**: ServiceDescription objektumot</span><span class="sxs-lookup"><span data-stu-id="2ff6c-190">**Property**: ServiceDescription</span></span>
* <span data-ttu-id="2ff6c-191">**További lépések**: ellenőrzés hello korrelált szolgáltatások ismertetése.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-191">**Next steps**: Check hello correlated service descriptions.</span></span>

## <a name="partition-system-health-reports"></a><span data-ttu-id="2ff6c-192">Partíció rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="2ff6c-192">Partition system health reports</span></span>
<span data-ttu-id="2ff6c-193">**System.FM**, amely hello Feladatátvevőfürt-kezelő szolgáltatást jelenti, a hello szervezet szolgáltatáspartíciók kapcsolatos információt.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-193">**System.FM**, which represents hello Failover Manager service, is hello authority that manages information about service partitions.</span></span>

### <a name="state"></a><span data-ttu-id="2ff6c-194">Állapot</span><span class="sxs-lookup"><span data-stu-id="2ff6c-194">State</span></span>
<span data-ttu-id="2ff6c-195">System.FM jelzi az OK gombra, ha hello partíció létrejött, és megfelelő állapotban.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-195">System.FM reports as OK when hello partition has been created and is healthy.</span></span> <span data-ttu-id="2ff6c-196">Hello entitás a hello health store törli a hello partíció törlése.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-196">It deletes hello entity from hello health store when hello partition is deleted.</span></span>

<span data-ttu-id="2ff6c-197">Hello partíció nem éri el hello replikakészlet minimális száma, ha hiba jelenti.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-197">If hello partition is below hello minimum replica count, it reports an error.</span></span> <span data-ttu-id="2ff6c-198">Ha hello partíció nem hello replikakészlet minimális száma alatt van, de kevesebb hello cél replika, a rendszer figyelmeztetést jelenti.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-198">If hello partition is not below hello minimum replica count, but it is below hello target replica count, it reports a warning.</span></span> <span data-ttu-id="2ff6c-199">Hello partíció kvórumveszteségben esetén System.FM jelent hibát.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-199">If hello partition is in quorum loss, System.FM reports an error.</span></span>

<span data-ttu-id="2ff6c-200">Egyéb fontos események tartalmazhat egy figyelmeztetés, ha hello újrakonfigurálás hosszabb időt vesz igénybe a vártnál, és ha hello build vártnál hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-200">Other important events include a warning when hello reconfiguration takes longer than expected and when hello build takes longer than expected.</span></span> <span data-ttu-id="2ff6c-201">várt hello hello build és újrakonfigurálását érvényes találhatók konfigurálható szolgáltatás forgatókönyvek alapján.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-201">hello expected times for hello build and reconfiguration are configurable based on service scenarios.</span></span> <span data-ttu-id="2ff6c-202">Például ha a szolgáltatás egy terabájt állapot, például az SQL-adatbázis, hello build tovább tart, mint egy szolgáltatás állapotát kis mennyiségű.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-202">For example, if a service has a terabyte of state, such as SQL Database, hello build takes longer than for a service with a small amount of state.</span></span>

* <span data-ttu-id="2ff6c-203">**SourceId**: System.FM</span><span class="sxs-lookup"><span data-stu-id="2ff6c-203">**SourceId**: System.FM</span></span>
* <span data-ttu-id="2ff6c-204">**Tulajdonság**: állapota</span><span class="sxs-lookup"><span data-stu-id="2ff6c-204">**Property**: State</span></span>
* <span data-ttu-id="2ff6c-205">**További lépések**: hello állapota nem OK, akkor lehetséges, hogy egyes másodpéldányok még nem lettek létrehozott, megnyitott vagy előléptetett tooprimary vagy másodlagos megfelelően.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-205">**Next steps**: If hello health state is not OK, it's possible that some replicas have not been created, opened, or promoted tooprimary or secondary correctly.</span></span> <span data-ttu-id="2ff6c-206">Sok esetben hello okozza-e nyitva hello vagy szerepkör módosítása végrehajtása szolgáltatási hiba.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-206">In many instances, hello root cause is a service bug in hello open or change-role implementation.</span></span>

<span data-ttu-id="2ff6c-207">a következő példa hello kifogástalan partíció jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="2ff6c-207">hello following example shows a healthy partition:</span></span>

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

<span data-ttu-id="2ff6c-208">hello következő példa bemutatja, javasoljuk, hogy kevesebb cél replika hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-208">hello following example shows hello health of a partition that is below target replica count.</span></span> <span data-ttu-id="2ff6c-209">következő lépés hello tooget hello partíció leírása, amely bemutatja, hogyan van konfigurálva: **MinReplicaSetSize** három és **TargetReplicaSetSize** hét.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-209">hello next step is tooget hello partition description, which shows how it is configured: **MinReplicaSetSize** is three and **TargetReplicaSetSize** is seven.</span></span> <span data-ttu-id="2ff6c-210">Majd kérje le a csomópontok száma hello hello fürt: öt.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-210">Then get hello number of nodes in hello cluster: five.</span></span> <span data-ttu-id="2ff6c-211">Ebben az esetben két replika nem kell elhelyezni, mert hello cél található replikák száma nagyobb, mint az elérhető csomópontok hello száma.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-211">So in this case, two replicas can't be placed because hello target number of replicas is higher than hello number of nodes available.</span></span>

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

### <a name="replica-constraint-violation"></a><span data-ttu-id="2ff6c-212">A replika korlátozássértést</span><span class="sxs-lookup"><span data-stu-id="2ff6c-212">Replica constraint violation</span></span>
<span data-ttu-id="2ff6c-213">**System.PLB** jelentések figyelmeztetés, ha egy replika korlátozássértést észlelt, és nem helyezhető el az összes partíció replika.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-213">**System.PLB** reports a warning if it detects a replica constraint violation and can't place all partition replicas.</span></span> <span data-ttu-id="2ff6c-214">hello jelentés részletek megjelenítése milyen korlátozások és tulajdonságok megelőzheti a hello replika elhelyezésének.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-214">hello report details show which constraints and properties prevent hello replica placement.</span></span>

* <span data-ttu-id="2ff6c-215">**SourceId**: System.PLB</span><span class="sxs-lookup"><span data-stu-id="2ff6c-215">**SourceId**: System.PLB</span></span>
* <span data-ttu-id="2ff6c-216">**Tulajdonság**: kezdődik **ReplicaConstraintViolation**</span><span class="sxs-lookup"><span data-stu-id="2ff6c-216">**Property**: Starts with **ReplicaConstraintViolation**</span></span>

## <a name="replica-system-health-reports"></a><span data-ttu-id="2ff6c-217">A replika rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="2ff6c-217">Replica system health reports</span></span>
<span data-ttu-id="2ff6c-218">**System.RA**, amely jelenti, hogy hello reconfiguration agent összetevő hello hatóság hello replika állapot.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-218">**System.RA**, which represents hello reconfiguration agent component, is hello authority for hello replica state.</span></span>

### <a name="state"></a><span data-ttu-id="2ff6c-219">Állapot</span><span class="sxs-lookup"><span data-stu-id="2ff6c-219">State</span></span>
<span data-ttu-id="2ff6c-220">**System.RA** OK jelentések hello replika létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-220">**System.RA** reports OK when hello replica has been created.</span></span>

* <span data-ttu-id="2ff6c-221">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="2ff6c-221">**SourceId**: System.RA</span></span>
* <span data-ttu-id="2ff6c-222">**Tulajdonság**: állapota</span><span class="sxs-lookup"><span data-stu-id="2ff6c-222">**Property**: State</span></span>

<span data-ttu-id="2ff6c-223">a következő példa hello jeleníti meg a megfelelő replika:</span><span class="sxs-lookup"><span data-stu-id="2ff6c-223">hello following example shows a healthy replica:</span></span>

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

### <a name="replica-open-status"></a><span data-ttu-id="2ff6c-224">Nyissa meg a replika állapota</span><span class="sxs-lookup"><span data-stu-id="2ff6c-224">Replica open status</span></span>
<span data-ttu-id="2ff6c-225">hello a jelentés leírása hello kezdete (egyezményes világidő) hello API-hívás metódus meghívásakor.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-225">hello description of this health report contains hello start time (Coordinated Universal Time) when hello API call was invoked.</span></span>

<span data-ttu-id="2ff6c-226">**System.RA** hiányára Ha hello replika nyissa meg a konfigurált hello időszak hosszabb időbe telik (alapértelmezett: 30 perc).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-226">**System.RA** reports a warning if hello replica open takes longer than hello configured period (default: 30 minutes).</span></span> <span data-ttu-id="2ff6c-227">Ha hello API szolgáltatás rendelkezésre állása befolyásolja, hello jelentés kiadott sokkal gyorsabb (egy konfigurálható időszakban, az alapértelmezett érték 30 másodperc).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-227">If hello API impacts service availability, hello report is issued much faster (a configurable interval, with a default of 30 seconds).</span></span> <span data-ttu-id="2ff6c-228">mért hello idő hello idő hello replikátor nyissa meg és a nyitott hello szolgáltatást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-228">hello time measured includes hello time taken for hello replicator open and hello service open.</span></span> <span data-ttu-id="2ff6c-229">hello tulajdonság módosítások tooOK Ha nyissa meg a hello befejeződik.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-229">hello property changes tooOK if hello open completes.</span></span>

* <span data-ttu-id="2ff6c-230">**SourceId**: System.RA</span><span class="sxs-lookup"><span data-stu-id="2ff6c-230">**SourceId**: System.RA</span></span>
* <span data-ttu-id="2ff6c-231">**Tulajdonság**: **ReplicaOpenStatus**</span><span class="sxs-lookup"><span data-stu-id="2ff6c-231">**Property**: **ReplicaOpenStatus**</span></span>
* <span data-ttu-id="2ff6c-232">**További lépések**: Ha hello rendszerállapot nem OK, vizsgálja meg, miért hello replika nyissa meg a vártnál hosszabb időbe telik.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-232">**Next steps**: If hello health state is not OK, investigate why hello replica open takes longer than expected.</span></span>

### <a name="slow-service-api-call"></a><span data-ttu-id="2ff6c-233">Lassú service API-hívás</span><span class="sxs-lookup"><span data-stu-id="2ff6c-233">Slow service API call</span></span>
<span data-ttu-id="2ff6c-234">**System.RAP** és **System.Replicator** jelentést egy figyelmeztetés, ha egy hívás toohello felhasználói szolgáltatás kód hello beállított idő hosszabb időbe telik.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-234">**System.RAP** and **System.Replicator** report a warning if a call toohello user service code takes longer than hello configured time.</span></span> <span data-ttu-id="2ff6c-235">hello figyelmeztetés hello hívás befejezését nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-235">hello warning is cleared when hello call completes.</span></span>

* <span data-ttu-id="2ff6c-236">**SourceId**: System.RAP vagy System.Replicator</span><span class="sxs-lookup"><span data-stu-id="2ff6c-236">**SourceId**: System.RAP or System.Replicator</span></span>
* <span data-ttu-id="2ff6c-237">**Tulajdonság**: hello lassú API hello neve.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-237">**Property**: hello name of hello slow API.</span></span> <span data-ttu-id="2ff6c-238">hello leírása itt hello idő hello API kapcsolatos további részletekért volt függőben.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-238">hello description provides more details about hello time hello API has been pending.</span></span>
* <span data-ttu-id="2ff6c-239">**További lépések**: vizsgálja meg, miért hello hívás vártnál hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-239">**Next steps**: Investigate why hello call takes longer than expected.</span></span>

<span data-ttu-id="2ff6c-240">a következő példa hello partíció kvórum elvesztése és hello vizsgálati lépéseket végre toofigure kimenő miért láthatók.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-240">hello following example shows a partition in quorum loss, and hello investigation steps done toofigure out why.</span></span> <span data-ttu-id="2ff6c-241">Hello replikák egyike figyelmeztetési állapot, hogy az állapotát.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-241">One of hello replicas has a warning health state, so you get its health.</span></span> <span data-ttu-id="2ff6c-242">Azt mutatja, hogy hello szolgáltatási művelet hosszabb időt vesz igénybe, mint a System.RAP által jelentett eseményt várt.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-242">It shows that hello service operation takes longer than expected, an event reported by System.RAP.</span></span> <span data-ttu-id="2ff6c-243">Ezek az információk fogadását követően hello tovább toolook hello szolgáltatás kódot, és vizsgálja meg a hiba.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-243">After this information is received, hello next step is toolook at hello service code and investigate there.</span></span> <span data-ttu-id="2ff6c-244">Ebben az esetben hello **RunAsync** hello állapotalapú szolgáltatás megvalósítása nem kezelt kivételt jelez.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-244">For this case, hello **RunAsync** implementation of hello stateful service throws an unhandled exception.</span></span> <span data-ttu-id="2ff6c-245">hello replikák vannak újrahasznosítása, ezért nem láthatók a replikák hello figyelmeztetési állapotban.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-245">hello replicas are recycling, so you may not see any replicas in hello warning state.</span></span> <span data-ttu-id="2ff6c-246">Ismételje meg az első hello állapotát, és keresse meg az eltérések a hello replika azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-246">You can retry getting hello health state and look for any differences in hello replica ID.</span></span> <span data-ttu-id="2ff6c-247">Bizonyos esetekben hello újrapróbálkozások tudhatja meg keresik.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-247">In certain cases, hello retries can give you clues.</span></span>

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

<span data-ttu-id="2ff6c-248">Hello hibás alkalmazást hello hibakereső indításakor hello diagnosztikai események windows hello kivétel lépett fel az RunAsync a jelenjen meg:</span><span class="sxs-lookup"><span data-stu-id="2ff6c-248">When you start hello faulty application under hello debugger, hello diagnostic events windows show hello exception thrown from RunAsync:</span></span>

![A Visual Studio 2015 diagnosztikai események: RunAsync hiba a fabric: / HelloWorldStatefulApplication.][1]

<span data-ttu-id="2ff6c-250">A Visual Studio 2015 diagnosztikai események: RunAsync hiba történt a **fabric: / HelloWorldStatefulApplication**.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-250">Visual Studio 2015 diagnostic events: RunAsync failure in **fabric:/HelloWorldStatefulApplication**.</span></span>

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a><span data-ttu-id="2ff6c-251">Replikációs sor megtelt</span><span class="sxs-lookup"><span data-stu-id="2ff6c-251">Replication queue full</span></span>
<span data-ttu-id="2ff6c-252">**System.Replicator** hiányára hello replikációs sor megtelt.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-252">**System.Replicator** reports a warning when hello replication queue is full.</span></span> <span data-ttu-id="2ff6c-253">Hello elsődleges, a replikációs várólistában általában megtelik mert egy vagy több másodlagos replikák lassú tooacknowledge műveletek.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-253">On hello primary, replication queue usually becomes full because one or more secondary replicas are slow tooacknowledge operations.</span></span> <span data-ttu-id="2ff6c-254">A másodlagos hello Ez általában történik, ha hello szolgáltatás lassú tooapply hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-254">On hello secondary, this usually happens when hello service is slow tooapply hello operations.</span></span> <span data-ttu-id="2ff6c-255">hello figyelmeztetés nincs bejelölve, ha hello várólista már nem teljes.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-255">hello warning is cleared when hello queue is no longer full.</span></span>

* <span data-ttu-id="2ff6c-256">**SourceId**: System.Replicator</span><span class="sxs-lookup"><span data-stu-id="2ff6c-256">**SourceId**: System.Replicator</span></span>
* <span data-ttu-id="2ff6c-257">**Tulajdonság**: **PrimaryReplicationQueueStatus** vagy **SecondaryReplicationQueueStatus**hello replika szerepkör attól függően, hogy</span><span class="sxs-lookup"><span data-stu-id="2ff6c-257">**Property**: **PrimaryReplicationQueueStatus** or **SecondaryReplicationQueueStatus**, depending on hello replica role</span></span>

### <a name="slow-naming-operations"></a><span data-ttu-id="2ff6c-258">Lassú elnevezési műveletek</span><span class="sxs-lookup"><span data-stu-id="2ff6c-258">Slow Naming operations</span></span>
<span data-ttu-id="2ff6c-259">**System.NamingService** jelentések állapotát az elsődleges replikán, amikor egy elnevezési művelet elfogadható hosszabb időbe telik.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-259">**System.NamingService** reports health on its primary replica when a Naming operation takes longer than acceptable.</span></span> <span data-ttu-id="2ff6c-260">Példák elnevezési műveletek [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) vagy [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-260">Examples of Naming operations are [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) or [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync).</span></span> <span data-ttu-id="2ff6c-261">Több metódust alatt található FabricClient, például a [módszereket szolgáltatás](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) vagy [tulajdonság módszereket](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-261">More methods can be found under FabricClient, for example under [service management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) or [property management methods](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).</span></span>

> [!NOTE]
> <span data-ttu-id="2ff6c-262">hello Naming service oldja fel a neveket tooa szolgáltatáskeresés hello fürtben, és lehetővé teszi, hogy a felhasználók toomanage szolgáltatás neveit és tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-262">hello Naming service resolves service names tooa location in hello cluster and enables users toomanage service names and properties.</span></span> <span data-ttu-id="2ff6c-263">A Service Fabric particionálva szolgáltatás megőrzött.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-263">It is a Service Fabric partitioned persisted service.</span></span> <span data-ttu-id="2ff6c-264">Hello Authority Owner, amely tartalmazza az összes Service Fabric-neveket és szolgáltatásokra vonatkozó metaadatok hello partíciók egyik jelöli.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-264">One of hello partitions represents hello Authority Owner, which contains metadata about all Service Fabric names and services.</span></span> <span data-ttu-id="2ff6c-265">hello Service Fabric műveletnevek a következők csatlakoztatott toodifferent partíciók nevű Name Owner partíciók, így hello szolgáltatás bővíthető.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-265">hello Service Fabric names are mapped toodifferent partitions, called Name Owner partitions, so hello service is extensible.</span></span> <span data-ttu-id="2ff6c-266">Tudjon meg többet az [Naming service](service-fabric-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-266">Read more about [Naming service](service-fabric-architecture.md).</span></span>
> 
> 

<span data-ttu-id="2ff6c-267">Amikor egy elnevezési művelet a vártnál hosszabb időt vesz igénybe, hello művelet megjelölt hello figyelmeztetés jelentést *hello partíciójára szolgáltatás, amely az elsődleges replika szolgál hello művelet*.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-267">When a Naming operation takes longer than expected, hello operation is flagged with a Warning report on hello *primary replica of hello Naming service partition that serves hello operation*.</span></span> <span data-ttu-id="2ff6c-268">Ha a hello művelet sikeresen befejeződött, hello figyelmeztetés törlődik.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-268">If hello operation completes successfully, hello Warning is cleared.</span></span> <span data-ttu-id="2ff6c-269">Ha hello művelet egy hibával befejeződött, a hello állapotjelentése tartalmazza hello hiba részleteit.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-269">If hello operation completes with an error, hello health report includes details about hello error.</span></span>

* <span data-ttu-id="2ff6c-270">**SourceId**: System.NamingService</span><span class="sxs-lookup"><span data-stu-id="2ff6c-270">**SourceId**: System.NamingService</span></span>
* <span data-ttu-id="2ff6c-271">**Tulajdonság**: előtaggal kezdődik **Duration_** és azonosítja a hello lassú művelet és hello Service Fabric nevét, mely hello művelet történik.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-271">**Property**: Starts with prefix **Duration_** and identifies hello slow operation and hello Service Fabric name on which hello operation is applied.</span></span> <span data-ttu-id="2ff6c-272">Például ha a szolgáltatás létrehozása a háló nevét: / MyApp/MyService túl sokáig tart, a hello tulajdonság Duration_AOCreateService.fabric:/MyApp/MyService.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-272">For example, if create service at name fabric:/MyApp/MyService takes too long, hello property is Duration_AOCreateService.fabric:/MyApp/MyService.</span></span> <span data-ttu-id="2ff6c-273">AO pontok toohello szerepköre hello elnevezési partíciót ezt a nevet és művelet.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-273">AO points toohello role of hello Naming partition for this name and operation.</span></span>
* <span data-ttu-id="2ff6c-274">**További lépések**: Miért hello elnevezési művelet sikertelen ellenőrzés.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-274">**Next steps**: Check why hello Naming operation fails.</span></span> <span data-ttu-id="2ff6c-275">Minden műveletet lehet különböző alapvető okait.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-275">Each operation can have different root causes.</span></span> <span data-ttu-id="2ff6c-276">Például törlése egy csomópont elakadt előfordulhat, hogy a szolgáltatás, mert hello alkalmazásgazda tartja összeomló hello szolgáltatást kód tooa felhasználói hibája miatt a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-276">For example, delete service may be stuck on a node because hello application host keeps crashing on a node due tooa user bug in hello service code.</span></span>

<span data-ttu-id="2ff6c-277">a következő példa hello szolgáltatás létrehozási művelet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-277">hello following example shows a create service operation.</span></span> <span data-ttu-id="2ff6c-278">hello művelet konfigurált hello időtartamnál hosszabb ideig tartott.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-278">hello operation took longer than hello configured duration.</span></span> <span data-ttu-id="2ff6c-279">AO újrapróbálkozik, és elküldi a munkahelyi tooNO.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-279">AO retries and sends work tooNO.</span></span> <span data-ttu-id="2ff6c-280">NINCS kész hello utolsó művelet – időtúllépés miatt.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-280">NO completed hello last operation with Timeout.</span></span> <span data-ttu-id="2ff6c-281">Ebben az esetben hello azonos replikája elsődleges hello AO mind egyetlen szerepkör.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-281">In this case, hello same replica is primary for both hello AO and NO roles.</span></span>

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
                        Description           : hello AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
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
                        Description           : hello NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a><span data-ttu-id="2ff6c-282">DeployedApplication rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="2ff6c-282">DeployedApplication system health reports</span></span>
<span data-ttu-id="2ff6c-283">**System.Hosting** hello hatóság az üzembe helyezett entitásokra.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-283">**System.Hosting** is hello authority on deployed entities.</span></span>

### <a name="activation"></a><span data-ttu-id="2ff6c-284">az aktiválás</span><span class="sxs-lookup"><span data-stu-id="2ff6c-284">Activation</span></span>
<span data-ttu-id="2ff6c-285">System.Hosting jelzi az OK gombra, ha egy alkalmazás aktiválása megtörtént hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-285">System.Hosting reports as OK when an application has been successfully activated on hello node.</span></span> <span data-ttu-id="2ff6c-286">Ellenkező esetben azt egy hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-286">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="2ff6c-287">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ff6c-287">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ff6c-288">**Tulajdonság**: aktiválási, beleértve hello bevezetés verzióját</span><span class="sxs-lookup"><span data-stu-id="2ff6c-288">**Property**: Activation, including hello rollout version</span></span>
* <span data-ttu-id="2ff6c-289">**További lépések**: Ha hello alkalmazás állapota nem megfelelő, vizsgálja meg, miért hello aktiválás sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-289">**Next steps**: If hello application is unhealthy, investigate why hello activation failed.</span></span>

<span data-ttu-id="2ff6c-290">a következő példa azt mutatja be a sikeres aktiválási hello:</span><span class="sxs-lookup"><span data-stu-id="2ff6c-290">hello following example shows successful activation:</span></span>

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
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a><span data-ttu-id="2ff6c-291">Letöltés</span><span class="sxs-lookup"><span data-stu-id="2ff6c-291">Download</span></span>
<span data-ttu-id="2ff6c-292">**System.Hosting** egy hibát jelez, ha hello alkalmazás csomag letöltése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-292">**System.Hosting** reports an error if hello application package download fails.</span></span>

* <span data-ttu-id="2ff6c-293">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ff6c-293">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ff6c-294">**Tulajdonság**:  **letöltése:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="2ff6c-294">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="2ff6c-295">**További lépések**: vizsgálja meg, miért nem sikerült letölteni a hello hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-295">**Next steps**: Investigate why hello download failed on hello node.</span></span>

## <a name="deployedservicepackage-system-health-reports"></a><span data-ttu-id="2ff6c-296">DeployedServicePackage rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="2ff6c-296">DeployedServicePackage system health reports</span></span>
<span data-ttu-id="2ff6c-297">**System.Hosting** hello hatóság az üzembe helyezett entitásokra.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-297">**System.Hosting** is hello authority on deployed entities.</span></span>

### <a name="service-package-activation"></a><span data-ttu-id="2ff6c-298">Szolgáltatás az alkalmazáscsomag aktiválása</span><span class="sxs-lookup"><span data-stu-id="2ff6c-298">Service package activation</span></span>
<span data-ttu-id="2ff6c-299">System.Hosting jelentések OK gombra, ha hello szolgáltatást az alkalmazáscsomag aktiválása hello csomóponton sikeres.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-299">System.Hosting reports as OK if hello service package activation on hello node is successful.</span></span> <span data-ttu-id="2ff6c-300">Ellenkező esetben azt egy hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-300">Otherwise, it reports an error.</span></span>

* <span data-ttu-id="2ff6c-301">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ff6c-301">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ff6c-302">**Tulajdonság**: aktiválás</span><span class="sxs-lookup"><span data-stu-id="2ff6c-302">**Property**: Activation</span></span>
* <span data-ttu-id="2ff6c-303">**További lépések**: vizsgálja meg, miért hello aktiválás sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-303">**Next steps**: Investigate why hello activation failed.</span></span>

### <a name="code-package-activation"></a><span data-ttu-id="2ff6c-304">Kód az alkalmazáscsomag aktiválása</span><span class="sxs-lookup"><span data-stu-id="2ff6c-304">Code package activation</span></span>
<span data-ttu-id="2ff6c-305">**System.Hosting** Ha hello aktiválás sikeres kód csomagonként OK jelenti.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-305">**System.Hosting** reports as OK for each code package if hello activation is successful.</span></span> <span data-ttu-id="2ff6c-306">Ha hello aktiválás sikertelen, jelenti a rendszer figyelmeztetést konfigurált módon.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-306">If hello activation fails, it reports a warning as configured.</span></span> <span data-ttu-id="2ff6c-307">Ha **CodePackage** tooactivate meghibásodik, vagy nagyobb, mint hello konfigurálva hibával leáll **CodePackageHealthErrorThreshold**, üzemeltető egy hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-307">If **CodePackage** fails tooactivate or terminates with an error greater than hello configured **CodePackageHealthErrorThreshold**, hosting reports an error.</span></span> <span data-ttu-id="2ff6c-308">A service-csomag kód több csomagot tartalmaz, az aktiválási jelentés minden egyes jön létre.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-308">If a service package contains multiple code packages, an activation report is generated for each one.</span></span>

* <span data-ttu-id="2ff6c-309">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ff6c-309">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ff6c-310">**Tulajdonság**: használ hello előtag **CodePackageActivation** hello kódcsomag és hello belépési pontot hello nevét tartalmazza, és  **CodePackageActivation:* CodePackageName*:*SetupEntryPoint/EntryPoint*** (például **CodePackageActivation:Code:SetupEntryPoint**)</span><span class="sxs-lookup"><span data-stu-id="2ff6c-310">**Property**: Uses hello prefix **CodePackageActivation** and contains hello name of hello code package and hello entry point as **CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint*** (for example, **CodePackageActivation:Code:SetupEntryPoint**)</span></span>

### <a name="service-type-registration"></a><span data-ttu-id="2ff6c-311">Szolgáltatási típus regisztrációs</span><span class="sxs-lookup"><span data-stu-id="2ff6c-311">Service type registration</span></span>
<span data-ttu-id="2ff6c-312">**System.Hosting** OK jelent, ha hello szolgáltatás típusának regisztrálása sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-312">**System.Hosting** reports as OK if hello service type has been registered successfully.</span></span> <span data-ttu-id="2ff6c-313">Az egy hibát jelez, ha hello regisztrációját nem végezték el, időben (használatával konfigurált **ServiceTypeRegistrationTimeout**).</span><span class="sxs-lookup"><span data-stu-id="2ff6c-313">It reports an error if hello registration wasn't done in time (as configured by using **ServiceTypeRegistrationTimeout**).</span></span> <span data-ttu-id="2ff6c-314">Ha hello futásidejű le van zárva, hello szolgáltatás típusának hello csomópont regisztrációját, és üzemeltetési hiányára.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-314">If hello runtime is closed, hello service type is unregistered from hello node and Hosting reports a warning.</span></span>

* <span data-ttu-id="2ff6c-315">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ff6c-315">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ff6c-316">**Tulajdonság**: használ hello előtag **ServiceTypeRegistration** és hello szolgáltatás környezettípus nevét tartalmazza (például **ServiceTypeRegistration:FileStoreServiceType**)</span><span class="sxs-lookup"><span data-stu-id="2ff6c-316">**Property**: Uses hello prefix **ServiceTypeRegistration** and contains hello service type name (for example, **ServiceTypeRegistration:FileStoreServiceType**)</span></span>

<span data-ttu-id="2ff6c-317">a következő példa hello jeleníti meg a megfelelő telepített szervizcsomag:</span><span class="sxs-lookup"><span data-stu-id="2ff6c-317">hello following example shows a healthy deployed service package:</span></span>

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
                             Description           : hello ServicePackage was activated successfully.
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
                             Description           : hello CodePackage was activated successfully.
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
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a><span data-ttu-id="2ff6c-318">Letöltés</span><span class="sxs-lookup"><span data-stu-id="2ff6c-318">Download</span></span>
<span data-ttu-id="2ff6c-319">**System.Hosting** egy hibát jelez, ha hello szolgáltatás csomag letöltése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-319">**System.Hosting** reports an error if hello service package download fails.</span></span>

* <span data-ttu-id="2ff6c-320">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ff6c-320">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ff6c-321">**Tulajdonság**:  **letöltése:*RolloutVersion***</span><span class="sxs-lookup"><span data-stu-id="2ff6c-321">**Property**: **Download:*RolloutVersion***</span></span>
* <span data-ttu-id="2ff6c-322">**További lépések**: vizsgálja meg, miért nem sikerült letölteni a hello hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-322">**Next steps**: Investigate why hello download failed on hello node.</span></span>

### <a name="upgrade-validation"></a><span data-ttu-id="2ff6c-323">Frissítésének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="2ff6c-323">Upgrade validation</span></span>
<span data-ttu-id="2ff6c-324">**System.Hosting** egy hibát jelez, ha hello frissítés során az érvényesítés meghiúsul, vagy ha hello sikertelen frissítés esetén hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2ff6c-324">**System.Hosting** reports an error if validation during hello upgrade fails or if hello upgrade fails on hello node.</span></span>

* <span data-ttu-id="2ff6c-325">**SourceId**: System.Hosting</span><span class="sxs-lookup"><span data-stu-id="2ff6c-325">**SourceId**: System.Hosting</span></span>
* <span data-ttu-id="2ff6c-326">**Tulajdonság**: használ hello előtag **FabricUpgradeValidation** és hello frissítési verziója</span><span class="sxs-lookup"><span data-stu-id="2ff6c-326">**Property**: Uses hello prefix **FabricUpgradeValidation** and contains hello upgrade version</span></span>
* <span data-ttu-id="2ff6c-327">**Leírás**: toohello hiba történt az mutat</span><span class="sxs-lookup"><span data-stu-id="2ff6c-327">**Description**: Points toohello error encountered</span></span>

## <a name="next-steps"></a><span data-ttu-id="2ff6c-328">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2ff6c-328">Next steps</span></span>
[<span data-ttu-id="2ff6c-329">A Service Fabric rendszerállapot-jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="2ff6c-329">View Service Fabric health reports</span></span>](service-fabric-view-entities-aggregated-health.md)

[<span data-ttu-id="2ff6c-330">Hogyan tooreport és ellenőrzés szolgáltatás állapotát</span><span class="sxs-lookup"><span data-stu-id="2ff6c-330">How tooreport and check service health</span></span>](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[<span data-ttu-id="2ff6c-331">Figyelése és diagnosztizálása helyileg szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2ff6c-331">Monitor and diagnose services locally</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="2ff6c-332">A Service Fabric-alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="2ff6c-332">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)


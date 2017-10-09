---
title: "aaaInduce Chaos a Service Fabric-fürtök |} Microsoft Docs"
description: "Van, és a fürt Analysis Service API-toomanage Chaos hello fürt használatával."
services: service-fabric
documentationcenter: .net
author: motanv
manager: anmola
editor: motanv
ms.assetid: 2bd13443-3478-4382-9a5a-1f6c6b32bfc9
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: motanv
ms.openlocfilehash: 7e87cae22645fc4ba52e258471d8f3a4ffdb1cce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a><span data-ttu-id="4d558-103">A Service Fabric-fürtök ellenőrzött Chaos idéz elő</span><span class="sxs-lookup"><span data-stu-id="4d558-103">Induce controlled Chaos in Service Fabric clusters</span></span>
<span data-ttu-id="4d558-104">Nagy méretű elosztott rendszerek, például a felhőalapú infrastruktúrák nem eredendően megbízhatóak.</span><span class="sxs-lookup"><span data-stu-id="4d558-104">Large-scale distributed systems like cloud infrastructures are inherently unreliable.</span></span> <span data-ttu-id="4d558-105">Az Azure Service Fabric lehetővé teszi, hogy a fejlesztők toowrite megbízható elosztott szolgáltatások egy nem megbízható infrastruktúrán.</span><span class="sxs-lookup"><span data-stu-id="4d558-105">Azure Service Fabric enables developers toowrite reliable distributed services on top of an unreliable infrastructure.</span></span> <span data-ttu-id="4d558-106">toowrite robusztus elosztott szolgáltatások egy nem megbízható infrastruktúrán, fejlesztők kell toobe képes tootest hello stabilitását szolgáltatásaik közben az alapul szolgáló megbízhatatlan infrastruktúra hello megy keresztül bonyolult Állapotváltások esedékes toofaults.</span><span class="sxs-lookup"><span data-stu-id="4d558-106">toowrite robust distributed services on top of an unreliable infrastructure, developers need toobe able tootest hello stability of their services while hello underlying unreliable infrastructure is going through complicated state transitions due toofaults.</span></span>

<span data-ttu-id="4d558-107">Hello [van, és a fürt Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (más néven a hiba az Analysis Services hello) lehetővé teszi a fejlesztők hello tooinduce hibákat tootest az szolgáltatásokba.</span><span class="sxs-lookup"><span data-stu-id="4d558-107">hello [Fault Injection and Cluster Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (also known as hello Fault Analysis Service) gives developers hello ability tooinduce faults tootest their services.</span></span> <span data-ttu-id="4d558-108">Ezek a megcélzott szimulált hibák, például [partíció újraindítása](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), hello leggyakoribb Állapotváltások megadásával segíthet.</span><span class="sxs-lookup"><span data-stu-id="4d558-108">These targeted simulated faults, like [restarting a partition](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), can help exercise hello most common state transitions.</span></span> <span data-ttu-id="4d558-109">Azonban célzott szimulált hibák definition vannak optimalizálva, és így nem teljesíti az hibák elhárítása, hogy másolatot csak a Állapotváltások rögzített előrejelzése, hosszúak és bonyolultak sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="4d558-109">However targeted simulated faults are biased by definition and thus may miss bugs that show up only in hard-to-predict, long and complicated sequence of state transitions.</span></span> <span data-ttu-id="4d558-110">Egy torzítatlan tesztelési Chaos is használhatja.</span><span class="sxs-lookup"><span data-stu-id="4d558-110">For an unbiased testing, you can use Chaos.</span></span>

<span data-ttu-id="4d558-111">Chaos időszakos, kihagyásos hibák (szabályos és ungraceful) hello fürt teljes szimulálja a hosszú idő alatt.</span><span class="sxs-lookup"><span data-stu-id="4d558-111">Chaos simulates periodic, interleaved faults (both graceful and ungraceful) throughout hello cluster over extended periods of time.</span></span> <span data-ttu-id="4d558-112">Miután konfigurálta a Chaos hello sebesség és a hibák hello típusú, megkezdheti a Chaos hibák generálása hello fürt és a szolgáltatások a C# vagy a Powershell API toostart keresztül.</span><span class="sxs-lookup"><span data-stu-id="4d558-112">Once you have configured Chaos with hello rate and hello kind of faults, you can start Chaos through C# or Powershell API toostart generating faults in hello cluster and in your services.</span></span> <span data-ttu-id="4d558-113">Chaos toorun konfigurálhatja egy adott időszakban (például egy órát), amely után Chaos automatikusan leáll, vagy (C# vagy Powershell) StopChaos API toostop hívása, tetszőleges időpontban.</span><span class="sxs-lookup"><span data-stu-id="4d558-113">You can configure Chaos toorun for a specified time period (for example, for one hour), after which Chaos stops automatically, or you can call StopChaos API (C# or Powershell) toostop it at any time.</span></span>

> [!NOTE]
> <span data-ttu-id="4d558-114">Az aktuális képernyőn Chaos kapott csak biztonságos hibák, ami azt jelenti, hogy külső hibák hello hiányában a kvórum elvesztése vagy adatvesztés soha nem történik.</span><span class="sxs-lookup"><span data-stu-id="4d558-114">In its current form, Chaos induces only safe faults, which implies that in hello absence of external faults a quorum loss, or data loss never occurs.</span></span>
>

<span data-ttu-id="4d558-115">Chaos futtatása közben különböző események hello pillanatban futtatása hello hello állapotát rögzítő hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4d558-115">While Chaos is running, it produces different events that capture hello state of hello run at hello moment.</span></span> <span data-ttu-id="4d558-116">Egy ExecutingFaultsEvent például, hogy Chaos úgy döntött, hogy a munkamenetben tooexecute összes hello hibákat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4d558-116">For example, an ExecutingFaultsEvent contains all hello faults that Chaos has decided tooexecute in that iteration.</span></span> <span data-ttu-id="4d558-117">Egy ValidationFailedEvent hello fürt hello ellenőrzése során talált érvényesítési hibája (állapotfigyelő és stabilitását hibát) hello részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4d558-117">A ValidationFailedEvent contains hello details of a validation failure (health or stability issues) that was found during hello validation of hello cluster.</span></span> <span data-ttu-id="4d558-118">Hello GetChaosReport API (C# vagy Powershell) tooget hello jelentés Chaos futtatásainak hívhat meg.</span><span class="sxs-lookup"><span data-stu-id="4d558-118">You can invoke hello GetChaosReport API (C# or Powershell) tooget hello report of Chaos runs.</span></span> <span data-ttu-id="4d558-119">Beolvasása őrzi meg ezeket az eseményeket egy [megbízható szótár](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), amelynek van két konfigurációban meghatározni csonkolása házirend: **MaxStoredChaosEventCount** (alapértelmezett értéke 25000) és **StoredActionCleanupIntervalInSeconds** (alapértelmezett értéke 3600).</span><span class="sxs-lookup"><span data-stu-id="4d558-119">These events get persisted in a [reliable dictionary](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), which has a truncation policy dictated by two configurations: **MaxStoredChaosEventCount** (default value is 25000) and **StoredActionCleanupIntervalInSeconds** (default value is 3600).</span></span> <span data-ttu-id="4d558-120">Minden *StoredActionCleanupIntervalInSeconds* Chaos ellenőrzések és az összes de hello legutóbbi *MaxStoredChaosEventCount* események, kiürítésekor hello megbízható szótárból.</span><span class="sxs-lookup"><span data-stu-id="4d558-120">Every *StoredActionCleanupIntervalInSeconds* Chaos checks and all but hello most recent *MaxStoredChaosEventCount* events, are purged from hello reliable dictionary.</span></span>

## <a name="faults-induced-in-chaos"></a><span data-ttu-id="4d558-121">A Chaos előidézett hibák</span><span class="sxs-lookup"><span data-stu-id="4d558-121">Faults induced in Chaos</span></span>
<span data-ttu-id="4d558-122">Chaos állít elő hibák hello teljes Service Fabric-fürt között, és tömöríti olyan néhány órát a hónap vagy év láthatók.</span><span class="sxs-lookup"><span data-stu-id="4d558-122">Chaos generates faults across hello entire Service Fabric cluster and compresses faults that are seen in months or years into a few hours.</span></span> <span data-ttu-id="4d558-123">hello kombinációja kihagyásos hibák a magas hello gyakorisága, amelyek egyébként kimaradhatnak esetekben keresi.</span><span class="sxs-lookup"><span data-stu-id="4d558-123">hello combination of interleaved faults with hello high fault rate finds corner cases that may otherwise be missed.</span></span> <span data-ttu-id="4d558-124">Ebben a gyakorlatban az Chaos hello kód minőségi hello szolgáltatás tooa jelentős fejlesztéseket vezet.</span><span class="sxs-lookup"><span data-stu-id="4d558-124">This exercise of Chaos leads tooa significant improvement in hello code quality of hello service.</span></span>

<span data-ttu-id="4d558-125">Chaos kapott a következő kategóriák hello hibák:</span><span class="sxs-lookup"><span data-stu-id="4d558-125">Chaos induces faults from hello following categories:</span></span>

* <span data-ttu-id="4d558-126">A csomópont újraindítása</span><span class="sxs-lookup"><span data-stu-id="4d558-126">Restart a node</span></span>
* <span data-ttu-id="4d558-127">Indítsa újra a központilag telepített kódcsomag</span><span class="sxs-lookup"><span data-stu-id="4d558-127">Restart a deployed code package</span></span>
* <span data-ttu-id="4d558-128">Replika eltávolítása</span><span class="sxs-lookup"><span data-stu-id="4d558-128">Remove a replica</span></span>
* <span data-ttu-id="4d558-129">Indítsa újra a replikát</span><span class="sxs-lookup"><span data-stu-id="4d558-129">Restart a replica</span></span>
* <span data-ttu-id="4d558-130">Helyezze át egy elsődleges replika (konfigurálható)</span><span class="sxs-lookup"><span data-stu-id="4d558-130">Move a primary replica (configurable)</span></span>
* <span data-ttu-id="4d558-131">Helyezze át egy másodlagos másodpéldány (konfigurálható)</span><span class="sxs-lookup"><span data-stu-id="4d558-131">Move a secondary replica (configurable)</span></span>

<span data-ttu-id="4d558-132">Chaos többszöri futtatja.</span><span class="sxs-lookup"><span data-stu-id="4d558-132">Chaos runs in multiple iterations.</span></span> <span data-ttu-id="4d558-133">Mindegyik iteráció hibák áll, és a fürt ellenőrzése hello a megadott időszak.</span><span class="sxs-lookup"><span data-stu-id="4d558-133">Each iteration consists of faults and cluster validation for hello specified period.</span></span> <span data-ttu-id="4d558-134">Hello fürt toostabilize és érvényesítési toosucceed hello idő is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="4d558-134">You can configure hello time spent for hello cluster toostabilize and for validation toosucceed.</span></span> <span data-ttu-id="4d558-135">Ha a fürt ellenőrzése hibát tartalmaz, a Chaos hoz létre, és továbbra is fennáll a ValidationFailedEvent hello UTC időbélyeg és hello hiba részletei.</span><span class="sxs-lookup"><span data-stu-id="4d558-135">If a failure is found in cluster validation, Chaos generates and persists a ValidationFailedEvent with hello UTC timestamp and hello failure details.</span></span> <span data-ttu-id="4d558-136">Vegye figyelembe például három egyidejű hibák legfeljebb egy órával toorun be Chaos példánya.</span><span class="sxs-lookup"><span data-stu-id="4d558-136">For example, consider an instance of Chaos that is set toorun for an hour with a maximum of three concurrent faults.</span></span> <span data-ttu-id="4d558-137">Chaos három hibák kapott, és majd a hello fürt állapotát ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="4d558-137">Chaos induces three faults, and then validates hello cluster health.</span></span> <span data-ttu-id="4d558-138">Az telepítéseket hello előző lépést, amíg explicit módon leállt keresztül hello StopChaosAsync API vagy egy óra továbbítja.</span><span class="sxs-lookup"><span data-stu-id="4d558-138">It iterates through hello previous step until it is explicitly stopped through hello StopChaosAsync API or one-hour passes.</span></span> <span data-ttu-id="4d558-139">Ha hello fürt akkor kerül sérült állapotba, az ismétlések (Ez azt jelenti, hogy azt nem stabilizálásához hello átadott-a MaxClusterStabilizationTimeout belül) Chaos egy ValidationFailedEvent állít elő.</span><span class="sxs-lookup"><span data-stu-id="4d558-139">If hello cluster becomes unhealthy in any iteration (that is, it does not stabilize within hello passed-in MaxClusterStabilizationTimeout), Chaos generates a ValidationFailedEvent.</span></span> <span data-ttu-id="4d558-140">Az esemény azt jelzi, hogy valami nem megfelelő állapotba került, és előfordulhat, hogy további vizsgálat szükséges.</span><span class="sxs-lookup"><span data-stu-id="4d558-140">This event indicates that something has gone wrong and might need further investigation.</span></span>

<span data-ttu-id="4d558-141">amely előidézett Chaos hibákat tooget, használhatja GetChaosReport API-t (powershell vagy C#).</span><span class="sxs-lookup"><span data-stu-id="4d558-141">tooget which faults Chaos induced, you can use GetChaosReport API (powershell or C#).</span></span> <span data-ttu-id="4d558-142">hello API lekérdezi a következő szegmens hello hello Chaos jelentés hello átadott-a folytatási kód vagy hello átadott-időtartomány alapján.</span><span class="sxs-lookup"><span data-stu-id="4d558-142">hello API gets hello next segment of hello Chaos report based on hello passed-in continuation token or hello passed-in time-range.</span></span> <span data-ttu-id="4d558-143">Megadható ContinuationToken tooget hello következő szegmens hello Chaos jelentés hello vagy StartTimeUtc és EndTimeUtc hello-időtartományt adhat meg, de nem adható meg mind a hello ContinuationToken, és a hello időtartomány hello az adott hívásban.</span><span class="sxs-lookup"><span data-stu-id="4d558-143">You can either specify hello ContinuationToken tooget hello next segment of hello Chaos report or you can specify hello time-range through StartTimeUtc and EndTimeUtc, but you cannot specify both hello ContinuationToken and hello time-range in hello same call.</span></span> <span data-ttu-id="4d558-144">Ha több mint 100 Chaos események, hello Chaos jelentés eredmény abban az esetben szegmensek Ha a szegmens ne legyen nagyobb 100 Chaos események tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4d558-144">When there are more than 100 Chaos events, hello Chaos report is returned in segments where a segment contains no more than 100 Chaos events.</span></span>

## <a name="important-configuration-options"></a><span data-ttu-id="4d558-145">Fontos konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="4d558-145">Important configuration options</span></span>
* <span data-ttu-id="4d558-146">**TimeToRun**: teljes idő, hogy Chaos fut, mielőtt befejezné sikerrel.</span><span class="sxs-lookup"><span data-stu-id="4d558-146">**TimeToRun**: Total time that Chaos runs before it finishes with success.</span></span> <span data-ttu-id="4d558-147">Le is Chaos hello TimeToRun időszak keresztül hello StopChaos API futása előtt.</span><span class="sxs-lookup"><span data-stu-id="4d558-147">You can stop Chaos before it has run for hello TimeToRun period through hello StopChaos API.</span></span>

* <span data-ttu-id="4d558-148">**MaxClusterStabilizationTimeout**: hello idő toowait hello fürt toobecome kifogástalan egy ValidationFailedEvent létrehozása előtt a maximális mennyisége.</span><span class="sxs-lookup"><span data-stu-id="4d558-148">**MaxClusterStabilizationTimeout**: hello maximum amount of time toowait for hello cluster toobecome healthy before producing a ValidationFailedEvent.</span></span> <span data-ttu-id="4d558-149">A Várakozás hello fürt tooreduce hello terhelése, míg a helyreállítja azt.</span><span class="sxs-lookup"><span data-stu-id="4d558-149">This wait is tooreduce hello load on hello cluster while it is recovering.</span></span> <span data-ttu-id="4d558-150">hello elvégzett a következők:</span><span class="sxs-lookup"><span data-stu-id="4d558-150">hello checks performed are:</span></span>
  * <span data-ttu-id="4d558-151">Ha hello fürt egészségügyi OK</span><span class="sxs-lookup"><span data-stu-id="4d558-151">If hello cluster health is OK</span></span>
  * <span data-ttu-id="4d558-152">Ha hello szolgáltatás állapota rendben</span><span class="sxs-lookup"><span data-stu-id="4d558-152">If hello service health is OK</span></span>
  * <span data-ttu-id="4d558-153">Ha hello cél replika méretének beállítása hello szolgáltatás partíció érhető el</span><span class="sxs-lookup"><span data-stu-id="4d558-153">If hello target replica set size is achieved for hello service partition</span></span>
  * <span data-ttu-id="4d558-154">Hogy létezik-e nem található InBuild replikák</span><span class="sxs-lookup"><span data-stu-id="4d558-154">That no InBuild replicas exist</span></span>
* <span data-ttu-id="4d558-155">**MaxConcurrentFaults**: hello, amely minden egyes ismétlés előidézett egyidejű hibák maximális száma.</span><span class="sxs-lookup"><span data-stu-id="4d558-155">**MaxConcurrentFaults**: hello maximum number of concurrent faults that are induced in each iteration.</span></span> <span data-ttu-id="4d558-156">hello nagyobb hello szám, hello szigorúbb Chaos és hello átvételek és hello állapot átmenet kombinációit, amelyek fürt hello végighalad összetettebbek is.</span><span class="sxs-lookup"><span data-stu-id="4d558-156">hello higher hello number, hello more aggressive Chaos is and hello failovers and hello state transition combinations that hello cluster goes through are also more complex.</span></span> 

> [!NOTE]
> <span data-ttu-id="4d558-157">Attól függetlenül történik hogyan nagy érték *MaxConcurrentFaults* rendelkezik, Chaos biztosítja, hogy - külső hibák - hello hiányában nincs kvórum elvesztése vagy adatvesztés.</span><span class="sxs-lookup"><span data-stu-id="4d558-157">Regardless how high a value *MaxConcurrentFaults* has, Chaos guarantees - in hello absence of external faults - there is no quorum loss or data loss.</span></span>
>

* <span data-ttu-id="4d558-158">**EnableMoveReplicaFaults**: engedélyezheti vagy letilthatja a hello elsődleges vagy másodlagos replikák toomove okozó hello hibák.</span><span class="sxs-lookup"><span data-stu-id="4d558-158">**EnableMoveReplicaFaults**: Enables or disables hello faults that cause hello primary or secondary replicas toomove.</span></span> <span data-ttu-id="4d558-159">Ezek a hibák alapesetben le vannak tiltva.</span><span class="sxs-lookup"><span data-stu-id="4d558-159">These faults are disabled by default.</span></span>
* <span data-ttu-id="4d558-160">**WaitTimeBetweenIterations**: ismétlések közötti idő toowait hello mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="4d558-160">**WaitTimeBetweenIterations**: hello amount of time toowait between iterations.</span></span> <span data-ttu-id="4d558-161">Ez azt jelenti, hogy hello idő Chaos felfüggeszti a hibák, és hogy hello megfelelő hello fürt hello állapotának ellenőrzése befejeződött egy ciklikus végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="4d558-161">That is, hello amount of time Chaos will pause after having executed a round of faults and having finished hello corresponding validation of hello health of hello cluster.</span></span> <span data-ttu-id="4d558-162">nagyobb hello érték hello alacsonyabb hello hello átlagos injektálási gyakorisága.</span><span class="sxs-lookup"><span data-stu-id="4d558-162">hello higher hello value, hello lower is hello average fault injection rate.</span></span>
* <span data-ttu-id="4d558-163">**WaitTimeBetweenFaults**: hello mennyisége idő toowait egy egyetlen munkamenetben két egymást követő hibák között.</span><span class="sxs-lookup"><span data-stu-id="4d558-163">**WaitTimeBetweenFaults**: hello amount of time toowait between two consecutive faults in a single iteration.</span></span> <span data-ttu-id="4d558-164">hello nagyobb hello érték, alacsonyabb hello egyidejűségi beállítása pedig hello (vagy közötti átfedés hello) hibák.</span><span class="sxs-lookup"><span data-stu-id="4d558-164">hello higher hello value, hello lower hello concurrency of (or hello overlap between) faults.</span></span>
* <span data-ttu-id="4d558-165">**ClusterHealthPolicy**: fürt állapotházirend hello fürt Between Chaos ismétlési használt toovalidate hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="4d558-165">**ClusterHealthPolicy**: Cluster health policy is used toovalidate hello health of hello cluster in between Chaos iterations.</span></span> <span data-ttu-id="4d558-166">Ha hello fürt állapotfigyelő hibás vagy váratlan kivétel során hiba történik, ha Chaos hello tovább-állapotellenőrzése - tooprovide hello fürt bizonyos idő toorecuperate 30 percet várakozik.</span><span class="sxs-lookup"><span data-stu-id="4d558-166">If hello cluster health is in error or if an unexpected exception happens during fault execution, Chaos will wait for 30 minutes before hello next health-check - tooprovide hello cluster with some time toorecuperate.</span></span>
* <span data-ttu-id="4d558-167">**A környezetben**: (karakterlánc, karakterlánc) gyűjteménye, írja be kulcs-érték párokat.</span><span class="sxs-lookup"><span data-stu-id="4d558-167">**Context**: A collection of (string, string) type key-value pairs.</span></span> <span data-ttu-id="4d558-168">hello térkép lehet hello Chaos futtatásához használt toorecord információ.</span><span class="sxs-lookup"><span data-stu-id="4d558-168">hello map can be used toorecord information about hello Chaos run.</span></span> <span data-ttu-id="4d558-169">Nem lehet 100-nál több ilyen párok, és minden karakterlánc (kulcs vagy érték), legfeljebb 4095 karakter hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="4d558-169">There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.</span></span> <span data-ttu-id="4d558-170">Ez a térkép hello alapszintű hello futtatása Chaos toooptionally tároló hello környezet kapcsolatos hello adott futtatása állítja be.</span><span class="sxs-lookup"><span data-stu-id="4d558-170">This map is set by hello starter of hello Chaos run toooptionally store hello context about hello specific run.</span></span>

## <a name="how-toorun-chaos"></a><span data-ttu-id="4d558-171">Hogyan toorun Chaos</span><span class="sxs-lookup"><span data-stu-id="4d558-171">How toorun Chaos</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in hello cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.Add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit hello loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```

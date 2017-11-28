---
title: "aaaCreate chaos és a feladatátvételi teszt Azure mikroszolgáltatások |} Microsoft Docs"
description: "Hello Service Fabric chaos vizsgálati és a feladatátvételi teszt forgatókönyvek tooinduce hibák, és ellenőrizze a szolgáltatások hello megbízhatóságát."
services: service-fabric
documentationcenter: .net
author: motanv
manager: rsinha
editor: toddabel
ms.assetid: 8eee7e89-404a-4605-8f00-7e4d4fb17553
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv
ms.openlocfilehash: 1cac4f9e0e4a6c8416d5220d1537b5110decd1f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="testability-scenarios"></a><span data-ttu-id="6dfac-103">Tesztelhetőségi forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="6dfac-103">Testability scenarios</span></span>
<span data-ttu-id="6dfac-104">Nagy méretű elosztott rendszerek hasonlóan a felhőalapú infrastruktúrák nem eredendően megbízhatóak.</span><span class="sxs-lookup"><span data-stu-id="6dfac-104">Large distributed systems like cloud infrastructures are inherently unreliable.</span></span> <span data-ttu-id="6dfac-105">Az Azure Service Fabric által biztosított fejlesztők hello képességét toowrite szolgáltatások toorun megbízhatatlan infrastruktúrák felett.</span><span class="sxs-lookup"><span data-stu-id="6dfac-105">Azure Service Fabric gives developers hello ability toowrite services toorun on top of unreliable infrastructures.</span></span> <span data-ttu-id="6dfac-106">Rendelés toowrite magas színvonalú szolgáltatásokat a fejlesztők kell toobe képes tooinduce a szolgáltatások ilyen megbízhatatlan infrastruktúra tootest hello stabilitását.</span><span class="sxs-lookup"><span data-stu-id="6dfac-106">In order toowrite high-quality services, developers need toobe able tooinduce such unreliable infrastructure tootest hello stability of their services.</span></span>

<span data-ttu-id="6dfac-107">hello hiba az Analysis Services hello képességét tooinduce tartalék műveletek tootest szolgáltatások adja meg a fejlesztők hibák hello jelenlétét.</span><span class="sxs-lookup"><span data-stu-id="6dfac-107">hello Fault Analysis Service gives developers hello ability tooinduce fault actions tootest services in hello presence of failures.</span></span> <span data-ttu-id="6dfac-108">Azonban célzott szimulált hibákat kap, csak eddig.</span><span class="sxs-lookup"><span data-stu-id="6dfac-108">However, targeted simulated faults will get you only so far.</span></span> <span data-ttu-id="6dfac-109">továbbá tesztelési tootake hello hello tesztesetek használhatja a Service Fabric: egy chaos teszt- és a feladatátvételi tesztet.</span><span class="sxs-lookup"><span data-stu-id="6dfac-109">tootake hello testing further, you can use hello test scenarios in Service Fabric: a chaos test and a failover test.</span></span> <span data-ttu-id="6dfac-110">Ezek a forgatókönyvek folyamatos kihagyásos hibák, a szabályos és a hosszú időn keresztül a hello fürt teljes ungraceful szimulálásához.</span><span class="sxs-lookup"><span data-stu-id="6dfac-110">These scenarios simulate continuous interleaved faults, both graceful and ungraceful, throughout hello cluster over extended periods of time.</span></span> <span data-ttu-id="6dfac-111">Miután egy teszt hello sebesség és a hibák típusú van beállítva, C# API-k vagy PowerShell, toogenerate hibák hello fürt és a szolgáltatás elindítható.</span><span class="sxs-lookup"><span data-stu-id="6dfac-111">Once a test is configured with hello rate and kind of faults, it can be started through either C# APIs or PowerShell, toogenerate faults in hello cluster and your service.</span></span>

> [!WARNING]
> <span data-ttu-id="6dfac-112">ChaosTestScenario rugalmasabb, szolgáltatás-alapú Chaos lesz cserélve.</span><span class="sxs-lookup"><span data-stu-id="6dfac-112">ChaosTestScenario is being replaced by a more resilient, service-based Chaos.</span></span> <span data-ttu-id="6dfac-113">Tekintse meg az új cikkek toohello [szabályozott Chaos](service-fabric-controlled-chaos.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="6dfac-113">Please refer toohello new article [Controlled Chaos](service-fabric-controlled-chaos.md) for more details.</span></span>
> 
> 

## <a name="chaos-test"></a><span data-ttu-id="6dfac-114">Chaos tesztelése</span><span class="sxs-lookup"><span data-stu-id="6dfac-114">Chaos test</span></span>
<span data-ttu-id="6dfac-115">hello chaos forgatókönyv hello teljes Service Fabric-fürt között állít elő hibák.</span><span class="sxs-lookup"><span data-stu-id="6dfac-115">hello chaos scenario generates faults across hello entire Service Fabric cluster.</span></span> <span data-ttu-id="6dfac-116">hello forgatókönyv tömöríti a hibák általában látható hónapokban vagy években tooa órát.</span><span class="sxs-lookup"><span data-stu-id="6dfac-116">hello scenario compresses faults generally seen in months or years tooa few hours.</span></span> <span data-ttu-id="6dfac-117">hello kombinációja kihagyásos hibák a magas hello gyakorisága, amelyek egyébként nem talált esetekben keresi.</span><span class="sxs-lookup"><span data-stu-id="6dfac-117">hello combination of interleaved faults with hello high fault rate finds corner cases that are otherwise missed.</span></span> <span data-ttu-id="6dfac-118">Ennek eredménye tooa jelentős fejlesztéseket hello kód minőségi hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6dfac-118">This leads tooa significant improvement in hello code quality of hello service.</span></span>

### <a name="faults-simulated-in-hello-chaos-test"></a><span data-ttu-id="6dfac-119">Hello chaos teszt szimulált hibák</span><span class="sxs-lookup"><span data-stu-id="6dfac-119">Faults simulated in hello chaos test</span></span>
* <span data-ttu-id="6dfac-120">A csomópont újraindítása</span><span class="sxs-lookup"><span data-stu-id="6dfac-120">Restart a node</span></span>
* <span data-ttu-id="6dfac-121">Indítsa újra a központilag telepített kódcsomag</span><span class="sxs-lookup"><span data-stu-id="6dfac-121">Restart a deployed code package</span></span>
* <span data-ttu-id="6dfac-122">Replika eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6dfac-122">Remove a replica</span></span>
* <span data-ttu-id="6dfac-123">Indítsa újra a replikát</span><span class="sxs-lookup"><span data-stu-id="6dfac-123">Restart a replica</span></span>
* <span data-ttu-id="6dfac-124">(Opcionális) elsődleges replika áthelyezése</span><span class="sxs-lookup"><span data-stu-id="6dfac-124">Move a primary replica (optional)</span></span>
* <span data-ttu-id="6dfac-125">Helyezze át egy másodlagos másodpéldány (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="6dfac-125">Move a secondary replica (optional)</span></span>

<span data-ttu-id="6dfac-126">hello chaos teszt hibák többszöri fut, és fürt érvényesítést hello a megadott időn belül.</span><span class="sxs-lookup"><span data-stu-id="6dfac-126">hello chaos test runs multiple iterations of faults and cluster validations for hello specified period of time.</span></span> <span data-ttu-id="6dfac-127">hello hello fürt toostabilize és érvényesítési toosucceed töltött ideje is konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="6dfac-127">hello time spent for hello cluster toostabilize and for validation toosucceed is also configurable.</span></span> <span data-ttu-id="6dfac-128">hello forgatókönyv sikertelen lesz, amikor egy egyetlen hiba a fürtellenőrzés kattint.</span><span class="sxs-lookup"><span data-stu-id="6dfac-128">hello scenario fails when you hit a single failure in cluster validation.</span></span>

<span data-ttu-id="6dfac-129">Tegyük fel, hogy egy teszt toorun beállítása három egyidejű hibák legfeljebb egy óra.</span><span class="sxs-lookup"><span data-stu-id="6dfac-129">For example, consider a test set toorun for one hour with a maximum of three concurrent faults.</span></span> <span data-ttu-id="6dfac-130">hello teszt idéz elő a három hibák, és ezután ellenőrizze az hello fürt állapotát.</span><span class="sxs-lookup"><span data-stu-id="6dfac-130">hello test will induce three faults, and then validate hello cluster health.</span></span> <span data-ttu-id="6dfac-131">hello teszt hello előző lépésben algoritmusa keretein belül hello fürt akkor kerül sérült állapotba, vagy egy órán keresztül továbbítja.</span><span class="sxs-lookup"><span data-stu-id="6dfac-131">hello test will iterate through hello previous step till hello cluster becomes unhealthy or one hour passes.</span></span> <span data-ttu-id="6dfac-132">Ha hello fürt egyetlen munkamenetben nem kifogástalan, azaz azt nem stabilizálását a beállított időn belül, hello teszt sikertelen lesz, és kivételt.</span><span class="sxs-lookup"><span data-stu-id="6dfac-132">If hello cluster becomes unhealthy in any iteration, i.e. it does not stabilize within a configured time, hello test will fail with an exception.</span></span> <span data-ttu-id="6dfac-133">Ez a kivétel azt jelzi, hogy valami nem megfelelő állapotba került további vizsgálat szükséges.</span><span class="sxs-lookup"><span data-stu-id="6dfac-133">This exception indicates that something has gone wrong and needs further investigation.</span></span>

<span data-ttu-id="6dfac-134">Az aktuális képernyőn hello tartalék generációs motor hello chaos tesztben kapott csak biztonságos hibák.</span><span class="sxs-lookup"><span data-stu-id="6dfac-134">In its current form, hello fault generation engine in hello chaos test induces only safe faults.</span></span> <span data-ttu-id="6dfac-135">Ez azt jelenti, hogy a külső hibák hello hiányában egy kvórum vagy adatvesztés soha nem történik.</span><span class="sxs-lookup"><span data-stu-id="6dfac-135">This means that in hello absence of external faults, a quorum or data loss will never occur.</span></span>

### <a name="important-configuration-options"></a><span data-ttu-id="6dfac-136">Fontos konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="6dfac-136">Important configuration options</span></span>
* <span data-ttu-id="6dfac-137">**TimeToRun**: teljes ideje, hogy hello teszt lefuttatásával sikerrel befejezése előtt.</span><span class="sxs-lookup"><span data-stu-id="6dfac-137">**TimeToRun**: Total time that hello test will run before finishing with success.</span></span> <span data-ttu-id="6dfac-138">hello teszt be tudja fejezni korábbi helyett egy érvényesítési hiba.</span><span class="sxs-lookup"><span data-stu-id="6dfac-138">hello test can finish earlier in lieu of a validation failure.</span></span>
* <span data-ttu-id="6dfac-139">**MaxClusterStabilizationTimeout**: legfeljebb ennyi idő toowait a hello fürt toobecome kifogástalan hello teszt végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="6dfac-139">**MaxClusterStabilizationTimeout**: Maximum amount of time toowait for hello cluster toobecome healthy before failing hello test.</span></span> <span data-ttu-id="6dfac-140">hello elvégzett e a fürt állapota rendben, szolgáltatásának állapota rendben, hello replikakészlet célméretének teljesíteni az hello szolgáltatás partíció, és nem található InBuild replikák létezik.</span><span class="sxs-lookup"><span data-stu-id="6dfac-140">hello checks performed are whether cluster health is OK, service health is OK, hello target replica set size is achieved for hello service partition, and no InBuild replicas exist.</span></span>
* <span data-ttu-id="6dfac-141">**MaxConcurrentFaults**: minden egyes ismétlés előidézett egyidejű hibák maximális száma.</span><span class="sxs-lookup"><span data-stu-id="6dfac-141">**MaxConcurrentFaults**: Maximum number of concurrent faults induced in each iteration.</span></span> <span data-ttu-id="6dfac-142">hello nagyobb hello szám, hello szigorúbb hello törik, így összetettebb feladatátvétel és az átmenet kombinációját.</span><span class="sxs-lookup"><span data-stu-id="6dfac-142">hello higher hello number, hello more aggressive hello test, hence resulting in more complex failovers and transition combinations.</span></span> <span data-ttu-id="6dfac-143">hello teszt biztosítja, hogy külső hibák hiányában nem fogja a kvórum vagy adatvesztés, függetlenül attól, milyen magas van ebben a konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="6dfac-143">hello test guarantees that in absence of external faults there will not be a quorum or data loss, irrespective of how high this configuration is.</span></span>
* <span data-ttu-id="6dfac-144">**EnableMoveReplicaFaults**: engedélyezheti vagy letilthatja a okozó hello hello áthelyezésekor elsődleges vagy másodlagos replikák hello hibák.</span><span class="sxs-lookup"><span data-stu-id="6dfac-144">**EnableMoveReplicaFaults**: Enables or disables hello faults that are causing hello move of hello primary or secondary replicas.</span></span> <span data-ttu-id="6dfac-145">Ezek a hibák alapesetben le vannak tiltva.</span><span class="sxs-lookup"><span data-stu-id="6dfac-145">These faults are disabled by default.</span></span>
* <span data-ttu-id="6dfac-146">**WaitTimeBetweenIterations**: ismétlési, azaz egy ciklikus hibák és a megfelelő érvényesítése után közötti idő toowait mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="6dfac-146">**WaitTimeBetweenIterations**: Amount of time toowait between iterations, i.e. after a round of faults and corresponding validation.</span></span>

### <a name="how-toorun-hello-chaos-test"></a><span data-ttu-id="6dfac-147">Hogyan toorun hello chaos tesztelése</span><span class="sxs-lookup"><span data-stu-id="6dfac-147">How toorun hello chaos test</span></span>
<span data-ttu-id="6dfac-148">C#-minta</span><span class="sxs-lookup"><span data-stu-id="6dfac-148">C# sample</span></span>

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunChaosTestScenarioAsync(clusterConnection).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunChaosTestScenarioAsync(string clusterConnection)
    {
        TimeSpan maxClusterStabilizationTimeout = TimeSpan.FromSeconds(180);
        uint maxConcurrentFaults = 3;
        bool enableMoveReplicaFaults = true;

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // hello chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        ChaosTestScenarioParameters scenarioParameters = new ChaosTestScenarioParameters(
          maxClusterStabilizationTimeout,
          maxConcurrentFaults,
          enableMoveReplicaFaults,
          timeToRun);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create hello scenario class and execute it asynchronously.
        ChaosTestScenario chaosScenario = new ChaosTestScenario(fabricClient, scenarioParameters);

        try
        {
            await chaosScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```

<span data-ttu-id="6dfac-149">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6dfac-149">PowerShell</span></span>

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a><span data-ttu-id="6dfac-150">Feladatátvételi teszt</span><span class="sxs-lookup"><span data-stu-id="6dfac-150">Failover test</span></span>
<span data-ttu-id="6dfac-151">hello feladatátvételi tesztkörnyezet hello chaos tesztkörnyezet, amelynek célja egy adott szolgáltatás partíció egy verziója telepítve.</span><span class="sxs-lookup"><span data-stu-id="6dfac-151">hello failover test scenario is a version of hello chaos test scenario that targets a specific service partition.</span></span> <span data-ttu-id="6dfac-152">Hello hatással a feladatátvétel egy adott szolgáltatás partíció azt teszteli, miközben hello más szolgáltatások nem érinti.</span><span class="sxs-lookup"><span data-stu-id="6dfac-152">It tests hello effect of failover on a specific service partition while leaving hello other services unaffected.</span></span> <span data-ttu-id="6dfac-153">Hello cél partíciónak az adatait és egyéb paraméterek beállítása után fut egy szolgáltatás-partíció vagy a C# API-k, vagy a PowerShell toogenerate hibák használó ügyféloldali eszközként.</span><span class="sxs-lookup"><span data-stu-id="6dfac-153">Once it's configured with hello target partition information and other parameters, it runs as a client-side tool that uses either C# APIs or PowerShell toogenerate faults for a service partition.</span></span> <span data-ttu-id="6dfac-154">hello forgatókönyv telepítéseket szimulált hibák és a szolgáltatás érvényesítése során a munkaterhelések hello ügyféloldali tooprovide fut az üzleti logikát.</span><span class="sxs-lookup"><span data-stu-id="6dfac-154">hello scenario iterates through a sequence of simulated faults and service validation while your business logic runs on hello side tooprovide a workload.</span></span> <span data-ttu-id="6dfac-155">Szolgáltatás érvényesítési hiba azt jelzi, hogy a hibát, amely további vizsgálat szükséges.</span><span class="sxs-lookup"><span data-stu-id="6dfac-155">A failure in service validation indicates an issue that needs further investigation.</span></span>

### <a name="faults-simulated-in-hello-failover-test"></a><span data-ttu-id="6dfac-156">Hello feladatátvételi teszt szimulált hibák</span><span class="sxs-lookup"><span data-stu-id="6dfac-156">Faults simulated in hello failover test</span></span>
* <span data-ttu-id="6dfac-157">Indítsa újra a telepített kódcsomag, ahol hello partíció tárolása</span><span class="sxs-lookup"><span data-stu-id="6dfac-157">Restart a deployed code package where hello partition is hosted</span></span>
* <span data-ttu-id="6dfac-158">Egy elsődleges és másodlagos replika- vagy állapotmentes eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6dfac-158">Remove a primary/secondary replica or stateless instance</span></span>
* <span data-ttu-id="6dfac-159">Indítsa újra a másodlagos replikáról (Ha egy megőrzött szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="6dfac-159">Restart a primary secondary replica (if a persisted service)</span></span>
* <span data-ttu-id="6dfac-160">Helyezze át az elsődleges replika</span><span class="sxs-lookup"><span data-stu-id="6dfac-160">Move a primary replica</span></span>
* <span data-ttu-id="6dfac-161">Helyezze át egy másodlagos másodpéldány</span><span class="sxs-lookup"><span data-stu-id="6dfac-161">Move a secondary replica</span></span>
* <span data-ttu-id="6dfac-162">Indítsa újra a hello partíció</span><span class="sxs-lookup"><span data-stu-id="6dfac-162">Restart hello partition</span></span>

<span data-ttu-id="6dfac-163">hello feladatátvételi teszt választott hibát kapott, és majd érvényesítési futó hello szolgáltatás tooensure stabilitását.</span><span class="sxs-lookup"><span data-stu-id="6dfac-163">hello failover test induces a chosen fault and then runs validation on hello service tooensure its stability.</span></span> <span data-ttu-id="6dfac-164">hello feladatátvételi teszt csak egy időben, mint toopossible: fault több hibák hello chaos tesztben kapott.</span><span class="sxs-lookup"><span data-stu-id="6dfac-164">hello failover test induces only one fault at a time, as opposed toopossible multiple faults in hello chaos test.</span></span> <span data-ttu-id="6dfac-165">Hello teszt sikertelen, ha hello szolgáltatás partíció nem stabilizálásához hello beállított időkorláton belül minden egyes hiba után.</span><span class="sxs-lookup"><span data-stu-id="6dfac-165">If hello service partition does not stabilize within hello configured timeout after each fault, hello test fails.</span></span> <span data-ttu-id="6dfac-166">hello teszt csak biztonságos hibák kapott.</span><span class="sxs-lookup"><span data-stu-id="6dfac-166">hello test induces only safe faults.</span></span> <span data-ttu-id="6dfac-167">Ez azt jelenti, hogy külső hibák hiányában egy kvórum vagy adatvesztés történik.</span><span class="sxs-lookup"><span data-stu-id="6dfac-167">This means that in absence of external failures, a quorum or data loss will not occur.</span></span>

### <a name="important-configuration-options"></a><span data-ttu-id="6dfac-168">Fontos konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="6dfac-168">Important configuration options</span></span>
* <span data-ttu-id="6dfac-169">**Partitionselector osztályt**: választó objektum, amely meghatározza, amelyet a megcélzott toobe hello partíció.</span><span class="sxs-lookup"><span data-stu-id="6dfac-169">**PartitionSelector**: Selector object that specifies hello partition that needs toobe targeted.</span></span>
* <span data-ttu-id="6dfac-170">**TimeToRun**: teljes ideje, hogy hello teszt lefuttatásával befejezése előtt.</span><span class="sxs-lookup"><span data-stu-id="6dfac-170">**TimeToRun**: Total time that hello test will run before finishing.</span></span>
* <span data-ttu-id="6dfac-171">**MaxServiceStabilizationTimeout**: legfeljebb ennyi idő toowait a hello fürt toobecome kifogástalan hello teszt végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="6dfac-171">**MaxServiceStabilizationTimeout**: Maximum amount of time toowait for hello cluster toobecome healthy before failing hello test.</span></span> <span data-ttu-id="6dfac-172">hello elvégzett e szolgáltatásának állapota rendben, hello replikakészlet célméretének teljesíteni az összes partíció, és nem található InBuild replikák létezik.</span><span class="sxs-lookup"><span data-stu-id="6dfac-172">hello checks performed are whether service health is OK, hello target replica set size is achieved for all partitions, and no InBuild replicas exist.</span></span>
* <span data-ttu-id="6dfac-173">**WaitTimeBetweenFaults**: minden hiba és érvényesítési ciklus közötti idő toowait mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="6dfac-173">**WaitTimeBetweenFaults**: Amount of time toowait between every fault and validation cycle.</span></span>

### <a name="how-toorun-hello-failover-test"></a><span data-ttu-id="6dfac-174">Hogyan toorun hello feladatátvétel tesztelése</span><span class="sxs-lookup"><span data-stu-id="6dfac-174">How toorun hello failover test</span></span>
<span data-ttu-id="6dfac-175">**C#**</span><span class="sxs-lookup"><span data-stu-id="6dfac-175">**C#**</span></span>

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunFailoverTestScenarioAsync(clusterConnection, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunFailoverTestScenarioAsync(string clusterConnection, Uri serviceName)
    {
        TimeSpan maxServiceStabilizationTimeout = TimeSpan.FromSeconds(180);
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // hello chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        FailoverTestScenarioParameters scenarioParameters = new FailoverTestScenarioParameters(
          randomPartitionSelector,
          timeToRun,
          maxServiceStabilizationTimeout);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create hello scenario class and execute it asynchronously.
        FailoverTestScenario failoverScenario = new FailoverTestScenario(fabricClient, scenarioParameters);

        try
        {
            await failoverScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```


<span data-ttu-id="6dfac-176">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="6dfac-176">**PowerShell**</span></span>

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```

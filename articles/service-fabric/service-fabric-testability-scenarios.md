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
# <a name="testability-scenarios"></a>Tesztelhetőségi forgatókönyvek
Nagy méretű elosztott rendszerek hasonlóan a felhőalapú infrastruktúrák nem eredendően megbízhatóak. Az Azure Service Fabric által biztosított fejlesztők hello képességét toowrite szolgáltatások toorun megbízhatatlan infrastruktúrák felett. Rendelés toowrite magas színvonalú szolgáltatásokat a fejlesztők kell toobe képes tooinduce a szolgáltatások ilyen megbízhatatlan infrastruktúra tootest hello stabilitását.

hello hiba az Analysis Services hello képességét tooinduce tartalék műveletek tootest szolgáltatások adja meg a fejlesztők hibák hello jelenlétét. Azonban célzott szimulált hibákat kap, csak eddig. továbbá tesztelési tootake hello hello tesztesetek használhatja a Service Fabric: egy chaos teszt- és a feladatátvételi tesztet. Ezek a forgatókönyvek folyamatos kihagyásos hibák, a szabályos és a hosszú időn keresztül a hello fürt teljes ungraceful szimulálásához. Miután egy teszt hello sebesség és a hibák típusú van beállítva, C# API-k vagy PowerShell, toogenerate hibák hello fürt és a szolgáltatás elindítható.

> [!WARNING]
> ChaosTestScenario rugalmasabb, szolgáltatás-alapú Chaos lesz cserélve. Tekintse meg az új cikkek toohello [szabályozott Chaos](service-fabric-controlled-chaos.md) további részleteket.
> 
> 

## <a name="chaos-test"></a>Chaos tesztelése
hello chaos forgatókönyv hello teljes Service Fabric-fürt között állít elő hibák. hello forgatókönyv tömöríti a hibák általában látható hónapokban vagy években tooa órát. hello kombinációja kihagyásos hibák a magas hello gyakorisága, amelyek egyébként nem talált esetekben keresi. Ennek eredménye tooa jelentős fejlesztéseket hello kód minőségi hello szolgáltatást.

### <a name="faults-simulated-in-hello-chaos-test"></a>Hello chaos teszt szimulált hibák
* A csomópont újraindítása
* Indítsa újra a központilag telepített kódcsomag
* Replika eltávolítása
* Indítsa újra a replikát
* (Opcionális) elsődleges replika áthelyezése
* Helyezze át egy másodlagos másodpéldány (nem kötelező)

hello chaos teszt hibák többszöri fut, és fürt érvényesítést hello a megadott időn belül. hello hello fürt toostabilize és érvényesítési toosucceed töltött ideje is konfigurálható. hello forgatókönyv sikertelen lesz, amikor egy egyetlen hiba a fürtellenőrzés kattint.

Tegyük fel, hogy egy teszt toorun beállítása három egyidejű hibák legfeljebb egy óra. hello teszt idéz elő a három hibák, és ezután ellenőrizze az hello fürt állapotát. hello teszt hello előző lépésben algoritmusa keretein belül hello fürt akkor kerül sérült állapotba, vagy egy órán keresztül továbbítja. Ha hello fürt egyetlen munkamenetben nem kifogástalan, azaz azt nem stabilizálását a beállított időn belül, hello teszt sikertelen lesz, és kivételt. Ez a kivétel azt jelzi, hogy valami nem megfelelő állapotba került további vizsgálat szükséges.

Az aktuális képernyőn hello tartalék generációs motor hello chaos tesztben kapott csak biztonságos hibák. Ez azt jelenti, hogy a külső hibák hello hiányában egy kvórum vagy adatvesztés soha nem történik.

### <a name="important-configuration-options"></a>Fontos konfigurációs beállítások
* **TimeToRun**: teljes ideje, hogy hello teszt lefuttatásával sikerrel befejezése előtt. hello teszt be tudja fejezni korábbi helyett egy érvényesítési hiba.
* **MaxClusterStabilizationTimeout**: legfeljebb ennyi idő toowait a hello fürt toobecome kifogástalan hello teszt végrehajtása előtt. hello elvégzett e a fürt állapota rendben, szolgáltatásának állapota rendben, hello replikakészlet célméretének teljesíteni az hello szolgáltatás partíció, és nem található InBuild replikák létezik.
* **MaxConcurrentFaults**: minden egyes ismétlés előidézett egyidejű hibák maximális száma. hello nagyobb hello szám, hello szigorúbb hello törik, így összetettebb feladatátvétel és az átmenet kombinációját. hello teszt biztosítja, hogy külső hibák hiányában nem fogja a kvórum vagy adatvesztés, függetlenül attól, milyen magas van ebben a konfigurációban.
* **EnableMoveReplicaFaults**: engedélyezheti vagy letilthatja a okozó hello hello áthelyezésekor elsődleges vagy másodlagos replikák hello hibák. Ezek a hibák alapesetben le vannak tiltva.
* **WaitTimeBetweenIterations**: ismétlési, azaz egy ciklikus hibák és a megfelelő érvényesítése után közötti idő toowait mennyiségét.

### <a name="how-toorun-hello-chaos-test"></a>Hogyan toorun hello chaos tesztelése
C#-minta

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

PowerShell

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a>Feladatátvételi teszt
hello feladatátvételi tesztkörnyezet hello chaos tesztkörnyezet, amelynek célja egy adott szolgáltatás partíció egy verziója telepítve. Hello hatással a feladatátvétel egy adott szolgáltatás partíció azt teszteli, miközben hello más szolgáltatások nem érinti. Hello cél partíciónak az adatait és egyéb paraméterek beállítása után fut egy szolgáltatás-partíció vagy a C# API-k, vagy a PowerShell toogenerate hibák használó ügyféloldali eszközként. hello forgatókönyv telepítéseket szimulált hibák és a szolgáltatás érvényesítése során a munkaterhelések hello ügyféloldali tooprovide fut az üzleti logikát. Szolgáltatás érvényesítési hiba azt jelzi, hogy a hibát, amely további vizsgálat szükséges.

### <a name="faults-simulated-in-hello-failover-test"></a>Hello feladatátvételi teszt szimulált hibák
* Indítsa újra a telepített kódcsomag, ahol hello partíció tárolása
* Egy elsődleges és másodlagos replika- vagy állapotmentes eltávolítása
* Indítsa újra a másodlagos replikáról (Ha egy megőrzött szolgáltatás)
* Helyezze át az elsődleges replika
* Helyezze át egy másodlagos másodpéldány
* Indítsa újra a hello partíció

hello feladatátvételi teszt választott hibát kapott, és majd érvényesítési futó hello szolgáltatás tooensure stabilitását. hello feladatátvételi teszt csak egy időben, mint toopossible: fault több hibák hello chaos tesztben kapott. Hello teszt sikertelen, ha hello szolgáltatás partíció nem stabilizálásához hello beállított időkorláton belül minden egyes hiba után. hello teszt csak biztonságos hibák kapott. Ez azt jelenti, hogy külső hibák hiányában egy kvórum vagy adatvesztés történik.

### <a name="important-configuration-options"></a>Fontos konfigurációs beállítások
* **Partitionselector osztályt**: választó objektum, amely meghatározza, amelyet a megcélzott toobe hello partíció.
* **TimeToRun**: teljes ideje, hogy hello teszt lefuttatásával befejezése előtt.
* **MaxServiceStabilizationTimeout**: legfeljebb ennyi idő toowait a hello fürt toobecome kifogástalan hello teszt végrehajtása előtt. hello elvégzett e szolgáltatásának állapota rendben, hello replikakészlet célméretének teljesíteni az összes partíció, és nem található InBuild replikák létezik.
* **WaitTimeBetweenFaults**: minden hiba és érvényesítési ciklus közötti idő toowait mennyiségét.

### <a name="how-toorun-hello-failover-test"></a>Hogyan toorun hello feladatátvétel tesztelése
**C#**

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


**PowerShell**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```

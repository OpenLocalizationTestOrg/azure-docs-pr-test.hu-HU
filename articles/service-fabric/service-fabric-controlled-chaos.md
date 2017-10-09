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
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>A Service Fabric-fürtök ellenőrzött Chaos idéz elő
Nagy méretű elosztott rendszerek, például a felhőalapú infrastruktúrák nem eredendően megbízhatóak. Az Azure Service Fabric lehetővé teszi, hogy a fejlesztők toowrite megbízható elosztott szolgáltatások egy nem megbízható infrastruktúrán. toowrite robusztus elosztott szolgáltatások egy nem megbízható infrastruktúrán, fejlesztők kell toobe képes tootest hello stabilitását szolgáltatásaik közben az alapul szolgáló megbízhatatlan infrastruktúra hello megy keresztül bonyolult Állapotváltások esedékes toofaults.

Hello [van, és a fürt Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (más néven a hiba az Analysis Services hello) lehetővé teszi a fejlesztők hello tooinduce hibákat tootest az szolgáltatásokba. Ezek a megcélzott szimulált hibák, például [partíció újraindítása](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), hello leggyakoribb Állapotváltások megadásával segíthet. Azonban célzott szimulált hibák definition vannak optimalizálva, és így nem teljesíti az hibák elhárítása, hogy másolatot csak a Állapotváltások rögzített előrejelzése, hosszúak és bonyolultak sorrendjét. Egy torzítatlan tesztelési Chaos is használhatja.

Chaos időszakos, kihagyásos hibák (szabályos és ungraceful) hello fürt teljes szimulálja a hosszú idő alatt. Miután konfigurálta a Chaos hello sebesség és a hibák hello típusú, megkezdheti a Chaos hibák generálása hello fürt és a szolgáltatások a C# vagy a Powershell API toostart keresztül. Chaos toorun konfigurálhatja egy adott időszakban (például egy órát), amely után Chaos automatikusan leáll, vagy (C# vagy Powershell) StopChaos API toostop hívása, tetszőleges időpontban.

> [!NOTE]
> Az aktuális képernyőn Chaos kapott csak biztonságos hibák, ami azt jelenti, hogy külső hibák hello hiányában a kvórum elvesztése vagy adatvesztés soha nem történik.
>

Chaos futtatása közben különböző események hello pillanatban futtatása hello hello állapotát rögzítő hoz létre. Egy ExecutingFaultsEvent például, hogy Chaos úgy döntött, hogy a munkamenetben tooexecute összes hello hibákat tartalmaz. Egy ValidationFailedEvent hello fürt hello ellenőrzése során talált érvényesítési hibája (állapotfigyelő és stabilitását hibát) hello részleteit tartalmazza. Hello GetChaosReport API (C# vagy Powershell) tooget hello jelentés Chaos futtatásainak hívhat meg. Beolvasása őrzi meg ezeket az eseményeket egy [megbízható szótár](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), amelynek van két konfigurációban meghatározni csonkolása házirend: **MaxStoredChaosEventCount** (alapértelmezett értéke 25000) és **StoredActionCleanupIntervalInSeconds** (alapértelmezett értéke 3600). Minden *StoredActionCleanupIntervalInSeconds* Chaos ellenőrzések és az összes de hello legutóbbi *MaxStoredChaosEventCount* események, kiürítésekor hello megbízható szótárból.

## <a name="faults-induced-in-chaos"></a>A Chaos előidézett hibák
Chaos állít elő hibák hello teljes Service Fabric-fürt között, és tömöríti olyan néhány órát a hónap vagy év láthatók. hello kombinációja kihagyásos hibák a magas hello gyakorisága, amelyek egyébként kimaradhatnak esetekben keresi. Ebben a gyakorlatban az Chaos hello kód minőségi hello szolgáltatás tooa jelentős fejlesztéseket vezet.

Chaos kapott a következő kategóriák hello hibák:

* A csomópont újraindítása
* Indítsa újra a központilag telepített kódcsomag
* Replika eltávolítása
* Indítsa újra a replikát
* Helyezze át egy elsődleges replika (konfigurálható)
* Helyezze át egy másodlagos másodpéldány (konfigurálható)

Chaos többszöri futtatja. Mindegyik iteráció hibák áll, és a fürt ellenőrzése hello a megadott időszak. Hello fürt toostabilize és érvényesítési toosucceed hello idő is konfigurálhat. Ha a fürt ellenőrzése hibát tartalmaz, a Chaos hoz létre, és továbbra is fennáll a ValidationFailedEvent hello UTC időbélyeg és hello hiba részletei. Vegye figyelembe például három egyidejű hibák legfeljebb egy órával toorun be Chaos példánya. Chaos három hibák kapott, és majd a hello fürt állapotát ellenőrzi. Az telepítéseket hello előző lépést, amíg explicit módon leállt keresztül hello StopChaosAsync API vagy egy óra továbbítja. Ha hello fürt akkor kerül sérült állapotba, az ismétlések (Ez azt jelenti, hogy azt nem stabilizálásához hello átadott-a MaxClusterStabilizationTimeout belül) Chaos egy ValidationFailedEvent állít elő. Az esemény azt jelzi, hogy valami nem megfelelő állapotba került, és előfordulhat, hogy további vizsgálat szükséges.

amely előidézett Chaos hibákat tooget, használhatja GetChaosReport API-t (powershell vagy C#). hello API lekérdezi a következő szegmens hello hello Chaos jelentés hello átadott-a folytatási kód vagy hello átadott-időtartomány alapján. Megadható ContinuationToken tooget hello következő szegmens hello Chaos jelentés hello vagy StartTimeUtc és EndTimeUtc hello-időtartományt adhat meg, de nem adható meg mind a hello ContinuationToken, és a hello időtartomány hello az adott hívásban. Ha több mint 100 Chaos események, hello Chaos jelentés eredmény abban az esetben szegmensek Ha a szegmens ne legyen nagyobb 100 Chaos események tartalmaz.

## <a name="important-configuration-options"></a>Fontos konfigurációs beállítások
* **TimeToRun**: teljes idő, hogy Chaos fut, mielőtt befejezné sikerrel. Le is Chaos hello TimeToRun időszak keresztül hello StopChaos API futása előtt.

* **MaxClusterStabilizationTimeout**: hello idő toowait hello fürt toobecome kifogástalan egy ValidationFailedEvent létrehozása előtt a maximális mennyisége. A Várakozás hello fürt tooreduce hello terhelése, míg a helyreállítja azt. hello elvégzett a következők:
  * Ha hello fürt egészségügyi OK
  * Ha hello szolgáltatás állapota rendben
  * Ha hello cél replika méretének beállítása hello szolgáltatás partíció érhető el
  * Hogy létezik-e nem található InBuild replikák
* **MaxConcurrentFaults**: hello, amely minden egyes ismétlés előidézett egyidejű hibák maximális száma. hello nagyobb hello szám, hello szigorúbb Chaos és hello átvételek és hello állapot átmenet kombinációit, amelyek fürt hello végighalad összetettebbek is. 

> [!NOTE]
> Attól függetlenül történik hogyan nagy érték *MaxConcurrentFaults* rendelkezik, Chaos biztosítja, hogy - külső hibák - hello hiányában nincs kvórum elvesztése vagy adatvesztés.
>

* **EnableMoveReplicaFaults**: engedélyezheti vagy letilthatja a hello elsődleges vagy másodlagos replikák toomove okozó hello hibák. Ezek a hibák alapesetben le vannak tiltva.
* **WaitTimeBetweenIterations**: ismétlések közötti idő toowait hello mennyiségét. Ez azt jelenti, hogy hello idő Chaos felfüggeszti a hibák, és hogy hello megfelelő hello fürt hello állapotának ellenőrzése befejeződött egy ciklikus végrehajtása után. nagyobb hello érték hello alacsonyabb hello hello átlagos injektálási gyakorisága.
* **WaitTimeBetweenFaults**: hello mennyisége idő toowait egy egyetlen munkamenetben két egymást követő hibák között. hello nagyobb hello érték, alacsonyabb hello egyidejűségi beállítása pedig hello (vagy közötti átfedés hello) hibák.
* **ClusterHealthPolicy**: fürt állapotházirend hello fürt Between Chaos ismétlési használt toovalidate hello állapotát. Ha hello fürt állapotfigyelő hibás vagy váratlan kivétel során hiba történik, ha Chaos hello tovább-állapotellenőrzése - tooprovide hello fürt bizonyos idő toorecuperate 30 percet várakozik.
* **A környezetben**: (karakterlánc, karakterlánc) gyűjteménye, írja be kulcs-érték párokat. hello térkép lehet hello Chaos futtatásához használt toorecord információ. Nem lehet 100-nál több ilyen párok, és minden karakterlánc (kulcs vagy érték), legfeljebb 4095 karakter hosszú lehet. Ez a térkép hello alapszintű hello futtatása Chaos toooptionally tároló hello környezet kapcsolatos hello adott futtatása állítja be.

## <a name="how-toorun-chaos"></a>Hogyan toorun Chaos

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

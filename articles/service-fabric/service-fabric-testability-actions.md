---
title: "az Azure mikroszolgáltatások aaaSimulate hibáinak |} Microsoft Docs"
description: "Ez a cikk beszél található a Microsoft Azure Service Fabric hello tesztelhetőségi műveletek."
services: service-fabric
documentationcenter: .net
author: motanv
manager: timlt
editor: toddabel
ms.assetid: ed53ca5c-4d5e-4b48-93c9-e386f32d8b7a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv;heeldin
ms.openlocfilehash: 5bdda1c0c5a40b243ab956c4791afd52e11c4089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="testability-actions"></a>Tesztelhetőségi műveletek
Rendelés toosimulate egy nem megbízható infrastruktúra, az Azure Service Fabric biztosít, hello fejlesztői módon toosimulate a különböző valós hibák és állapotváltozási adat áramlik. Ezek tesztelhetőségi műveletként érhetők el. hello műveletek vannak hello alacsony szintű API-k, amelyek egy adott van, az állapotváltás vagy az érvényesítés. Ezek a műveletek kombinálásával átfogó Tesztelési forgatókönyvek írhat a szolgáltatások.

A Service Fabric biztosít néhány gyakori tesztesetek tevődik össze ezeket a műveleteket. Erősen ajánlott, hogy használhatja a beépített forgatókönyvekben, gondosan kiválasztott tootest közös Állapotváltások és hiba esetekben. Műveletek azonban lehet használt toocreate egyéni Tesztelési forgatókönyvek, ha a forgatókönyvek, amelyek nem tartoznak hello beépített forgatókönyvek még vagy egyéni alkalmazás igazított tooadd érvényességének használni szeretne.

C# hello műveletek implementációja hello System.Fabric.dll szerelvény található. hello rendszer Fabric PowerShell-modul hello Microsoft.ServiceFabric.Powershell.dll szerelvény található. Runtime telepítésének részeként hello ServiceFabric PowerShell-modul a könnyű használatra telepített tooallow.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Biztonságos és ungraceful tartalék műveletek
Tesztelhetőségi műveletek két fő gyűjtők sorolhatók:

* Ungraceful hibák: ezek a hibák hibák gépek újraindításának például szimulálásához, és dolgozza fel az összeomlások. Ebben az esetben a hibákat hello végrehajtási környezet folyamat váratlanul leáll. Ez azt jelenti, hogy hello állapot nélküli karbantartása hello alkalmazás újra indítása előtt futtatható.
* Sikeres-e hibák: ezek a hibák szimulálása szabályos műveletek, például a replika helyezi át, és elhagyta a(z) terheléselosztást által indított. Ebben az esetben hello szolgáltatás lekérdezi egy értesítés, amely hello zárja be, és mielőtt kilépne hello állapot fel tudja szabadítani.

Jobb minőségű érvényesítéshez hello szolgáltatás futtatásához és az üzleti munkaterhelés közben, hogy a különböző szabályos és ungraceful hibák. Ungraceful hibák gyakorolja forgatókönyvek, ahol hello szolgáltatás folyamat váratlanul kilép néhány munkafolyamat hello közepén. A vizsgálatok hello helyreállítási elérési úton található hello szolgáltatás replika Service Fabric által történt visszaállítása után. Ez segít tesztelése az adatok konzisztenciájának, és hogy hello szolgáltatás állapotának karban kell hiba után. hello más hibák (hello szabályos hibák) készletét tesztelje, hogy helyesen hello szolgáltatás reagál a Service Fabric mozgatásának tooreplicas. Megszakítási kezelésének ellenőrzi a hello RunAsync metódusában. hello szolgáltatás toocheck igényeihez hello cancellation token alatt állítsa be megfelelően menteni az állapotát és kilépés hello RunAsync metódusában.

## <a name="testability-actions-list"></a>Tesztelhetőségi műveletek listája
| Műveletek | Leírás | Felügyelt API | PowerShell-parancsmag | Biztonságos/ungraceful hibák |
| --- | --- | --- | --- | --- |
| CleanTestState |Eltávolítja az összes hello teszt állapota hello fürt esetén a hibás leállítást hello teszt illesztőprogram. |CleanTestStateAsync |Remove-ServiceFabricTestState |Nem alkalmazható |
| InvokeDataLoss |Adatvesztés kapott egy szolgáltatás partícióra. |InvokeDataLossAsync |Invoke-ServiceFabricPartitionDataLoss |Biztonságos |
| InvokeQuorumLoss |Egy adott állapot-nyilvántartó szolgáltatása partíció elhelyezi a kvórum elvesztése. |InvokeQuorumLossAsync |Invoke-ServiceFabricQuorumLoss |Biztonságos |
| Elsődleges áthelyezése |A kurzor hello megadott állapotalapú szolgáltatási toohello megadott fürtcsomópont elsődleges másodpéldány. |MovePrimaryAsync |MOVE-ServiceFabricPrimaryReplica |Biztonságos |
| Másodlagos áthelyezése |Az állapotalapú szolgáltatás tooa másik fürtcsomópont másodlagos másodpéldány aktuális hello helyezi. |MoveSecondaryAsync |MOVE-ServiceFabricSecondaryReplica |Biztonságos |
| RemoveReplica |Replika hiba szimulálja által replika eltávolítása egy fürtből. Ez hello replika bezárul, és ismerhetik azt toorole "None", eltávolítja az összes szerinti hello fürtből. |RemoveReplicaAsync |Remove-ServiceFabricReplica |Biztonságos |
| RestartDeployedCodePackage |A kód csomag folyamat hibájának szimulálja egy egy fürt egy csomópontján telepített kódcsomag újraindításával. Ez a újraindul, hogy a folyamat minden hello felhasználói szolgáltatás üzemeltetett replikák hello kód csomag folyamat megszakítása. |RestartDeployedCodePackageAsync |Újraindítás-ServiceFabricDeployedCodePackage |Ungraceful |
| A RestartNode |A Service Fabric-fürt Csomóponthiba szimulálja egy csomópont újraindításával. |RestartNodeAsync |Újraindítás-ServiceFabricNode |Ungraceful |
| RestartPartition |A datacenter blackout vagy a fürt blackout forgatókönyv szimulálja néhány vagy az összes partíció replikák újraindításával. |RestartPartitionAsync |Restart-ServiceFabricPartition |Biztonságos |
| RestartReplica |A replika hiba szimulálja egy megőrzött replikát újraindításával fürtben, hello replika bezárása, és majd megnyitni. |RestartReplicaAsync |Újraindítás-ServiceFabricReplica |Biztonságos |
| A StartNode |A csomópont elindul egy fürt, amely már le van állítva. |StartNodeAsync |Start-ServiceFabricNode |Nem alkalmazható |
| Stopnode parancs |Egy Csomóponthiba szimulálja a fürt egyik csomópontjában levő leállításával. hello csomópont le maradnak, amíg StartNode nevezik. |StopNodeAsync |Stop-ServiceFabricNode |Ungraceful |
| ValidateApplication |Hello rendelkezésre állását és, hogy néhány tartalék hello rendszerbe után általában az alkalmazáson belül minden Service Fabric-szolgáltatás állapotát ellenőrzi. |ValidateApplicationAsync |Teszt-ServiceFabricApplication |Nem alkalmazható |
| ValidateService |Hello rendelkezésre állás és a Service Fabric-szolgáltatás állapotát ellenőrzi a általában után néhány tartalék hogy hello rendszerbe. |ValidateServiceAsync |Teszt-ServiceFabricService |Nem alkalmazható |

## <a name="running-a-testability-action-using-powershell"></a>PowerShell-lel tesztelhetőségi műveletek futtatása
Az oktatóanyag bemutatja, hogyan toorun egy tesztelhetőségi művelet PowerShell használatával. Megtudhatja, hogyan toorun egy tesztelhetőségi műveletet a helyi (1-box) fürt vagy egy Azure-fürttel. Microsoft.Fabric.Powershell.dll – hello Service Fabric PowerShell-modul – telepíti a rendszer automatikusan telepíti a Microsoft Service Fabric MSI hello. hello modul automatikusan betöltődik, amikor megnyit egy PowerShell-parancssorba.

Útmutató szegmensek:

* [Egy műveletet futtatni egy beépített fürt](#run-an-action-against-a-one-box-cluster)
* [Egy műveletet futtatni egy Azure-fürttel](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Egy műveletet futtatni egy beépített fürt
toorun egy tesztelhetőségi műveletet a helyi fürthöz, először kapcsolódik toohello fürt és a rendszergazda módban megnyitott hello PowerShell-parancssorba. Ossza meg velünk nézze meg hello **újraindítás-ServiceFabricNode** művelet.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Itt hello művelet **újraindítás-ServiceFabricNode** "1. csomópont" nevű csomópont futtatja. hello végrehajtási mód határozza meg, hogy azt nem ellenőrizze hogy hello újraindítás-csomópont művelet ténylegesen sikeres volt. Adja meg hello befejezési mint a "Hitelesítés" módnál azt tooverify hogy ténylegesen hello újraindítási művelete sikeresen befejeződött. Helyett a nevével adja meg közvetlenül hello csomópont, megadhatja azt a kulcsot és hello partíciófajta replika, az alábbiak szerint:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Újraindítás-ServiceFabricNode** kell használt toorestart a Service Fabric egy fürt csomópontja. Ezzel leállítja a hello Fabric.exe folyamat, amely újraindítja az összes hello rendszer szolgáltatás és a felhasználó szolgáltatás üzemeltetett replikák ezen a csomóponton. Az API tootest a szolgáltatás segítségével fedik le a hibák hello feladatátvételi helyreállítási elérési út mentén. Ennek segítségével szimulálhatja hello fürtben lévő csomópontok hibáit.

hello alábbi képernyőfelvételen látható hello **újraindítás-ServiceFabricNode** tesztelhetőségi parancs működés közben.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

hello az első kimeneti hello **Get-ServiceFabricNode** (hello Service Fabric PowerShell-modul a parancsmag) jeleníti meg, hogy hello helyi fürt az öt csomóponttal rendelkezik: Node.1 tooNode.5. Hello tesztelhetőségi művelet (parancsmag) után **újraindítás-ServiceFabricNode** hello csomóponton végrehajtása nevű Node.4, látható, hogy hello csomópont hasznos üzemidő alaphelyzetbe lett állítva.

### <a name="run-an-action-against-an-azure-cluster"></a>Egy műveletet futtatni egy Azure-fürttel
A helyi fürthöz hasonló toorunning hello műveletet tesztelhetőségi műveletek futtatása (a PowerShell használatával) egy Azure fürtön hajtották. hello egyetlen különbség az, hogy helyett csatlakozó toohello helyi fürthöz, a hello művelet futtatása előtt kell tooconnect toohello Azure először a fürt.

## <a name="running-a-testability-action-using-c35"></a>C &#35; használatával tesztelhetőségi műveletek futtatása
toorun egy tesztelhetőségi művelet C# használatával, előbb kell tooconnect toohello fürt FabricClient használatával. Beszereznie hello szükséges paramétereket toorun hello művelet. Különböző paraméterek használhatók toorun hello műveletet.
Megnézi hello RestartServiceFabricNode művelet, hello csomópont információk (a csomópont neve és csomópont azonosítója) segítségével van egyirányú toorun hello fürtben.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

A paraméter magyarázata:

* **CompleteMode** határozza meg, hogy hello üzemmód nem ellenőriznie kell, hogy sikerült-e ténylegesen hello újraindítási művelete. Adja meg hello befejezési mint a "Hitelesítés" módnál azt tooverify hogy ténylegesen hello újraindítási művelete sikeresen befejeződött.  
* **OperationTimeout** beállítása hello hello művelet toofinish idő, mielőtt egy TimeoutException kivétel történt.
* **CancellationToken** lehetővé teszi, hogy a függőben lévő hívás toobe megszakítva.

Adja meg közvetlenül a hello csomópont neve, azt a kulcsot és hello partíciófajta replika adhatja.

További információkért lásd: [partitionselector osztályt és replicaselector osztályt](#partition_replica_selector).

```csharp
// Add a reference tooSystem.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart hello node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way toorestart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>Partitionselector osztályt és replicaselector osztályt
### <a name="partitionselector"></a>Partitionselector osztályt
Partitionselector osztályt tesztelhetőségi felfedett segítő és van egy adott használt tooselect partícióazonosító mely tooperform a hello tesztelhetőségi műveletek. Egy adott partícióra használt tooselect lehet, ha hello Partícióazonosító előzetesen is ismert. Vagy megadhatja a hello partíciós kulcs, és hello művelet belső hello Partícióazonosító fel. Akkor is hello lehetőség kiválasztásával egy véletlenszerű partíció.

toouse a segítő létrehozása hello partitionselector osztályt objektum és hello partíció hello Select * módszerek egyikének használatával. Hello partitionselector osztályt objektum toohello API írja elő, akkor továbbítja. Ha nincs lehetőség van kiválasztva, tooa véletlenszerű partíció alapértelmezés szerint.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>Replicaselector osztályt
Replicaselector osztályt tesztelhetőségi felfedett segítő és van használt toohelp kiválasztani a replikát a mely tooperform hello tesztelhetőségi műveletek. Egy adott replika használt tooselect lehet, ha előzetesen ismert hello másodpéldány-azonosító. Ezenkívül hello lehetőség kiválasztásával egy elsődleges másodpéldány, vagy egy véletlenszerű másodlagos rendelkezik. Replicaselector osztályt a partitionselector osztályt osztályból származik, ezért meg kell tooselect mindkét hello replika és hello partíciót, amelyen tooperform hello tesztelhetőségi művelet kívánja.

a segítő toouse replicaselector osztályt objektum létrehozása, és állítsa be a kívánt módon hello tooselect hello replika- és hello partíció. Majd továbbíthatja azt hello API, amely igényli azt be. Ha nincs lehetőség van kiválasztva, tooa véletlenszerű replika és a véletlenszerű partíció alapértelmezés szerint.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select hello primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select hello replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Következő lépések
* [Tesztelhetőségi forgatókönyvek](service-fabric-testability-scenarios.md)
* Hogyan tootest a szolgáltatás
  * [Szolgáltatás-munkaterhelések során hibák szimulálása](service-fabric-testability-workload-tests.md)
  * [Szolgáltatások közötti kommunikációs hibái](service-fabric-testability-scenarios-service-communication.md)


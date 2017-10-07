---
title: "a Service Fabric-szolgáltatásokra adatvesztés aaaHow tooInvoke |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello adatvesztés api"
services: service-fabric
documentationcenter: .net
author: LMWF
manager: rsinha
editor: 
ms.assetid: f4e70f6f-cad9-4a3e-9655-009b4db09c6d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/19/2016
ms.author: lemai
redirect_url: /azure/service-fabric/service-fabric-testability-overview
ms.openlocfilehash: 014c7ebfd2c42d79a5fe1802ecc3fa0c1f26f9d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinvoke-data-loss-on-services"></a>Hogyan tooInvoke adatvesztéshez szolgáltatások
> [!WARNING]
> A dokumentum ismerteti hogyan toocause adatvesztés a szolgáltatásokban, és körültekintéssel kell alkalmazni.
> 
> 

## <a name="introduction"></a>Bevezetés
A Service Fabric-szolgáltatás hívó StartPartitionDataLossAsync() partíciók adatvesztés hívhat meg.  Ez az api hello van, és az Analysis Services tooperform hello munkahelyi toocause adatok elvesztését feltételek használja.

## <a name="using-hello-fault-injection-and-analysis-service"></a>Hello van, és az Analysis Services használatával
hello van, és az Analysis Services jelenleg a következő API-k az alábbi hello diagram hello.  hello diagram jobb oldalán hello hello megfelelő PowerShell-parancsmagot szemlélteti.  További információ az egyes tekintse meg az egyes API toohello az msdn dokumentációját.

| C# API | PowerShell-parancsmag |
| --- | ---:|
| [StartPartitionDataLossAsync][dl] |[Start-ServiceFabricPartitionDataLoss][psdl] |
| [StartPartitionQuorumLossAsync][ql] |[Start-ServiceFabricPartitionQuorumLoss][psql] |
| [StartPartitionRestartAsync][rp] |[Start-ServiceFabricPartitionRestart][psrp] |

## <a name="conceptual-overview-of-running-a-command"></a>A parancs futtatása elméleti áttekintése
egy aszinkron modellt az először hello parancsot egy API-val tooas hello "Start" API ebben a dokumentumban, majd ellenőrzi hello folyamatban van a parancs egy "GetProgress" API használatával, amíg el nem éri a Terminálszolgáltatások említett van, és az Analysis Services által használt hello állapotú, vagy amíg megszakítja a műveletet.
toostart egy parancs hello "Start" API hello megfelelő API-hívás.  Ez az API ad vissza, ha hello van, és az Analysis Services elfogadta hello kérelem.  Azonban azt nem jelzi, hogy a parancs futtatása, vagy még akkor is, ha még megkezdődött.  Rendelés toocheck a folyamatban lévő utasítás hello "GetProgress" API-t, amely megfelel a korábbi megnevezése toohello "Start" API hívása.  hello "GetProgress" API hello hello parancs belül az állapot tulajdonság aktuális állapotát jelző egy objektumot ad vissza.  A parancs futtatása határozatlan ideig, amíg:

1. Sikeresen befejeződött.  Meghívja a "GetProgress" rajta ebben az esetben, ha hello folyamatban objektum állapota befejeződik.
2. Helyreállíthatatlan hibába ütközik.  Meghívja a "GetProgress" rajta ebben az esetben, ha hello folyamatban objektum állapota hibás lesz
3. Hello keresztül Mégse [CancelTestCommandAsync] [ cancel] API-t vagy [Stop-ServiceFabricTestCommand] [ cancelps] PowerShell-parancsmagot.  Meghívja a "GetProgress" rajta ebben az esetben, ha hello folyamatban objektum állapota lesz megszakítva vagy ForceCancelled, attól függően, hogy egy argumentum toothat API.  A dokumentációban hello [CancelTestCommandAsync] [ cancel] további részleteket.

## <a name="details-of-running-a-command"></a>A parancs futtatása részleteit
A sorrend toostart parancsot hívja meg hello Start API hello várt argumentum.  Minden Start API rendelkezik egy OperationID azonosítójú nevű Guid-argumentum.  Meg kell nyomon követheti, hello OperationID azonosítójú argumentum, mert ez a parancs tootrack előrehaladását.  Ez a hello "GetProgress" API-hello parancs rendelés tootrack folyamatban kell átadni.  hello OperationID azonosítójú egyedinek kell lennie.

Hello Start API hello GetProgress API ciklust kell hívható, amíg hello visszaadott folyamatban sikeresen hívása után objektum State tulajdonsága befejeződött.  Minden [FabricTransientException tartozó] [ fte] és OperationCanceledException tartozó meg kell ismételni.
Hello parancs egy terminál állapota (befejezve, Faulted vagy megszakítva) elérésekor hello folyamatban objektum Result tulajdonságnak lesz további információt adott vissza.  Ha hello állapotban befejeződött, Result.SelectedPartition.PartitionId hello partícióazonosító kiválasztott fogja tartalmazni.  Result.Exception null értékű lesz.  Ha hello állapot hiba történt a működésében, Result.Exception hello OK hello van és a hibát jelzett az Analysis Services hello parancs lesz.  Result.SelectedPartition.PartitionId hello partícióazonosító kiválasztott fog rendelkezni.  Bizonyos esetekben hello parancs lehet, hogy nem rendelkezik kezdett sokkal elegendő egy partíción toochoose.  Ebben az esetben a hello PartitionId 0 lesz.  Ha hello állapot meg lett szakítva, Result.Exception nem null értékű.  Hello Faulted eset, például Result.SelectedPartition.PartitionId lesz a kiválasztott hello partícióazonosító, de hello parancs nem járt sokkal elég toodo úgy, hogy 0 lesz.  További információt is alábbi toohello minta.

az alábbi hello mintakód bemutatja, hogyan toostart majd ellenőrzése folyamatban van a parancs toocause adatvesztés egy adott partícióra.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for hello command below
        Guid operationId = Guid.NewGuid();

        // Note: Use hello appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // hello name of hello target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // hello id of hello target partition inside hello target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start hello command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means hello Fault Injection and Analysis Service has saved hello intent tooperform this work.  It does not say anything about hello progress
        // of hello command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll hello progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // hello command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have hello chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, hello progress object's Result property's Exception property will have hello reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

az alábbi hello példa bemutatja, hogyan toouse hello partitionselector osztályt toochoose egy megadott szolgáltatás véletlenszerű partíciójának:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for hello command below
        Guid operationId = Guid.NewGuid();

        // Note: Use hello appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // hello name of hello target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have hello Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start hello command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means hello Fault Injection and Analysis Service has saved hello intent tooperform this work.  It does not say anything about hello progress
        // of hello command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll hello progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // hello command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, hello progress object's Result property's Exception property will have hello reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Előzmények és csonkolása
Miután parancs elérte a Terminálszolgáltatások állapot, a metaadatait megmarad hello van, és elemzési szolgáltatás bizonyos idő, csak ezután lesz eltávolítva toosave terület.  Ha a "GetProgress" nevezik, hello OperationID azonosítójú parancs használata után el lett távolítva, egy FabricException egy KeyNotFound ErrorCode rendelkező ad vissza.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx

---
title: "aaaStart, majd szüntesse meg a fürt csomópontjai tootest Azure mikroszolgáltatások |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse fault injektálási tootest a Service Fabric-alkalmazás indítása és leállítása a fürtcsomópontok által."
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
ms.date: 6/12/2017
ms.author: lemai
ms.openlocfilehash: 7d3f5147328e6233a67533fbfb2a525aa5fc060e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replacing-hello-start-node-and-stop-node-apis-with-hello-node-transition-api"></a>Hello csomópont elindítása és leállítása csomópont API-k lecserélését hello csomópont átmenet API

## <a name="what-do-hello-stop-node-and-start-node-apis-do"></a>Mi tegye hello csomópont leállítása és elindítása csomópont API-k elvégezni?

hello csomópont API leállítása (felügyelt: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) leáll a Service Fabric-csomópont.  A Service Fabric-csomópont folyamatban, nem a virtuális gép vagy a gép – hello VM vagy a számítógép továbbra is futtatja.  "Csomópont" hello dokumentum többi részén hello az azt jelenti, hogy a Service Fabric-csomópont.  Csomópont leállítása állapotba állítja be azt a *leállt* állapotba, ha azt nem hello fürt tagja, amely nem nyújt szolgáltatásokat, így szimulálva egy *le* csomópont.  Ez akkor hasznos, a rendszer tootest hello hibák hogy az alkalmazás.  hello Start csomópont API (felügyelt: [StartNodeAsync()][startnode], PowerShell: [Start-ServiceFabricNode][startnodeps]]) fordított hello csomópont API leállítása  amely megteremti hello csomópont hátsó tooa normál állapotban.

## <a name="why-are-we-replacing-these"></a>Miért azt cserélni ezeket?

A korábban ismertetett módon egy *leállt* Service Fabric-csomópont a csomópont szándékosan megcélzott hello leállítása csomópont API használatával.  A *le* csomópontja a csomópont (pl. a virtuális gép vagy gépek hello le van tiltva) bármilyen más okból nem működik.  Hello állítsa le a csomópont API-t, a hello rendszer nem fed fel adatokat toodifferentiate közötti *leállt* csomópontok és *le* csomópontok.

Emellett az ezen API-k által visszaadott hibák tartoznak, leíró jellegű, azok lehet.  Például meghívása hello csomópont API állítsa le a már egy *leállt* csomópont hibaüzenetet eredményezi hello *InvalidAddress*.  Ez a felület javítható ki.

Egy csomópont le legyen állítva hello időtartama is "végtelen" csomópont API Start meghívták hello amíg.  Jelenleg ez problémákat okozhat, és lehet, hogy hibalehetőség talált.  Például ha egy felhasználó meghívott hello csomópont API állítsa le a csomópont, és majd elfelejtette kapcsolatos problémák is láttuk.  Később, nem egyértelmű, ha a hello csomópont nem volt *le* vagy *leállt*.


## <a name="introducing-hello-node-transition-apis"></a>Introducing hello csomópont átmenet API-k

Azt is venni ezeket a problémákat fent egy új sor API-k.  hello új csomópont átmenet API (felügyelt: [StartNodeTransitionAsync()][snt]) lehet, hogy használt tootransition a Service Fabric-csomópont tooa *leállt* állapot vagy a tootransition azt az egy *leállt* állapot tooa normál állapotát.  Vegye figyelembe, hogy "a Start" hello hello nevű hello API nem hivatkozik toostarting egy csomópont.  Egy aszinkron művelet, hogy hello rendszer végrehajtja a tootransition hello csomópont tooeither toobeginning hivatkozik *leállt* vagy elindította az állapot.

**Használat**

Ha hello csomópont átmenet API nem lépett fel kivételhiba meghívásakor, majd hello rendszer elfogadta hello aszinkron művelet, és hajtja végre.  Sikeres meghívását nem feltétlenül jelenti azt, hello művelet még befejeződött.  hello művelet, a csomópont átviteli folyamat API hívása hello aktuális állapotának hello tooget információ (felügyelt: [GetNodeTransitionProgressAsync()][gntp]) csomópont meghívásakor használt hello GUID azonosítóval rendelkező Átmenet API ehhez a művelethez.  hello csomópont átviteli folyamat API NodeTransitionProgress objektum beállítása/beolvasása.  Ez az objektum State tulajdonsága hello művelet hello aktuális állapotát határozza meg.  Ha hello állapota "fut" hello művelet végrehajtásakor.  Ha elkészült, hello művelet hiba nélkül befejeződött.  Ha a hibás, nem sikerült hello művelet végrehajtásakor.  a tulajdonság jelzi, milyen hello ki hello Result tulajdonsága kivétel volt.  Lásd: https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate hello-State tulajdonsága és hello "Példa" című szakaszt kódpéldák további információt.


**Egy leállított csomópont és lefelé csomópont megkülönböztetése** Ha egy csomópont *leállt* csomópont átmenet API, a csomópont-lekérdezés hello kimeneti használatával hello (felügyelt: [GetNodeListAsync()] [ nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) jelennek meg, hogy rendelkezik-e a csomópont egy *IsStopped* tulajdonság értéke TRUE.  Megjegyzés: Ez nem azonos a hello hello értékének *NodeStatus* tulajdonságot, amelynek megtudhatja, hogy *le*.  Ha hello *NodeStatus* tulajdonság értéke *le*, de *IsStopped* értéke HAMIS, akkor hello csomópont nem állította le a hello csomópont átmenet API használatával, és  *Lefelé* valamilyen más okból miatt.  Ha hello *IsStopped* tulajdonság értéke igaz, és hello *NodeStatus* tulajdonság *le*, majd leállt hello csomópont átmenet API használatával.

Indítása egy *leállt* hello csomópont átmenet API használatával csomópont visszatér az toofunction hello fürt normál tagjaként újra.  hello hello csomópont-lekérdezés API ekkor *IsStopped* hamis értéket, és *NodeStatus* , valamit, hogy nem működik (pl.).


**Korlátozott időtartam** hello csomópont átmenet API toostop csomópont használatakor hello egyik kötelező paraméterek, *stopNodeDurationInSeconds*, jelöli hello ennyi másodpercig tookeep hello csomópont  *leállítva*.  Ez az érték a megengedett tartományt, amelynek legalább 600, és legfeljebb 14400 hello kell lennie.  Az idő lejárta után hello csomópont magát a állapotát automatikusan újraindul.  Tekintse meg a tooSample 1 az alábbi használati példát.

> [!WARNING]
> Csomópont átmenet API-k elkerülése érdekében, és hello csomópont leállítása és elindítása csomópont API-kat.  hello ajánljuk, a túl a csomópont átmenet API hello használata.  > Ha egy csomópont már lett leállítva hello állítsa le a csomópont API-t használ, azt kell lehet elindítani a hello csomópont API elindításához először hello használata előtt > csomópont átmenet API-k.

> [!WARNING]
> Több csomópont átmenet API-hívások nem végezhető el a hello párhuzamosan ugyanahhoz a csomóponthoz.  Ilyen esetben hello csomópont átmenet API fog > throw egy FabricException NodeTransitionInProgress ErrorCode tulajdonság értékkel.  Miután egy adott csomóponton csomópont átmenet > lett elindítva, meg kell várnia, amíg hello művelet eléri a Terminálszolgáltatások állapot (befejezve, Faulted vagy ForceCancelled) megkezdése előtt egy > új átmenet a hello azonos csomópont.  Engedélyezettek a Parallel csomópont átmenet hívások különböző csomópontokon.


#### <a name="sample-usage"></a>Példa


**Minta 1** -mintát használja a következő hello hello csomópont átmenet API toostop egy csomópont.

```csharp
        // Helper function tooget information about a node
        static Node GetNodeInfo(FabricClient fc, string node)
        {
            NodeList n = null;
            while (n == null)
            {
                n = fc.QueryManager.GetNodeListAsync(node).GetAwaiter().GetResult();
                Task.Delay(TimeSpan.FromSeconds(1)).GetAwaiter();
            };

            return n.FirstOrDefault();
        }

        static async Task WaitForStateAsync(FabricClient fc, Guid operationId, TestCommandProgressState targetState)
        {
            NodeTransitionProgress progress = null;

            do
            {
                bool exceptionObserved = false;
                try
                {
                    progress = await fc.TestManager.GetNodeTransitionProgressAsync(operationId, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                    exceptionObserved = true;
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                    exceptionObserved = true;
                }

                if (!exceptionObserved)
                {
                    Console.WriteLine("Current state of operation '{0}': {1}", operationId, progress.State);

                    if (progress.State == TestCommandProgressState.Faulted)
                    {
                        // Inspect hello progress object's Result.Exception.HResult tooget hello error code.
                        Console.WriteLine("'{0}' failed with: {1}, HResult: {2}", operationId, progress.Result.Exception, progress.Result.Exception.HResult);

                        // ...additional logic as required
                    }

                    if (progress.State == targetState)
                    {
                        Console.WriteLine("Target state '{0}' has been reached", targetState);
                        break;
                    }
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (true);
        }

        static async Task StopNodeAsync(FabricClient fc, string nodeName, int durationInSeconds)
        {
            // Uses hello GetNodeListAsync() API tooget information about hello target node
            Node n = GetNodeInfo(fc, nodeName);

            // Create a Guid
            Guid guid = Guid.NewGuid();

            // Create a NodeStopDescription object, which will be used as a parameter into StartNodeTransition
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, durationInSeconds);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStopDescription from above, which will stop hello target node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    // This is retryable
                }
                catch (FabricTransientException fte)
                {
                    // This is retryable
                }

                // Backoff
                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Minta 2** – hello a következő minta elindul egy *leállt* csomópont.  Néhány segédmódszereket hello első mintából használ.

```csharp
        static async Task StartNodeAsync(FabricClient fc, string nodeName)
        {
            // Uses hello GetNodeListAsync() API tooget information about hello target node
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = n.NodeInstanceId;

            // Create a NodeStartDescription object, which will be used as a parameter into StartNodeTransition
            NodeStartDescription description = new NodeStartDescription(guid, n.NodeName, nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStartDescription from above, which will start hello target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Minta 3** – hello helytelen használatát a következő minta mutatja.  A szintaxis érvénytelen mert hello *stopDurationInSeconds* nagyobb, mint a megengedett tartományon hello biztosít.  StartNodeTransitionAsync() egy végzetes hibával meghiúsul, mert hello művelet nem fogadta el, és hello folyamatban API nem hívható.  Ez a minta néhány segédmódszereket hello első mintából használja.

```csharp
        static async Task StopNodeWithOutOfRangeDurationAsync(FabricClient fc, string nodeName)
        {
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();

            // Use an out of range value for stopDurationInSeconds toodemonstrate error
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, 99999);

            try
            {
                await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
            }

            catch (FabricException e)
            {
                Console.WriteLine("Caught {0}", e);
                Console.WriteLine("ErrorCode {0}", e.ErrorCode);
                // Output:
                // Caught System.Fabric.FabricException: System.Runtime.InteropServices.COMException (-2147017629)
                // StopDurationInSeconds is out of range ---> System.Runtime.InteropServices.COMException: Exception from HRESULT: 0x80071C63
                // << Parts of exception omitted>>
                //
                // ErrorCode InvalidDuration
            }
        }
```

**Minta 4** – hello következő példa bemutatja hello hibaadatokat, amelyek a hello csomópont átviteli folyamat API hello csomópont átmenet API által kezdeményezett hello művelet elfogadható, de később végrehajtása során nem sikerül adja vissza.  Hello esetében akkor sikertelen, mert a csomópont átmenet API hello kísérletek toostart egy csomópont nem létezik.  Ez a minta néhány segédmódszereket hello első mintából használja.

```csharp
        static async Task StartNodeWithNonexistentNodeAsync(FabricClient fc)
        {
            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = 12345;

            // Intentionally use a nonexistent node
            NodeStartDescription description = new NodeStartDescription(guid, "NonexistentNode", nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStartDescription from above, which will start hello target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hello desired state is reached.  In this case, it will end up in hello Faulted state since hello node does not exist.
            // When StartNodeTransitionProgressAsync()'s returned progress object has a State if Faulted, inspect hello progress object's Result.Exception.HResult tooget hello error code.
            // In this case, it will be NodeNotFound.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Faulted).ConfigureAwait(false);
        }
```

[stopnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StopNodeAsync_System_String_System_Numerics_BigInteger_System_Fabric_CompletionMode_
[stopnodeps]: https://msdn.microsoft.com/library/mt125982.aspx
[startnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StartNodeAsync_System_String_System_Numerics_BigInteger_System_String_System_Int32_System_Fabric_CompletionMode_System_Threading_CancellationToken_
[startnodeps]: https://msdn.microsoft.com/library/mt163520.aspx
[nodequery]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient#System_Fabric_FabricClient_QueryClient_GetNodeListAsync_System_String_
[nodequeryps]: https://docs.microsoft.com/powershell/servicefabric/vlatest/Get-ServiceFabricNode?redirectedfrom=msdn
[snt]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_StartNodeTransitionAsync_System_Fabric_Description_NodeTransitionDescription_System_TimeSpan_System_Threading_CancellationToken_
[gntp]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_GetNodeTransitionProgressAsync_System_Guid_System_TimeSpan_System_Threading_CancellationToken_

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
# <a name="replacing-hello-start-node-and-stop-node-apis-with-hello-node-transition-api"></a><span data-ttu-id="b5c0f-103">Hello csomópont elindítása és leállítása csomópont API-k lecserélését hello csomópont átmenet API</span><span class="sxs-lookup"><span data-stu-id="b5c0f-103">Replacing hello Start Node and Stop node APIs with hello Node Transition API</span></span>

## <a name="what-do-hello-stop-node-and-start-node-apis-do"></a><span data-ttu-id="b5c0f-104">Mi tegye hello csomópont leállítása és elindítása csomópont API-k elvégezni?</span><span class="sxs-lookup"><span data-stu-id="b5c0f-104">What do hello Stop Node and Start Node APIs do?</span></span>

<span data-ttu-id="b5c0f-105">hello csomópont API leállítása (felügyelt: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) leáll a Service Fabric-csomópont.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-105">hello Stop Node API (managed: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) stops a Service Fabric node.</span></span>  <span data-ttu-id="b5c0f-106">A Service Fabric-csomópont folyamatban, nem a virtuális gép vagy a gép – hello VM vagy a számítógép továbbra is futtatja.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-106">A Service Fabric node is process, not a VM or machine – hello VM or machine will still be running.</span></span>  <span data-ttu-id="b5c0f-107">"Csomópont" hello dokumentum többi részén hello az azt jelenti, hogy a Service Fabric-csomópont.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-107">For hello rest of hello document "node" will mean Service Fabric node.</span></span>  <span data-ttu-id="b5c0f-108">Csomópont leállítása állapotba állítja be azt a *leállt* állapotba, ha azt nem hello fürt tagja, amely nem nyújt szolgáltatásokat, így szimulálva egy *le* csomópont.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-108">Stopping a node puts it into a *stopped* state where it is not a member of hello cluster and cannot host services, thus simulating a *down* node.</span></span>  <span data-ttu-id="b5c0f-109">Ez akkor hasznos, a rendszer tootest hello hibák hogy az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-109">This is useful for injecting faults into hello system tootest your application.</span></span>  <span data-ttu-id="b5c0f-110">hello Start csomópont API (felügyelt: [StartNodeAsync()][startnode], PowerShell: [Start-ServiceFabricNode][startnodeps]]) fordított hello csomópont API leállítása  amely megteremti hello csomópont hátsó tooa normál állapotban.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-110">hello Start Node API (managed: [StartNodeAsync()][startnode], PowerShell: [Start-ServiceFabricNode][startnodeps]]) reverses hello Stop Node API,  which brings hello node back tooa normal state.</span></span>

## <a name="why-are-we-replacing-these"></a><span data-ttu-id="b5c0f-111">Miért azt cserélni ezeket?</span><span class="sxs-lookup"><span data-stu-id="b5c0f-111">Why are we replacing these?</span></span>

<span data-ttu-id="b5c0f-112">A korábban ismertetett módon egy *leállt* Service Fabric-csomópont a csomópont szándékosan megcélzott hello leállítása csomópont API használatával.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-112">As described earlier, a *stopped* Service Fabric node is a node intentionally targeted using hello Stop Node API.</span></span>  <span data-ttu-id="b5c0f-113">A *le* csomópontja a csomópont (pl. a virtuális gép vagy gépek hello le van tiltva) bármilyen más okból nem működik.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-113">A *down* node is a node that is down for any other reason (e.g. hello VM or machine is off).</span></span>  <span data-ttu-id="b5c0f-114">Hello állítsa le a csomópont API-t, a hello rendszer nem fed fel adatokat toodifferentiate közötti *leállt* csomópontok és *le* csomópontok.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-114">With hello Stop Node API, hello system does not expose information toodifferentiate between *stopped* nodes and *down* nodes.</span></span>

<span data-ttu-id="b5c0f-115">Emellett az ezen API-k által visszaadott hibák tartoznak, leíró jellegű, azok lehet.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-115">In addition, some errors returned by these APIs are not as descriptive as they could be.</span></span>  <span data-ttu-id="b5c0f-116">Például meghívása hello csomópont API állítsa le a már egy *leállt* csomópont hibaüzenetet eredményezi hello *InvalidAddress*.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-116">For example, invoking hello Stop Node API on an already *stopped* node will return hello error *InvalidAddress*.</span></span>  <span data-ttu-id="b5c0f-117">Ez a felület javítható ki.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-117">This experience could be improved.</span></span>

<span data-ttu-id="b5c0f-118">Egy csomópont le legyen állítva hello időtartama is "végtelen" csomópont API Start meghívták hello amíg.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-118">Also, hello duration a node is stopped for is “infinite” until hello Start Node API is invoked.</span></span>  <span data-ttu-id="b5c0f-119">Jelenleg ez problémákat okozhat, és lehet, hogy hibalehetőség talált.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-119">We’ve found this can cause problems and may be error-prone.</span></span>  <span data-ttu-id="b5c0f-120">Például ha egy felhasználó meghívott hello csomópont API állítsa le a csomópont, és majd elfelejtette kapcsolatos problémák is láttuk.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-120">For example, we’ve seen problems where a user invoked hello Stop Node API on a node and then forgot about it.</span></span>  <span data-ttu-id="b5c0f-121">Később, nem egyértelmű, ha a hello csomópont nem volt *le* vagy *leállt*.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-121">Later, it was unclear if hello node was *down* or *stopped*.</span></span>


## <a name="introducing-hello-node-transition-apis"></a><span data-ttu-id="b5c0f-122">Introducing hello csomópont átmenet API-k</span><span class="sxs-lookup"><span data-stu-id="b5c0f-122">Introducing hello Node Transition APIs</span></span>

<span data-ttu-id="b5c0f-123">Azt is venni ezeket a problémákat fent egy új sor API-k.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-123">We’ve addressed these issues above in a new set of APIs.</span></span>  <span data-ttu-id="b5c0f-124">hello új csomópont átmenet API (felügyelt: [StartNodeTransitionAsync()][snt]) lehet, hogy használt tootransition a Service Fabric-csomópont tooa *leállt* állapot vagy a tootransition azt az egy *leállt* állapot tooa normál állapotát.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-124">hello new Node Transition API (managed: [StartNodeTransitionAsync()][snt]) may be used tootransition a Service Fabric node tooa *stopped* state, or tootransition it from a *stopped* state tooa normal up state.</span></span>  <span data-ttu-id="b5c0f-125">Vegye figyelembe, hogy "a Start" hello hello nevű hello API nem hivatkozik toostarting egy csomópont.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-125">Please note that hello “Start” in hello name of hello API does not refer toostarting a node.</span></span>  <span data-ttu-id="b5c0f-126">Egy aszinkron művelet, hogy hello rendszer végrehajtja a tootransition hello csomópont tooeither toobeginning hivatkozik *leállt* vagy elindította az állapot.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-126">It refers toobeginning an asynchronous operation that hello system will execute tootransition hello node tooeither *stopped* or started state.</span></span>

<span data-ttu-id="b5c0f-127">**Használat**</span><span class="sxs-lookup"><span data-stu-id="b5c0f-127">**Usage**</span></span>

<span data-ttu-id="b5c0f-128">Ha hello csomópont átmenet API nem lépett fel kivételhiba meghívásakor, majd hello rendszer elfogadta hello aszinkron művelet, és hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-128">If hello Node Transition API does not throw an exception when invoked, then hello system has accepted hello asynchronous operation, and will execute it.</span></span>  <span data-ttu-id="b5c0f-129">Sikeres meghívását nem feltétlenül jelenti azt, hello művelet még befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-129">A successful call does not imply hello operation is finished yet.</span></span>  <span data-ttu-id="b5c0f-130">hello művelet, a csomópont átviteli folyamat API hívása hello aktuális állapotának hello tooget információ (felügyelt: [GetNodeTransitionProgressAsync()][gntp]) csomópont meghívásakor használt hello GUID azonosítóval rendelkező Átmenet API ehhez a művelethez.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-130">tooget information about hello current state of hello operation, call hello Node Transition Progress API (managed: [GetNodeTransitionProgressAsync()][gntp]) with hello guid used when invoking Node Transition API for this operation.</span></span>  <span data-ttu-id="b5c0f-131">hello csomópont átviteli folyamat API NodeTransitionProgress objektum beállítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-131">hello Node Transition Progress API returns an NodeTransitionProgress object.</span></span>  <span data-ttu-id="b5c0f-132">Ez az objektum State tulajdonsága hello művelet hello aktuális állapotát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-132">This object’s State property specifies hello current state of hello operation.</span></span>  <span data-ttu-id="b5c0f-133">Ha hello állapota "fut" hello művelet végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-133">If hello state is “Running” then hello operation is executing.</span></span>  <span data-ttu-id="b5c0f-134">Ha elkészült, hello művelet hiba nélkül befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-134">If it is Completed, hello operation finished without error.</span></span>  <span data-ttu-id="b5c0f-135">Ha a hibás, nem sikerült hello művelet végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-135">If it is Faulted, there was a problem executing hello operation.</span></span>  <span data-ttu-id="b5c0f-136">a tulajdonság jelzi, milyen hello ki hello Result tulajdonsága kivétel volt.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-136">hello Result property’s Exception property will indicate what hello issue was.</span></span>  <span data-ttu-id="b5c0f-137">Lásd: https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate hello-State tulajdonsága és hello "Példa" című szakaszt kódpéldák további információt.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-137">See https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate for more information about hello State property, and hello “Sample Usage” section below for code examples.</span></span>


<span data-ttu-id="b5c0f-138">**Egy leállított csomópont és lefelé csomópont megkülönböztetése** Ha egy csomópont *leállt* csomópont átmenet API, a csomópont-lekérdezés hello kimeneti használatával hello (felügyelt: [GetNodeListAsync()] [ nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) jelennek meg, hogy rendelkezik-e a csomópont egy *IsStopped* tulajdonság értéke TRUE.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-138">**Differentiating between a stopped node and a down node** If a node is *stopped* using hello Node Transition API, hello output of a node query (managed: [GetNodeListAsync()][nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) will show that this node has an *IsStopped* property value of true.</span></span>  <span data-ttu-id="b5c0f-139">Megjegyzés: Ez nem azonos a hello hello értékének *NodeStatus* tulajdonságot, amelynek megtudhatja, hogy *le*.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-139">Note this is different from hello value of hello *NodeStatus* property, which will say *Down*.</span></span>  <span data-ttu-id="b5c0f-140">Ha hello *NodeStatus* tulajdonság értéke *le*, de *IsStopped* értéke HAMIS, akkor hello csomópont nem állította le a hello csomópont átmenet API használatával, és  *Lefelé* valamilyen más okból miatt.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-140">If hello *NodeStatus* property has a value of *Down*, but *IsStopped* is false, then hello node was not stopped using hello Node Transition API, and is *Down* due some other reason.</span></span>  <span data-ttu-id="b5c0f-141">Ha hello *IsStopped* tulajdonság értéke igaz, és hello *NodeStatus* tulajdonság *le*, majd leállt hello csomópont átmenet API használatával.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-141">If hello *IsStopped* property is true, and hello *NodeStatus* property is *Down*, then it was stopped using hello Node Transition API.</span></span>

<span data-ttu-id="b5c0f-142">Indítása egy *leállt* hello csomópont átmenet API használatával csomópont visszatér az toofunction hello fürt normál tagjaként újra.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-142">Starting a *stopped* node using hello Node Transition API will return it toofunction as a normal member of hello cluster again.</span></span>  <span data-ttu-id="b5c0f-143">hello hello csomópont-lekérdezés API ekkor *IsStopped* hamis értéket, és *NodeStatus* , valamit, hogy nem működik (pl.).</span><span class="sxs-lookup"><span data-stu-id="b5c0f-143">hello output of hello node query API will show *IsStopped* as false, and *NodeStatus* as something that is not Down (e.g. Up).</span></span>


<span data-ttu-id="b5c0f-144">**Korlátozott időtartam** hello csomópont átmenet API toostop csomópont használatakor hello egyik kötelező paraméterek, *stopNodeDurationInSeconds*, jelöli hello ennyi másodpercig tookeep hello csomópont  *leállítva*.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-144">**Limited Duration** When using hello Node Transition API toostop a node, one of hello required parameters, *stopNodeDurationInSeconds*, represents hello amount of time in seconds tookeep hello node *stopped*.</span></span>  <span data-ttu-id="b5c0f-145">Ez az érték a megengedett tartományt, amelynek legalább 600, és legfeljebb 14400 hello kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-145">This value must be in hello allowed range, which has a minimum of 600, and a maximum of 14400.</span></span>  <span data-ttu-id="b5c0f-146">Az idő lejárta után hello csomópont magát a állapotát automatikusan újraindul.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-146">After this time expires, hello node will restart itself into Up state automatically.</span></span>  <span data-ttu-id="b5c0f-147">Tekintse meg a tooSample 1 az alábbi használati példát.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-147">Refer tooSample 1 below for an example of usage.</span></span>

> [!WARNING]
> <span data-ttu-id="b5c0f-148">Csomópont átmenet API-k elkerülése érdekében, és hello csomópont leállítása és elindítása csomópont API-kat.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-148">Avoid mixing Node Transition APIs and hello Stop Node and Start Node APIs.</span></span>  <span data-ttu-id="b5c0f-149">hello ajánljuk, a túl a csomópont átmenet API hello használata.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-149">hello recommendation is too use hello Node Transition API only.</span></span>  <span data-ttu-id="b5c0f-150">> Ha egy csomópont már lett leállítva hello állítsa le a csomópont API-t használ, azt kell lehet elindítani a hello csomópont API elindításához először hello használata előtt > csomópont átmenet API-k.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-150">> If a node has been already been stopped using hello Stop Node API, it should be started using hello Start Node API first before using hello > Node Transition APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="b5c0f-151">Több csomópont átmenet API-hívások nem végezhető el a hello párhuzamosan ugyanahhoz a csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-151">Multiple Node Transition APIs calls cannot be made on hello same node in parallel.</span></span>  <span data-ttu-id="b5c0f-152">Ilyen esetben hello csomópont átmenet API fog > throw egy FabricException NodeTransitionInProgress ErrorCode tulajdonság értékkel.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-152">In such a situation, hello Node Transition API will    > throw a FabricException with an ErrorCode property value of NodeTransitionInProgress.</span></span>  <span data-ttu-id="b5c0f-153">Miután egy adott csomóponton csomópont átmenet > lett elindítva, meg kell várnia, amíg hello művelet eléri a Terminálszolgáltatások állapot (befejezve, Faulted vagy ForceCancelled) megkezdése előtt egy > új átmenet a hello azonos csomópont.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-153">Once a node transition on a specific node has  > been started, you should wait until hello operation reaches a terminal state (Completed, Faulted, or ForceCancelled) before starting a  > new transition on hello same node.</span></span>  <span data-ttu-id="b5c0f-154">Engedélyezettek a Parallel csomópont átmenet hívások különböző csomópontokon.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-154">Parallel node transition calls on different nodes are allowed.</span></span>


#### <a name="sample-usage"></a><span data-ttu-id="b5c0f-155">Példa</span><span class="sxs-lookup"><span data-stu-id="b5c0f-155">Sample Usage</span></span>


<span data-ttu-id="b5c0f-156">**Minta 1** -mintát használja a következő hello hello csomópont átmenet API toostop egy csomópont.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-156">**Sample 1** - hello following sample uses hello Node Transition API toostop a node.</span></span>

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

<span data-ttu-id="b5c0f-157">**Minta 2** – hello a következő minta elindul egy *leállt* csomópont.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-157">**Sample 2** - hello following sample starts a *stopped* node.</span></span>  <span data-ttu-id="b5c0f-158">Néhány segédmódszereket hello első mintából használ.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-158">It uses some helper methods from hello first sample.</span></span>

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

<span data-ttu-id="b5c0f-159">**Minta 3** – hello helytelen használatát a következő minta mutatja.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-159">**Sample 3** - hello following sample shows incorrect usage.</span></span>  <span data-ttu-id="b5c0f-160">A szintaxis érvénytelen mert hello *stopDurationInSeconds* nagyobb, mint a megengedett tartományon hello biztosít.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-160">This usage is incorrect because hello *stopDurationInSeconds* it provides is greater than hello allowed range.</span></span>  <span data-ttu-id="b5c0f-161">StartNodeTransitionAsync() egy végzetes hibával meghiúsul, mert hello művelet nem fogadta el, és hello folyamatban API nem hívható.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-161">Since StartNodeTransitionAsync() will fail with a fatal error, hello operation was not accepted, and hello progress API should not be called.</span></span>  <span data-ttu-id="b5c0f-162">Ez a minta néhány segédmódszereket hello első mintából használja.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-162">This sample uses some helper methods from hello first sample.</span></span>

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

<span data-ttu-id="b5c0f-163">**Minta 4** – hello következő példa bemutatja hello hibaadatokat, amelyek a hello csomópont átviteli folyamat API hello csomópont átmenet API által kezdeményezett hello művelet elfogadható, de később végrehajtása során nem sikerül adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-163">**Sample 4** - hello following sample shows hello error information that will be returned from hello Node Transition Progress API when hello operation initiated by hello Node Transition API is accepted, but fails later while executing.</span></span>  <span data-ttu-id="b5c0f-164">Hello esetében akkor sikertelen, mert a csomópont átmenet API hello kísérletek toostart egy csomópont nem létezik.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-164">In hello case, it fails because hello Node Transition API attempts toostart a node that does not exist.</span></span>  <span data-ttu-id="b5c0f-165">Ez a minta néhány segédmódszereket hello első mintából használja.</span><span class="sxs-lookup"><span data-stu-id="b5c0f-165">This sample uses some helper methods from hello first sample.</span></span>

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

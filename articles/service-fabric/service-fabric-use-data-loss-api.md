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
# <a name="how-tooinvoke-data-loss-on-services"></a><span data-ttu-id="cac93-103">Hogyan tooInvoke adatvesztéshez szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cac93-103">How tooInvoke Data Loss on Services</span></span>
> [!WARNING]
> <span data-ttu-id="cac93-104">A dokumentum ismerteti hogyan toocause adatvesztés a szolgáltatásokban, és körültekintéssel kell alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="cac93-104">This document describe how toocause data loss in your services, and should be used with care.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="cac93-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="cac93-105">Introduction</span></span>
<span data-ttu-id="cac93-106">A Service Fabric-szolgáltatás hívó StartPartitionDataLossAsync() partíciók adatvesztés hívhat meg.</span><span class="sxs-lookup"><span data-stu-id="cac93-106">You can invoke data loss on a partition of your Service Fabric Service by calling StartPartitionDataLossAsync().</span></span>  <span data-ttu-id="cac93-107">Ez az api hello van, és az Analysis Services tooperform hello munkahelyi toocause adatok elvesztését feltételek használja.</span><span class="sxs-lookup"><span data-stu-id="cac93-107">This api uses hello Fault Injection and Analysis Service tooperform hello work toocause data loss conditions.</span></span>

## <a name="using-hello-fault-injection-and-analysis-service"></a><span data-ttu-id="cac93-108">Hello van, és az Analysis Services használatával</span><span class="sxs-lookup"><span data-stu-id="cac93-108">Using hello Fault Injection and Analysis Service</span></span>
<span data-ttu-id="cac93-109">hello van, és az Analysis Services jelenleg a következő API-k az alábbi hello diagram hello.</span><span class="sxs-lookup"><span data-stu-id="cac93-109">hello Fault Injection and Analysis Service currently supports hello following APIs in hello chart below.</span></span>  <span data-ttu-id="cac93-110">hello diagram jobb oldalán hello hello megfelelő PowerShell-parancsmagot szemlélteti.</span><span class="sxs-lookup"><span data-stu-id="cac93-110">hello right side of hello chart shows hello corresponding PowerShell cmdlet.</span></span>  <span data-ttu-id="cac93-111">További információ az egyes tekintse meg az egyes API toohello az msdn dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="cac93-111">Please refer toohello msdn documentation on each API for more information on each one.</span></span>

| <span data-ttu-id="cac93-112">C# API</span><span class="sxs-lookup"><span data-stu-id="cac93-112">C# API</span></span> | <span data-ttu-id="cac93-113">PowerShell-parancsmag</span><span class="sxs-lookup"><span data-stu-id="cac93-113">PowerShell Cmdlet</span></span> |
| --- | ---:|
| <span data-ttu-id="cac93-114">[StartPartitionDataLossAsync][dl]</span><span class="sxs-lookup"><span data-stu-id="cac93-114">[StartPartitionDataLossAsync][dl]</span></span> |<span data-ttu-id="cac93-115">[Start-ServiceFabricPartitionDataLoss][psdl]</span><span class="sxs-lookup"><span data-stu-id="cac93-115">[Start-ServiceFabricPartitionDataLoss][psdl]</span></span> |
| <span data-ttu-id="cac93-116">[StartPartitionQuorumLossAsync][ql]</span><span class="sxs-lookup"><span data-stu-id="cac93-116">[StartPartitionQuorumLossAsync][ql]</span></span> |<span data-ttu-id="cac93-117">[Start-ServiceFabricPartitionQuorumLoss][psql]</span><span class="sxs-lookup"><span data-stu-id="cac93-117">[Start-ServiceFabricPartitionQuorumLoss][psql]</span></span> |
| <span data-ttu-id="cac93-118">[StartPartitionRestartAsync][rp]</span><span class="sxs-lookup"><span data-stu-id="cac93-118">[StartPartitionRestartAsync][rp]</span></span> |<span data-ttu-id="cac93-119">[Start-ServiceFabricPartitionRestart][psrp]</span><span class="sxs-lookup"><span data-stu-id="cac93-119">[Start-ServiceFabricPartitionRestart][psrp]</span></span> |

## <a name="conceptual-overview-of-running-a-command"></a><span data-ttu-id="cac93-120">A parancs futtatása elméleti áttekintése</span><span class="sxs-lookup"><span data-stu-id="cac93-120">Conceptual Overview of Running a Command</span></span>
<span data-ttu-id="cac93-121">egy aszinkron modellt az először hello parancsot egy API-val tooas hello "Start" API ebben a dokumentumban, majd ellenőrzi hello folyamatban van a parancs egy "GetProgress" API használatával, amíg el nem éri a Terminálszolgáltatások említett van, és az Analysis Services által használt hello állapotú, vagy amíg megszakítja a műveletet.</span><span class="sxs-lookup"><span data-stu-id="cac93-121">hello Fault Injection and Analysis Service uses an asynchronous model where you start hello command with one API, referred tooas hello “Start” API in this document, then checks hello progress of this command using a “GetProgress” API until it has reached a terminal state, or until you cancel it.</span></span>
<span data-ttu-id="cac93-122">toostart egy parancs hello "Start" API hello megfelelő API-hívás.</span><span class="sxs-lookup"><span data-stu-id="cac93-122">toostart a command, call hello “Start” API for hello corresponding API.</span></span>  <span data-ttu-id="cac93-123">Ez az API ad vissza, ha hello van, és az Analysis Services elfogadta hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="cac93-123">This API returns when hello Fault Injection and Analysis Service has accepted hello request.</span></span>  <span data-ttu-id="cac93-124">Azonban azt nem jelzi, hogy a parancs futtatása, vagy még akkor is, ha még megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="cac93-124">However, it does not indicate how far a command has run, or even if it has started yet.</span></span>  <span data-ttu-id="cac93-125">Rendelés toocheck a folyamatban lévő utasítás hello "GetProgress" API-t, amely megfelel a korábbi megnevezése toohello "Start" API hívása.</span><span class="sxs-lookup"><span data-stu-id="cac93-125">In order toocheck progress of a command, call hello “GetProgress” API that corresponds toohello “Start” API previously called.</span></span>  <span data-ttu-id="cac93-126">hello "GetProgress" API hello hello parancs belül az állapot tulajdonság aktuális állapotát jelző egy objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="cac93-126">hello “GetProgress” API will return an object indicating hello current status of hello command inside its State property.</span></span>  <span data-ttu-id="cac93-127">A parancs futtatása határozatlan ideig, amíg:</span><span class="sxs-lookup"><span data-stu-id="cac93-127">A command runs indefinitely until:</span></span>

1. <span data-ttu-id="cac93-128">Sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="cac93-128">It completes successfully.</span></span>  <span data-ttu-id="cac93-129">Meghívja a "GetProgress" rajta ebben az esetben, ha hello folyamatban objektum állapota befejeződik.</span><span class="sxs-lookup"><span data-stu-id="cac93-129">If you call “GetProgress” on it in this case, hello progress object’s State will be Completed.</span></span>
2. <span data-ttu-id="cac93-130">Helyreállíthatatlan hibába ütközik.</span><span class="sxs-lookup"><span data-stu-id="cac93-130">It encounters a fatal error.</span></span>  <span data-ttu-id="cac93-131">Meghívja a "GetProgress" rajta ebben az esetben, ha hello folyamatban objektum állapota hibás lesz</span><span class="sxs-lookup"><span data-stu-id="cac93-131">If you call “GetProgress” on it in this case, hello progress object’s State will be Faulted</span></span>
3. <span data-ttu-id="cac93-132">Hello keresztül Mégse [CancelTestCommandAsync] [ cancel] API-t vagy [Stop-ServiceFabricTestCommand] [ cancelps] PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="cac93-132">You cancel it through hello [CancelTestCommandAsync][cancel] API, or [Stop-ServiceFabricTestCommand][cancelps] PowerShell cmdlet.</span></span>  <span data-ttu-id="cac93-133">Meghívja a "GetProgress" rajta ebben az esetben, ha hello folyamatban objektum állapota lesz megszakítva vagy ForceCancelled, attól függően, hogy egy argumentum toothat API.</span><span class="sxs-lookup"><span data-stu-id="cac93-133">If you call “GetProgress” on it in this case, hello progress object’s State will be either Cancelled or ForceCancelled, depending on an argument toothat API.</span></span>  <span data-ttu-id="cac93-134">A dokumentációban hello [CancelTestCommandAsync] [ cancel] további részleteket.</span><span class="sxs-lookup"><span data-stu-id="cac93-134">See hello documentation for [CancelTestCommandAsync][cancel] for more details.</span></span>

## <a name="details-of-running-a-command"></a><span data-ttu-id="cac93-135">A parancs futtatása részleteit</span><span class="sxs-lookup"><span data-stu-id="cac93-135">Details of Running a Command</span></span>
<span data-ttu-id="cac93-136">A sorrend toostart parancsot hívja meg hello Start API hello várt argumentum.</span><span class="sxs-lookup"><span data-stu-id="cac93-136">In order toostart a command, call hello Start API with hello expected arguments.</span></span>  <span data-ttu-id="cac93-137">Minden Start API rendelkezik egy OperationID azonosítójú nevű Guid-argumentum.</span><span class="sxs-lookup"><span data-stu-id="cac93-137">All Start APIs have a Guid argument named operationId.</span></span>  <span data-ttu-id="cac93-138">Meg kell nyomon követheti, hello OperationID azonosítójú argumentum, mert ez a parancs tootrack előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="cac93-138">You should keep track of hello operationId argument, since it is used tootrack progress of this command.</span></span>  <span data-ttu-id="cac93-139">Ez a hello "GetProgress" API-hello parancs rendelés tootrack folyamatban kell átadni.</span><span class="sxs-lookup"><span data-stu-id="cac93-139">This must be passed into hello “GetProgress” API in order tootrack progress of hello command.</span></span>  <span data-ttu-id="cac93-140">hello OperationID azonosítójú egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cac93-140">hello operationId must be unique.</span></span>

<span data-ttu-id="cac93-141">Hello Start API hello GetProgress API ciklust kell hívható, amíg hello visszaadott folyamatban sikeresen hívása után objektum State tulajdonsága befejeződött.</span><span class="sxs-lookup"><span data-stu-id="cac93-141">After successfully calling hello Start API, hello GetProgress API should be called in a loop until hello returned progress object’s State property is Completed.</span></span>  <span data-ttu-id="cac93-142">Minden [FabricTransientException tartozó] [ fte] és OperationCanceledException tartozó meg kell ismételni.</span><span class="sxs-lookup"><span data-stu-id="cac93-142">All [FabricTransientException’s][fte] and OperationCanceledException’s should be retried.</span></span>
<span data-ttu-id="cac93-143">Hello parancs egy terminál állapota (befejezve, Faulted vagy megszakítva) elérésekor hello folyamatban objektum Result tulajdonságnak lesz további információt adott vissza.</span><span class="sxs-lookup"><span data-stu-id="cac93-143">When hello command has reached a terminal state (Completed, Faulted, or Cancelled), hello returned progress object’s Result property will have additional information.</span></span>  <span data-ttu-id="cac93-144">Ha hello állapotban befejeződött, Result.SelectedPartition.PartitionId hello partícióazonosító kiválasztott fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="cac93-144">If hello state is Completed, Result.SelectedPartition.PartitionId will contain hello partition id that was selected.</span></span>  <span data-ttu-id="cac93-145">Result.Exception null értékű lesz.</span><span class="sxs-lookup"><span data-stu-id="cac93-145">Result.Exception will be null.</span></span>  <span data-ttu-id="cac93-146">Ha hello állapot hiba történt a működésében, Result.Exception hello OK hello van és a hibát jelzett az Analysis Services hello parancs lesz.</span><span class="sxs-lookup"><span data-stu-id="cac93-146">If hello state is Faulted, Result.Exception will have hello reason hello Fault Injection and Analysis Service faulted hello command.</span></span>  <span data-ttu-id="cac93-147">Result.SelectedPartition.PartitionId hello partícióazonosító kiválasztott fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="cac93-147">Result.SelectedPartition.PartitionId will have hello partition id that was selected.</span></span>  <span data-ttu-id="cac93-148">Bizonyos esetekben hello parancs lehet, hogy nem rendelkezik kezdett sokkal elegendő egy partíción toochoose.</span><span class="sxs-lookup"><span data-stu-id="cac93-148">In some situations, hello command may not have proceeded far enough toochoose a partition.</span></span>  <span data-ttu-id="cac93-149">Ebben az esetben a hello PartitionId 0 lesz.</span><span class="sxs-lookup"><span data-stu-id="cac93-149">In that case, hello PartitionId will be 0.</span></span>  <span data-ttu-id="cac93-150">Ha hello állapot meg lett szakítva, Result.Exception nem null értékű.</span><span class="sxs-lookup"><span data-stu-id="cac93-150">If hello state is Cancelled, Result.Exception will be null.</span></span>  <span data-ttu-id="cac93-151">Hello Faulted eset, például Result.SelectedPartition.PartitionId lesz a kiválasztott hello partícióazonosító, de hello parancs nem járt sokkal elég toodo úgy, hogy 0 lesz.</span><span class="sxs-lookup"><span data-stu-id="cac93-151">Like hello Faulted case, Result.SelectedPartition.PartitionId will have hello partition id that was chosen, but if hello command has not proceeded far enough toodo so, it will be 0.</span></span>  <span data-ttu-id="cac93-152">További információt is alábbi toohello minta.</span><span class="sxs-lookup"><span data-stu-id="cac93-152">Please also refer toohello sample below.</span></span>

<span data-ttu-id="cac93-153">az alábbi hello mintakód bemutatja, hogyan toostart majd ellenőrzése folyamatban van a parancs toocause adatvesztés egy adott partícióra.</span><span class="sxs-lookup"><span data-stu-id="cac93-153">hello sample code below shows how toostart then check progress on a command toocause data loss on a specific partition.</span></span>

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

<span data-ttu-id="cac93-154">az alábbi hello példa bemutatja, hogyan toouse hello partitionselector osztályt toochoose egy megadott szolgáltatás véletlenszerű partíciójának:</span><span class="sxs-lookup"><span data-stu-id="cac93-154">hello sample below shows how toouse hello PartitionSelector toochoose a random partition of a specified service:</span></span>

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

## <a name="history-and-truncation"></a><span data-ttu-id="cac93-155">Előzmények és csonkolása</span><span class="sxs-lookup"><span data-stu-id="cac93-155">History and Truncation</span></span>
<span data-ttu-id="cac93-156">Miután parancs elérte a Terminálszolgáltatások állapot, a metaadatait megmarad hello van, és elemzési szolgáltatás bizonyos idő, csak ezután lesz eltávolítva toosave terület.</span><span class="sxs-lookup"><span data-stu-id="cac93-156">After a command has reached a terminal state, its metadata will remain in hello Fault Injection and Analysis Service for a certain time, before it will be removed toosave space.</span></span>  <span data-ttu-id="cac93-157">Ha a "GetProgress" nevezik, hello OperationID azonosítójú parancs használata után el lett távolítva, egy FabricException egy KeyNotFound ErrorCode rendelkező ad vissza.</span><span class="sxs-lookup"><span data-stu-id="cac93-157">If “GetProgress” is called using hello operationId of a command after it has been removed, it will return a FabricException with an ErrorCode of KeyNotFound.</span></span>

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx

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
# <a name="testability-actions"></a><span data-ttu-id="8987c-103">Tesztelhetőségi műveletek</span><span class="sxs-lookup"><span data-stu-id="8987c-103">Testability actions</span></span>
<span data-ttu-id="8987c-104">Rendelés toosimulate egy nem megbízható infrastruktúra, az Azure Service Fabric biztosít, hello fejlesztői módon toosimulate a különböző valós hibák és állapotváltozási adat áramlik.</span><span class="sxs-lookup"><span data-stu-id="8987c-104">In order toosimulate an unreliable infrastructure, Azure Service Fabric provides you, hello developer, with ways toosimulate various real-world failures and state transitions.</span></span> <span data-ttu-id="8987c-105">Ezek tesztelhetőségi műveletként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="8987c-105">These are exposed as testability actions.</span></span> <span data-ttu-id="8987c-106">hello műveletek vannak hello alacsony szintű API-k, amelyek egy adott van, az állapotváltás vagy az érvényesítés.</span><span class="sxs-lookup"><span data-stu-id="8987c-106">hello actions are hello low-level APIs that cause a specific fault injection, state transition, or validation.</span></span> <span data-ttu-id="8987c-107">Ezek a műveletek kombinálásával átfogó Tesztelési forgatókönyvek írhat a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="8987c-107">By combining these actions, you can write comprehensive test scenarios for your services.</span></span>

<span data-ttu-id="8987c-108">A Service Fabric biztosít néhány gyakori tesztesetek tevődik össze ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="8987c-108">Service Fabric provides some common test scenarios composed of these actions.</span></span> <span data-ttu-id="8987c-109">Erősen ajánlott, hogy használhatja a beépített forgatókönyvekben, gondosan kiválasztott tootest közös Állapotváltások és hiba esetekben.</span><span class="sxs-lookup"><span data-stu-id="8987c-109">We highly recommend that you utilize these built-in scenarios, which are carefully chosen tootest common state transitions and failure cases.</span></span> <span data-ttu-id="8987c-110">Műveletek azonban lehet használt toocreate egyéni Tesztelési forgatókönyvek, ha a forgatókönyvek, amelyek nem tartoznak hello beépített forgatókönyvek még vagy egyéni alkalmazás igazított tooadd érvényességének használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="8987c-110">However, actions can be used toocreate custom test scenarios when you want tooadd coverage for scenarios that are not covered by hello built-in scenarios yet or that are custom tailored for your application.</span></span>

<span data-ttu-id="8987c-111">C# hello műveletek implementációja hello System.Fabric.dll szerelvény található.</span><span class="sxs-lookup"><span data-stu-id="8987c-111">C# implementations of hello actions are found in hello System.Fabric.dll assembly.</span></span> <span data-ttu-id="8987c-112">hello rendszer Fabric PowerShell-modul hello Microsoft.ServiceFabric.Powershell.dll szerelvény található.</span><span class="sxs-lookup"><span data-stu-id="8987c-112">hello System Fabric PowerShell module is found in hello Microsoft.ServiceFabric.Powershell.dll assembly.</span></span> <span data-ttu-id="8987c-113">Runtime telepítésének részeként hello ServiceFabric PowerShell-modul a könnyű használatra telepített tooallow.</span><span class="sxs-lookup"><span data-stu-id="8987c-113">As part of runtime installation, hello ServiceFabric PowerShell module is installed tooallow for ease of use.</span></span>

## <a name="graceful-vs-ungraceful-fault-actions"></a><span data-ttu-id="8987c-114">Biztonságos és ungraceful tartalék műveletek</span><span class="sxs-lookup"><span data-stu-id="8987c-114">Graceful vs. ungraceful fault actions</span></span>
<span data-ttu-id="8987c-115">Tesztelhetőségi műveletek két fő gyűjtők sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="8987c-115">Testability actions are classified into two major buckets:</span></span>

* <span data-ttu-id="8987c-116">Ungraceful hibák: ezek a hibák hibák gépek újraindításának például szimulálásához, és dolgozza fel az összeomlások.</span><span class="sxs-lookup"><span data-stu-id="8987c-116">Ungraceful faults: These faults simulate failures like machine restarts and process crashes.</span></span> <span data-ttu-id="8987c-117">Ebben az esetben a hibákat hello végrehajtási környezet folyamat váratlanul leáll.</span><span class="sxs-lookup"><span data-stu-id="8987c-117">In such cases of failures, hello execution context of process stops abruptly.</span></span> <span data-ttu-id="8987c-118">Ez azt jelenti, hogy hello állapot nélküli karbantartása hello alkalmazás újra indítása előtt futtatható.</span><span class="sxs-lookup"><span data-stu-id="8987c-118">This means no cleanup of hello state can run before hello application starts up again.</span></span>
* <span data-ttu-id="8987c-119">Sikeres-e hibák: ezek a hibák szimulálása szabályos műveletek, például a replika helyezi át, és elhagyta a(z) terheléselosztást által indított.</span><span class="sxs-lookup"><span data-stu-id="8987c-119">Graceful faults: These faults simulate graceful actions like replica moves and drops triggered by load balancing.</span></span> <span data-ttu-id="8987c-120">Ebben az esetben hello szolgáltatás lekérdezi egy értesítés, amely hello zárja be, és mielőtt kilépne hello állapot fel tudja szabadítani.</span><span class="sxs-lookup"><span data-stu-id="8987c-120">In such cases, hello service gets a notification of hello close and can clean up hello state before exiting.</span></span>

<span data-ttu-id="8987c-121">Jobb minőségű érvényesítéshez hello szolgáltatás futtatásához és az üzleti munkaterhelés közben, hogy a különböző szabályos és ungraceful hibák.</span><span class="sxs-lookup"><span data-stu-id="8987c-121">For better quality validation, run hello service and business workload while inducing various graceful and ungraceful faults.</span></span> <span data-ttu-id="8987c-122">Ungraceful hibák gyakorolja forgatókönyvek, ahol hello szolgáltatás folyamat váratlanul kilép néhány munkafolyamat hello közepén.</span><span class="sxs-lookup"><span data-stu-id="8987c-122">Ungraceful faults exercise scenarios where hello service process abruptly exits in hello middle of some workflow.</span></span> <span data-ttu-id="8987c-123">A vizsgálatok hello helyreállítási elérési úton található hello szolgáltatás replika Service Fabric által történt visszaállítása után.</span><span class="sxs-lookup"><span data-stu-id="8987c-123">This tests  hello recovery path once hello service replica is restored by Service Fabric.</span></span> <span data-ttu-id="8987c-124">Ez segít tesztelése az adatok konzisztenciájának, és hogy hello szolgáltatás állapotának karban kell hiba után.</span><span class="sxs-lookup"><span data-stu-id="8987c-124">This will help test data consistency and whether hello service state is maintained correctly after failures.</span></span> <span data-ttu-id="8987c-125">hello más hibák (hello szabályos hibák) készletét tesztelje, hogy helyesen hello szolgáltatás reagál a Service Fabric mozgatásának tooreplicas.</span><span class="sxs-lookup"><span data-stu-id="8987c-125">hello other set of failures (hello graceful failures) test that hello service correctly reacts tooreplicas being moved around by Service Fabric.</span></span> <span data-ttu-id="8987c-126">Megszakítási kezelésének ellenőrzi a hello RunAsync metódusában.</span><span class="sxs-lookup"><span data-stu-id="8987c-126">This tests handling of cancellation in hello RunAsync method.</span></span> <span data-ttu-id="8987c-127">hello szolgáltatás toocheck igényeihez hello cancellation token alatt állítsa be megfelelően menteni az állapotát és kilépés hello RunAsync metódusában.</span><span class="sxs-lookup"><span data-stu-id="8987c-127">hello service needs toocheck for hello cancellation token being set, correctly save its state, and exit hello RunAsync method.</span></span>

## <a name="testability-actions-list"></a><span data-ttu-id="8987c-128">Tesztelhetőségi műveletek listája</span><span class="sxs-lookup"><span data-stu-id="8987c-128">Testability actions list</span></span>
| <span data-ttu-id="8987c-129">Műveletek</span><span class="sxs-lookup"><span data-stu-id="8987c-129">Action</span></span> | <span data-ttu-id="8987c-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="8987c-130">Description</span></span> | <span data-ttu-id="8987c-131">Felügyelt API</span><span class="sxs-lookup"><span data-stu-id="8987c-131">Managed API</span></span> | <span data-ttu-id="8987c-132">PowerShell-parancsmag</span><span class="sxs-lookup"><span data-stu-id="8987c-132">PowerShell cmdlet</span></span> | <span data-ttu-id="8987c-133">Biztonságos/ungraceful hibák</span><span class="sxs-lookup"><span data-stu-id="8987c-133">Graceful/ungraceful faults</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="8987c-134">CleanTestState</span><span class="sxs-lookup"><span data-stu-id="8987c-134">CleanTestState</span></span> |<span data-ttu-id="8987c-135">Eltávolítja az összes hello teszt állapota hello fürt esetén a hibás leállítást hello teszt illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="8987c-135">Removes all hello test state from hello cluster in case of a bad shutdown of hello test driver.</span></span> |<span data-ttu-id="8987c-136">CleanTestStateAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-136">CleanTestStateAsync</span></span> |<span data-ttu-id="8987c-137">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="8987c-137">Remove-ServiceFabricTestState</span></span> |<span data-ttu-id="8987c-138">Nem alkalmazható</span><span class="sxs-lookup"><span data-stu-id="8987c-138">Not applicable</span></span> |
| <span data-ttu-id="8987c-139">InvokeDataLoss</span><span class="sxs-lookup"><span data-stu-id="8987c-139">InvokeDataLoss</span></span> |<span data-ttu-id="8987c-140">Adatvesztés kapott egy szolgáltatás partícióra.</span><span class="sxs-lookup"><span data-stu-id="8987c-140">Induces data loss into a service partition.</span></span> |<span data-ttu-id="8987c-141">InvokeDataLossAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-141">InvokeDataLossAsync</span></span> |<span data-ttu-id="8987c-142">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="8987c-142">Invoke-ServiceFabricPartitionDataLoss</span></span> |<span data-ttu-id="8987c-143">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="8987c-143">Graceful</span></span> |
| <span data-ttu-id="8987c-144">InvokeQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="8987c-144">InvokeQuorumLoss</span></span> |<span data-ttu-id="8987c-145">Egy adott állapot-nyilvántartó szolgáltatása partíció elhelyezi a kvórum elvesztése.</span><span class="sxs-lookup"><span data-stu-id="8987c-145">Puts a given stateful service partition into quorum loss.</span></span> |<span data-ttu-id="8987c-146">InvokeQuorumLossAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-146">InvokeQuorumLossAsync</span></span> |<span data-ttu-id="8987c-147">Invoke-ServiceFabricQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="8987c-147">Invoke-ServiceFabricQuorumLoss</span></span> |<span data-ttu-id="8987c-148">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="8987c-148">Graceful</span></span> |
| <span data-ttu-id="8987c-149">Elsődleges áthelyezése</span><span class="sxs-lookup"><span data-stu-id="8987c-149">Move Primary</span></span> |<span data-ttu-id="8987c-150">A kurzor hello megadott állapotalapú szolgáltatási toohello megadott fürtcsomópont elsődleges másodpéldány.</span><span class="sxs-lookup"><span data-stu-id="8987c-150">Moves hello specified primary replica of a stateful service toohello specified cluster node.</span></span> |<span data-ttu-id="8987c-151">MovePrimaryAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-151">MovePrimaryAsync</span></span> |<span data-ttu-id="8987c-152">MOVE-ServiceFabricPrimaryReplica</span><span class="sxs-lookup"><span data-stu-id="8987c-152">Move-ServiceFabricPrimaryReplica</span></span> |<span data-ttu-id="8987c-153">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="8987c-153">Graceful</span></span> |
| <span data-ttu-id="8987c-154">Másodlagos áthelyezése</span><span class="sxs-lookup"><span data-stu-id="8987c-154">Move Secondary</span></span> |<span data-ttu-id="8987c-155">Az állapotalapú szolgáltatás tooa másik fürtcsomópont másodlagos másodpéldány aktuális hello helyezi.</span><span class="sxs-lookup"><span data-stu-id="8987c-155">Moves hello current secondary replica of a stateful service tooa different cluster node.</span></span> |<span data-ttu-id="8987c-156">MoveSecondaryAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-156">MoveSecondaryAsync</span></span> |<span data-ttu-id="8987c-157">MOVE-ServiceFabricSecondaryReplica</span><span class="sxs-lookup"><span data-stu-id="8987c-157">Move-ServiceFabricSecondaryReplica</span></span> |<span data-ttu-id="8987c-158">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="8987c-158">Graceful</span></span> |
| <span data-ttu-id="8987c-159">RemoveReplica</span><span class="sxs-lookup"><span data-stu-id="8987c-159">RemoveReplica</span></span> |<span data-ttu-id="8987c-160">Replika hiba szimulálja által replika eltávolítása egy fürtből.</span><span class="sxs-lookup"><span data-stu-id="8987c-160">Simulates a replica failure by removing a replica from a cluster.</span></span> <span data-ttu-id="8987c-161">Ez hello replika bezárul, és ismerhetik azt toorole "None", eltávolítja az összes szerinti hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="8987c-161">This will close hello replica and will transition it toorole 'None', removing all of its state from hello cluster.</span></span> |<span data-ttu-id="8987c-162">RemoveReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-162">RemoveReplicaAsync</span></span> |<span data-ttu-id="8987c-163">Remove-ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="8987c-163">Remove-ServiceFabricReplica</span></span> |<span data-ttu-id="8987c-164">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="8987c-164">Graceful</span></span> |
| <span data-ttu-id="8987c-165">RestartDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="8987c-165">RestartDeployedCodePackage</span></span> |<span data-ttu-id="8987c-166">A kód csomag folyamat hibájának szimulálja egy egy fürt egy csomópontján telepített kódcsomag újraindításával.</span><span class="sxs-lookup"><span data-stu-id="8987c-166">Simulates a code package process failure by restarting a code package deployed on a node in a cluster.</span></span> <span data-ttu-id="8987c-167">Ez a újraindul, hogy a folyamat minden hello felhasználói szolgáltatás üzemeltetett replikák hello kód csomag folyamat megszakítása.</span><span class="sxs-lookup"><span data-stu-id="8987c-167">This aborts hello code package process, which will restart all hello user service replicas hosted in that process.</span></span> |<span data-ttu-id="8987c-168">RestartDeployedCodePackageAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-168">RestartDeployedCodePackageAsync</span></span> |<span data-ttu-id="8987c-169">Újraindítás-ServiceFabricDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="8987c-169">Restart-ServiceFabricDeployedCodePackage</span></span> |<span data-ttu-id="8987c-170">Ungraceful</span><span class="sxs-lookup"><span data-stu-id="8987c-170">Ungraceful</span></span> |
| <span data-ttu-id="8987c-171">A RestartNode</span><span class="sxs-lookup"><span data-stu-id="8987c-171">RestartNode</span></span> |<span data-ttu-id="8987c-172">A Service Fabric-fürt Csomóponthiba szimulálja egy csomópont újraindításával.</span><span class="sxs-lookup"><span data-stu-id="8987c-172">Simulates a Service Fabric cluster node failure by restarting a node.</span></span> |<span data-ttu-id="8987c-173">RestartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-173">RestartNodeAsync</span></span> |<span data-ttu-id="8987c-174">Újraindítás-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="8987c-174">Restart-ServiceFabricNode</span></span> |<span data-ttu-id="8987c-175">Ungraceful</span><span class="sxs-lookup"><span data-stu-id="8987c-175">Ungraceful</span></span> |
| <span data-ttu-id="8987c-176">RestartPartition</span><span class="sxs-lookup"><span data-stu-id="8987c-176">RestartPartition</span></span> |<span data-ttu-id="8987c-177">A datacenter blackout vagy a fürt blackout forgatókönyv szimulálja néhány vagy az összes partíció replikák újraindításával.</span><span class="sxs-lookup"><span data-stu-id="8987c-177">Simulates a datacenter blackout or cluster blackout scenario by restarting some or all replicas of a partition.</span></span> |<span data-ttu-id="8987c-178">RestartPartitionAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-178">RestartPartitionAsync</span></span> |<span data-ttu-id="8987c-179">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="8987c-179">Restart-ServiceFabricPartition</span></span> |<span data-ttu-id="8987c-180">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="8987c-180">Graceful</span></span> |
| <span data-ttu-id="8987c-181">RestartReplica</span><span class="sxs-lookup"><span data-stu-id="8987c-181">RestartReplica</span></span> |<span data-ttu-id="8987c-182">A replika hiba szimulálja egy megőrzött replikát újraindításával fürtben, hello replika bezárása, és majd megnyitni.</span><span class="sxs-lookup"><span data-stu-id="8987c-182">Simulates a replica failure by restarting a persisted replica in a cluster, closing hello replica and then reopening it.</span></span> |<span data-ttu-id="8987c-183">RestartReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-183">RestartReplicaAsync</span></span> |<span data-ttu-id="8987c-184">Újraindítás-ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="8987c-184">Restart-ServiceFabricReplica</span></span> |<span data-ttu-id="8987c-185">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="8987c-185">Graceful</span></span> |
| <span data-ttu-id="8987c-186">A StartNode</span><span class="sxs-lookup"><span data-stu-id="8987c-186">StartNode</span></span> |<span data-ttu-id="8987c-187">A csomópont elindul egy fürt, amely már le van állítva.</span><span class="sxs-lookup"><span data-stu-id="8987c-187">Starts a node in a cluster that is already stopped.</span></span> |<span data-ttu-id="8987c-188">StartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-188">StartNodeAsync</span></span> |<span data-ttu-id="8987c-189">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="8987c-189">Start-ServiceFabricNode</span></span> |<span data-ttu-id="8987c-190">Nem alkalmazható</span><span class="sxs-lookup"><span data-stu-id="8987c-190">Not applicable</span></span> |
| <span data-ttu-id="8987c-191">Stopnode parancs</span><span class="sxs-lookup"><span data-stu-id="8987c-191">StopNode</span></span> |<span data-ttu-id="8987c-192">Egy Csomóponthiba szimulálja a fürt egyik csomópontjában levő leállításával.</span><span class="sxs-lookup"><span data-stu-id="8987c-192">Simulates a node failure by stopping a node in a cluster.</span></span> <span data-ttu-id="8987c-193">hello csomópont le maradnak, amíg StartNode nevezik.</span><span class="sxs-lookup"><span data-stu-id="8987c-193">hello node will stay down until StartNode is called.</span></span> |<span data-ttu-id="8987c-194">StopNodeAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-194">StopNodeAsync</span></span> |<span data-ttu-id="8987c-195">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="8987c-195">Stop-ServiceFabricNode</span></span> |<span data-ttu-id="8987c-196">Ungraceful</span><span class="sxs-lookup"><span data-stu-id="8987c-196">Ungraceful</span></span> |
| <span data-ttu-id="8987c-197">ValidateApplication</span><span class="sxs-lookup"><span data-stu-id="8987c-197">ValidateApplication</span></span> |<span data-ttu-id="8987c-198">Hello rendelkezésre állását és, hogy néhány tartalék hello rendszerbe után általában az alkalmazáson belül minden Service Fabric-szolgáltatás állapotát ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="8987c-198">Validates hello availability and health of all Service Fabric services within an application, usually after inducing some fault into hello system.</span></span> |<span data-ttu-id="8987c-199">ValidateApplicationAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-199">ValidateApplicationAsync</span></span> |<span data-ttu-id="8987c-200">Teszt-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="8987c-200">Test-ServiceFabricApplication</span></span> |<span data-ttu-id="8987c-201">Nem alkalmazható</span><span class="sxs-lookup"><span data-stu-id="8987c-201">Not applicable</span></span> |
| <span data-ttu-id="8987c-202">ValidateService</span><span class="sxs-lookup"><span data-stu-id="8987c-202">ValidateService</span></span> |<span data-ttu-id="8987c-203">Hello rendelkezésre állás és a Service Fabric-szolgáltatás állapotát ellenőrzi a általában után néhány tartalék hogy hello rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="8987c-203">Validates hello availability and health of a Service Fabric service, usually after inducing some fault into hello system.</span></span> |<span data-ttu-id="8987c-204">ValidateServiceAsync</span><span class="sxs-lookup"><span data-stu-id="8987c-204">ValidateServiceAsync</span></span> |<span data-ttu-id="8987c-205">Teszt-ServiceFabricService</span><span class="sxs-lookup"><span data-stu-id="8987c-205">Test-ServiceFabricService</span></span> |<span data-ttu-id="8987c-206">Nem alkalmazható</span><span class="sxs-lookup"><span data-stu-id="8987c-206">Not applicable</span></span> |

## <a name="running-a-testability-action-using-powershell"></a><span data-ttu-id="8987c-207">PowerShell-lel tesztelhetőségi műveletek futtatása</span><span class="sxs-lookup"><span data-stu-id="8987c-207">Running a testability action using PowerShell</span></span>
<span data-ttu-id="8987c-208">Az oktatóanyag bemutatja, hogyan toorun egy tesztelhetőségi művelet PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="8987c-208">This tutorial shows you how toorun a testability action by using PowerShell.</span></span> <span data-ttu-id="8987c-209">Megtudhatja, hogyan toorun egy tesztelhetőségi műveletet a helyi (1-box) fürt vagy egy Azure-fürttel.</span><span class="sxs-lookup"><span data-stu-id="8987c-209">You will learn how toorun a testability action against a local (one-box) cluster or an Azure cluster.</span></span> <span data-ttu-id="8987c-210">Microsoft.Fabric.Powershell.dll – hello Service Fabric PowerShell-modul – telepíti a rendszer automatikusan telepíti a Microsoft Service Fabric MSI hello.</span><span class="sxs-lookup"><span data-stu-id="8987c-210">Microsoft.Fabric.Powershell.dll--hello Service Fabric PowerShell module--is installed automatically when you install hello Microsoft Service Fabric MSI.</span></span> <span data-ttu-id="8987c-211">hello modul automatikusan betöltődik, amikor megnyit egy PowerShell-parancssorba.</span><span class="sxs-lookup"><span data-stu-id="8987c-211">hello module is loaded automatically when you open a PowerShell prompt.</span></span>

<span data-ttu-id="8987c-212">Útmutató szegmensek:</span><span class="sxs-lookup"><span data-stu-id="8987c-212">Tutorial segments:</span></span>

* [<span data-ttu-id="8987c-213">Egy műveletet futtatni egy beépített fürt</span><span class="sxs-lookup"><span data-stu-id="8987c-213">Run an action against a one-box cluster</span></span>](#run-an-action-against-a-one-box-cluster)
* [<span data-ttu-id="8987c-214">Egy műveletet futtatni egy Azure-fürttel</span><span class="sxs-lookup"><span data-stu-id="8987c-214">Run an action against an Azure cluster</span></span>](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a><span data-ttu-id="8987c-215">Egy műveletet futtatni egy beépített fürt</span><span class="sxs-lookup"><span data-stu-id="8987c-215">Run an action against a one-box cluster</span></span>
<span data-ttu-id="8987c-216">toorun egy tesztelhetőségi műveletet a helyi fürthöz, először kapcsolódik toohello fürt és a rendszergazda módban megnyitott hello PowerShell-parancssorba.</span><span class="sxs-lookup"><span data-stu-id="8987c-216">toorun a testability action against a local cluster, first connect toohello cluster and open hello PowerShell prompt in administrator mode.</span></span> <span data-ttu-id="8987c-217">Ossza meg velünk nézze meg hello **újraindítás-ServiceFabricNode** művelet.</span><span class="sxs-lookup"><span data-stu-id="8987c-217">Let us look at hello **Restart-ServiceFabricNode** action.</span></span>

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

<span data-ttu-id="8987c-218">Itt hello művelet **újraindítás-ServiceFabricNode** "1. csomópont" nevű csomópont futtatja.</span><span class="sxs-lookup"><span data-stu-id="8987c-218">Here hello action **Restart-ServiceFabricNode** is being run on a node named "Node1".</span></span> <span data-ttu-id="8987c-219">hello végrehajtási mód határozza meg, hogy azt nem ellenőrizze hogy hello újraindítás-csomópont művelet ténylegesen sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="8987c-219">hello completion mode specifies that it should not verify whether hello restart-node action actually succeeded.</span></span> <span data-ttu-id="8987c-220">Adja meg hello befejezési mint a "Hitelesítés" módnál azt tooverify hogy ténylegesen hello újraindítási művelete sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8987c-220">Specifying hello completion mode as "Verify" will cause it tooverify whether hello restart action actually succeeded.</span></span> <span data-ttu-id="8987c-221">Helyett a nevével adja meg közvetlenül hello csomópont, megadhatja azt a kulcsot és hello partíciófajta replika, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="8987c-221">Instead of directly specifying hello node by its name, you can specify it via a partition key and hello kind of replica, as follows:</span></span>

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

<span data-ttu-id="8987c-222">**Újraindítás-ServiceFabricNode** kell használt toorestart a Service Fabric egy fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="8987c-222">**Restart-ServiceFabricNode** should be used toorestart a Service Fabric node in a cluster.</span></span> <span data-ttu-id="8987c-223">Ezzel leállítja a hello Fabric.exe folyamat, amely újraindítja az összes hello rendszer szolgáltatás és a felhasználó szolgáltatás üzemeltetett replikák ezen a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="8987c-223">This will stop hello Fabric.exe process, which will restart all of hello system service and user service replicas hosted on that node.</span></span> <span data-ttu-id="8987c-224">Az API tootest a szolgáltatás segítségével fedik le a hibák hello feladatátvételi helyreállítási elérési út mentén.</span><span class="sxs-lookup"><span data-stu-id="8987c-224">Using this API tootest your service helps uncover bugs along hello failover recovery paths.</span></span> <span data-ttu-id="8987c-225">Ennek segítségével szimulálhatja hello fürtben lévő csomópontok hibáit.</span><span class="sxs-lookup"><span data-stu-id="8987c-225">It helps simulate node failures in hello cluster.</span></span>

<span data-ttu-id="8987c-226">hello alábbi képernyőfelvételen látható hello **újraindítás-ServiceFabricNode** tesztelhetőségi parancs működés közben.</span><span class="sxs-lookup"><span data-stu-id="8987c-226">hello following screenshot shows hello **Restart-ServiceFabricNode** testability command in action.</span></span>

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

<span data-ttu-id="8987c-227">hello az első kimeneti hello **Get-ServiceFabricNode** (hello Service Fabric PowerShell-modul a parancsmag) jeleníti meg, hogy hello helyi fürt az öt csomóponttal rendelkezik: Node.1 tooNode.5.</span><span class="sxs-lookup"><span data-stu-id="8987c-227">hello output of hello first **Get-ServiceFabricNode** (a cmdlet from hello Service Fabric PowerShell module) shows that hello local cluster has five nodes: Node.1 tooNode.5.</span></span> <span data-ttu-id="8987c-228">Hello tesztelhetőségi művelet (parancsmag) után **újraindítás-ServiceFabricNode** hello csomóponton végrehajtása nevű Node.4, látható, hogy hello csomópont hasznos üzemidő alaphelyzetbe lett állítva.</span><span class="sxs-lookup"><span data-stu-id="8987c-228">After hello testability action (cmdlet) **Restart-ServiceFabricNode** is executed on hello node, named Node.4, we see that hello node's uptime has been reset.</span></span>

### <a name="run-an-action-against-an-azure-cluster"></a><span data-ttu-id="8987c-229">Egy műveletet futtatni egy Azure-fürttel</span><span class="sxs-lookup"><span data-stu-id="8987c-229">Run an action against an Azure cluster</span></span>
<span data-ttu-id="8987c-230">A helyi fürthöz hasonló toorunning hello műveletet tesztelhetőségi műveletek futtatása (a PowerShell használatával) egy Azure fürtön hajtották.</span><span class="sxs-lookup"><span data-stu-id="8987c-230">Running a testability action (by using PowerShell) against an Azure cluster is similar toorunning hello action against a local cluster.</span></span> <span data-ttu-id="8987c-231">hello egyetlen különbség az, hogy helyett csatlakozó toohello helyi fürthöz, a hello művelet futtatása előtt kell tooconnect toohello Azure először a fürt.</span><span class="sxs-lookup"><span data-stu-id="8987c-231">hello only difference is that before you can run hello action, instead of connecting toohello local cluster, you need tooconnect toohello Azure cluster first.</span></span>

## <a name="running-a-testability-action-using-c35"></a><span data-ttu-id="8987c-232">C &#35; használatával tesztelhetőségi műveletek futtatása</span><span class="sxs-lookup"><span data-stu-id="8987c-232">Running a testability action using C&#35;</span></span>
<span data-ttu-id="8987c-233">toorun egy tesztelhetőségi művelet C# használatával, előbb kell tooconnect toohello fürt FabricClient használatával.</span><span class="sxs-lookup"><span data-stu-id="8987c-233">toorun a testability action by using C#, first you need tooconnect toohello cluster by using FabricClient.</span></span> <span data-ttu-id="8987c-234">Beszereznie hello szükséges paramétereket toorun hello művelet.</span><span class="sxs-lookup"><span data-stu-id="8987c-234">Then obtain hello parameters needed toorun hello action.</span></span> <span data-ttu-id="8987c-235">Különböző paraméterek használhatók toorun hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="8987c-235">Different parameters can be used toorun hello same action.</span></span>
<span data-ttu-id="8987c-236">Megnézi hello RestartServiceFabricNode művelet, hello csomópont információk (a csomópont neve és csomópont azonosítója) segítségével van egyirányú toorun hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="8987c-236">Looking at hello RestartServiceFabricNode action, one way toorun it is by using hello node information (node name and node instance ID) in hello cluster.</span></span>

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

<span data-ttu-id="8987c-237">A paraméter magyarázata:</span><span class="sxs-lookup"><span data-stu-id="8987c-237">Parameter explanation:</span></span>

* <span data-ttu-id="8987c-238">**CompleteMode** határozza meg, hogy hello üzemmód nem ellenőriznie kell, hogy sikerült-e ténylegesen hello újraindítási művelete.</span><span class="sxs-lookup"><span data-stu-id="8987c-238">**CompleteMode** specifies that hello mode should not verify whether hello restart action actually succeeded.</span></span> <span data-ttu-id="8987c-239">Adja meg hello befejezési mint a "Hitelesítés" módnál azt tooverify hogy ténylegesen hello újraindítási művelete sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8987c-239">Specifying hello completion mode as "Verify" will cause it tooverify whether hello restart action actually succeeded.</span></span>  
* <span data-ttu-id="8987c-240">**OperationTimeout** beállítása hello hello művelet toofinish idő, mielőtt egy TimeoutException kivétel történt.</span><span class="sxs-lookup"><span data-stu-id="8987c-240">**OperationTimeout** sets hello amount of time for hello operation toofinish before a TimeoutException exception is thrown.</span></span>
* <span data-ttu-id="8987c-241">**CancellationToken** lehetővé teszi, hogy a függőben lévő hívás toobe megszakítva.</span><span class="sxs-lookup"><span data-stu-id="8987c-241">**CancellationToken** enables a pending call toobe canceled.</span></span>

<span data-ttu-id="8987c-242">Adja meg közvetlenül a hello csomópont neve, azt a kulcsot és hello partíciófajta replika adhatja.</span><span class="sxs-lookup"><span data-stu-id="8987c-242">Instead of directly specifying hello node by its name, you can specify it via a partition key and hello kind of replica.</span></span>

<span data-ttu-id="8987c-243">További információkért lásd: [partitionselector osztályt és replicaselector osztályt](#partition_replica_selector).</span><span class="sxs-lookup"><span data-stu-id="8987c-243">For further information, see [PartitionSelector and ReplicaSelector](#partition_replica_selector).</span></span>

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

## <a name="partitionselector-and-replicaselector"></a><span data-ttu-id="8987c-244">Partitionselector osztályt és replicaselector osztályt</span><span class="sxs-lookup"><span data-stu-id="8987c-244">PartitionSelector and ReplicaSelector</span></span>
### <a name="partitionselector"></a><span data-ttu-id="8987c-245">Partitionselector osztályt</span><span class="sxs-lookup"><span data-stu-id="8987c-245">PartitionSelector</span></span>
<span data-ttu-id="8987c-246">Partitionselector osztályt tesztelhetőségi felfedett segítő és van egy adott használt tooselect partícióazonosító mely tooperform a hello tesztelhetőségi műveletek.</span><span class="sxs-lookup"><span data-stu-id="8987c-246">PartitionSelector is a helper exposed in testability and is used tooselect a specific partition on which tooperform any of hello testability actions.</span></span> <span data-ttu-id="8987c-247">Egy adott partícióra használt tooselect lehet, ha hello Partícióazonosító előzetesen is ismert.</span><span class="sxs-lookup"><span data-stu-id="8987c-247">It can be used tooselect a specific partition if hello partition ID is known beforehand.</span></span> <span data-ttu-id="8987c-248">Vagy megadhatja a hello partíciós kulcs, és hello művelet belső hello Partícióazonosító fel.</span><span class="sxs-lookup"><span data-stu-id="8987c-248">Or, you can provide hello partition key and hello operation will resolve hello partition ID internally.</span></span> <span data-ttu-id="8987c-249">Akkor is hello lehetőség kiválasztásával egy véletlenszerű partíció.</span><span class="sxs-lookup"><span data-stu-id="8987c-249">You also have hello option of selecting a random partition.</span></span>

<span data-ttu-id="8987c-250">toouse a segítő létrehozása hello partitionselector osztályt objektum és hello partíció hello Select * módszerek egyikének használatával.</span><span class="sxs-lookup"><span data-stu-id="8987c-250">toouse this helper, create hello PartitionSelector object and select hello partition by using one of hello Select* methods.</span></span> <span data-ttu-id="8987c-251">Hello partitionselector osztályt objektum toohello API írja elő, akkor továbbítja.</span><span class="sxs-lookup"><span data-stu-id="8987c-251">Then pass in hello PartitionSelector object toohello API that requires it.</span></span> <span data-ttu-id="8987c-252">Ha nincs lehetőség van kiválasztva, tooa véletlenszerű partíció alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="8987c-252">If no option is selected, it defaults tooa random partition.</span></span>

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

### <a name="replicaselector"></a><span data-ttu-id="8987c-253">Replicaselector osztályt</span><span class="sxs-lookup"><span data-stu-id="8987c-253">ReplicaSelector</span></span>
<span data-ttu-id="8987c-254">Replicaselector osztályt tesztelhetőségi felfedett segítő és van használt toohelp kiválasztani a replikát a mely tooperform hello tesztelhetőségi műveletek.</span><span class="sxs-lookup"><span data-stu-id="8987c-254">ReplicaSelector is a helper exposed in testability and is used toohelp select a replica on which tooperform any of hello testability actions.</span></span> <span data-ttu-id="8987c-255">Egy adott replika használt tooselect lehet, ha előzetesen ismert hello másodpéldány-azonosító.</span><span class="sxs-lookup"><span data-stu-id="8987c-255">It can be used tooselect a specific replica if hello replica ID is known beforehand.</span></span> <span data-ttu-id="8987c-256">Ezenkívül hello lehetőség kiválasztásával egy elsődleges másodpéldány, vagy egy véletlenszerű másodlagos rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8987c-256">In addition, you have hello option of selecting a primary replica or a random secondary.</span></span> <span data-ttu-id="8987c-257">Replicaselector osztályt a partitionselector osztályt osztályból származik, ezért meg kell tooselect mindkét hello replika és hello partíciót, amelyen tooperform hello tesztelhetőségi művelet kívánja.</span><span class="sxs-lookup"><span data-stu-id="8987c-257">ReplicaSelector derives from PartitionSelector, so you need tooselect both hello replica and hello partition on which you wish tooperform hello testability operation.</span></span>

<span data-ttu-id="8987c-258">a segítő toouse replicaselector osztályt objektum létrehozása, és állítsa be a kívánt módon hello tooselect hello replika- és hello partíció.</span><span class="sxs-lookup"><span data-stu-id="8987c-258">toouse this helper, create a ReplicaSelector object and set hello way you want tooselect hello replica and hello partition.</span></span> <span data-ttu-id="8987c-259">Majd továbbíthatja azt hello API, amely igényli azt be.</span><span class="sxs-lookup"><span data-stu-id="8987c-259">You can then pass it into hello API that requires it.</span></span> <span data-ttu-id="8987c-260">Ha nincs lehetőség van kiválasztva, tooa véletlenszerű replika és a véletlenszerű partíció alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="8987c-260">If no option is selected, it defaults tooa random replica and random partition.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="8987c-261">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8987c-261">Next steps</span></span>
* [<span data-ttu-id="8987c-262">Tesztelhetőségi forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="8987c-262">Testability scenarios</span></span>](service-fabric-testability-scenarios.md)
* <span data-ttu-id="8987c-263">Hogyan tootest a szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8987c-263">How tootest your service</span></span>
  * [<span data-ttu-id="8987c-264">Szolgáltatás-munkaterhelések során hibák szimulálása</span><span class="sxs-lookup"><span data-stu-id="8987c-264">Simulate failures during service workloads</span></span>](service-fabric-testability-workload-tests.md)
  * [<span data-ttu-id="8987c-265">Szolgáltatások közötti kommunikációs hibái</span><span class="sxs-lookup"><span data-stu-id="8987c-265">Service-to-service communication failures</span></span>](service-fabric-testability-scenarios-service-communication.md)


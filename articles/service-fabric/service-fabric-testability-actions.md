---
title: "Azure mikroszolgáltatások hibáinak szimulálása |} Microsoft Docs"
description: "Ez a cikk beszél található a Microsoft Azure Service Fabric tesztelhetőségi műveleteket."
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
ms.openlocfilehash: c8ddc7732999ae555323bebaef60aa34c8f2ec17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="testability-actions"></a><span data-ttu-id="90967-103">Tesztelhetőségi műveletek</span><span class="sxs-lookup"><span data-stu-id="90967-103">Testability actions</span></span>
<span data-ttu-id="90967-104">Ahhoz, hogy egy nem megbízható infrastruktúra szimulálása, Azure Service Fabric biztosít, a fejlesztői, azzal, hogy különböző valós hibák és állapotváltozási adat áramlik szimulálásához.</span><span class="sxs-lookup"><span data-stu-id="90967-104">In order to simulate an unreliable infrastructure, Azure Service Fabric provides you, the developer, with ways to simulate various real-world failures and state transitions.</span></span> <span data-ttu-id="90967-105">Ezek tesztelhetőségi műveletként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="90967-105">These are exposed as testability actions.</span></span> <span data-ttu-id="90967-106">A műveletek végezhetők, amelyek egy adott van, állapotváltás vagy érvényesítési alacsony szintű API-k.</span><span class="sxs-lookup"><span data-stu-id="90967-106">The actions are the low-level APIs that cause a specific fault injection, state transition, or validation.</span></span> <span data-ttu-id="90967-107">Ezek a műveletek kombinálásával átfogó Tesztelési forgatókönyvek írhat a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="90967-107">By combining these actions, you can write comprehensive test scenarios for your services.</span></span>

<span data-ttu-id="90967-108">A Service Fabric biztosít néhány gyakori tesztesetek tevődik össze ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="90967-108">Service Fabric provides some common test scenarios composed of these actions.</span></span> <span data-ttu-id="90967-109">Erősen ajánlott, hogy használhatja a beépített forgatókönyvekben, gondosan kiválasztott közös Állapotváltások és hiba esetekben teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="90967-109">We highly recommend that you utilize these built-in scenarios, which are carefully chosen to test common state transitions and failure cases.</span></span> <span data-ttu-id="90967-110">Azonban műveletek segítségével hozzon létre egyéni Tesztelési forgatókönyvek, ha hozzá szeretne adni a forgatókönyvek, amelyek nem tartoznak a beépített forgatókönyvek még vagy egyéni alkalmazás igazított érvényességének.</span><span class="sxs-lookup"><span data-stu-id="90967-110">However, actions can be used to create custom test scenarios when you want to add coverage for scenarios that are not covered by the built-in scenarios yet or that are custom tailored for your application.</span></span>

<span data-ttu-id="90967-111">A műveletek implementációja C# System.Fabric.dll szerelvényben találhatók.</span><span class="sxs-lookup"><span data-stu-id="90967-111">C# implementations of the actions are found in the System.Fabric.dll assembly.</span></span> <span data-ttu-id="90967-112">A rendszer Fabric PowerShell-modul a Microsoft.ServiceFabric.Powershell.dll szerelvény található.</span><span class="sxs-lookup"><span data-stu-id="90967-112">The System Fabric PowerShell module is found in the Microsoft.ServiceFabric.Powershell.dll assembly.</span></span> <span data-ttu-id="90967-113">Runtime telepítésének részeként a ServiceFabric PowerShell modul telepítve engedélyezi a használat megkönnyítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="90967-113">As part of runtime installation, the ServiceFabric PowerShell module is installed to allow for ease of use.</span></span>

## <a name="graceful-vs-ungraceful-fault-actions"></a><span data-ttu-id="90967-114">Biztonságos és ungraceful tartalék műveletek</span><span class="sxs-lookup"><span data-stu-id="90967-114">Graceful vs. ungraceful fault actions</span></span>
<span data-ttu-id="90967-115">Tesztelhetőségi műveletek két fő gyűjtők sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="90967-115">Testability actions are classified into two major buckets:</span></span>

* <span data-ttu-id="90967-116">Ungraceful hibák: ezek a hibák hibák gépek újraindításának például szimulálásához, és dolgozza fel az összeomlások.</span><span class="sxs-lookup"><span data-stu-id="90967-116">Ungraceful faults: These faults simulate failures like machine restarts and process crashes.</span></span> <span data-ttu-id="90967-117">Ebben az esetben a hibákat a végrehajtási környezet folyamat váratlanul leáll.</span><span class="sxs-lookup"><span data-stu-id="90967-117">In such cases of failures, the execution context of process stops abruptly.</span></span> <span data-ttu-id="90967-118">Ez azt jelenti, hogy az állapot nem karbantartás előtt ismét elindul az alkalmazás futtatható.</span><span class="sxs-lookup"><span data-stu-id="90967-118">This means no cleanup of the state can run before the application starts up again.</span></span>
* <span data-ttu-id="90967-119">Sikeres-e hibák: ezek a hibák szimulálása szabályos műveletek, például a replika helyezi át, és elhagyta a(z) terheléselosztást által indított.</span><span class="sxs-lookup"><span data-stu-id="90967-119">Graceful faults: These faults simulate graceful actions like replica moves and drops triggered by load balancing.</span></span> <span data-ttu-id="90967-120">Ezekben az esetekben a szolgáltatás lekérdezi egy értesítés, amely a Bezárás gombra, és mielőtt kilépne a állapotot fel tudja szabadítani.</span><span class="sxs-lookup"><span data-stu-id="90967-120">In such cases, the service gets a notification of the close and can clean up the state before exiting.</span></span>

<span data-ttu-id="90967-121">Jobb minőségű ellenőrzéséhez futtassa le a szolgáltatást és az üzleti alkalmazások és szolgáltatások közben, hogy a különböző szabályos és ungraceful hibák.</span><span class="sxs-lookup"><span data-stu-id="90967-121">For better quality validation, run the service and business workload while inducing various graceful and ungraceful faults.</span></span> <span data-ttu-id="90967-122">Ungraceful hibák forgatókönyvek, ahol a szolgáltatás-folyamat váratlanul kilép közepén egyes munkafolyamat megadásával.</span><span class="sxs-lookup"><span data-stu-id="90967-122">Ungraceful faults exercise scenarios where the service process abruptly exits in the middle of some workflow.</span></span> <span data-ttu-id="90967-123">A szolgáltatás replika Service Fabric által történt visszaállítása után ellenőrzi a helyreállítási elérési úton található.</span><span class="sxs-lookup"><span data-stu-id="90967-123">This tests  the recovery path once the service replica is restored by Service Fabric.</span></span> <span data-ttu-id="90967-124">Ez segít, tesztelje az adatok konzisztenciájának, és hogy a szolgáltatás állapotának karban kell hiba után.</span><span class="sxs-lookup"><span data-stu-id="90967-124">This will help test data consistency and whether the service state is maintained correctly after failures.</span></span> <span data-ttu-id="90967-125">Hibák (a szabályos hibák) más készlete tesztelje, hogy a szolgáltatás megfelelően reagál a Service Fabric mozgatásának replikákra.</span><span class="sxs-lookup"><span data-stu-id="90967-125">The other set of failures (the graceful failures) test that the service correctly reacts to replicas being moved around by Service Fabric.</span></span> <span data-ttu-id="90967-126">A RunAsync metódusában a megszakítási kezelésének ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="90967-126">This tests handling of cancellation in the RunAsync method.</span></span> <span data-ttu-id="90967-127">A szolgáltatás szüksége van a megszakítási token alatt állítsa be, megfelelően menteni az állapotát, és lépjen ki a RunAsync metódusában.</span><span class="sxs-lookup"><span data-stu-id="90967-127">The service needs to check for the cancellation token being set, correctly save its state, and exit the RunAsync method.</span></span>

## <a name="testability-actions-list"></a><span data-ttu-id="90967-128">Tesztelhetőségi műveletek listája</span><span class="sxs-lookup"><span data-stu-id="90967-128">Testability actions list</span></span>
| <span data-ttu-id="90967-129">Műveletek</span><span class="sxs-lookup"><span data-stu-id="90967-129">Action</span></span> | <span data-ttu-id="90967-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="90967-130">Description</span></span> | <span data-ttu-id="90967-131">Felügyelt API</span><span class="sxs-lookup"><span data-stu-id="90967-131">Managed API</span></span> | <span data-ttu-id="90967-132">PowerShell-parancsmag</span><span class="sxs-lookup"><span data-stu-id="90967-132">PowerShell cmdlet</span></span> | <span data-ttu-id="90967-133">Biztonságos/ungraceful hibák</span><span class="sxs-lookup"><span data-stu-id="90967-133">Graceful/ungraceful faults</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="90967-134">CleanTestState</span><span class="sxs-lookup"><span data-stu-id="90967-134">CleanTestState</span></span> |<span data-ttu-id="90967-135">Eltávolítja az összes teszt állapota a fürt esetén a teszt illesztőprogram rossz leállítására.</span><span class="sxs-lookup"><span data-stu-id="90967-135">Removes all the test state from the cluster in case of a bad shutdown of the test driver.</span></span> |<span data-ttu-id="90967-136">CleanTestStateAsync</span><span class="sxs-lookup"><span data-stu-id="90967-136">CleanTestStateAsync</span></span> |<span data-ttu-id="90967-137">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="90967-137">Remove-ServiceFabricTestState</span></span> |<span data-ttu-id="90967-138">Nem alkalmazható</span><span class="sxs-lookup"><span data-stu-id="90967-138">Not applicable</span></span> |
| <span data-ttu-id="90967-139">InvokeDataLoss</span><span class="sxs-lookup"><span data-stu-id="90967-139">InvokeDataLoss</span></span> |<span data-ttu-id="90967-140">Adatvesztés kapott egy szolgáltatás partícióra.</span><span class="sxs-lookup"><span data-stu-id="90967-140">Induces data loss into a service partition.</span></span> |<span data-ttu-id="90967-141">InvokeDataLossAsync</span><span class="sxs-lookup"><span data-stu-id="90967-141">InvokeDataLossAsync</span></span> |<span data-ttu-id="90967-142">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="90967-142">Invoke-ServiceFabricPartitionDataLoss</span></span> |<span data-ttu-id="90967-143">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="90967-143">Graceful</span></span> |
| <span data-ttu-id="90967-144">InvokeQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="90967-144">InvokeQuorumLoss</span></span> |<span data-ttu-id="90967-145">Egy adott állapot-nyilvántartó szolgáltatása partíció elhelyezi a kvórum elvesztése.</span><span class="sxs-lookup"><span data-stu-id="90967-145">Puts a given stateful service partition into quorum loss.</span></span> |<span data-ttu-id="90967-146">InvokeQuorumLossAsync</span><span class="sxs-lookup"><span data-stu-id="90967-146">InvokeQuorumLossAsync</span></span> |<span data-ttu-id="90967-147">Invoke-ServiceFabricQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="90967-147">Invoke-ServiceFabricQuorumLoss</span></span> |<span data-ttu-id="90967-148">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="90967-148">Graceful</span></span> |
| <span data-ttu-id="90967-149">Elsődleges áthelyezése</span><span class="sxs-lookup"><span data-stu-id="90967-149">Move Primary</span></span> |<span data-ttu-id="90967-150">A megadott elsődleges replika az állapotalapú szolgáltatás áthelyezése a megadott fürtcsomópont.</span><span class="sxs-lookup"><span data-stu-id="90967-150">Moves the specified primary replica of a stateful service to the specified cluster node.</span></span> |<span data-ttu-id="90967-151">MovePrimaryAsync</span><span class="sxs-lookup"><span data-stu-id="90967-151">MovePrimaryAsync</span></span> |<span data-ttu-id="90967-152">MOVE-ServiceFabricPrimaryReplica</span><span class="sxs-lookup"><span data-stu-id="90967-152">Move-ServiceFabricPrimaryReplica</span></span> |<span data-ttu-id="90967-153">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="90967-153">Graceful</span></span> |
| <span data-ttu-id="90967-154">Másodlagos áthelyezése</span><span class="sxs-lookup"><span data-stu-id="90967-154">Move Secondary</span></span> |<span data-ttu-id="90967-155">A jelenlegi másodlagos másodpéldány egy állapotalapú szolgáltatás áthelyezése egy másik fürtcsomópontra.</span><span class="sxs-lookup"><span data-stu-id="90967-155">Moves the current secondary replica of a stateful service to a different cluster node.</span></span> |<span data-ttu-id="90967-156">MoveSecondaryAsync</span><span class="sxs-lookup"><span data-stu-id="90967-156">MoveSecondaryAsync</span></span> |<span data-ttu-id="90967-157">MOVE-ServiceFabricSecondaryReplica</span><span class="sxs-lookup"><span data-stu-id="90967-157">Move-ServiceFabricSecondaryReplica</span></span> |<span data-ttu-id="90967-158">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="90967-158">Graceful</span></span> |
| <span data-ttu-id="90967-159">RemoveReplica</span><span class="sxs-lookup"><span data-stu-id="90967-159">RemoveReplica</span></span> |<span data-ttu-id="90967-160">Replika hiba szimulálja által replika eltávolítása egy fürtből.</span><span class="sxs-lookup"><span data-stu-id="90967-160">Simulates a replica failure by removing a replica from a cluster.</span></span> <span data-ttu-id="90967-161">Ez a replika bezárul, és állapotba kerül, hogy szerepkör "None", a fürt összes állapotában eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="90967-161">This will close the replica and will transition it to role 'None', removing all of its state from the cluster.</span></span> |<span data-ttu-id="90967-162">RemoveReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="90967-162">RemoveReplicaAsync</span></span> |<span data-ttu-id="90967-163">Remove-ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="90967-163">Remove-ServiceFabricReplica</span></span> |<span data-ttu-id="90967-164">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="90967-164">Graceful</span></span> |
| <span data-ttu-id="90967-165">RestartDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="90967-165">RestartDeployedCodePackage</span></span> |<span data-ttu-id="90967-166">A kód csomag folyamat hibájának szimulálja egy egy fürt egy csomópontján telepített kódcsomag újraindításával.</span><span class="sxs-lookup"><span data-stu-id="90967-166">Simulates a code package process failure by restarting a code package deployed on a node in a cluster.</span></span> <span data-ttu-id="90967-167">Ez a minden felhasználó szolgáltatás replika üzemeltetett újraindul, hogy a folyamat kód csomag folyamat megszakítása.</span><span class="sxs-lookup"><span data-stu-id="90967-167">This aborts the code package process, which will restart all the user service replicas hosted in that process.</span></span> |<span data-ttu-id="90967-168">RestartDeployedCodePackageAsync</span><span class="sxs-lookup"><span data-stu-id="90967-168">RestartDeployedCodePackageAsync</span></span> |<span data-ttu-id="90967-169">Újraindítás-ServiceFabricDeployedCodePackage</span><span class="sxs-lookup"><span data-stu-id="90967-169">Restart-ServiceFabricDeployedCodePackage</span></span> |<span data-ttu-id="90967-170">Ungraceful</span><span class="sxs-lookup"><span data-stu-id="90967-170">Ungraceful</span></span> |
| <span data-ttu-id="90967-171">A RestartNode</span><span class="sxs-lookup"><span data-stu-id="90967-171">RestartNode</span></span> |<span data-ttu-id="90967-172">A Service Fabric-fürt Csomóponthiba szimulálja egy csomópont újraindításával.</span><span class="sxs-lookup"><span data-stu-id="90967-172">Simulates a Service Fabric cluster node failure by restarting a node.</span></span> |<span data-ttu-id="90967-173">RestartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="90967-173">RestartNodeAsync</span></span> |<span data-ttu-id="90967-174">Újraindítás-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="90967-174">Restart-ServiceFabricNode</span></span> |<span data-ttu-id="90967-175">Ungraceful</span><span class="sxs-lookup"><span data-stu-id="90967-175">Ungraceful</span></span> |
| <span data-ttu-id="90967-176">RestartPartition</span><span class="sxs-lookup"><span data-stu-id="90967-176">RestartPartition</span></span> |<span data-ttu-id="90967-177">A datacenter blackout vagy a fürt blackout forgatókönyv szimulálja néhány vagy az összes partíció replikák újraindításával.</span><span class="sxs-lookup"><span data-stu-id="90967-177">Simulates a datacenter blackout or cluster blackout scenario by restarting some or all replicas of a partition.</span></span> |<span data-ttu-id="90967-178">RestartPartitionAsync</span><span class="sxs-lookup"><span data-stu-id="90967-178">RestartPartitionAsync</span></span> |<span data-ttu-id="90967-179">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="90967-179">Restart-ServiceFabricPartition</span></span> |<span data-ttu-id="90967-180">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="90967-180">Graceful</span></span> |
| <span data-ttu-id="90967-181">RestartReplica</span><span class="sxs-lookup"><span data-stu-id="90967-181">RestartReplica</span></span> |<span data-ttu-id="90967-182">A replika hiba szimulálja a megőrzött replika újraindításával fürtben, a replika bezárása, és majd megnyitni.</span><span class="sxs-lookup"><span data-stu-id="90967-182">Simulates a replica failure by restarting a persisted replica in a cluster, closing the replica and then reopening it.</span></span> |<span data-ttu-id="90967-183">RestartReplicaAsync</span><span class="sxs-lookup"><span data-stu-id="90967-183">RestartReplicaAsync</span></span> |<span data-ttu-id="90967-184">Újraindítás-ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="90967-184">Restart-ServiceFabricReplica</span></span> |<span data-ttu-id="90967-185">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="90967-185">Graceful</span></span> |
| <span data-ttu-id="90967-186">A StartNode</span><span class="sxs-lookup"><span data-stu-id="90967-186">StartNode</span></span> |<span data-ttu-id="90967-187">A csomópont elindul egy fürt, amely már le van állítva.</span><span class="sxs-lookup"><span data-stu-id="90967-187">Starts a node in a cluster that is already stopped.</span></span> |<span data-ttu-id="90967-188">StartNodeAsync</span><span class="sxs-lookup"><span data-stu-id="90967-188">StartNodeAsync</span></span> |<span data-ttu-id="90967-189">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="90967-189">Start-ServiceFabricNode</span></span> |<span data-ttu-id="90967-190">Nem alkalmazható</span><span class="sxs-lookup"><span data-stu-id="90967-190">Not applicable</span></span> |
| <span data-ttu-id="90967-191">Stopnode parancs</span><span class="sxs-lookup"><span data-stu-id="90967-191">StopNode</span></span> |<span data-ttu-id="90967-192">Egy Csomóponthiba szimulálja a fürt egyik csomópontjában levő leállításával.</span><span class="sxs-lookup"><span data-stu-id="90967-192">Simulates a node failure by stopping a node in a cluster.</span></span> <span data-ttu-id="90967-193">A csomópont le maradnak, amíg StartNode nevezik.</span><span class="sxs-lookup"><span data-stu-id="90967-193">The node will stay down until StartNode is called.</span></span> |<span data-ttu-id="90967-194">StopNodeAsync</span><span class="sxs-lookup"><span data-stu-id="90967-194">StopNodeAsync</span></span> |<span data-ttu-id="90967-195">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="90967-195">Stop-ServiceFabricNode</span></span> |<span data-ttu-id="90967-196">Ungraceful</span><span class="sxs-lookup"><span data-stu-id="90967-196">Ungraceful</span></span> |
| <span data-ttu-id="90967-197">ValidateApplication</span><span class="sxs-lookup"><span data-stu-id="90967-197">ValidateApplication</span></span> |<span data-ttu-id="90967-198">A rendelkezésre állási és, hogy bizonyos hiba a rendszerbe után általában az alkalmazáson belül minden Service Fabric-szolgáltatás állapotát ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="90967-198">Validates the availability and health of all Service Fabric services within an application, usually after inducing some fault into the system.</span></span> |<span data-ttu-id="90967-199">ValidateApplicationAsync</span><span class="sxs-lookup"><span data-stu-id="90967-199">ValidateApplicationAsync</span></span> |<span data-ttu-id="90967-200">Teszt-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="90967-200">Test-ServiceFabricApplication</span></span> |<span data-ttu-id="90967-201">Nem alkalmazható</span><span class="sxs-lookup"><span data-stu-id="90967-201">Not applicable</span></span> |
| <span data-ttu-id="90967-202">ValidateService</span><span class="sxs-lookup"><span data-stu-id="90967-202">ValidateService</span></span> |<span data-ttu-id="90967-203">A rendelkezésre állás és a Service Fabric-szolgáltatás állapotát ellenőrzi a általában után néhány tartalék hogy a rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="90967-203">Validates the availability and health of a Service Fabric service, usually after inducing some fault into the system.</span></span> |<span data-ttu-id="90967-204">ValidateServiceAsync</span><span class="sxs-lookup"><span data-stu-id="90967-204">ValidateServiceAsync</span></span> |<span data-ttu-id="90967-205">Teszt-ServiceFabricService</span><span class="sxs-lookup"><span data-stu-id="90967-205">Test-ServiceFabricService</span></span> |<span data-ttu-id="90967-206">Nem alkalmazható</span><span class="sxs-lookup"><span data-stu-id="90967-206">Not applicable</span></span> |

## <a name="running-a-testability-action-using-powershell"></a><span data-ttu-id="90967-207">PowerShell-lel tesztelhetőségi műveletek futtatása</span><span class="sxs-lookup"><span data-stu-id="90967-207">Running a testability action using PowerShell</span></span>
<span data-ttu-id="90967-208">Az oktatóanyag bemutatja, hogyan tesztelhetőségi műveletet futtatni a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="90967-208">This tutorial shows you how to run a testability action by using PowerShell.</span></span> <span data-ttu-id="90967-209">Megtudhatja, hogyan egy tesztelhetőségi művelet futtatásához helyi (1-box) fürt vagy egy Azure-fürttel.</span><span class="sxs-lookup"><span data-stu-id="90967-209">You will learn how to run a testability action against a local (one-box) cluster or an Azure cluster.</span></span> <span data-ttu-id="90967-210">Microsoft.Fabric.Powershell.dll--a Service Fabric PowerShell-modul – a Microsoft Service Fabric MSI telepítésekor automatikusan telepítve van.</span><span class="sxs-lookup"><span data-stu-id="90967-210">Microsoft.Fabric.Powershell.dll--the Service Fabric PowerShell module--is installed automatically when you install the Microsoft Service Fabric MSI.</span></span> <span data-ttu-id="90967-211">A modul automatikusan betöltődik, amikor megnyit egy PowerShell-parancssorba.</span><span class="sxs-lookup"><span data-stu-id="90967-211">The module is loaded automatically when you open a PowerShell prompt.</span></span>

<span data-ttu-id="90967-212">Útmutató szegmensek:</span><span class="sxs-lookup"><span data-stu-id="90967-212">Tutorial segments:</span></span>

* [<span data-ttu-id="90967-213">Egy műveletet futtatni egy beépített fürt</span><span class="sxs-lookup"><span data-stu-id="90967-213">Run an action against a one-box cluster</span></span>](#run-an-action-against-a-one-box-cluster)
* [<span data-ttu-id="90967-214">Egy műveletet futtatni egy Azure-fürttel</span><span class="sxs-lookup"><span data-stu-id="90967-214">Run an action against an Azure cluster</span></span>](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a><span data-ttu-id="90967-215">Egy műveletet futtatni egy beépített fürt</span><span class="sxs-lookup"><span data-stu-id="90967-215">Run an action against a one-box cluster</span></span>
<span data-ttu-id="90967-216">Egy tesztelhetőségi művelet futtatni a helyi fürthöz, először csatlakozzon a fürthöz, és nyissa meg a PowerShell-parancssorba rendszergazdai módban.</span><span class="sxs-lookup"><span data-stu-id="90967-216">To run a testability action against a local cluster, first connect to the cluster and open the PowerShell prompt in administrator mode.</span></span> <span data-ttu-id="90967-217">Ossza meg velünk tekintse meg a **újraindítás-ServiceFabricNode** művelet.</span><span class="sxs-lookup"><span data-stu-id="90967-217">Let us look at the **Restart-ServiceFabricNode** action.</span></span>

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

<span data-ttu-id="90967-218">Itt a művelet **újraindítás-ServiceFabricNode** "1. csomópont" nevű csomópont futtatja.</span><span class="sxs-lookup"><span data-stu-id="90967-218">Here the action **Restart-ServiceFabricNode** is being run on a node named "Node1".</span></span> <span data-ttu-id="90967-219">A végrehajtási mód határozza meg, hogy azt kell nem ellenőrizze, hogy az újraindítás-csomópont művelet ténylegesen sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="90967-219">The completion mode specifies that it should not verify whether the restart-node action actually succeeded.</span></span> <span data-ttu-id="90967-220">Adja meg a befejezési módot, mint a "Hitelesítés", akkor ellenőrizheti, hogy sikerült-e ténylegesen az újraindítási művelete.</span><span class="sxs-lookup"><span data-stu-id="90967-220">Specifying the completion mode as "Verify" will cause it to verify whether the restart action actually succeeded.</span></span> <span data-ttu-id="90967-221">Helyett a nevével adja meg közvetlenül a csomópont, megadhatja a partíciós kulcs és milyen típusú replika, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="90967-221">Instead of directly specifying the node by its name, you can specify it via a partition key and the kind of replica, as follows:</span></span>

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

<span data-ttu-id="90967-222">**Újraindítás-ServiceFabricNode** használatával indítsa újra a Service Fabric-csomópont a fürtben.</span><span class="sxs-lookup"><span data-stu-id="90967-222">**Restart-ServiceFabricNode** should be used to restart a Service Fabric node in a cluster.</span></span> <span data-ttu-id="90967-223">Ezzel leállítja a Fabric.exe folyamatban, ami fog indítsa újra az összes, a rendszer szolgáltatás és a felhasználó szolgáltatás replikák adott csomóponton található alkotóelem.</span><span class="sxs-lookup"><span data-stu-id="90967-223">This will stop the Fabric.exe process, which will restart all of the system service and user service replicas hosted on that node.</span></span> <span data-ttu-id="90967-224">Ez az API a szolgáltatás tesztelése segítségével fedje fel a feladatátvételi helyreállítási elérési út mentén hibák.</span><span class="sxs-lookup"><span data-stu-id="90967-224">Using this API to test your service helps uncover bugs along the failover recovery paths.</span></span> <span data-ttu-id="90967-225">Ennek segítségével szimulálása a fürtben lévő csomópontok hibáit.</span><span class="sxs-lookup"><span data-stu-id="90967-225">It helps simulate node failures in the cluster.</span></span>

<span data-ttu-id="90967-226">Az alábbi képernyőfelvételen látható a **újraindítás-ServiceFabricNode** tesztelhetőségi parancs működés közben.</span><span class="sxs-lookup"><span data-stu-id="90967-226">The following screenshot shows the **Restart-ServiceFabricNode** testability command in action.</span></span>

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

<span data-ttu-id="90967-227">Az első kimeneti **Get-ServiceFabricNode** (a Service Fabric PowerShell-modul a parancsmag) jeleníti meg, hogy a helyi fürt rendelkezik-e az öt csomóponttal: Node.1 Node.5 számára.</span><span class="sxs-lookup"><span data-stu-id="90967-227">The output of the first **Get-ServiceFabricNode** (a cmdlet from the Service Fabric PowerShell module) shows that the local cluster has five nodes: Node.1 to Node.5.</span></span> <span data-ttu-id="90967-228">A tesztelhetőségi (parancsmag) művelet után **újraindítás-ServiceFabricNode** a csomópont végrehajtása nevű Node.4, látható, hogy a csomópont hasznos üzemidő alaphelyzetbe lett állítva.</span><span class="sxs-lookup"><span data-stu-id="90967-228">After the testability action (cmdlet) **Restart-ServiceFabricNode** is executed on the node, named Node.4, we see that the node's uptime has been reset.</span></span>

### <a name="run-an-action-against-an-azure-cluster"></a><span data-ttu-id="90967-229">Egy műveletet futtatni egy Azure-fürttel</span><span class="sxs-lookup"><span data-stu-id="90967-229">Run an action against an Azure cluster</span></span>
<span data-ttu-id="90967-230">Tesztelhetőségi műveletek futtatása (a PowerShell használatával) egy Azure fürtön hajtották hasonlít a helyi fürtön hajtották a műveletek futtatása.</span><span class="sxs-lookup"><span data-stu-id="90967-230">Running a testability action (by using PowerShell) against an Azure cluster is similar to running the action against a local cluster.</span></span> <span data-ttu-id="90967-231">Az egyetlen különbség az, hogy helyett a helyi fürthöz, a művelet futtatása előtt kell először csatlakozzon az Azure-fürttel.</span><span class="sxs-lookup"><span data-stu-id="90967-231">The only difference is that before you can run the action, instead of connecting to the local cluster, you need to connect to the Azure cluster first.</span></span>

## <a name="running-a-testability-action-using-c35"></a><span data-ttu-id="90967-232">C &#35; használatával tesztelhetőségi műveletek futtatása</span><span class="sxs-lookup"><span data-stu-id="90967-232">Running a testability action using C&#35;</span></span>
<span data-ttu-id="90967-233">C# használatával tesztelhetőségi műveletet futtatni, először meg kell FabricClient használatával csatlakozzon a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="90967-233">To run a testability action by using C#, first you need to connect to the cluster by using FabricClient.</span></span> <span data-ttu-id="90967-234">A művelet futtatásához szükséges paraméterek majd beszerzése.</span><span class="sxs-lookup"><span data-stu-id="90967-234">Then obtain the parameters needed to run the action.</span></span> <span data-ttu-id="90967-235">Különböző paraméterek használhatók ugyanaz a művelet futtatásához.</span><span class="sxs-lookup"><span data-stu-id="90967-235">Different parameters can be used to run the same action.</span></span>
<span data-ttu-id="90967-236">A RestartServiceFabricNode művelet megnézi, egy futtassa módja a fürtben lévő csomópont információk (a csomópont neve és csomópont azonosítója) segítségével.</span><span class="sxs-lookup"><span data-stu-id="90967-236">Looking at the RestartServiceFabricNode action, one way to run it is by using the node information (node name and node instance ID) in the cluster.</span></span>

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

<span data-ttu-id="90967-237">A paraméter magyarázata:</span><span class="sxs-lookup"><span data-stu-id="90967-237">Parameter explanation:</span></span>

* <span data-ttu-id="90967-238">**CompleteMode** határozza meg, hogy a mód nem ellenőrizze, hogy sikerült-e ténylegesen az újraindítási művelete.</span><span class="sxs-lookup"><span data-stu-id="90967-238">**CompleteMode** specifies that the mode should not verify whether the restart action actually succeeded.</span></span> <span data-ttu-id="90967-239">Adja meg a befejezési módot, mint a "Hitelesítés", akkor ellenőrizheti, hogy sikerült-e ténylegesen az újraindítási művelete.</span><span class="sxs-lookup"><span data-stu-id="90967-239">Specifying the completion mode as "Verify" will cause it to verify whether the restart action actually succeeded.</span></span>  
* <span data-ttu-id="90967-240">**OperationTimeout** beállítja azt az időtartamot, amíg a művelet befejezését, mielőtt egy TimeoutException kivétel történt az.</span><span class="sxs-lookup"><span data-stu-id="90967-240">**OperationTimeout** sets the amount of time for the operation to finish before a TimeoutException exception is thrown.</span></span>
* <span data-ttu-id="90967-241">**CancellationToken** lehetővé teszi, hogy a függőben lévő hívása szakítható meg.</span><span class="sxs-lookup"><span data-stu-id="90967-241">**CancellationToken** enables a pending call to be canceled.</span></span>

<span data-ttu-id="90967-242">Helyett a nevével adja meg közvetlenül a csomópont, megadhatja a partíciós kulcs és a replika típusú keresztül.</span><span class="sxs-lookup"><span data-stu-id="90967-242">Instead of directly specifying the node by its name, you can specify it via a partition key and the kind of replica.</span></span>

<span data-ttu-id="90967-243">További információkért lásd: [partitionselector osztályt és replicaselector osztályt](#partition_replica_selector).</span><span class="sxs-lookup"><span data-stu-id="90967-243">For further information, see [PartitionSelector and ReplicaSelector](#partition_replica_selector).</span></span>

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
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
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
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

## <a name="partitionselector-and-replicaselector"></a><span data-ttu-id="90967-244">Partitionselector osztályt és replicaselector osztályt</span><span class="sxs-lookup"><span data-stu-id="90967-244">PartitionSelector and ReplicaSelector</span></span>
### <a name="partitionselector"></a><span data-ttu-id="90967-245">Partitionselector osztályt</span><span class="sxs-lookup"><span data-stu-id="90967-245">PartitionSelector</span></span>
<span data-ttu-id="90967-246">Partitionselector osztályt tesztelhetőségi felfedett segítő, és válassza ki a tesztelhetőségi műveletek végrehajtásához egy adott partícióra szolgál.</span><span class="sxs-lookup"><span data-stu-id="90967-246">PartitionSelector is a helper exposed in testability and is used to select a specific partition on which to perform any of the testability actions.</span></span> <span data-ttu-id="90967-247">Egy adott partícióra válassza, ha a Partícióazonosító előzetesen ismert használható.</span><span class="sxs-lookup"><span data-stu-id="90967-247">It can be used to select a specific partition if the partition ID is known beforehand.</span></span> <span data-ttu-id="90967-248">Vagy megadhatja a partíciós kulcs, és a művelet feloldja a Partícióazonosító belső.</span><span class="sxs-lookup"><span data-stu-id="90967-248">Or, you can provide the partition key and the operation will resolve the partition ID internally.</span></span> <span data-ttu-id="90967-249">Lehetősége is van egy véletlenszerű partíció kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="90967-249">You also have the option of selecting a random partition.</span></span>

<span data-ttu-id="90967-250">A segítő használatához a partitionselector osztályt objektum létrehozása, és válassza ki a partíciót a Select * módszerek egyikének használatával.</span><span class="sxs-lookup"><span data-stu-id="90967-250">To use this helper, create the PartitionSelector object and select the partition by using one of the Select* methods.</span></span> <span data-ttu-id="90967-251">Az API-hoz, írja elő, akkor továbbítja a partitionselector osztályt objektumban.</span><span class="sxs-lookup"><span data-stu-id="90967-251">Then pass in the PartitionSelector object to the API that requires it.</span></span> <span data-ttu-id="90967-252">Ha nincs lehetőség van kiválasztva, alapértelmezés szerint egy véletlenszerű partícióra.</span><span class="sxs-lookup"><span data-stu-id="90967-252">If no option is selected, it defaults to a random partition.</span></span>

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

### <a name="replicaselector"></a><span data-ttu-id="90967-253">Replicaselector osztályt</span><span class="sxs-lookup"><span data-stu-id="90967-253">ReplicaSelector</span></span>
<span data-ttu-id="90967-254">Replicaselector osztályt tesztelhetőségi felfedett segítő, és használja, válasszon egy replikát a tesztelhetőségi műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="90967-254">ReplicaSelector is a helper exposed in testability and is used to help select a replica on which to perform any of the testability actions.</span></span> <span data-ttu-id="90967-255">Válasszon egy adott replikát, ha a másodpéldány-azonosító előzetesen ismert használható.</span><span class="sxs-lookup"><span data-stu-id="90967-255">It can be used to select a specific replica if the replica ID is known beforehand.</span></span> <span data-ttu-id="90967-256">Ezenkívül lehetősége nyílik egy elsődleges másodpéldány, vagy egy véletlenszerű másodlagos kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="90967-256">In addition, you have the option of selecting a primary replica or a random secondary.</span></span> <span data-ttu-id="90967-257">Replicaselector osztályt partitionselector osztályt, származik, ezért meg kell a replika és a partíció, amelyen a tesztelhetőségi művelet elvégezni kívánt egyaránt.</span><span class="sxs-lookup"><span data-stu-id="90967-257">ReplicaSelector derives from PartitionSelector, so you need to select both the replica and the partition on which you wish to perform the testability operation.</span></span>

<span data-ttu-id="90967-258">Ez a segédprogram használatához replicaselector osztályt objektum létrehozása, és állítsa be a kívánt válassza ki a replika és a partíció.</span><span class="sxs-lookup"><span data-stu-id="90967-258">To use this helper, create a ReplicaSelector object and set the way you want to select the replica and the partition.</span></span> <span data-ttu-id="90967-259">Majd továbbíthatja azt be, amely ezt az API-t.</span><span class="sxs-lookup"><span data-stu-id="90967-259">You can then pass it into the API that requires it.</span></span> <span data-ttu-id="90967-260">Ha nincs lehetőség van kiválasztva, alapértelmezés szerint egy véletlenszerű replika és a véletlenszerű partíció.</span><span class="sxs-lookup"><span data-stu-id="90967-260">If no option is selected, it defaults to a random replica and random partition.</span></span>

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a><span data-ttu-id="90967-261">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90967-261">Next steps</span></span>
* [<span data-ttu-id="90967-262">Tesztelhetőségi forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="90967-262">Testability scenarios</span></span>](service-fabric-testability-scenarios.md)
* <span data-ttu-id="90967-263">A szolgáltatás tesztelése</span><span class="sxs-lookup"><span data-stu-id="90967-263">How to test your service</span></span>
  * [<span data-ttu-id="90967-264">Szolgáltatás-munkaterhelések során hibák szimulálása</span><span class="sxs-lookup"><span data-stu-id="90967-264">Simulate failures during service workloads</span></span>](service-fabric-testability-workload-tests.md)
  * [<span data-ttu-id="90967-265">Szolgáltatások közötti kommunikációs hibái</span><span class="sxs-lookup"><span data-stu-id="90967-265">Service-to-service communication failures</span></span>](service-fabric-testability-scenarios-service-communication.md)


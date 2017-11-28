---
title: "Alkalmazásfrissítések hibaelhárítása |} Microsoft Docs"
description: "Ez a cikk a Service Fabric-alkalmazás és azok megoldását frissítése körül jelentkező gyakori problémákat tárgyalja."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 19ad152e-ec50-4327-9f19-065c875c003c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: f7f6bc0c29e2b43fbc8e451c5a4a50110b78349e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-application-upgrades"></a><span data-ttu-id="7a08f-103">Alkalmazásfrissítések hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="7a08f-103">Troubleshoot application upgrades</span></span>
<span data-ttu-id="7a08f-104">Ez a cikk ismerteti az Azure Service Fabric-alkalmazás és azok megoldását frissítése körül gyakori problémák némelyikéről.</span><span class="sxs-lookup"><span data-stu-id="7a08f-104">This article covers some of the common issues around upgrading an Azure Service Fabric application and how to resolve them.</span></span>

## <a name="troubleshoot-a-failed-application-upgrade"></a><span data-ttu-id="7a08f-105">A hibás alkalmazást-frissítés hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="7a08f-105">Troubleshoot a failed application upgrade</span></span>
<span data-ttu-id="7a08f-106">Ha egy frissítés sikertelen, a kimenetét a **Get-ServiceFabricApplicationUpgrade** parancs tartalmaz további információt a hiba a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="7a08f-106">When an upgrade fails, the output of the **Get-ServiceFabricApplicationUpgrade** command contains additional information for debugging the failure.</span></span>  <span data-ttu-id="7a08f-107">Az alábbi listában határozza meg, hogy miként legyen használható a további információt:</span><span class="sxs-lookup"><span data-stu-id="7a08f-107">The following list specifies how the additional information can be used:</span></span>

1. <span data-ttu-id="7a08f-108">Azonosítsa a hiba típusa.</span><span class="sxs-lookup"><span data-stu-id="7a08f-108">Identify the failure type.</span></span>
2. <span data-ttu-id="7a08f-109">A hiba okának azonosításához.</span><span class="sxs-lookup"><span data-stu-id="7a08f-109">Identify the failure reason.</span></span>
3. <span data-ttu-id="7a08f-110">Különítse el a további vizsgálatok esetén egy vagy több hibás összetevőt.</span><span class="sxs-lookup"><span data-stu-id="7a08f-110">Isolate one or more failing components for further investigation.</span></span>

<span data-ttu-id="7a08f-111">Ez az információ esetén érhető el a Service Fabric észleli a hibát, függetlenül attól, hogy a **FailureAction** visszaállítása vagy a frissítés felfüggesztése.</span><span class="sxs-lookup"><span data-stu-id="7a08f-111">This information is available when Service Fabric detects the failure regardless of whether the **FailureAction** is to roll back or suspend the upgrade.</span></span>

### <a name="identify-the-failure-type"></a><span data-ttu-id="7a08f-112">Azonosítsa a hiba típusa</span><span class="sxs-lookup"><span data-stu-id="7a08f-112">Identify the failure type</span></span>
<span data-ttu-id="7a08f-113">A kimenet **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** azonosítja az időbélyegzőnek (UTC), amelyen a Service Fabric frissítési hibát észlelt, és  **FailureAction** lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="7a08f-113">In the output of **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identifies the timestamp (in UTC) at which an upgrade failure was detected by Service Fabric and **FailureAction** was triggered.</span></span> <span data-ttu-id="7a08f-114">**Hibaoka** azonosítja a hiba három lehetséges magas szintű okok egyike:</span><span class="sxs-lookup"><span data-stu-id="7a08f-114">**FailureReason** identifies one of three potential high-level causes of the failure:</span></span>

1. <span data-ttu-id="7a08f-115">UpgradeDomainTimeout - azt jelzi, hogy egy adott frissítési tartomány túl sokáig tartott, és **UpgradeDomainTimeout** lejárt.</span><span class="sxs-lookup"><span data-stu-id="7a08f-115">UpgradeDomainTimeout - Indicates that a particular upgrade domain took too long to complete and **UpgradeDomainTimeout** expired.</span></span>
2. <span data-ttu-id="7a08f-116">OverallUpgradeTimeout - azt jelzi, hogy a teljes frissítés túl sokáig tartott, és **UpgradeTimeout** lejárt.</span><span class="sxs-lookup"><span data-stu-id="7a08f-116">OverallUpgradeTimeout - Indicates that the overall upgrade took too long to complete and **UpgradeTimeout** expired.</span></span>
3. <span data-ttu-id="7a08f-117">HealthCheck - azt jelzi, hogy egy frissítési tartományt a frissítés után az alkalmazás maradt a meghatározott házirendek szerint sérült és **HealthCheckRetryTimeout** lejárt.</span><span class="sxs-lookup"><span data-stu-id="7a08f-117">HealthCheck - Indicates that after upgrading an update domain, the application remained unhealthy according to the specified health policies and **HealthCheckRetryTimeout** expired.</span></span>

<span data-ttu-id="7a08f-118">Ezek a bejegyzések csak jelenik meg a kimeneti a frissítés sikertelen lesz, és elindítja a visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="7a08f-118">These entries only show up in the output when the upgrade fails and starts rolling back.</span></span> <span data-ttu-id="7a08f-119">További információk a hiba típusától függően.</span><span class="sxs-lookup"><span data-stu-id="7a08f-119">Further information is displayed depending on the type of the failure.</span></span>

### <a name="investigate-upgrade-timeouts"></a><span data-ttu-id="7a08f-120">Vizsgálja meg a frissítési időtúllépések</span><span class="sxs-lookup"><span data-stu-id="7a08f-120">Investigate upgrade timeouts</span></span>
<span data-ttu-id="7a08f-121">Frissítse az időtúllépési hibákat leggyakrabban okozzák a szolgáltatás elérhetőségével kapcsolatos problémákat.</span><span class="sxs-lookup"><span data-stu-id="7a08f-121">Upgrade timeout failures are most commonly caused by service availability issues.</span></span> <span data-ttu-id="7a08f-122">Az alábbi eredménye a tipikus, a frissítési ahol szolgáltatás replikák és a példány nem indulnak el az új kódot verzióban.</span><span class="sxs-lookup"><span data-stu-id="7a08f-122">The output following this paragraph is typical of upgrades where service replicas or instances fail to start in the new code version.</span></span> <span data-ttu-id="7a08f-123">A **UpgradeDomainProgressAtFailure** mező rögzíti a hibát időpontjában függőben lévő frissítési munka pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="7a08f-123">The **UpgradeDomainProgressAtFailure** field captures a snapshot of any pending upgrade work at the time of failure.</span></span>

```
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
```

<span data-ttu-id="7a08f-124">Ebben a példában a frissítése nem sikerült a frissítési tartomány *MYUD1* és két partíció (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* és *4b43f4d8-b26b-424e-9307-7a7a62e79750*) volt akadt-e.</span><span class="sxs-lookup"><span data-stu-id="7a08f-124">In this example, the upgrade failed at upgrade domain *MYUD1* and two partitions (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* and *4b43f4d8-b26b-424e-9307-7a7a62e79750*) were stuck.</span></span> <span data-ttu-id="7a08f-125">A partíciók akadt volt, mert a futtatókörnyezet nem helyezhető el az elsődleges replikára változott (*WaitForPrimaryPlacement*) a célcsomópontokat *csomópont1* és *csomópont4*.</span><span class="sxs-lookup"><span data-stu-id="7a08f-125">The partitions were stuck because the runtime was unable to place primary replicas (*WaitForPrimaryPlacement*) on target nodes *Node1* and *Node4*.</span></span>

<span data-ttu-id="7a08f-126">A **Get-ServiceFabricNode** parancs segítségével ellenőrizze, hogy a két csomópont frissítési tartomány *MYUD1*.</span><span class="sxs-lookup"><span data-stu-id="7a08f-126">The **Get-ServiceFabricNode** command can be used to verify that these two nodes are in upgrade domain *MYUD1*.</span></span> <span data-ttu-id="7a08f-127">A *UpgradePhase* szerint *PostUpgradeSafetyCheck*, ami azt jelenti, hogy a biztonsági ellenőrzést követően a frissítési tartomány minden csomópontja rendelkezik is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="7a08f-127">The *UpgradePhase* says *PostUpgradeSafetyCheck*, which means that these safety checks are occurring after all nodes in the upgrade domain have finished upgrading.</span></span> <span data-ttu-id="7a08f-128">Ezt az információt a problémát az új verzió az alkalmazás kódjának mutat.</span><span class="sxs-lookup"><span data-stu-id="7a08f-128">All this information points to a potential issue with the new version of the application code.</span></span> <span data-ttu-id="7a08f-129">A leggyakoribb problémák a Megnyitás vagy elsődleges kódelérési használatával való előléptetést hibákat.</span><span class="sxs-lookup"><span data-stu-id="7a08f-129">The most common issues are service errors in the open or promotion to primary code paths.</span></span>

<span data-ttu-id="7a08f-130">Egy *UpgradePhase* a *PreUpgradeSafetyCheck* azt jelenti, hogy a frissítési tartomány előkészítése előtt történt meg az problémák merültek.</span><span class="sxs-lookup"><span data-stu-id="7a08f-130">An *UpgradePhase* of *PreUpgradeSafetyCheck* means there were issues preparing the upgrade domain before it was performed.</span></span> <span data-ttu-id="7a08f-131">A leggyakoribb problémák ebben az esetben a Bezárás gombra vagy a lefokozás elsődleges kód elérési utakról a hibákat.</span><span class="sxs-lookup"><span data-stu-id="7a08f-131">The most common issues in this case are service errors in the close or demotion from primary code paths.</span></span>

<span data-ttu-id="7a08f-132">Az aktuális **UpgradeState** van *RollingBackCompleted*, így az eredeti frissítésének kell elvégezte a visszaállítás **FailureAction**, mely automatikusan összesített biztonsági hiba esetén a frissítés.</span><span class="sxs-lookup"><span data-stu-id="7a08f-132">The current **UpgradeState** is *RollingBackCompleted*, so the original upgrade must have been performed with a rollback **FailureAction**, which automatically rolled back the upgrade upon failure.</span></span> <span data-ttu-id="7a08f-133">Ha az eredeti frissítésének elvégezték a kézi **FailureAction**, majd a frissítés kellene inkább kell felfüggesztett állapotban, hogy az élő Hibakeresés az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7a08f-133">If the original upgrade was performed with a manual **FailureAction**, then the upgrade would instead be in a suspended state to allow live debugging of the application.</span></span>

### <a name="investigate-health-check-failures"></a><span data-ttu-id="7a08f-134">Vizsgálja meg a állapotának ellenőrzése sikertelen</span><span class="sxs-lookup"><span data-stu-id="7a08f-134">Investigate health check failures</span></span>
<span data-ttu-id="7a08f-135">Különböző problémákat, ami bekövetkezhet a frissítési tartományok összes csomópontjának frissítése, és átadja az biztonsági ellenőrzés befejezése után forrása a állapotának ellenőrzése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="7a08f-135">Health check failures can be triggered by various issues that can happen after all nodes in an upgrade domain finish upgrading and passing all safety checks.</span></span> <span data-ttu-id="7a08f-136">Az alábbi eredménye egy frissítési hiba miatt sikertelen állapotellenőrzést jellemző.</span><span class="sxs-lookup"><span data-stu-id="7a08f-136">The output following this paragraph is typical of an upgrade failure due to failed health checks.</span></span> <span data-ttu-id="7a08f-137">A **UnhealthyEvaluations** mező rögzíti, amely szerint a megadott frissítés alkalmával sikertelen állapotellenőrzést pillanatképe [állapotházirend](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7a08f-137">The **UnhealthyEvaluations** field captures a snapshot of health checks that failed at the time of the upgrade according to the specified [health policy](service-fabric-health-introduction.md).</span></span>

```
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
```

<span data-ttu-id="7a08f-138">Először vizsgálja állapotának ellenőrzése sikertelen kell a Service Fabric állapotmodell érteni.</span><span class="sxs-lookup"><span data-stu-id="7a08f-138">Investigating health check failures first requires an understanding of the Service Fabric health model.</span></span> <span data-ttu-id="7a08f-139">Ilyen alapos megértése, nélkül is láthatja, hogy a két szolgáltatás sérült állapotban, de: *fabric: / DemoApp/Svc3* és *fabric: / DemoApp/Svc2*, együtt a hiba állapotjelentések ("InjectedFault" Ebben az esetben).</span><span class="sxs-lookup"><span data-stu-id="7a08f-139">But even without such an in-depth understanding, we can see that two services are unhealthy: *fabric:/DemoApp/Svc3* and *fabric:/DemoApp/Svc2*, along with the error health reports ("InjectedFault" in this case).</span></span> <span data-ttu-id="7a08f-140">Ebben a példában két négy szolgáltatások sérült állapotban, amelyek nem éri el az alapértelmezett cél sérült 0 % (*MaxPercentUnhealthyServices*).</span><span class="sxs-lookup"><span data-stu-id="7a08f-140">In this example, two out of four services are unhealthy, which is below the default target of 0% unhealthy (*MaxPercentUnhealthyServices*).</span></span>

<span data-ttu-id="7a08f-141">A frissítés fel lett függesztve, hibás megadásával egy **FailureAction** manuális, ha a frissítés megkezdése.</span><span class="sxs-lookup"><span data-stu-id="7a08f-141">The upgrade was suspended upon failing by specifying a **FailureAction** of manual when starting the upgrade.</span></span> <span data-ttu-id="7a08f-142">Ez a mód lehetővé teszi vizsgálja meg az élő rendszer a hibás állapotban lévő minden további megtétele előtt.</span><span class="sxs-lookup"><span data-stu-id="7a08f-142">This mode allows us to investigate the live system in the failed state before taking any further action.</span></span>

### <a name="recover-from-a-suspended-upgrade"></a><span data-ttu-id="7a08f-143">A felfüggesztett frissítés helyreállítása</span><span class="sxs-lookup"><span data-stu-id="7a08f-143">Recover from a suspended upgrade</span></span>
<span data-ttu-id="7a08f-144">A visszaállítás **FailureAction**, az egyetlen helyreállítási szükséges, mert a frissítés automatikusan visszaállítja a meghibásodása esetén.</span><span class="sxs-lookup"><span data-stu-id="7a08f-144">With a rollback **FailureAction**, there is no recovery needed since the upgrade automatically rolls back upon failing.</span></span> <span data-ttu-id="7a08f-145">A kézi **FailureAction**, több helyreállítási lehetőség:</span><span class="sxs-lookup"><span data-stu-id="7a08f-145">With a manual **FailureAction**, there are several recovery options:</span></span>

1.  <span data-ttu-id="7a08f-146">a visszaállítás eseményindító</span><span class="sxs-lookup"><span data-stu-id="7a08f-146">trigger a rollback</span></span>
2. <span data-ttu-id="7a08f-147">Végezze el a frissítési fennmaradó manuális</span><span class="sxs-lookup"><span data-stu-id="7a08f-147">Proceed through the remainder of the upgrade manually</span></span>
3. <span data-ttu-id="7a08f-148">A figyelt frissítésének folytatása</span><span class="sxs-lookup"><span data-stu-id="7a08f-148">Resume the monitored upgrade</span></span>

<span data-ttu-id="7a08f-149">A **Start-ServiceFabricApplicationRollback** parancs használható bármikor visszaállítása az alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="7a08f-149">The **Start-ServiceFabricApplicationRollback** command can be used at any time to start rolling back the application.</span></span> <span data-ttu-id="7a08f-150">A parancs sikeresen adja vissza, ha a visszaállítási kérelem regisztrálva van a rendszerben, és ezt követően hamarosan elindul.</span><span class="sxs-lookup"><span data-stu-id="7a08f-150">Once the command returns successfully, the rollback request has been registered in the system and starts shortly thereafter.</span></span>

<span data-ttu-id="7a08f-151">A **Resume-ServiceFabricApplicationUpgrade** parancs segítségével végezze el a frissítési fennmaradó manuálisan, egyszerre több frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="7a08f-151">The **Resume-ServiceFabricApplicationUpgrade** command can be used to proceed through the remainder of the upgrade manually, one upgrade domain at a time.</span></span> <span data-ttu-id="7a08f-152">Ebben az üzemmódban csak biztonsági ellenőrzéseket hajtja végre a rendszer.</span><span class="sxs-lookup"><span data-stu-id="7a08f-152">In this mode, only safety checks are performed by the system.</span></span> <span data-ttu-id="7a08f-153">Nincs több állapotellenőrzést végez.</span><span class="sxs-lookup"><span data-stu-id="7a08f-153">No more health checks are performed.</span></span> <span data-ttu-id="7a08f-154">Ez a parancs csak lehet használni a *UpgradeState* látható *RollingForwardPending*, ami azt jelenti, hogy az aktuális frissítési tartomány frissítése befejeződött, de a következő nem indult el (függőben).</span><span class="sxs-lookup"><span data-stu-id="7a08f-154">This command can only be used when the *UpgradeState* shows *RollingForwardPending*, which means that the current upgrade domain has finished upgrading but the next one has not started (pending).</span></span>

<span data-ttu-id="7a08f-155">A **frissítés-ServiceFabricApplicationUpgrade** végrehajtás alatt álló ellenőrzi és folytatni a figyelt frissítés mindkét biztonsági parancs használható.</span><span class="sxs-lookup"><span data-stu-id="7a08f-155">The **Update-ServiceFabricApplicationUpgrade** command can be used to resume the monitored upgrade with both safety and health checks being performed.</span></span>

```
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
```

<span data-ttu-id="7a08f-156">A frissítés továbbra is fennáll, a frissítési tartomány, ahol utolsó felfüggesztették és azonos frissítse a paraméterek és az állapotházirendeket, mielőtt használja.</span><span class="sxs-lookup"><span data-stu-id="7a08f-156">The upgrade continues from the upgrade domain where it was last suspended and use the same upgrade parameters and health policies as before.</span></span> <span data-ttu-id="7a08f-157">Ha szükséges, a frissítési paraméterek és a fenti kimenetben megjelenő állapotházirendeket módosítható ugyanazzal a paranccsal, ha a frissítés folytatása.</span><span class="sxs-lookup"><span data-stu-id="7a08f-157">If needed, any of the upgrade parameters and health policies shown in the preceding output can be changed in the same command when the upgrade resumes.</span></span> <span data-ttu-id="7a08f-158">Ebben a példában a frissítés a figyelt módban, a paraméterek és az állapotházirendeket változatlan folytatódik.</span><span class="sxs-lookup"><span data-stu-id="7a08f-158">In this example, the upgrade was resumed in Monitored mode, with the parameters and the health policies unchanged.</span></span>

## <a name="further-troubleshooting"></a><span data-ttu-id="7a08f-159">További hibaelhárításhoz</span><span class="sxs-lookup"><span data-stu-id="7a08f-159">Further troubleshooting</span></span>
### <a name="service-fabric-is-not-following-the-specified-health-policies"></a><span data-ttu-id="7a08f-160">A Service Fabric nem követi a megadott házirendek</span><span class="sxs-lookup"><span data-stu-id="7a08f-160">Service Fabric is not following the specified health policies</span></span>
<span data-ttu-id="7a08f-161">Lehetséges ok: 1:</span><span class="sxs-lookup"><span data-stu-id="7a08f-161">Possible Cause 1:</span></span>

<span data-ttu-id="7a08f-162">A Service Fabric összes százalékos fordítja le az entitások (például a replikákat, partíciók és szolgáltatások) állapotának kiértékelését tényleges számok, és mindig teljes entitások kerekít.</span><span class="sxs-lookup"><span data-stu-id="7a08f-162">Service Fabric translates all percentages into actual numbers of entities (for example, replicas, partitions, and services) for health evaluation and always rounds up to whole entities.</span></span> <span data-ttu-id="7a08f-163">Például ha a maximálisan engedélyezett *MaxPercentUnhealthyReplicasPerPartition* 21 %, és öt replikákat, akkor a Service Fabric lehetővé teszi, hogy legfeljebb két nem megfelelő állapotú replika (Ez azt jelenti, hogy`Math.Ceiling (5*0.21)`).</span><span class="sxs-lookup"><span data-stu-id="7a08f-163">For example, if the maximum *MaxPercentUnhealthyReplicasPerPartition* is 21% and there are five replicas, then Service Fabric allows up to two unhealthy replicas (that is,`Math.Ceiling (5*0.21)`).</span></span> <span data-ttu-id="7a08f-164">Ebből kifolyólag állapotházirendeket ennek megfelelően kell állítani.</span><span class="sxs-lookup"><span data-stu-id="7a08f-164">Thus, health policies should be set accordingly.</span></span>

<span data-ttu-id="7a08f-165">2 lehetséges ok:</span><span class="sxs-lookup"><span data-stu-id="7a08f-165">Possible Cause 2:</span></span>

<span data-ttu-id="7a08f-166">Házirendek teljes szolgáltatások és szolgáltatáspéldány nem adott százalékos értékben vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="7a08f-166">Health policies are specified in terms of percentages of total services and not specific service instances.</span></span> <span data-ttu-id="7a08f-167">Például a frissítés, ha egy alkalmazás négy előtt szolgáltatás példányok A, B, C és D, ahol D szolgáltatás állapota nem megfelelő, de minimális hatással van az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="7a08f-167">For example, before an upgrade, if an application has four service instances A, B, C, and D, where service D is unhealthy but with little impact to the application.</span></span> <span data-ttu-id="7a08f-168">Azt szeretnénk, figyelmen kívül hagyja az ismert sérült szolgáltatás D frissítés során, és a paraméter *MaxPercentUnhealthyServices* 25 %, feltéve, hogy csak A, B és C kell lennie kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="7a08f-168">We want to ignore the known unhealthy service D during upgrade and set the parameter *MaxPercentUnhealthyServices* to be 25%, assuming only A, B, and C need to be healthy.</span></span>

<span data-ttu-id="7a08f-169">Azonban a frissítés során D válhat kifogástalan közben C akkor kerül sérült állapotba.</span><span class="sxs-lookup"><span data-stu-id="7a08f-169">However, during the upgrade, D may become healthy while C becomes unhealthy.</span></span> <span data-ttu-id="7a08f-170">A frissítés továbbra is szeretné sikertelen, mert a szolgáltatások csak 25 %-át sérült állapotban.</span><span class="sxs-lookup"><span data-stu-id="7a08f-170">The upgrade would still succeed because only 25% of the services are unhealthy.</span></span> <span data-ttu-id="7a08f-171">Azonban váratlan hibák miatt váratlanul sérült helyett D. alatt C eredményezi Ebben a helyzetben D kell modellezni egy másik szolgáltatás típusa a, B és c kiszolgálóra. Állapotházirendeket szolgáltatás meg van adva, mert különböző szolgáltatásokhoz különböző sérült százalékos küszöbérték alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="7a08f-171">However, it might result in unanticipated errors due to C being unexpectedly unhealthy instead of D. In this situation, D should be modeled as a different service type from A, B, and C. Since health policies are specified per service type, different unhealthy percentage thresholds can be applied to different services.</span></span> 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a><span data-ttu-id="7a08f-172">Az alkalmazásfrissítés állapotházirend nem szeretnék adta meg, de néhány soha nem megadott időtúllépési továbbra is sikertelen, a frissítés</span><span class="sxs-lookup"><span data-stu-id="7a08f-172">I did not specify a health policy for application upgrade, but the upgrade still fails for some time-outs that I never specified</span></span>
<span data-ttu-id="7a08f-173">Ha állapotházirendeket nem biztosított a frissítési kérelmet, hogy azok kell venni a *ApplicationManifest.xml* aktuális alkalmazás verziója.</span><span class="sxs-lookup"><span data-stu-id="7a08f-173">When health policies aren't provided to the upgrade request, they are taken from the *ApplicationManifest.xml* of the current application version.</span></span> <span data-ttu-id="7a08f-174">Például ha az 1.0-s verziója a 2.0-s verziójában, alkalmazás állapotházirendeket 1.0-s verziója a megadott alkalmazás X frissít szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="7a08f-174">For example, if you're upgrading Application X from version 1.0 to version 2.0, application health policies specified for in version 1.0 are used.</span></span> <span data-ttu-id="7a08f-175">Ha a frissítés egy másik állapotházirend kell használni, majd a házirend kell adni a frissítési API-hívás alkalmazás részeként.</span><span class="sxs-lookup"><span data-stu-id="7a08f-175">If a different health policy should be used for the upgrade, then the policy needs to be specified as part of the application upgrade API call.</span></span> <span data-ttu-id="7a08f-176">A frissítés során csak alkalmazni a házirendeket, az API-hívás megadott.</span><span class="sxs-lookup"><span data-stu-id="7a08f-176">The policies specified as part of the API call only apply during the upgrade.</span></span> <span data-ttu-id="7a08f-177">Ha a frissítés befejeződött, a házirendek szerepel a *ApplicationManifest.xml* szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="7a08f-177">Once the upgrade is complete, the policies specified in the *ApplicationManifest.xml* are used.</span></span>

### <a name="incorrect-time-outs-are-specified"></a><span data-ttu-id="7a08f-178">Helytelen időtúllépéseket vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="7a08f-178">Incorrect time-outs are specified</span></span>
<span data-ttu-id="7a08f-179">Akkor lehet, hogy rendelkezik már azon, hogy következményeiről időtúllépéseket érvénytelenként van beállítva.</span><span class="sxs-lookup"><span data-stu-id="7a08f-179">You may have wondered about what happens when time-outs are set inconsistently.</span></span> <span data-ttu-id="7a08f-180">Például előfordulhat, hogy egy *UpgradeTimeout* meg kisebb, mint a *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="7a08f-180">For example, you may have an *UpgradeTimeout* that's less than the *UpgradeDomainTimeout*.</span></span> <span data-ttu-id="7a08f-181">A válasz hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="7a08f-181">The answer is that an error is returned.</span></span> <span data-ttu-id="7a08f-182">Hibák vannak adott vissza, ha a *UpgradeDomainTimeout* kisebb, mint a *HealthCheckWaitDuration* és *HealthCheckRetryTimeout*, vagy ha  *UpgradeDomainTimeout* kisebb, mint a összege *HealthCheckWaitDuration* és *HealthCheckStableDuration*.</span><span class="sxs-lookup"><span data-stu-id="7a08f-182">Errors are returned if the *UpgradeDomainTimeout* is less than the sum of *HealthCheckWaitDuration* and *HealthCheckRetryTimeout*, or if *UpgradeDomainTimeout* is less than the sum of *HealthCheckWaitDuration* and *HealthCheckStableDuration*.</span></span>

### <a name="my-upgrades-are-taking-too-long"></a><span data-ttu-id="7a08f-183">Túl sokáig tart a frissítések</span><span class="sxs-lookup"><span data-stu-id="7a08f-183">My upgrades are taking too long</span></span>
<span data-ttu-id="7a08f-184">A frissítés befejezéséhez ideje az állapot-ellenőrzési eredményeire és a megadott időtúllépéseket függ.</span><span class="sxs-lookup"><span data-stu-id="7a08f-184">The time for an upgrade to complete depends on the health checks and time-outs specified.</span></span> <span data-ttu-id="7a08f-185">Mennyi ideig tart, telepítése, és az alkalmazás stabilizálását függő állapot-ellenőrzési eredményeire és időtúllépéseket.</span><span class="sxs-lookup"><span data-stu-id="7a08f-185">Health checks and time-outs depend on how long it takes to copy, deploy, and stabilize the application.</span></span> <span data-ttu-id="7a08f-186">Túl agresszív a várakozási idő alatt több sikertelen frissítések jelentheti, ezért azt javasoljuk, hosszabb időtúllépéseket konzervatív módon kezdve.</span><span class="sxs-lookup"><span data-stu-id="7a08f-186">Being too aggressive with time-outs might mean more failed upgrades, so we recommend starting conservatively with longer time-outs.</span></span>

<span data-ttu-id="7a08f-187">Itt gyors frissítő megtalálható a várakozási idő és a frissítési alkalommal együttműködését:</span><span class="sxs-lookup"><span data-stu-id="7a08f-187">Here's a quick refresher on how the time-outs interact with the upgrade times:</span></span>

<span data-ttu-id="7a08f-188">Frissíti a frissítési tartományok nem tudja elvégezni gyorsabb, mint a *HealthCheckWaitDuration* + *HealthCheckStableDuration*.</span><span class="sxs-lookup"><span data-stu-id="7a08f-188">Upgrades for an upgrade domain cannot complete faster than *HealthCheckWaitDuration* + *HealthCheckStableDuration*.</span></span>

<span data-ttu-id="7a08f-189">Frissítés nem sikerülne addig nem kerülhet sor gyorsabb, mint a *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.</span><span class="sxs-lookup"><span data-stu-id="7a08f-189">Upgrade failure cannot occur faster than *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.</span></span>

<span data-ttu-id="7a08f-190">A frissítési idő egy frissítési tartomány korlátozza *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="7a08f-190">The upgrade time for an upgrade domain is limited by *UpgradeDomainTimeout*.</span></span>  <span data-ttu-id="7a08f-191">Ha *HealthCheckRetryTimeout* és *HealthCheckStableDuration* mindkettő nem nulla, és az alkalmazás állapotának tartja oda-vissza vált, akkor a frissítés végül időtúllépése a *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="7a08f-191">If *HealthCheckRetryTimeout* and *HealthCheckStableDuration* are both non-zero and the health of the application keeps switching back and forth, then the upgrade eventually times out on *UpgradeDomainTimeout*.</span></span> <span data-ttu-id="7a08f-192">*UpgradeDomainTimeout* számbavételi le egyszer a frissítés az aktuális frissítési tartomány kezdete a kezdődik.</span><span class="sxs-lookup"><span data-stu-id="7a08f-192">*UpgradeDomainTimeout* starts counting down once the upgrade for the current upgrade domain begins.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a08f-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7a08f-193">Next steps</span></span>
<span data-ttu-id="7a08f-194">[Az alkalmazás használata a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="7a08f-194">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>

<span data-ttu-id="7a08f-195">[Az alkalmazás használatával Powershell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="7a08f-195">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>

<span data-ttu-id="7a08f-196">Szabályozhatja, hogy az alkalmazás használatával frissíti [frissítése paraméterek](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="7a08f-196">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>

<span data-ttu-id="7a08f-197">Az alkalmazásfrissítéseket által használatának megtanulása kompatibilissé [Adatszerializálás](service-fabric-application-upgrade-data-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="7a08f-197">Make your application upgrades compatible by learning how to use [Data Serialization](service-fabric-application-upgrade-data-serialization.md).</span></span>

<span data-ttu-id="7a08f-198">Összetettebb funkciók használata az alkalmazás frissítésekor szakaszra [Speciális témakörök](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="7a08f-198">Learn how to use advanced functionality while upgrading your application by referring to [Advanced Topics](service-fabric-application-upgrade-advanced.md).</span></span>

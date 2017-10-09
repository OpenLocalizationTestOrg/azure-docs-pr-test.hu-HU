---
title: "alkalmazásfrissítések aaaTroubleshooting |} Microsoft Docs"
description: "Ez a cikk a Service Fabric-alkalmazás frissítése körül jelentkező gyakori problémákat tárgyalja, és hogyan tooresolve őket."
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
ms.openlocfilehash: 0f56fa61db9b4e32824623f162dc1bfe7fda0f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-application-upgrades"></a><span data-ttu-id="1fd2f-103">Alkalmazásfrissítések hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="1fd2f-103">Troubleshoot application upgrades</span></span>
<span data-ttu-id="1fd2f-104">Ez a cikk ismertet néhány gyakori problémákat hello frissítése az Azure Service Fabric-alkalmazás körül, és hogyan tooresolve őket.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-104">This article covers some of hello common issues around upgrading an Azure Service Fabric application and how tooresolve them.</span></span>

## <a name="troubleshoot-a-failed-application-upgrade"></a><span data-ttu-id="1fd2f-105">A hibás alkalmazást-frissítés hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="1fd2f-105">Troubleshoot a failed application upgrade</span></span>
<span data-ttu-id="1fd2f-106">Ha egy frissítés sikertelen, hello hello kimenete **Get-ServiceFabricApplicationUpgrade** parancs hello hiba hibakereséshez további információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-106">When an upgrade fails, hello output of hello **Get-ServiceFabricApplicationUpgrade** command contains additional information for debugging hello failure.</span></span>  <span data-ttu-id="1fd2f-107">hello következő lista megadja, hogy miként legyen használható hello további információt:</span><span class="sxs-lookup"><span data-stu-id="1fd2f-107">hello following list specifies how hello additional information can be used:</span></span>

1. <span data-ttu-id="1fd2f-108">Azonosítsa a hello hiba típusa.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-108">Identify hello failure type.</span></span>
2. <span data-ttu-id="1fd2f-109">Azonosítsa a hello hiba okát is megadva.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-109">Identify hello failure reason.</span></span>
3. <span data-ttu-id="1fd2f-110">Különítse el a további vizsgálatok esetén egy vagy több hibás összetevőt.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-110">Isolate one or more failing components for further investigation.</span></span>

<span data-ttu-id="1fd2f-111">Ez az információ esetén érhető el a Service Fabric észleli hello hiba, függetlenül attól, hogy hello **FailureAction** tooroll vissza vagy felfüggesztése hello frissítését.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-111">This information is available when Service Fabric detects hello failure regardless of whether hello **FailureAction** is tooroll back or suspend hello upgrade.</span></span>

### <a name="identify-hello-failure-type"></a><span data-ttu-id="1fd2f-112">Azonosítsa a hello hiba típusa</span><span class="sxs-lookup"><span data-stu-id="1fd2f-112">Identify hello failure type</span></span>
<span data-ttu-id="1fd2f-113">Hello kimenetét a **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** hello időbélyegzőnek (UTC), amellyel frissítési hibát észlelt a Service Fabric azonosítja és  **FailureAction** lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-113">In hello output of **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identifies hello timestamp (in UTC) at which an upgrade failure was detected by Service Fabric and **FailureAction** was triggered.</span></span> <span data-ttu-id="1fd2f-114">**Hibaoka** azonosítja hello hiba három lehetséges magas szintű okok egyike:</span><span class="sxs-lookup"><span data-stu-id="1fd2f-114">**FailureReason** identifies one of three potential high-level causes of hello failure:</span></span>

1. <span data-ttu-id="1fd2f-115">UpgradeDomainTimeout - azt jelzi, hogy egy adott frissítési tartomány tartott túl hosszú toocomplete és **UpgradeDomainTimeout** lejárt.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-115">UpgradeDomainTimeout - Indicates that a particular upgrade domain took too long toocomplete and **UpgradeDomainTimeout** expired.</span></span>
2. <span data-ttu-id="1fd2f-116">OverallUpgradeTimeout - azt jelzi, hogy a teljes frissítési hello toocomplete túl sokáig tartott, és **UpgradeTimeout** lejárt.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-116">OverallUpgradeTimeout - Indicates that hello overall upgrade took too long toocomplete and **UpgradeTimeout** expired.</span></span>
3. <span data-ttu-id="1fd2f-117">HealthCheck - azt jelzi, hogy egy frissítési tartományt a frissítés után hello alkalmazás maradt sérült toohello szerint megadott házirendek és **HealthCheckRetryTimeout** lejárt.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-117">HealthCheck - Indicates that after upgrading an update domain, hello application remained unhealthy according toohello specified health policies and **HealthCheckRetryTimeout** expired.</span></span>

<span data-ttu-id="1fd2f-118">Ezek a bejegyzések csak jelennek meg az hello kimeneti hello frissítés meghiúsul, és elindítja a visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-118">These entries only show up in hello output when hello upgrade fails and starts rolling back.</span></span> <span data-ttu-id="1fd2f-119">További információk a hello hiba hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-119">Further information is displayed depending on hello type of hello failure.</span></span>

### <a name="investigate-upgrade-timeouts"></a><span data-ttu-id="1fd2f-120">Vizsgálja meg a frissítési időtúllépések</span><span class="sxs-lookup"><span data-stu-id="1fd2f-120">Investigate upgrade timeouts</span></span>
<span data-ttu-id="1fd2f-121">Frissítse az időtúllépési hibákat leggyakrabban okozzák a szolgáltatás elérhetőségével kapcsolatos problémákat.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-121">Upgrade timeout failures are most commonly caused by service availability issues.</span></span> <span data-ttu-id="1fd2f-122">hello alábbi eredménye a frissítési tipikus Ha szolgáltatás replikákat vagy a példány nem toostart hello új kódot verzióban.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-122">hello output following this paragraph is typical of upgrades where service replicas or instances fail toostart in hello new code version.</span></span> <span data-ttu-id="1fd2f-123">Hello **UpgradeDomainProgressAtFailure** mező hiba hello időpontban függőben lévő frissítési munka pillanatképe rögzíti.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-123">hello **UpgradeDomainProgressAtFailure** field captures a snapshot of any pending upgrade work at hello time of failure.</span></span>

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

<span data-ttu-id="1fd2f-124">Ebben a példában a hello frissítése nem sikerült a frissítési tartomány *MYUD1* és két partíció (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* és *4b43f4d8-b26b-424e-9307-7a7a62e79750*) volt akadt-e.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-124">In this example, hello upgrade failed at upgrade domain *MYUD1* and two partitions (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* and *4b43f4d8-b26b-424e-9307-7a7a62e79750*) were stuck.</span></span> <span data-ttu-id="1fd2f-125">hello partíciók akadt volt, mert hello futásidejű nem tooplace elsődleges replikára változott (*WaitForPrimaryPlacement*) a célcsomópontokat *csomópont1* és *csomópont4*.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-125">hello partitions were stuck because hello runtime was unable tooplace primary replicas (*WaitForPrimaryPlacement*) on target nodes *Node1* and *Node4*.</span></span>

<span data-ttu-id="1fd2f-126">Hello **Get-ServiceFabricNode** parancs lehet frissítési tartomány a két csomópont által használt tooverify *MYUD1*.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-126">hello **Get-ServiceFabricNode** command can be used tooverify that these two nodes are in upgrade domain *MYUD1*.</span></span> <span data-ttu-id="1fd2f-127">Hello *UpgradePhase* szerint *PostUpgradeSafetyCheck*, ami azt jelenti, hogy a biztonsági ellenőrzést követően hello frissítési tartomány minden csomópontja rendelkezik is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-127">hello *UpgradePhase* says *PostUpgradeSafetyCheck*, which means that these safety checks are occurring after all nodes in hello upgrade domain have finished upgrading.</span></span> <span data-ttu-id="1fd2f-128">Ezt az információt mutat tooa lehetséges problémát a hello alkalmazáskód hello új verziójával.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-128">All this information points tooa potential issue with hello new version of hello application code.</span></span> <span data-ttu-id="1fd2f-129">hello leggyakoribb problémák nyitott hello vagy előléptetés tooprimary kód elérési hibákat.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-129">hello most common issues are service errors in hello open or promotion tooprimary code paths.</span></span>

<span data-ttu-id="1fd2f-130">Egy *UpgradePhase* a *PreUpgradeSafetyCheck* azt jelenti, hogy hello frissítési tartomány előkészítése előtt történt meg az problémák merültek.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-130">An *UpgradePhase* of *PreUpgradeSafetyCheck* means there were issues preparing hello upgrade domain before it was performed.</span></span> <span data-ttu-id="1fd2f-131">hello leggyakoribb problémák ebben az esetben olyan hello zárja be, vagy az elsődleges kódelérési lefokozás hibákat.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-131">hello most common issues in this case are service errors in hello close or demotion from primary code paths.</span></span>

<span data-ttu-id="1fd2f-132">aktuális hello **UpgradeState** van *RollingBackCompleted*, így az eredeti frissítés hello kell elvégezte a visszaállítás **FailureAction**, mely automatikusan visszaállítása hello frissítés sikertelenség esetén.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-132">hello current **UpgradeState** is *RollingBackCompleted*, so hello original upgrade must have been performed with a rollback **FailureAction**, which automatically rolled back hello upgrade upon failure.</span></span> <span data-ttu-id="1fd2f-133">Ha a kézi hello eredeti frissítés elvégezték **FailureAction**, majd hello frissítés helyette lenne egy felfüggesztett állapotban tooallow élő hello alkalmazás hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-133">If hello original upgrade was performed with a manual **FailureAction**, then hello upgrade would instead be in a suspended state tooallow live debugging of hello application.</span></span>

### <a name="investigate-health-check-failures"></a><span data-ttu-id="1fd2f-134">Vizsgálja meg a állapotának ellenőrzése sikertelen</span><span class="sxs-lookup"><span data-stu-id="1fd2f-134">Investigate health check failures</span></span>
<span data-ttu-id="1fd2f-135">Különböző problémákat, ami bekövetkezhet a frissítési tartományok összes csomópontjának frissítése, és átadja az biztonsági ellenőrzés befejezése után forrása a állapotának ellenőrzése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-135">Health check failures can be triggered by various issues that can happen after all nodes in an upgrade domain finish upgrading and passing all safety checks.</span></span> <span data-ttu-id="1fd2f-136">hello alábbi eredménye egy frissítési hiba miatt toofailed állapotellenőrzést jellemző.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-136">hello output following this paragraph is typical of an upgrade failure due toofailed health checks.</span></span> <span data-ttu-id="1fd2f-137">Hello **UnhealthyEvaluations** mező rögzíti a pillanatkép készítése sikertelen a következő hello idő szerint megadott toohello hello frissítési állapot-ellenőrzési eredményeire [állapotházirend](service-fabric-health-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1fd2f-137">hello **UnhealthyEvaluations** field captures a snapshot of health checks that failed at hello time of hello upgrade according toohello specified [health policy](service-fabric-health-introduction.md).</span></span>

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

<span data-ttu-id="1fd2f-138">Először vizsgálja állapotának ellenőrzése sikertelen kell hello Service Fabric állapotmodell érteni.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-138">Investigating health check failures first requires an understanding of hello Service Fabric health model.</span></span> <span data-ttu-id="1fd2f-139">Ilyen alapos megértése, nélkül is láthatja, hogy a két szolgáltatás sérült állapotban, de: *fabric: / DemoApp/Svc3* és *fabric: / DemoApp/Svc2*, hello hiba állapotjelentések ("InjectedFault együtt "Ebben az esetben).</span><span class="sxs-lookup"><span data-stu-id="1fd2f-139">But even without such an in-depth understanding, we can see that two services are unhealthy: *fabric:/DemoApp/Svc3* and *fabric:/DemoApp/Svc2*, along with hello error health reports ("InjectedFault" in this case).</span></span> <span data-ttu-id="1fd2f-140">Ebben a példában két négy szolgáltatások sérült állapotban, amelyek nem éri el hello alapértelmezett cél sérült 0 % (*MaxPercentUnhealthyServices*).</span><span class="sxs-lookup"><span data-stu-id="1fd2f-140">In this example, two out of four services are unhealthy, which is below hello default target of 0% unhealthy (*MaxPercentUnhealthyServices*).</span></span>

<span data-ttu-id="1fd2f-141">hello frissítés fel lett függesztve, hibás megadásával egy **FailureAction** , ha manuális elindítása hello frissítését.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-141">hello upgrade was suspended upon failing by specifying a **FailureAction** of manual when starting hello upgrade.</span></span> <span data-ttu-id="1fd2f-142">Ez a mód lehetővé teszi tooinvestigate hello élő rendszer hello sikertelen állapotban előtt további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-142">This mode allows us tooinvestigate hello live system in hello failed state before taking any further action.</span></span>

### <a name="recover-from-a-suspended-upgrade"></a><span data-ttu-id="1fd2f-143">A felfüggesztett frissítés helyreállítása</span><span class="sxs-lookup"><span data-stu-id="1fd2f-143">Recover from a suspended upgrade</span></span>
<span data-ttu-id="1fd2f-144">A visszaállítás **FailureAction**, nincs helyreállítási hello frissítés automatikusan visszaállítja a futtatása sikertelen, mert szükség van.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-144">With a rollback **FailureAction**, there is no recovery needed since hello upgrade automatically rolls back upon failing.</span></span> <span data-ttu-id="1fd2f-145">A kézi **FailureAction**, több helyreállítási lehetőség:</span><span class="sxs-lookup"><span data-stu-id="1fd2f-145">With a manual **FailureAction**, there are several recovery options:</span></span>

1.  <span data-ttu-id="1fd2f-146">a visszaállítás eseményindító</span><span class="sxs-lookup"><span data-stu-id="1fd2f-146">trigger a rollback</span></span>
2. <span data-ttu-id="1fd2f-147">Manuálisan végezze el a hello maradéka hello frissítése</span><span class="sxs-lookup"><span data-stu-id="1fd2f-147">Proceed through hello remainder of hello upgrade manually</span></span>
3. <span data-ttu-id="1fd2f-148">Folytatás hello figyelt frissítése</span><span class="sxs-lookup"><span data-stu-id="1fd2f-148">Resume hello monitored upgrade</span></span>

<span data-ttu-id="1fd2f-149">Hello **Start-ServiceFabricApplicationRollback** parancs minden alkalommal toostart hello alkalmazás visszaállítása lehet használni.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-149">hello **Start-ServiceFabricApplicationRollback** command can be used at any time toostart rolling back hello application.</span></span> <span data-ttu-id="1fd2f-150">Hello parancs sikeresen adja vissza, ha hello visszaállítási kérés hello rendszerben regisztrálva van, és ezt követően hamarosan elindul.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-150">Once hello command returns successfully, hello rollback request has been registered in hello system and starts shortly thereafter.</span></span>

<span data-ttu-id="1fd2f-151">Hello **Resume-ServiceFabricApplicationUpgrade** parancs használható tooproceed keresztül hello hello maradéka frissítse manuálisan, egyszerre több frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-151">hello **Resume-ServiceFabricApplicationUpgrade** command can be used tooproceed through hello remainder of hello upgrade manually, one upgrade domain at a time.</span></span> <span data-ttu-id="1fd2f-152">Ebben az üzemmódban csak biztonsági ellenőrzéseket hello rendszer hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-152">In this mode, only safety checks are performed by hello system.</span></span> <span data-ttu-id="1fd2f-153">Nincs több állapotellenőrzést végez.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-153">No more health checks are performed.</span></span> <span data-ttu-id="1fd2f-154">Ez a parancs csak használható, ha hello *UpgradeState* látható *RollingForwardPending*, ami azt jelenti, hogy hello jelenlegi frissítési tartománya befejezte a frissítését, de hello mellett egy nem indult el (függőben).</span><span class="sxs-lookup"><span data-stu-id="1fd2f-154">This command can only be used when hello *UpgradeState* shows *RollingForwardPending*, which means that hello current upgrade domain has finished upgrading but hello next one has not started (pending).</span></span>

<span data-ttu-id="1fd2f-155">Hello **frissítés-ServiceFabricApplicationUpgrade** parancsot a mind biztonsági és állapot-ellenőrzést használt tooresume figyelt hello frissítés alatt elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-155">hello **Update-ServiceFabricApplicationUpgrade** command can be used tooresume hello monitored upgrade with both safety and health checks being performed.</span></span>

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

<span data-ttu-id="1fd2f-156">hello frissítés az hello frissítési tartomány, ahol utolsó a felfüggesztés és a használata hello azonos frissítse a paraméterek és az állapotházirendeket, mielőtt folytatódik.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-156">hello upgrade continues from hello upgrade domain where it was last suspended and use hello same upgrade parameters and health policies as before.</span></span> <span data-ttu-id="1fd2f-157">Szükség esetén hello frissítési paraméterek és az állapotházirendeket kimeneti megelőző hello látható azonos parancsot, amikor hello frissítés folytatja hello lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-157">If needed, any of hello upgrade parameters and health policies shown in hello preceding output can be changed in hello same command when hello upgrade resumes.</span></span> <span data-ttu-id="1fd2f-158">Ebben a példában a figyelt módban, hello paraméterek és változatlan hello állapotházirendeket hello frissítés folytatódik.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-158">In this example, hello upgrade was resumed in Monitored mode, with hello parameters and hello health policies unchanged.</span></span>

## <a name="further-troubleshooting"></a><span data-ttu-id="1fd2f-159">További hibaelhárításhoz</span><span class="sxs-lookup"><span data-stu-id="1fd2f-159">Further troubleshooting</span></span>
### <a name="service-fabric-is-not-following-hello-specified-health-policies"></a><span data-ttu-id="1fd2f-160">A Service Fabric nem követi a megadott hello házirendek</span><span class="sxs-lookup"><span data-stu-id="1fd2f-160">Service Fabric is not following hello specified health policies</span></span>
<span data-ttu-id="1fd2f-161">Lehetséges ok: 1:</span><span class="sxs-lookup"><span data-stu-id="1fd2f-161">Possible Cause 1:</span></span>

<span data-ttu-id="1fd2f-162">A Service Fabric összes százalékos fordítja le az entitások (például a replikákat, partíciók és szolgáltatások) tényleges szám állapotának értékeléséhez, és mindig toowhole entitások kerekít.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-162">Service Fabric translates all percentages into actual numbers of entities (for example, replicas, partitions, and services) for health evaluation and always rounds up toowhole entities.</span></span> <span data-ttu-id="1fd2f-163">Például, ha hello maximális *MaxPercentUnhealthyReplicasPerPartition* 21 %, és öt replikákat, majd a Service Fabric lehetővé teszi, hogy másolatot tootwo sérült replikák (Ez azt jelenti, hogy`Math.Ceiling (5*0.21)`).</span><span class="sxs-lookup"><span data-stu-id="1fd2f-163">For example, if hello maximum *MaxPercentUnhealthyReplicasPerPartition* is 21% and there are five replicas, then Service Fabric allows up tootwo unhealthy replicas (that is,`Math.Ceiling (5*0.21)`).</span></span> <span data-ttu-id="1fd2f-164">Ebből kifolyólag állapotházirendeket ennek megfelelően kell állítani.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-164">Thus, health policies should be set accordingly.</span></span>

<span data-ttu-id="1fd2f-165">2 lehetséges ok:</span><span class="sxs-lookup"><span data-stu-id="1fd2f-165">Possible Cause 2:</span></span>

<span data-ttu-id="1fd2f-166">Házirendek teljes szolgáltatások és szolgáltatáspéldány nem adott százalékos értékben vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-166">Health policies are specified in terms of percentages of total services and not specific service instances.</span></span> <span data-ttu-id="1fd2f-167">Például a frissítés, ha egy alkalmazás négy előtt szolgáltatás példányok A, B, C és D, ahol D szolgáltatás állapota nem megfelelő, de csekély hatást toohello alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-167">For example, before an upgrade, if an application has four service instances A, B, C, and D, where service D is unhealthy but with little impact toohello application.</span></span> <span data-ttu-id="1fd2f-168">Azt szeretnénk, hogy közben a frissítést, és állítsa be hello paraméter nem megfelelő állapotú szolgáltatás D ismert tooignore hello *MaxPercentUnhealthyServices* toobe 25 %, feltéve, hogy csak A, B és C kell toobe kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-168">We want tooignore hello known unhealthy service D during upgrade and set hello parameter *MaxPercentUnhealthyServices* toobe 25%, assuming only A, B, and C need toobe healthy.</span></span>

<span data-ttu-id="1fd2f-169">Azonban hello a frissítés során D válhat kifogástalan közben C akkor kerül sérült állapotba.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-169">However, during hello upgrade, D may become healthy while C becomes unhealthy.</span></span> <span data-ttu-id="1fd2f-170">hello frissítés továbbra is szeretné sikertelen, mert csak 25 %-át hello szolgáltatások sérült állapotban.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-170">hello upgrade would still succeed because only 25% of hello services are unhealthy.</span></span> <span data-ttu-id="1fd2f-171">Azonban váratlan hibák miatt tooC alatt váratlanul sérült helyett D. eredményezi Ebben a helyzetben D kell modellezni egy másik szolgáltatás típusa a, B és c kiszolgálóra. Házirendek szolgáltatás meg van adva, mert különböző sérült százalékos küszöbértékek alkalmazott toodifferent szolgáltatások is lehet.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-171">However, it might result in unanticipated errors due tooC being unexpectedly unhealthy instead of D. In this situation, D should be modeled as a different service type from A, B, and C. Since health policies are specified per service type, different unhealthy percentage thresholds can be applied toodifferent services.</span></span> 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-hello-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a><span data-ttu-id="1fd2f-172">I nem adta meg az alkalmazásfrissítés állapotházirend, de néhány soha nem megadott időtúllépési továbbra sem sikerül hello frissítése</span><span class="sxs-lookup"><span data-stu-id="1fd2f-172">I did not specify a health policy for application upgrade, but hello upgrade still fails for some time-outs that I never specified</span></span>
<span data-ttu-id="1fd2f-173">Házirendek nem biztosított toohello frissítési kérelmet, ha azok a hello végrehajtott *ApplicationManifest.xml* hello az aktuális alkalmazás verzió.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-173">When health policies aren't provided toohello upgrade request, they are taken from hello *ApplicationManifest.xml* of hello current application version.</span></span> <span data-ttu-id="1fd2f-174">Például ha alkalmazás X 1.0-s verziója tooversion 2.0 rendszert szeretne frissíteni, 1.0-s verziója a megadott alkalmazás állapotházirendeket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-174">For example, if you're upgrading Application X from version 1.0 tooversion 2.0, application health policies specified for in version 1.0 are used.</span></span> <span data-ttu-id="1fd2f-175">Ha egy másik állapotházirend hello frissítéshez kell használni, hello házirend hello alkalmazás frissítési API-hívás részeként megadott toobe van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-175">If a different health policy should be used for hello upgrade, then hello policy needs toobe specified as part of hello application upgrade API call.</span></span> <span data-ttu-id="1fd2f-176">hello hello API-hívás részeként megadott házirendek csak érvényes hello frissítés során.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-176">hello policies specified as part of hello API call only apply during hello upgrade.</span></span> <span data-ttu-id="1fd2f-177">Hello frissítés végrehajtása után hello házirendek megadott hello *ApplicationManifest.xml* szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-177">Once hello upgrade is complete, hello policies specified in hello *ApplicationManifest.xml* are used.</span></span>

### <a name="incorrect-time-outs-are-specified"></a><span data-ttu-id="1fd2f-178">Helytelen időtúllépéseket vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-178">Incorrect time-outs are specified</span></span>
<span data-ttu-id="1fd2f-179">Akkor lehet, hogy rendelkezik már azon, hogy következményeiről időtúllépéseket érvénytelenként van beállítva.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-179">You may have wondered about what happens when time-outs are set inconsistently.</span></span> <span data-ttu-id="1fd2f-180">Például előfordulhat, hogy egy *UpgradeTimeout* , amely kisebb, mint hello *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-180">For example, you may have an *UpgradeTimeout* that's less than hello *UpgradeDomainTimeout*.</span></span> <span data-ttu-id="1fd2f-181">hello választ ki kell, hogy hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-181">hello answer is that an error is returned.</span></span> <span data-ttu-id="1fd2f-182">Ha hello visszaküldött hibák *UpgradeDomainTimeout* kisebb, mint a hello összege *HealthCheckWaitDuration* és *HealthCheckRetryTimeout*, vagy ha  *UpgradeDomainTimeout* kisebb, mint a hello összege *HealthCheckWaitDuration* és *HealthCheckStableDuration*.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-182">Errors are returned if hello *UpgradeDomainTimeout* is less than hello sum of *HealthCheckWaitDuration* and *HealthCheckRetryTimeout*, or if *UpgradeDomainTimeout* is less than hello sum of *HealthCheckWaitDuration* and *HealthCheckStableDuration*.</span></span>

### <a name="my-upgrades-are-taking-too-long"></a><span data-ttu-id="1fd2f-183">Túl sokáig tart a frissítések</span><span class="sxs-lookup"><span data-stu-id="1fd2f-183">My upgrades are taking too long</span></span>
<span data-ttu-id="1fd2f-184">egy frissítési toocomplete hello ideje hello állapot-ellenőrzési eredményeire és a megadott időtúllépéseket függ.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-184">hello time for an upgrade toocomplete depends on hello health checks and time-outs specified.</span></span> <span data-ttu-id="1fd2f-185">Állapot-ellenőrzési eredményeire és időtúllépéseket függ, hogy mennyi ideig tart toocopy, telepítése és stabilizálását hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-185">Health checks and time-outs depend on how long it takes toocopy, deploy, and stabilize hello application.</span></span> <span data-ttu-id="1fd2f-186">Túl agresszív a várakozási idő alatt több sikertelen frissítések jelentheti, ezért azt javasoljuk, hosszabb időtúllépéseket konzervatív módon kezdve.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-186">Being too aggressive with time-outs might mean more failed upgrades, so we recommend starting conservatively with longer time-outs.</span></span>

<span data-ttu-id="1fd2f-187">Íme egy gyors frissítő hogyan hello időtúllépéseket hello frissítési alkalommal használják a:</span><span class="sxs-lookup"><span data-stu-id="1fd2f-187">Here's a quick refresher on how hello time-outs interact with hello upgrade times:</span></span>

<span data-ttu-id="1fd2f-188">Frissíti a frissítési tartományok nem tudja elvégezni gyorsabb, mint a *HealthCheckWaitDuration* + *HealthCheckStableDuration*.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-188">Upgrades for an upgrade domain cannot complete faster than *HealthCheckWaitDuration* + *HealthCheckStableDuration*.</span></span>

<span data-ttu-id="1fd2f-189">Frissítés nem sikerülne addig nem kerülhet sor gyorsabb, mint a *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-189">Upgrade failure cannot occur faster than *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.</span></span>

<span data-ttu-id="1fd2f-190">hello egy frissítési tartomány frissítési idő korlátozza *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-190">hello upgrade time for an upgrade domain is limited by *UpgradeDomainTimeout*.</span></span>  <span data-ttu-id="1fd2f-191">Ha *HealthCheckRetryTimeout* és *HealthCheckStableDuration* mindkettő nem nulla és hello alkalmazás állapotáról hello tartja oda-vissza vált, akkor hello frissítés végül időtúllépése a *UpgradeDomainTimeout*.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-191">If *HealthCheckRetryTimeout* and *HealthCheckStableDuration* are both non-zero and hello health of hello application keeps switching back and forth, then hello upgrade eventually times out on *UpgradeDomainTimeout*.</span></span> <span data-ttu-id="1fd2f-192">*UpgradeDomainTimeout* számbavételi egyszer indítása hello frissítés az aktuális frissítési tartomány hello kezdődik.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-192">*UpgradeDomainTimeout* starts counting down once hello upgrade for hello current upgrade domain begins.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1fd2f-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1fd2f-193">Next steps</span></span>
<span data-ttu-id="1fd2f-194">[Az alkalmazás használata a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-194">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>

<span data-ttu-id="1fd2f-195">[Az alkalmazás használatával Powershell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="1fd2f-195">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>

<span data-ttu-id="1fd2f-196">Szabályozhatja, hogy az alkalmazás használatával frissíti [frissítése paraméterek](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="1fd2f-196">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>

<span data-ttu-id="1fd2f-197">Az alkalmazásfrissítéseket Learning kompatibilissé hogyan toouse [Adatszerializálás](service-fabric-application-upgrade-data-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="1fd2f-197">Make your application upgrades compatible by learning how toouse [Data Serialization](service-fabric-application-upgrade-data-serialization.md).</span></span>

<span data-ttu-id="1fd2f-198">Ismerje meg, hogyan toouse speciális azáltal túl az alkalmazás frissítésekor funkció[Speciális témakörök](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="1fd2f-198">Learn how toouse advanced functionality while upgrading your application by referring too[Advanced Topics](service-fabric-application-upgrade-advanced.md).</span></span>

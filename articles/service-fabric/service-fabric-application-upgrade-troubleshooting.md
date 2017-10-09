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
# <a name="troubleshoot-application-upgrades"></a>Alkalmazásfrissítések hibaelhárítása
Ez a cikk ismertet néhány gyakori problémákat hello frissítése az Azure Service Fabric-alkalmazás körül, és hogyan tooresolve őket.

## <a name="troubleshoot-a-failed-application-upgrade"></a>A hibás alkalmazást-frissítés hibaelhárítása
Ha egy frissítés sikertelen, hello hello kimenete **Get-ServiceFabricApplicationUpgrade** parancs hello hiba hibakereséshez további információkat tartalmaz.  hello következő lista megadja, hogy miként legyen használható hello további információt:

1. Azonosítsa a hello hiba típusa.
2. Azonosítsa a hello hiba okát is megadva.
3. Különítse el a további vizsgálatok esetén egy vagy több hibás összetevőt.

Ez az információ esetén érhető el a Service Fabric észleli hello hiba, függetlenül attól, hogy hello **FailureAction** tooroll vissza vagy felfüggesztése hello frissítését.

### <a name="identify-hello-failure-type"></a>Azonosítsa a hello hiba típusa
Hello kimenetét a **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** hello időbélyegzőnek (UTC), amellyel frissítési hibát észlelt a Service Fabric azonosítja és  **FailureAction** lett elindítva. **Hibaoka** azonosítja hello hiba három lehetséges magas szintű okok egyike:

1. UpgradeDomainTimeout - azt jelzi, hogy egy adott frissítési tartomány tartott túl hosszú toocomplete és **UpgradeDomainTimeout** lejárt.
2. OverallUpgradeTimeout - azt jelzi, hogy a teljes frissítési hello toocomplete túl sokáig tartott, és **UpgradeTimeout** lejárt.
3. HealthCheck - azt jelzi, hogy egy frissítési tartományt a frissítés után hello alkalmazás maradt sérült toohello szerint megadott házirendek és **HealthCheckRetryTimeout** lejárt.

Ezek a bejegyzések csak jelennek meg az hello kimeneti hello frissítés meghiúsul, és elindítja a visszaállítása. További információk a hello hiba hello típusától függően.

### <a name="investigate-upgrade-timeouts"></a>Vizsgálja meg a frissítési időtúllépések
Frissítse az időtúllépési hibákat leggyakrabban okozzák a szolgáltatás elérhetőségével kapcsolatos problémákat. hello alábbi eredménye a frissítési tipikus Ha szolgáltatás replikákat vagy a példány nem toostart hello új kódot verzióban. Hello **UpgradeDomainProgressAtFailure** mező hiba hello időpontban függőben lévő frissítési munka pillanatképe rögzíti.

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

Ebben a példában a hello frissítése nem sikerült a frissítési tartomány *MYUD1* és két partíció (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* és *4b43f4d8-b26b-424e-9307-7a7a62e79750*) volt akadt-e. hello partíciók akadt volt, mert hello futásidejű nem tooplace elsődleges replikára változott (*WaitForPrimaryPlacement*) a célcsomópontokat *csomópont1* és *csomópont4*.

Hello **Get-ServiceFabricNode** parancs lehet frissítési tartomány a két csomópont által használt tooverify *MYUD1*. Hello *UpgradePhase* szerint *PostUpgradeSafetyCheck*, ami azt jelenti, hogy a biztonsági ellenőrzést követően hello frissítési tartomány minden csomópontja rendelkezik is megjelenhetnek. Ezt az információt mutat tooa lehetséges problémát a hello alkalmazáskód hello új verziójával. hello leggyakoribb problémák nyitott hello vagy előléptetés tooprimary kód elérési hibákat.

Egy *UpgradePhase* a *PreUpgradeSafetyCheck* azt jelenti, hogy hello frissítési tartomány előkészítése előtt történt meg az problémák merültek. hello leggyakoribb problémák ebben az esetben olyan hello zárja be, vagy az elsődleges kódelérési lefokozás hibákat.

aktuális hello **UpgradeState** van *RollingBackCompleted*, így az eredeti frissítés hello kell elvégezte a visszaállítás **FailureAction**, mely automatikusan visszaállítása hello frissítés sikertelenség esetén. Ha a kézi hello eredeti frissítés elvégezték **FailureAction**, majd hello frissítés helyette lenne egy felfüggesztett állapotban tooallow élő hello alkalmazás hibakeresést.

### <a name="investigate-health-check-failures"></a>Vizsgálja meg a állapotának ellenőrzése sikertelen
Különböző problémákat, ami bekövetkezhet a frissítési tartományok összes csomópontjának frissítése, és átadja az biztonsági ellenőrzés befejezése után forrása a állapotának ellenőrzése sikertelen. hello alábbi eredménye egy frissítési hiba miatt toofailed állapotellenőrzést jellemző. Hello **UnhealthyEvaluations** mező rögzíti a pillanatkép készítése sikertelen a következő hello idő szerint megadott toohello hello frissítési állapot-ellenőrzési eredményeire [állapotházirend](service-fabric-health-introduction.md).

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

Először vizsgálja állapotának ellenőrzése sikertelen kell hello Service Fabric állapotmodell érteni. Ilyen alapos megértése, nélkül is láthatja, hogy a két szolgáltatás sérült állapotban, de: *fabric: / DemoApp/Svc3* és *fabric: / DemoApp/Svc2*, hello hiba állapotjelentések ("InjectedFault együtt "Ebben az esetben). Ebben a példában két négy szolgáltatások sérült állapotban, amelyek nem éri el hello alapértelmezett cél sérült 0 % (*MaxPercentUnhealthyServices*).

hello frissítés fel lett függesztve, hibás megadásával egy **FailureAction** , ha manuális elindítása hello frissítését. Ez a mód lehetővé teszi tooinvestigate hello élő rendszer hello sikertelen állapotban előtt további lépéseket.

### <a name="recover-from-a-suspended-upgrade"></a>A felfüggesztett frissítés helyreállítása
A visszaállítás **FailureAction**, nincs helyreállítási hello frissítés automatikusan visszaállítja a futtatása sikertelen, mert szükség van. A kézi **FailureAction**, több helyreállítási lehetőség:

1.  a visszaállítás eseményindító
2. Manuálisan végezze el a hello maradéka hello frissítése
3. Folytatás hello figyelt frissítése

Hello **Start-ServiceFabricApplicationRollback** parancs minden alkalommal toostart hello alkalmazás visszaállítása lehet használni. Hello parancs sikeresen adja vissza, ha hello visszaállítási kérés hello rendszerben regisztrálva van, és ezt követően hamarosan elindul.

Hello **Resume-ServiceFabricApplicationUpgrade** parancs használható tooproceed keresztül hello hello maradéka frissítse manuálisan, egyszerre több frissítési tartományt. Ebben az üzemmódban csak biztonsági ellenőrzéseket hello rendszer hajtja végre. Nincs több állapotellenőrzést végez. Ez a parancs csak használható, ha hello *UpgradeState* látható *RollingForwardPending*, ami azt jelenti, hogy hello jelenlegi frissítési tartománya befejezte a frissítését, de hello mellett egy nem indult el (függőben).

Hello **frissítés-ServiceFabricApplicationUpgrade** parancsot a mind biztonsági és állapot-ellenőrzést használt tooresume figyelt hello frissítés alatt elvégezhető.

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

hello frissítés az hello frissítési tartomány, ahol utolsó a felfüggesztés és a használata hello azonos frissítse a paraméterek és az állapotházirendeket, mielőtt folytatódik. Szükség esetén hello frissítési paraméterek és az állapotházirendeket kimeneti megelőző hello látható azonos parancsot, amikor hello frissítés folytatja hello lehet módosítani. Ebben a példában a figyelt módban, hello paraméterek és változatlan hello állapotházirendeket hello frissítés folytatódik.

## <a name="further-troubleshooting"></a>További hibaelhárításhoz
### <a name="service-fabric-is-not-following-hello-specified-health-policies"></a>A Service Fabric nem követi a megadott hello házirendek
Lehetséges ok: 1:

A Service Fabric összes százalékos fordítja le az entitások (például a replikákat, partíciók és szolgáltatások) tényleges szám állapotának értékeléséhez, és mindig toowhole entitások kerekít. Például, ha hello maximális *MaxPercentUnhealthyReplicasPerPartition* 21 %, és öt replikákat, majd a Service Fabric lehetővé teszi, hogy másolatot tootwo sérült replikák (Ez azt jelenti, hogy`Math.Ceiling (5*0.21)`). Ebből kifolyólag állapotházirendeket ennek megfelelően kell állítani.

2 lehetséges ok:

Házirendek teljes szolgáltatások és szolgáltatáspéldány nem adott százalékos értékben vannak megadva. Például a frissítés, ha egy alkalmazás négy előtt szolgáltatás példányok A, B, C és D, ahol D szolgáltatás állapota nem megfelelő, de csekély hatást toohello alkalmazással. Azt szeretnénk, hogy közben a frissítést, és állítsa be hello paraméter nem megfelelő állapotú szolgáltatás D ismert tooignore hello *MaxPercentUnhealthyServices* toobe 25 %, feltéve, hogy csak A, B és C kell toobe kifogástalan.

Azonban hello a frissítés során D válhat kifogástalan közben C akkor kerül sérült állapotba. hello frissítés továbbra is szeretné sikertelen, mert csak 25 %-át hello szolgáltatások sérült állapotban. Azonban váratlan hibák miatt tooC alatt váratlanul sérült helyett D. eredményezi Ebben a helyzetben D kell modellezni egy másik szolgáltatás típusa a, B és c kiszolgálóra. Házirendek szolgáltatás meg van adva, mert különböző sérült százalékos küszöbértékek alkalmazott toodifferent szolgáltatások is lehet. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-hello-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>I nem adta meg az alkalmazásfrissítés állapotházirend, de néhány soha nem megadott időtúllépési továbbra sem sikerül hello frissítése
Házirendek nem biztosított toohello frissítési kérelmet, ha azok a hello végrehajtott *ApplicationManifest.xml* hello az aktuális alkalmazás verzió. Például ha alkalmazás X 1.0-s verziója tooversion 2.0 rendszert szeretne frissíteni, 1.0-s verziója a megadott alkalmazás állapotházirendeket fogja használni. Ha egy másik állapotházirend hello frissítéshez kell használni, hello házirend hello alkalmazás frissítési API-hívás részeként megadott toobe van szüksége. hello hello API-hívás részeként megadott házirendek csak érvényes hello frissítés során. Hello frissítés végrehajtása után hello házirendek megadott hello *ApplicationManifest.xml* szolgálnak.

### <a name="incorrect-time-outs-are-specified"></a>Helytelen időtúllépéseket vannak megadva.
Akkor lehet, hogy rendelkezik már azon, hogy következményeiről időtúllépéseket érvénytelenként van beállítva. Például előfordulhat, hogy egy *UpgradeTimeout* , amely kisebb, mint hello *UpgradeDomainTimeout*. hello választ ki kell, hogy hibát ad vissza. Ha hello visszaküldött hibák *UpgradeDomainTimeout* kisebb, mint a hello összege *HealthCheckWaitDuration* és *HealthCheckRetryTimeout*, vagy ha  *UpgradeDomainTimeout* kisebb, mint a hello összege *HealthCheckWaitDuration* és *HealthCheckStableDuration*.

### <a name="my-upgrades-are-taking-too-long"></a>Túl sokáig tart a frissítések
egy frissítési toocomplete hello ideje hello állapot-ellenőrzési eredményeire és a megadott időtúllépéseket függ. Állapot-ellenőrzési eredményeire és időtúllépéseket függ, hogy mennyi ideig tart toocopy, telepítése és stabilizálását hello alkalmazás. Túl agresszív a várakozási idő alatt több sikertelen frissítések jelentheti, ezért azt javasoljuk, hosszabb időtúllépéseket konzervatív módon kezdve.

Íme egy gyors frissítő hogyan hello időtúllépéseket hello frissítési alkalommal használják a:

Frissíti a frissítési tartományok nem tudja elvégezni gyorsabb, mint a *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Frissítés nem sikerülne addig nem kerülhet sor gyorsabb, mint a *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

hello egy frissítési tartomány frissítési idő korlátozza *UpgradeDomainTimeout*.  Ha *HealthCheckRetryTimeout* és *HealthCheckStableDuration* mindkettő nem nulla és hello alkalmazás állapotáról hello tartja oda-vissza vált, akkor hello frissítés végül időtúllépése a *UpgradeDomainTimeout*. *UpgradeDomainTimeout* számbavételi egyszer indítása hello frissítés az aktuális frissítési tartomány hello kezdődik.

## <a name="next-steps"></a>Következő lépések
[Az alkalmazás használata a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.

[Az alkalmazás használatával Powershell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.

Szabályozhatja, hogy az alkalmazás használatával frissíti [frissítése paraméterek](service-fabric-application-upgrade-parameters.md).

Az alkalmazásfrissítéseket Learning kompatibilissé hogyan toouse [Adatszerializálás](service-fabric-application-upgrade-data-serialization.md).

Ismerje meg, hogyan toouse speciális azáltal túl az alkalmazás frissítésekor funkció[Speciális témakörök](service-fabric-application-upgrade-advanced.md).

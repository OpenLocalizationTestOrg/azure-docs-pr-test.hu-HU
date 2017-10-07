---
title: "a rendszerállapot-jelentések aaaTroubleshoot |} Microsoft Docs"
description: "Hibaelhárítási fürt vagy az alkalmazással kapcsolatos problémák Azure Service Fabric-összetevők és a használati által küldött hello állapotjelentések ismerteti."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 52574ea7-eb37-47e0-a20a-101539177625
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: c77a6cdd0440ce5d354cd8760f40151f674a3529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-system-health-reports-tootroubleshoot"></a>Használja a rendszer állapotát jelentések tootroubleshoot
Az Azure Service Fabric összetevői jelentés hello kezdő verzióról hello fürt valamennyi entitást. Hello [a health Store adatbázisban](service-fabric-health-introduction.md#health-store) hoz létre, és törli az entitások hello rendszer jelentések alapján. Azt is rendszerezi azokat a hierarchiában, amely rögzíti az entitás interakciókat.

> [!NOTE]
> toounderstand állapotfigyelő kapcsolatos fogalmak, további információ: [Service Fabric állapotmodell](service-fabric-health-introduction.md).
> 
> 

Rendszerállapot-jelentések adja meg a fürt és az alkalmazás funkciói és keresztül állapotát jelző problémák láthatósága. Az alkalmazások és szolgáltatások rendszerállapot-jelentések győződjön meg arról, hogy az entitások vannak megvalósítva, és a Service Fabric perspektíva hello megfelelően működik. hello jelentések nem biztosít semmilyen állapotfigyelés hello üzleti logika hello szolgáltatást vagy a lefagyott folyamatok észlelése. Felhasználó szolgáltatások állapotfigyelő hello állapotadatok információt adott tootheir logikával.

> [!NOTE]
> Watchdogs állapotjelentések láthatók csak *után* hello rendszerösszetevők entitás létrehozása. Egy entitás törlésekor a hello a health Store adatbázisban automatikusan törli a hozzá társított összes rendszerállapot-jelentések. hello is igaz hello entitás új példányának létrehozásakor (például egy új állapot-nyilvántartó megőrzött replika szolgáltatáspéldány jön létre). Régi hello-példányhoz társított összes jelentések törlése és hello-tárolóból törölni.
> 
> 

hello rendszer összetevő jelentések azonosítva hello forrás, amely hello kezdődik "**System.**" előtag. Watchdogs nem használhatja ugyanazt a források előtag hello érvénytelen paramétereket tartalmazó jelentések utasítja el.
Most néhány rendszer pillantást jelentés toounderstand mi elindítja azokat, és hogyan toocorrect hello lehetséges problémákat jelölnek.

> [!NOTE]
> A Service Fabric továbbra is fennáll, a feltételek egyik fontos, hogy mi történik a hello fürt- és láthatósága javítása tooadd jelentéseket. Meglévő jelentések is fejleszthető további részletekkel toohelp hello probléma elhárításához gyorsabb.
> 
> 

## <a name="cluster-system-health-reports"></a>Fürt rendszerállapot-jelentések
hello fürt állapotfigyelő entitás hello a health Store adatbázisban automatikusan jön létre. Ha minden megfelelően működik, a rendszer a jelentés nem tartalmaz.

### <a name="neighborhood-loss"></a>Hálózatok adatvesztés
**System.Federation** egy hibát jelez, amikor azt észleli, hogy a helyek adatvesztés. hello jelentés az egyes csomópontokon, és hello tulajdonság neve tartalmazza a hello csomópont-Azonosítót. Ha egy helyek hello a Service Fabric gyűrűben teljes elveszik, általában számíthat két esemény (mindkét végének az hello résnek jelentés). További környékeken elvesznek, ha nincsenek további események.

hello jelentés hello globális bérleti idejét toolive hello időt határozza meg. hello jelentés minden hello TTL időtartama felében Újraküldte mindaddig, amíg hello feltétel aktív marad. hello esemény automatikusan törlődik, amikor ez megtörténik. Távolítsa el, ha lejárt a viselkedés garantálja az, hogy hello jelentés törlődnek a hello health store megfelelően, még akkor is, ha hello jelentéskészítési csomópont nem működik.

* **SourceId**: System.Federation
* **Tulajdonság**: kezdődik **hálózatok** és csomópont adatai
* **További lépések**: vizsgálja meg, miért hello hálózatok elvész (például ellenőrizze hello kommunikációs fürt csomópontjai között).

## <a name="node-system-health-reports"></a>Csomópont rendszerállapot-jelentések
**System.FM**, amely hello Feladatátvevőfürt-kezelő szolgáltatás jelöli, a hello hatóság gondoskodik a fürtcsomópontok kapcsolatos információkat. Minden csomópont állapotát megjelenítő System.FM egy jelentést kell rendelkeznie. hello csomópont entitások hello csomópont állapota eltávolításakor törlődnek (lásd: [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).

### <a name="node-updown"></a>A csomópont fel/le
System.FM jelentések OK, amikor hello csomópont csatlakozik hello ring (már fut). Az egy hibát jelez, amikor hello csomópont indulását hello ring (szolgáltatás leállt, vagy frissítés, vagy egyszerűen mert sikertelen volt). hello állapotfigyelő hierarchia által hello a health Store adatbázisban a korrelációs System.FM csomópont jelentések a telepített entitások hajt végre műveletet. Figyelembe vételével hello csomópont összes telepített entitás virtuális szülője. Ezen a csomóponton telepített hello entitások lekérdezések keresztül közzétett hello csomópont készként való mentése System.FM, amelyet a hello azonos példány hello entitások társított hello példányt. Amikor System.FM hello csomóponton nem működik, vagy újra (egy új példány) jelez, hello a health Store adatbázisban automatikusan törli a szükségtelenné létezhet csak hello csomópont le vagy hello hello csomópont korábbi példányán telepítve hello entitások.

* **SourceId**: System.FM
* **Tulajdonság**: állapota
* **További lépések**: hello csomópont frissítés szempontjából nem működik, ha kellene tartoznia hozzá biztonsági után frissítették. Ebben az esetben hello állapota hátsó tooOK kell váltani. Hello csomópont nem térjen vissza, vagy nem sikerül, ha a probléma hello kell további vizsgálat.

hello alábbi példa bemutatja a csomópont OK állapotot hello System.FM esemény:

```powershell
PS C:\> Get-ServiceFabricNodeHealth  _Node_0

NodeName              : _Node_0
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 8
                        SentAt                : 7/14/2017 4:54:51 PM
                        ReceivedAt            : 7/14/2017 4:55:14 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```


### <a name="certificate-expiration"></a>Tanúsítvány lejárta
**System.FabricNode** Ha hello csomópont által használt tanúsítványok lejáró hiányára. Három tanúsítvány csomópontonként: **Certificate_cluster**, **Certificate_server**, és **Certificate_default_client**. Legalább két héttel hello lejárati esetén hello jelentés állapota rendben. Ha hello lejárati két héten belül, hello jelentés típus: figyelmeztetés. Ezek az események TTL végtelen, és egy csomópont elhagyásakor hello fürt eltávolítja.

* **SourceId**: System.FabricNode
* **Tulajdonság**: kezdődik **tanúsítvány** és hello tanúsítványtípust további információt tartalmaz
* **További lépések**: hello tanúsítványok frissítése, ha azok lejáró.

### <a name="load-capacity-violation"></a>Betöltési kapacitás megsértése
Service Fabric terheléselosztó hello hiányára amikor azt észleli, hogy a csomópont-kapacitás megsértése.

* **SourceId**: System.PLB
* **Tulajdonság**: kezdődik **kapacitás**
* **További lépések**: Ellenőrizze a megadott metrikák és a nézet hello aktuális kapacitás hello csomóponton.

## <a name="application-system-health-reports"></a>Alkalmazás rendszerállapot-jelentések
**System.CM**, amely hello kezelő szolgáltatást jelenti, hello szervezet, amely felügyeli az alkalmazással kapcsolatos információkat.

### <a name="state"></a>Állapot
System.CM jelzi az OK gombra hello alkalmazás létrehozásakor vagy frissítésekor. Az tájékoztatja hello a health Store adatbázisban hello alkalmazás törlésekor, hogy az áruházból el kell távolítani.

* **SourceId**: System.CM
* **Tulajdonság**: állapota
* **További lépések**: hello alkalmazás létrehozás vagy frissítés, ha hello kezelő állapotjelentést tartalmaznia kell. Ellenkező esetben ellenőrizze a lekérdezés kiállításával hello alkalmazás hello állapota (például a PowerShell-parancsmag hello **Get-ServiceFabricApplication - ApplicationName *applicationName***).

hello alábbi példa látható hello állapot esemény hello **fabric: / WordCount** alkalmazás:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/14/2017 4:55:10 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="service-system-health-reports"></a>Szolgáltatás rendszerállapot-jelentések
**System.FM**, amely jelöli hello Feladatátvevőfürt-kezelő szolgáltatás, a hello szolgáltatót, amely kezeli a szolgáltatással kapcsolatban további információt.

### <a name="state"></a>Állapot
System.FM jelzi az OK gombra hello szolgáltatás létrehozásakor. Hello entitás a hello health store törli a hello szolgáltatás törlésekor.

* **SourceId**: System.FM
* **Tulajdonság**: állapota

hello alábbi példa azt mutatja hello állapot esemény hello szolgáltatásban **fabric: / WordCount/wordcountwebservice verziója**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountWebService -ExcludeHealthStatistics


ServiceName           : fabric:/WordCount/WordCountWebService
AggregatedHealthState : Ok
PartitionHealthStates : 
                        PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
                        AggregatedHealthState : Ok
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 14
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="service-correlation-error"></a>Korrelációs hibája
**System.PLB** egy hibát jelez, amikor azt észleli, hogy a szolgáltatás toobe szorosan összefügg az egy másik szolgáltatás frissítése létrehoz egy affinitási láncot. hello jelentés sikeres frissítés történik, ha nincs bejelölve.

* **SourceId**: System.PLB
* **Tulajdonság**: ServiceDescription objektumot
* **További lépések**: ellenőrzés hello korrelált szolgáltatások ismertetése.

## <a name="partition-system-health-reports"></a>Partíció rendszerállapot-jelentések
**System.FM**, amely hello Feladatátvevőfürt-kezelő szolgáltatást jelenti, a hello szervezet szolgáltatáspartíciók kapcsolatos információt.

### <a name="state"></a>Állapot
System.FM jelzi az OK gombra, ha hello partíció létrejött, és megfelelő állapotban. Hello entitás a hello health store törli a hello partíció törlése.

Hello partíció nem éri el hello replikakészlet minimális száma, ha hiba jelenti. Ha hello partíció nem hello replikakészlet minimális száma alatt van, de kevesebb hello cél replika, a rendszer figyelmeztetést jelenti. Hello partíció kvórumveszteségben esetén System.FM jelent hibát.

Egyéb fontos események tartalmazhat egy figyelmeztetés, ha hello újrakonfigurálás hosszabb időt vesz igénybe a vártnál, és ha hello build vártnál hosszabb ideig tart. várt hello hello build és újrakonfigurálását érvényes találhatók konfigurálható szolgáltatás forgatókönyvek alapján. Például ha a szolgáltatás egy terabájt állapot, például az SQL-adatbázis, hello build tovább tart, mint egy szolgáltatás állapotát kis mennyiségű.

* **SourceId**: System.FM
* **Tulajdonság**: állapota
* **További lépések**: hello állapota nem OK, akkor lehetséges, hogy egyes másodpéldányok még nem lettek létrehozott, megnyitott vagy előléptetett tooprimary vagy másodlagos megfelelően. Sok esetben hello okozza-e nyitva hello vagy szerepkör módosítása végrehajtása szolgáltatási hiba.

a következő példa hello kifogástalan partíció jeleníti meg:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountWebService | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics -ReplicasFilter None

PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 70
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

hello következő példa bemutatja, javasoljuk, hogy kevesebb cél replika hello állapotát. következő lépés hello tooget hello partíció leírása, amely bemutatja, hogyan van konfigurálva: **MinReplicaSetSize** három és **TargetReplicaSetSize** hét. Majd kérje le a csomópontok száma hello hello fürt: öt. Ebben az esetben két replika nem kell elhelyezni, mert hello cél található replikák száma nagyobb, mint az elérhető csomópontok hello száma.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None -ExcludeHealthStatistics


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 123
                        SentAt                : 7/14/2017 4:55:39 PM
                        ReceivedAt            : 7/14/2017 4:55:44 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/S RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/P RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:55:44 PM, LastOk = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131445250939703027
                        SentAt                : 7/14/2017 4:58:13 PM
                        ReceivedAt            : 7/14/2017 4:58:14 PM
                        TTL                   : 00:01:05
                        Description           : hello Load Balancer was unable toofind a placement for one or more of hello Service's Replicas:
                        Secondary replica could not be placed due toohello following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:56:14 PM, LastOk = 1/1/0001 12:00:00 AM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | select MinReplicaSetSize,TargetReplicaSetSize

MinReplicaSetSize TargetReplicaSetSize
----------------- --------------------
                2                    7                        

PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>A replika korlátozássértést
**System.PLB** jelentések figyelmeztetés, ha egy replika korlátozássértést észlelt, és nem helyezhető el az összes partíció replika. hello jelentés részletek megjelenítése milyen korlátozások és tulajdonságok megelőzheti a hello replika elhelyezésének.

* **SourceId**: System.PLB
* **Tulajdonság**: kezdődik **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>A replika rendszerállapot-jelentések
**System.RA**, amely jelenti, hogy hello reconfiguration agent összetevő hello hatóság hello replika állapot.

### <a name="state"></a>Állapot
**System.RA** OK jelentések hello replika létrehozásakor.

* **SourceId**: System.RA
* **Tulajdonság**: állapota

a következő példa hello jeleníti meg a megfelelő replika:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422293118721
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131445248920273536
                        SentAt                : 7/14/2017 4:54:52 PM
                        ReceivedAt            : 7/14/2017 4:55:13 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_0
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:13 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="replica-open-status"></a>Nyissa meg a replika állapota
hello a jelentés leírása hello kezdete (egyezményes világidő) hello API-hívás metódus meghívásakor.

**System.RA** hiányára Ha hello replika nyissa meg a konfigurált hello időszak hosszabb időbe telik (alapértelmezett: 30 perc). Ha hello API szolgáltatás rendelkezésre állása befolyásolja, hello jelentés kiadott sokkal gyorsabb (egy konfigurálható időszakban, az alapértelmezett érték 30 másodperc). mért hello idő hello idő hello replikátor nyissa meg és a nyitott hello szolgáltatást tartalmaz. hello tulajdonság módosítások tooOK Ha nyissa meg a hello befejeződik.

* **SourceId**: System.RA
* **Tulajdonság**: **ReplicaOpenStatus**
* **További lépések**: Ha hello rendszerállapot nem OK, vizsgálja meg, miért hello replika nyissa meg a vártnál hosszabb időbe telik.

### <a name="slow-service-api-call"></a>Lassú service API-hívás
**System.RAP** és **System.Replicator** jelentést egy figyelmeztetés, ha egy hívás toohello felhasználói szolgáltatás kód hello beállított idő hosszabb időbe telik. hello figyelmeztetés hello hívás befejezését nincs bejelölve.

* **SourceId**: System.RAP vagy System.Replicator
* **Tulajdonság**: hello lassú API hello neve. hello leírása itt hello idő hello API kapcsolatos további részletekért volt függőben.
* **További lépések**: vizsgálja meg, miért hello hívás vártnál hosszabb ideig tart.

a következő példa hello partíció kvórum elvesztése és hello vizsgálati lépéseket végre toofigure kimenő miért láthatók. Hello replikák egyike figyelmeztetési állapot, hogy az állapotát. Azt mutatja, hogy hello szolgáltatási művelet hosszabb időt vesz igénybe, mint a System.RAP által jelentett eseményt várt. Ezek az információk fogadását követően hello tovább toolook hello szolgáltatás kódot, és vizsgálja meg a hiba. Ebben az esetben hello **RunAsync** hello állapotalapú szolgáltatás megvalósítása nem kezelt kivételt jelez. hello replikák vannak újrahasznosítása, ezért nem láthatók a replikák hello figyelmeztetési állapotban. Ismételje meg az első hello állapotát, és keresse meg az eltérések a hello replika azonosítója. Bizonyos esetekben hello újrapróbálkozások tudhatja meg keresik.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 3
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Hello hibás alkalmazást hello hibakereső indításakor hello diagnosztikai események windows hello kivétel lépett fel az RunAsync a jelenjen meg:

![A Visual Studio 2015 diagnosztikai események: RunAsync hiba a fabric: / HelloWorldStatefulApplication.][1]

A Visual Studio 2015 diagnosztikai események: RunAsync hiba történt a **fabric: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Replikációs sor megtelt
**System.Replicator** hiányára hello replikációs sor megtelt. Hello elsődleges, a replikációs várólistában általában megtelik mert egy vagy több másodlagos replikák lassú tooacknowledge műveletek. A másodlagos hello Ez általában történik, ha hello szolgáltatás lassú tooapply hello műveletek. hello figyelmeztetés nincs bejelölve, ha hello várólista már nem teljes.

* **SourceId**: System.Replicator
* **Tulajdonság**: **PrimaryReplicationQueueStatus** vagy **SecondaryReplicationQueueStatus**hello replika szerepkör attól függően, hogy

### <a name="slow-naming-operations"></a>Lassú elnevezési műveletek
**System.NamingService** jelentések állapotát az elsődleges replikán, amikor egy elnevezési művelet elfogadható hosszabb időbe telik. Példák elnevezési műveletek [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) vagy [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync). Több metódust alatt található FabricClient, például a [módszereket szolgáltatás](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) vagy [tulajdonság módszereket](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).

> [!NOTE]
> hello Naming service oldja fel a neveket tooa szolgáltatáskeresés hello fürtben, és lehetővé teszi, hogy a felhasználók toomanage szolgáltatás neveit és tulajdonságait. A Service Fabric particionálva szolgáltatás megőrzött. Hello Authority Owner, amely tartalmazza az összes Service Fabric-neveket és szolgáltatásokra vonatkozó metaadatok hello partíciók egyik jelöli. hello Service Fabric műveletnevek a következők csatlakoztatott toodifferent partíciók nevű Name Owner partíciók, így hello szolgáltatás bővíthető. Tudjon meg többet az [Naming service](service-fabric-architecture.md).
> 
> 

Amikor egy elnevezési művelet a vártnál hosszabb időt vesz igénybe, hello művelet megjelölt hello figyelmeztetés jelentést *hello partíciójára szolgáltatás, amely az elsődleges replika szolgál hello művelet*. Ha a hello művelet sikeresen befejeződött, hello figyelmeztetés törlődik. Ha hello művelet egy hibával befejeződött, a hello állapotjelentése tartalmazza hello hiba részleteit.

* **SourceId**: System.NamingService
* **Tulajdonság**: előtaggal kezdődik **Duration_** és azonosítja a hello lassú művelet és hello Service Fabric nevét, mely hello művelet történik. Például ha a szolgáltatás létrehozása a háló nevét: / MyApp/MyService túl sokáig tart, a hello tulajdonság Duration_AOCreateService.fabric:/MyApp/MyService. AO pontok toohello szerepköre hello elnevezési partíciót ezt a nevet és művelet.
* **További lépések**: Miért hello elnevezési művelet sikertelen ellenőrzés. Minden műveletet lehet különböző alapvető okait. Például törlése egy csomópont elakadt előfordulhat, hogy a szolgáltatás, mert hello alkalmazásgazda tartja összeomló hello szolgáltatást kód tooa felhasználói hibája miatt a csomóponton.

a következő példa hello szolgáltatás létrehozási művelet jeleníti meg. hello művelet konfigurált hello időtartamnál hosszabb ideig tartott. AO újrapróbálkozik, és elküldi a munkahelyi tooNO. NINCS kész hello utolsó művelet – időtúllépés miatt. Ebben az esetben hello azonos replikája elsődleges hello AO mind egyetlen szerepkör.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : hello AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : hello NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication rendszerállapot-jelentések
**System.Hosting** hello hatóság az üzembe helyezett entitásokra.

### <a name="activation"></a>az aktiválás
System.Hosting jelzi az OK gombra, ha egy alkalmazás aktiválása megtörtént hello csomóponton. Ellenkező esetben azt egy hibát jelez.

* **SourceId**: System.Hosting
* **Tulajdonság**: aktiválási, beleértve hello bevezetés verzióját
* **További lépések**: Ha hello alkalmazás állapota nem megfelelő, vizsgálja meg, miért hello aktiválás sikertelen volt.

a következő példa azt mutatja be a sikeres aktiválási hello:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ExcludeHealthStatistics

ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_1
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131445249083836329
                                     SentAt                : 7/14/2017 4:55:08 PM
                                     ReceivedAt            : 7/14/2017 4:55:14 PM
                                     TTL                   : Infinite
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Letöltés
**System.Hosting** egy hibát jelez, ha hello alkalmazás csomag letöltése sikertelen.

* **SourceId**: System.Hosting
* **Tulajdonság**:  **letöltése:*RolloutVersion***
* **További lépések**: vizsgálja meg, miért nem sikerült letölteni a hello hello csomóponton.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage rendszerállapot-jelentések
**System.Hosting** hello hatóság az üzembe helyezett entitásokra.

### <a name="service-package-activation"></a>Szolgáltatás az alkalmazáscsomag aktiválása
System.Hosting jelentések OK gombra, ha hello szolgáltatást az alkalmazáscsomag aktiválása hello csomóponton sikeres. Ellenkező esetben azt egy hibát jelez.

* **SourceId**: System.Hosting
* **Tulajdonság**: aktiválás
* **További lépések**: vizsgálja meg, miért hello aktiválás sikertelen volt.

### <a name="code-package-activation"></a>Kód az alkalmazáscsomag aktiválása
**System.Hosting** Ha hello aktiválás sikeres kód csomagonként OK jelenti. Ha hello aktiválás sikertelen, jelenti a rendszer figyelmeztetést konfigurált módon. Ha **CodePackage** tooactivate meghibásodik, vagy nagyobb, mint hello konfigurálva hibával leáll **CodePackageHealthErrorThreshold**, üzemeltető egy hibát jelez. A service-csomag kód több csomagot tartalmaz, az aktiválási jelentés minden egyes jön létre.

* **SourceId**: System.Hosting
* **Tulajdonság**: használ hello előtag **CodePackageActivation** hello kódcsomag és hello belépési pontot hello nevét tartalmazza, és  **CodePackageActivation:* CodePackageName*:*SetupEntryPoint/EntryPoint*** (például **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Szolgáltatási típus regisztrációs
**System.Hosting** OK jelent, ha hello szolgáltatás típusának regisztrálása sikeresen befejeződött. Az egy hibát jelez, ha hello regisztrációját nem végezték el, időben (használatával konfigurált **ServiceTypeRegistrationTimeout**). Ha hello futásidejű le van zárva, hello szolgáltatás típusának hello csomópont regisztrációját, és üzemeltetési hiányára.

* **SourceId**: System.Hosting
* **Tulajdonság**: használ hello előtag **ServiceTypeRegistration** és hello szolgáltatás környezettípus nevét tartalmazza (például **ServiceTypeRegistration:FileStoreServiceType**)

a következő példa hello jeleníti meg a megfelelő telepített szervizcsomag:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_1
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131445249084026346
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : hello ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131445249084306362
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : hello CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131445249088096842
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Letöltés
**System.Hosting** egy hibát jelez, ha hello szolgáltatás csomag letöltése sikertelen.

* **SourceId**: System.Hosting
* **Tulajdonság**:  **letöltése:*RolloutVersion***
* **További lépések**: vizsgálja meg, miért nem sikerült letölteni a hello hello csomóponton.

### <a name="upgrade-validation"></a>Frissítésének ellenőrzése
**System.Hosting** egy hibát jelez, ha hello frissítés során az érvényesítés meghiúsul, vagy ha hello sikertelen frissítés esetén hello csomóponton.

* **SourceId**: System.Hosting
* **Tulajdonság**: használ hello előtag **FabricUpgradeValidation** és hello frissítési verziója
* **Leírás**: toohello hiba történt az mutat

## <a name="next-steps"></a>Következő lépések
[A Service Fabric rendszerállapot-jelentések megtekintése](service-fabric-view-entities-aggregated-health.md)

[Hogyan tooreport és ellenőrzés szolgáltatás állapotát](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Figyelése és diagnosztizálása helyileg szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[A Service Fabric-alkalmazás frissítése](service-fabric-application-upgrade.md)


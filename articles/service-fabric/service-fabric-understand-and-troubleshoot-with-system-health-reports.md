---
title: "Hibaelhárítás a rendszerállapot-jelentések |} Microsoft Docs"
description: "A rendszerállapot-jelentések küldött hibaelhárítási fürt vagy az alkalmazással kapcsolatos problémák Azure Service Fabric-összetevők és használatát ismerteti."
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
ms.openlocfilehash: 54e20146b2f1e0ca6153b66319be70c6f7c2fb59
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-system-health-reports-to-troubleshoot"></a>Rendszerállapot-jelentések használata a hibaelhárítás során
Az Azure Service Fabric összetevői jelentést nem a fürt összes entitás. A [a health Store adatbázisban](service-fabric-health-introduction.md#health-store) hoz létre, és törli a rendszer-jelentéseken alapuló entitásokat. Azt is rendszerezi azokat a hierarchiában, amely rögzíti az entitás interakciókat.

> [!NOTE]
> Állapotfigyelő kapcsolatos fogalmak megismerése, olvassa el: [Service Fabric állapotmodell](service-fabric-health-introduction.md).
> 
> 

Rendszerállapot-jelentések adja meg a fürt és az alkalmazás funkciói és keresztül állapotát jelző problémák láthatósága. Az alkalmazások és szolgáltatások rendszerállapot-jelentések győződjön meg arról, hogy az entitások vannak megvalósítva, és a Service Fabric szempontjából megfelelően működik. A jelentések nem biztosít semmilyen állapotfigyelés az üzleti logika, a szolgáltatás vagy a lefagyott folyamatok észlelése. Felhasználó szolgáltatások állapotfigyelő üzemállapot-adatait a logika vonatkozó információkkal.

> [!NOTE]
> Watchdogs állapotjelentések láthatók csak *után* rendszerösszetevőn entitás létrehozása. A törölt egy entitás a health Store adatbázisban automatikusan törli a vele társított összes rendszerállapot-jelentések. Ugyanez vonatkozik az entitás új példányának létrehozásakor (például egy új állapot-nyilvántartó megőrzött replika szolgáltatáspéldány jön létre). A régi példányhoz társított összes jelentések törlése, és a-tárolóból törölni.
> 
> 

A rendszer az összetevő jelent azonosítják a forrás, amely kezdődik-e a "**System.**" előtag. Watchdogs nem használhatja ugyanazt az előtagot az eseményforrásokat, érvénytelen paramétereket tartalmazó jelentések utasítja el.
Vizsgáljuk meg bizonyos rendszer jelentéseket megtudhatja, hogy mi elindítja azokat és a lehetséges problémákat jelölnek javítása.

> [!NOTE]
> A Service Fabric továbbra is fennáll, a feltételek iránt, amelyek javítják a láthatósága, mi történik a fürt- és a jelentések hozzáadásához. Meglévő jelentések is fejleszthető a további részleteket a probléma gyorsabb megoldása érdekében.
> 
> 

## <a name="cluster-system-health-reports"></a>Fürt rendszerállapot-jelentések
A fürt állapotának entitás a health Store adatbázisban automatikusan jön létre. Ha minden megfelelően működik, a rendszer a jelentés nem tartalmaz.

### <a name="neighborhood-loss"></a>Hálózatok adatvesztés
**System.Federation** egy hibát jelez, amikor azt észleli, hogy a helyek adatvesztés. A jelentés az egyes csomópontokon, és a csomópont-azonosító szerepel a tulajdonságnév. Ha egy alkalmazás a teljes Service Fabric gyűrű elvesznek, általában számíthat két esemény (mindkét oldalán, a résnek jelentés). További környékeken elvesznek, ha nincsenek további események.

A jelentés az élettartamnak adja meg a globális bérleti idejét. A jelentés minden fele a TTL időtartama Újraküldte mindaddig, amíg a feltétel aktív marad. Az esemény lejáratot követően automatikusan törlődnek. Távolítsa el, ha lejárt a viselkedés garantálja az, hogy a jelentés törlődnek a a health Store adatbázisból megfelelően, még akkor is, ha a jelentéskészítési csomópont nem működik.

* **SourceId**: System.Federation
* **Tulajdonság**: kezdődik **hálózatok** és csomópont adatai
* **További lépések**: vizsgálja meg, miért a helyek nem vesztek el (például ellenőrizze a fürt csomópontjai közötti kommunikáció).

## <a name="node-system-health-reports"></a>Csomópont rendszerállapot-jelentések
**System.FM**, a szolgáltatót, amely a fürtcsomópontokon kapcsolatos információt, amely jelenti, hogy a Feladatátvevőfürt-kezelő szolgáltatás. Minden csomópont állapotát megjelenítő System.FM egy jelentést kell rendelkeznie. A csomópont entitások törlődnek, amikor a rendszer eltávolítja a csomópont állapota (lásd: [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).

### <a name="node-updown"></a>A csomópont fel/le
Amikor a csomópont csatlakozik a (is működik, és) gyűrű a OK System.FM jelentés. Az egy hibát jelez, amikor a csomópont a gyűrűben indulását (szolgáltatás leállt, vagy frissítés, vagy egyszerűen mert sikertelen volt). A rendszerállapot-hierarchia által a health Store adatbázisban a korrelációs System.FM csomópont jelentések a telepített entitások hajt végre műveletet. Úgy ítéli meg, a csomópont minden telepített entitás virtuális szülője. Ezen a csomóponton telepített entitások közzétéve az lekérdezésekben, ha a csomópont állapotúként jelentette-e System.FM entitások társított példányt az azonos példánnyal rendelkező által. Amikor System.FM jelzi, hogy a csomópont nem működik, vagy újra (egy új példány), a health Store adatbázisban automatikusan megtisztítja a telepített entitás, amely csak a lefelé csomópontján vagy a csomópont előző példány létezhet.

* **SourceId**: System.FM
* **Tulajdonság**: állapota
* **További lépések**: a csomópont frissítés szempontjából nem működik, ha kellene tartoznia hozzá biztonsági után frissítették. Ebben az esetben állapotát kell váltson vissza az OK gombra. A csomópont nem térjen vissza, vagy nem sikerül, ha a probléma további vizsgálat van szüksége.

A következő példa bemutatja a csomópont OK állapotot System.FM esemény:

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
**System.FabricNode** jelentések figyelmeztetés, ha a csomópont által használt tanúsítványok lejáró. Három tanúsítvány csomópontonként: **Certificate_cluster**, **Certificate_server**, és **Certificate_default_client**. Legalább két héttel a lejárati esetén a jelentés állapota rendben. A lejárati két héten belül van, a jelentés típusa esetén figyelmeztetés. Ezek az események TTL végtelen, és amikor a csomópont elhagyja a fürt eltávolítja.

* **SourceId**: System.FabricNode
* **Tulajdonság**: kezdődik **tanúsítvány** és a tanúsítvány típusát további információt tartalmaz
* **További lépések**: frissítse a tanúsítványokat, ha azok lejáró.

### <a name="load-capacity-violation"></a>Betöltési kapacitás megsértése
A Service Fabric terheléselosztó hiányára amikor azt észleli, hogy a csomópont-kapacitás megsértése.

* **SourceId**: System.PLB
* **Tulajdonság**: kezdődik **kapacitás**
* **További lépések**: biztosított metrikákat és olvassa el az aktuális kapacitása a csomóponton.

## <a name="application-system-health-reports"></a>Alkalmazás rendszerállapot-jelentések
**System.CM**, amely jelenti, hogy a kezelő szolgáltatás a szolgáltató, amely felügyeli az alkalmazással kapcsolatos információkat.

### <a name="state"></a>Állapot
System.CM jelzi az OK gombra az alkalmazás létrehozásakor vagy frissítésekor. Az tájékoztatja a health Store adatbázisban az alkalmazás törlésekor, hogy az áruházból el kell távolítani.

* **SourceId**: System.CM
* **Tulajdonság**: állapota
* **További lépések**: Ha az alkalmazás létrehozott vagy frissített, a kezelő állapotjelentése tartalmaznia kell. Ellenkező esetben ellenőrizze a lekérdezés kiállításával az alkalmazás állapota (például a PowerShell-parancsmag **Get-ServiceFabricApplication - ApplicationName *applicationName***).

Az alábbi példában látható az esemény a **fabric: / WordCount** alkalmazás:

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
**System.FM**, amely jelenti, hogy a Feladatátvevőfürt-kezelő szolgáltatás, amely kezeli a szolgáltatással kapcsolatban további információt a szolgáltató.

### <a name="state"></a>Állapot
System.FM jelzi az OK gombra a szolgáltatás létrehozásakor. Ez törli az entitás a health Store adatbázisban a szolgáltatás törlésekor.

* **SourceId**: System.FM
* **Tulajdonság**: állapota

A következő példa bemutatja az esemény a szolgáltatás **fabric: / WordCount/wordcountwebservice verziója**:

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
**System.PLB** egy hibát jelez, amikor azt észleli, hogy egy szolgáltatás, amellyel egyeztetés szükséges egy másik szolgáltatás frissítése létrehoz egy affinitási láncot. A jelentés sikeres frissítés történik, ha nincs bejelölve.

* **SourceId**: System.PLB
* **Tulajdonság**: ServiceDescription objektumot
* **További lépések**: Ellenőrizze a kapcsolódó szolgáltatások ismertetése.

## <a name="partition-system-health-reports"></a>Partíció rendszerállapot-jelentések
**System.FM**, a szolgáltatót, amely a szolgáltatáspartíciók kapcsolatos információt, amely jelenti, hogy a Feladatátvevőfürt-kezelő szolgáltatás.

### <a name="state"></a>Állapot
System.FM jelzi az OK gombra, ha a partíció létrejött, és megfelelő állapotban. Ez törli az entitás a health Store adatbázisban a partíció törlése.

A partíció nem éri el a replikakészlet minimális száma, ha hiba jelenti. Ha a partíció nincs kevesebb a minimális replika, de kevesebb a cél replika, a rendszer figyelmeztetést jelenti. Ha a partíció kvórumveszteségben van, System.FM jelent hibát.

Egyéb fontos események tartalmazhat egy figyelmeztetés, ha az újrakonfigurálás hosszabb időt vesz igénybe a vártnál, és amikor a vártnál hosszabb ideig tart a build. A várt a build és újrakonfigurálását konfigurálható: a szolgáltatás forgatókönyvek alapján alkalommal. Például egy terabájt állapot, például az SQL-adatbázis, a szolgáltatás-e a build tovább tart, mint egy szolgáltatás állapotát kis mennyiségű számára.

* **SourceId**: System.FM
* **Tulajdonság**: állapota
* **További lépések**: Ha a rendszerállapot nem OK, úgy is, hogy egyes másodpéldányok nem létrehozott, megnyitott, vagy a tartományvezérlővé való előléptetés elsődleges vagy másodlagos megfelelően. Sok esetben okozza-e a Megnyitás vagy szerepkör módosítása végrehajtása szolgáltatási hiba.

A következő példa bemutatja a megfelelő partíció:

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

A következő példa bemutatja, javasoljuk, hogy kevesebb cél replika állapotát. A következő lépés az, hogy beolvasása a partíció leírása, amely bemutatja, hogyan van konfigurálva: **MinReplicaSetSize** három és **TargetReplicaSetSize** hét. Majd kérje le a fürt a csomópontok számát: öt. Ebben az esetben két replika nem kell elhelyezni, mert a cél replikák száma nagyobb, mint az elérhető csomópontok számát.

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
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
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
**System.PLB** jelentések figyelmeztetés, ha egy replika korlátozássértést észlelt, és nem helyezhető el az összes partíció replika. A jelentés-részletek megjelenítése milyen korlátozások és tulajdonságok megakadályozza a replika elhelyezésének.

* **SourceId**: System.PLB
* **Tulajdonság**: kezdődik **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>A replika rendszerállapot-jelentések
**System.RA**, amely jelenti, hogy a reconfiguration agent összetevő a replika állapota az szolgáltatót.

### <a name="state"></a>Állapot
**System.RA** jelentések OK, a replika létrehozása után.

* **SourceId**: System.RA
* **Tulajdonság**: állapota

A következő példa bemutatja a megfelelő replika:

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
A jelentés leírása a kezdési idő (egyezményes világidő) az API-hívás metódus meghívásakor.

**System.RA** jelenti a figyelmeztetést, ha a replika megnyitása tovább tart, mint a beállított időtartam (alapértelmezés: 30 perc). Ha az API-t a szolgáltatás rendelkezésre állása befolyásolja, a jelentés kiadott sokkal gyorsabb (egy konfigurálható időszakban, az alapértelmezett érték 30 másodperc). Mért idő magában foglalja a replikátor nyitó és a szolgáltatás megnyitása igénybe vett idő. Ha a megnyitási elvégzi a OK vált a tulajdonság.

* **SourceId**: System.RA
* **Tulajdonság**: **ReplicaOpenStatus**
* **További lépések**: Ha a rendszerállapot nem OK, vizsgálja meg, miért nyissa meg a replikát a vártnál hosszabb időbe telik.

### <a name="slow-service-api-call"></a>Lassú service API-hívás
**System.RAP** és **System.Replicator** jelentést egy figyelmeztetés, ha a szolgáltatás felhasználói kód hívása tovább tart, mint a beállított időn. A figyelmeztetés nincs bejelölve, a hívás befejezését.

* **SourceId**: System.RAP vagy System.Replicator
* **Tulajdonság**: a lassú API neve. A leírás nyújt további információt az idő, az API-t a függőben lévő lett.
* **További lépések**: vizsgálja meg, miért vártnál hosszabb ideig tart a hívást.

Az alábbi példában egy partíciót a kvórum elvesztése, és a vizsgálat lépéseket annak mérje fel, hogy miért történik. A replikák egyike figyelmeztetési állapot, hogy az állapotát. Azt mutatja, hogy a szolgáltatási művelet hosszabb időt vesz igénybe, mint a System.RAP által jelentett eseményt várt. Ezek az információk fogadását követően a következő lépéssel nézze meg a szolgáltatáskód hibáit, és vizsgálja meg a hiba. Ebben az esetben a **RunAsync** az állapotalapú szolgáltatásból végrehajtása nem kezelt kivételt jelez. A replikák vannak újrahasznosítása, ezért nem láthatók a figyelmeztetési állapot a replikákat. Ismételje meg a kezdeti állapotát, és keresse meg a másodpéldány-azonosító az eltérések Bizonyos esetekben a próbálkozások tudhatja meg keresik.

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

A hibás alkalmazást a hibakereső indításakor a diagnosztikai események windows megjelenítése RunAsync a kivétel:

![A Visual Studio 2015 diagnosztikai események: RunAsync hiba a fabric: / HelloWorldStatefulApplication.][1]

A Visual Studio 2015 diagnosztikai események: RunAsync hiba történt a **fabric: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Replikációs sor megtelt
**System.Replicator** jelentések figyelmeztetés, ha a replikációs sor megtelt. Az elsődleges replikációs várólista általában megtelik, mert egy vagy több másodlagos replikák lassúak múlva nyugtázza a műveletek. A másodlagos Ez általában történik, ha a szolgáltatás lassú műveletek alkalmazásához. A figyelmeztetés nincs bejelölve, ha a várólista már nem teljes.

* **SourceId**: System.Replicator
* **Tulajdonság**: **PrimaryReplicationQueueStatus** vagy **SecondaryReplicationQueueStatus**, attól függően, a replika szerepkör

### <a name="slow-naming-operations"></a>Lassú elnevezési műveletek
**System.NamingService** jelentések állapotát az elsődleges replikán, amikor egy elnevezési művelet elfogadható hosszabb időbe telik. Példák elnevezési műveletek [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) vagy [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync). Több metódust alatt található FabricClient, például a [módszereket szolgáltatás](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) vagy [tulajdonság módszereket](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).

> [!NOTE]
> A Naming service szolgáltatásnevek megszünteti a fürt helyre, és lehetővé teszi a felhasználók kezeléséhez a szolgáltatás nevét és tulajdonságait. A Service Fabric particionálva szolgáltatás megőrzött. A Authority Owner, amely tartalmazza az összes Service Fabric-neveket és szolgáltatásokra vonatkozó metaadatok egyik partíciója jelöli. A Service Fabric-nevek különböző partíciók nevű Name Owner partíciók, így a szolgáltatás nem bővíthető van leképezve. Tudjon meg többet az [Naming service](service-fabric-architecture.md).
> 
> 

Ha egy elnevezési művelet a vártnál hosszabb időt vesz igénybe, a művelet megjelölt figyelmeztetés jelentést a a *a Naming service partíció a művelet ellátó elsődleges replika*. Ha a művelet sikeresen befejeződött, a figyelmeztetés nincs bejelölve. Ha a művelet egy hibával befejeződött, a jelentés tartalmazza a hiba részleteit.

* **SourceId**: System.NamingService
* **Tulajdonság**: előtaggal kezdődik **Duration_** és azonosítja a lassú művelet és a Service Fabric nevét, amelyen a művelet történik. Például ha a szolgáltatás létrehozása a háló nevét: / MyApp/MyService túl sokáig tart, Duration_AOCreateService.fabric:/MyApp/MyService tulajdonság. AO szerepe a Naming ezt a nevet és a művelet a mutat.
* **További lépések**: Miért nem sikerül a Naming művelet ellenőrzése. Minden műveletet lehet különböző alapvető okait. Például törlése egy csomópont elakadt előfordulhat, hogy a szolgáltatás, mert alkalmazásgazda tartja összeomló szolgáltatáskód felhasználói hibája miatt a csomóponton.

A következő példa bemutatja a szolgáltatás létrehozási művelet. A művelet a megadott időtartamnál hosszabb ideig tartott. AO újrapróbálkozik, és elküldi a munkahelyi nem. NEM az utolsó művelet időtúllépéssel fejeződött be. Ebben az esetben a azonos replika nem elsődleges, mind a AO, és nincs szerepkör.

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
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
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
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication rendszerállapot-jelentések
**System.Hosting** a szervezet az üzembe helyezett entitásokra.

### <a name="activation"></a>az aktiválás
System.Hosting jelzi az OK gombra, ha egy alkalmazás aktiválása megtörtént a csomóponton. Ellenkező esetben azt egy hibát jelez.

* **SourceId**: System.Hosting
* **Tulajdonság**: aktiválási, beleértve a bevezetés verzióját
* **További lépések**: Ha az alkalmazás állapota nem megfelelő, vizsgálja meg, miért nem sikerült az aktiválást.

A következő példa bemutatja a sikeres aktiválási:

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
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Letöltés
**System.Hosting** egy hibát jelez, ha az alkalmazás csomag letöltése sikertelen.

* **SourceId**: System.Hosting
* **Tulajdonság**:  **letöltése:*RolloutVersion***
* **További lépések**: vizsgálja meg, miért nem sikerült a letöltés a csomóponton.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage rendszerállapot-jelentések
**System.Hosting** a szervezet az üzembe helyezett entitásokra.

### <a name="service-package-activation"></a>Szolgáltatás az alkalmazáscsomag aktiválása
System.Hosting jelentések OK gombra, ha a csomóponton a szolgáltatás az alkalmazáscsomag aktiválása sikeres. Ellenkező esetben azt egy hibát jelez.

* **SourceId**: System.Hosting
* **Tulajdonság**: aktiválás
* **További lépések**: vizsgálja meg, miért nem sikerült az aktiválást.

### <a name="code-package-activation"></a>Kód az alkalmazáscsomag aktiválása
**System.Hosting** Ha az aktiválás sikeres kód csomagonként OK jelenti. Ha az aktiválás sikertelen, akkor hiányára konfigurált módon. Ha **CodePackage** nem tudja aktiválni vagy nagyobb, mint a beállított hibával leáll **CodePackageHealthErrorThreshold**, üzemeltető egy hibát jelez. A service-csomag kód több csomagot tartalmaz, az aktiválási jelentés minden egyes jön létre.

* **SourceId**: System.Hosting
* **Tulajdonság**: az előtag- **CodePackageActivation** és nevét, valamint a kódcsomag a belépési pontot tartalmaz  **CodePackageActivation:*CodePackageName* :*SetupEntryPoint/EntryPoint*** (például **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Szolgáltatási típus regisztrációs
**System.Hosting** OK jelent, ha a szolgáltatás típusának regisztrálása sikeresen befejeződött. Azt egy hibát jelez, ha a regisztráció nem volt meg időben (használatával konfigurált **ServiceTypeRegistrationTimeout**). A futtatókörnyezet le van zárva, ha a szolgáltatás típusa a csomópont regisztrációját, és üzemeltetési hiányára.

* **SourceId**: System.Hosting
* **Tulajdonság**: az előtag- **ServiceTypeRegistration** és a szolgáltatás környezettípus nevét tartalmazza (például **ServiceTypeRegistration:FileStoreServiceType**)

A következő példa bemutatja a megfelelő telepített szervizcsomag:

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
                             Description           : The ServicePackage was activated successfully.
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
                             Description           : The CodePackage was activated successfully.
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
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Letöltés
**System.Hosting** egy hibát jelez, ha a service-csomag letöltése sikertelen.

* **SourceId**: System.Hosting
* **Tulajdonság**:  **letöltése:*RolloutVersion***
* **További lépések**: vizsgálja meg, miért nem sikerült a letöltés a csomóponton.

### <a name="upgrade-validation"></a>Frissítésének ellenőrzése
**System.Hosting** egy hibát jelez, ha a frissítés során az érvényesítés meghiúsul, vagy ha a frissítés sikertelen lesz a csomóponton.

* **SourceId**: System.Hosting
* **Tulajdonság**: az előtag- **FabricUpgradeValidation** , és tartalmazza a frissített verzió
* **Leírás**: mutat, a következő hiba

## <a name="next-steps"></a>Következő lépések
[A Service Fabric rendszerállapot-jelentések megtekintése](service-fabric-view-entities-aggregated-health.md)

[Jelentés és a szolgáltatás állapotának ellenőrzése](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Figyelése és diagnosztizálása helyileg szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[A Service Fabric-alkalmazás frissítése](service-fabric-application-upgrade.md)


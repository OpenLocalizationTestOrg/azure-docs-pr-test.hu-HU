---
title: "aaaHow tooview Azure Service Fabric entitásainak összesített állapotát |} Microsoft Docs"
description: "Leírja, hogyan tooquery, megtekintheti, és Azure Service Fabric entitások összesített állapotát, állapotlekérdezések és általános lekérdezések kiértékelése."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: fa34c52d-3a74-4b90-b045-ad67afa43fe5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: add810551cac26d2b4ff81b57d94ddd780c2cc2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-service-fabric-health-reports"></a>A Service Fabric rendszerállapot-jelentések megtekintése
Az Azure Service Fabric vezet be a [állapotmodell](service-fabric-health-introduction.md) figyelés az állapotfigyelő entitások mely rendszerösszetevők és watchdogs a hajthatja végre helyi feltételek, amelyek jelentés. Hello [a health Store adatbázisban](service-fabric-health-introduction.md#health-store) összes rendszerállapot-adatok toodetermine összefoglalja, hogy kifogástalan-e entitások.

hello fürt automatikusan töltődik hello rendszerösszetevők által küldött állapotjelentések. További információ: [használata rendszerállapot-jelentéseket tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

A Service Fabric hello entitások több módon tooget hello összesített állapotát tartalmazza:

* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) vagy más képi megjelenítés eszközök
* Állapotlekérdezések száma (a PowerShell, API vagy REST)
* Általános lekérdezi az állapotfigyelő egyik hello tulajdonságaként (a PowerShell, API vagy REST) rendelkező entitásokat, amely visszatérési listája

Ezeket a beállításokat, most toodemonstrate egy helyi fürt használatára öt csomópontot és hello [fabric: / WordCount alkalmazás](http://aka.ms/servicefabric-wordcountapp). Hello **fabric: / WordCount** két alapértelmezett szolgáltatások, a következő típusú állapot-nyilvántartó szolgáltatást tartalmazó alkalmazás `WordCountServiceType`, és egy állapotmentes szolgáltatások típusú `WordCountWebServiceType`. Hello módosítottam `ApplicationManifest.xml` toorequire hét cél replikák hello állapotalapú szolgáltatás és egy partíciót. Mivel hello fürt csak az öt csomóponttal, hello rendszerösszetevők jelentés figyelmeztetés hello szolgáltatás partíció mert kevesebb hello cél.

```xml
<Service Name="WordCountService">
<<<<<<< HEAD
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="3">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
=======
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
>>>>>>> 5e84dbdd8e45a5d6b36f435a550b7433b873bf11
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>A Service Fabric Explorerben állapota
Hello fürt vizuális áttekintést nyújt a Service Fabric Explorerben talál. Az alábbi hello kép láthatja, hogy:

* alkalmazás hello **fabric: / WordCount** piros (a hiba), mert egy hibaesemény által jelentett **MyWatchdog** hello tulajdonság **rendelkezésre állási**.
* A szolgáltatások egyik **fabric: / WordCount/WordCountService** sárga (figyelmeztetés) a. hét replikák hello szolgáltatás van konfigurálva, és hello fürt az öt csomóponttal rendelkezik, így a két repicas nem helyezhető el. Bár ez nem látható itt, hello szolgáltatás partíciója sárga miatt a rendszer jelentése `System.FM` arról, hogy `Partition is below target replica or instance count`. hello sárga partíció eseményindítók hello sárga szolgáltatás.
* hello fürtre piros piros hello alkalmazás miatt.

hello értékelésének alapértelmezett házirendeket hello fürtjegyzékben és az alkalmazás jegyzékében használ. Szigorú házirendek, és nem legyenek tűrni minden hiba.

A Service Fabric Explorerrel hello fürt megtekintése:

![A Service Fabric Explorerrel hello fürt ábrázolása.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> Tudjon meg többet az [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

## <a name="health-queries"></a>Állapotlekérdezések száma
A Service Fabric állapotlekérdezések mutatja az egyes támogatott hello [entitástípusok](service-fabric-health-introduction.md#health-entities-and-hierarchy). API-t, a módszerekkel hello keresztül hozzáférhetők [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell-parancsmagok és a többi. Ezeket a lekérdezéseket információval teljes állapotát figyelhesse hello entitás: hello összesíti az állapotokat, entitás állapotával kapcsolatos események, gyermek állapotokat (ha alkalmazható), a nem megfelelő értékelések (ha hello entitás állapota nem kifogástalan) és gyermekek állapotstatisztika (Ha érvényes).

> [!NOTE]
> Teljes mértékben a telepítéskor hello a health Store adatbázisban egy egészségügyi entitás érték érkezett vissza. hello entitás aktív (nem törölte a) és egy jelentést. A szülő entitások hello hierarchia lánc rendszer jelentések is rendelkeznie kell. Ha ezek a feltételek nem teljesülnek, a hello állapotfigyelő visszatérési lekérdezi egy [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) rendelkező [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` , amely bemutatja, hogy miért hello entitás nem adott vissza.
>
>

hello állapotlekérdezések kell eltelnie a hello entitás azonosító, amely hello entitástípus függ. hello lekérdezések választható állapotfigyelő házirend paraméterek fogadja el. Ha nincsenek házirendek meg van adva, hello [állapotházirendeket](service-fabric-health-introduction.md#health-policies) hello fürt vagy az alkalmazás jegyzékfájlból használt értékelésre. Ha hello jegyzékfájlokat nem tartalmazzák a következő definícióját: állapotházirendeket, hello alapértelmezett állapotházirendeket értékelésre használja. hello alapértelmezett állapotházirendeket nem működését az esetleges hibákat. hello lekérdezéseket is fogadja el a szűrők csak részleges gyermekek visszaküldésére használatos, vagy események – hello néhányat a meglévők közül, figyelembe vegyék hello a megadott szűrők. Egy másik szűrő lehetővé teszi, hogy hello gyermekek statisztika kivételével.

> [!NOTE]
> hello kimeneti alkalmazza a rendszer hello kiszolgáló oldalán, így csökken a hello üzenet válasz mérete. Hello kimeneti szűrők toolimit hello adatokat adott vissza, nem pedig szűrőket alkalmazhat hello ügyféloldalon használata javasolt.
>
>

Egy entitás állapotát tartalmazza:

* hello hello entitás állapotát összesíti. Hello a health Store adatbázisban entitás állapotjelentések száma, a gyermek állapotokat (ha alkalmazható) és az állapotházirendeket alapján számítja ki. Tudjon meg többet az [entitás állapotának kiértékelését](service-fabric-health-introduction.md#health-evaluation).  
* hello állapotával kapcsolatos események hello entitás.
* Minden gyermeknek hello entitások, amelyeken gyermekek állapotának hello gyűjteménye. hello állapotok entitás azonosítók tartalmazhat, és hello összesített állapotát. tooget teljes állapotát figyelhesse a gyermek hello lekérdezés állapotfigyelő hívás hello gyermek entitástípus számára pedig hello gyermek azonosítója.
* hello adott pont toohello jelenti, hogy a nem megfelelő értékelések hello entitás hello állapota elindul, ha hello entitás nem működik megfelelően. hello értékelések rekurzív hello gyermekek állapotfigyelő értékelések aktuális állapot kiváltó tartalmazó. Például egy figyelő elleni replika hibát jelzett. hello alkalmazás állapotának miatt tooan sérült szolgáltatás; a nem megfelelő próbaverzióként jeleníti meg. hello szolgáltatás állapota nem megfelelő miatt tooa partíció hibás; hello partíció nem kifogástalan miatt tooa replika hibás; hello a replika állapota nem megfelelő jelentés toohello figyelő hiba miatt.
* hello állapotstatisztika gyermekkel hello entitások minden gyermek típusú. Például a fürt állapota látható hello teljes száma, az alkalmazások, szolgáltatások, partíciók, a replikákat, és entitások hello fürt telepíteni. Szolgáltatás állapotát jeleníti meg a partíciók száma hello és replikák hello alatt megadott szolgáltatás.

## <a name="get-cluster-health"></a>Fürt állapotának beolvasása
Értéket ad vissza hello hello fürt entitás állapotát, és tartalmazza az alkalmazások és a csomópontok (gyermekek hello fürt) hello állapotokat. Bemenet:

* [Választható] hello fürt állapotházirend használt tooevaluate hello csomópontokat és hello fürthöz kapcsolódó események.
* [Választható] hello alkalmazás állapotának házirend-hozzárendelés, az hello állapotházirendeket toooverride hello alkalmazás-jegyzékfájl házirendek használható.
* [Választható] Események, a csomópontok és az alkalmazásokat, amelyek adja meg, mely tételek szűrők érdeklő, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák). Összes esemény, csomópontokat és alkalmazások úgy legyenek használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.
* [Választható] Szűrés tooexclude állapotstatisztika.
* [Választható] Szűrés tooinclude fabric: / rendszer állapotfigyelő statisztikákat hello állapotstatisztika. Csak érvényes hello állapotstatisztika nem dimenziónevek kizárásakor. Alapértelmezés szerint a hello egészségügyi statisztikák csak felhasználói alkalmazások és hello rendszeralkalmazás statisztika tartalmazza.

### <a name="api"></a>API
tooget fürt állapotát, és hozzon létre egy `FabricClient` és hívás hello [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) metódust a **HealthManager**.

hello következő hívás lekérdezi hello fürt állapotát:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

hello alábbira hello fürt állapotfigyelő lekérdezi az egyéni fürterőforrás állapotát házirend használatával és szűri a csomópontok és alkalmazások. Meghatározza, hogy a hello állapotstatisztika hello háló tartalmaz: / System statisztika. Létrehozza [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), hello bemeneti adatokat tartalmazó.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var healthStatisticsFilter = new ClusterHealthStatisticsFilter()
{
    ExcludeHealthStatistics = false,
    IncludeSystemApplicationHealthStatistics = true
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
    HealthStatisticsFilter = healthStatisticsFilter
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
hello parancsmag tooget hello fürt állapota [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth). Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.

hello hello fürt állapota az öt csomóponttal, hello rendszeralkalmazás és fabric: / WordCount leírtak szerint konfigurálva.

a következő parancsmag hello lekérdezi a fürt állapotának alapértelmezett házirendek használatával. hello összesített állapota figyelmet igényel, mert hello fabric: / WordCount alkalmazás figyelmeztetés is van. Vegye figyelembe, hogyan hello sérült értékelések hello feltételek hello összesített állapotát kiváltó részletekkel szolgálnak.

```xml
PS D:\ServiceFabric> Get-ServiceFabricClusterHealth


AggregatedHealthState   : Warning
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                          
                          
NodeHealthStates        : 
                          NodeName              : _Node_4
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_0
                          AggregatedHealthState : Ok
                          
ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok
                          
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning
                          
HealthEvents            : None
HealthStatistics        : 
                          Node                  : 5 Ok, 0 Warning, 0 Error
                          Replica               : 6 Ok, 0 Warning, 0 Error
                          Partition             : 1 Ok, 1 Warning, 0 Error
                          Service               : 1 Ok, 1 Warning, 0 Error
                          DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                          DeployedApplication   : 5 Ok, 0 Warning, 0 Error
                          Application           : 0 Ok, 1 Warning, 0 Error
```

hello következő PowerShell-parancsmag lekéri hello fürt hello állapotát egy egyéni alkalmazás-házirend használatával. Eredmények tooget alkalmazások és a hiba vagy figyelmeztetés csomópontjára szűri. Ennek eredményeképpen a csomópontok nem ad vissza, mivel ezek az összes kifogástalan. Csak hello fabric: / WordCount alkalmazás tiszteletben tartja hello alkalmazások szűrő. Mivel a hello egyéni házirend hello a háló hibaként tooconsider figyelmeztetések határozza meg: / WordCount alkalmazás hello alkalmazás hasonlóan hiba történik, és így hello fürt.

```powershell
PS D:\ServiceFabric> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics


AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                          
                          
NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error
                          
HealthEvents            : None
```

### <a name="rest"></a>REST
Kaphat a fürt health használata az egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.

## <a name="get-node-health"></a>Csomópont állapotának beolvasása
Értéket ad vissza hello egy csomópont entitás állapotát, és hello állapotával kapcsolatos események hello csomóponton jelentett tartalmazza. Bemenet:

* [Szükséges] hello csomópont nevét, amely azonosítja a hello csomópont.
* [Választható] hello fürt állapotfigyelő házirend-beállítások használt tooevaluate állapotát.
* [Választható] Adja meg, mely tételek események szűrők iránt, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák). Összes esemény használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.

### <a name="api"></a>API
tooget csomópont állapotát keresztül hello API, hozzon létre egy `FabricClient` és hívás hello [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) a HealthManager metódust.

hello következő kód jogosultságot kap hello megadott csomópontnév hello csomópont állapotát:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

hello következő kód jogosultságot kap hello csomópont állapotát az hello adott csomópont neve és fázisok eseményszűrő és egyéni házirend használatával [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
hello parancsmag tooget hello csomópont állapota [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth). Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.
a következő parancsmag hello alapértelmezett házirendek használatával lekérdezi a hello csomópont állapotát:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 7/13/2017 4:39:23 PM
                        ReceivedAt            : 7/13/2017 4:40:47 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 4:40:47 PM, LastWarning = 1/1/0001 12:00:00 AM
```

hello következő parancsmag lekéri az összes csomópont állapotát hello hello fürt:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_4                     Ok
_Node_3                     Ok
_Node_2                     Ok
_Node_1                     Ok
_Node_0                     Ok
```

### <a name="rest"></a>REST
Kaphat a csomópont állapotát egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.

## <a name="get-application-health"></a>Alkalmazás állapotának beolvasása
Az alkalmazás entitás értéket ad vissza a hello állapotát. Hello állapotának hello telepített alkalmazás és szolgáltatás gyermekeket tartalmaz. Bemenet:

* [Szükséges] hello alkalmazásnév (URI), amely azonosítja a hello alkalmazás.
* [Választható] hello az alkalmazás állapotházirendje toooverride hello application manifest házirendeket használt.
* [Választható] Események, szolgáltatások, és adja meg, mely tételek üzembe helyezett alkalmazások szűrők érdeklő, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák). Minden események, a szolgáltatások és a központilag telepített alkalmazások is használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.
* [Választható] Szűrés tooexclude hello állapotstatisztika. Ha nincs megadva, hello állapotstatisztika közé tartozik a hello ok, figyelmeztetés és hibák száma az összes alkalmazás számára: szolgáltatások, partíciók, replikák alkalmazások telepítését, és központilag telepített szervizcsomagok.

### <a name="api"></a>API
tooget alkalmazás állapotának, hozzon létre egy `FabricClient` és hívás hello [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) a HealthManager metódust.

hello következő kód jogosultságot kap hello alkalmazás állapotának hello megadott neve (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

hello következő kód jogosultságot kap hello alkalmazás állapotának hello megadott neve (URI), szűrők és a megadott egyéni házirendek [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
hello parancsmag tooget hello alkalmazás üzemállapota [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps). Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.

hello következő parancsmag visszaadja hello hello állapotának **fabric: / WordCount** alkalmazás:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok
                                  
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning
                                  
DeployedApplicationHealthStates : 
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok
                                  
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/13/2017 5:57:05 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
                                  
HealthStatistics                : 
                                  Replica               : 6 Ok, 0 Warning, 0 Error
                                  Partition             : 1 Ok, 1 Warning, 0 Error
                                  Service               : 1 Ok, 1 Warning, 0 Error
                                  DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                                  DeployedApplication   : 5 Ok, 0 Warning, 0 Error
```

a következő PowerShell-parancsmag cserél az egyéni házirendek hello. Szűrők is gyermekek és események.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error
                                  
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>REST
Beszerezheti az alkalmazás állapotának egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.

## <a name="get-service-health"></a>Szolgáltatás állapotának beolvasása
Egy szolgáltatás entitás hello állapotát értéket ad vissza. Hello partíció állapotokat tartalmaz. Bemenet:

* [Szükséges] hello szolgáltatás neve (URI), amely azonosítja a hello szolgáltatást.
* [Választható] hello az alkalmazás állapotházirendje használt toooverride hello alkalmazás-jegyzékfájl házirend.
* [Választható] Az események és adja meg, mely tételek partícióknak szűrők érdeklő, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák). Események és a partíciók olyan használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.
* [Választható] Szűrés tooexclude állapotstatisztika. Ha nincs megadva, egészségügyi statisztika megjelenítése hello hello ok, figyelmeztetés és hiba száma az összes partíció és replikák hello szolgáltatást.

### <a name="api"></a>API
tooget szolgáltatásának állapota keresztül hello API, hozzon létre egy `FabricClient` és hívás hello [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) a HealthManager metódust.

hello alábbi példa lekérdezi hello Állapotfigyelő szolgáltatás a megadott szolgáltatásnév (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

hello következő kód jogosultságot kap hello szolgáltatás állapotát a hello megadott szolgáltatásnév (URI), szűrők és egyéb egyéni házirend használatával [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
hello parancsmag tooget hello szolgáltatás állapota [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth). Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.

a következő parancsmag hello alapértelmezett házirendek használatával lekérdezi a hello szolgáltatás állapota:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                        
                        Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                        
                            Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
PartitionHealthStates : 
                        PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                        AggregatedHealthState : Warning
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 15
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
                        Partition             : 0 Ok, 1 Warning, 0 Error
```

### <a name="rest"></a>REST
Kaphat a szolgáltatás állapotát a egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.

## <a name="get-partition-health"></a>Partíció állapotának beolvasása
Egy partíció entitás hello állapotát értéket ad vissza. Hello replika állapotokat tartalmaz. Bemenet:

* [Szükséges] hello partíció Azonosítót (GUID), amely azonosítja a hello partíció.
* [Választható] hello az alkalmazás állapotházirendje használt toooverride hello alkalmazás-jegyzékfájl házirend.
* [Választható] Az események és adja meg, mely tételek replikák szűrők érdeklő, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák). Események és a replikák használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.
* [Választható] Szűrés tooexclude állapotstatisztika. Ha nincs megadva, hello állapotstatisztika megjelenítése a rendszer hány replikák ok, figyelmeztetés és hiba állapotok.

### <a name="api"></a>API
tooget partíció állapotfigyelő keresztül hello API, hozzon létre egy `FabricClient` és hívás hello [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) a HealthManager metódust. Választható paraméterek: toospecify, hozzon létre [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
hello parancsmag tooget hello partíció állapota [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth). Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.

hello következő parancsmag lekéri hello állapotát minden partíciója hello **fabric: / WordCount/WordCountService** szolgáltatás és a szűrők replika állapotokat ki:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 72
                        SentAt                : 7/13/2017 5:57:29 PM
                        ReceivedAt            : 7/13/2017 5:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/P RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/S RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 7/13/2017 5:57:48 PM, LastError = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131444445174851664
                        SentAt                : 7/13/2017 6:35:17 PM
                        ReceivedAt            : 7/13/2017 6:35:18 PM
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
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/13/2017 5:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
Kaphat a partíció health használata az egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.

## <a name="get-replica-health"></a>A replika állapotának beolvasása
Állapotalapú szolgáltatási replika vagy állapotmentes szolgáltatáspéldány hello állapotának beolvasása. Bemenet:

* [Szükséges] hello Azonosítót (GUID) és a replika Partícióazonosító hello replika azonosító.
* [Választható] hello alkalmazás állapotának házirend paramétereket toooverride hello alkalmazás-jegyzékfájl házirendek használni.
* [Választható] Adja meg, mely tételek események szűrők iránt, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák). Összes esemény használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.

### <a name="api"></a>API
tooget hello replika állapotfigyelő keresztül hello API, hozzon létre egy `FabricClient` és hívás hello [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) a HealthManager metódust. speciális paramétereket használja toospecify [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
hello parancsmag tooget hello replika állapota [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth). Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.

hello következő parancsmag lekéri hello elsődleges replika hello szolgáltatás összes partíciók hello állapotát:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Kaphat a replika health használata az egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.

## <a name="get-deployed-application-health"></a>Telepített alkalmazás állapotának beolvasása
Egy csomópont entitás központi telepítésű alkalmazás hello állapotát adja vissza. Telepített hello szolgáltatás csomag állapotokat tartalmaz. Bemenet:

* [Szükséges] hello alkalmazásnév (URI) és a csomópont neve (karakterlánc) azonosító hello telepített alkalmazás.
* [Választható] hello az alkalmazás állapotházirendje toooverride hello application manifest házirendeket használt.
* [Választható] Az események és adja meg, mely tételek telepített szervizcsomagok szűrők érdeklő, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák). Események és a telepített szervizcsomagok használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.
* [Választható] Szűrés tooexclude állapotstatisztika. Ha nincs megadva, hello állapotstatisztika telepített szervizcsomagok hello száma ok, figyelmeztetés és hiba állapotának megjelenítése.

### <a name="api"></a>API
egy csomóponton keresztül hello API-t központi telepítésű alkalmazás állapotának tooget hello hozzon létre egy `FabricClient` és hívás hello [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) a HealthManager metódust. Választható paraméterek: toospecify, használjon [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
hello parancsmag tooget hello telepített alkalmazás állapotának van [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps). Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag. futtassa, ha egy alkalmazás központi telepítése, toofind [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) és hello pillantást telepített alkalmazás gyermekei.

hello következő parancsmag lekéri hello hello állapotának **fabric: / WordCount** a központilag telepített alkalmazás **_Node_2**.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_0


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_0
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
                                     ServiceManifestName   : WordCountWebServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131444422261848308
                                     SentAt                : 7/13/2017 5:57:06 PM
                                     ReceivedAt            : 7/13/2017 5:57:17 PM
                                     TTL                   : Infinite
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/13/2017 5:57:17 PM, LastWarning = 1/1/0001 12:00:00 AM
                                     
HealthStatistics                   : 
                                     DeployedServicePackage : 2 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
Kaphat a telepített alkalmazás health használata az egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.

## <a name="get-deployed-service-package-health"></a>Telepített szolgáltatások csomag állapotának beolvasása
Értéket ad vissza egy telepített szolgáltatást csomag entitás állapotának hello. Bemenet:

* [Szükséges] hello alkalmazásnév (URI), a csomópont neve (karakterlánc) és szolgáltatás jegyzékfájljának nevét (karakterlánc) azonosító hello telepített service-csomag.
* [Választható] hello az alkalmazás állapotházirendje használt toooverride hello alkalmazás-jegyzékfájl házirend.
* [Választható] Adja meg, mely tételek események szűrők iránt, és vissza kell adni a hello eredmény (például csak, a hibák vagy figyelmeztetések és hibák). Összes esemény használt tooevaluate hello összesítve entitásállapot, függetlenül attól, hello szűrő.

### <a name="api"></a>API
tooget hello állapotát egy telepített szervizcsomag keresztül hello API, hozzon létre egy `FabricClient` és hívás hello [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) a HealthManager metódust. Választható paraméterek: toospecify, használjon [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
hello parancsmag tooget telepített hello szolgáltatás csomag állapota van [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth). Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag. Ha egy alkalmazás központi telepítése, toosee futtatása [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) , és tekintse meg a hello központilag telepített alkalmazások. service-csomagok jelenleg egy alkalmazásban keresse meg hello toosee telepített szolgáltatás csomag gyermekek hello [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) kimeneti.

hello következő parancsmag lekéri hello hello állapotának **WordCountServicePkg** hello a service-csomag **fabric: / WordCount** a központilag telepített alkalmazás **_Node_2**. hello entitásnak van **System.Hosting** sikeres service-csomag és a belépési pont aktiválása és sikeres szolgáltatástípus regisztráció a jelentésekre.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_2
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131444422267693359
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131444422267903345
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131444422272458374
                             SentAt                : 7/13/2017 5:57:07 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Kaphat a telepített szolgáltatások csomag health használata az egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) , amely tartalmazza a hello törzsében leírt állapotházirendeket.

## <a name="health-chunk-queries"></a>Adatrészlet állapotlekérdezések száma
hello adatrészlet állapotlekérdezések több szintű fürt gyermekek (rekurzív), egy bemeneti szűrőket adhat vissza. Támogatja a speciális szűrők, amelyek lehetővé teszik a magas fokú rugalmasságot biztosít a hello gyermekek kiválasztása toobe adott vissza. hello szűrőket adhat meg a gyermekek hello egyedi azonosítója vagy más csoportazonosítókhoz és/vagy állapotokat. Alapértelmezés szerint nincsenek gyermekei tartoznak, mint első szintű gyermekek mindig tartalmazó megakadályozását toohealth parancsokat.

Hello [állapotlekérdezések](service-fabric-view-entities-aggregated-health.md#health-queries) hello gyermekeinek visszatérési csak az első szintű megadott szükséges szűrők egy entitást. hello gyermekek tooget hello gyermekei, meg kell hívnia további rendszerállapot API-k minden egyes entitásnál iránt. Hasonlóképpen konkrét személyek tooget hello állapotát, meg kell hívnia egy rendszerállapot API minden kívánt entitás. hello adatrészlet lekérdezés egyéb szűrési lehetővé teszi toorequest több elem egyik fontos egy lekérdezést, minimalizálja a hello üzenet mérete és hello üzenetek száma.

hello hello adatrészlet lekérdezés értéke, hogy kaphat állapota további fürt entitásokat (vélhetően fürt entitásokhoz kezdve a kötelező gyökérszintű) egy hívásban. Összetett állapot lekérdezés is express, mint:

* Visszatérési csak alkalmazások hiba, és ezeket az alkalmazásokat tartalmaznak minden figyelmeztetés vagy hiba. A kiadott szolgáltatások tartalmazza az összes partíció.
* A név által meghatározott négy alkalmazások csak hello állapotának visszaadása.
* A kívánt alkalmazáshoz típusú alkalmazások csak hello állapotának visszaadása.
* Térjen vissza az összes üzembe helyezett entitások csomóponton. Összes alkalmazás, hello megadott csomóponton összes üzembe helyezett alkalmazás és minden telepített hello szolgáltatás csomagok ezen a csomóponton adja vissza.
* Hiba található összes replika visszaadása. Alkalmazások, szolgáltatások, partíciók és csak a replikák hiba adja vissza.
* Térjen vissza az összes alkalmazást. Az egy megadott szolgáltatás tartalmazza az összes partíció.

Hello állapotfigyelő adatrészlet lekérdezés jelenleg ki van téve, csak a hello fürt entitás. Azt jelzi, hogy a fürt állapotfigyelő adattömb, amely tartalmazza:

* hello fürt állapotát összesíti.
* hello egészségügyi állapot adatrészlet csomópontlista, figyelembe vegyék bemeneti szűrők.
* hello egészségügyi állapot adatrészlet alkalmazások listáját tiszteletben tartják bemeneti szűrők. Egyes alkalmazás állapotának állapot adattömbök adatrészlet listáját összes szolgáltatások tiszteletben bemeneti szűrők és adatrészlet hello szűrők tiszteletben összes üzembe helyezett alkalmazások listáját tartalmazza. A szolgáltatások és telepített alkalmazások hello gyermekek esetében azonos. Ezzel a módszerrel hello fürt összes entitásának potenciálisan visszaadható kérésre, hierarchikus.

### <a name="cluster-health-chunk-query"></a>Fürt állapotfigyelő adatrészlet lekérdezés
Beolvasása hello hello fürt entitás állapotát, és hello hierarchikus egészségügyi állapot adattömbök szükséges gyermekek tartalmazza. Bemenet:

* [Választható] hello fürt állapotházirend használt tooevaluate hello csomópontokat és hello fürthöz kapcsolódó események.
* [Választható] hello alkalmazás állapotának házirend-hozzárendelés, az hello állapotházirendeket toooverride hello alkalmazás-jegyzékfájl házirendek használható.
* [Választható] Csomópont-és alkalmazásokat, amelyek adja meg, mely tételek érdeklő, és vissza kell adni a hello eredmény. hello szűrők adott tooan entitás/csoport az entitások vagy azon a szinten alkalmazható tooall entitásokat. szűrők hello listáját egy általános szűrő és/vagy az adott azonosítók toofine-felbontása entitások hello lekérdezés által visszaadott szűrők tartalmazhat. Ha üres, hello gyermeke nem lehet megjeleníteni alapértelmezés szerint.
  További információk: hello szűrők [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) és [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter). hello alkalmazás szűrők is rekurzív módon speciális szűrők megadása a gyermekek számára.

hello adatrészlet eredmény hello szűrők tiszteletben hello alárendelt tartalmazza.

Jelenleg hello adatrészlet lekérdezés nem ad vissza a nem megfelelő értékelések vagy entitás események. További adatokat hello meglévő fürt állapotának lekérdezés nyerhetők.

### <a name="api"></a>API
tooget fürt állapotfigyelő darabos, hozzon létre egy `FabricClient` és hívás hello [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) metódust a **HealthManager**. Adhasson [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) toodescribe házirendek és a speciális szűrők.

hello következő kód jogosultságot kap fürt állapotfigyelő adatrészlet speciális szűrők.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except hello ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
hello parancsmag tooget hello fürt állapota [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk). Csatlakoztasson toohello fürt hello segítségével [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag.

hello következő kód jogosultságot kap csomópontok csak akkor, ha egy konkrét csomóponton, amely mindig vissza kell adni az kivételével hiba vannak.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in hello cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters


HealthState                  : Warning
NodeHealthStateChunks        : 
                               TotalCount            : 1
                               
                               NodeName              : _Node_1
                               HealthState           : Ok
                               
ApplicationHealthStateChunks : None
```

a következő parancsmag hello lekérdezi a fürt adattömbök alkalmazás szűrők.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 1
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks : 
                                TotalCount            : 1
                               
                                ServiceName           : fabric:/WordCount/WordCountService
                                HealthState           : Error
                                PartitionHealthStateChunks : 
                                    TotalCount            : 1
                               
                                    PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                                    HealthState           : Error
                                    ReplicaHealthStateChunks : 
                                        TotalCount            : 5
                               
                                        ReplicaOrInstanceId   : 131444422293118720
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293118721
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113678
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113679
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422260002646
                                        HealthState           : Error
```

hello következő parancsmag entitásokat ad vissza összes üzembe helyezett egy csomóponton.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 2
                               
                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : FAS
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
                               
                               
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : WordCountServicePkg
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
```

### <a name="rest"></a>REST
Kaphat a fürt állapotának rendszer egy [GET kérés](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) vagy egy [POST kérelem](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) házirendek és a speciális szűrők hello törzsében leírt foglal magában.

## <a name="general-queries"></a>Általános lekérdezések
Általános lekérdezésekben Service Fabric entitások, megadott típusú listáját adja vissza. Hello API keresztül ki vannak téve (keresztül hello metódusai **FabricClient.QueryManager**), PowerShell-parancsmagok és a többi. Ezeket a lekérdezéseket a segédlekérdezések több összetevőkből összesíteni. Hello őket egyik [a health Store adatbázisban](service-fabric-health-introduction.md#health-store), hello tölti fel, amelyek összesített állapota minden egyes lekérdezés eredménye.  

> [!NOTE]
> Általános lekérdezések hello entitás állapotát összesíti hello vissza, és nem tartalmaz gazdag egészségügyi adatokat. Entitás állapota nem kifogástalan, ha nyomon lehet követni az egészségügyi lekérdezések tooget a egészségügyi adatok, beleértve az események, a gyermek állapotokat és a nem megfelelő értékelések.
>
>

Általános lekérdezések vissza egy entitás ismeretlen állapot, akkor lehetséges, hogy hello a health Store adatbázisban nem rendelkezik teljes hello entitás adatait. Akkor is, hogy a segédlekérdezés toohello a health Store adatbázisban nem volt sikeres (például egy kommunikációs hiba vagy hello a health Store adatbázisban halmozódni volt). Hello entitás állapotának lekérdezés nyomon követéséhez. Hello allekérdezés hibát jelzett a átmeneti, például a hálózati problémák, a követési lekérdezés esetleg sikeresek lehetnek. Azt is kaphat további részleteket a hello health Store adatbázisból kapcsolatos miért hello entitás nem lesz közzétéve.

lekérdezések hello **HealthState** a szervezetek a következők:

* Csomópontlista: hello lista csomópontok (lapozható) hello fürt adja vissza.
  * Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)
  * PowerShell: Get-ServiceFabricNode
* Alkalmazáslista: hello fürt (lapozható) alkalmazások hello listáját adja vissza.
  * Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)
  * PowerShell: Get-ServiceFabricApplication
* Lista: egy alkalmazás (lapozható) szolgáltatások hello listája értéket ad vissza.
  * Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)
  * PowerShell: Get-ServiceFabricService
* Partition listában: (lapozható) szolgáltatás a partíciók hello listáját adja vissza.
  * Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)
  * PowerShell: Get-ServiceFabricPartition
* Listáján: egy partíció (lapozható) replikák hello listáját adja vissza.
  * Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)
  * PowerShell: Get-ServiceFabricReplica
* Telepített alkalmazások listája: csomóponton központilag telepített alkalmazások hello listáját adja vissza.
  * Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication
* Telepített szolgáltatás csomaglista: beolvasása hello listát a szolgáltatás egy telepített alkalmazás.
  * Alkalmazásprogramozási felületek: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication

> [!NOTE]
> Lapozható eredmények hello lekérdezések némelyike visszaadása. hello ezeket a lekérdezéseket a return származtatva listáját [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1). Hello eredmények nem férnek el egy üzenetet, ha csak egy lap ad vissza, és egy ContinuationToken, amely nyomon követi, ahol számbavételi leállt. Továbbra is toocall hello azonos lekérdezés számára pedig hello folytatási kód hello előző lekérdezés tooget következő eredménybe.
>
>

### <a name="examples"></a>Példák
hello következő kód jogosultságot kap a nem megfelelő alkalmazások hello hello fürt:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

hello következő parancsmag lekéri hello alkalmazás részletei hello fabric: / WordCount alkalmazás. Láthatja, hogy állapot figyelmeztetés.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

hello következő parancsmag lekéri hiba állapotot hello szolgáltatások:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Error"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Error
```

## <a name="cluster-and-application-upgrades"></a>Fürt- és frissítések
Hello fürt és az alkalmazás egy figyelt frissítés során a Service Fabric ellenőrzi, hogy minden továbbra is kifogástalan állapotát tooensure. Ha egy entitás nem kifogástalan, mint a konfigurált házirendek kiértékelése, hello frissítés alkalmazva frissítés rendszerre jellemző házirendek toodetermine hello tovább művelet. hello frissítés szüneteltetett tooallow felhasználói beavatkozást (például a hiba kijavítása, vagy házirendek módosítása), vagy az lehet, hogy automatikusan állítsa vissza toohello helyes verziót.

Során egy *fürt* frissítési, hogy megkaphassa hello fürt frissítési állapot. hello frissítésének állapota nem kifogástalan értékelések, mely pont toowhat állapota nem megfelelő hello fürt tartalmaz. Ha hello frissítés toohealth problémák miatt vissza lesz állítva, hello frissítési állapot emlékszik hello utolsó sérült okok miatt. Az információk segítségével a rendszergazdák kivizsgálni, mi a probléma után hello frissítés visszaállításra, vagy leállt.

Ehhez hasonlóan során egy *alkalmazás* frissítés, bármely sérült értékelések található hello alkalmazás frissítési állapot.

hello következő állapotát jeleníti meg hello alkalmazás frissítési a módosított fabric: / WordCount alkalmazás. A figyelő hibát jelzett az egyik a replikáit. hello frissítése folyamatban van, mert hello egészségügyi ellenőrzések nem veszik.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2017 5:23:26 PM
FailureTimestampUtc           : 4/21/2017 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

További információk a hello [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-tootroubleshoot"></a>Állapotfigyelő értékelések tootroubleshoot használata
Ha probléma van a hello fürt vagy egy alkalmazás, hello fürt vagy az alkalmazás állapotának toopinpoint nézze meg mi. a nem megfelelő értékelések hello milyen kiváltott hello aktuális állapota nem kifogástalan részleteit tartalmazzák. Ha szeretné, részletezhető le nem megfelelő állapotú gyermekfigyelőket entitások tooidentify hello alapvető okát.

Vegye figyelembe például az alkalmazás nem megfelelő, mert a replikáit egyik egy esetleges hibajelentésben való megjelenítéshez. hello következő Powershell-parancsmag jeleníti meg a nem megfelelő értékelések hello:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount -EventsFilter None -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.
                                  
                                        Unhealthy replica: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', ReplicaOrInstanceId='131444422260002646', AggregatedHealthState='Error'.
                                  
                                            Error event: SourceId='MyWatchdog', Property='Memory'.
                                  
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

Vessen egy pillantást hello replika tooget további információt:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricReplicaHealth -ReplicaOrInstanceId 131444422260002646 -PartitionId af2e3e44-a8f8-45ac-9f31-4093eb897600


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Error
UnhealthyEvaluations  : 
                        Error event: SourceId='MyWatchdog', Property='Memory'.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
                        SourceId              : MyWatchdog
                        Property              : Memory
                        HealthState           : Error
                        SequenceNumber        : 131444451657749403
                        SentAt                : 7/13/2017 6:46:05 PM
                        ReceivedAt            : 7/13/2017 6:46:05 PM
                        TTL                   : Infinite
                        Description           : 
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 7/13/2017 6:46:05 PM, LastOk = 1/1/0001 12:00:00 AM
```

> [!NOTE]
> hello sérült értékelések megjelenítése hello első OK hello entitás kiértékelése toocurrent állapotát. Előfordulhat, hogy több más kiváltó események ebben az állapotban, de nem kell megjelenő hello értékelések. tooget hello állapotfigyelő entitások toofigure ki minden hello sérült jelentések hello fürt a részletezés olvashat.
>
>

## <a name="next-steps"></a>Következő lépések
[Használja a rendszer állapotát jelentések tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Adja hozzá az egyéni Service Fabric állapotjelentések száma](service-fabric-report-health.md)

[Hogyan tooreport és ellenőrzés szolgáltatás állapotát](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Figyelése és diagnosztizálása helyileg szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[A Service Fabric-alkalmazás frissítése](service-fabric-application-upgrade.md)

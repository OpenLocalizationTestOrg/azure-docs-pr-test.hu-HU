---
title: "az Azure Service Fabric aaaReport és ellenőrzés állapotának |} Microsoft Docs"
description: "Ismerje meg, hogyan toosend rendszerállapot-jelentéseket a szolgáltatás kódból és hogyan toocheck hello állapotát a szolgáltatás adott Azure Service Fabric hello állapotfigyelési eszközök használatával biztosít."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: mfussell
editor: 
ms.assetid: 7c712c22-d333-44bc-b837-d0b3603d9da8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: dekapur
ms.openlocfilehash: bcb838fefe3f2054447e1731d709e455560260e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="report-and-check-service-health"></a>Szolgáltatásállapot jelentése és ellenőrzése
A szolgáltatások problémája, amikor a képes toorespond tooand javítás incidensek és kimaradások függ a képes toodetect hello problémák gyorsan. Ha a szolgáltatás kódból jelentéssel a problémák és hibák toohello Azure Service Fabric állapotkezelő szabványos állapotfigyelési, hogy a Service Fabric toocheck hello állapot biztosít eszközök is használhatja.

Három módon, hogy jelenthetik-e a hello szolgáltatás állapotát:

* Használjon [partíció](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) vagy [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objektumok.  
  Használhatja a hello `Partition` és `CodePackageActivationContext` tooreport hello állapotát elemeit tartalmazza az aktuális környezetben hello objektumokat. Például egy replikát részeként futó kódok is jelentés állapotfigyelő csak, hogy a replika tartozik hello partíció és hello alkalmazás, amely egy része.
* Használjon `FabricClient`.   
  Használhat `FabricClient` tooreport állapotfigyelő hello szolgáltatás kódból Ha hello fürt nem [biztonságos](service-fabric-cluster-security.md) , vagy ha hello szolgáltatás rendszergazdai jogosultságokkal futtatja. A legtöbb olyan valós forgatókönyv a nem biztonságos fürtök használata, vagy adjon meg rendszergazdai jogosultságokkal. A `FabricClient`, minden entitás, amely hello fürt része a rendszerállapot jelentést. Ideális esetben azonban szolgáltatás kódot csak küldjön kapcsolódó tooits egészségügyi jelentéseket.
* Használjon hello REST API-k hello fürt, alkalmazások, központilag telepített alkalmazás, szolgáltatás, szolgáltatáscsomagot, partíció, replika, vagy a csomópont szintek. Ez lehet a használt tooreport állapotát a tárolóban.

Ez a cikk végigvezeti egy példa, amely a jelentések hello szolgáltatás kódot az állapotát. hello példa azt is bemutatja, hogyan a Service Fabric által biztosított hello eszközöket lehet használt toocheck hello állapotát. Ez a cikk tervezett toobe van egy gyors bevezetés toohello állapotfigyelési Service Fabric képességeit. További részletes információt részletes ismertető cikk gyűjtemény állapotát a cikk végén hello hello hivatkozás kezdődő hello adatsorozat érheti el.

## <a name="prerequisites"></a>Előfeltételek
Hello következőkkel kell rendelkeznie:

* Visual Studio 2015-öt vagy a Visual Studio 2017
* A Service Fabric SDK

## <a name="toocreate-a-local-secure-dev-cluster"></a>egy helyi biztonságos fejlesztési fürtöt toocreate
* Nyissa meg a Powershellt rendszergazdai jogosultságokkal, és futtassa a következő parancsok hello:

![A parancsokat, hogy hogyan toocreate egy biztonságos fejlesztési fürtöt](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="toodeploy-an-application-and-check-its-health"></a>egy alkalmazás toodeploy és állapotának ellenőrzése
1. Nyissa meg a Visual Studio rendszergazdaként.
2. A projekt létrehozása hello segítségével **állapotalapú alkalmazások és szolgáltatások szolgáltatás** sablont.
   
    ![Állapotalapú szolgáltatással a Service Fabric-alkalmazás létrehozása](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
3. Nyomja le az **F5** toorun hello alkalmazás hibakeresési módban. hello alkalmazás telepített toohello helyi fürtön.
4. Miután hello alkalmazás fut, kattintson a jobb gombbal a hello Local Cluster Manager hello értesítési területen megjelenő ikonra, és válassza ki **helyi fürt kezelése** a hello helyi menü tooopen Service Fabric Explorerben talál.
   
    ![Nyissa meg a Service Fabric Explorer az értesítési területről](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
5. hello alkalmazás állapotának üzenetnek kell megjelennie, mint a kép. Ilyenkor hello alkalmazás kifogástalan, és hiba nélkül kell lennie.
   
    ![A Service Fabric Explorerben megfelelő alkalmazás](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
6. Hello állapotát a PowerShell használatával is ellenőrizheti. Használhat ```Get-ServiceFabricApplicationHealth``` használhatja az alkalmazás állapotát, és toocheck ```Get-ServiceFabricServiceHealth``` toocheck egy szolgáltatás állapotát. hello állapotjelentése az hello ugyanahhoz az alkalmazáshoz a PowerShellben megtalálható-e a lemezkép.
   
    ![A PowerShell megfelelő alkalmazás](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="tooadd-custom-health-events-tooyour-service-code"></a>tooadd egyéni állapotfigyelő események tooyour szolgáltatáskódot
hello Service Fabric a Visual Studio projektsablonjai példakódot tartalmaz. hello következő lépések bemutatják, hogyan lehet jelentést egyéni állapotával kapcsolatos események a szolgáltatás kódból. Ezek a jelentések jelennek meg automatikusan, hogy a Service Fabric biztosítanak, például a Service Fabric Explorer, az Azure portál állapotának megtekintése és PowerShell állapotfigyelési hello szabványos eszközei.

1. Nyissa meg újra a Visual Studio korábban létrehozott alkalmazás hello, vagy hozzon létre egy új alkalmazást hello segítségével **állapotalapú alkalmazások és szolgáltatások szolgáltatás** Visual Studio-sablont.
2. Nyissa meg hello Stateful1.cs fájlt, és megkeresi hello `myDictionary.TryGetValueAsync` hello-hívás `RunAsync` metódust. Láthatja, hogy ez a metódus visszaadja a `result` , hogy tartás hello hello számláló aktuális értéke, mert ezt az alkalmazást a hello kulcs logikai tookeep száma futó. Ha ez egy valós alkalmazás, és az eredmény hello hiánya képviselt Hiba, célszerű az esemény tooflag.
3. tooreport eredmény hello hiánya jelenti. a hiba, ha olyan állapotesemény hello lépések hozzáadása.
   
    a. Adja hozzá a hello `System.Fabric.Health` névtér toohello Stateful1.cs fájlt.
   
    ```csharp
    using System.Fabric.Health;
    ```
   
    b. Adja hozzá a következő kód után hello hello `myDictionary.TryGetValueAsync` hívása
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    A replika állapota azt jelenti, mivel azt egy állapotalapú szolgáltatásból jelentve. Hello `HealthInformation` paraméter ügynökökről hello állapottal kapcsolatos probléma kapcsolatos információkat tárolja.
   
    Ha állapotmentes szolgáltatások hozna létre, használja a következő kód hello
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
4. Ha a szolgáltatás rendszergazda jogosultságokkal fut, vagy ha hello fürt nincs [biztonságos](service-fabric-cluster-security.md), is `FabricClient` tooreport állapotát, ahogy az alábbi lépésekkel hello.  
   
    a. Hozzon létre hello `FabricClient` után hello példány `var myDictionary` nyilatkozatot.
   
    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```
   
    b. Adja hozzá a következő kód után hello hello `myDictionary.TryGetValueAsync` hívható meg.
   
    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.Context.PartitionId,
            this.Context.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```
5. Most szimulálása a hiba, és látni hello állapota Hálózatfigyelő eszközök megjelennek. toosimulate hello hiba, hello első sort a korábban hozzáadott kódot reporting hello állapotfigyelő megjegyzésbe. Miután az első sor hello megjegyzéssé, hello kód a következő példa hello fog megjelenni.
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   Ezzel a kóddal akkor következik be, hello állapotjelentése minden alkalommal, amikor `RunAsync` hajt végre. Hello módosítása után nyomja le az **F5** toorun hello alkalmazás.
6. Miután hello alkalmazás fut, nyissa meg a Service Fabric Explorer toocheck hello hello alkalmazás állapotáról. Megadott idő Service Fabric Explorer mutatja, hogy hello alkalmazás nem kifogástalan. Ez a jelentett hello kódból, hogy azt korábban hozzáadott hello hiba miatt.
   
    ![A Service Fabric Explorerben a nem megfelelő alkalmazás](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
7. Ha hello faszerkezetes nézetben a Service Fabric Explorer hello elsődleges másodpéldány, láthatja, hogy **állapota** jelzi a hiba, túl. Service Fabric Explorer is megjeleníti, amelyek részleteit toohello hello állapotjelentése `HealthInformation` hello kódban paraméter. Megtekintheti a PowerShellben ugyanazt állapotjelentések hello és hello Azure-portálon.
   
    ![A Service Fabric Explorerben a replika állapota](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Ez a jelentés marad, a hello állapotkezelő azt egy másik jelentés, vagy amíg a replika nem cseréli le. Mivel jelenleg nincs beállítva `TimeToLive` a rendszerállapot-jelentés hello a `HealthInformation` objektum hello jelentés soha nem jár le.

Azt javasoljuk, hogy egészségügyi hello legtöbb részletes szinten, amely a jelen esetben ez hello replika kell megadni. A health is jelentheti `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

tooreport-állapot `Application`, `DeployedApplication`, és `DeployedServicePackage`, használjon `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Következő lépések
* [Részletes bemutatója a Service Fabric állapota](service-fabric-health-introduction.md)
* [REST API-t a jelentéskészítési szolgáltatás állapota](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [REST API-alkalmazás állapotának jelentéskészítéshez](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)


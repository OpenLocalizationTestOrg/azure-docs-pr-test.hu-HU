---
title: "Azure Service Fabric Alkalmazáskezelés aaaAutomate |} Microsoft Docs"
description: "Központi telepítése, frissítése, tesztelése és PowerShell használatával távolítsa el a Service Fabric-alkalmazások."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: f03f4294-571d-4262-8a77-cc8b481b959d
ms.service: service-fabric
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/16/2017
ms.author: ryanwi
redirect_url: /azure/service-fabric/service-fabric-deploy-remove-applications
ms.openlocfilehash: a64a39dbee26be8ac15fee767a90fd06bfe4b896
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-hello-application-lifecycle-using-powershell"></a>Automatizálása a PowerShell használatával hello alkalmazás életciklusa
Hello sok aspektusainak [Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md) automatizálható.  Ez a cikk bemutatja, hogyan toouse PowerShell tooautomate gyakori feladatokat telepítése, frissítése, eltávolítása és tesztelés az Azure Service Fabric-alkalmazások.  Felügyelt, és az Alkalmazáskezelés HTTP API-k is elérhetők. Lásd: [alkalmazás-életciklus](service-fabric-application-lifecycle.md) további információt.  

## <a name="prerequisites"></a>Előfeltételek
Mielőtt áthelyez toohello feladatok hello cikkben, ügyeljen arra, hogy:

* Ismerkedjen meg ismertetett hello Service Fabric fogalmakat [technikai áttekintése a Service Fabric](service-fabric-technical-overview.md).
* [Hello futtatókörnyezet, az SDK és az eszközök telepítése](service-fabric-get-started.md), amely telepíti hello **ServiceFabric** PowerShell-modult.
* [PowerShell-parancsfájl végrehajtásának engedélyezése](service-fabric-get-started.md#enable-powershell-script-execution).
* A helyi fürt elindításához.  Indítsa el a rendszergazdaként egy új PowerShell-ablakot, és futtassa a fürtbeállítási parancsfájlt hello hello SDK mappából:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
* Ebben a cikkben a PowerShell-parancsok futtatása, előtt első alkalommal csatlakoztatja toohello helyi Service Fabric-fürt használatával [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps):`Connect-ServiceFabricCluster localhost:19000`
* a következő feladatok hello a V1-es alkalmazás csomag toodeploy és a v2 alkalmazáscsomagok van szükségük a frissítést. Töltse le a hello [ **WordCount** mintaalkalmazás](http://aka.ms/servicefabricsamples) (első lépések minták hello található). Hozza létre, és a Visual Studio hello alkalmazás becsomagolása (kattintson a jobb gombbal a **WordCount** megoldáskezelő, és válasszon **csomag**). Hello a V1-es csomag másolása `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` túl`C:\Temp\WordCount`. Másolás `C:\Temp\WordCount` túl`C:\Temp\WordCountV2`, hello v2 alkalmazáscsomag frissítés létrehozása. Nyissa meg `C:\Temp\WordCountV2\ApplicationManifest.xml` egy szövegszerkesztőben. A hello **applicationmanifest jegyzékben** elem, a módosítás hello **ApplicationTypeVersion** túl "1.0.0" attribútumot "2.0.0" tooupdate hello alkalmazás verziószáma. Mentse a módosított hello ApplicationManifest.xml fájlt.

## <a name="task-deploy-a-service-fabric-application"></a>Feladat: A Service Fabric-alkalmazás központi telepítése
Azt követően már beépített és csomagolt alkalmazás hello (vagy letöltött hello alkalmazáscsomag), telepíthet egy helyi Service Fabric-fürt hello alkalmazást. Központi telepítés magában foglalja a hello alkalmazáscsomag feltöltését, hello alkalmazástípus regisztrálása és hello alkalmazás példányának létrehozásakor. Ez a szakasz toodeploy új alkalmazás tooa fürt hello utasításokat használja.

### <a name="step-1-upload-hello-application-package"></a>1. lépés: Hello alkalmazáscsomag feltöltése
Hello alkalmazás csomag toohello feltöltése lemezképtárolóhoz hozzáadja egy helyen elérhető toointernal Service Fabric összetevői.  hello alkalmazáscsomag hello szükséges alkalmazás jegyzékfájlja, a szolgáltatás jegyzékfájlban és a kódot, konfigurációs, és a csomag toocreate hello alkalmazás és a szolgáltatáspéldány tartalmazza. Ha azt szeretné, hogy tooverify hello alkalmazáscsomagot helyileg, használja a hello [teszt-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) parancsmag.  Hello [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) feltöltések hello csomag parancsot. Példa:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-hello-application-type"></a>2. lépés: Hello alkalmazástípus regisztrálása
Regisztrálásakor hello alkalmazáscsomag teszi hello alkalmazástípus és -verzió a jegyzékfájlban deklarált hello alkalmazás érhető el használatra. hello rendszer beolvassa az 1. lépésben hello feltöltött hello csomag, ellenőrizni hello csomagot (egyenértékű toorunning [teszt-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) helyileg), feldolgozza hello csomag tartalmát, és másolja a feldolgozott hello csomag tooan belső rendszer helye.  Futtassa a hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancsmagot:

```powershell
Register-ServiceFabricApplicationType WordCount
```
hello fürthöz, futtassa a hello regisztrált összes hello alkalmazástípusok toosee [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancsmagot:

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-hello-application-instance"></a>3. lépés: Hello alkalmazáspéldány létrehozása
Az alkalmazás minden alkalmazástípus verziója hello segítségével sikeresen regisztrált használatával példányosítható [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) parancsot. alkalmazások hello nevét van deklarálva idő telepítése és hello kell kezdődnie **háló:** sémáját, és minden alkalmazáspéldányhoz egyedinek lennie. hello alkalmazás nevét, és az alkalmazástípus verziója táblákon hello **ApplicationManifest.xml** hello alkalmazáscsomag fájlt. Ha az alapértelmezett szolgáltatások hello célalkalmazás típusa hello alkalmazás jegyzékében meghatározott, majd azokat jönnek létre most.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) parancs felsorolja az összes alkalmazáspéldányok sikeresen létrehozott, valamint az átfogó állapotával. Hello [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) parancs megjeleníti az összes hello szolgáltatás példányainak sikeresen létrehozott egy adott alkalmazáspéldány belül. (Ha van ilyen) alapértelmezett szolgáltatások szerepelnek.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Feladat: A Service Fabric-alkalmazás frissítése
Frissíthet egy frissített alkalmazáscsomagot és egy korábban telepített Service Fabric-alkalmazás. Ez a feladat frissíti a telepített hello WordCount alkalmazás "feladat: a Service Fabric-alkalmazás központi telepítése." Olvassa végig [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md) további információt.

az ebben a példában csak hello alkalmazás verziószáma tookeep dolgot egyszerű hello Előfeltételek létrehozott hello WordCountV2 alkalmazáscsomagban frissítve lett. A modell forgatókönyv olyan tartalmaz, amely a szolgáltatás kódot, konfigurációs vagy az adatok fájlok frissítése, majd újraépítése, és frissített verziószámok hello alkalmazás csomagolás.  

### <a name="step-1-upload-hello-updated-application-package"></a>1. lépés: Hello frissített alkalmazáscsomag feltöltése
hello WordCount V1-es alkalmazás készen áll a toobe frissítve. Ha megnyit egy PowerShell-ablak rendszergazda, írja be [ **Get-ServiceFabricApplication**](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), megjelenik a WordCount alkalmazás típusának hello 1.0.0 verzió van telepítve.  

Másolás hello most alkalmazás csomag toohello Service Fabric lemezképtárolóhoz (hello csomagok tárolására a Service Fabric által) frissítése. hello paraméter **ApplicationPackagePathInImageStore** arról tájékoztatja a Service Fabric hol találnak a hello alkalmazáscsomagot. a következő parancs másolatok hello alkalmazáscsomag túl hello**WordCountV2** hello kép tárolóban:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-hello-updated-application-type"></a>2. lépés: A frissített alkalmazástípus regisztrálása hello
hello következő lépésre tooregister hello alkalmazás új verzióját hello szolgáltatás-hálóval hello segítségével végrehajtható [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancsmagot:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-hello-upgrade"></a>3. lépés: Indítsa el a hello frissítését
Különböző frissítési paraméterek időtúllépések és health kritériumokat alkalmazott tooapplication frissítések lehetnek. Olvassa végig hello [alkalmazás frissítési paraméterei](service-fabric-application-upgrade-parameters.md) és [frissítési folyamat](service-fabric-application-upgrade.md) további toolearn dokumentumokat. Összes szolgáltatását és példányok kell *kifogástalan* hello frissítés után.  Set hello **HealthCheckStableDuration** too60 másodperc (így hello szolgáltatások esetében működik megfelelően, a következő frissítési tartomány előtt hello frissítés előrehalad toohello legalább 20 másodperc).  Is set hello **UpgradeDomainTimeout** too1200 másodpercig és hello **UpgradeTimeout** too3000 másodperc. Végezetül be hello **UpgradeFailureAction** túl**visszaállítási**, amely a Service Fabric visszaállítja a kérelmek hello toohello korábbi verziója, ha a sikertelen frissítés során hibát.

Most elindíthatja az alkalmazásfrissítés hello hello segítségével [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) parancsmagot:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Vegye figyelembe, hogy hello alkalmazásnév van hello azonos hello korábban telepített v1.0.0 alkalmazásnév (fabric: / WordCount). A Service Fabric használja a név tooidentify, mely az alkalmazás első frissítés. Ha hello időtúllépéseket toobe túl rövid, akkor léphetnek fel, hogy állapotok hello probléma időtúllépési hiba üzenet. Tekintse meg a túl[alkalmazásfrissítések hibaelhárítása](service-fabric-application-upgrade-troubleshooting.md), vagy pedig növelje a hello időtúllépéseket.

### <a name="step-4-check-upgrade-progress"></a>4. lépés: Ellenőrizze frissítési folyamat állapota
Alkalmazás frissítése folyamatban van a figyelheti [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), vagy hello segítségével [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) parancsmagot:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

Néhány perc múlva, hello [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) parancsmag jeleníti meg, hogy minden frissítési tartományok frissített (elvégezve).

## <a name="task-test-a-service-fabric-application"></a>Feladat: A Service Fabric-alkalmazás tesztelése
toowrite magas színvonalú szolgáltatásokat, a fejlesztők kell toobe képes tooinduce megbízhatatlan infrastruktúra hibák tootest hello stabilitását a szolgáltatások. A Service Fabric lehetőséget nyújt a fejlesztők hello képességét tooinduce tartalék műveletek, valamint a hibák használatával hello jelenléte teszt szolgáltatások hello chaos és a feladatátvételi teszt forgatókönyvek.  Olvassa végig [bemutatása toohello hiba az Analysis Services](service-fabric-testability-overview.md) további információt.

### <a name="step-1-run-hello-chaos-test-scenario"></a>1. lépés: Hello chaos tesztkörnyezet futtatása
hello chaos tesztkörnyezet hello teljes Service Fabric-fürt között állít elő hibák. hello forgatókönyv tömöríti a hibák általában hónapokban vagy években tooa keresztül látott néhány óra. hello kombinációja magas tartalék sebességet kihagyásos hibák megkeresése, amelyek egyébként lehet kihagyni esetekben. hello alábbi példa futtatása hello chaos tesztkörnyezet 60 perc.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-hello-failover-test-scenario"></a>2. lépés: Hello feladatátvételi tesztkörnyezet futtatása
hello feladatátvételi teszt forgatókönyv célok hello fürtözési helyett egy adott szolgáltatás partíció, és más szolgáltatások változatlanul hagyja. hello forgatókönyv telepítéseket szolgáltatás érvényesítési szimulált hibák sorozata, az üzleti logika futtatása közben. Szolgáltatás érvényesítési hiba azt jelzi, hogy a hibát, amely további vizsgálat szükséges. hello feladatátvételi teszt mint több hibák helyeken megakadályozását toohello chaos vizsgálati eset egyszerre csak egy tartalék kapott. hello alábbi példa fut hello feladatátvételi teszt 60 percig hello fabric: / WordCount/WordCountService szolgáltatás.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Feladat: Távolítsa el a Service Fabric-alkalmazás
A központilag telepített alkalmazás egy példányának törlése, távolítsa el kiépített hello alkalmazástípus hello fürt, és hello Lemezképtárolóba hello alkalmazáscsomag eltávolítása.

### <a name="step-1-remove-an-application-instance"></a>1. lépés: Az alkalmazáspéldány eltávolítása
Az alkalmazáspéldány nincs szükség, ha véglegesen eltávolíthatja azt hello segítségével [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) parancsmag. Automatikusan is távolítja el az összes szolgáltatás tartozó toohello alkalmazás végleg eltávolítja az összes szolgáltatás állapota. Ez a művelet nem vonható vissza, és alkalmazás állapota nem állítható helyre.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-hello-application-type"></a>2. lépés: Hello alkalmazástípus regisztrációjának törlése
Ha már nincs szüksége egy adott verzióját egy alkalmazás típusa, regisztrációjának megszüntetésére hello segítségével [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) parancsmag. Regisztrációjának megszüntetése nem használt típusok kiadásokban tárolóhely hello alkalmazáscsomag a hello lemezképtárolóhoz használják. Az alkalmazástípus regisztrációjának törlése lehet, mindaddig, amíg nincsenek rajta, vagy függőben hivatkoznak rá alkalmazásfrissítések példányként létrehozott alkalmazások.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-hello-application-package"></a>3. lépés: Hello az alkalmazáscsomag eltávolítása
Miután hello alkalmazástípus regisztrációjának törlése, hello alkalmazáscsomag eltávolítható hello lemezképtárolóhoz hello segítségével [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) parancsmag.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Következő lépések
[A Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md)

[Alkalmazás üzembe helyezése](service-fabric-deploy-remove-applications.md)

[Az alkalmazásfrissítés](service-fabric-application-upgrade.md)

[Azure Service Fabric-parancsmagokkal](/powershell/azure/overview?view=azureservicefabricps)


---
title: "Automatizált Azure Service Fabric-alkalmazás felügyelete |} Microsoft Docs"
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
ms.openlocfilehash: fb3c2a77ea887289eebf343e223c190781d0e4c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="automate-the-application-lifecycle-using-powershell"></a>Az alkalmazás-életciklus PowerShell használatával automatizálhatja
Sok aspektusainak a [Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md) automatizálható.  Ez a cikk bemutatja, hogyan használhatja a PowerShell telepítése, frissítése, eltávolítása és tesztelés az Azure Service Fabric-alkalmazások esetében gyakori feladatok automatizálására.  Felügyelt, és az Alkalmazáskezelés HTTP API-k is elérhetők. Lásd: [alkalmazás-életciklus](service-fabric-application-lifecycle.md) további információt.  

## <a name="prerequisites"></a>Előfeltételek
Mielőtt továbblép a feladatokat a cikkben, ügyeljen arra, hogy:

* Ismerkedjen meg a Service Fabric ismertetett fogalmakat [technikai áttekintése a Service Fabric](service-fabric-technical-overview.md).
* [A futtatókörnyezet, az SDK és az eszközök telepítése](service-fabric-get-started.md), amely is telepíti a **ServiceFabric** PowerShell-modult.
* [PowerShell-parancsfájl végrehajtásának engedélyezése](service-fabric-get-started.md#enable-powershell-script-execution).
* A helyi fürt elindításához.  Indítsa el a rendszergazdaként egy új PowerShell-ablakot, és futtassa a fürtbeállítási parancsfájlt az SDK mappából:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
* Ebben a cikkben a PowerShell-parancsok futtatása, előtt először csatlakozik a helyi Service Fabric-fürt [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps):`Connect-ServiceFabricCluster localhost:19000`
* A következő feladatokat a V1-es alkalmazáscsomag telepítéséhez és a v2 alkalmazáscsomagok van szükségük a frissítést. Töltse le a [ **WordCount** mintaalkalmazás](http://aka.ms/servicefabricsamples) (az első lépések minták található). Hozza létre, és a Visual Studio alkalmazás becsomagolása (kattintson a jobb gombbal a **WordCount** megoldáskezelő, és válasszon **csomag**). Másolja a v1 csomagot a `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` való `C:\Temp\WordCount`. Másolás `C:\Temp\WordCount` való `C:\Temp\WordCountV2`, a frissítés a v2 alkalmazáscsomagot létrehozása. Nyissa meg `C:\Temp\WordCountV2\ApplicationManifest.xml` egy szövegszerkesztőben. Az a **applicationmanifest jegyzékben** elem, módosítsa a **ApplicationTypeVersion** attribútumot "1.0.0" a "2.0.0" gombra az alkalmazás verziószám frissítéséhez. Mentse a módosított ApplicationManifest.xml fájlt.

## <a name="task-deploy-a-service-fabric-application"></a>Feladat: A Service Fabric-alkalmazás központi telepítése
Miután már beépített és az alkalmazás csomagolt (vagy letöltött alkalmazáscsomag), telepítheti az alkalmazást egy helyi Service Fabric-fürt. Központi telepítés magában foglalja az alkalmazáscsomag feltöltését, az alkalmazástípus regisztrálása és az alkalmazás példányának létrehozásakor. Kövesse az utasításokat ebben a szakaszban egy fürt egy új alkalmazást telepíti.

### <a name="step-1-upload-the-application-package"></a>1. lépés: Az alkalmazáscsomag feltöltése
Az alkalmazáscsomag feltöltését az image store helyezi helyen elérhető belső Service Fabric-összetevők.  Az alkalmazáscsomag tartalmazza a szükséges alkalmazás jegyzékfájlja, a szolgáltatás jegyzékfájlban és a kódot, konfiguráció és adatok csomag(ok) az alkalmazás és szolgáltatás példányainak létrehozásához. Ha szeretné ellenőrizni a csomag helyi, használja a [teszt-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) parancsmag.  A [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) parancs feltölti a csomagot. Példa:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>2. lépés: Az alkalmazástípus regisztrálása
Az alkalmazáscsomag regisztrációja lehetővé teszi az alkalmazás típusától és verziójától érhető el az alkalmazás jegyzékében használható deklarált. A rendszer olvasás a csomag feltöltése az 1. lépésben a csomag ellenőrzése (futó egyenértékű [teszt-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) helyileg), feldolgozza a csomag tartalmát, és másolja a feldolgozott csomagot egy belső rendszer hely.  Futtassa a [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancsmagot:

```powershell
Register-ServiceFabricApplicationType WordCount
```
A fürt regisztrált alkalmazástípusok megtekintéséhez futtassa a [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancsmagot:

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>3. lépés: Az alkalmazáspéldány létrehozása
Egy alkalmazás példányosítható bármely alkalmazástípus verziója, amely regisztrálása sikeresen befejeződött a használatával a [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) parancsot. Minden alkalmazás neve van deklarálva idő telepítése és kell kezdődnie az **háló:** sémáját, és minden alkalmazáspéldányhoz egyedinek lennie. Az alkalmazás nevét, és az alkalmazástípus verziója deklarálva van a **ApplicationManifest.xml** alkalmazáscsomag fájlt. Ha a célalkalmazás típusa, az alkalmazás jegyzékében meghatározott alapértelmezett szolgáltatások, akkor azok jönnek létre most.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

A [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) parancs felsorolja az összes alkalmazáspéldányok sikeresen létrehozott, valamint az átfogó állapotával. A [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) parancs felsorolja a szolgáltatáspéldány sikeresen létrehozott egy adott alkalmazáspéldány belül. (Ha van ilyen) alapértelmezett szolgáltatások szerepelnek.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Feladat: A Service Fabric-alkalmazás frissítése
Frissíthet egy frissített alkalmazáscsomagot és egy korábban telepített Service Fabric-alkalmazás. Ez a feladat frissíti a telepített a WordCount alkalmazás "feladat: a Service Fabric-alkalmazás központi telepítése." Olvassa végig [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md) további információt.

Dolgot egyszerű ehhez a példához megtartásához csak az alkalmazás verziószáma frissült az Előfeltételek létrehozott WordCountV2 alkalmazáscsomagban. A modell forgatókönyv olyan tartalmaz, amely a szolgáltatás kódot, konfigurációs vagy az adatok fájlok frissítése, majd újraépítése, és frissített verziószámok az alkalmazás csomagolás.  

### <a name="step-1-upload-the-updated-application-package"></a>1. lépés: A frissített alkalmazáscsomag feltöltése
A WordCount V1-es alkalmazás készen áll a frissíthető. Ha megnyit egy PowerShell-ablak rendszergazda, írja be [ **Get-ServiceFabricApplication**](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), láthatja, hogy a WordCount alkalmazás típusú 1.0.0 verzió van telepítve.  

Most másolja a frissített alkalmazáscsomagot (az alkalmazáscsomagok tárolására Service Fabric által) a Service Fabric lemezképtárolóhoz. A paraméter **ApplicationPackagePathInImageStore** arról tájékoztatja a Service Fabric hol találnak a alkalmazáscsomagot. A következő parancsot, másolja át az alkalmazáscsomagot **WordCountV2** a az image store:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>2. lépés: A frissített alkalmazástípus regisztrálása
A következő lépés az, hogy regisztrálja az alkalmazás új verzióját a Service Fabric, amelyek segítségével hajtható végre a [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancsmagot:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>3. lépés: A frissítés megkezdése
Különböző frissítési paraméterek időtúllépések és health kritériumokat alkalmazásfrissítések alkalmazhatja. Olvassa végig a [alkalmazás frissítési paraméterei](service-fabric-application-upgrade-parameters.md) és [frissítési folyamat](service-fabric-application-upgrade.md) további dokumentumokat. Összes szolgáltatását és példányok kell *kifogástalan* a frissítés után.  Állítsa be a **HealthCheckStableDuration** 60 másodperc (így a szolgáltatások csak kifogástalan legalább 20 másodperc, mielőtt továbblép a frissítés a következő frissítési tartományra).  Állítson be úgy a **UpgradeDomainTimeout** 1200-as másodpercre és a **UpgradeTimeout** 3000 másodpercre. Végezetül be a **UpgradeFailureAction** való **visszaállítási**, amely kéri, hogy a Service Fabric visszaállítja a az alkalmazás korábbi verziójának Ha a sikertelen frissítés során hibát.

Az alkalmazásfrissítés használatával most elindíthatja a [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) parancsmagot:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Vegye figyelembe, hogy az alkalmazás neve ugyanaz, mint a korábban telepített v1.0.0 alkalmazásnév (fabric: / WordCount). A Service Fabric azonosítására, mely az alkalmazás első frissítés használja ezt a nevet. Ha az időtúllépéseket túl rövid, esetleg felmerülő egy időtúllépési hibaüzenet arról, hogy a problémát. Tekintse meg [alkalmazásfrissítések hibaelhárítása](service-fabric-application-upgrade-troubleshooting.md), vagy növelje az időtúllépéseket.

### <a name="step-4-check-upgrade-progress"></a>4. lépés: Ellenőrizze frissítési folyamat állapota
Alkalmazás frissítése folyamatban van a figyelheti [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), vagy a [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) parancsmagot:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

Néhány perc múlva a [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) parancsmag jeleníti meg, hogy minden frissítési tartományok frissített (elvégezve).

## <a name="task-test-a-service-fabric-application"></a>Feladat: A Service Fabric-alkalmazás tesztelése
Magas színvonalú szolgáltatásokat írni fejlesztők kell tudni idéz elő a nem megbízható infrastruktúra hibák szolgáltatásaik stabilitásának teszteléséhez. A Service Fabric lehetővé teszi a fejlesztők a tartalék műveletek végrehajtását, és tesztelje a szolgáltatásokat hibák esetén a chaos és a feladatátvételi teszt forgatókönyv.  Olvassa végig [bemutatása a tartalék Analysis Service](service-fabric-testability-overview.md) további információt.

### <a name="step-1-run-the-chaos-test-scenario"></a>1. lépés: A chaos vizsgálati eset futtatása
A chaos tesztkörnyezet hibák között a teljes Service Fabric-fürtöt hoz létre. A forgatókönyv tömöríti a hibák általában hónapokban vagy években néhány óra alatt látható. Magas tartalék sebességet kihagyásos hibák kombinációja, ellenkező esetben lehet kihagyni esetekben keresi. A következő példa a chaos tesztkörnyezet 60 percig fut.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>2. lépés: A feladatátvételi teszt forgatókönyv futtatása
A feladatátvételi teszt forgatókönyv célokat egy adott szolgáltatás partíció helyett a teljes fürtöt, és más szolgáltatások változatlanul hagyja. A forgatókönyv telepítéseket szolgáltatás érvényesítési szimulált hibák sorozata, az üzleti logika futtatása közben. Szolgáltatás érvényesítési hiba azt jelzi, hogy a hibát, amely további vizsgálat szükséges. A feladatátvételi teszt csak egy hibaüzenetet kapott egyszerre több hibák helyeken chaos tesztkörnyezet szemben. A következő példa a feladatátvételi teszt 60 percig fut a fabric: / WordCount/WordCountService szolgáltatás.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Feladat: Távolítsa el a Service Fabric-alkalmazás
A központilag telepített alkalmazás egy példányának törlése, a kiépített alkalmazástípus eltávolítása a fürtből, és az alkalmazáscsomag eltávolítása az ImageStore.

### <a name="step-1-remove-an-application-instance"></a>1. lépés: Az alkalmazáspéldány eltávolítása
Az alkalmazáspéldány nincs szükség, ha véglegesen eltávolíthatja azt használatával a [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) parancsmag. Végleg eltávolítja az összes szolgáltatás állapotának alkalmazáshoz tartozó összes szolgáltatás automatikusan is távolítja el. Ez a művelet nem vonható vissza, és alkalmazás állapota nem állítható helyre.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>2. lépés: Az alkalmazástípus regisztrációjának törlése
Ha már nincs szüksége egy adott verzióját egy alkalmazás típusa, regisztráció segítségével a [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) parancsmag. Az image store az alkalmazáscsomag által felhasznált tárterület beállításjegyzékből való törlésekor a nem használt típusok kiadását. Az alkalmazástípus regisztrációjának törlése lehet, mindaddig, amíg nincsenek rajta, vagy függőben hivatkoznak rá alkalmazásfrissítések példányként létrehozott alkalmazások.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>3. lépés: Az alkalmazáscsomag eltávolítása
Miután az alkalmazástípus regisztrációjának törlése, az alkalmazáscsomag eltávolíthatja az image store használatával a [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) parancsmag.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Következő lépések
[A Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md)

[Alkalmazás üzembe helyezése](service-fabric-deploy-remove-applications.md)

[Az alkalmazásfrissítés](service-fabric-application-upgrade.md)

[Azure Service Fabric-parancsmagokkal](/powershell/azure/overview?view=azureservicefabricps)


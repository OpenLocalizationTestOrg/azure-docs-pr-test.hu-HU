---
title: "PowerShell-lel aaaService háló alkalmazás frissítése |} Microsoft Docs"
description: "Ez a cikk végigvezeti a Service Fabric-alkalmazás telepítése, hello kód megváltoztatása és PowerShell használatával frissítés terítésével hello élménye."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 9bc75748-96b0-49ca-8d8a-41fe08398f25
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: f31212264de45c3b257a0efafb75c10c279b989f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-using-powershell"></a>Az alkalmazásfrissítés Service Fabric PowerShell-lel
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

a leggyakrabban használt hello és ajánlott frissítési módszer figyelt hello működés közbeni frissítés.  Az Azure Service Fabric figyeli a hello állapotát hello alkalmazás frissítés alatt állapotfigyelő házirendjei alapján. Miután egy frissítési tartományt (UD) frissítve van, a Service Fabric hello alkalmazás állapotának kiértékeli, és megtennie toohello tovább frissítési tartományt vagy attól függően, hogy hello állapotházirendeket hello frissítése sikertelen.

A figyelt alkalmazáshoz frissítés használatával végezheti el felügyelt hello vagy natív API-k, PowerShell vagy a többi. Visual Studio használatával történő frissítése, lásd: [Visual Studio segítségével az alkalmazás frissítését](service-fabric-application-upgrade-tutorial.md).

A Service Fabric figyelt működés közbeni frissítések hello alkalmazás-rendszergazda segítségével beállíthatja hello értékelési állapotházirend, hogy a Service Fabric toodetermine használja, ha hello alkalmazás állapota kifogástalan. Ezenkívül hello rendszergazda úgy is konfigurálhatja a hello művelet toobe hajt végre, amikor hello állapotának kiértékelését sikertelen (például egy automatikus visszaállítási történt.) Ez a szakasz végigvezeti egy PowerShell használó hello SDK-minták a figyelt frissítés. hello következő a Microsoft Virtual Academy videó is végigvezeti egy alkalmazás frissítése:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=OrHJH66yC_6406218965">
<img src="./media/service-fabric-application-upgrade-tutorial-powershell/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="step-1-build-and-deploy-hello-visual-objects-sample"></a>1. lépés: Build és telepít hello Visual objektumok
Hozza létre, és kattintson a jobb gombbal a hello-alkalmazás projekt hello alkalmazás közzététele **VisualObjectsApplication,** , majd válassza a hello **közzététel** parancsot.  További információkért lásd: [Service Fabric-alkalmazás frissítési oktatóanyag](service-fabric-application-upgrade-tutorial.md).  Másik megoldásként használhatja PowerShell toodeploy az alkalmazást.

> [!NOTE]
> PowerShell hello Service Fabric parancsok is használhatók, először jóvá kell tooconnect toohello fürt hello segítségével `Connect-ServiceFabricCluster` parancsmag. Hasonlóképpen feltételezzük, hogy hello fürt már be van állítva a helyi számítógépen. Hello cikkében [a Service Fabric fejlesztési környezet létrehozása](service-fabric-get-started.md).
> 
> 

Hello PowerShell parancs használata után felépítése hello projektre a Visual Studióban, [másolási-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) toocopy hello alkalmazás csomag toohello Lemezképtárolóba. Ha azt szeretné, hogy tooverify hello alkalmazáscsomagot helyileg, használja a hello [teszt-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) parancsmag. hello következő lépésre tooregister hello alkalmazás toohello Service Fabric-futtatókörnyezet hello segítségével [Register-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) parancsmag. hello utolsó lépésként toostart hello alkalmazás egy példánya segítségével el hello [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) parancsmag.  Ezen három lépésekre hasonló toousing hello **telepítés** menüpont a Visual Studióban.

Most már használhatja [Service Fabric Explorer tooview hello fürt és hello alkalmazás](service-fabric-visualizing-your-cluster.md). hello alkalmazás rendelkezik egy webszolgáltatás, amely az Internet Explorer tooin nyílik beírásával [http://localhost: 8081/visualobjects](http://localhost:8081/visualobjects) hello címsorába.  Megtekintheti az egyes lebegőpontos visual objektumokat, üdvözlő képernyőt Navigálás.  Emellett használhatja [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) toocheck hello alkalmazás állapotát.

## <a name="step-2-update-hello-visual-objects-sample"></a>2. lépés: A frissítés hello Visual objektumok minta
Bizonyára észrevette, hogy, hogy 1. lépésben telepített hello verziójával, hello visual objektumok nem elforgatása. Most frissítse az alkalmazás tooone, ahol hello látható objektumokat is elforgatása.

Válassza ki a hello VisualObjects.ActorService projekt hello VisualObjects megoldás belül, és nyissa meg a hello StatefulVisualObjectActor.cs fájlt. Ugrás a fájlt, toohello metódus `MoveObject`, megjegyzéssé `this.State.Move()`, és állítsa vissza `this.State.Move(true)`. Ez a változás hello objektumok elforgatja, hello szolgáltatás frissítése után.

Igazolnia kell tooupdate hello *ServiceManifest.xml* fájlt (PackageRoot) hello projekt **VisualObjects.ActorService**. Frissítés hello *CodePackage* hello szolgáltatás verziója too2.0 és hello sorai hello *ServiceManifest.xml* fájlt.
Használhatja a Visual Studio hello *Manifest fájlok szerkesztése* lehetőséget, kattintson a jobb gombbal a hello megoldás toomake hello jegyzékfájl módosítások után.

Hello módosítása után, hello jegyzékfájl hello hasonlóan kell kinéznie (a kijelölt részei hello módosítások megjelenítése):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Most hello *ApplicationManifest.xml* fájl (hello alatt található **VisualObjects** projekt alatt hello **VisualObjects** megoldás) frissített tooversion 2.0 hello van **VisualObjects.ActorService** projekt. Ezenkívül hello alkalmazás verziója a 1.0.0.0 frissített too2.0.0.0. Hello *ApplicationManifest.xml* kell hello kinézetét következő kódrészletet:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Most, hello projekt buildjének elkészítéséhez, csak hello kiválasztásával **ActorService** projektre, majd kattintson a jobb gombbal, és hello kiválasztása **Build** lehetőséget a Visual Studio. Ha **építse újra az összes**, mivel hello kód volna frissítenie kell az összes projektek hello verziók. Ezt követően most csomag hello frissítése kattintson a jobb gombbal az alkalmazás ***VisualObjectsApplication***, hello Service Fabric menü kiválasztásával, és válassza **csomag**. Ez a művelet létrehoz egy alkalmazáscsomagot, amely telepíthető.  A frissített alkalmazás készen áll a toobe telepítve.

## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>3. lépés: Házirendek mellett dönt, és frissítse a paraméterek
Ismerkedjen meg hello [alkalmazás frissítési paraméterei](service-fabric-application-upgrade-parameters.md) és hello [frissítési folyamat](service-fabric-application-upgrade.md) tooget különböző paraméterek, a várakozási idő és a rendszerállapot feltétel alkalmazása frissítése hello beható ismerete . Ennél a bemutatónál hello szolgáltatás állapotának kiértékelési feltételnek toohello alapértelmezett (és a javasolt) értéket, ami azt jelenti, hogy az összes szolgáltatás és példányok legyen *kifogástalan* hello frissítés után.  

Azonban nézzük növelése hello *HealthCheckStableDuration* too60 másodperc (így hello szolgáltatások esetében működik megfelelően, a előtt hello frissítés előrehalad toohello legalább 20 másodperc a következő frissítés tartomány).  Is beállíthat a hello *UpgradeDomainTimeout* toobe 1200-as másodperc és hello *UpgradeTimeout* toobe 3000 másodperc.

Végezetül is állítsa hello *UpgradeFailureAction* toorollback. Ez a beállítás a Service Fabric tooroll hátsó hello alkalmazás toohello előző verziója szükséges, ha probléma merül fel hello frissítés során találkozik. Ebből kifolyólag hello frissítése (a 4. lépés) indításakor hello következő paraméterek vannak megadva:

FailureAction visszaállítási =

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000

## <a name="step-4-prepare-application-for-upgrade"></a>4. lépés: A frissítésre alkalmazás előkészítése
Most hello alkalmazás épül, és készen áll a toobe frissíteni. Ha megnyit egy PowerShell-ablak rendszergazda, írja be [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), akkor hagyja, hogy azt jelzi, hogy a rendszer alkalmazás típusú 1.0.0.0 **VisualObjects** telepített van.  

hello alkalmazás csomag megtalálható a hello következő relatív elérési utat, ahol tömörítetlen a Service Fabric SDK - hello *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. Ebben a könyvtárban, hello alkalmazáscsomag tárolására látnia kell egy "Csomagja" mappát. Ellenőrizze, hogy a rendszer hello (szükség lehet annak megfelelően, valamint toomodify hello elérési utak) legújabb buildjével hello időbélyegeket tooensure.

Most tegyük a másolási hello alkalmazás csomag toohello Service Fabric Lemezképtárolóba (hello csomagok tárolására a Service Fabric által) frissítése. hello paraméter *ApplicationPackagePathInImageStore* arról tájékoztatja a Service Fabric hol találnak a hello alkalmazáscsomagot. Azt vezettek frissítése hello alkalmazás "VisualObjects\_V2" rendelkező hello a következő parancsot (esetleg toomodify elérési utak újra megfelelően).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

következő lépés hello tooregister van ehhez az alkalmazáshoz, amely hello használatával végezheti el a Service Fabric [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancs:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Hello előző parancs nem jár sikerrel, akkor valószínű, hogy kell-e egy újjáépítését az összes szolgáltatás. 2. lépésben említett, előfordulhat, hogy tooupdate, valamint a WebService verziója.

## <a name="step-5-start-hello-application-upgrade"></a>5. lépés: Indítsa el a hello alkalmazás frissítését
Most, a rendszer minden set toostart hello alkalmazás frissítési hello segítségével [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) parancs:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


hello alkalmazásnév van hello azonos leírtaknak azt hello *ApplicationManifest.xml* fájlt. A Service Fabric használja a név tooidentify, mely az alkalmazás első frissítés. Ha hello időtúllépéseket toobe túl rövid, egy hibaüzenet, hogy állapotok hello probléma merülhetnek fel. Tekintse meg a hibaelhárítási szakaszában toohello, vagy növelje a hello időtúllépéseket.

Most, mert hello alkalmazás frissítési folytatódik, követhető figyelemmel Service Fabric Explorerrel, vagy hello segítségével [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) PowerShell-parancsot: 

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/VisualObjects
```

Néhány perc múlva, hello állapot portáltól megelőző PowerShell-paranccsal, hello segítségével kell nyújtaniuk, hogy minden frissítési tartományok frissített (elvégezve). És látnia kell, hogy hello vizuális objektum a böngészőablakban elindította elforgatása!

Próbálja meg frissíteni a 2-es tooversion 3 vagy a 2-es tooversion egy gyakorlatot 1. 1. 2. verziójú tooversion áthelyezése is figyelembe veszi frissítés. Lejátszásához időtúllépések és állapotfigyelő házirendek toomake saját kezűleg ismeri a őket. Azure-fürttel tooan telepítésekor hello kell toobe paraméterkészlet megfelelően. Jó tooset hello időtúllépéseket konzervatív módon is.

## <a name="next-steps"></a>Következő lépések
[Visual Studio segítségével az alkalmazás frissítését](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.

Szabályozhatja, hogy az alkalmazás használatával frissíti [frissítési paraméterek](service-fabric-application-upgrade-parameters.md).

Az alkalmazásfrissítéseket Learning kompatibilissé hogyan toouse [adatszerializálás](service-fabric-application-upgrade-data-serialization.md).

Ismerje meg, hogyan toouse speciális azáltal túl az alkalmazás frissítésekor funkció[témakörök speciális](service-fabric-application-upgrade-advanced.md).

Javítsa ki a gyakori problémákat alkalmazásfrissítések toohello lépéseit hivatkozással [alkalmazásfrissítések hibaelhárítási](service-fabric-application-upgrade-troubleshooting.md).


---
title: "egy Azure Service Fabric-alkalmazás aaaPackage |} Microsoft Docs"
description: "Hogyan toopackage a Service Fabric-alkalmazás tooa fürt üzembe helyezése előtt."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: b3918e1e25e532acdc9440855213e1fa364ea000
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="package-an-application"></a>Egy alkalmazás becsomagolása
Ez a cikk ismerteti, hogyan toopackage a Service Fabric-alkalmazás, és készen állnak a használatra.

## <a name="package-layout"></a>Csomag elrendezés
hello alkalmazásjegyzék, egy vagy több szolgáltatás jegyzékfájlban és egyéb szükséges fájlokat kell beállítani egy adott elrendezésben átesett szolgáltatássablonjaikat a Service Fabric-fürt csomag. hello példa jegyzékfájlokat az ebben a cikkben a következő könyvtárstruktúrát hello rendezve toobe lenne szükség:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

hello elnevezése toomatch hello **neve** minden megfelelő elem attribútumaihoz. Például akkor, ha hello szolgáltatás jegyzékfájl tartalmazott két kód csomagok hello nevű **MyCodeA** és **MyCodeB**, majd két azonos nevű tartalmazná hello mappa hello minden egyes szükséges bináris fájlokat a csomag.

## <a name="use-setupentrypoint"></a>SetupEntryPoint használata
A jellemző forgatókönyvek **SetupEntryPoint** toorun egy végrehajtható fájl hello szolgáltatás kezdete előtt van szüksége, vagy tooperform emelt szintű jogosultságokkal rendelkező művelet van szüksége van. Példa:

* Beállítását, valamint inicializálása környezeti változókat, amelyek hello végrehajtható van szüksége. Már nem korlátozott tooonly végrehajtható fájlok keresztül hello Service Fabric programozási modellek írása. Például npm.exe kell néhány környezetiblokk-változót, egy node.js-alkalmazás telepítéséhez konfigurált.
* Hozzáférés-vezérlés beállítása biztonsági tanúsítványok telepítésével.

További információt a tooconfigure hello **SetupEntryPoint**, lásd: [egy szolgáltatás telepítője a belépési pont hello házirend konfigurálása](service-fabric-application-runas-security.md)

<a id="Package-App"></a>
## <a name="configure"></a>Konfigurálás
### <a name="build-a-package-by-using-visual-studio"></a>Csomag létrehozása a Visual Studio használatával
Ha a Visual Studio 2015 toocreate használhassa az alkalmazást, hello parancs tooautomatically, hozzon létre egy csomagot, amely megfelel a fenti hello elrendezés csomag is használhatja.

toocreate a csomag, kattintson a jobb gombbal a projektre a Solution Explorer hello és hello csomag parancs, válassza alább látható módon:

![A Visual Studio alkalmazás csomagolás][vs-package-command]

Csomagolás befejeződése után az hello hello csomag hello helyen található **kimeneti** ablak. hello csomagolás lépés esetén automatikusan telepíteni, vagy a Visual Studióban az alkalmazás hibakeresését.

### <a name="build-a-package-by-command-line"></a>Csomag létrehozása a parancssorból
Az is lehetséges tooprogrammatically csomag fel az alkalmazást a `msbuild.exe`. Alatt hello technikai részletek Visual Studio fut, így hello kimeneti azonos.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-hello-package"></a>Hello tesztcsomag
Hello segítségével ellenőrizheti a PowerShell segítségével helyileg hello csomag struktúra [teszt-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) parancsot.
Ez a parancs problémák elemzése jegyzékfájl keres, és ellenőrizze az összes hivatkozást. A parancs csak hello strukturális hello könyvtárak és fájlok hello csomag helyességét ellenőrzi.
Bármelyik hello kóddal vagy az adatok csomag tartalmának ellenőrzése, hogy jelen-e minden szükséges fájlok túl nem ellenőrzése.

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : hello EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Ez a hiba jeleníti meg, hogy hello *MySetup.bat* hello szolgáltatás jegyzékben hivatkozott fájl **SetupEntryPoint** hello kód csomagból hiányzik. Hello hiányzó fájl hozzáadása után hello alkalmazás ellenőrzési továbbítja:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
```

Ha az alkalmazás [alkalmazás paraméterei](service-fabric-manage-multiple-environment-app-configuration.md) definiált, is adja meg azokat a [teszt-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) megfelelő érvényesítés.

Ha tudja, hova szeretné telepíteni hello alkalmazás hello fürt, akkor ajánlott hello adjon át `ImageStoreConnectionString` paraméter. Ebben az esetben a hello csomag is összevetni hello alkalmazás korábbi verzióiban már fut a hello fürtben. Például hello érvényesítési észleli, hogy egy csomag a hello ugyanazt a verziót, de eltérő tartalomra már telepítve lett.  

Miután hello alkalmazás megfelelően van csomagolva, és ellenőrzése sikeres, értékelje ki van szüksége a tömörítési hello méret és a fájlok hello száma alapján.

## <a name="compress-a-package"></a>A csomag tömörítése
Ha nagy vagy sok fájl van egy csomagot, akkor gyorsabb telepítése tömöríthetők. Tömörítés csökkenti a fájlok és a csomag mérete hello hello számát.
A tömörített alkalmazáscsomagot [Uploading hello alkalmazáscsomag](service-fabric-deploy-remove-applications.md#upload-the-application-package) eltarthat már összehasonlított toouploading hello kibontott csomagot (ha kifejezetten tömörítési idő Beleszámítja a), de [regisztrálása](service-fabric-deploy-remove-applications.md#register-the-application-package) és [típustár regisztrációjának hello alkalmazástípus](service-fabric-deploy-remove-applications.md#unregister-an-application-type) gyorsabb a tömörített alkalmazáscsomagot.

hello mechanizmussal legyen, a tömörített és tömörítetlen csomagokat. Ha hello csomag tömörített, a fürt lemezképtárolóhoz hello ilyen tárolja, és azt hello csomóponton van tömörítetlen hello alkalmazás futtatása előtt.
hello tömörítési hello érvényes Service Fabric-csomag hello tömörített verziójával váltja fel. hello mappa engedélyeznie kell írási engedéllyel. Nincs változás tömörítési futó egy már tömörített csomagot adja eredményül.

Egy csomag tömörítheti hello Powershell parancs futtatásával [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) rendelkező `CompressPackage` váltani. Hello kibonthatja csomag hello az azonos parancsot `UncompressPackage` váltani.

hello parancsban tömöríti hello csomag nélkül másolás toohello lemezképtárolóhoz. A tömörített csomag tooone vagy másolhatja további Service Fabric-fürtök használatával szükség szerint [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) nélkül hello `SkipCopy` jelzőt.
hello csomag most már tartalmaz hello tömörített fájlok `code`, `config`, és `data` csomagok. hello alkalmazásjegyzék és hello szolgáltatás jegyzékfájlokban vannak nem zip, mert szükség van a több belső műveletekhez (például a csomag megosztási, alkalmazás neve és verziója kapcsolattípus kibontása az egyes ellenőrzések).
Például .zip fájlba hello jegyzékfájlokban nem elég hatékony teszi ezeket a műveleteket.

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy

PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

Azt is megteheti, tömöríti, és másolja a hello csomagot [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) egy lépésben.
Ha hello csomag túl nagy, adja meg a hello csomag tömörítés és a hello feltöltés toohello fürt elég magas időtúllépési tooallow időpontját.
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Belső a Service Fabric kiszámítja ellenőrzőösszegeket hello alkalmazáscsomagok érvényesítéshez. Tömörítés használata esetén a hello ellenőrzőösszegeket minden csomag verziója zip hello a kiszámítása a történik.
Ha a másolt egy alkalmazáscsomag tömörítetlen verzióját, és azt szeretné, hogy hello toouse tömörítése ugyanazon csomag módosítania kell a hello hello verziói `code`, `config`, és `data` csomagok tooavoid ellenőrzőösszeg eltérése. Ha hello csomagok nem változnak, hello verzió módosítása helyett, akkor használhatja [különbözeti kiépítés](service-fabric-application-upgrade-advanced.md). Ezzel a beállítással nem tartalmaznak hello változatlan csomag helyette hello szolgáltatás jegyzékfájlból hivatkozni rá.

Hasonlóképpen ha szeretné toouse egy tömörítetlen csomag hello csomag tömörített változatának feltöltött, frissítenie kell a hello verziók tooavoid hello nem egyeznek az ellenőrzőösszegek is.

hello csomag most már megfelelően csomagolt, érvényesítve, és tömörített (ha szükséges), hogy készen álljanak [telepítési](service-fabric-deploy-remove-applications.md) tooone vagy további Service Fabric-fürtök.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Tömöríti a csomagok központi telepítésekor a Visual Studio használatával
Visual Studio toocompress csomagokat a központi telepítés, utasíthatja hello hozzáadásával `CopyPackageParameters` elem tooyour közzétételi profillal, és állítsa be a hello `CompressPackage` túl attribútum`true`.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a>Következő lépések
[Központi telepítése és távolíthat el alkalmazásokat] [ 10] ismerteti, hogyan toouse PowerShell toomanage alkalmazáspéldányok

[Alkalmazás paramétereinek több környezet kezelése] [ 11] ismerteti, hogyan tooconfigure paraméterek és a környezeti változók különböző alkalmazás-példányok.

[Az alkalmazás biztonsági szabályzatainak konfigurálásához] [ 12] ismerteti, hogyan toorun a biztonsági házirendek toorestrict hozzáférés szolgáltatásokat.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md

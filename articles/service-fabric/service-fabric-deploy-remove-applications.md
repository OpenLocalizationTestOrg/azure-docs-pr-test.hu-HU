---
title: "a Service Fabric-alkalmazás központi telepítésének aaaAzure |} Microsoft Docs"
description: "Hogyan toodeploy, és távolítsa el a Service Fabric PowerShell-lel alkalmazások."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: 3de9c6a937ee7b29bf9ec86d6e9e631487797507
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-powershell"></a>Központi telepítése, és távolítsa el az alkalmazásokat a PowerShell használatával
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [FabricClient API-k](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Egyszer egy [alkalmazástípus csomagolás][10], az Azure Service Fabric-fürt szolgáltatássablonjaikat használatra kész. Központi telepítés hello a következő három lépést foglal magában:

1. Hello alkalmazás csomag toohello lemezképtárolóhoz feltöltése
2. Hello alkalmazástípus regisztrálása
3. Hello alkalmazáspéldány létrehozása

Miután egy alkalmazás lett telepítve, és a példány hello fürtben fut, törölheti a hello alkalmazáspéldány és az alkalmazás típusa. toocompletely eltávolítása egy alkalmazáskészletet hello fürtből hello lépéseket foglalja magában:

1. Alkalmazás-példányt futtató hello eltávolítása (vagy törlése)
2. Ha már nincs szüksége a hello alkalmazástípus regisztrációjának törlése
3. Hello lemezképtárolóhoz hello alkalmazáscsomag eltávolítása

Ha [központi telepítéséhez és alkalmazások hibakeresése a Visual Studio](service-fabric-publish-app-remote-cluster.md) a helyi fejlesztési fürtöt, a fenti lépéseket minden hello egy PowerShell-parancsfájl segítségével automatikusan kezeli.  Ez a parancsfájl található hello *parancsfájlok* hello projekt mappájából. Ez a cikk ismerteti a háttérben, hogy a parancsfájl tevékenységétől, hogy végezheti el a Visual Studio kívül ugyanazokat a műveleteket hello. 
 
## <a name="connect-toohello-cluster"></a>Csatlakoztassa toohello fürtöt
Ebben a cikkben a PowerShell-parancsok futtatása, előtt mindig használatával indítsa el [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) tooconnect toohello Service Fabric-fürt. tooconnect toohello helyi fejlesztési fürtöt, futtassa a következő hello:

```powershell
PS C:\>Connect-ServiceFabricCluster
```

Példák a kapcsolódó tooa távoli fürt vagy az Azure Active Directoryval, X509 védett fürt tanúsítványokat, vagy a Windows Active Directory [Connect tooa biztonságos fürt](service-fabric-connect-to-secure-cluster.md).

## <a name="upload-hello-application-package"></a>Hello alkalmazáscsomag feltöltése
Feltöltése hello alkalmazáscsomag belső Service Fabric összetevői által elérhető helyen helyezi.
Ha azt szeretné, hogy tooverify hello alkalmazáscsomagot helyileg, használja a hello [teszt-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) parancsmag.

Hello [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) feltöltések hello alkalmazás csomag toohello fürt lemezképtárolóhoz parancsot.
Hello **Get-ImageStoreConnectionStringFromClusterManifest** hello Service Fabric SDK PowerShell modul részét képező parancsmag értéke használt tooget hello image kapcsolati karakterláncot tárolni.  tooimport hello SDK modul, futtassa:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Tegyük fel, hogy most felépíti és nevű csomag *MyApplication* a Visual Studio 2015-öt. Alapértelmezés szerint a felsorolt hello ApplicationManifest.xml hello alkalmazástípus neve "MyApplicationType".  hello alkalmazáscsomagot, amely tartalmazza a hello szükséges alkalmazás jegyzékfájlja, a szolgáltatás jegyzékfájlban és a kód/config/adatok csomagok, található *C:\Users\<felhasználónév\>\Documents\Visual Studio 2015\Projects\ MyApplication\MyApplication\pkg\Debug*. 

hello következő parancs kilistázza hello alkalmazáscsomag hello tartalmát.

```powershell
PS C:\> $path = 'C:\Users\<user\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
│   ApplicationManifest.xml
│
└───Stateless1Pkg
    │   ServiceManifest.xml
    │
    ├───Code
    │       Microsoft.ServiceFabric.Data.dll
    │       Microsoft.ServiceFabric.Data.Interfaces.dll
    │       Microsoft.ServiceFabric.Internal.dll
    │       Microsoft.ServiceFabric.Internal.Strings.dll
    │       Microsoft.ServiceFabric.Services.dll
    │       ServiceFabricServiceModel.dll
    │       Stateless1.exe
    │       Stateless1.exe.config
    │       Stateless1.pdb
    │       System.Fabric.dll
    │       System.Fabric.Strings.dll
    │
    └───Config
            Settings.xml
```

Ha hello alkalmazáscsomag nagy és/vagy sok fájl van, akkor [a tömörítés](service-fabric-package-apps.md#compress-a-package). hello tömörítés csökkenti a hello mérete és a fájlok hello száma.
hello mellékhatása a regisztrálási és típustár regisztrációjának hello alkalmazástípus gyorsabbak. Ideje csökkenhet jelenleg, különösen akkor, ha megadja a hello idő toocompress hello csomag. 

a csomag, használjon toocompress hello azonos [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) parancsot. Tömörítés külön között elvégezhető feltöltés hello segítségével `SkipCopy` jelzőt, vagy együtt hello töltse fel a műveletet. A tömörített csomag tömörítés alkalmazása műveletvégzés.
a tömörített csomag, használjon toouncompress hello azonos [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) hello parancsot `UncompressPackage` váltani.

hello következő parancsmag tömöríti hello csomag nélkül másolás toohello lemezképtárolóhoz. hello csomag most már tartalmaz hello tömörített fájlok `Code` és `Config` csomagok. hello alkalmazás és hello szolgáltatás jegyzékfájlokban vannak nem zip, mert szükség van a több belső műveletekhez (például a csomag megosztási, alkalmazás neve és verziója kapcsolattípus kibontása az egyes ellenőrzések). Például .zip fájlba hello jegyzékfájlokban nem elég hatékony teszi ezeket a műveleteket.

```
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -CompressPackage -SkipCopy
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
|   ApplicationManifest.xml
|
└───Stateless1Pkg
       Code.zip
       Config.zip
       ServiceManifest.xml
```

A nagy alkalmazáscsomagok hello tömörítési időt vesz igénybe. A legjobb eredmények elérése érdekében a gyors SSD meghajtó használatát. hello tömörítési idejét és a tömörített csomag hello hello méretének is attól függően változnak, hello csomag tartalmát.
Például ez egyes csomagok, amelyek hello kezdeti megjelenítése és a tömörített csomag mérete, a hello tömörítési idővel hello tömörítési statisztika.

|Kezdeti méret (MB)|Fájlok száma|Tömörítés idő|Tömörített mérete (MB)|
|----------------:|---------:|---------------:|---------------------------:|
|100|100|00:00:03.3547592|60|
|512|100|00:00:16.3850303|307|
|1024|500|00:00:32.5907950|615|
|2048|1000|00:01:04.3775554|1231|
|5012|100|00:02:45.2951288|3074|

Ha egy csomag tömörített, feltöltött tooone lehet, vagy több Service Fabric-fürtök szükség szerint. hello mechanizmussal legyen, a tömörített és tömörítetlen csomagokat. Ha hello csomag tömörített, a fürt lemezképtárolóhoz hello ilyen tárolja, és azt hello csomóponton van tömörítetlen hello alkalmazás futtatása előtt.


hello alábbi példa feltölt hello csomag toohello lemezképtárolóhoz "MyApplicationV1" nevű mappába:

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
```

Hello **Get-ImageStoreConnectionStringFromClusterManifest** hello Service Fabric SDK PowerShell modul részét képező parancsmag értéke használt tooget hello image kapcsolati karakterláncot tárolni.  tooimport hello SDK modul, futtassa:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Ha nem adja meg a hello *- ApplicationPackagePathInImageStore* paraméter, hello alkalmazáscsomag hello lemezképtárolóhoz hello "Debug" mappába történő másolása.

hello időt tooupload egy csomag több tényezőktől függően eltér. Ezek a tényezők némelyike hello hello csomag, a hello mérete és a fájlméret hello fájlok száma. hello hálózati sebesség hello forrásgép és hello Service Fabric-fürt között is hatással van a hello ideje. az alapértelmezett időkorlát hello [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) 30 perc.
Attól függően, hello ismertetett tényezők, előfordulhat, hogy tooincrease hello időtúllépés. Hello csomag hello másolási hívásban gazdameghajtóhoz, ha szüksége van-e tooalso fontolja meg a hello tömörítési idő.

Lásd: [hello lemezkép tárolási kapcsolati karakterlánc megértéséhez](service-fabric-image-store-connection-string.md) kiegészítő információkat megtudni hello image store és lemezkép tárolásához kapcsolati karakterláncot.

## <a name="register-hello-application-package"></a>Hello alkalmazáscsomag regisztrálása
regisztrálva van-e hello alkalmazáscsomag válnak használható hello alkalmazás jegyzékfájlban deklarált hello alkalmazástípus és -verzió. hello rendszer beolvassa az előző lépésben hello feltöltött hello csomag, hello csomag ellenőrzi, hello csomag tartalmának folyamatokat, és másolja át a feldolgozott hello csomagok tooan belső rendszer helyét.  

Futtassa a hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancsmag tooregister hello alkalmazástípus hello fürt, és tegye elérhetővé a központi telepítéshez:

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

"MyApplicationV1" az hello mappa, ahol hello alkalmazáscsomag hello kép tárolójában. hello alkalmazás típus neve "MyApplicationType" és "1.0.0" verzió (egyaránt megtalálható hello alkalmazásjegyzék) hello fürt most már regisztrálva van.

Hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancs csak azt követően hello rendszeren csak a sikeresen regisztrált hello alkalmazáscsomag adja vissza. Mennyi ideig regisztrációs vesz hello méretét és hello alkalmazáscsomag tartalmának függ. Szükség esetén – hello **- TimeoutSec** paraméter lehet használt toosupply egy hosszabb időtúllépési (hello alapértelmezett időtúllépési érték 60 másodperc).

Ha egy csomagot, vagy ha időtúllépéseket tapasztalnak, használjon hello nagy alkalmazás **- aszinkron** paraméter. hello parancs hello fürt elfogadja hello register parancsot, és hello feldolgozási továbbra is fennáll, igény szerint adja vissza.
Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancs megjeleníti az összes sikeresen regisztrált alkalmazástípus-verziók és a regisztrációs állapotot. Ez a parancs toodetermine is használhatja, amikor hello regisztrációs hajtja végre.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="create-hello-application"></a>Hello alkalmazás létrehozása
Az alkalmazás minden alkalmazástípus verziója hello segítségével sikeresen regisztrált a példányosítható [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) parancsmag. minden alkalmazás hello neve kezdődhet hello *"fabric:"* sémáját, és minden alkalmazáspéldányhoz egyedinek kell lennie. Hello célalkalmazás típusa hello alkalmazás jegyzékében definiált alapértelmezett szolgáltatások is jönnek létre.

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
Több alkalmazáspéldányt is létrehozható egy regisztrált alkalmazástípus bármely adott verzióját. Minden egyes alkalmazáspéldány feladata elkülönítve, a saját munkahelyi directory és a folyamat.

toosee, amely az alkalmazások és szolgáltatások nevű futtató hello fürthöz, futtassa a hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) és [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) parancsmagokat:

```powershell
PS C:\> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : {}

PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/Stateless1
ServiceKind            : Stateless
ServiceTypeName        : Stateless1Type
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
ServiceStatus          : Active
HealthState            : Ok
```

## <a name="remove-an-application"></a>Alkalmazások eltávolítása
Az alkalmazáspéldány nincs szükség, ha véglegesen eltávolíthatja azt hello használatával név szerint [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) parancsmag. [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automatikusan eltávolítja az is, végleges eltávolítása az összes szolgáltatás állapotának toohello alkalmazáshoz tartozó összes szolgáltatás. 

> [!WARNING]
> Ez a művelet nem vonható vissza, és az alkalmazás állapota nem állítható helyre.

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a>Az alkalmazástípus regisztrációjának törlése
Egy adott verzióját egy alkalmazás típus már nem szükséges, amikor meg kell alkalmazástípus regisztrációjának törlése hello hello segítségével [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) parancsmag. Regisztrációjának megszüntetése nem használt alkalmazás típusok kiadásokban tárolóhely hello lemezképtárolóhoz használják. Az alkalmazástípus regisztrációjának törlése lehet, mindaddig, amíg nincs alkalmazások létrehozásának rajta, és nem alkalmazás függőben lévő frissítések hivatkozik rá.

Futtatás [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) toosee hello alkalmazástípusok jelenleg regisztrált hello fürt:

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

Futtatás [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) toounregister egy adott alkalmazás típusa:

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="remove-an-application-package-from-hello-image-store"></a>Az alkalmazáscsomag eltávolítása hello lemezképtárolóhoz
Ha már nincs szükség egy alkalmazáscsomagot, hello lemezképet tároló toofree rendszer erőforrásait a törlése.

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

## <a name="troubleshooting"></a>Hibaelhárítás
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Másolás-ServiceFabricApplicationPackage kér egy előtaggal
hello Service Fabric SDK környezet már rendelkezik hello megfelelő alapértelmezett beállítása. De szükség esetén – hello előtaggal összes parancs meg kell felelnie hello értéket, hogy a Service Fabric fürt által használt hello. Hello előtaggal hello fürtjegyzékben található beolvasott hello segítségével [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) és Get-ImageStoreConnectionStringFromClusterManifest parancsokat:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

Hello **Get-ImageStoreConnectionStringFromClusterManifest** hello Service Fabric SDK PowerShell modul részét képező parancsmag értéke használt tooget hello image kapcsolati karakterláncot tárolni.  tooimport hello SDK modul, futtassa:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

hello előtaggal megtalálható a fürtjegyzékben hello:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Lásd: [hello lemezkép tárolási kapcsolati karakterlánc megértéséhez](service-fabric-image-store-connection-string.md) kiegészítő információkat megtudni hello image store és lemezkép tárolásához kapcsolati karakterláncot.

### <a name="deploy-large-application-package"></a>Nagy alkalmazáscsomag telepítése
Probléma: [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) túllépi az időkorlátot a nagy alkalmazáscsomagok (sorrendet, GB).
Próbálja meg:
- Adjon meg egy nagyobb időkorlátjának [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) parancsot, `TimeoutSec` paraméter. Alapértelmezés szerint hello időtúllépés értéke 30 perc.
- Ellenőrizze a hello hálózati kapcsolat a forrásgép és fürt között. Ha hello kapcsolat lassú, érdemes lehet a gépek nagyobb hálózati kapcsolattal rendelkező.
Ha hello ügyfélszámítógép mint hello fürt egy másik régióban van, érdemes lehet egy ügyfélszámítógépre egy szorosabb vagy ugyanabban a régióban hello fürtként.
- Ellenőrizze, hogy ha hogy elérte-e külső szabályozás. Például ha hello lemezképtárolóhoz konfigurált toouse azure tárolási, feltöltés előfordulhat, hogy szabályozva.

Probléma: Feltöltés csomag sikeresen befejeződött, de [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) túllépi az időkorlátot. Próbálja meg:
- [Hello csomag tömörítése](service-fabric-package-apps.md#compress-a-package) toohello lemezképtárolóhoz másolása előtt.
hello tömörítés csökkenti hello és hello fájlok száma, ami viszont hello forgalom mennyiségét csökkenti, és működnek, hogy a Service Fabric kell elvégezni. lehet, hogy hello feltöltési művelet lassabb (különösen ha hello tömörítési idő), de regisztrálása és regisztrációjának hello alkalmazástípus gyorsabb.
- Adjon meg egy nagyobb időkorlátjának [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) a `TimeoutSec` paraméter.
- Adja meg `Async` átkapcsolni a [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). hello parancs ad vissza, ha hello fürt elfogadja hello parancsot, és hello alkalmazástípus regisztrációjának hello aszinkron módon folytatódik. Emiatt nincs nincs szükség toospecify magasabb időkorlát ebben az esetben. Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancs megjeleníti az összes sikeresen regisztrált alkalmazástípus-verziók és a regisztrációs állapotot. Ez a parancs toodetermine is használhatja, amikor hello regisztrációs hajtja végre.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a>Sok fájlokkal alkalmazáscsomag telepítése
Probléma: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) sok fájlt (akár több ezer sorrendben) az alkalmazás csomagot időtúllépése.
Próbálja meg:
- [Hello csomag tömörítése](service-fabric-package-apps.md#compress-a-package) toohello lemezképtárolóhoz másolása előtt. hello tömörítés csökkenti a fájlok hello száma.
- Adjon meg egy nagyobb időkorlátjának [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) a `TimeoutSec` paraméter.
- Adja meg `Async` átkapcsolni a [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). hello parancs ad vissza, ha hello fürt elfogadja hello parancsot, és hello alkalmazástípus regisztrációjának hello aszinkron módon folytatódik.
Emiatt nincs nincs szükség toospecify magasabb időkorlát ebben az esetben. Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancs megjeleníti az összes sikeresen regisztrált alkalmazástípus-verziók és a regisztrációs állapotot. Ez a parancs toodetermine is használhatja, amikor hello regisztrációs hajtja végre.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a>Következő lépések
[A Service Fabric-alkalmazás frissítése](service-fabric-application-upgrade.md)

[A Service Fabric állapotának bemutatása](service-fabric-health-introduction.md)

[Diagnosztizálása és megoldása a Service Fabric-szolgáltatás](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[A Service Fabric alkalmazás minta](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

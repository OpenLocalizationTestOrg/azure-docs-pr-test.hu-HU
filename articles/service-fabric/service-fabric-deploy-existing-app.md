---
title: "egy létező végrehajtható tooAzure Service Fabric aaaDeploy |} Microsoft Docs"
description: "A forgatókönyv a hogyan toopackage egy meglévő alkalmazást, és a Vendég végrehajtható fájlja, így azok telepítve tooa Service Fabric-fürt"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: mfussell;mikhegn
ms.openlocfilehash: 5599802bdb6bda2407a138d77e12148ccb64f437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-guest-executable-tooservice-fabric"></a>Központi telepítése egy Vendég végrehajtható tooService háló
Az Azure Service Fabric szolgáltatásként futtatható kódok, például a Node.js, Java vagy C++ bármilyen típusú. A Service Fabric toothese típusú szolgáltatások Vendég végrehajtható fájlok néven hivatkozik.

Vendég végrehajtható fájlok állapotmentes szolgáltatásokhoz hasonlóan a Service Fabric által kell kezelni. Emiatt a fürt rendelkezésre állási és más metrikák alapján csomópontján kerülnek. Ez a cikk ismerteti, hogyan toopackage és központi telepítése egy Vendég végrehajtható tooa Service Fabric-fürt, a Visual Studio vagy a parancssori segédprogram segítségével.

Ebben a cikkben a Microsoft hello lépéseket toopackage egy végrehajtható Vendég fedik le és telepítse azt tooService háló.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>A Vendég a Service Fabric végrehajtható előnyei
Van több előnyeit toorunning egy Vendég végrehajtható, a Service Fabric-fürt:

* Magas rendelkezésre állású. A Service Fabric futó alkalmazások vannak magas rendelkezésre állású. A Service Fabric biztosítja, hogy futnak-e egy alkalmazás példányai.
* Állapotfigyelés. Service Fabric állapotfigyelésének észleli, ha egy alkalmazás fut, és diagnosztikai információkat nyújt, ha hiba történik.   
* Alkalmazás-életciklus kezelésének. Mellett frissítéseket biztosító állásidő nélkül, a Service Fabric automatikus visszaállítási toohello korábbi verziója biztosít, ha egy frissítés során rossz állapotesemény.    
* Sűrűség. A fürt, amely szükségtelenné teszi hello minden alkalmazás toorun saját hardveren több alkalmazást is futtathatja.
* Felderíthetőség: Használó többi hívása hello Service Fabric Naming service toofind más szolgáltatások hello fürtben. 

## <a name="samples"></a>Példák
* [Minta csomagolás és központi telepítése egy Vendég végrehajtható fájl](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [A két Vendég végrehajtható fájlok (C# és nodejs)-en keresztül hello elnevezési szolgáltatás REST minta](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Alkalmazás és szolgáltatás jegyzékfájlt áttekintése
Központi telepítése egy Vendég végrehajtható részeként hasznos toounderstand hello Service Fabric csomagolás és üzembe helyezési modellben a [alkalmazásmodell](service-fabric-application-model.md). Service Fabric-csomagban modell hello két XML-fájlok támaszkodik: hello alkalmazás és szolgáltatás jegyzékfájljai. hello sémadefiníciót hello ApplicationManifest.xml és ServiceManifest.xml fájlok van telepítve a hello a Service Fabric SDK *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Az alkalmazásjegyzék** hello alkalmazásjegyzék használt toodescribe hello alkalmazás. Az azt alkotó hello szolgáltatások, és hogyan egy vagy több szolgáltatás kell telepíteni, például a példányok száma hello használt toodefine más paramétereket sorolja fel.

  A Service Fabric egy alkalmazás központi telepítése és frissítése munkaegység. Ahol lehetséges hibák és a potenciális visszagörgetése felügyelete egyetlen egységként alkalmazás frissítése. A Service Fabric biztosítja, hogy a frissítési folyamat hello vagy sikeres, vagy hello adatokat sikertelen frissítés esetén, ha nem hagynak hello kérelem ismeretlen vagy instabil állapotban.
* **Szolgáltatás jegyzékfájl** hello szolgáltatás jegyzékfájl szolgáltatás hello összetevőit mutatja be. Ez magában foglalja az adatok, például hello nevét és típusát. a szolgáltatás, és a kódjával és a konfiguráció. hello szolgáltatás jegyzékfájl is néhány további paramétereket tartalmaz, amelyek lehetnek tooconfigure hello szolgáltatást használja, ha telepítve van.

## <a name="application-package-file-structure"></a>Alkalmazás csomag fájlstruktúra
egy alkalmazás tooService háló toodeploy, hello alkalmazás egy előre meghatározott könyvtárstruktúrát kell követnie. hello struktúra egy példa látható.

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

hello ApplicationPackageRoot hello alkalmazás meghatározó hello ApplicationManifest.xml fájl tartalmazza. Egy alkönyvtárban hello alkalmazáshoz tartozó minden egyes szolgáltatás összes hello hello szolgáltatást igénylő összetevők használt toocontain. Ezek alkönyvtárai hello ServiceManifest.xml, és általában a következő hello:

* *Kód*. Ez a könyvtár hello szolgáltatás kódot tartalmaz.
* *Config*. Ez a könyvtár tartalmaz egy Settings.xml fájlban (és egyéb fájlokat, ha szükséges), hogy hello szolgáltatás hozzá tud-e férni futásidejű tooretrieve megadott konfigurációs beállítások.
* *Adatok*. Ez az egy további könyvtár toostore további helyi hello szolgáltatást is szükséges adatokat. Az adatok használt toostore csak rövid élettartamú adatok kell lennie. A Service Fabric másolása vagy replicate módosítások toohello adatkönyvtára végzi, ezért ha hello szolgáltatás toobe (például a feladatátvétel során) áthelyezését.

> [!NOTE]
> Nincs toocreate hello `config` és `data` könyvtárak, ha már nincs szükség.
>
>

## <a name="package-an-existing-executable"></a>Egy létező végrehajtható fájl csomag
Amikor egy Vendég végrehajtható csomagolás, dönthet úgy, vagy toouse a Visual Studio-projektsablont vagy túl[hozzon létre manuálisan hello alkalmazáscsomag](#manually). Visual Studio használatával hello alkalmazás csomag struktúra, és a fájlok a rendszer létrehozza a hello új projektsablon által.

> [!TIP]
> hello legegyszerűbb módja toopackage végrehajtható a szolgáltatásba egy meglévő Windows rendszer toouse Visual Studio és a Linux toouse Yeoman
>

## <a name="use-visual-studio-toopackage-and-deploy-an-existing-executable"></a>Visual Studio toopackage használja, és egy létező végrehajtható fájl központi telepítése
A Visual Studio biztosít a Service Fabric szolgáltatás sablon toohelp Vendég végrehajtható tooa Service Fabric-fürt központi telepítése.

1. Válasszon **fájl** > **új projekt**, és a Service Fabric-alkalmazás létrehozása.
2. Válasszon **Vendég végrehajtható** hello szolgáltatássablonként.
3. Kattintson a **Tallózás** tooselect hello mappában, amelynek a végrehajtható fájl, és adja meg a többi hello hello paraméterek toocreate hello szolgáltatást.
   * *Csomag viselkedés Code*. Lehet set toocopy a mappa toohello Visual Studio-projekt tartalmának összes hello Ez akkor hasznos, ha végrehajtható hello nem változik. Ha várhatóan hello végrehajtható toochange és hello képességét toopick mentése új buildek dinamikusan, ehelyett kiválaszthatja toolink toohello mappa. Csatolt mappák hello projektet a Visual Studio létrehozásakor használható. Ez a művelet összeköti toohello forráshely a hello projektben, lehetővé téve, tooupdate hello Vendég végrehajtható a forrás cél. Ezek a frissítések a build hello alkalmazáscsomag részét képezik.
   * *Program* toostart hello szolgáltatás fusson hello végrehajtható határozza meg.
   * *Argumentumok* határozza meg a legyen átadva toohello végrehajtható hello argumentumokat. Azok a argumentumokkal paraméterek listáját.
   * *WorkingFolder* határozza meg, amelyet lépések toobe hello folyamat hello munkakönyvtára. Három értékeket adhat meg:
     * `CodeBase`Meghatározza, hogy hello munkakönyvtár érintetlen toobe toohello kódjának könyvtárában beállítása hello alkalmazáscsomagban (`Code` fájlstruktúra megelőző hello könyvtárral).
     * `CodePackage`Megadja, hogy hello munkakönyvtár érintetlen toobe beállítása toohello legfelső szintű hello alkalmazáscsomag (`GuestService1Pkg` fájlstruktúra megelőző hello látható).
     * `Work`Megadja, hogy a hello fájlok vannak egy munkahelyi nevű alkönyvtárban.
4. Nevezze el a szolgáltatást, és kattintson az **OK** gombra.
5. Ha a szolgáltatás egy olyan végpont-kommunikációra van szüksége, hozzáadhat hello protokoll, port és típus toohello ServiceManifest.xml fájlt. Például: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Mostantól hello csomag és a helyi fürt által hello megoldást a Visual Studio hibakeresési művelet közzétételt. Ha készen áll, hello alkalmazás tooa távoli fürt közzététele, vagy ellenőrizze hello megoldás toosource vezérlőben.
7. Nyissa meg a cikk toosee toohello végét hogyan tooview a Vendég végrehajtható szolgáltatás fut a Service Fabric Explorerben.

## <a name="use-yoeman-toopackage-and-deploy-an-existing-executable-on-linux"></a>Yoeman toopackage használja, és egy létező végrehajtható fájl Linux telepítése

hello eljárás létrehozása és telepítése egy Vendég végrehajtható Linux rendszer hello ugyanaz, mint a csharp vagy java-alkalmazás telepítése.

1. Írja be a terminálba a következőt: `yo azuresfguest`.
2. Adjon nevet az alkalmazásnak.
3. A szolgáltatás, és adja meg hello adatait, beleértve a hello végrehajtható fájl elérési útját és hello paramétereket együtt kell meghívni.

Yeoman hello megfelelő alkalmazást hoz létre egy alkalmazáscsomagot, és együtt fájlok telepítése, és távolítsa el a parancsfájlok.

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a>Manuálisan csomag és egy létező végrehajtható fájl központi telepítése
a Vendég végrehajtható manuálisan csomagolási hello folyamat alábbi általános lépésekkel hello alapul:

1. Hello csomag könyvtárstruktúrát létrehozása.
2. Adja hozzá a hello alkalmazás kódja és konfigurációs fájljait.
3. Hello service manifest-fájl szerkesztése.
4. Hello Alkalmazásjegyzék-fájl szerkesztése.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you toocreate hello ApplicationPackage automatically. hello tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-hello-package-directory-structure"></a>Hello csomag könyvtárstruktúrát létrehozása
Hello előző szakaszban leírtak szerint indítsa el hello könyvtárstruktúrát, létrehozásával "Alkalmazás csomag fájlstruktúra."

### <a name="add-hello-applications-code-and-configuration-files"></a>Hello alkalmazás kódja és konfigurációs fájlok hozzáadása
A létrehozást követően hello könyvtárstruktúrát, hello alkalmazás kódja és konfigurációs fájljait a hello kódot és a konfigurációverziókat címtárak is hozzáadhat. További címtárak vagy hello vagy konfigurációverziót könyvtárak alkönyvtárába is létrehozhat.

A Service Fabric does egy `xcopy` hello tartalmának hello alkalmazás gyökérkönyvtárában, így nincs előre definiált struktúra toouse eltérő két legfontosabb könyvtárak, a kód és a beállítások létrehozása. (Tárolhat különböző neveket, ha azt szeretné. További részletek szerepelnek hello következő szakaszt.)

> [!NOTE]
> Győződjön meg arról, hogy minden hello fájlok és alkalmazások igényeihez hello függőségeit tartalmazza. A Service Fabric átmásolja a terjesztendő hello fürt, ahol hello alkalmazási szolgáltatások folyamatos toobe telepített hello tartalom hello alkalmazáscsomag összes csomópontján. hello csomagnak tartalmaznia kell, hogy hello alkalmazás toorun kell-e az összes hello kód. Feltételezi azt, hogy hello függősége már telepítve van.
>
>

### <a name="edit-hello-service-manifest-file"></a>Hello service manifest-fájl szerkesztése
hello következő lépésre tooedit hello szolgáltatás jegyzékfájl tooinclude hello a következő információkat:

* hello hello szolgáltatás típusának neve. Ez az, hogy a Service Fabric tooidentify szolgáltatás által használt azonosító.
* hello parancs toouse toolaunch hello alkalmazás (ExeHost).
* Bármely, amelyet a toobe parancsfájlt tooset mentése hello alkalmazás (SetupEntrypoint).

hello az alábbiakban látható egy példa egy `ServiceManifest.xml` fájlt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

a következő szakaszok hello hello különböző részeit, hogy kell-e tooupdate hello fájl ismerteti.

#### <a name="update-servicetypes"></a>Frissítés ServiceTypes
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* Kiválaszthatja a nevét, aki a `ServiceTypeName`. hello érték szerepel hello `ApplicationManifest.xml` tooidentify hello szolgáltatás.
* Adja meg `UseImplicitHost="true"`. Ez az attribútum a Service Fabric közli az, hogy hello szolgáltatást egy önálló alkalmazás alapul, így minden Service Fabric kell toodo toolaunch azt folyamatként és állapotának figyelése.

#### <a name="update-codepackage"></a>Frissítés CodePackage
hello CodePackage elem hello helye (és verzió) hello szolgáltatást kód adja meg.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

Hello `Name` elem hello könyvtár hello szolgáltatás kódot tartalmazó hello alkalmazáscsomagban foglalt toospecify hello nevét. `CodePackage`szintén tartalmazza a hello `version` attribútum. Ez lehet hello kód használt toospecify hello verzióját, és potenciálisan is lehet tooupgrade hello szolgáltatást használja, a Service Fabric hello alkalmazás életciklusa felügyeleti infrastruktúra használatával.

#### <a name="optional-update-setupentrypoint"></a>Választható lehetőség: Frissítés SetupEntrypoint
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
hello SetupEntryPoint elem van használt toospecify végrehajtható vagy kötegelt fájlok hello szolgáltatást kód elindítása előtt kell végrehajtani. Egy opcionális lépés, így nem kell a része, ha nincs inicializálás szükséges toobe. hello SetupEntryPoint végrehajtása minden egyes hello szolgáltatás újraindításakor.

Csak egy SetupEntryPoint van így telepítési parancsfájlokat kell toobe csoportosítva egyetlen parancsfájlban, ha hello alkalmazás telepítéséhez több parancsfájlt. hello SetupEntryPoint végrehajtható fájl bármilyen: végrehajtható fájlok, a parancsfájlokat és a PowerShell-parancsmagokkal. További részletekért lásd: [konfigurálása SetupEntryPoint](service-fabric-application-runas-security.md).

A fenti példa hello, hello SetupEntryPoint fut egy kötegfájlt nevű `LaunchConfig.cmd` , amely a található hello `scripts` alkönyvtárába hello kódot (feltéve, hogy hello WorkingFolder elem értéke tooCodeBase).

#### <a name="update-entrypoint"></a>Frissítés belépési pont
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

Hello `EntryPoint` hello szolgáltatás jegyzékfájl eleme használt toospecify hogyan toolaunch hello szolgáltatást. Hello `ExeHost` elem végrehajtható hello (és az argumentumok) meghatározza, hogy toolaunch hello szolgáltatást használja.

* `Program`Megadja a hello hello végrehajtható fájl nevét, kezdő hello szolgáltatást.
* `Arguments`Adja meg a legyen átadva toohello végrehajtható hello argumentumokat. Azok a argumentumokkal paraméterek listáját.
* `WorkingFolder`Adja meg, amelyet lépések toobe hello folyamat hello munkakönyvtára. Három értékeket adhat meg:
  * `CodeBase`Meghatározza, hogy hello munkakönyvtár érintetlen toobe toohello kódjának könyvtárában beállítása hello alkalmazáscsomagban (`Code` könyvtárban lévő fájlstruktúra megelőző hello).
  * `CodePackage`Megadja, hogy hello munkakönyvtár érintetlen toobe beállítása toohello legfelső szintű hello alkalmazáscsomag (`GuestService1Pkg` a fájlstruktúra megelőző hello).
    * `Work`Megadja, hogy a hello fájlok vannak egy munkahelyi nevű alkönyvtárban.

hello WorkingFolder lesz hasznos tooset hello megfelelő munkakönyvtár, így a relatív elérési utak vagy hello inicializálása vagy alkalmazás parancsfájlok által használható.

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a>Végpontok frissítése és a kommunikációs szolgáltatás regisztrálása
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
A fenti példa hello, hello `Endpoint` elem hello alkalmazás figyelheti a hello végpontokhoz határozza meg. Ebben a példában a Node.js-alkalmazás hello port 3000 HTTP-figyeli.

Továbbá megkérheti a Service Fabric toopublish a végpont toohello Naming Service, más szolgáltatásokkal hello végpont címe toothis szolgáltatás képes felderíteni. Ez lehetővé teszi toobe képes toocommunicate Vendég futtatható szolgáltatások között.
hello közzétett végpont címe hello űrlap `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`és `PathSuffix` nem kötelező attribútum. `IPAddressOrFQDN`hello IP-cím vagy a végrehajtható fájl helyezve lekérdezi hello csomópont teljesen minősített tartománynevét, de az Ön számítja ki.

A hello következő példának, egyszer hello szolgáltatás telepítve van a Service Fabric Explorerben úgy, hogy hasonló végpont túl`http://10.1.4.92:3000/myapp/` hello szolgáltatáspéldány közzé. Vagy ha a helyi számítógépen, látható `http://localhost:3000/myapp/`.

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Ezekre a címekre használható [fordított proxy](service-fabric-reverseproxy.md) toocommunicate szolgáltatások között.

### <a name="edit-hello-application-manifest-file"></a>Hello Alkalmazásjegyzék-fájl szerkesztése
Miután konfigurálta a hello `Servicemanifest.xml` fájl, toomake egyes módosítások toohello kell `ApplicationManifest.xml` fájlt, amely hello tooensure helyes-e használni a szolgáltatás típusa és neve.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport
A hello `ServiceManifestImport` elem, megadhatja, hogy szeretné-e tooinclude hello alkalmazásban egy vagy több szolgáltatást. A hivatkozott szolgáltatások `ServiceManifestName`, amely adja meg hello hello könyvtár nevét, ahol hello `ServiceManifest.xml` -fájl.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>A naplózás beállítása
Az Vendég végrehajtható hasznos toobe képes toosee konzol naplók toofind ki, ha alkalmazás- és konfigurációs parancsfájlokat hello hibák megjelenítése.
Hello segítségével konfigurálható `ServiceManifest.xml` hello fájl `ConsoleRedirection` elemet.

> [!WARNING]
> Az alkalmazásban, mert ez hatással lehet a hello alkalmazás feladatátvételi éles környezetben telepített soha ne használja a hello konzol átirányítási házirend. *Csak* használja ezt a helyi fejlesztési és hibakeresési célra.  
>
>

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

`ConsoleRedirection`használt tooredirect konzol (az stdout és az stderr) kimeneti tooa munkakönyvtár lehet. Ez itt hello képességét tooverify, hogy nincsenek-e hibák hello telepítése vagy a Service Fabric-fürt hello hello alkalmazás végrehajtása során.

`FileRetentionCount`azt határozza meg, hány fájl hello munkakönyvtár lesznek mentve. 5-öt, például azt jelenti, hogy a hello előző öt végrehajtások hello naplófájlokban hello munkakönyvtár vannak tárolva.

`FileMaxSizeInKb`hello hello naplófájlok maximális méretét határozza meg.

Naplófájlok mentése hello szolgáltatás használatának könyvtárak egyikében. toodetermine, ahol hello fájlok találhatók, használja a Service Fabric Explorer toodetermine melyik csomópont hello szolgáltatás fut, és melyik munkakönyvtár használatban van. Ez a folyamat a cikk vonatkozik.

## <a name="deployment"></a>Környezet
hello utolsó lépése túl van[az alkalmazás központi telepítése](service-fabric-deploy-remove-applications.md). a következő PowerShell-parancsfájl bemutatja hogyan hello toodeploy a alkalmazás toohello helyi fejlesztési fürtöt, és egy új Service Fabric-szolgáltatás elindításához.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> [Hello csomag tömörítése](service-fabric-package-apps.md#compress-a-package) toohello lemezképtárolóhoz másolás, ha hello csomag nagy, vagy sok fájl van előtt. További [Itt](service-fabric-deploy-remove-applications.md#upload-the-application-package).
>

A Service Fabric-szolgáltatás telepíthető különböző "konfigurációkban." Például, egy vagy több példányát is telepíthető, vagy úgy, hogy nincs-e hello szolgáltatás hello Service Fabric-fürt mindegyik csomópontján több példánya is telepíthető.

Hello `InstanceCount` hello paramétere `New-ServiceFabricService` parancsmag használt toospecify kell indítani a Service Fabric-fürt hello hello szolgáltatás hány példánya. Beállíthatja a hello `InstanceCount` érték, amely elérhetővé tett alkalmazás hello típusától függően. a két hello leggyakoribb forgatókönyvek a következők:

* `InstanceCount = "1"`. Ebben az esetben hello fürt hello szolgáltatás csak egy példánya van telepítve. A Service Fabric-ütemező meghatározza, hogy melyik csomópont hello szolgáltatás toobe telepítve lesz.
* `InstanceCount ="-1"`. Ebben az esetben hello Service Fabric-fürt minden csomópontja hello szolgáltatás egy példánya van telepítve. hello eredmény az egyes csomópontok hello szolgáltatás egy (és csak egy) példánya nehézségekkel hello fürtben.

Ennek oka az előtér-alkalmazás (például egy REST-végpont), egy hasznos konfigurációjának ügyfélalkalmazások kell túl "Csatlakozás" tooany hello fürt toouse hello végpont hello csomópontot. Ez a konfiguráció is használható, ha, például hello Service Fabric-fürt összes csomópontja csatlakoztatott tooa terheléselosztóhoz. Ügyfél forgalmát a Hozzáadás után terjeszthetők hello szolgáltatásban futó hello fürt összes csomópontján.

## <a name="check-your-running-application"></a>Ellenőrizze, fut-alkalmazás
A Service Fabric Explorerben hello szolgáltatást futtató hello csomópont azonosításához. Ebben a példában az fut az 1. csomópont:

![Csomópont, ahol a szolgáltatás fut.](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Ha toohello csomópont keresse meg, és keresse meg az alkalmazás toohello, lásd: hello alapvető csomópont információt, beleértve a helye a lemezen.

![Hely a lemezen](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Toohello directory Server Explorer használatával keresse meg, ha található hello munkakönyvtár és hello szolgáltatás naplómappában, ahogy az alábbi képernyőfelvétel a hello: 

![Napló helye](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan toopackage egy Vendég végrehajtható fájlt, és telepítse azt tooService háló. Tekintse meg a következő cikkeket összegyűjtő kapcsolódó információkat és feladatokat hello.

* [Minta csomagolás és központi telepítése egy Vendég végrehajtható](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), beleértve a hivatkozás toohello előzetes verzióját hello csomagolás eszköz
* [A két Vendég végrehajtható fájlok (C# és nodejs)-en keresztül hello elnevezési szolgáltatás REST minta](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
* [Több futtatható vendégalkalmazás üzembe helyezése](service-fabric-deploy-multiple-apps.md)
* [A Visual Studio használatával első Service Fabric-alkalmazás létrehozása](service-fabric-create-your-first-application-in-visual-studio.md)

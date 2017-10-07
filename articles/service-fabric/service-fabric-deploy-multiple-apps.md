---
title: "a Node.js-alkalmazás által használt MongoDB aaaDeploy |} Microsoft Docs"
description: "Általános bemutató toopackage több Vendég végrehajtható fájlok toodeploy tooan Azure Service Fabric-fürt"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: b76bb756-c1ba-49f9-9666-e9807cf8f92f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: msfussell;mikhegn
ms.openlocfilehash: 2775080f0d9d42d6ba15cca911e23067106be26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-multiple-guest-executables"></a>Több futtatható vendégalkalmazás üzembe helyezése
Ez a cikk bemutatja, hogyan toopackage és több Vendég végrehajtható fájlok tooAzure Service Fabric telepítése. A létrehozása és telepítése egyetlen Service Fabric-csomag hogyan túl olvasási[központi telepítése egy Vendég végrehajtható tooService háló](service-fabric-deploy-existing-app.md).

Amíg ez a bemutató ismerteti, hogyan lehet Node.js előtér hello adattár MongoDB használó alkalmazás toodeploy, alkalmazhat hello lépéseket tooany alkalmazás, amely egy másik alkalmazás függ.   

Használhatja a Visual Studio tooproduce hello alkalmazáscsomagot, amely több, Vendég végrehajtható fájlokat tartalmazza. Lásd: [egy meglévő alkalmazást a Visual Studio használatával toopackage](service-fabric-deploy-existing-app.md). Miután hozzáadta a hello első Vendég végrehajtható fájlt, kattintson a jobb gombbal hello alkalmazási projektet és select hello **Hozzáadás -> új Service Fabric-szolgáltatás** tooadd hello második Vendég végrehajtható projekt toohello megoldás. Megjegyzés: Ha úgy dönt, hogy a toolink hello forrás hello Visual Studio-projekt felépítése hello Visual Studio megoldás, a rendszer ellenőrizze, hogy az alkalmazáscsomag működik-e toodate hello forrás a módosításokkal. 

## <a name="samples"></a>Példák
* [Minta csomagolás és központi telepítése egy Vendég végrehajtható fájl](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [A két Vendég végrehajtható fájlok (C# és nodejs)-en keresztül hello elnevezési szolgáltatás REST minta](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="manually-package-hello-multiple-guest-executable-application"></a>Manuálisan csomag hello több, Vendég futtatható alkalmazás
Másik lehetőségként hello Vendég végrehajtható manuálisan csomagot. Hello manuális csomagolása, az ebben a cikkben az elérhető hello Service Fabric csomagolás eszköz [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).

### <a name="packaging-hello-nodejs-application"></a>Node.js-alkalmazás csomagolás hello
Ez a cikk feltételezi, hogy Node.js hello Service Fabric-fürt hello csomópontján nincs telepítve. Következésképpen tooadd Node.exe toohello a csomópont alkalmazás gyökérkönyvtárában csomagolás előtt van szüksége. hello könyvtárszerkezete hello Node.js-alkalmazás (Express webes keretrendszer és a sablon Jade motorral használatával) alábbihoz hasonló toohello egy alatt:

```
|-- NodeApplication
    |-- bin
        |-- www
    |-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
    |-- public
        |-- images
        |-- etc.
    |-- routes
        |-- index.js
        |-- users.js
    |-- views
        |-- index.jade
        |-- etc.
    |-- app.js
    |-- package.json
    |-- node.exe
```

Következő lépésként hozzon létre egy alkalmazáscsomagot, a Node.js-alkalmazás hello. hello kódot hoz létre a Node.js-alkalmazás hello tartalmazó Service Fabric-alkalmazáscsomagot.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Az alábbiakban az éppen használt hello paraméterek leírását:

* **/ source** pontok toohello directory hello alkalmazás, amely kell csomagolva.
* **/ target** meghatározása hello directory melyik hello csomag kell létrehozni. Ez a könyvtár hello forráskönyvtár eltérő toobe rendelkezik.
* **típustárnevek** hello meglévő alkalmazás hello alkalmazás nevét adja meg. Fontos, hogy ez több hello jegyzékfájl toohello szolgáltatás nevét, és nem toohello Service Fabric-alkalmazás neve toounderstand.
* **/exe** határozza meg, hogy a Service Fabric kellene toolaunch, ebben az esetben végrehajtható hello `node.exe`.
* **/Ma** határozza meg, hogy folyamatban van a használt toolaunch hello végrehajtható hello argumentum. Node.js nincs telepítve, a Service Fabric kell-e a következő futtatásával toolaunch hello Node.js webalkalmazás-kiszolgáló `node.exe bin/www`.  `/ma:'bin/www'`hello csomagolás eszköz toouse közli `bin/ma` node.exe hello argumentumaként.
* **/ Alkalmazástípus** hello Service Fabric-alkalmazás típusnév határozza meg.

Ha tallózással toohello directory hello/TARGET paraméterben megadott, lásd: hello eszköz hozott létre egy teljes mértékben működő Service Fabric-csomag, alább látható módon:

```
|--[yourtargetdirectory]
    |-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
generált ServiceManifest.xml most már rendelkezik egy szakaszt, amely leírja, hogyan hello Node.js web server kell indítani, ahogy az alábbi hello kódrészlet hello:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
Ez a példa hello Node.js webalkalmazás-kiszolgáló figyel a tooport 3000, ezért meg kell tooupdate hello végpont adatokat hello ServiceManifest.xml fájlban alább látható módon.   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-hello-mongodb-application"></a>MongoDB alkalmazás csomagolás hello
Most, hogy a Node.js-alkalmazás hello van csomagolva, lépjen tovább, és MongoDB csomag. Ahogy korábban említettük, hello lépéseket, amelyek most halad át kell nem adott tooNode.js és MongoDB. Valójában tooall alkalmazások, amelyek együtt, egyetlen csomagban egy Service Fabric-alkalmazás toobe alkalmazza őket.  

toopackage MongoDB, azt szeretné, hogy Mongod.exe és Mongo.exe csomag toomake. Mindkét bináris fájljai találhatók hello `bin` mappában található a MongoDB telepítési könyvtára. hello könyvtárstruktúrát egy hasonló toohello alatt keres.

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric kell toostart MongoDB parancs hasonló toohello egyet az alábbi, így toouse hello kell `/ma` paramétert, ha a MongoDB-csomagban.

```
mongod.exe --dbpath [path toodata]
```
> [!NOTE]
> hello adatok nem alatt megőrződik hello esetében egy csomópont leáll, ha hello MongoDB adatkönyvtára hello helyi könyvtárban hello csomópont. Kell vagy tartós tárhelyet használja, vagy egy rendelés tooprevent adatvesztés MongoDB replikakészlet valósítja meg.  
>
>

A PowerShell vagy hello parancs-rendszerhéjban a következő paraméterek hello hello csomagolás eszköz futtassa azt:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path toodata]' /AppType:NodeAppType
```

A sorrend tooadd MongoDB tooyour Service Fabric alkalmazáscsomagot, arról, hogy hello/TARGET paraméter mutat toohello toomake szükség van, amely már tartalmazza az alkalmazásjegyzék hello együtt hello Node.js-alkalmazás könyvtárába. Is kell, hogy használ toomake hello ApplicationType ugyanazzal a névvel.

Most toohello könyvtár tallózása, és vizsgálja meg milyen hello eszköz hozott létre.

```
|--[yourtargetdirectory]
    |-- MyNodeApplication
    |-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
Ahogy látja, hello eszköz hozzá egy új mappát, a MongoDB, toohello tartalmazó könyvtár hello MongoDB bináris fájljait. Ha megnyitja hello `ApplicationManifest.xml` fájl, láthatja, hogy hello csomag most hello Node.js-alkalmazás és a MongoDB is tartalmaz. hello kódot mutatja hello hello az alkalmazás jegyzékének tartalom.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-hello-application"></a>Közzétételi hello alkalmazás
hello utolsó lépése toopublish hello alkalmazás toohello helyi Service Fabric-fürtöt az alábbi hello PowerShell-parancsfájlok használatával:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Ha hello alkalmazás sikeresen közzétett toohello helyi fürthöz, hello Node.js-alkalmazás, amely a Microsoft hello szolgáltatást jegyzékében hello Node.js-alkalmazás – például http://localhost: 3000 megadott hello porton végezheti el.

Ebben az oktatóanyagban láthatta, hogyan tooeasily két meglévő alkalmazások csomagolása, egy Service Fabric-alkalmazás. Is megtanulta, hogyan toodeploy azt tooService úgy, hogy bizonyos hello is kihasználhatja a Service Fabric-szolgáltatások, például magas rendelkezésre állás és a rendszerállapot rendszerintegráció háló.


## <a name="adding-more-guest-executables-tooan-existing-application-using-yeoman-on-linux"></a>További Vendég végrehajtható fájlok tooan meglévő alkalmazás hozzáadása Yeoman használata Linux rendszeren

tooadd egy másik tooan szolgáltatásalkalmazás már létrehozott `yo`, hajtsa végre az alábbi lépésekkel hello: 
1. Hello meglévő alkalmazás gyökérkönyvtárának toohello módosítása.  Például `cd ~/YeomanSamples/MyApplication`, ha `MyApplication` Yeoman által létrehozott hello alkalmazás.
2. Futtatás `yo azuresfguest:AddService` és hello szükséges részleteket biztosítanak.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók a tárolók telepítése [Service Fabric és a tárolók – áttekintés](service-fabric-containers-overview.md)
* [Minta csomagolás és központi telepítése egy Vendég végrehajtható fájl](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [A két Vendég végrehajtható fájlok (C# és nodejs)-en keresztül hello elnevezési szolgáltatás REST minta](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

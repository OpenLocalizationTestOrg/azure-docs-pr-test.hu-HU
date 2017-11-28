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
# <a name="deploy-multiple-guest-executables"></a><span data-ttu-id="03c2c-103">Több futtatható vendégalkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="03c2c-103">Deploy multiple guest executables</span></span>
<span data-ttu-id="03c2c-104">Ez a cikk bemutatja, hogyan toopackage és több Vendég végrehajtható fájlok tooAzure Service Fabric telepítése.</span><span class="sxs-lookup"><span data-stu-id="03c2c-104">This article shows how toopackage and deploy multiple guest executables tooAzure Service Fabric.</span></span> <span data-ttu-id="03c2c-105">A létrehozása és telepítése egyetlen Service Fabric-csomag hogyan túl olvasási[központi telepítése egy Vendég végrehajtható tooService háló](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="03c2c-105">For building and deploying a single Service Fabric package read how too[deploy a guest executable tooService Fabric](service-fabric-deploy-existing-app.md).</span></span>

<span data-ttu-id="03c2c-106">Amíg ez a bemutató ismerteti, hogyan lehet Node.js előtér hello adattár MongoDB használó alkalmazás toodeploy, alkalmazhat hello lépéseket tooany alkalmazás, amely egy másik alkalmazás függ.</span><span class="sxs-lookup"><span data-stu-id="03c2c-106">While this walkthrough shows how toodeploy an application with a Node.js front end that uses MongoDB as hello data store, you can apply hello steps tooany application that has dependencies on another application.</span></span>   

<span data-ttu-id="03c2c-107">Használhatja a Visual Studio tooproduce hello alkalmazáscsomagot, amely több, Vendég végrehajtható fájlokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="03c2c-107">You can use Visual Studio tooproduce hello application package that contains multiple guest executables.</span></span> <span data-ttu-id="03c2c-108">Lásd: [egy meglévő alkalmazást a Visual Studio használatával toopackage](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="03c2c-108">See [Using Visual Studio toopackage an existing application](service-fabric-deploy-existing-app.md).</span></span> <span data-ttu-id="03c2c-109">Miután hozzáadta a hello első Vendég végrehajtható fájlt, kattintson a jobb gombbal hello alkalmazási projektet és select hello **Hozzáadás -> új Service Fabric-szolgáltatás** tooadd hello második Vendég végrehajtható projekt toohello megoldás.</span><span class="sxs-lookup"><span data-stu-id="03c2c-109">After you have added hello first guest executable, right click on hello application project and select hello **Add->New Service Fabric service** tooadd hello second guest executable project toohello solution.</span></span> <span data-ttu-id="03c2c-110">Megjegyzés: Ha úgy dönt, hogy a toolink hello forrás hello Visual Studio-projekt felépítése hello Visual Studio megoldás, a rendszer ellenőrizze, hogy az alkalmazáscsomag működik-e toodate hello forrás a módosításokkal.</span><span class="sxs-lookup"><span data-stu-id="03c2c-110">Note: If you choose toolink hello source in hello Visual Studio project, building hello Visual Studio solution, will make sure that your application package is up toodate with changes in hello source.</span></span> 

## <a name="samples"></a><span data-ttu-id="03c2c-111">Példák</span><span class="sxs-lookup"><span data-stu-id="03c2c-111">Samples</span></span>
* [<span data-ttu-id="03c2c-112">Minta csomagolás és központi telepítése egy Vendég végrehajtható fájl</span><span class="sxs-lookup"><span data-stu-id="03c2c-112">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="03c2c-113">A két Vendég végrehajtható fájlok (C# és nodejs)-en keresztül hello elnevezési szolgáltatás REST minta</span><span class="sxs-lookup"><span data-stu-id="03c2c-113">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="manually-package-hello-multiple-guest-executable-application"></a><span data-ttu-id="03c2c-114">Manuálisan csomag hello több, Vendég futtatható alkalmazás</span><span class="sxs-lookup"><span data-stu-id="03c2c-114">Manually package hello multiple guest executable application</span></span>
<span data-ttu-id="03c2c-115">Másik lehetőségként hello Vendég végrehajtható manuálisan csomagot.</span><span class="sxs-lookup"><span data-stu-id="03c2c-115">Alternatively you can manually package hello guest executable.</span></span> <span data-ttu-id="03c2c-116">Hello manuális csomagolása, az ebben a cikkben az elérhető hello Service Fabric csomagolás eszköz [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).</span><span class="sxs-lookup"><span data-stu-id="03c2c-116">For hello manual packaging, this article uses hello Service Fabric packaging tool, which is available at [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).</span></span>

### <a name="packaging-hello-nodejs-application"></a><span data-ttu-id="03c2c-117">Node.js-alkalmazás csomagolás hello</span><span class="sxs-lookup"><span data-stu-id="03c2c-117">Packaging hello Node.js application</span></span>
<span data-ttu-id="03c2c-118">Ez a cikk feltételezi, hogy Node.js hello Service Fabric-fürt hello csomópontján nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="03c2c-118">This article assumes that Node.js is not installed on hello nodes in hello Service Fabric cluster.</span></span> <span data-ttu-id="03c2c-119">Következésképpen tooadd Node.exe toohello a csomópont alkalmazás gyökérkönyvtárában csomagolás előtt van szüksége.</span><span class="sxs-lookup"><span data-stu-id="03c2c-119">As a consequence, you need tooadd Node.exe toohello root directory of your node application before packaging.</span></span> <span data-ttu-id="03c2c-120">hello könyvtárszerkezete hello Node.js-alkalmazás (Express webes keretrendszer és a sablon Jade motorral használatával) alábbihoz hasonló toohello egy alatt:</span><span class="sxs-lookup"><span data-stu-id="03c2c-120">hello directory structure of hello Node.js application (using Express web framework and Jade template engine) should look similar toohello one below:</span></span>

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

<span data-ttu-id="03c2c-121">Következő lépésként hozzon létre egy alkalmazáscsomagot, a Node.js-alkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="03c2c-121">As a next step, you create an application package for hello Node.js application.</span></span> <span data-ttu-id="03c2c-122">hello kódot hoz létre a Node.js-alkalmazás hello tartalmazó Service Fabric-alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="03c2c-122">hello code below creates a Service Fabric application package that contains hello Node.js application.</span></span>

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

<span data-ttu-id="03c2c-123">Az alábbiakban az éppen használt hello paraméterek leírását:</span><span class="sxs-lookup"><span data-stu-id="03c2c-123">Below is a description of hello parameters that are being used:</span></span>

* <span data-ttu-id="03c2c-124">**/ source** pontok toohello directory hello alkalmazás, amely kell csomagolva.</span><span class="sxs-lookup"><span data-stu-id="03c2c-124">**/source** points toohello directory of hello application that should be packaged.</span></span>
* <span data-ttu-id="03c2c-125">**/ target** meghatározása hello directory melyik hello csomag kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="03c2c-125">**/target** defines hello directory in which hello package should be created.</span></span> <span data-ttu-id="03c2c-126">Ez a könyvtár hello forráskönyvtár eltérő toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="03c2c-126">This directory has toobe different from hello source directory.</span></span>
* <span data-ttu-id="03c2c-127">**típustárnevek** hello meglévő alkalmazás hello alkalmazás nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="03c2c-127">**/appname** defines hello application name of hello existing application.</span></span> <span data-ttu-id="03c2c-128">Fontos, hogy ez több hello jegyzékfájl toohello szolgáltatás nevét, és nem toohello Service Fabric-alkalmazás neve toounderstand.</span><span class="sxs-lookup"><span data-stu-id="03c2c-128">It's important toounderstand that this translates toohello service name in hello manifest, and not toohello Service Fabric application name.</span></span>
* <span data-ttu-id="03c2c-129">**/exe** határozza meg, hogy a Service Fabric kellene toolaunch, ebben az esetben végrehajtható hello `node.exe`.</span><span class="sxs-lookup"><span data-stu-id="03c2c-129">**/exe** defines hello executable that Service Fabric is supposed toolaunch, in this case `node.exe`.</span></span>
* <span data-ttu-id="03c2c-130">**/Ma** határozza meg, hogy folyamatban van a használt toolaunch hello végrehajtható hello argumentum.</span><span class="sxs-lookup"><span data-stu-id="03c2c-130">**/ma** defines hello argument that is being used toolaunch hello executable.</span></span> <span data-ttu-id="03c2c-131">Node.js nincs telepítve, a Service Fabric kell-e a következő futtatásával toolaunch hello Node.js webalkalmazás-kiszolgáló `node.exe bin/www`.</span><span class="sxs-lookup"><span data-stu-id="03c2c-131">As Node.js is not installed, Service Fabric needs toolaunch hello Node.js web server by executing `node.exe bin/www`.</span></span>  <span data-ttu-id="03c2c-132">`/ma:'bin/www'`hello csomagolás eszköz toouse közli `bin/ma` node.exe hello argumentumaként.</span><span class="sxs-lookup"><span data-stu-id="03c2c-132">`/ma:'bin/www'` tells hello packaging tool toouse `bin/ma` as hello argument for node.exe.</span></span>
* <span data-ttu-id="03c2c-133">**/ Alkalmazástípus** hello Service Fabric-alkalmazás típusnév határozza meg.</span><span class="sxs-lookup"><span data-stu-id="03c2c-133">**/AppType** defines hello Service Fabric application type name.</span></span>

<span data-ttu-id="03c2c-134">Ha tallózással toohello directory hello/TARGET paraméterben megadott, lásd: hello eszköz hozott létre egy teljes mértékben működő Service Fabric-csomag, alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="03c2c-134">If you browse toohello directory that was specified in hello /target parameter, you can see that hello tool has created a fully functioning Service Fabric package as shown below:</span></span>

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
<span data-ttu-id="03c2c-135">generált ServiceManifest.xml most már rendelkezik egy szakaszt, amely leírja, hogyan hello Node.js web server kell indítani, ahogy az alábbi hello kódrészlet hello:</span><span class="sxs-lookup"><span data-stu-id="03c2c-135">hello generated ServiceManifest.xml now has a section that describes how hello Node.js web server should be launched, as shown in hello code snippet below:</span></span>

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
<span data-ttu-id="03c2c-136">Ez a példa hello Node.js webalkalmazás-kiszolgáló figyel a tooport 3000, ezért meg kell tooupdate hello végpont adatokat hello ServiceManifest.xml fájlban alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="03c2c-136">In this sample, hello Node.js web server listens tooport 3000, so you need tooupdate hello endpoint information in hello ServiceManifest.xml file as shown below.</span></span>   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-hello-mongodb-application"></a><span data-ttu-id="03c2c-137">MongoDB alkalmazás csomagolás hello</span><span class="sxs-lookup"><span data-stu-id="03c2c-137">Packaging hello MongoDB application</span></span>
<span data-ttu-id="03c2c-138">Most, hogy a Node.js-alkalmazás hello van csomagolva, lépjen tovább, és MongoDB csomag.</span><span class="sxs-lookup"><span data-stu-id="03c2c-138">Now that you have packaged hello Node.js application, you can go ahead and package MongoDB.</span></span> <span data-ttu-id="03c2c-139">Ahogy korábban említettük, hello lépéseket, amelyek most halad át kell nem adott tooNode.js és MongoDB.</span><span class="sxs-lookup"><span data-stu-id="03c2c-139">As mentioned before, hello steps that you go through now are not specific tooNode.js and MongoDB.</span></span> <span data-ttu-id="03c2c-140">Valójában tooall alkalmazások, amelyek együtt, egyetlen csomagban egy Service Fabric-alkalmazás toobe alkalmazza őket.</span><span class="sxs-lookup"><span data-stu-id="03c2c-140">In fact, they apply tooall applications that are meant toobe packaged together as one Service Fabric application.</span></span>  

<span data-ttu-id="03c2c-141">toopackage MongoDB, azt szeretné, hogy Mongod.exe és Mongo.exe csomag toomake.</span><span class="sxs-lookup"><span data-stu-id="03c2c-141">toopackage MongoDB, you want toomake sure you package Mongod.exe and Mongo.exe.</span></span> <span data-ttu-id="03c2c-142">Mindkét bináris fájljai találhatók hello `bin` mappában található a MongoDB telepítési könyvtára.</span><span class="sxs-lookup"><span data-stu-id="03c2c-142">Both binaries are located in hello `bin` directory of your MongoDB installation directory.</span></span> <span data-ttu-id="03c2c-143">hello könyvtárstruktúrát egy hasonló toohello alatt keres.</span><span class="sxs-lookup"><span data-stu-id="03c2c-143">hello directory structure looks similar toohello one below.</span></span>

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
<span data-ttu-id="03c2c-144">Service Fabric kell toostart MongoDB parancs hasonló toohello egyet az alábbi, így toouse hello kell `/ma` paramétert, ha a MongoDB-csomagban.</span><span class="sxs-lookup"><span data-stu-id="03c2c-144">Service Fabric needs toostart MongoDB with a command similar toohello one below, so you need toouse hello `/ma` parameter when packaging MongoDB.</span></span>

```
mongod.exe --dbpath [path toodata]
```
> [!NOTE]
> <span data-ttu-id="03c2c-145">hello adatok nem alatt megőrződik hello esetében egy csomópont leáll, ha hello MongoDB adatkönyvtára hello helyi könyvtárban hello csomópont.</span><span class="sxs-lookup"><span data-stu-id="03c2c-145">hello data is not being preserved in hello case of a node failure if you put hello MongoDB data directory on hello local directory of hello node.</span></span> <span data-ttu-id="03c2c-146">Kell vagy tartós tárhelyet használja, vagy egy rendelés tooprevent adatvesztés MongoDB replikakészlet valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="03c2c-146">You should either use durable storage or implement a MongoDB replica set in order tooprevent data loss.</span></span>  
>
>

<span data-ttu-id="03c2c-147">A PowerShell vagy hello parancs-rendszerhéjban a következő paraméterek hello hello csomagolás eszköz futtassa azt:</span><span class="sxs-lookup"><span data-stu-id="03c2c-147">In PowerShell or hello command shell, we run hello packaging tool with hello following parameters:</span></span>

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path toodata]' /AppType:NodeAppType
```

<span data-ttu-id="03c2c-148">A sorrend tooadd MongoDB tooyour Service Fabric alkalmazáscsomagot, arról, hogy hello/TARGET paraméter mutat toohello toomake szükség van, amely már tartalmazza az alkalmazásjegyzék hello együtt hello Node.js-alkalmazás könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="03c2c-148">In order tooadd MongoDB tooyour Service Fabric application package, you need toomake sure that hello /target parameter points toohello same directory that already contains hello application manifest along with hello Node.js application.</span></span> <span data-ttu-id="03c2c-149">Is kell, hogy használ toomake hello ApplicationType ugyanazzal a névvel.</span><span class="sxs-lookup"><span data-stu-id="03c2c-149">You also need toomake sure that you are using hello same ApplicationType name.</span></span>

<span data-ttu-id="03c2c-150">Most toohello könyvtár tallózása, és vizsgálja meg milyen hello eszköz hozott létre.</span><span class="sxs-lookup"><span data-stu-id="03c2c-150">Let's browse toohello directory and examine what hello tool has created.</span></span>

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
<span data-ttu-id="03c2c-151">Ahogy látja, hello eszköz hozzá egy új mappát, a MongoDB, toohello tartalmazó könyvtár hello MongoDB bináris fájljait.</span><span class="sxs-lookup"><span data-stu-id="03c2c-151">As you can see, hello tool added a new folder, MongoDB, toohello directory that contains hello MongoDB binaries.</span></span> <span data-ttu-id="03c2c-152">Ha megnyitja hello `ApplicationManifest.xml` fájl, láthatja, hogy hello csomag most hello Node.js-alkalmazás és a MongoDB is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="03c2c-152">If you open hello `ApplicationManifest.xml` file, you can see that hello package now contains both hello Node.js application and MongoDB.</span></span> <span data-ttu-id="03c2c-153">hello kódot mutatja hello hello az alkalmazás jegyzékének tartalom.</span><span class="sxs-lookup"><span data-stu-id="03c2c-153">hello code below shows hello content of hello application manifest.</span></span>

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

### <a name="publishing-hello-application"></a><span data-ttu-id="03c2c-154">Közzétételi hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="03c2c-154">Publishing hello application</span></span>
<span data-ttu-id="03c2c-155">hello utolsó lépése toopublish hello alkalmazás toohello helyi Service Fabric-fürtöt az alábbi hello PowerShell-parancsfájlok használatával:</span><span class="sxs-lookup"><span data-stu-id="03c2c-155">hello last step is toopublish hello application toohello local Service Fabric cluster by using hello PowerShell scripts below:</span></span>

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

<span data-ttu-id="03c2c-156">Ha hello alkalmazás sikeresen közzétett toohello helyi fürthöz, hello Node.js-alkalmazás, amely a Microsoft hello szolgáltatást jegyzékében hello Node.js-alkalmazás – például http://localhost: 3000 megadott hello porton végezheti el.</span><span class="sxs-lookup"><span data-stu-id="03c2c-156">Once hello application is successfully published toohello local cluster, you can access hello Node.js application on hello port that we have entered in hello service manifest of hello Node.js application--for example http://localhost:3000.</span></span>

<span data-ttu-id="03c2c-157">Ebben az oktatóanyagban láthatta, hogyan tooeasily két meglévő alkalmazások csomagolása, egy Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="03c2c-157">In this tutorial, you have seen how tooeasily package two existing applications as one Service Fabric application.</span></span> <span data-ttu-id="03c2c-158">Is megtanulta, hogyan toodeploy azt tooService úgy, hogy bizonyos hello is kihasználhatja a Service Fabric-szolgáltatások, például magas rendelkezésre állás és a rendszerállapot rendszerintegráció háló.</span><span class="sxs-lookup"><span data-stu-id="03c2c-158">You have also learned how toodeploy it tooService Fabric so that it can benefit from some of hello Service Fabric features, such as high availability and health system integration.</span></span>


## <a name="adding-more-guest-executables-tooan-existing-application-using-yeoman-on-linux"></a><span data-ttu-id="03c2c-159">További Vendég végrehajtható fájlok tooan meglévő alkalmazás hozzáadása Yeoman használata Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="03c2c-159">Adding more guest executables tooan existing application using Yeoman on Linux</span></span>

<span data-ttu-id="03c2c-160">tooadd egy másik tooan szolgáltatásalkalmazás már létrehozott `yo`, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="03c2c-160">tooadd another service tooan application already created using `yo`, perform hello following steps:</span></span> 
1. <span data-ttu-id="03c2c-161">Hello meglévő alkalmazás gyökérkönyvtárának toohello módosítása.</span><span class="sxs-lookup"><span data-stu-id="03c2c-161">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="03c2c-162">Például `cd ~/YeomanSamples/MyApplication`, ha `MyApplication` Yeoman által létrehozott hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="03c2c-162">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="03c2c-163">Futtatás `yo azuresfguest:AddService` és hello szükséges részleteket biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="03c2c-163">Run `yo azuresfguest:AddService` and provide hello necessary details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="03c2c-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="03c2c-164">Next steps</span></span>
* <span data-ttu-id="03c2c-165">További tudnivalók a tárolók telepítése [Service Fabric és a tárolók – áttekintés](service-fabric-containers-overview.md)</span><span class="sxs-lookup"><span data-stu-id="03c2c-165">Learn about deploying containers with [Service Fabric and containers overview](service-fabric-containers-overview.md)</span></span>
* [<span data-ttu-id="03c2c-166">Minta csomagolás és központi telepítése egy Vendég végrehajtható fájl</span><span class="sxs-lookup"><span data-stu-id="03c2c-166">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="03c2c-167">A két Vendég végrehajtható fájlok (C# és nodejs)-en keresztül hello elnevezési szolgáltatás REST minta</span><span class="sxs-lookup"><span data-stu-id="03c2c-167">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

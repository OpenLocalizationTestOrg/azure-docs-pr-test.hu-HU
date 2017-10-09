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
# <a name="package-an-application"></a><span data-ttu-id="7875f-103">Egy alkalmazás becsomagolása</span><span class="sxs-lookup"><span data-stu-id="7875f-103">Package an application</span></span>
<span data-ttu-id="7875f-104">Ez a cikk ismerteti, hogyan toopackage a Service Fabric-alkalmazás, és készen állnak a használatra.</span><span class="sxs-lookup"><span data-stu-id="7875f-104">This article describes how toopackage a Service Fabric application and make it ready for deployment.</span></span>

## <a name="package-layout"></a><span data-ttu-id="7875f-105">Csomag elrendezés</span><span class="sxs-lookup"><span data-stu-id="7875f-105">Package layout</span></span>
<span data-ttu-id="7875f-106">hello alkalmazásjegyzék, egy vagy több szolgáltatás jegyzékfájlban és egyéb szükséges fájlokat kell beállítani egy adott elrendezésben átesett szolgáltatássablonjaikat a Service Fabric-fürt csomag.</span><span class="sxs-lookup"><span data-stu-id="7875f-106">hello application manifest, one or more service manifests, and other necessary package files must be organized in a specific layout for deployment into a Service Fabric cluster.</span></span> <span data-ttu-id="7875f-107">hello példa jegyzékfájlokat az ebben a cikkben a következő könyvtárstruktúrát hello rendezve toobe lenne szükség:</span><span class="sxs-lookup"><span data-stu-id="7875f-107">hello example manifests in this article would need toobe organized in hello following directory structure:</span></span>

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

<span data-ttu-id="7875f-108">hello elnevezése toomatch hello **neve** minden megfelelő elem attribútumaihoz.</span><span class="sxs-lookup"><span data-stu-id="7875f-108">hello folders are named toomatch hello **Name** attributes of each corresponding element.</span></span> <span data-ttu-id="7875f-109">Például akkor, ha hello szolgáltatás jegyzékfájl tartalmazott két kód csomagok hello nevű **MyCodeA** és **MyCodeB**, majd két azonos nevű tartalmazná hello mappa hello minden egyes szükséges bináris fájlokat a csomag.</span><span class="sxs-lookup"><span data-stu-id="7875f-109">For example, if hello service manifest contained two code packages with hello names **MyCodeA** and **MyCodeB**, then two folders with hello same names would contain hello necessary binaries for each code package.</span></span>

## <a name="use-setupentrypoint"></a><span data-ttu-id="7875f-110">SetupEntryPoint használata</span><span class="sxs-lookup"><span data-stu-id="7875f-110">Use SetupEntryPoint</span></span>
<span data-ttu-id="7875f-111">A jellemző forgatókönyvek **SetupEntryPoint** toorun egy végrehajtható fájl hello szolgáltatás kezdete előtt van szüksége, vagy tooperform emelt szintű jogosultságokkal rendelkező művelet van szüksége van.</span><span class="sxs-lookup"><span data-stu-id="7875f-111">Typical scenarios for using **SetupEntryPoint** are when you need toorun an executable before hello service starts or you need tooperform an operation with elevated privileges.</span></span> <span data-ttu-id="7875f-112">Példa:</span><span class="sxs-lookup"><span data-stu-id="7875f-112">For example:</span></span>

* <span data-ttu-id="7875f-113">Beállítását, valamint inicializálása környezeti változókat, amelyek hello végrehajtható van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7875f-113">Setting up and initializing environment variables that hello service executable needs.</span></span> <span data-ttu-id="7875f-114">Már nem korlátozott tooonly végrehajtható fájlok keresztül hello Service Fabric programozási modellek írása.</span><span class="sxs-lookup"><span data-stu-id="7875f-114">It is not limited tooonly executables written via hello Service Fabric programming models.</span></span> <span data-ttu-id="7875f-115">Például npm.exe kell néhány környezetiblokk-változót, egy node.js-alkalmazás telepítéséhez konfigurált.</span><span class="sxs-lookup"><span data-stu-id="7875f-115">For example, npm.exe needs some environment variables configured for deploying a node.js application.</span></span>
* <span data-ttu-id="7875f-116">Hozzáférés-vezérlés beállítása biztonsági tanúsítványok telepítésével.</span><span class="sxs-lookup"><span data-stu-id="7875f-116">Setting up access control by installing security certificates.</span></span>

<span data-ttu-id="7875f-117">További információt a tooconfigure hello **SetupEntryPoint**, lásd: [egy szolgáltatás telepítője a belépési pont hello házirend konfigurálása](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="7875f-117">For more information on how tooconfigure hello **SetupEntryPoint**, see [Configure hello policy for a service setup entry point](service-fabric-application-runas-security.md)</span></span>

<a id="Package-App"></a>
## <a name="configure"></a><span data-ttu-id="7875f-118">Konfigurálás</span><span class="sxs-lookup"><span data-stu-id="7875f-118">Configure</span></span>
### <a name="build-a-package-by-using-visual-studio"></a><span data-ttu-id="7875f-119">Csomag létrehozása a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="7875f-119">Build a package by using Visual Studio</span></span>
<span data-ttu-id="7875f-120">Ha a Visual Studio 2015 toocreate használhassa az alkalmazást, hello parancs tooautomatically, hozzon létre egy csomagot, amely megfelel a fenti hello elrendezés csomag is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7875f-120">If you use Visual Studio 2015 toocreate your application, you can use hello Package command tooautomatically create a package that matches hello layout described above.</span></span>

<span data-ttu-id="7875f-121">toocreate a csomag, kattintson a jobb gombbal a projektre a Solution Explorer hello és hello csomag parancs, válassza alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="7875f-121">toocreate a package, right-click hello application project in Solution Explorer and choose hello Package command, as shown below:</span></span>

![A Visual Studio alkalmazás csomagolás][vs-package-command]

<span data-ttu-id="7875f-123">Csomagolás befejeződése után az hello hello csomag hello helyen található **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="7875f-123">When packaging is complete, you can find hello location of hello package in hello **Output** window.</span></span> <span data-ttu-id="7875f-124">hello csomagolás lépés esetén automatikusan telepíteni, vagy a Visual Studióban az alkalmazás hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="7875f-124">hello packaging step occurs automatically when you deploy or debug your application in Visual Studio.</span></span>

### <a name="build-a-package-by-command-line"></a><span data-ttu-id="7875f-125">Csomag létrehozása a parancssorból</span><span class="sxs-lookup"><span data-stu-id="7875f-125">Build a package by command line</span></span>
<span data-ttu-id="7875f-126">Az is lehetséges tooprogrammatically csomag fel az alkalmazást a `msbuild.exe`.</span><span class="sxs-lookup"><span data-stu-id="7875f-126">It is also possible tooprogrammatically package up your application using `msbuild.exe`.</span></span> <span data-ttu-id="7875f-127">Alatt hello technikai részletek Visual Studio fut, így hello kimeneti azonos.</span><span class="sxs-lookup"><span data-stu-id="7875f-127">Under hello hood, Visual Studio is running it so hello output is same.</span></span>

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-hello-package"></a><span data-ttu-id="7875f-128">Hello tesztcsomag</span><span class="sxs-lookup"><span data-stu-id="7875f-128">Test hello package</span></span>
<span data-ttu-id="7875f-129">Hello segítségével ellenőrizheti a PowerShell segítségével helyileg hello csomag struktúra [teszt-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) parancsot.</span><span class="sxs-lookup"><span data-stu-id="7875f-129">You can verify hello package structure locally through PowerShell by using hello [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) command.</span></span>
<span data-ttu-id="7875f-130">Ez a parancs problémák elemzése jegyzékfájl keres, és ellenőrizze az összes hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="7875f-130">This command checks for manifest parsing issues and verify all references.</span></span> <span data-ttu-id="7875f-131">A parancs csak hello strukturális hello könyvtárak és fájlok hello csomag helyességét ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="7875f-131">This command only verifies hello structural correctness of hello directories and files in hello package.</span></span>
<span data-ttu-id="7875f-132">Bármelyik hello kóddal vagy az adatok csomag tartalmának ellenőrzése, hogy jelen-e minden szükséges fájlok túl nem ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="7875f-132">It doesn't verify any of hello code or data package contents beyond checking that all necessary files are present.</span></span>

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : hello EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

<span data-ttu-id="7875f-133">Ez a hiba jeleníti meg, hogy hello *MySetup.bat* hello szolgáltatás jegyzékben hivatkozott fájl **SetupEntryPoint** hello kód csomagból hiányzik.</span><span class="sxs-lookup"><span data-stu-id="7875f-133">This error shows that hello *MySetup.bat* file referenced in hello service manifest **SetupEntryPoint** is missing from hello code package.</span></span> <span data-ttu-id="7875f-134">Hello hiányzó fájl hozzáadása után hello alkalmazás ellenőrzési továbbítja:</span><span class="sxs-lookup"><span data-stu-id="7875f-134">After hello missing file is added, hello application verification passes:</span></span>

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

<span data-ttu-id="7875f-135">Ha az alkalmazás [alkalmazás paraméterei](service-fabric-manage-multiple-environment-app-configuration.md) definiált, is adja meg azokat a [teszt-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) megfelelő érvényesítés.</span><span class="sxs-lookup"><span data-stu-id="7875f-135">If your application has [application parameters](service-fabric-manage-multiple-environment-app-configuration.md) defined, you can pass them in [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) for proper validation.</span></span>

<span data-ttu-id="7875f-136">Ha tudja, hova szeretné telepíteni hello alkalmazás hello fürt, akkor ajánlott hello adjon át `ImageStoreConnectionString` paraméter.</span><span class="sxs-lookup"><span data-stu-id="7875f-136">If you know hello cluster where hello application will be deployed, it is recommended you pass in hello `ImageStoreConnectionString` parameter.</span></span> <span data-ttu-id="7875f-137">Ebben az esetben a hello csomag is összevetni hello alkalmazás korábbi verzióiban már fut a hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="7875f-137">In this case, hello package is also validated against previous versions of hello application that are already running in hello cluster.</span></span> <span data-ttu-id="7875f-138">Például hello érvényesítési észleli, hogy egy csomag a hello ugyanazt a verziót, de eltérő tartalomra már telepítve lett.</span><span class="sxs-lookup"><span data-stu-id="7875f-138">For example, hello validation can detect whether a package with hello same version but different content was already deployed.</span></span>  

<span data-ttu-id="7875f-139">Miután hello alkalmazás megfelelően van csomagolva, és ellenőrzése sikeres, értékelje ki van szüksége a tömörítési hello méret és a fájlok hello száma alapján.</span><span class="sxs-lookup"><span data-stu-id="7875f-139">Once hello application is packaged correctly and passes validation, evaluate based on hello size and hello number of files if compression is needed.</span></span>

## <a name="compress-a-package"></a><span data-ttu-id="7875f-140">A csomag tömörítése</span><span class="sxs-lookup"><span data-stu-id="7875f-140">Compress a package</span></span>
<span data-ttu-id="7875f-141">Ha nagy vagy sok fájl van egy csomagot, akkor gyorsabb telepítése tömöríthetők.</span><span class="sxs-lookup"><span data-stu-id="7875f-141">When a package is large or has many files, you can compress it for faster deployment.</span></span> <span data-ttu-id="7875f-142">Tömörítés csökkenti a fájlok és a csomag mérete hello hello számát.</span><span class="sxs-lookup"><span data-stu-id="7875f-142">Compression reduces hello number of files and hello package size.</span></span>
<span data-ttu-id="7875f-143">A tömörített alkalmazáscsomagot [Uploading hello alkalmazáscsomag](service-fabric-deploy-remove-applications.md#upload-the-application-package) eltarthat már összehasonlított toouploading hello kibontott csomagot (ha kifejezetten tömörítési idő Beleszámítja a), de [regisztrálása](service-fabric-deploy-remove-applications.md#register-the-application-package) és [típustár regisztrációjának hello alkalmazástípus](service-fabric-deploy-remove-applications.md#unregister-an-application-type) gyorsabb a tömörített alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="7875f-143">For a compressed application package, [Uploading hello application package](service-fabric-deploy-remove-applications.md#upload-the-application-package) may take longer compared toouploading hello uncompressed package (specially if compression time is factored in), but [registering](service-fabric-deploy-remove-applications.md#register-the-application-package) and [un-registering hello application type](service-fabric-deploy-remove-applications.md#unregister-an-application-type) are faster for a compressed application package.</span></span>

<span data-ttu-id="7875f-144">hello mechanizmussal legyen, a tömörített és tömörítetlen csomagokat.</span><span class="sxs-lookup"><span data-stu-id="7875f-144">hello deployment mechanism is same for compressed and uncompressed packages.</span></span> <span data-ttu-id="7875f-145">Ha hello csomag tömörített, a fürt lemezképtárolóhoz hello ilyen tárolja, és azt hello csomóponton van tömörítetlen hello alkalmazás futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="7875f-145">If hello package is compressed, it is stored as such in hello cluster image store and it's uncompressed on hello node before hello application is run.</span></span>
<span data-ttu-id="7875f-146">hello tömörítési hello érvényes Service Fabric-csomag hello tömörített verziójával váltja fel.</span><span class="sxs-lookup"><span data-stu-id="7875f-146">hello compression replaces hello valid Service Fabric package with hello compressed version.</span></span> <span data-ttu-id="7875f-147">hello mappa engedélyeznie kell írási engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="7875f-147">hello folder must allow write permissions.</span></span> <span data-ttu-id="7875f-148">Nincs változás tömörítési futó egy már tömörített csomagot adja eredményül.</span><span class="sxs-lookup"><span data-stu-id="7875f-148">Running compression on an already compressed package yields no changes.</span></span>

<span data-ttu-id="7875f-149">Egy csomag tömörítheti hello Powershell parancs futtatásával [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) rendelkező `CompressPackage` váltani.</span><span class="sxs-lookup"><span data-stu-id="7875f-149">You can compress a package by running hello Powershell command [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) with `CompressPackage` switch.</span></span> <span data-ttu-id="7875f-150">Hello kibonthatja csomag hello az azonos parancsot `UncompressPackage` váltani.</span><span class="sxs-lookup"><span data-stu-id="7875f-150">You can uncompress hello package with hello same command, using `UncompressPackage` switch.</span></span>

<span data-ttu-id="7875f-151">hello parancsban tömöríti hello csomag nélkül másolás toohello lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="7875f-151">hello following command compresses hello package without copying it toohello image store.</span></span> <span data-ttu-id="7875f-152">A tömörített csomag tooone vagy másolhatja további Service Fabric-fürtök használatával szükség szerint [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) nélkül hello `SkipCopy` jelzőt.</span><span class="sxs-lookup"><span data-stu-id="7875f-152">You can copy a compressed package tooone or more Service Fabric clusters, as needed, using [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) without hello `SkipCopy` flag.</span></span>
<span data-ttu-id="7875f-153">hello csomag most már tartalmaz hello tömörített fájlok `code`, `config`, és `data` csomagok.</span><span class="sxs-lookup"><span data-stu-id="7875f-153">hello package now includes zipped files for hello `code`, `config`, and `data` packages.</span></span> <span data-ttu-id="7875f-154">hello alkalmazásjegyzék és hello szolgáltatás jegyzékfájlokban vannak nem zip, mert szükség van a több belső műveletekhez (például a csomag megosztási, alkalmazás neve és verziója kapcsolattípus kibontása az egyes ellenőrzések).</span><span class="sxs-lookup"><span data-stu-id="7875f-154">hello application manifest and hello service manifests are not zipped, because they are needed for many internal operations (like package sharing, application type name and version extraction for certain validations).</span></span>
<span data-ttu-id="7875f-155">Például .zip fájlba hello jegyzékfájlokban nem elég hatékony teszi ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="7875f-155">Zipping hello manifests would make these operations inefficient.</span></span>

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

<span data-ttu-id="7875f-156">Azt is megteheti, tömöríti, és másolja a hello csomagot [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) egy lépésben.</span><span class="sxs-lookup"><span data-stu-id="7875f-156">Alternatively, you can compress and copy hello package with [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) in one step.</span></span>
<span data-ttu-id="7875f-157">Ha hello csomag túl nagy, adja meg a hello csomag tömörítés és a hello feltöltés toohello fürt elég magas időtúllépési tooallow időpontját.</span><span class="sxs-lookup"><span data-stu-id="7875f-157">If hello package is large, provide a high enough timeout tooallow time for both hello package compression and hello upload toohello cluster.</span></span>
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

<span data-ttu-id="7875f-158">Belső a Service Fabric kiszámítja ellenőrzőösszegeket hello alkalmazáscsomagok érvényesítéshez.</span><span class="sxs-lookup"><span data-stu-id="7875f-158">Internally, Service Fabric computes checksums for hello application packages for validation.</span></span> <span data-ttu-id="7875f-159">Tömörítés használata esetén a hello ellenőrzőösszegeket minden csomag verziója zip hello a kiszámítása a történik.</span><span class="sxs-lookup"><span data-stu-id="7875f-159">When using compression, hello checksums are computed on hello zipped versions of each package.</span></span>
<span data-ttu-id="7875f-160">Ha a másolt egy alkalmazáscsomag tömörítetlen verzióját, és azt szeretné, hogy hello toouse tömörítése ugyanazon csomag módosítania kell a hello hello verziói `code`, `config`, és `data` csomagok tooavoid ellenőrzőösszeg eltérése.</span><span class="sxs-lookup"><span data-stu-id="7875f-160">If you copied an uncompressed version of your application package, and you want toouse compression for hello same package, you must change hello versions of hello `code`, `config`, and `data` packages tooavoid checksum mismatch.</span></span> <span data-ttu-id="7875f-161">Ha hello csomagok nem változnak, hello verzió módosítása helyett, akkor használhatja [különbözeti kiépítés](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="7875f-161">If hello packages are unchanged, instead of changing hello version, you can use [diff provisioning](service-fabric-application-upgrade-advanced.md).</span></span> <span data-ttu-id="7875f-162">Ezzel a beállítással nem tartalmaznak hello változatlan csomag helyette hello szolgáltatás jegyzékfájlból hivatkozni rá.</span><span class="sxs-lookup"><span data-stu-id="7875f-162">With this option, do not include hello unchanged package instead reference it from hello service manifest.</span></span>

<span data-ttu-id="7875f-163">Hasonlóképpen ha szeretné toouse egy tömörítetlen csomag hello csomag tömörített változatának feltöltött, frissítenie kell a hello verziók tooavoid hello nem egyeznek az ellenőrzőösszegek is.</span><span class="sxs-lookup"><span data-stu-id="7875f-163">Similarly, if you uploaded a compressed version of hello package and you want toouse an uncompressed package, you must update hello versions tooavoid hello checksum mismatch.</span></span>

<span data-ttu-id="7875f-164">hello csomag most már megfelelően csomagolt, érvényesítve, és tömörített (ha szükséges), hogy készen álljanak [telepítési](service-fabric-deploy-remove-applications.md) tooone vagy további Service Fabric-fürtök.</span><span class="sxs-lookup"><span data-stu-id="7875f-164">hello package is now packaged correctly, validated, and compressed (if needed), so it is ready for [deployment](service-fabric-deploy-remove-applications.md) tooone or more Service Fabric clusters.</span></span>

### <a name="compress-packages-when-deploying-using-visual-studio"></a><span data-ttu-id="7875f-165">Tömöríti a csomagok központi telepítésekor a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="7875f-165">Compress packages when deploying using Visual Studio</span></span>
<span data-ttu-id="7875f-166">Visual Studio toocompress csomagokat a központi telepítés, utasíthatja hello hozzáadásával `CopyPackageParameters` elem tooyour közzétételi profillal, és állítsa be a hello `CompressPackage` túl attribútum`true`.</span><span class="sxs-lookup"><span data-stu-id="7875f-166">You can instruct Visual Studio toocompress packages on deployment, by adding hello `CopyPackageParameters` element tooyour publish profile, and set hello `CompressPackage` attribute too`true`.</span></span>

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a><span data-ttu-id="7875f-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7875f-167">Next steps</span></span>
<span data-ttu-id="7875f-168">[Központi telepítése és távolíthat el alkalmazásokat] [ 10] ismerteti, hogyan toouse PowerShell toomanage alkalmazáspéldányok</span><span class="sxs-lookup"><span data-stu-id="7875f-168">[Deploy and remove applications][10] describes how toouse PowerShell toomanage application instances</span></span>

<span data-ttu-id="7875f-169">[Alkalmazás paramétereinek több környezet kezelése] [ 11] ismerteti, hogyan tooconfigure paraméterek és a környezeti változók különböző alkalmazás-példányok.</span><span class="sxs-lookup"><span data-stu-id="7875f-169">[Managing application parameters for multiple environments][11] describes how tooconfigure parameters and environment variables for different application instances.</span></span>

<span data-ttu-id="7875f-170">[Az alkalmazás biztonsági szabályzatainak konfigurálásához] [ 12] ismerteti, hogyan toorun a biztonsági házirendek toorestrict hozzáférés szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="7875f-170">[Configure security policies for your application][12] describes how toorun services under security policies toorestrict access.</span></span>

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md

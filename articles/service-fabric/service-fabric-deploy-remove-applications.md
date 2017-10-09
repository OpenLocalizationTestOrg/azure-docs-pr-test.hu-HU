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
# <a name="deploy-and-remove-applications-using-powershell"></a><span data-ttu-id="5f6db-103">Központi telepítése, és távolítsa el az alkalmazásokat a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="5f6db-103">Deploy and remove applications using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5f6db-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5f6db-104">PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
> * [<span data-ttu-id="5f6db-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f6db-105">Visual Studio</span></span>](service-fabric-publish-app-remote-cluster.md)
> * [<span data-ttu-id="5f6db-106">FabricClient API-k</span><span class="sxs-lookup"><span data-stu-id="5f6db-106">FabricClient APIs</span></span>](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

<span data-ttu-id="5f6db-107">Egyszer egy [alkalmazástípus csomagolás][10], az Azure Service Fabric-fürt szolgáltatássablonjaikat használatra kész.</span><span class="sxs-lookup"><span data-stu-id="5f6db-107">Once an [application type has been packaged][10], it's ready for deployment into an Azure Service Fabric cluster.</span></span> <span data-ttu-id="5f6db-108">Központi telepítés hello a következő három lépést foglal magában:</span><span class="sxs-lookup"><span data-stu-id="5f6db-108">Deployment involves hello following three steps:</span></span>

1. <span data-ttu-id="5f6db-109">Hello alkalmazás csomag toohello lemezképtárolóhoz feltöltése</span><span class="sxs-lookup"><span data-stu-id="5f6db-109">Upload hello application package toohello image store</span></span>
2. <span data-ttu-id="5f6db-110">Hello alkalmazástípus regisztrálása</span><span class="sxs-lookup"><span data-stu-id="5f6db-110">Register hello application type</span></span>
3. <span data-ttu-id="5f6db-111">Hello alkalmazáspéldány létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f6db-111">Create hello application instance</span></span>

<span data-ttu-id="5f6db-112">Miután egy alkalmazás lett telepítve, és a példány hello fürtben fut, törölheti a hello alkalmazáspéldány és az alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="5f6db-112">After an application is deployed and an instance is running in hello cluster, you can delete hello application instance and its application type.</span></span> <span data-ttu-id="5f6db-113">toocompletely eltávolítása egy alkalmazáskészletet hello fürtből hello lépéseket foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="5f6db-113">toocompletely remove an application from hello cluster involves hello following steps:</span></span>

1. <span data-ttu-id="5f6db-114">Alkalmazás-példányt futtató hello eltávolítása (vagy törlése)</span><span class="sxs-lookup"><span data-stu-id="5f6db-114">Remove (or delete) hello running application instance</span></span>
2. <span data-ttu-id="5f6db-115">Ha már nincs szüksége a hello alkalmazástípus regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="5f6db-115">Unregister hello application type if you no longer need it</span></span>
3. <span data-ttu-id="5f6db-116">Hello lemezképtárolóhoz hello alkalmazáscsomag eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5f6db-116">Remove hello application package from hello image store</span></span>

<span data-ttu-id="5f6db-117">Ha [központi telepítéséhez és alkalmazások hibakeresése a Visual Studio](service-fabric-publish-app-remote-cluster.md) a helyi fejlesztési fürtöt, a fenti lépéseket minden hello egy PowerShell-parancsfájl segítségével automatikusan kezeli.</span><span class="sxs-lookup"><span data-stu-id="5f6db-117">If you use [Visual Studio for deploying and debugging applications](service-fabric-publish-app-remote-cluster.md) on your local development cluster, all hello preceding steps are handled automatically through a PowerShell script.</span></span>  <span data-ttu-id="5f6db-118">Ez a parancsfájl található hello *parancsfájlok* hello projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="5f6db-118">This script is found in hello *Scripts* folder of hello application project.</span></span> <span data-ttu-id="5f6db-119">Ez a cikk ismerteti a háttérben, hogy a parancsfájl tevékenységétől, hogy végezheti el a Visual Studio kívül ugyanazokat a műveleteket hello.</span><span class="sxs-lookup"><span data-stu-id="5f6db-119">This article provides background on what that script is doing so that you can perform hello same operations outside of Visual Studio.</span></span> 
 
## <a name="connect-toohello-cluster"></a><span data-ttu-id="5f6db-120">Csatlakoztassa toohello fürtöt</span><span class="sxs-lookup"><span data-stu-id="5f6db-120">Connect toohello cluster</span></span>
<span data-ttu-id="5f6db-121">Ebben a cikkben a PowerShell-parancsok futtatása, előtt mindig használatával indítsa el [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) tooconnect toohello Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="5f6db-121">Before you run any PowerShell commands in this article, always start by using [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) tooconnect toohello Service Fabric cluster.</span></span> <span data-ttu-id="5f6db-122">tooconnect toohello helyi fejlesztési fürtöt, futtassa a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5f6db-122">tooconnect toohello local development cluster, run hello following:</span></span>

```powershell
PS C:\>Connect-ServiceFabricCluster
```

<span data-ttu-id="5f6db-123">Példák a kapcsolódó tooa távoli fürt vagy az Azure Active Directoryval, X509 védett fürt tanúsítványokat, vagy a Windows Active Directory [Connect tooa biztonságos fürt](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="5f6db-123">For examples of connecting tooa remote cluster or cluster secured using Azure Active Directory, X509 certificates, or Windows Active Directory see [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

## <a name="upload-hello-application-package"></a><span data-ttu-id="5f6db-124">Hello alkalmazáscsomag feltöltése</span><span class="sxs-lookup"><span data-stu-id="5f6db-124">Upload hello application package</span></span>
<span data-ttu-id="5f6db-125">Feltöltése hello alkalmazáscsomag belső Service Fabric összetevői által elérhető helyen helyezi.</span><span class="sxs-lookup"><span data-stu-id="5f6db-125">Uploading hello application package puts it in a location that's accessible by internal Service Fabric components.</span></span>
<span data-ttu-id="5f6db-126">Ha azt szeretné, hogy tooverify hello alkalmazáscsomagot helyileg, használja a hello [teszt-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5f6db-126">If you want tooverify hello application package locally, use hello [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="5f6db-127">Hello [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) feltöltések hello alkalmazás csomag toohello fürt lemezképtárolóhoz parancsot.</span><span class="sxs-lookup"><span data-stu-id="5f6db-127">hello [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command uploads hello application package toohello cluster image store.</span></span>
<span data-ttu-id="5f6db-128">Hello **Get-ImageStoreConnectionStringFromClusterManifest** hello Service Fabric SDK PowerShell modul részét képező parancsmag értéke használt tooget hello image kapcsolati karakterláncot tárolni.</span><span class="sxs-lookup"><span data-stu-id="5f6db-128">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="5f6db-129">tooimport hello SDK modul, futtassa:</span><span class="sxs-lookup"><span data-stu-id="5f6db-129">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="5f6db-130">Tegyük fel, hogy most felépíti és nevű csomag *MyApplication* a Visual Studio 2015-öt.</span><span class="sxs-lookup"><span data-stu-id="5f6db-130">Suppose you build and package an application named *MyApplication* in Visual Studio 2015.</span></span> <span data-ttu-id="5f6db-131">Alapértelmezés szerint a felsorolt hello ApplicationManifest.xml hello alkalmazástípus neve "MyApplicationType".</span><span class="sxs-lookup"><span data-stu-id="5f6db-131">By default, hello application type name listed in hello ApplicationManifest.xml is "MyApplicationType".</span></span>  <span data-ttu-id="5f6db-132">hello alkalmazáscsomagot, amely tartalmazza a hello szükséges alkalmazás jegyzékfájlja, a szolgáltatás jegyzékfájlban és a kód/config/adatok csomagok, található *C:\Users\<felhasználónév\>\Documents\Visual Studio 2015\Projects\ MyApplication\MyApplication\pkg\Debug*.</span><span class="sxs-lookup"><span data-stu-id="5f6db-132">hello application package, which contains hello necessary application manifest, service manifests, and code/config/data packages, is located in *C:\Users\<username\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*.</span></span> 

<span data-ttu-id="5f6db-133">hello következő parancs kilistázza hello alkalmazáscsomag hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="5f6db-133">hello following command lists hello contents of hello application package:</span></span>

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

<span data-ttu-id="5f6db-134">Ha hello alkalmazáscsomag nagy és/vagy sok fájl van, akkor [a tömörítés](service-fabric-package-apps.md#compress-a-package).</span><span class="sxs-lookup"><span data-stu-id="5f6db-134">If hello application package is large and/or has many files, you can [compress it](service-fabric-package-apps.md#compress-a-package).</span></span> <span data-ttu-id="5f6db-135">hello tömörítés csökkenti a hello mérete és a fájlok hello száma.</span><span class="sxs-lookup"><span data-stu-id="5f6db-135">hello compression reduces hello size and hello number of files.</span></span>
<span data-ttu-id="5f6db-136">hello mellékhatása a regisztrálási és típustár regisztrációjának hello alkalmazástípus gyorsabbak.</span><span class="sxs-lookup"><span data-stu-id="5f6db-136">hello side effect is that registering and un-registering hello application type are faster.</span></span> <span data-ttu-id="5f6db-137">Ideje csökkenhet jelenleg, különösen akkor, ha megadja a hello idő toocompress hello csomag.</span><span class="sxs-lookup"><span data-stu-id="5f6db-137">Upload time may be slower currently, especially if you include hello time toocompress hello package.</span></span> 

<span data-ttu-id="5f6db-138">a csomag, használjon toocompress hello azonos [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) parancsot.</span><span class="sxs-lookup"><span data-stu-id="5f6db-138">toocompress a package, use hello same [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command.</span></span> <span data-ttu-id="5f6db-139">Tömörítés külön között elvégezhető feltöltés hello segítségével `SkipCopy` jelzőt, vagy együtt hello töltse fel a műveletet.</span><span class="sxs-lookup"><span data-stu-id="5f6db-139">Compression can be done separate from upload, by using hello `SkipCopy` flag, or together with hello upload operation.</span></span> <span data-ttu-id="5f6db-140">A tömörített csomag tömörítés alkalmazása műveletvégzés.</span><span class="sxs-lookup"><span data-stu-id="5f6db-140">Applying compression on a compressed package is no-op.</span></span>
<span data-ttu-id="5f6db-141">a tömörített csomag, használjon toouncompress hello azonos [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) hello parancsot `UncompressPackage` váltani.</span><span class="sxs-lookup"><span data-stu-id="5f6db-141">toouncompress a compressed package, use hello same [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command with hello `UncompressPackage` switch.</span></span>

<span data-ttu-id="5f6db-142">hello következő parancsmag tömöríti hello csomag nélkül másolás toohello lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="5f6db-142">hello following cmdlet compresses hello package without copying it toohello image store.</span></span> <span data-ttu-id="5f6db-143">hello csomag most már tartalmaz hello tömörített fájlok `Code` és `Config` csomagok.</span><span class="sxs-lookup"><span data-stu-id="5f6db-143">hello package now includes zipped files for hello `Code` and `Config` packages.</span></span> <span data-ttu-id="5f6db-144">hello alkalmazás és hello szolgáltatás jegyzékfájlokban vannak nem zip, mert szükség van a több belső műveletekhez (például a csomag megosztási, alkalmazás neve és verziója kapcsolattípus kibontása az egyes ellenőrzések).</span><span class="sxs-lookup"><span data-stu-id="5f6db-144">hello application and hello service manifests are not zipped, because they are needed for many internal operations (like package sharing, application type name and version extraction for certain validations).</span></span> <span data-ttu-id="5f6db-145">Például .zip fájlba hello jegyzékfájlokban nem elég hatékony teszi ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="5f6db-145">Zipping hello manifests would make these operations inefficient.</span></span>

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

<span data-ttu-id="5f6db-146">A nagy alkalmazáscsomagok hello tömörítési időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="5f6db-146">For large application packages, hello compression takes time.</span></span> <span data-ttu-id="5f6db-147">A legjobb eredmények elérése érdekében a gyors SSD meghajtó használatát.</span><span class="sxs-lookup"><span data-stu-id="5f6db-147">For best results, use a fast SSD drive.</span></span> <span data-ttu-id="5f6db-148">hello tömörítési idejét és a tömörített csomag hello hello méretének is attól függően változnak, hello csomag tartalmát.</span><span class="sxs-lookup"><span data-stu-id="5f6db-148">hello compression times and hello size of hello compressed package also differ based on hello package content.</span></span>
<span data-ttu-id="5f6db-149">Például ez egyes csomagok, amelyek hello kezdeti megjelenítése és a tömörített csomag mérete, a hello tömörítési idővel hello tömörítési statisztika.</span><span class="sxs-lookup"><span data-stu-id="5f6db-149">For example, here is compression statistics for some packages, which show hello initial and hello compressed package size, with hello compression time.</span></span>

|<span data-ttu-id="5f6db-150">Kezdeti méret (MB)</span><span class="sxs-lookup"><span data-stu-id="5f6db-150">Initial size (MB)</span></span>|<span data-ttu-id="5f6db-151">Fájlok száma</span><span class="sxs-lookup"><span data-stu-id="5f6db-151">File count</span></span>|<span data-ttu-id="5f6db-152">Tömörítés idő</span><span class="sxs-lookup"><span data-stu-id="5f6db-152">Compression Time</span></span>|<span data-ttu-id="5f6db-153">Tömörített mérete (MB)</span><span class="sxs-lookup"><span data-stu-id="5f6db-153">Compressed package size (MB)</span></span>|
|----------------:|---------:|---------------:|---------------------------:|
|<span data-ttu-id="5f6db-154">100</span><span class="sxs-lookup"><span data-stu-id="5f6db-154">100</span></span>|<span data-ttu-id="5f6db-155">100</span><span class="sxs-lookup"><span data-stu-id="5f6db-155">100</span></span>|<span data-ttu-id="5f6db-156">00:00:03.3547592</span><span class="sxs-lookup"><span data-stu-id="5f6db-156">00:00:03.3547592</span></span>|<span data-ttu-id="5f6db-157">60</span><span class="sxs-lookup"><span data-stu-id="5f6db-157">60</span></span>|
|<span data-ttu-id="5f6db-158">512</span><span class="sxs-lookup"><span data-stu-id="5f6db-158">512</span></span>|<span data-ttu-id="5f6db-159">100</span><span class="sxs-lookup"><span data-stu-id="5f6db-159">100</span></span>|<span data-ttu-id="5f6db-160">00:00:16.3850303</span><span class="sxs-lookup"><span data-stu-id="5f6db-160">00:00:16.3850303</span></span>|<span data-ttu-id="5f6db-161">307</span><span class="sxs-lookup"><span data-stu-id="5f6db-161">307</span></span>|
|<span data-ttu-id="5f6db-162">1024</span><span class="sxs-lookup"><span data-stu-id="5f6db-162">1024</span></span>|<span data-ttu-id="5f6db-163">500</span><span class="sxs-lookup"><span data-stu-id="5f6db-163">500</span></span>|<span data-ttu-id="5f6db-164">00:00:32.5907950</span><span class="sxs-lookup"><span data-stu-id="5f6db-164">00:00:32.5907950</span></span>|<span data-ttu-id="5f6db-165">615</span><span class="sxs-lookup"><span data-stu-id="5f6db-165">615</span></span>|
|<span data-ttu-id="5f6db-166">2048</span><span class="sxs-lookup"><span data-stu-id="5f6db-166">2048</span></span>|<span data-ttu-id="5f6db-167">1000</span><span class="sxs-lookup"><span data-stu-id="5f6db-167">1000</span></span>|<span data-ttu-id="5f6db-168">00:01:04.3775554</span><span class="sxs-lookup"><span data-stu-id="5f6db-168">00:01:04.3775554</span></span>|<span data-ttu-id="5f6db-169">1231</span><span class="sxs-lookup"><span data-stu-id="5f6db-169">1231</span></span>|
|<span data-ttu-id="5f6db-170">5012</span><span class="sxs-lookup"><span data-stu-id="5f6db-170">5012</span></span>|<span data-ttu-id="5f6db-171">100</span><span class="sxs-lookup"><span data-stu-id="5f6db-171">100</span></span>|<span data-ttu-id="5f6db-172">00:02:45.2951288</span><span class="sxs-lookup"><span data-stu-id="5f6db-172">00:02:45.2951288</span></span>|<span data-ttu-id="5f6db-173">3074</span><span class="sxs-lookup"><span data-stu-id="5f6db-173">3074</span></span>|

<span data-ttu-id="5f6db-174">Ha egy csomag tömörített, feltöltött tooone lehet, vagy több Service Fabric-fürtök szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="5f6db-174">Once a package is compressed, it can be uploaded tooone or multiple Service Fabric clusters as needed.</span></span> <span data-ttu-id="5f6db-175">hello mechanizmussal legyen, a tömörített és tömörítetlen csomagokat.</span><span class="sxs-lookup"><span data-stu-id="5f6db-175">hello deployment mechanism is same for compressed and uncompressed packages.</span></span> <span data-ttu-id="5f6db-176">Ha hello csomag tömörített, a fürt lemezképtárolóhoz hello ilyen tárolja, és azt hello csomóponton van tömörítetlen hello alkalmazás futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="5f6db-176">If hello package is compressed, it is stored as such in hello cluster image store and it's uncompressed on hello node before hello application is run.</span></span>


<span data-ttu-id="5f6db-177">hello alábbi példa feltölt hello csomag toohello lemezképtárolóhoz "MyApplicationV1" nevű mappába:</span><span class="sxs-lookup"><span data-stu-id="5f6db-177">hello following example uploads hello package toohello image store, into a folder named "MyApplicationV1":</span></span>

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
```

<span data-ttu-id="5f6db-178">Hello **Get-ImageStoreConnectionStringFromClusterManifest** hello Service Fabric SDK PowerShell modul részét képező parancsmag értéke használt tooget hello image kapcsolati karakterláncot tárolni.</span><span class="sxs-lookup"><span data-stu-id="5f6db-178">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="5f6db-179">tooimport hello SDK modul, futtassa:</span><span class="sxs-lookup"><span data-stu-id="5f6db-179">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="5f6db-180">Ha nem adja meg a hello *- ApplicationPackagePathInImageStore* paraméter, hello alkalmazáscsomag hello lemezképtárolóhoz hello "Debug" mappába történő másolása.</span><span class="sxs-lookup"><span data-stu-id="5f6db-180">If you do not specify hello *-ApplicationPackagePathInImageStore* parameter, hello application package is copied into hello "Debug" folder in hello image store.</span></span>

<span data-ttu-id="5f6db-181">hello időt tooupload egy csomag több tényezőktől függően eltér.</span><span class="sxs-lookup"><span data-stu-id="5f6db-181">hello time it takes tooupload a package differs depending on multiple factors.</span></span> <span data-ttu-id="5f6db-182">Ezek a tényezők némelyike hello hello csomag, a hello mérete és a fájlméret hello fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="5f6db-182">Some of these factors are hello number of files in hello package, hello package size, and hello file sizes.</span></span> <span data-ttu-id="5f6db-183">hello hálózati sebesség hello forrásgép és hello Service Fabric-fürt között is hatással van a hello ideje.</span><span class="sxs-lookup"><span data-stu-id="5f6db-183">hello network speed between hello source machine and hello Service Fabric cluster also impacts hello upload time.</span></span> <span data-ttu-id="5f6db-184">az alapértelmezett időkorlát hello [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) 30 perc.</span><span class="sxs-lookup"><span data-stu-id="5f6db-184">hello default timeout for [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) is 30 minutes.</span></span>
<span data-ttu-id="5f6db-185">Attól függően, hello ismertetett tényezők, előfordulhat, hogy tooincrease hello időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="5f6db-185">Depending on hello described factors, you may have tooincrease hello timeout.</span></span> <span data-ttu-id="5f6db-186">Hello csomag hello másolási hívásban gazdameghajtóhoz, ha szüksége van-e tooalso fontolja meg a hello tömörítési idő.</span><span class="sxs-lookup"><span data-stu-id="5f6db-186">If you are compressing hello package in hello copy call, you need tooalso consider hello compression time.</span></span>

<span data-ttu-id="5f6db-187">Lásd: [hello lemezkép tárolási kapcsolati karakterlánc megértéséhez](service-fabric-image-store-connection-string.md) kiegészítő információkat megtudni hello image store és lemezkép tárolásához kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="5f6db-187">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

## <a name="register-hello-application-package"></a><span data-ttu-id="5f6db-188">Hello alkalmazáscsomag regisztrálása</span><span class="sxs-lookup"><span data-stu-id="5f6db-188">Register hello application package</span></span>
<span data-ttu-id="5f6db-189">regisztrálva van-e hello alkalmazáscsomag válnak használható hello alkalmazás jegyzékfájlban deklarált hello alkalmazástípus és -verzió.</span><span class="sxs-lookup"><span data-stu-id="5f6db-189">hello application type and version declared in hello application manifest become available for use when hello application package is registered.</span></span> <span data-ttu-id="5f6db-190">hello rendszer beolvassa az előző lépésben hello feltöltött hello csomag, hello csomag ellenőrzi, hello csomag tartalmának folyamatokat, és másolja át a feldolgozott hello csomagok tooan belső rendszer helyét.</span><span class="sxs-lookup"><span data-stu-id="5f6db-190">hello system reads hello package uploaded in hello previous step, verifies hello package, processes hello package contents, and copies hello processed package tooan internal system location.</span></span>  

<span data-ttu-id="5f6db-191">Futtassa a hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancsmag tooregister hello alkalmazástípus hello fürt, és tegye elérhetővé a központi telepítéshez:</span><span class="sxs-lookup"><span data-stu-id="5f6db-191">Run hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet tooregister hello application type in hello cluster and make it available for deployment:</span></span>

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

<span data-ttu-id="5f6db-192">"MyApplicationV1" az hello mappa, ahol hello alkalmazáscsomag hello kép tárolójában.</span><span class="sxs-lookup"><span data-stu-id="5f6db-192">"MyApplicationV1" is hello folder in hello image store where hello application package is located.</span></span> <span data-ttu-id="5f6db-193">hello alkalmazás típus neve "MyApplicationType" és "1.0.0" verzió (egyaránt megtalálható hello alkalmazásjegyzék) hello fürt most már regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="5f6db-193">hello application type with name "MyApplicationType" and version "1.0.0" (both are found in hello application manifest) is now registered in hello cluster.</span></span>

<span data-ttu-id="5f6db-194">Hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancs csak azt követően hello rendszeren csak a sikeresen regisztrált hello alkalmazáscsomag adja vissza.</span><span class="sxs-lookup"><span data-stu-id="5f6db-194">hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) command returns only after hello system has successfully registered hello application package.</span></span> <span data-ttu-id="5f6db-195">Mennyi ideig regisztrációs vesz hello méretét és hello alkalmazáscsomag tartalmának függ.</span><span class="sxs-lookup"><span data-stu-id="5f6db-195">How long registration takes depends on hello size and contents of hello application package.</span></span> <span data-ttu-id="5f6db-196">Szükség esetén – hello **- TimeoutSec** paraméter lehet használt toosupply egy hosszabb időtúllépési (hello alapértelmezett időtúllépési érték 60 másodperc).</span><span class="sxs-lookup"><span data-stu-id="5f6db-196">If needed, hello **-TimeoutSec** parameter can be used toosupply a longer timeout (hello default timeout is 60 seconds).</span></span>

<span data-ttu-id="5f6db-197">Ha egy csomagot, vagy ha időtúllépéseket tapasztalnak, használjon hello nagy alkalmazás **- aszinkron** paraméter.</span><span class="sxs-lookup"><span data-stu-id="5f6db-197">If you have a large application package or if you are experiencing timeouts, use hello **-Async** parameter.</span></span> <span data-ttu-id="5f6db-198">hello parancs hello fürt elfogadja hello register parancsot, és hello feldolgozási továbbra is fennáll, igény szerint adja vissza.</span><span class="sxs-lookup"><span data-stu-id="5f6db-198">hello command returns when hello cluster accepts hello register command, and hello processing continues as needed.</span></span>
<span data-ttu-id="5f6db-199">Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancs megjeleníti az összes sikeresen regisztrált alkalmazástípus-verziók és a regisztrációs állapotot.</span><span class="sxs-lookup"><span data-stu-id="5f6db-199">hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="5f6db-200">Ez a parancs toodetermine is használhatja, amikor hello regisztrációs hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="5f6db-200">You can use this command toodetermine when hello registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="create-hello-application"></a><span data-ttu-id="5f6db-201">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f6db-201">Create hello application</span></span>
<span data-ttu-id="5f6db-202">Az alkalmazás minden alkalmazástípus verziója hello segítségével sikeresen regisztrált a példányosítható [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5f6db-202">You can instantiate an application from any application type version that has been registered successfully by using hello [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="5f6db-203">minden alkalmazás hello neve kezdődhet hello *"fabric:"* sémáját, és minden alkalmazáspéldányhoz egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5f6db-203">hello name of each application must start with hello *"fabric:"* scheme and must be unique for each application instance.</span></span> <span data-ttu-id="5f6db-204">Hello célalkalmazás típusa hello alkalmazás jegyzékében definiált alapértelmezett szolgáltatások is jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="5f6db-204">Any default services defined in hello application manifest of hello target application type are also created.</span></span>

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
<span data-ttu-id="5f6db-205">Több alkalmazáspéldányt is létrehozható egy regisztrált alkalmazástípus bármely adott verzióját.</span><span class="sxs-lookup"><span data-stu-id="5f6db-205">Multiple application instances can be created for any given version of a registered application type.</span></span> <span data-ttu-id="5f6db-206">Minden egyes alkalmazáspéldány feladata elkülönítve, a saját munkahelyi directory és a folyamat.</span><span class="sxs-lookup"><span data-stu-id="5f6db-206">Each application instance runs in isolation, with its own work directory and process.</span></span>

<span data-ttu-id="5f6db-207">toosee, amely az alkalmazások és szolgáltatások nevű futtató hello fürthöz, futtassa a hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) és [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="5f6db-207">toosee which named apps and services are running in hello cluster, run hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) and [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlets:</span></span>

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

## <a name="remove-an-application"></a><span data-ttu-id="5f6db-208">Alkalmazások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5f6db-208">Remove an application</span></span>
<span data-ttu-id="5f6db-209">Az alkalmazáspéldány nincs szükség, ha véglegesen eltávolíthatja azt hello használatával név szerint [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5f6db-209">When an application instance is no longer needed, you can permanently remove it by name using hello [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="5f6db-210">[Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automatikusan eltávolítja az is, végleges eltávolítása az összes szolgáltatás állapotának toohello alkalmazáshoz tartozó összes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5f6db-210">[Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automatically removes all services that belong toohello application as well, permanently removing all service state.</span></span> 

> [!WARNING]
> <span data-ttu-id="5f6db-211">Ez a művelet nem vonható vissza, és az alkalmazás állapota nem állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="5f6db-211">This operation cannot be reversed, and application state cannot be recovered.</span></span>

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a><span data-ttu-id="5f6db-212">Az alkalmazástípus regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="5f6db-212">Unregister an application type</span></span>
<span data-ttu-id="5f6db-213">Egy adott verzióját egy alkalmazás típus már nem szükséges, amikor meg kell alkalmazástípus regisztrációjának törlése hello hello segítségével [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5f6db-213">When a particular version of an application type is no longer needed, you should unregister hello application type using hello [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="5f6db-214">Regisztrációjának megszüntetése nem használt alkalmazás típusok kiadásokban tárolóhely hello lemezképtárolóhoz használják.</span><span class="sxs-lookup"><span data-stu-id="5f6db-214">Unregistering unused application types releases storage space used by hello image store.</span></span> <span data-ttu-id="5f6db-215">Az alkalmazástípus regisztrációjának törlése lehet, mindaddig, amíg nincs alkalmazások létrehozásának rajta, és nem alkalmazás függőben lévő frissítések hivatkozik rá.</span><span class="sxs-lookup"><span data-stu-id="5f6db-215">An application type can be unregistered as long as no applications are instantiated against it and no pending application upgrades are referencing it.</span></span>

<span data-ttu-id="5f6db-216">Futtatás [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) toosee hello alkalmazástípusok jelenleg regisztrált hello fürt:</span><span class="sxs-lookup"><span data-stu-id="5f6db-216">Run [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) toosee hello application types currently registered in hello cluster:</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

<span data-ttu-id="5f6db-217">Futtatás [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) toounregister egy adott alkalmazás típusa:</span><span class="sxs-lookup"><span data-stu-id="5f6db-217">Run [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) toounregister a specific application type:</span></span>

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="remove-an-application-package-from-hello-image-store"></a><span data-ttu-id="5f6db-218">Az alkalmazáscsomag eltávolítása hello lemezképtárolóhoz</span><span class="sxs-lookup"><span data-stu-id="5f6db-218">Remove an application package from hello image store</span></span>
<span data-ttu-id="5f6db-219">Ha már nincs szükség egy alkalmazáscsomagot, hello lemezképet tároló toofree rendszer erőforrásait a törlése.</span><span class="sxs-lookup"><span data-stu-id="5f6db-219">When an application package is no longer needed, you can delete it from hello image store toofree up system resources.</span></span>

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

## <a name="troubleshooting"></a><span data-ttu-id="5f6db-220">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="5f6db-220">Troubleshooting</span></span>
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a><span data-ttu-id="5f6db-221">Másolás-ServiceFabricApplicationPackage kér egy előtaggal</span><span class="sxs-lookup"><span data-stu-id="5f6db-221">Copy-ServiceFabricApplicationPackage asks for an ImageStoreConnectionString</span></span>
<span data-ttu-id="5f6db-222">hello Service Fabric SDK környezet már rendelkezik hello megfelelő alapértelmezett beállítása.</span><span class="sxs-lookup"><span data-stu-id="5f6db-222">hello Service Fabric SDK environment should already have hello correct defaults set up.</span></span> <span data-ttu-id="5f6db-223">De szükség esetén – hello előtaggal összes parancs meg kell felelnie hello értéket, hogy a Service Fabric fürt által használt hello.</span><span class="sxs-lookup"><span data-stu-id="5f6db-223">But if needed, hello ImageStoreConnectionString for all commands should match hello value that hello Service Fabric cluster is using.</span></span> <span data-ttu-id="5f6db-224">Hello előtaggal hello fürtjegyzékben található beolvasott hello segítségével [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) és Get-ImageStoreConnectionStringFromClusterManifest parancsokat:</span><span class="sxs-lookup"><span data-stu-id="5f6db-224">You can find hello ImageStoreConnectionString in hello cluster manifest, retrieved using hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) and Get-ImageStoreConnectionStringFromClusterManifest commands:</span></span>

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

<span data-ttu-id="5f6db-225">Hello **Get-ImageStoreConnectionStringFromClusterManifest** hello Service Fabric SDK PowerShell modul részét képező parancsmag értéke használt tooget hello image kapcsolati karakterláncot tárolni.</span><span class="sxs-lookup"><span data-stu-id="5f6db-225">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="5f6db-226">tooimport hello SDK modul, futtassa:</span><span class="sxs-lookup"><span data-stu-id="5f6db-226">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="5f6db-227">hello előtaggal megtalálható a fürtjegyzékben hello:</span><span class="sxs-lookup"><span data-stu-id="5f6db-227">hello ImageStoreConnectionString is found in hello cluster manifest:</span></span>

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

<span data-ttu-id="5f6db-228">Lásd: [hello lemezkép tárolási kapcsolati karakterlánc megértéséhez](service-fabric-image-store-connection-string.md) kiegészítő információkat megtudni hello image store és lemezkép tárolásához kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="5f6db-228">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

### <a name="deploy-large-application-package"></a><span data-ttu-id="5f6db-229">Nagy alkalmazáscsomag telepítése</span><span class="sxs-lookup"><span data-stu-id="5f6db-229">Deploy large application package</span></span>
<span data-ttu-id="5f6db-230">Probléma: [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) túllépi az időkorlátot a nagy alkalmazáscsomagok (sorrendet, GB).</span><span class="sxs-lookup"><span data-stu-id="5f6db-230">Issue: [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) times out for a large application package (order of GB).</span></span>
<span data-ttu-id="5f6db-231">Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="5f6db-231">Try:</span></span>
- <span data-ttu-id="5f6db-232">Adjon meg egy nagyobb időkorlátjának [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) parancsot, `TimeoutSec` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5f6db-232">Specify a larger timeout for [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command, with `TimeoutSec` parameter.</span></span> <span data-ttu-id="5f6db-233">Alapértelmezés szerint hello időtúllépés értéke 30 perc.</span><span class="sxs-lookup"><span data-stu-id="5f6db-233">By default, hello timeout is 30 minutes.</span></span>
- <span data-ttu-id="5f6db-234">Ellenőrizze a hello hálózati kapcsolat a forrásgép és fürt között.</span><span class="sxs-lookup"><span data-stu-id="5f6db-234">Check hello network connection between your source machine and cluster.</span></span> <span data-ttu-id="5f6db-235">Ha hello kapcsolat lassú, érdemes lehet a gépek nagyobb hálózati kapcsolattal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="5f6db-235">If hello connection is slow, consider using a machine with a better network connection.</span></span>
<span data-ttu-id="5f6db-236">Ha hello ügyfélszámítógép mint hello fürt egy másik régióban van, érdemes lehet egy ügyfélszámítógépre egy szorosabb vagy ugyanabban a régióban hello fürtként.</span><span class="sxs-lookup"><span data-stu-id="5f6db-236">If hello client machine is in another region than hello cluster, consider using a client machine in a closer or same region as hello cluster.</span></span>
- <span data-ttu-id="5f6db-237">Ellenőrizze, hogy ha hogy elérte-e külső szabályozás.</span><span class="sxs-lookup"><span data-stu-id="5f6db-237">Check if you are hitting external throttling.</span></span> <span data-ttu-id="5f6db-238">Például ha hello lemezképtárolóhoz konfigurált toouse azure tárolási, feltöltés előfordulhat, hogy szabályozva.</span><span class="sxs-lookup"><span data-stu-id="5f6db-238">For example, when hello image store is configured toouse azure storage, upload may be throttled.</span></span>

<span data-ttu-id="5f6db-239">Probléma: Feltöltés csomag sikeresen befejeződött, de [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) túllépi az időkorlátot. Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="5f6db-239">Issue: Upload package completed successfully, but [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) times out. Try:</span></span>
- <span data-ttu-id="5f6db-240">[Hello csomag tömörítése](service-fabric-package-apps.md#compress-a-package) toohello lemezképtárolóhoz másolása előtt.</span><span class="sxs-lookup"><span data-stu-id="5f6db-240">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span>
<span data-ttu-id="5f6db-241">hello tömörítés csökkenti hello és hello fájlok száma, ami viszont hello forgalom mennyiségét csökkenti, és működnek, hogy a Service Fabric kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="5f6db-241">hello compression reduces hello size and hello number of files, which in turn reduces hello amount of traffic and work that Service Fabric must perform.</span></span> <span data-ttu-id="5f6db-242">lehet, hogy hello feltöltési művelet lassabb (különösen ha hello tömörítési idő), de regisztrálása és regisztrációjának hello alkalmazástípus gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="5f6db-242">hello upload operation may be slower (especially if you include hello compression time), but register and un-register hello application type are faster.</span></span>
- <span data-ttu-id="5f6db-243">Adjon meg egy nagyobb időkorlátjának [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) a `TimeoutSec` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5f6db-243">Specify a larger timeout for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) with `TimeoutSec` parameter.</span></span>
- <span data-ttu-id="5f6db-244">Adja meg `Async` átkapcsolni a [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="5f6db-244">Specify `Async` switch for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span></span> <span data-ttu-id="5f6db-245">hello parancs ad vissza, ha hello fürt elfogadja hello parancsot, és hello alkalmazástípus regisztrációjának hello aszinkron módon folytatódik.</span><span class="sxs-lookup"><span data-stu-id="5f6db-245">hello command returns when hello cluster accepts hello command and hello registration of hello application type continues asynchronously.</span></span> <span data-ttu-id="5f6db-246">Emiatt nincs nincs szükség toospecify magasabb időkorlát ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="5f6db-246">For this reason, there is no need toospecify a higher timeout in this case.</span></span> <span data-ttu-id="5f6db-247">Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancs megjeleníti az összes sikeresen regisztrált alkalmazástípus-verziók és a regisztrációs állapotot.</span><span class="sxs-lookup"><span data-stu-id="5f6db-247">hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="5f6db-248">Ez a parancs toodetermine is használhatja, amikor hello regisztrációs hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="5f6db-248">You can use this command toodetermine when hello registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a><span data-ttu-id="5f6db-249">Sok fájlokkal alkalmazáscsomag telepítése</span><span class="sxs-lookup"><span data-stu-id="5f6db-249">Deploy application package with many files</span></span>
<span data-ttu-id="5f6db-250">Probléma: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) sok fájlt (akár több ezer sorrendben) az alkalmazás csomagot időtúllépése.</span><span class="sxs-lookup"><span data-stu-id="5f6db-250">Issue: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) times out for an application package with many files (order of thousands).</span></span>
<span data-ttu-id="5f6db-251">Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="5f6db-251">Try:</span></span>
- <span data-ttu-id="5f6db-252">[Hello csomag tömörítése](service-fabric-package-apps.md#compress-a-package) toohello lemezképtárolóhoz másolása előtt.</span><span class="sxs-lookup"><span data-stu-id="5f6db-252">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span> <span data-ttu-id="5f6db-253">hello tömörítés csökkenti a fájlok hello száma.</span><span class="sxs-lookup"><span data-stu-id="5f6db-253">hello compression reduces hello number of files.</span></span>
- <span data-ttu-id="5f6db-254">Adjon meg egy nagyobb időkorlátjának [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) a `TimeoutSec` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5f6db-254">Specify a larger timeout for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) with `TimeoutSec` parameter.</span></span>
- <span data-ttu-id="5f6db-255">Adja meg `Async` átkapcsolni a [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="5f6db-255">Specify `Async` switch for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span></span> <span data-ttu-id="5f6db-256">hello parancs ad vissza, ha hello fürt elfogadja hello parancsot, és hello alkalmazástípus regisztrációjának hello aszinkron módon folytatódik.</span><span class="sxs-lookup"><span data-stu-id="5f6db-256">hello command returns when hello cluster accepts hello command and hello registration of hello application type continues asynchronously.</span></span>
<span data-ttu-id="5f6db-257">Emiatt nincs nincs szükség toospecify magasabb időkorlát ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="5f6db-257">For this reason, there is no need toospecify a higher timeout in this case.</span></span> <span data-ttu-id="5f6db-258">Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancs megjeleníti az összes sikeresen regisztrált alkalmazástípus-verziók és a regisztrációs állapotot.</span><span class="sxs-lookup"><span data-stu-id="5f6db-258">hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="5f6db-259">Ez a parancs toodetermine is használhatja, amikor hello regisztrációs hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="5f6db-259">You can use this command toodetermine when hello registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a><span data-ttu-id="5f6db-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5f6db-260">Next steps</span></span>
[<span data-ttu-id="5f6db-261">A Service Fabric-alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="5f6db-261">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="5f6db-262">A Service Fabric állapotának bemutatása</span><span class="sxs-lookup"><span data-stu-id="5f6db-262">Service Fabric health introduction</span></span>](service-fabric-health-introduction.md)

[<span data-ttu-id="5f6db-263">Diagnosztizálása és megoldása a Service Fabric-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5f6db-263">Diagnose and troubleshoot a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="5f6db-264">A Service Fabric alkalmazás minta</span><span class="sxs-lookup"><span data-stu-id="5f6db-264">Model an application in Service Fabric</span></span>](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

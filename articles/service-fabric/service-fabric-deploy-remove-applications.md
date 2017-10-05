---
title: "Az Azure Service Fabric-alkalmazás központi telepítése |} Microsoft Docs"
description: "Hogyan telepítheti és távolíthat el alkalmazásokat a Service Fabric PowerShell-lel."
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
ms.openlocfilehash: edef23a8cdab7fd0bef54456f0caabb9db273bf9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-remove-applications-using-powershell"></a><span data-ttu-id="e49e9-103">Központi telepítése, és távolítsa el az alkalmazásokat a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="e49e9-103">Deploy and remove applications using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e49e9-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e49e9-104">PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
> * [<span data-ttu-id="e49e9-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e49e9-105">Visual Studio</span></span>](service-fabric-publish-app-remote-cluster.md)
> * [<span data-ttu-id="e49e9-106">FabricClient API-k</span><span class="sxs-lookup"><span data-stu-id="e49e9-106">FabricClient APIs</span></span>](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

<span data-ttu-id="e49e9-107">Egyszer egy [alkalmazástípus csomagolás][10], az Azure Service Fabric-fürt szolgáltatássablonjaikat használatra kész.</span><span class="sxs-lookup"><span data-stu-id="e49e9-107">Once an [application type has been packaged][10], it's ready for deployment into an Azure Service Fabric cluster.</span></span> <span data-ttu-id="e49e9-108">Központi telepítés a következő három lépést foglal magában:</span><span class="sxs-lookup"><span data-stu-id="e49e9-108">Deployment involves the following three steps:</span></span>

1. <span data-ttu-id="e49e9-109">Az image store az alkalmazáscsomag feltöltése</span><span class="sxs-lookup"><span data-stu-id="e49e9-109">Upload the application package to the image store</span></span>
2. <span data-ttu-id="e49e9-110">Az alkalmazástípus regisztrálása</span><span class="sxs-lookup"><span data-stu-id="e49e9-110">Register the application type</span></span>
3. <span data-ttu-id="e49e9-111">Az alkalmazáspéldány létrehozása</span><span class="sxs-lookup"><span data-stu-id="e49e9-111">Create the application instance</span></span>

<span data-ttu-id="e49e9-112">Miután egy alkalmazás lett telepítve, és egy példányát a fürtben fut, törölheti az alkalmazáspéldány és az alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="e49e9-112">After an application is deployed and an instance is running in the cluster, you can delete the application instance and its application type.</span></span> <span data-ttu-id="e49e9-113">Az alkalmazás teljes eltávolítása a fürt a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="e49e9-113">To completely remove an application from the cluster involves the following steps:</span></span>

1. <span data-ttu-id="e49e9-114">Távolítsa el (vagy törlése) futó alkalmazáspéldány</span><span class="sxs-lookup"><span data-stu-id="e49e9-114">Remove (or delete) the running application instance</span></span>
2. <span data-ttu-id="e49e9-115">Az alkalmazástípus regisztrációjának törlése, ha már nincs szüksége</span><span class="sxs-lookup"><span data-stu-id="e49e9-115">Unregister the application type if you no longer need it</span></span>
3. <span data-ttu-id="e49e9-116">Az alkalmazáscsomag eltávolítása a lemezképtárolóból</span><span class="sxs-lookup"><span data-stu-id="e49e9-116">Remove the application package from the image store</span></span>

<span data-ttu-id="e49e9-117">Ha [központi telepítéséhez és alkalmazások hibakeresése a Visual Studio](service-fabric-publish-app-remote-cluster.md) meg a helyi fejlesztési fürtöt, az előző lépések kezeli automatikusan egy PowerShell-parancsfájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="e49e9-117">If you use [Visual Studio for deploying and debugging applications](service-fabric-publish-app-remote-cluster.md) on your local development cluster, all the preceding steps are handled automatically through a PowerShell script.</span></span>  <span data-ttu-id="e49e9-118">Ez a parancsfájl megtalálható a *parancsfájlok* mappában található a projektet.</span><span class="sxs-lookup"><span data-stu-id="e49e9-118">This script is found in the *Scripts* folder of the application project.</span></span> <span data-ttu-id="e49e9-119">Ez a cikk nyújt háttér milyen, hogy a parancsfájl módon, hogy a Visual Studio kívül ugyanazokat a műveleteket végezheti el.</span><span class="sxs-lookup"><span data-stu-id="e49e9-119">This article provides background on what that script is doing so that you can perform the same operations outside of Visual Studio.</span></span> 
 
## <a name="connect-to-the-cluster"></a><span data-ttu-id="e49e9-120">Csatlakozás a fürthöz</span><span class="sxs-lookup"><span data-stu-id="e49e9-120">Connect to the cluster</span></span>
<span data-ttu-id="e49e9-121">Ebben a cikkben a PowerShell-parancsok futtatása, előtt mindig használatával indítsa el [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) csatlakozni a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="e49e9-121">Before you run any PowerShell commands in this article, always start by using [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) to connect to the Service Fabric cluster.</span></span> <span data-ttu-id="e49e9-122">Szeretne csatlakozni a helyi fejlesztési fürtöt, futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="e49e9-122">To connect to the local development cluster, run the following:</span></span>

```powershell
PS C:\>Connect-ServiceFabricCluster
```

<span data-ttu-id="e49e9-123">Csatlakozás távoli fürt vagy az Azure Active Directoryval, X509 védett fürt példákat tanúsítványokat, vagy a Windows Active Directory [Csatlakozás biztonságos fürthöz](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="e49e9-123">For examples of connecting to a remote cluster or cluster secured using Azure Active Directory, X509 certificates, or Windows Active Directory see [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

## <a name="upload-the-application-package"></a><span data-ttu-id="e49e9-124">Az alkalmazáscsomag feltöltése</span><span class="sxs-lookup"><span data-stu-id="e49e9-124">Upload the application package</span></span>
<span data-ttu-id="e49e9-125">Az alkalmazáscsomag feltöltése helyezi belső Service Fabric összetevői által elérhető helyen.</span><span class="sxs-lookup"><span data-stu-id="e49e9-125">Uploading the application package puts it in a location that's accessible by internal Service Fabric components.</span></span>
<span data-ttu-id="e49e9-126">Ha szeretné ellenőrizni a alkalmazáscsomagot helyileg, használja a [teszt-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e49e9-126">If you want to verify the application package locally, use the [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="e49e9-127">A [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) parancs feltölti az alkalmazáscsomagot a fürt lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="e49e9-127">The [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command uploads the application package to the cluster image store.</span></span>
<span data-ttu-id="e49e9-128">A **Get-ImageStoreConnectionStringFromClusterManifest** parancsmagot, amely a Service Fabric SDK PowerShell modulja részét képezi, a lemezkép tárolási kapcsolati karakterlánc beolvasása szolgál.</span><span class="sxs-lookup"><span data-stu-id="e49e9-128">The **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of the Service Fabric SDK PowerShell module, is used to get the image store connection string.</span></span>  <span data-ttu-id="e49e9-129">Az SDK modul importálásához futtassa:</span><span class="sxs-lookup"><span data-stu-id="e49e9-129">To import the SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="e49e9-130">Tegyük fel, hogy most felépíti és nevű csomag *MyApplication* a Visual Studio 2015-öt.</span><span class="sxs-lookup"><span data-stu-id="e49e9-130">Suppose you build and package an application named *MyApplication* in Visual Studio 2015.</span></span> <span data-ttu-id="e49e9-131">Alapértelmezés szerint az alkalmazás szerepel a ApplicationManifest.xml típusneve "MyApplicationType".</span><span class="sxs-lookup"><span data-stu-id="e49e9-131">By default, the application type name listed in the ApplicationManifest.xml is "MyApplicationType".</span></span>  <span data-ttu-id="e49e9-132">Az alkalmazáscsomag, amely tartalmazza a szükséges alkalmazás jegyzékfájlja, a szolgáltatás jegyzékfájlban és a kód/config/adatok csomagok, található *C:\Users\<felhasználónév\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*.</span><span class="sxs-lookup"><span data-stu-id="e49e9-132">The application package, which contains the necessary application manifest, service manifests, and code/config/data packages, is located in *C:\Users\<username\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*.</span></span> 

<span data-ttu-id="e49e9-133">A következő parancs megjeleníti az alkalmazáscsomag tartalmának:</span><span class="sxs-lookup"><span data-stu-id="e49e9-133">The following command lists the contents of the application package:</span></span>

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

<span data-ttu-id="e49e9-134">Ha az alkalmazáscsomag nagy és/vagy sok fájl van, akkor [a tömörítés](service-fabric-package-apps.md#compress-a-package).</span><span class="sxs-lookup"><span data-stu-id="e49e9-134">If the application package is large and/or has many files, you can [compress it](service-fabric-package-apps.md#compress-a-package).</span></span> <span data-ttu-id="e49e9-135">A tömörítés csökkenti a méretét és a fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="e49e9-135">The compression reduces the size and the number of files.</span></span>
<span data-ttu-id="e49e9-136">A mellékhatása, amely regisztrálásakor és típustár regisztrációjának megszüntetése az alkalmazástípus gyorsabbak.</span><span class="sxs-lookup"><span data-stu-id="e49e9-136">The side effect is that registering and un-registering the application type are faster.</span></span> <span data-ttu-id="e49e9-137">Ideje csökkenhet jelenleg, különösen akkor, ha adja meg az időt a csomag tömörítése.</span><span class="sxs-lookup"><span data-stu-id="e49e9-137">Upload time may be slower currently, especially if you include the time to compress the package.</span></span> 

<span data-ttu-id="e49e9-138">Csomag tömörítése, használja a azonos [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e49e9-138">To compress a package, use the same [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command.</span></span> <span data-ttu-id="e49e9-139">Tömörítés hajtható végre külön feltöltés, a használatával a `SkipCopy` jelzőt, vagy a feltöltési művelet együtt.</span><span class="sxs-lookup"><span data-stu-id="e49e9-139">Compression can be done separate from upload, by using the `SkipCopy` flag, or together with the upload operation.</span></span> <span data-ttu-id="e49e9-140">A tömörített csomag tömörítés alkalmazása műveletvégzés.</span><span class="sxs-lookup"><span data-stu-id="e49e9-140">Applying compression on a compressed package is no-op.</span></span>
<span data-ttu-id="e49e9-141">Bontsa ki a tömörített csomag, használja a azonos [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) parancsot a `UncompressPackage` váltani.</span><span class="sxs-lookup"><span data-stu-id="e49e9-141">To uncompress a compressed package, use the same [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command with the `UncompressPackage` switch.</span></span>

<span data-ttu-id="e49e9-142">A következő parancsmagot a csomag nélkül másolja az image store tömöríti.</span><span class="sxs-lookup"><span data-stu-id="e49e9-142">The following cmdlet compresses the package without copying it to the image store.</span></span> <span data-ttu-id="e49e9-143">A csomag most már tartalmazza a fájlok kibontását a `Code` és `Config` csomagok.</span><span class="sxs-lookup"><span data-stu-id="e49e9-143">The package now includes zipped files for the `Code` and `Config` packages.</span></span> <span data-ttu-id="e49e9-144">Az alkalmazás és a szolgáltatás jegyzékfájlokat vannak nem zip, mert szükség van a sok belső műveletekhez (például a csomag megosztási, alkalmazás neve és verziója kapcsolattípus kibontása az egyes ellenőrzések).</span><span class="sxs-lookup"><span data-stu-id="e49e9-144">The application and the service manifests are not zipped, because they are needed for many internal operations (like package sharing, application type name and version extraction for certain validations).</span></span> <span data-ttu-id="e49e9-145">A jegyzékfájlokban tömörítés teszi ezeket a műveleteket nem hatékony.</span><span class="sxs-lookup"><span data-stu-id="e49e9-145">Zipping the manifests would make these operations inefficient.</span></span>

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

<span data-ttu-id="e49e9-146">A tömörítés nagy alkalmazáscsomagok esetén időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="e49e9-146">For large application packages, the compression takes time.</span></span> <span data-ttu-id="e49e9-147">A legjobb eredmények elérése érdekében a gyors SSD meghajtó használatát.</span><span class="sxs-lookup"><span data-stu-id="e49e9-147">For best results, use a fast SSD drive.</span></span> <span data-ttu-id="e49e9-148">A tömörítési ideje és a tömörített csomag is attól függően változnak, a csomag tartalmát.</span><span class="sxs-lookup"><span data-stu-id="e49e9-148">The compression times and the size of the compressed package also differ based on the package content.</span></span>
<span data-ttu-id="e49e9-149">Például ez tömörítési statisztika egyes csomagok, amely a kezdeti és a tömörített csomag mérete, a tömörítési időponttal megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="e49e9-149">For example, here is compression statistics for some packages, which show the initial and the compressed package size, with the compression time.</span></span>

|<span data-ttu-id="e49e9-150">Kezdeti méret (MB)</span><span class="sxs-lookup"><span data-stu-id="e49e9-150">Initial size (MB)</span></span>|<span data-ttu-id="e49e9-151">Fájlok száma</span><span class="sxs-lookup"><span data-stu-id="e49e9-151">File count</span></span>|<span data-ttu-id="e49e9-152">Tömörítés idő</span><span class="sxs-lookup"><span data-stu-id="e49e9-152">Compression Time</span></span>|<span data-ttu-id="e49e9-153">Tömörített mérete (MB)</span><span class="sxs-lookup"><span data-stu-id="e49e9-153">Compressed package size (MB)</span></span>|
|----------------:|---------:|---------------:|---------------------------:|
|<span data-ttu-id="e49e9-154">100</span><span class="sxs-lookup"><span data-stu-id="e49e9-154">100</span></span>|<span data-ttu-id="e49e9-155">100</span><span class="sxs-lookup"><span data-stu-id="e49e9-155">100</span></span>|<span data-ttu-id="e49e9-156">00:00:03.3547592</span><span class="sxs-lookup"><span data-stu-id="e49e9-156">00:00:03.3547592</span></span>|<span data-ttu-id="e49e9-157">60</span><span class="sxs-lookup"><span data-stu-id="e49e9-157">60</span></span>|
|<span data-ttu-id="e49e9-158">512</span><span class="sxs-lookup"><span data-stu-id="e49e9-158">512</span></span>|<span data-ttu-id="e49e9-159">100</span><span class="sxs-lookup"><span data-stu-id="e49e9-159">100</span></span>|<span data-ttu-id="e49e9-160">00:00:16.3850303</span><span class="sxs-lookup"><span data-stu-id="e49e9-160">00:00:16.3850303</span></span>|<span data-ttu-id="e49e9-161">307</span><span class="sxs-lookup"><span data-stu-id="e49e9-161">307</span></span>|
|<span data-ttu-id="e49e9-162">1024</span><span class="sxs-lookup"><span data-stu-id="e49e9-162">1024</span></span>|<span data-ttu-id="e49e9-163">500</span><span class="sxs-lookup"><span data-stu-id="e49e9-163">500</span></span>|<span data-ttu-id="e49e9-164">00:00:32.5907950</span><span class="sxs-lookup"><span data-stu-id="e49e9-164">00:00:32.5907950</span></span>|<span data-ttu-id="e49e9-165">615</span><span class="sxs-lookup"><span data-stu-id="e49e9-165">615</span></span>|
|<span data-ttu-id="e49e9-166">2048</span><span class="sxs-lookup"><span data-stu-id="e49e9-166">2048</span></span>|<span data-ttu-id="e49e9-167">1000</span><span class="sxs-lookup"><span data-stu-id="e49e9-167">1000</span></span>|<span data-ttu-id="e49e9-168">00:01:04.3775554</span><span class="sxs-lookup"><span data-stu-id="e49e9-168">00:01:04.3775554</span></span>|<span data-ttu-id="e49e9-169">1231</span><span class="sxs-lookup"><span data-stu-id="e49e9-169">1231</span></span>|
|<span data-ttu-id="e49e9-170">5012</span><span class="sxs-lookup"><span data-stu-id="e49e9-170">5012</span></span>|<span data-ttu-id="e49e9-171">100</span><span class="sxs-lookup"><span data-stu-id="e49e9-171">100</span></span>|<span data-ttu-id="e49e9-172">00:02:45.2951288</span><span class="sxs-lookup"><span data-stu-id="e49e9-172">00:02:45.2951288</span></span>|<span data-ttu-id="e49e9-173">3074</span><span class="sxs-lookup"><span data-stu-id="e49e9-173">3074</span></span>|

<span data-ttu-id="e49e9-174">Ha egy csomag tömörített, akkor is feltölthetők a egy vagy több Service Fabric-fürtök igény szerint.</span><span class="sxs-lookup"><span data-stu-id="e49e9-174">Once a package is compressed, it can be uploaded to one or multiple Service Fabric clusters as needed.</span></span> <span data-ttu-id="e49e9-175">A központi telepítési módszer használata ugyanabban a tömörített és tömörítetlen csomagokat.</span><span class="sxs-lookup"><span data-stu-id="e49e9-175">The deployment mechanism is same for compressed and uncompressed packages.</span></span> <span data-ttu-id="e49e9-176">Ha a csomag tömörített, a fürt lemezképtárolóhoz ilyen van tárolva, és azt van tömörítetlen a csomóponton, az alkalmazás futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="e49e9-176">If the package is compressed, it is stored as such in the cluster image store and it's uncompressed on the node before the application is run.</span></span>


<span data-ttu-id="e49e9-177">Az alábbi példa feltölti a csomag az image store "MyApplicationV1" nevű mappába:</span><span class="sxs-lookup"><span data-stu-id="e49e9-177">The following example uploads the package to the image store, into a folder named "MyApplicationV1":</span></span>

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
```

<span data-ttu-id="e49e9-178">A **Get-ImageStoreConnectionStringFromClusterManifest** parancsmagot, amely a Service Fabric SDK PowerShell modulja részét képezi, a lemezkép tárolási kapcsolati karakterlánc beolvasása szolgál.</span><span class="sxs-lookup"><span data-stu-id="e49e9-178">The **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of the Service Fabric SDK PowerShell module, is used to get the image store connection string.</span></span>  <span data-ttu-id="e49e9-179">Az SDK modul importálásához futtassa:</span><span class="sxs-lookup"><span data-stu-id="e49e9-179">To import the SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="e49e9-180">Ha nem adja meg a *- ApplicationPackagePathInImageStore* paraméter, a alkalmazáscsomag másolódik a az image store "Debug" mappájába.</span><span class="sxs-lookup"><span data-stu-id="e49e9-180">If you do not specify the *-ApplicationPackagePathInImageStore* parameter, the application package is copied into the "Debug" folder in the image store.</span></span>

<span data-ttu-id="e49e9-181">A csomag feltöltéséhez szükséges eltér attól függően, hogy több tényezővel.</span><span class="sxs-lookup"><span data-stu-id="e49e9-181">The time it takes to upload a package differs depending on multiple factors.</span></span> <span data-ttu-id="e49e9-182">Ezek a tényezők a csomag, a csomag mérete, és a fájl a fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="e49e9-182">Some of these factors are the number of files in the package, the package size, and the file sizes.</span></span> <span data-ttu-id="e49e9-183">A hálózat sebességétől, a forráskiszolgáló és a Service Fabric-fürt között is hatással van a ideje.</span><span class="sxs-lookup"><span data-stu-id="e49e9-183">The network speed between the source machine and the Service Fabric cluster also impacts the upload time.</span></span> <span data-ttu-id="e49e9-184">Az alapértelmezett időkorlátjának [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) 30 perc.</span><span class="sxs-lookup"><span data-stu-id="e49e9-184">The default timeout for [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) is 30 minutes.</span></span>
<span data-ttu-id="e49e9-185">Az ismertetett tényezőktől függően előfordulhat, hogy az időtúllépési érték növeléséhez.</span><span class="sxs-lookup"><span data-stu-id="e49e9-185">Depending on the described factors, you may have to increase the timeout.</span></span> <span data-ttu-id="e49e9-186">Ha a csomag másolása hívásában gazdameghajtóhoz, fontolja meg a tömörítési idő is szeretné.</span><span class="sxs-lookup"><span data-stu-id="e49e9-186">If you are compressing the package in the copy call, you need to also consider the compression time.</span></span>

<span data-ttu-id="e49e9-187">Lásd: [megérteni a lemezkép tárolási kapcsolati karakterlánc](service-fabric-image-store-connection-string.md) az image store és a lemezkép kiegészítő információt tárolja a kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="e49e9-187">See [Understand the image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about the image store and image store connection string.</span></span>

## <a name="register-the-application-package"></a><span data-ttu-id="e49e9-188">Az alkalmazáscsomag regisztrálása</span><span class="sxs-lookup"><span data-stu-id="e49e9-188">Register the application package</span></span>
<span data-ttu-id="e49e9-189">Az alkalmazástípus és -verzió az alkalmazás jegyzékében használható válnak, amikor regisztrál az alkalmazáscsomag deklarálva.</span><span class="sxs-lookup"><span data-stu-id="e49e9-189">The application type and version declared in the application manifest become available for use when the application package is registered.</span></span> <span data-ttu-id="e49e9-190">A rendszer beolvassa az előző lépésben feltöltött csomag, ellenőrzi a csomag, feldolgozza a csomag tartalmát, és a feldolgozott csomag belső rendszer helyre másolja.</span><span class="sxs-lookup"><span data-stu-id="e49e9-190">The system reads the package uploaded in the previous step, verifies the package, processes the package contents, and copies the processed package to an internal system location.</span></span>  

<span data-ttu-id="e49e9-191">Futtassa a [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancsmag az alkalmazástípus regisztrálása a fürtben, és lehetővé teszi az üzembe:</span><span class="sxs-lookup"><span data-stu-id="e49e9-191">Run the [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet to register the application type in the cluster and make it available for deployment:</span></span>

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

<span data-ttu-id="e49e9-192">"MyApplicationV1" az image store, ahol az alkalmazáscsomag-e a mappát.</span><span class="sxs-lookup"><span data-stu-id="e49e9-192">"MyApplicationV1" is the folder in the image store where the application package is located.</span></span> <span data-ttu-id="e49e9-193">Az alkalmazástípus neve "MyApplicationType" és "1.0.0" (egyaránt megtalálható az alkalmazás jegyzékében) verziója most már regisztrálva van a fürtben.</span><span class="sxs-lookup"><span data-stu-id="e49e9-193">The application type with name "MyApplicationType" and version "1.0.0" (both are found in the application manifest) is now registered in the cluster.</span></span>

<span data-ttu-id="e49e9-194">A [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) parancs csak azt követően a rendszer sikeresen regisztrálta az alkalmazáscsomag adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e49e9-194">The [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) command returns only after the system has successfully registered the application package.</span></span> <span data-ttu-id="e49e9-195">Mennyi ideig regisztrációs vesz méretét és az alkalmazáscsomag tartalmának függ.</span><span class="sxs-lookup"><span data-stu-id="e49e9-195">How long registration takes depends on the size and contents of the application package.</span></span> <span data-ttu-id="e49e9-196">Ha szükséges, a **- TimeoutSec** paraméter segítségével adjon meg egy hosszabb időtúllépési érték (az alapértelmezett időtúllépési érték 60 másodperc).</span><span class="sxs-lookup"><span data-stu-id="e49e9-196">If needed, the **-TimeoutSec** parameter can be used to supply a longer timeout (the default timeout is 60 seconds).</span></span>

<span data-ttu-id="e49e9-197">Ha egy nagy alkalmazás csomagot, vagy ha időtúllépéseket tapasztalnak, használja a **- aszinkron** paraméter.</span><span class="sxs-lookup"><span data-stu-id="e49e9-197">If you have a large application package or if you are experiencing timeouts, use the **-Async** parameter.</span></span> <span data-ttu-id="e49e9-198">A parancs ad vissza, ha a fürt elfogadja a register parancsot, és a feldolgozási továbbra is fennáll, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="e49e9-198">The command returns when the cluster accepts the register command, and the processing continues as needed.</span></span>
<span data-ttu-id="e49e9-199">A [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancs megjeleníti az összes sikeresen regisztrált alkalmazástípus-verziók és a regisztrációs állapotot.</span><span class="sxs-lookup"><span data-stu-id="e49e9-199">The [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="e49e9-200">Ez a parancs segítségével határozható meg, hogy a regisztrációs történik.</span><span class="sxs-lookup"><span data-stu-id="e49e9-200">You can use this command to determine when the registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="create-the-application"></a><span data-ttu-id="e49e9-201">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e49e9-201">Create the application</span></span>
<span data-ttu-id="e49e9-202">Az alkalmazás minden alkalmazástípus verziója segítségével sikeresen regisztrált a példányosítható a [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e49e9-202">You can instantiate an application from any application type version that has been registered successfully by using the [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="e49e9-203">Minden alkalmazás nevére kell kezdődnie az *"fabric:"* sémáját, és minden alkalmazáspéldányhoz egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e49e9-203">The name of each application must start with the *"fabric:"* scheme and must be unique for each application instance.</span></span> <span data-ttu-id="e49e9-204">A célalkalmazás típusa alkalmazás jegyzékében definiált alapértelmezett szolgáltatások is jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="e49e9-204">Any default services defined in the application manifest of the target application type are also created.</span></span>

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
<span data-ttu-id="e49e9-205">Több alkalmazáspéldányt is létrehozható egy regisztrált alkalmazástípus bármely adott verzióját.</span><span class="sxs-lookup"><span data-stu-id="e49e9-205">Multiple application instances can be created for any given version of a registered application type.</span></span> <span data-ttu-id="e49e9-206">Minden egyes alkalmazáspéldány feladata elkülönítve, a saját munkahelyi directory és a folyamat.</span><span class="sxs-lookup"><span data-stu-id="e49e9-206">Each application instance runs in isolation, with its own work directory and process.</span></span>

<span data-ttu-id="e49e9-207">Meg amely az alkalmazások és szolgáltatások futnak a fürthöz, futtassa a [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) és [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="e49e9-207">To see which named apps and services are running in the cluster, run the [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) and [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlets:</span></span>

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

## <a name="remove-an-application"></a><span data-ttu-id="e49e9-208">Alkalmazások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="e49e9-208">Remove an application</span></span>
<span data-ttu-id="e49e9-209">Az alkalmazáspéldány nincs szükség, ha véglegesen eltávolíthatja azt név használatával a [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e49e9-209">When an application instance is no longer needed, you can permanently remove it by name using the [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="e49e9-210">[Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automatikusan eltávolítja az is, végleges eltávolítása az összes szolgáltatás állapota az alkalmazáshoz tartozó összes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e49e9-210">[Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automatically removes all services that belong to the application as well, permanently removing all service state.</span></span> 

> [!WARNING]
> <span data-ttu-id="e49e9-211">Ez a művelet nem vonható vissza, és az alkalmazás állapota nem állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="e49e9-211">This operation cannot be reversed, and application state cannot be recovered.</span></span>

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a><span data-ttu-id="e49e9-212">Az alkalmazástípus regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="e49e9-212">Unregister an application type</span></span>
<span data-ttu-id="e49e9-213">Amikor egy alkalmazás típus adott verziójának már nem szükséges, meg kell regisztrációját használt alkalmazások típusa a [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e49e9-213">When a particular version of an application type is no longer needed, you should unregister the application type using the [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="e49e9-214">Az image store által felhasznált tárterület nem használt alkalmazástípus regisztrációjának kiadását.</span><span class="sxs-lookup"><span data-stu-id="e49e9-214">Unregistering unused application types releases storage space used by the image store.</span></span> <span data-ttu-id="e49e9-215">Az alkalmazástípus regisztrációjának törlése lehet, mindaddig, amíg nincs alkalmazások létrehozásának rajta, és nem alkalmazás függőben lévő frissítések hivatkozik rá.</span><span class="sxs-lookup"><span data-stu-id="e49e9-215">An application type can be unregistered as long as no applications are instantiated against it and no pending application upgrades are referencing it.</span></span>

<span data-ttu-id="e49e9-216">Futtatás [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) a fürt jelenleg regisztrált alkalmazás típusának:</span><span class="sxs-lookup"><span data-stu-id="e49e9-216">Run [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) to see the application types currently registered in the cluster:</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

<span data-ttu-id="e49e9-217">Futtatás [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) egy adott alkalmazás típusa beállításjegyzékből való törlése:</span><span class="sxs-lookup"><span data-stu-id="e49e9-217">Run [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) to unregister a specific application type:</span></span>

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="remove-an-application-package-from-the-image-store"></a><span data-ttu-id="e49e9-218">Az alkalmazáscsomag eltávolítása a lemezképtárolóból</span><span class="sxs-lookup"><span data-stu-id="e49e9-218">Remove an application package from the image store</span></span>
<span data-ttu-id="e49e9-219">Ha már nincs szükség egy alkalmazáscsomagot, törölheti a lemezképtárolóból rendszererőforrások felszabadítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="e49e9-219">When an application package is no longer needed, you can delete it from the image store to free up system resources.</span></span>

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

## <a name="troubleshooting"></a><span data-ttu-id="e49e9-220">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="e49e9-220">Troubleshooting</span></span>
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a><span data-ttu-id="e49e9-221">Másolás-ServiceFabricApplicationPackage kér egy előtaggal</span><span class="sxs-lookup"><span data-stu-id="e49e9-221">Copy-ServiceFabricApplicationPackage asks for an ImageStoreConnectionString</span></span>
<span data-ttu-id="e49e9-222">A Service Fabric SDK környezet már rendelkezik a megfelelő alapértelmezett beállítása.</span><span class="sxs-lookup"><span data-stu-id="e49e9-222">The Service Fabric SDK environment should already have the correct defaults set up.</span></span> <span data-ttu-id="e49e9-223">De ha szükséges, az összes parancs előtaggal meg kell felelnie a Service Fabric fürt által használt érték.</span><span class="sxs-lookup"><span data-stu-id="e49e9-223">But if needed, the ImageStoreConnectionString for all commands should match the value that the Service Fabric cluster is using.</span></span> <span data-ttu-id="e49e9-224">A előtaggal megtalálható a fürtjegyzékben le kérni a [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) és Get-ImageStoreConnectionStringFromClusterManifest parancsokat:</span><span class="sxs-lookup"><span data-stu-id="e49e9-224">You can find the ImageStoreConnectionString in the cluster manifest, retrieved using the [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) and Get-ImageStoreConnectionStringFromClusterManifest commands:</span></span>

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

<span data-ttu-id="e49e9-225">A **Get-ImageStoreConnectionStringFromClusterManifest** parancsmagot, amely a Service Fabric SDK PowerShell modulja részét képezi, a lemezkép tárolási kapcsolati karakterlánc beolvasása szolgál.</span><span class="sxs-lookup"><span data-stu-id="e49e9-225">The **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of the Service Fabric SDK PowerShell module, is used to get the image store connection string.</span></span>  <span data-ttu-id="e49e9-226">Az SDK modul importálásához futtassa:</span><span class="sxs-lookup"><span data-stu-id="e49e9-226">To import the SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

<span data-ttu-id="e49e9-227">A előtaggal megtalálható a fürtjegyzékben:</span><span class="sxs-lookup"><span data-stu-id="e49e9-227">The ImageStoreConnectionString is found in the cluster manifest:</span></span>

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

<span data-ttu-id="e49e9-228">Lásd: [megérteni a lemezkép tárolási kapcsolati karakterlánc](service-fabric-image-store-connection-string.md) az image store és a lemezkép kiegészítő információt tárolja a kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="e49e9-228">See [Understand the image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about the image store and image store connection string.</span></span>

### <a name="deploy-large-application-package"></a><span data-ttu-id="e49e9-229">Nagy alkalmazáscsomag telepítése</span><span class="sxs-lookup"><span data-stu-id="e49e9-229">Deploy large application package</span></span>
<span data-ttu-id="e49e9-230">Probléma: [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) túllépi az időkorlátot a nagy alkalmazáscsomagok (sorrendet, GB).</span><span class="sxs-lookup"><span data-stu-id="e49e9-230">Issue: [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) times out for a large application package (order of GB).</span></span>
<span data-ttu-id="e49e9-231">Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="e49e9-231">Try:</span></span>
- <span data-ttu-id="e49e9-232">Adjon meg egy nagyobb időkorlátjának [másolási-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) parancsot, `TimeoutSec` paraméter.</span><span class="sxs-lookup"><span data-stu-id="e49e9-232">Specify a larger timeout for [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) command, with `TimeoutSec` parameter.</span></span> <span data-ttu-id="e49e9-233">Alapértelmezés szerint az időtúllépés értéke 30 perc.</span><span class="sxs-lookup"><span data-stu-id="e49e9-233">By default, the timeout is 30 minutes.</span></span>
- <span data-ttu-id="e49e9-234">Ellenőrizze a hálózati kapcsolat a forrásgép és fürt között.</span><span class="sxs-lookup"><span data-stu-id="e49e9-234">Check the network connection between your source machine and cluster.</span></span> <span data-ttu-id="e49e9-235">Ha a kapcsolat lassú, érdemes lehet a gépek nagyobb hálózati kapcsolattal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="e49e9-235">If the connection is slow, consider using a machine with a better network connection.</span></span>
<span data-ttu-id="e49e9-236">Ha az ügyfélszámítógép mint a fürt egy másik régióban van, érdemes lehet egy ügyfélszámítógépre egy szorosabb vagy ugyanabban a régióban a fürttel.</span><span class="sxs-lookup"><span data-stu-id="e49e9-236">If the client machine is in another region than the cluster, consider using a client machine in a closer or same region as the cluster.</span></span>
- <span data-ttu-id="e49e9-237">Ellenőrizze, hogy ha hogy elérte-e külső szabályozás.</span><span class="sxs-lookup"><span data-stu-id="e49e9-237">Check if you are hitting external throttling.</span></span> <span data-ttu-id="e49e9-238">Például ha az image store az azure storage használatára van konfigurálva, feltöltés előfordulhat, hogy szabályozva.</span><span class="sxs-lookup"><span data-stu-id="e49e9-238">For example, when the image store is configured to use azure storage, upload may be throttled.</span></span>

<span data-ttu-id="e49e9-239">Probléma: Feltöltés csomag sikeresen befejeződött, de [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) túllépi az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="e49e9-239">Issue: Upload package completed successfully, but [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) times out.</span></span>
<span data-ttu-id="e49e9-240">Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="e49e9-240">Try:</span></span>
- <span data-ttu-id="e49e9-241">[A csomag tömörítése](service-fabric-package-apps.md#compress-a-package) az image store másolás előtt.</span><span class="sxs-lookup"><span data-stu-id="e49e9-241">[Compress the package](service-fabric-package-apps.md#compress-a-package) before copying to the image store.</span></span>
<span data-ttu-id="e49e9-242">A tömörítés csökkenti a, és a fájlok, számát, amely pedig csökkenti a forgalom mennyiségét, és működnek, hogy a Service Fabric kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="e49e9-242">The compression reduces the size and the number of files, which in turn reduces the amount of traffic and work that Service Fabric must perform.</span></span> <span data-ttu-id="e49e9-243">Lehet, hogy a feltöltési művelet lassabb (különösen ha a tömörítés idő), de a regisztrálása és az alkalmazástípus regisztrációjának gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="e49e9-243">The upload operation may be slower (especially if you include the compression time), but register and un-register the application type are faster.</span></span>
- <span data-ttu-id="e49e9-244">Adjon meg egy nagyobb időkorlátjának [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) a `TimeoutSec` paraméter.</span><span class="sxs-lookup"><span data-stu-id="e49e9-244">Specify a larger timeout for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) with `TimeoutSec` parameter.</span></span>
- <span data-ttu-id="e49e9-245">Adja meg `Async` átkapcsolni a [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="e49e9-245">Specify `Async` switch for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span></span> <span data-ttu-id="e49e9-246">A parancs ad vissza, ha a fürt elfogadja a parancsot, és az alkalmazástípus regisztrációjának aszinkron módon folytatódik.</span><span class="sxs-lookup"><span data-stu-id="e49e9-246">The command returns when the cluster accepts the command and the registration of the application type continues asynchronously.</span></span> <span data-ttu-id="e49e9-247">Emiatt nincs szükség ebben az esetben a magasabb időkorlát megadásához.</span><span class="sxs-lookup"><span data-stu-id="e49e9-247">For this reason, there is no need to specify a higher timeout in this case.</span></span> <span data-ttu-id="e49e9-248">A [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancs megjeleníti az összes sikeresen regisztrált alkalmazástípus-verziók és a regisztrációs állapotot.</span><span class="sxs-lookup"><span data-stu-id="e49e9-248">The [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="e49e9-249">Ez a parancs segítségével határozható meg, hogy a regisztrációs történik.</span><span class="sxs-lookup"><span data-stu-id="e49e9-249">You can use this command to determine when the registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a><span data-ttu-id="e49e9-250">Sok fájlokkal alkalmazáscsomag telepítése</span><span class="sxs-lookup"><span data-stu-id="e49e9-250">Deploy application package with many files</span></span>
<span data-ttu-id="e49e9-251">Probléma: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) sok fájlt (akár több ezer sorrendben) az alkalmazás csomagot időtúllépése.</span><span class="sxs-lookup"><span data-stu-id="e49e9-251">Issue: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) times out for an application package with many files (order of thousands).</span></span>
<span data-ttu-id="e49e9-252">Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="e49e9-252">Try:</span></span>
- <span data-ttu-id="e49e9-253">[A csomag tömörítése](service-fabric-package-apps.md#compress-a-package) az image store másolás előtt.</span><span class="sxs-lookup"><span data-stu-id="e49e9-253">[Compress the package](service-fabric-package-apps.md#compress-a-package) before copying to the image store.</span></span> <span data-ttu-id="e49e9-254">A tömörítés csökkenti a fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="e49e9-254">The compression reduces the number of files.</span></span>
- <span data-ttu-id="e49e9-255">Adjon meg egy nagyobb időkorlátjának [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) a `TimeoutSec` paraméter.</span><span class="sxs-lookup"><span data-stu-id="e49e9-255">Specify a larger timeout for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) with `TimeoutSec` parameter.</span></span>
- <span data-ttu-id="e49e9-256">Adja meg `Async` átkapcsolni a [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="e49e9-256">Specify `Async` switch for [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps).</span></span> <span data-ttu-id="e49e9-257">A parancs ad vissza, ha a fürt elfogadja a parancsot, és az alkalmazástípus regisztrációjának aszinkron módon folytatódik.</span><span class="sxs-lookup"><span data-stu-id="e49e9-257">The command returns when the cluster accepts the command and the registration of the application type continues asynchronously.</span></span>
<span data-ttu-id="e49e9-258">Emiatt nincs szükség ebben az esetben a magasabb időkorlát megadásához.</span><span class="sxs-lookup"><span data-stu-id="e49e9-258">For this reason, there is no need to specify a higher timeout in this case.</span></span> <span data-ttu-id="e49e9-259">A [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) parancs megjeleníti az összes sikeresen regisztrált alkalmazástípus-verziók és a regisztrációs állapotot.</span><span class="sxs-lookup"><span data-stu-id="e49e9-259">The [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) command lists all successfully registered application type versions and their registration status.</span></span> <span data-ttu-id="e49e9-260">Ez a parancs segítségével határozható meg, hogy a regisztrációs történik.</span><span class="sxs-lookup"><span data-stu-id="e49e9-260">You can use this command to determine when the registration is done.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a><span data-ttu-id="e49e9-261">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e49e9-261">Next steps</span></span>
[<span data-ttu-id="e49e9-262">A Service Fabric-alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="e49e9-262">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="e49e9-263">A Service Fabric állapotának bemutatása</span><span class="sxs-lookup"><span data-stu-id="e49e9-263">Service Fabric health introduction</span></span>](service-fabric-health-introduction.md)

[<span data-ttu-id="e49e9-264">Diagnosztizálása és megoldása a Service Fabric-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="e49e9-264">Diagnose and troubleshoot a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="e49e9-265">A Service Fabric alkalmazás minta</span><span class="sxs-lookup"><span data-stu-id="e49e9-265">Model an application in Service Fabric</span></span>](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

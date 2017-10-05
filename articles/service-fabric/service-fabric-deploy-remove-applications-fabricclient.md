---
title: "Az Azure Service Fabric-alkalmazás központi telepítése |} Microsoft Docs"
description: "A FabricClient API-k segítségével telepítheti és távolíthat el alkalmazásokat a Service Fabric."
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
ms.date: 07/07/2017
ms.author: ryanwi
ms.openlocfilehash: 2e4ca1069b4e8e473b26b790e81770b41e25ff50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a><span data-ttu-id="c8874-103">Központi telepítése és eltávolítása a FabricClient használó alkalmazások</span><span class="sxs-lookup"><span data-stu-id="c8874-103">Deploy and remove applications using FabricClient</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c8874-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8874-104">PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
> * [<span data-ttu-id="c8874-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8874-105">Visual Studio</span></span>](service-fabric-publish-app-remote-cluster.md)
> * [<span data-ttu-id="c8874-106">FabricClient API-k</span><span class="sxs-lookup"><span data-stu-id="c8874-106">FabricClient APIs</span></span>](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

<span data-ttu-id="c8874-107">Egyszer egy [alkalmazástípus csomagolás][10], az Azure Service Fabric-fürt szolgáltatássablonjaikat használatra kész.</span><span class="sxs-lookup"><span data-stu-id="c8874-107">Once an [application type has been packaged][10], it's ready for deployment into an Azure Service Fabric cluster.</span></span> <span data-ttu-id="c8874-108">Központi telepítés a következő három lépést foglal magában:</span><span class="sxs-lookup"><span data-stu-id="c8874-108">Deployment involves the following three steps:</span></span>

1. <span data-ttu-id="c8874-109">Az image store az alkalmazáscsomag feltöltése</span><span class="sxs-lookup"><span data-stu-id="c8874-109">Upload the application package to the image store</span></span>
2. <span data-ttu-id="c8874-110">Az alkalmazástípus regisztrálása</span><span class="sxs-lookup"><span data-stu-id="c8874-110">Register the application type</span></span>
3. <span data-ttu-id="c8874-111">Az alkalmazáspéldány létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8874-111">Create the application instance</span></span>

<span data-ttu-id="c8874-112">Miután egy alkalmazás lett telepítve, és egy példányát a fürtben fut, törölheti az alkalmazáspéldány és az alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="c8874-112">After an application is deployed and an instance is running in the cluster, you can delete the application instance and its application type.</span></span> <span data-ttu-id="c8874-113">Az alkalmazás teljes eltávolítása a fürt a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="c8874-113">To completely remove an application from the cluster involves the following steps:</span></span>

1. <span data-ttu-id="c8874-114">Távolítsa el (vagy törlése) futó alkalmazáspéldány</span><span class="sxs-lookup"><span data-stu-id="c8874-114">Remove (or delete) the running application instance</span></span>
2. <span data-ttu-id="c8874-115">Az alkalmazástípus regisztrációjának törlése, ha már nincs szüksége</span><span class="sxs-lookup"><span data-stu-id="c8874-115">Unregister the application type if you no longer need it</span></span>
3. <span data-ttu-id="c8874-116">Az alkalmazáscsomag eltávolítása a lemezképtárolóból</span><span class="sxs-lookup"><span data-stu-id="c8874-116">Remove the application package from the image store</span></span>

<span data-ttu-id="c8874-117">Ha [központi telepítéséhez és alkalmazások hibakeresése a Visual Studio](service-fabric-publish-app-remote-cluster.md) meg a helyi fejlesztési fürtöt, az előző lépések kezeli automatikusan egy PowerShell-parancsfájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="c8874-117">If you use [Visual Studio for deploying and debugging applications](service-fabric-publish-app-remote-cluster.md) on your local development cluster, all the preceding steps are handled automatically through a PowerShell script.</span></span>  <span data-ttu-id="c8874-118">Ez a parancsfájl megtalálható a *parancsfájlok* mappában található a projektet.</span><span class="sxs-lookup"><span data-stu-id="c8874-118">This script is found in the *Scripts* folder of the application project.</span></span> <span data-ttu-id="c8874-119">Ez a cikk nyújt háttér milyen, hogy a parancsfájl módon, hogy a Visual Studio kívül ugyanazokat a műveleteket végezheti el.</span><span class="sxs-lookup"><span data-stu-id="c8874-119">This article provides background on what that script is doing so that you can perform the same operations outside of Visual Studio.</span></span> 
 
## <a name="connect-to-the-cluster"></a><span data-ttu-id="c8874-120">Csatlakozás a fürthöz</span><span class="sxs-lookup"><span data-stu-id="c8874-120">Connect to the cluster</span></span>
<span data-ttu-id="c8874-121">Csatlakozzon a fürthöz, hozzon létre egy [FabricClient](/dotnet/api/system.fabric.fabricclient) példány, mielőtt futtatja példák ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="c8874-121">Connect to the cluster by creating a [FabricClient](/dotnet/api/system.fabric.fabricclient) instance before you run any of the code examples in this article.</span></span> <span data-ttu-id="c8874-122">Példák a helyi fejlesztési fürtök vagy egy távoli fürtöt vagy az Azure Active Directoryval, X509 védett fürt csatlakozik a tanúsítványokat, vagy a Windows Active Directory [Csatlakozás biztonságos fürthöz](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).</span><span class="sxs-lookup"><span data-stu-id="c8874-122">For examples of connecting to a local development cluster or a remote cluster or cluster secured using Azure Active Directory, X509 certificates, or Windows Active Directory see [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).</span></span> <span data-ttu-id="c8874-123">Szeretne csatlakozni a helyi fejlesztési fürtöt, futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="c8874-123">To connect to the local development cluster, run the following:</span></span>

```csharp
// Connect to the local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-the-application-package"></a><span data-ttu-id="c8874-124">Az alkalmazáscsomag feltöltése</span><span class="sxs-lookup"><span data-stu-id="c8874-124">Upload the application package</span></span>
<span data-ttu-id="c8874-125">Tegyük fel, hogy most felépíti és nevű csomag *MyApplication* a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="c8874-125">Suppose you build and package an application named *MyApplication* in Visual Studio.</span></span> <span data-ttu-id="c8874-126">Alapértelmezés szerint az alkalmazás szerepel a ApplicationManifest.xml típusneve "MyApplicationType".</span><span class="sxs-lookup"><span data-stu-id="c8874-126">By default, the application type name listed in the ApplicationManifest.xml is "MyApplicationType".</span></span>  <span data-ttu-id="c8874-127">Az alkalmazáscsomag, amely tartalmazza a szükséges alkalmazás jegyzékfájlja, a szolgáltatás jegyzékfájlban és a kód/config/adatok csomagok, található *C:\Users\&lt; felhasználónév&gt;\Documents\Visual Studio 2017\Projects\ MyApplication\MyApplication\pkg\Debug*.</span><span class="sxs-lookup"><span data-stu-id="c8874-127">The application package, which contains the necessary application manifest, service manifests, and code/config/data packages, is located in *C:\Users\&lt;username&gt;\Documents\Visual Studio 2017\Projects\MyApplication\MyApplication\pkg\Debug*.</span></span>

<span data-ttu-id="c8874-128">Az alkalmazáscsomag feltöltése helyezi a belső Service Fabric összetevői által elérhető helyen.</span><span class="sxs-lookup"><span data-stu-id="c8874-128">Uploading the application package puts it in a location that's accessible by the internal Service Fabric components.</span></span> <span data-ttu-id="c8874-129">A Service Fabric ellenőrzi az alkalmazáscsomag alkalmazáscsomag a regisztráció során.</span><span class="sxs-lookup"><span data-stu-id="c8874-129">Service Fabric verifies the application package during the registration of the application package.</span></span> <span data-ttu-id="c8874-130">Ha szeretné ellenőrizni a alkalmazáscsomagot helyileg (azaz feltöltés előtt), használja a [teszt-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c8874-130">However, if you want to verify the application package locally (i.e., before uploading), use the [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="c8874-131">A [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API feltölti az alkalmazáscsomagot a fürt lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="c8874-131">The [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API uploads the application package to the cluster image store.</span></span> 

<span data-ttu-id="c8874-132">Ha az alkalmazáscsomag nagy és/vagy sok fájl van, akkor [a tömörítés](service-fabric-package-apps.md#compress-a-package) , és másolja az image store PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="c8874-132">If the application package is large and/or has many files, you can [compress it](service-fabric-package-apps.md#compress-a-package) and copy it to the image store using PowerShell.</span></span> <span data-ttu-id="c8874-133">A tömörítés csökkenti a méretét és a fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="c8874-133">The compression reduces the size and the number of files.</span></span>

<span data-ttu-id="c8874-134">Lásd: [megérteni a lemezkép tárolási kapcsolati karakterlánc](service-fabric-image-store-connection-string.md) az image store és a lemezkép kiegészítő információt tárolja a kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="c8874-134">See [Understand the image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about the image store and image store connection string.</span></span>

## <a name="register-the-application-package"></a><span data-ttu-id="c8874-135">Az alkalmazáscsomag regisztrálása</span><span class="sxs-lookup"><span data-stu-id="c8874-135">Register the application package</span></span>
<span data-ttu-id="c8874-136">Az alkalmazástípus és -verzió az alkalmazás jegyzékében használható válnak, amikor regisztrál az alkalmazáscsomag deklarálva.</span><span class="sxs-lookup"><span data-stu-id="c8874-136">The application type and version declared in the application manifest become available for use when the application package is registered.</span></span> <span data-ttu-id="c8874-137">A rendszer beolvassa az előző lépésben feltöltött csomag, ellenőrzi a csomag, feldolgozza a csomag tartalmát, és a feldolgozott csomag belső rendszer helyre másolja.</span><span class="sxs-lookup"><span data-stu-id="c8874-137">The system reads the package uploaded in the previous step, verifies the package, processes the package contents, and copies the processed package to an internal system location.</span></span>  

<span data-ttu-id="c8874-138">A [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API regiszterekben az alkalmazás írja be a fürt, és tegye elérhetővé a telepítésre.</span><span class="sxs-lookup"><span data-stu-id="c8874-138">The [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API registers the application type in the cluster and make it available for deployment.</span></span>

<span data-ttu-id="c8874-139">A [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API sikeresen regisztrált alkalmazástípusokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c8874-139">The [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API provides information about all successfully registered application types.</span></span> <span data-ttu-id="c8874-140">Ez az API segítségével határozható meg, hogy a regisztrációs történik.</span><span class="sxs-lookup"><span data-stu-id="c8874-140">You can use this API to determine when the registration is done.</span></span>

## <a name="create-an-application-instance"></a><span data-ttu-id="c8874-141">Hozzon létre egy alkalmazáspéldányt</span><span class="sxs-lookup"><span data-stu-id="c8874-141">Create an application instance</span></span>
<span data-ttu-id="c8874-142">Az alkalmazás regisztrálása sikeresen befejeződött a alkalmazás típussal a példányosítható a [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API.</span><span class="sxs-lookup"><span data-stu-id="c8874-142">You can instantiate an application from any application type that has been registered successfully by using the [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API.</span></span> <span data-ttu-id="c8874-143">Minden alkalmazás nevére kell kezdődnie az *"fabric:"* sémáját, és minden alkalmazáspéldányhoz (fürtözött) egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c8874-143">The name of each application must start with the *"fabric:"* scheme and must be unique for each application instance (within a cluster).</span></span> <span data-ttu-id="c8874-144">A célalkalmazás típusa alkalmazás jegyzékében definiált alapértelmezett szolgáltatások is jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="c8874-144">Any default services defined in the application manifest of the target application type are also created.</span></span>

<span data-ttu-id="c8874-145">Több alkalmazáspéldányt is létrehozható egy regisztrált alkalmazástípus bármely adott verzióját.</span><span class="sxs-lookup"><span data-stu-id="c8874-145">Multiple application instances can be created for any given version of a registered application type.</span></span> <span data-ttu-id="c8874-146">Minden egyes alkalmazáspéldány feladata elkülönítve, a saját munkakönyvtár és a folyamatok.</span><span class="sxs-lookup"><span data-stu-id="c8874-146">Each application instance runs in isolation, with its own working directory and set of processes.</span></span>

<span data-ttu-id="c8874-147">Megtekintéséhez, amely nevű alkalmazások és szolgáltatások futnak a fürthöz, futtassa a [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) és [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) API-k.</span><span class="sxs-lookup"><span data-stu-id="c8874-147">To see which named applications and services are running in the cluster, run the [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) and [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) APIs.</span></span>

## <a name="create-a-service-instance"></a><span data-ttu-id="c8874-148">A szolgáltatáspéldány létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8874-148">Create a service instance</span></span>
<span data-ttu-id="c8874-149">Adott szolgáltatásnak a szolgáltatás típus használatával példányosítható a [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.</span><span class="sxs-lookup"><span data-stu-id="c8874-149">You can instantiate a service from a service type using the [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.</span></span>  <span data-ttu-id="c8874-150">Ha a szolgáltatás alapértelmezett szolgáltatásként az alkalmazásjegyzékben van deklarálva, a szolgáltatás létrejön, amikor az alkalmazás létrejön.</span><span class="sxs-lookup"><span data-stu-id="c8874-150">If the service is declared as a default service in the application manifest, the service is instantiated when the application is instantiated.</span></span>  <span data-ttu-id="c8874-151">Hívja a [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API-t a már létrehozott egy szolgáltatást fog visszaadni a FabricErrorCode.ServiceAlreadyExists értékű hiba kódot tartalmazó FabricException típusú kivételt.</span><span class="sxs-lookup"><span data-stu-id="c8874-151">Calling the [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API for a service that is already instantiated will return an exception of type FabricException containing an error code with a value of FabricErrorCode.ServiceAlreadyExists.</span></span>

## <a name="remove-a-service-instance"></a><span data-ttu-id="c8874-152">A szolgáltatás példányának eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c8874-152">Remove a service instance</span></span>
<span data-ttu-id="c8874-153">Amikor egy szolgáltatáspéldány már nem szükséges, eltávolíthatja a futását meghívásával alkalmazáspéldány a [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.</span><span class="sxs-lookup"><span data-stu-id="c8874-153">When a service instance is no longer needed, you can remove it from the running application instance by calling the [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.</span></span>  

> [!WARNING]
> <span data-ttu-id="c8874-154">Ez a művelet nem vonható vissza, és a szolgáltatás állapota nem állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="c8874-154">This operation cannot be reversed, and service state cannot be recovered.</span></span>

## <a name="remove-an-application-instance"></a><span data-ttu-id="c8874-155">Az alkalmazáspéldány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c8874-155">Remove an application instance</span></span>
<span data-ttu-id="c8874-156">Az alkalmazáspéldány nincs szükség, ha véglegesen eltávolíthatja azt név használatával a [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API.</span><span class="sxs-lookup"><span data-stu-id="c8874-156">When an application instance is no longer needed, you can permanently remove it by name using the [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API.</span></span> <span data-ttu-id="c8874-157">[DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automatikusan eltávolítja az is, végleges eltávolítása az összes szolgáltatás állapota az alkalmazáshoz tartozó összes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c8874-157">[DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automatically removes all services that belong to the application as well, permanently removing all service state.</span></span>

> [!WARNING]
> <span data-ttu-id="c8874-158">Ez a művelet nem vonható vissza, és az alkalmazás állapota nem állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="c8874-158">This operation cannot be reversed, and application state cannot be recovered.</span></span>

## <a name="unregister-an-application-type"></a><span data-ttu-id="c8874-159">Az alkalmazástípus regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="c8874-159">Unregister an application type</span></span>
<span data-ttu-id="c8874-160">Amikor egy alkalmazás típus adott verziójának már nem szükséges, az alkalmazás típust használja, hogy adott verziójának kell regisztrációjának törlése a [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API.</span><span class="sxs-lookup"><span data-stu-id="c8874-160">When a particular version of an application type is no longer needed, you should unregister that particular version of the application type using the [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API.</span></span> <span data-ttu-id="c8874-161">Az image store által felhasznált tárterület beállításjegyzékből való törlésekor alkalmazástípus nem használt verziók kiadását.</span><span class="sxs-lookup"><span data-stu-id="c8874-161">Unregistering unused versions of application types releases storage space used by the image store.</span></span> <span data-ttu-id="c8874-162">Egy alkalmazástípus verziója mindaddig, amíg nincs alkalmazások létrehozásának ellen, hogy az alkalmazástípus verziója, és nincs függőben lévő alkalmazásfrissítések hivatkoznak, hogy az alkalmazástípus verziója lehet regisztrációjának törlése.</span><span class="sxs-lookup"><span data-stu-id="c8874-162">A version of an application type can be unregistered as long as no applications are instantiated against that version of the application type and no pending application upgrades are referencing that version of the application type.</span></span>

## <a name="remove-an-application-package-from-the-image-store"></a><span data-ttu-id="c8874-163">Az alkalmazáscsomag eltávolítása a lemezképtárolóból</span><span class="sxs-lookup"><span data-stu-id="c8874-163">Remove an application package from the image store</span></span>
<span data-ttu-id="c8874-164">Ha már nincs szükség egy alkalmazáscsomagot, törölheti a lemezképtárolóból használatával rendszererőforrások felszabadítása érdekében a [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.</span><span class="sxs-lookup"><span data-stu-id="c8874-164">When an application package is no longer needed, you can delete it from the image store to free up system resources using the [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c8874-165">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="c8874-165">Troubleshooting</span></span>
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a><span data-ttu-id="c8874-166">Másolás-ServiceFabricApplicationPackage kér egy előtaggal</span><span class="sxs-lookup"><span data-stu-id="c8874-166">Copy-ServiceFabricApplicationPackage asks for an ImageStoreConnectionString</span></span>
<span data-ttu-id="c8874-167">A Service Fabric SDK környezet már rendelkezik a megfelelő alapértelmezett beállítása.</span><span class="sxs-lookup"><span data-stu-id="c8874-167">The Service Fabric SDK environment should already have the correct defaults set up.</span></span> <span data-ttu-id="c8874-168">De ha szükséges, az összes parancs előtaggal meg kell felelnie a Service Fabric fürt által használt érték.</span><span class="sxs-lookup"><span data-stu-id="c8874-168">But if needed, the ImageStoreConnectionString for all commands should match the value that the Service Fabric cluster is using.</span></span> <span data-ttu-id="c8874-169">A előtaggal megtalálható a fürtjegyzékben le kérni a [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) és Get-ImageStoreConnectionStringFromClusterManifest parancsokat:</span><span class="sxs-lookup"><span data-stu-id="c8874-169">You can find the ImageStoreConnectionString in the cluster manifest, retrieved using the [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) and Get-ImageStoreConnectionStringFromClusterManifest commands:</span></span>

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

<span data-ttu-id="c8874-170">A **Get-ImageStoreConnectionStringFromClusterManifest** parancsmagot, amely a Service Fabric SDK PowerShell modulja részét képezi, a lemezkép tárolási kapcsolati karakterlánc beolvasása szolgál.</span><span class="sxs-lookup"><span data-stu-id="c8874-170">The **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of the Service Fabric SDK PowerShell module, is used to get the image store connection string.</span></span>  <span data-ttu-id="c8874-171">Az SDK modul importálásához futtassa:</span><span class="sxs-lookup"><span data-stu-id="c8874-171">To import the SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


<span data-ttu-id="c8874-172">A előtaggal megtalálható a fürtjegyzékben:</span><span class="sxs-lookup"><span data-stu-id="c8874-172">The ImageStoreConnectionString is found in the cluster manifest:</span></span>

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

<span data-ttu-id="c8874-173">Lásd: [megérteni a lemezkép tárolási kapcsolati karakterlánc](service-fabric-image-store-connection-string.md) az image store és a lemezkép kiegészítő információt tárolja a kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="c8874-173">See [Understand the image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about the image store and image store connection string.</span></span>

### <a name="deploy-large-application-package"></a><span data-ttu-id="c8874-174">Nagy alkalmazáscsomag telepítése</span><span class="sxs-lookup"><span data-stu-id="c8874-174">Deploy large application package</span></span>
<span data-ttu-id="c8874-175">Probléma: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) a nagy alkalmazáscsomagok (GB-os sorrendben) időtúllépése API.</span><span class="sxs-lookup"><span data-stu-id="c8874-175">Issue: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API times out for a large application package (order of GB).</span></span>
<span data-ttu-id="c8874-176">Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="c8874-176">Try:</span></span>
- <span data-ttu-id="c8874-177">Adjon meg egy nagyobb időkorlátjának [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) metódus, a `timeout` paraméter.</span><span class="sxs-lookup"><span data-stu-id="c8874-177">Specify a larger timeout for [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) method, with `timeout` parameter.</span></span> <span data-ttu-id="c8874-178">Alapértelmezés szerint az időtúllépés értéke 30 perc.</span><span class="sxs-lookup"><span data-stu-id="c8874-178">By default, the timeout is 30 minutes.</span></span>
- <span data-ttu-id="c8874-179">Ellenőrizze a hálózati kapcsolat a forrásgép és fürt között.</span><span class="sxs-lookup"><span data-stu-id="c8874-179">Check the network connection between your source machine and cluster.</span></span> <span data-ttu-id="c8874-180">Ha a kapcsolat lassú, érdemes lehet a gépek nagyobb hálózati kapcsolattal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="c8874-180">If the connection is slow, consider using a machine with a better network connection.</span></span>
<span data-ttu-id="c8874-181">Ha az ügyfélszámítógép mint a fürt egy másik régióban van, érdemes lehet egy ügyfélszámítógépre egy szorosabb vagy ugyanabban a régióban a fürttel.</span><span class="sxs-lookup"><span data-stu-id="c8874-181">If the client machine is in another region than the cluster, consider using a client machine in a closer or same region as the cluster.</span></span>
- <span data-ttu-id="c8874-182">Ellenőrizze, hogy ha hogy elérte-e külső szabályozás.</span><span class="sxs-lookup"><span data-stu-id="c8874-182">Check if you are hitting external throttling.</span></span> <span data-ttu-id="c8874-183">Például ha az image store az azure storage használatára van konfigurálva, feltöltés előfordulhat, hogy szabályozva.</span><span class="sxs-lookup"><span data-stu-id="c8874-183">For example, when the image store is configured to use azure storage, upload may be throttled.</span></span>

<span data-ttu-id="c8874-184">Probléma: Feltöltés csomag sikeresen befejeződött, de [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API túllépi az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="c8874-184">Issue: Upload package completed successfully, but [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API times out.</span></span>
<span data-ttu-id="c8874-185">Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="c8874-185">Try:</span></span>
- <span data-ttu-id="c8874-186">[A csomag tömörítése](service-fabric-package-apps.md#compress-a-package) az image store másolás előtt.</span><span class="sxs-lookup"><span data-stu-id="c8874-186">[Compress the package](service-fabric-package-apps.md#compress-a-package) before copying to the image store.</span></span>
<span data-ttu-id="c8874-187">A tömörítés csökkenti a, és a fájlok, számát, amely pedig csökkenti a forgalom mennyiségét, és működnek, hogy a Service Fabric kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="c8874-187">The compression reduces the size and the number of files, which in turn reduces the amount of traffic and work that Service Fabric must perform.</span></span> <span data-ttu-id="c8874-188">Lehet, hogy a feltöltési művelet lassabb (különösen ha a tömörítés idő), de a regisztrálása és az alkalmazástípus regisztrációjának gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="c8874-188">The upload operation may be slower (especially if you include the compression time), but register and un-register the application type are faster.</span></span>
- <span data-ttu-id="c8874-189">Adjon meg egy nagyobb időkorlátjának [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API-t `timeout` paraméter.</span><span class="sxs-lookup"><span data-stu-id="c8874-189">Specify a larger timeout for [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API with `timeout` parameter.</span></span>

### <a name="deploy-application-package-with-many-files"></a><span data-ttu-id="c8874-190">Sok fájlokkal alkalmazáscsomag telepítése</span><span class="sxs-lookup"><span data-stu-id="c8874-190">Deploy application package with many files</span></span>
<span data-ttu-id="c8874-191">Probléma: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) sok fájlt (akár több ezer sorrendben) az alkalmazás csomagot időtúllépése.</span><span class="sxs-lookup"><span data-stu-id="c8874-191">Issue: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) times out for an application package with many files (order of thousands).</span></span>
<span data-ttu-id="c8874-192">Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="c8874-192">Try:</span></span>
- <span data-ttu-id="c8874-193">[A csomag tömörítése](service-fabric-package-apps.md#compress-a-package) az image store másolás előtt.</span><span class="sxs-lookup"><span data-stu-id="c8874-193">[Compress the package](service-fabric-package-apps.md#compress-a-package) before copying to the image store.</span></span> <span data-ttu-id="c8874-194">A tömörítés csökkenti a fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="c8874-194">The compression reduces the number of files.</span></span>
- <span data-ttu-id="c8874-195">Adjon meg egy nagyobb időkorlátjának [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) a `timeout` paraméter.</span><span class="sxs-lookup"><span data-stu-id="c8874-195">Specify a larger timeout for [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) with `timeout` parameter.</span></span>

## <a name="code-example"></a><span data-ttu-id="c8874-196">Mintakód</span><span class="sxs-lookup"><span data-stu-id="c8874-196">Code example</span></span>
<span data-ttu-id="c8874-197">Az alábbi példa alkalmazáscsomag másolja az image store, látja el az alkalmazás típusa, létrehoz egy alkalmazáspéldányt, létrehoz egy szolgáltatáspéldány, eltávolítja az alkalmazáspéldány, un-kiosztja az alkalmazástípus és törli a a lemezképtárolóból alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="c8874-197">The following example copies an application package to the image store, provisions the application type, creates an application instance, creates a service instance, removes the application instance, un-provisions the application type, and deletes the application package from the image store.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using System.Threading.Tasks;

using System.Fabric;
using System.Fabric.Description;
using System.Threading;

namespace ServiceFabricAppLifecycle
{
class Program
{
static void Main(string[] args)
{

    string clusterConnection = "localhost:19000";
    string appName = "fabric:/MyApplication";
    string appType = "MyApplicationType";
    string appVersion = "1.0.0";
    string serviceName = "fabric:/MyApplication/Stateless1";
    string imageStoreConnectionString = "file:C:\\SfDevCluster\\Data\\ImageStoreShare";
    string packagePathInImageStore = "MyApplication";
    string packagePath = "C:\\Users\\username\\Documents\\Visual Studio 2017\\Projects\\MyApplication\\MyApplication\\pkg\\Debug";
    string serviceType = "Stateless1Type";

    // Connect to the cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy the application package to a location in the image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied to {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy to Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision the application.  "MyApplicationV1" is the folder in the image store where the application package is located. 
    // The application type with name "MyApplicationType" and version "1.0.0" (both are found in the application manifest) 
    // is now registered in the cluster.            
    try
    {
        fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

        Console.WriteLine("Provisioned application type {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Provision Application Type failed:");

        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    //  Create the application instance.
    try
    {
        ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
        fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
        Console.WriteLine("Created application instance of type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Create the stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create the service instance.  If the service is declared as a default service in the ApplicationManifest.xml,
    // the service instance is already running and this call will fail.
    try
    {
        fabricClient.ServiceManager.CreateServiceAsync(serviceDescription).Wait();
        Console.WriteLine("Created service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete a service instance.
    try
    {
        DeleteServiceDescription deleteServiceDescription = new DeleteServiceDescription(new Uri(serviceName));

        fabricClient.ServiceManager.DeleteServiceAsync(deleteServiceDescription);
        Console.WriteLine("Deleted service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete an application instance from the application type.
    try
    {
        DeleteApplicationDescription deleteApplicationDescription = new DeleteApplicationDescription(new Uri(appName));

        fabricClient.ApplicationManager.DeleteApplicationAsync(deleteApplicationDescription).Wait();
        Console.WriteLine("Deleted application instance {0}", appName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Un-provision the application type.
    try
    {
        fabricClient.ApplicationManager.UnprovisionApplicationAsync(appType, appVersion).Wait();
        Console.WriteLine("Un-provisioned application type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Un-provision application type failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete the application package from a location in the image store.
    try
    {
        fabricClient.ApplicationManager.RemoveApplicationPackage(imageStoreConnectionString, packagePathInImageStore);
        Console.WriteLine("Application package removed from {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package removal from Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    Console.WriteLine("Hit enter...");
    Console.Read();
}        
}
}

```

## <a name="next-steps"></a><span data-ttu-id="c8874-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8874-198">Next steps</span></span>
[<span data-ttu-id="c8874-199">A Service Fabric-alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="c8874-199">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="c8874-200">A Service Fabric állapotának bemutatása</span><span class="sxs-lookup"><span data-stu-id="c8874-200">Service Fabric health introduction</span></span>](service-fabric-health-introduction.md)

[<span data-ttu-id="c8874-201">Diagnosztizálása és megoldása a Service Fabric-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c8874-201">Diagnose and troubleshoot a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="c8874-202">A Service Fabric alkalmazás minta</span><span class="sxs-lookup"><span data-stu-id="c8874-202">Model an application in Service Fabric</span></span>](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

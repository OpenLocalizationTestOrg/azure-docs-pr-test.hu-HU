---
title: "a Service Fabric-alkalmazás központi telepítésének aaaAzure |} Microsoft Docs"
description: "Hello FabricClient API-k toodeploy használja, és távolítsa el az alkalmazások a Service Fabric."
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
ms.openlocfilehash: b2986b71c461f3e785ba16ec1b827fe47ad852fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a><span data-ttu-id="0e10c-103">Központi telepítése és eltávolítása a FabricClient használó alkalmazások</span><span class="sxs-lookup"><span data-stu-id="0e10c-103">Deploy and remove applications using FabricClient</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0e10c-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0e10c-104">PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
> * [<span data-ttu-id="0e10c-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e10c-105">Visual Studio</span></span>](service-fabric-publish-app-remote-cluster.md)
> * [<span data-ttu-id="0e10c-106">FabricClient API-k</span><span class="sxs-lookup"><span data-stu-id="0e10c-106">FabricClient APIs</span></span>](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

<span data-ttu-id="0e10c-107">Egyszer egy [alkalmazástípus csomagolás][10], az Azure Service Fabric-fürt szolgáltatássablonjaikat használatra kész.</span><span class="sxs-lookup"><span data-stu-id="0e10c-107">Once an [application type has been packaged][10], it's ready for deployment into an Azure Service Fabric cluster.</span></span> <span data-ttu-id="0e10c-108">Központi telepítés hello a következő három lépést foglal magában:</span><span class="sxs-lookup"><span data-stu-id="0e10c-108">Deployment involves hello following three steps:</span></span>

1. <span data-ttu-id="0e10c-109">Hello alkalmazás csomag toohello lemezképtárolóhoz feltöltése</span><span class="sxs-lookup"><span data-stu-id="0e10c-109">Upload hello application package toohello image store</span></span>
2. <span data-ttu-id="0e10c-110">Hello alkalmazástípus regisztrálása</span><span class="sxs-lookup"><span data-stu-id="0e10c-110">Register hello application type</span></span>
3. <span data-ttu-id="0e10c-111">Hello alkalmazáspéldány létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e10c-111">Create hello application instance</span></span>

<span data-ttu-id="0e10c-112">Miután egy alkalmazás lett telepítve, és a példány hello fürtben fut, törölheti a hello alkalmazáspéldány és az alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="0e10c-112">After an application is deployed and an instance is running in hello cluster, you can delete hello application instance and its application type.</span></span> <span data-ttu-id="0e10c-113">toocompletely eltávolítása egy alkalmazáskészletet hello fürtből hello lépéseket foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="0e10c-113">toocompletely remove an application from hello cluster involves hello following steps:</span></span>

1. <span data-ttu-id="0e10c-114">Alkalmazás-példányt futtató hello eltávolítása (vagy törlése)</span><span class="sxs-lookup"><span data-stu-id="0e10c-114">Remove (or delete) hello running application instance</span></span>
2. <span data-ttu-id="0e10c-115">Ha már nincs szüksége a hello alkalmazástípus regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="0e10c-115">Unregister hello application type if you no longer need it</span></span>
3. <span data-ttu-id="0e10c-116">Hello lemezképtárolóhoz hello alkalmazáscsomag eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0e10c-116">Remove hello application package from hello image store</span></span>

<span data-ttu-id="0e10c-117">Ha [központi telepítéséhez és alkalmazások hibakeresése a Visual Studio](service-fabric-publish-app-remote-cluster.md) a helyi fejlesztési fürtöt, a fenti lépéseket minden hello egy PowerShell-parancsfájl segítségével automatikusan kezeli.</span><span class="sxs-lookup"><span data-stu-id="0e10c-117">If you use [Visual Studio for deploying and debugging applications](service-fabric-publish-app-remote-cluster.md) on your local development cluster, all hello preceding steps are handled automatically through a PowerShell script.</span></span>  <span data-ttu-id="0e10c-118">Ez a parancsfájl található hello *parancsfájlok* hello projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="0e10c-118">This script is found in hello *Scripts* folder of hello application project.</span></span> <span data-ttu-id="0e10c-119">Ez a cikk ismerteti a háttérben, hogy a parancsfájl tevékenységétől, hogy végezheti el a Visual Studio kívül ugyanazokat a műveleteket hello.</span><span class="sxs-lookup"><span data-stu-id="0e10c-119">This article provides background on what that script is doing so that you can perform hello same operations outside of Visual Studio.</span></span> 
 
## <a name="connect-toohello-cluster"></a><span data-ttu-id="0e10c-120">Csatlakoztassa toohello fürtöt</span><span class="sxs-lookup"><span data-stu-id="0e10c-120">Connect toohello cluster</span></span>
<span data-ttu-id="0e10c-121">Csatlakoztassa a toohello fürtöt hozzon létre egy [FabricClient](/dotnet/api/system.fabric.fabricclient) példány futtatása előtt minden hello kódpéldák ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="0e10c-121">Connect toohello cluster by creating a [FabricClient](/dotnet/api/system.fabric.fabricclient) instance before you run any of hello code examples in this article.</span></span> <span data-ttu-id="0e10c-122">Példák a kapcsolódó tooa helyi fejlesztési fürtök vagy egy távoli fürtöt vagy az Azure Active Directoryval, X509 védett fürt tanúsítványokat, vagy a Windows Active Directory [Connect tooa biztonságos fürt](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).</span><span class="sxs-lookup"><span data-stu-id="0e10c-122">For examples of connecting tooa local development cluster or a remote cluster or cluster secured using Azure Active Directory, X509 certificates, or Windows Active Directory see [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).</span></span> <span data-ttu-id="0e10c-123">tooconnect toohello helyi fejlesztési fürtöt, futtassa a következő hello:</span><span class="sxs-lookup"><span data-stu-id="0e10c-123">tooconnect toohello local development cluster, run hello following:</span></span>

```csharp
// Connect toohello local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-hello-application-package"></a><span data-ttu-id="0e10c-124">Hello alkalmazáscsomag feltöltése</span><span class="sxs-lookup"><span data-stu-id="0e10c-124">Upload hello application package</span></span>
<span data-ttu-id="0e10c-125">Tegyük fel, hogy most felépíti és nevű csomag *MyApplication* a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="0e10c-125">Suppose you build and package an application named *MyApplication* in Visual Studio.</span></span> <span data-ttu-id="0e10c-126">Alapértelmezés szerint a felsorolt hello ApplicationManifest.xml hello alkalmazástípus neve "MyApplicationType".</span><span class="sxs-lookup"><span data-stu-id="0e10c-126">By default, hello application type name listed in hello ApplicationManifest.xml is "MyApplicationType".</span></span>  <span data-ttu-id="0e10c-127">hello alkalmazáscsomagot, amely tartalmazza a hello szükséges alkalmazás jegyzékfájlja, a szolgáltatás jegyzékfájlban és a kód/config/adatok csomagok, található *C:\Users\&lt; felhasználónév&gt;\Documents\Visual Studio 2017\Projects\ MyApplication\MyApplication\pkg\Debug*.</span><span class="sxs-lookup"><span data-stu-id="0e10c-127">hello application package, which contains hello necessary application manifest, service manifests, and code/config/data packages, is located in *C:\Users\&lt;username&gt;\Documents\Visual Studio 2017\Projects\MyApplication\MyApplication\pkg\Debug*.</span></span>

<span data-ttu-id="0e10c-128">Hello alkalmazáscsomag feltöltése hello belső Service Fabric összetevői által elérhető helyen helyezi.</span><span class="sxs-lookup"><span data-stu-id="0e10c-128">Uploading hello application package puts it in a location that's accessible by hello internal Service Fabric components.</span></span> <span data-ttu-id="0e10c-129">A Service Fabric hello alkalmazáscsomag hello alkalmazáscsomag hello regisztrálás során ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="0e10c-129">Service Fabric verifies hello application package during hello registration of hello application package.</span></span> <span data-ttu-id="0e10c-130">Azonban ha tooverify hello alkalmazáscsomagot helyileg (azaz feltöltés előtt), használható hello [teszt-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0e10c-130">However, if you want tooverify hello application package locally (i.e., before uploading), use hello [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="0e10c-131">Hello [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API feltölt hello alkalmazás csomag toohello fürt lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="0e10c-131">hello [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API uploads hello application package toohello cluster image store.</span></span> 

<span data-ttu-id="0e10c-132">Ha hello alkalmazáscsomag nagy és/vagy sok fájl van, akkor [a tömörítés](service-fabric-package-apps.md#compress-a-package) és a PowerShell használatával toohello lemezképtárolóhoz másolásához.</span><span class="sxs-lookup"><span data-stu-id="0e10c-132">If hello application package is large and/or has many files, you can [compress it](service-fabric-package-apps.md#compress-a-package) and copy it toohello image store using PowerShell.</span></span> <span data-ttu-id="0e10c-133">hello tömörítés csökkenti a hello mérete és a fájlok hello száma.</span><span class="sxs-lookup"><span data-stu-id="0e10c-133">hello compression reduces hello size and hello number of files.</span></span>

<span data-ttu-id="0e10c-134">Lásd: [hello lemezkép tárolási kapcsolati karakterlánc megértéséhez](service-fabric-image-store-connection-string.md) kiegészítő információkat megtudni hello image store és lemezkép tárolásához kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="0e10c-134">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

## <a name="register-hello-application-package"></a><span data-ttu-id="0e10c-135">Hello alkalmazáscsomag regisztrálása</span><span class="sxs-lookup"><span data-stu-id="0e10c-135">Register hello application package</span></span>
<span data-ttu-id="0e10c-136">regisztrálva van-e hello alkalmazáscsomag válnak használható hello alkalmazás jegyzékfájlban deklarált hello alkalmazástípus és -verzió.</span><span class="sxs-lookup"><span data-stu-id="0e10c-136">hello application type and version declared in hello application manifest become available for use when hello application package is registered.</span></span> <span data-ttu-id="0e10c-137">hello rendszer beolvassa az előző lépésben hello feltöltött hello csomag, hello csomag ellenőrzi, hello csomag tartalmának folyamatokat, és másolja át a feldolgozott hello csomagok tooan belső rendszer helyét.</span><span class="sxs-lookup"><span data-stu-id="0e10c-137">hello system reads hello package uploaded in hello previous step, verifies hello package, processes hello package contents, and copies hello processed package tooan internal system location.</span></span>  

<span data-ttu-id="0e10c-138">Hello [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API regiszterekben hello alkalmazástípus hello fürt, és tegye elérhetővé a központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="0e10c-138">hello [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API registers hello application type in hello cluster and make it available for deployment.</span></span>

<span data-ttu-id="0e10c-139">Hello [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API sikeresen regisztrált alkalmazástípusokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0e10c-139">hello [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API provides information about all successfully registered application types.</span></span> <span data-ttu-id="0e10c-140">Az API toodetermine is használhatja, amikor hello regisztrációs hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="0e10c-140">You can use this API toodetermine when hello registration is done.</span></span>

## <a name="create-an-application-instance"></a><span data-ttu-id="0e10c-141">Hozzon létre egy alkalmazáspéldányt</span><span class="sxs-lookup"><span data-stu-id="0e10c-141">Create an application instance</span></span>
<span data-ttu-id="0e10c-142">Az alkalmazás regisztrálása sikeresen befejeződött hello segítségével alkalmazás típussal a példányosítható [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API.</span><span class="sxs-lookup"><span data-stu-id="0e10c-142">You can instantiate an application from any application type that has been registered successfully by using hello [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API.</span></span> <span data-ttu-id="0e10c-143">minden alkalmazás hello neve kezdődhet hello *"fabric:"* sémáját, és minden alkalmazáspéldányhoz (fürtözött) egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0e10c-143">hello name of each application must start with hello *"fabric:"* scheme and must be unique for each application instance (within a cluster).</span></span> <span data-ttu-id="0e10c-144">Hello célalkalmazás típusa hello alkalmazás jegyzékében definiált alapértelmezett szolgáltatások is jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="0e10c-144">Any default services defined in hello application manifest of hello target application type are also created.</span></span>

<span data-ttu-id="0e10c-145">Több alkalmazáspéldányt is létrehozható egy regisztrált alkalmazástípus bármely adott verzióját.</span><span class="sxs-lookup"><span data-stu-id="0e10c-145">Multiple application instances can be created for any given version of a registered application type.</span></span> <span data-ttu-id="0e10c-146">Minden egyes alkalmazáspéldány feladata elkülönítve, a saját munkakönyvtár és a folyamatok.</span><span class="sxs-lookup"><span data-stu-id="0e10c-146">Each application instance runs in isolation, with its own working directory and set of processes.</span></span>

<span data-ttu-id="0e10c-147">toosee, amely nevű alkalmazások és szolgáltatások futnak hello fürthöz, futtassa a hello [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) és [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) API-k.</span><span class="sxs-lookup"><span data-stu-id="0e10c-147">toosee which named applications and services are running in hello cluster, run hello [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) and [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) APIs.</span></span>

## <a name="create-a-service-instance"></a><span data-ttu-id="0e10c-148">A szolgáltatáspéldány létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e10c-148">Create a service instance</span></span>
<span data-ttu-id="0e10c-149">A szolgáltatás használatával hello szolgáltatás típusból példányosítható [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.</span><span class="sxs-lookup"><span data-stu-id="0e10c-149">You can instantiate a service from a service type using hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.</span></span>  <span data-ttu-id="0e10c-150">Ha hello szolgáltatás alapértelmezett szolgáltatásként hello alkalmazásjegyzékben van deklarálva, hello szolgáltatás létrejön, amikor hello alkalmazás létrejön.</span><span class="sxs-lookup"><span data-stu-id="0e10c-150">If hello service is declared as a default service in hello application manifest, hello service is instantiated when hello application is instantiated.</span></span>  <span data-ttu-id="0e10c-151">Hívása hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API-t a már létrehozott egy szolgáltatást fog visszaadni a FabricErrorCode.ServiceAlreadyExists értékű hiba kódot tartalmazó FabricException típusú kivételt.</span><span class="sxs-lookup"><span data-stu-id="0e10c-151">Calling hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API for a service that is already instantiated will return an exception of type FabricException containing an error code with a value of FabricErrorCode.ServiceAlreadyExists.</span></span>

## <a name="remove-a-service-instance"></a><span data-ttu-id="0e10c-152">A szolgáltatás példányának eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0e10c-152">Remove a service instance</span></span>
<span data-ttu-id="0e10c-153">Amikor egy szolgáltatáspéldány már nem szükséges, eltávolíthatja azt hívó hello alkalmazáspéldány futó hello [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.</span><span class="sxs-lookup"><span data-stu-id="0e10c-153">When a service instance is no longer needed, you can remove it from hello running application instance by calling hello [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.</span></span>  

> [!WARNING]
> <span data-ttu-id="0e10c-154">Ez a művelet nem vonható vissza, és a szolgáltatás állapota nem állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="0e10c-154">This operation cannot be reversed, and service state cannot be recovered.</span></span>

## <a name="remove-an-application-instance"></a><span data-ttu-id="0e10c-155">Az alkalmazáspéldány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0e10c-155">Remove an application instance</span></span>
<span data-ttu-id="0e10c-156">Az alkalmazáspéldány nincs szükség, ha véglegesen eltávolíthatja azt hello használatával név szerint [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API.</span><span class="sxs-lookup"><span data-stu-id="0e10c-156">When an application instance is no longer needed, you can permanently remove it by name using hello [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API.</span></span> <span data-ttu-id="0e10c-157">[DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automatikusan eltávolítja az is, végleges eltávolítása az összes szolgáltatás állapotának toohello alkalmazáshoz tartozó összes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0e10c-157">[DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automatically removes all services that belong toohello application as well, permanently removing all service state.</span></span>

> [!WARNING]
> <span data-ttu-id="0e10c-158">Ez a művelet nem vonható vissza, és az alkalmazás állapota nem állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="0e10c-158">This operation cannot be reversed, and application state cannot be recovered.</span></span>

## <a name="unregister-an-application-type"></a><span data-ttu-id="0e10c-159">Az alkalmazástípus regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="0e10c-159">Unregister an application type</span></span>
<span data-ttu-id="0e10c-160">Amikor egy alkalmazás típus adott verziójának már nem szükséges, hello alkalmazástípus hello segítségével, hogy adott verziójának kell regisztrációjának törlése [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API.</span><span class="sxs-lookup"><span data-stu-id="0e10c-160">When a particular version of an application type is no longer needed, you should unregister that particular version of hello application type using hello [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API.</span></span> <span data-ttu-id="0e10c-161">Alkalmazás típusú kiadásokban tárolóhely hello lemezképtárolóhoz által használt regisztrációjának megszüntetése nem használt verziók.</span><span class="sxs-lookup"><span data-stu-id="0e10c-161">Unregistering unused versions of application types releases storage space used by hello image store.</span></span> <span data-ttu-id="0e10c-162">Egy alkalmazástípus verziója mindaddig, amíg nincs alkalmazások létrehozásának ellen, hogy hello alkalmazástípus verziója, és nincs függőben lévő alkalmazásfrissítések hivatkoznak, hogy hello alkalmazástípus verziója lehet regisztrációjának törlése.</span><span class="sxs-lookup"><span data-stu-id="0e10c-162">A version of an application type can be unregistered as long as no applications are instantiated against that version of hello application type and no pending application upgrades are referencing that version of hello application type.</span></span>

## <a name="remove-an-application-package-from-hello-image-store"></a><span data-ttu-id="0e10c-163">Az alkalmazáscsomag eltávolítása hello lemezképtárolóhoz</span><span class="sxs-lookup"><span data-stu-id="0e10c-163">Remove an application package from hello image store</span></span>
<span data-ttu-id="0e10c-164">Már nem szükséges egy alkalmazáscsomagot, törölheti azt hello lemezképet tároló toofree rendszer erőforrásait hello segítségével a [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.</span><span class="sxs-lookup"><span data-stu-id="0e10c-164">When an application package is no longer needed, you can delete it from hello image store toofree up system resources using hello [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0e10c-165">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="0e10c-165">Troubleshooting</span></span>
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a><span data-ttu-id="0e10c-166">Másolás-ServiceFabricApplicationPackage kér egy előtaggal</span><span class="sxs-lookup"><span data-stu-id="0e10c-166">Copy-ServiceFabricApplicationPackage asks for an ImageStoreConnectionString</span></span>
<span data-ttu-id="0e10c-167">hello Service Fabric SDK környezet már rendelkezik hello megfelelő alapértelmezett beállítása.</span><span class="sxs-lookup"><span data-stu-id="0e10c-167">hello Service Fabric SDK environment should already have hello correct defaults set up.</span></span> <span data-ttu-id="0e10c-168">De szükség esetén – hello előtaggal összes parancs meg kell felelnie hello értéket, hogy a Service Fabric fürt által használt hello.</span><span class="sxs-lookup"><span data-stu-id="0e10c-168">But if needed, hello ImageStoreConnectionString for all commands should match hello value that hello Service Fabric cluster is using.</span></span> <span data-ttu-id="0e10c-169">Hello előtaggal hello fürtjegyzékben található beolvasott hello segítségével [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) és Get-ImageStoreConnectionStringFromClusterManifest parancsokat:</span><span class="sxs-lookup"><span data-stu-id="0e10c-169">You can find hello ImageStoreConnectionString in hello cluster manifest, retrieved using hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) and Get-ImageStoreConnectionStringFromClusterManifest commands:</span></span>

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

<span data-ttu-id="0e10c-170">Hello **Get-ImageStoreConnectionStringFromClusterManifest** hello Service Fabric SDK PowerShell modul részét képező parancsmag értéke használt tooget hello image kapcsolati karakterláncot tárolni.</span><span class="sxs-lookup"><span data-stu-id="0e10c-170">hello **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, which is part of hello Service Fabric SDK PowerShell module, is used tooget hello image store connection string.</span></span>  <span data-ttu-id="0e10c-171">tooimport hello SDK modul, futtassa:</span><span class="sxs-lookup"><span data-stu-id="0e10c-171">tooimport hello SDK module, run:</span></span>

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


<span data-ttu-id="0e10c-172">hello előtaggal megtalálható a fürtjegyzékben hello:</span><span class="sxs-lookup"><span data-stu-id="0e10c-172">hello ImageStoreConnectionString is found in hello cluster manifest:</span></span>

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

<span data-ttu-id="0e10c-173">Lásd: [hello lemezkép tárolási kapcsolati karakterlánc megértéséhez](service-fabric-image-store-connection-string.md) kiegészítő információkat megtudni hello image store és lemezkép tárolásához kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="0e10c-173">See [Understand hello image store connection string](service-fabric-image-store-connection-string.md) for supplementary information about hello image store and image store connection string.</span></span>

### <a name="deploy-large-application-package"></a><span data-ttu-id="0e10c-174">Nagy alkalmazáscsomag telepítése</span><span class="sxs-lookup"><span data-stu-id="0e10c-174">Deploy large application package</span></span>
<span data-ttu-id="0e10c-175">Probléma: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) a nagy alkalmazáscsomagok (GB-os sorrendben) időtúllépése API.</span><span class="sxs-lookup"><span data-stu-id="0e10c-175">Issue: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API times out for a large application package (order of GB).</span></span>
<span data-ttu-id="0e10c-176">Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="0e10c-176">Try:</span></span>
- <span data-ttu-id="0e10c-177">Adjon meg egy nagyobb időkorlátjának [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) metódus, a `timeout` paraméter.</span><span class="sxs-lookup"><span data-stu-id="0e10c-177">Specify a larger timeout for [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) method, with `timeout` parameter.</span></span> <span data-ttu-id="0e10c-178">Alapértelmezés szerint hello időtúllépés értéke 30 perc.</span><span class="sxs-lookup"><span data-stu-id="0e10c-178">By default, hello timeout is 30 minutes.</span></span>
- <span data-ttu-id="0e10c-179">Ellenőrizze a hello hálózati kapcsolat a forrásgép és fürt között.</span><span class="sxs-lookup"><span data-stu-id="0e10c-179">Check hello network connection between your source machine and cluster.</span></span> <span data-ttu-id="0e10c-180">Ha hello kapcsolat lassú, érdemes lehet a gépek nagyobb hálózati kapcsolattal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="0e10c-180">If hello connection is slow, consider using a machine with a better network connection.</span></span>
<span data-ttu-id="0e10c-181">Ha hello ügyfélszámítógép mint hello fürt egy másik régióban van, érdemes lehet egy ügyfélszámítógépre egy szorosabb vagy ugyanabban a régióban hello fürtként.</span><span class="sxs-lookup"><span data-stu-id="0e10c-181">If hello client machine is in another region than hello cluster, consider using a client machine in a closer or same region as hello cluster.</span></span>
- <span data-ttu-id="0e10c-182">Ellenőrizze, hogy ha hogy elérte-e külső szabályozás.</span><span class="sxs-lookup"><span data-stu-id="0e10c-182">Check if you are hitting external throttling.</span></span> <span data-ttu-id="0e10c-183">Például ha hello lemezképtárolóhoz konfigurált toouse azure tárolási, feltöltés előfordulhat, hogy szabályozva.</span><span class="sxs-lookup"><span data-stu-id="0e10c-183">For example, when hello image store is configured toouse azure storage, upload may be throttled.</span></span>

<span data-ttu-id="0e10c-184">Probléma: Feltöltés csomag sikeresen befejeződött, de [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API túllépi az időkorlátot. Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="0e10c-184">Issue: Upload package completed successfully, but [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API times out. Try:</span></span>
- <span data-ttu-id="0e10c-185">[Hello csomag tömörítése](service-fabric-package-apps.md#compress-a-package) toohello lemezképtárolóhoz másolása előtt.</span><span class="sxs-lookup"><span data-stu-id="0e10c-185">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span>
<span data-ttu-id="0e10c-186">hello tömörítés csökkenti hello és hello fájlok száma, ami viszont hello forgalom mennyiségét csökkenti, és működnek, hogy a Service Fabric kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="0e10c-186">hello compression reduces hello size and hello number of files, which in turn reduces hello amount of traffic and work that Service Fabric must perform.</span></span> <span data-ttu-id="0e10c-187">lehet, hogy hello feltöltési művelet lassabb (különösen ha hello tömörítési idő), de regisztrálása és regisztrációjának hello alkalmazástípus gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="0e10c-187">hello upload operation may be slower (especially if you include hello compression time), but register and un-register hello application type are faster.</span></span>
- <span data-ttu-id="0e10c-188">Adjon meg egy nagyobb időkorlátjának [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API-t `timeout` paraméter.</span><span class="sxs-lookup"><span data-stu-id="0e10c-188">Specify a larger timeout for [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API with `timeout` parameter.</span></span>

### <a name="deploy-application-package-with-many-files"></a><span data-ttu-id="0e10c-189">Sok fájlokkal alkalmazáscsomag telepítése</span><span class="sxs-lookup"><span data-stu-id="0e10c-189">Deploy application package with many files</span></span>
<span data-ttu-id="0e10c-190">Probléma: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) sok fájlt (akár több ezer sorrendben) az alkalmazás csomagot időtúllépése.</span><span class="sxs-lookup"><span data-stu-id="0e10c-190">Issue: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) times out for an application package with many files (order of thousands).</span></span>
<span data-ttu-id="0e10c-191">Próbálja meg:</span><span class="sxs-lookup"><span data-stu-id="0e10c-191">Try:</span></span>
- <span data-ttu-id="0e10c-192">[Hello csomag tömörítése](service-fabric-package-apps.md#compress-a-package) toohello lemezképtárolóhoz másolása előtt.</span><span class="sxs-lookup"><span data-stu-id="0e10c-192">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store.</span></span> <span data-ttu-id="0e10c-193">hello tömörítés csökkenti a fájlok hello száma.</span><span class="sxs-lookup"><span data-stu-id="0e10c-193">hello compression reduces hello number of files.</span></span>
- <span data-ttu-id="0e10c-194">Adjon meg egy nagyobb időkorlátjának [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) a `timeout` paraméter.</span><span class="sxs-lookup"><span data-stu-id="0e10c-194">Specify a larger timeout for [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) with `timeout` parameter.</span></span>

## <a name="code-example"></a><span data-ttu-id="0e10c-195">Mintakód</span><span class="sxs-lookup"><span data-stu-id="0e10c-195">Code example</span></span>
<span data-ttu-id="0e10c-196">hello következő például egy alkalmazás csomag toohello lemezképtárolóhoz másolja hello alkalmazástípus kiépítését, létrehoz egy alkalmazáspéldányt, hoz létre a szolgáltatáspéldány, eltávolítja hello alkalmazáspéldányt, un-rendelkezések hello alkalmazástípust, és hello alkalmazáscsomag törlése hello lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="0e10c-196">hello following example copies an application package toohello image store, provisions hello application type, creates an application instance, creates a service instance, removes hello application instance, un-provisions hello application type, and deletes hello application package from hello image store.</span></span>

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

    // Connect toohello cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy hello application package tooa location in hello image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied too{0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy tooImage Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision hello application.  "MyApplicationV1" is hello folder in hello image store where hello application package is located. 
    // hello application type with name "MyApplicationType" and version "1.0.0" (both are found in hello application manifest) 
    // is now registered in hello cluster.            
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

    //  Create hello application instance.
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

    // Create hello stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create hello service instance.  If hello service is declared as a default service in hello ApplicationManifest.xml,
    // hello service instance is already running and this call will fail.
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

    // Delete an application instance from hello application type.
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

    // Un-provision hello application type.
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

    // Delete hello application package from a location in hello image store.
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

## <a name="next-steps"></a><span data-ttu-id="0e10c-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0e10c-197">Next steps</span></span>
[<span data-ttu-id="0e10c-198">A Service Fabric-alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="0e10c-198">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="0e10c-199">A Service Fabric állapotának bemutatása</span><span class="sxs-lookup"><span data-stu-id="0e10c-199">Service Fabric health introduction</span></span>](service-fabric-health-introduction.md)

[<span data-ttu-id="0e10c-200">Diagnosztizálása és megoldása a Service Fabric-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="0e10c-200">Diagnose and troubleshoot a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="0e10c-201">A Service Fabric alkalmazás minta</span><span class="sxs-lookup"><span data-stu-id="0e10c-201">Model an application in Service Fabric</span></span>](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

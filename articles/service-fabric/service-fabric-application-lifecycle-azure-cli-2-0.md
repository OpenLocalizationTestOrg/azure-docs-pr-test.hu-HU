---
title: "az Azure CLI 2.0 verziót használja aaaManage Azure Service Fabric-alkalmazások"
description: "Ismerje meg, hogyan toodeploy és eltávolítás alkalmazások az Azure Service Fabric fürt által az Azure CLI 2.0 verziót használja."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: ae1ba19513978b0f95ffb65d5f1f7a21ed5f2894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-cli-20"></a><span data-ttu-id="22d25-103">Az Azure Service Fabric-alkalmazás kezelése az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="22d25-103">Manage an Azure Service Fabric application by using Azure CLI 2.0</span></span>

<span data-ttu-id="22d25-104">Megtudhatja, hogyan toocreate és törli az alkalmazást, hogy az Azure Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="22d25-104">Learn how toocreate and delete applications that are running in an Azure Service Fabric cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22d25-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="22d25-105">Prerequisites</span></span>

* <span data-ttu-id="22d25-106">Az Azure CLI 2.0 telepítése.</span><span class="sxs-lookup"><span data-stu-id="22d25-106">Install Azure CLI 2.0.</span></span> <span data-ttu-id="22d25-107">Ezután válassza ki a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="22d25-107">Then, select your Service Fabric cluster.</span></span> <span data-ttu-id="22d25-108">További információkért lásd: [Ismerkedés az Azure CLI 2.0](service-fabric-azure-cli-2-0.md).</span><span class="sxs-lookup"><span data-stu-id="22d25-108">For more information, see [Get started with Azure CLI 2.0](service-fabric-azure-cli-2-0.md).</span></span>

* <span data-ttu-id="22d25-109">A Service Fabric application csomag készen toobe telepítve van.</span><span class="sxs-lookup"><span data-stu-id="22d25-109">Have a Service Fabric application package ready toobe deployed.</span></span> <span data-ttu-id="22d25-110">További információ tooauthor és a csomag egy alkalmazást, olvassa el hello [Service Fabric-alkalmazás modell](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="22d25-110">For more information about how tooauthor and package an application, read about hello [Service Fabric application model](service-fabric-application-model.md).</span></span>

## <a name="overview"></a><span data-ttu-id="22d25-111">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="22d25-111">Overview</span></span>

<span data-ttu-id="22d25-112">toodeploy egy új alkalmazást, végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="22d25-112">toodeploy a new application, complete these steps:</span></span>

1. <span data-ttu-id="22d25-113">Töltse fel az alkalmazás csomag toohello Service Fabric lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="22d25-113">Upload an application package toohello Service Fabric image store.</span></span>
2. <span data-ttu-id="22d25-114">Az alkalmazástípus kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="22d25-114">Provision an application type.</span></span>
3. <span data-ttu-id="22d25-115">Adja meg, és hozzon létre egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="22d25-115">Specify and create an application.</span></span>
4. <span data-ttu-id="22d25-116">Adja meg, és a szolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="22d25-116">Specify and create services.</span></span>

<span data-ttu-id="22d25-117">tooremove egy meglévő alkalmazást, kövesse ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="22d25-117">tooremove an existing application, complete these steps:</span></span>

1. <span data-ttu-id="22d25-118">Hello alkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="22d25-118">Delete hello application.</span></span>
2. <span data-ttu-id="22d25-119">Leépíteni hello társított alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="22d25-119">Unprovision hello associated application type.</span></span>
3. <span data-ttu-id="22d25-120">Hello lemezképet tároló tartalom törlése.</span><span class="sxs-lookup"><span data-stu-id="22d25-120">Delete hello image store content.</span></span>

## <a name="deploy-a-new-application"></a><span data-ttu-id="22d25-121">Egy új alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="22d25-121">Deploy a new application</span></span>

<span data-ttu-id="22d25-122">egy új alkalmazást, a következő teljes hello toodeploy feladatok.</span><span class="sxs-lookup"><span data-stu-id="22d25-122">toodeploy a new application, complete hello following tasks.</span></span>

### <a name="upload-a-new-application-package-toohello-image-store"></a><span data-ttu-id="22d25-123">Egy új alkalmazás csomag toohello lemezképtárolóhoz feltöltése</span><span class="sxs-lookup"><span data-stu-id="22d25-123">Upload a new application package toohello image store</span></span>

<span data-ttu-id="22d25-124">Az alkalmazás létrehozása előtt töltse fel a hello alkalmazás csomag toohello Service Fabric lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="22d25-124">Before you create an application, upload hello application package toohello Service Fabric image store.</span></span> 

<span data-ttu-id="22d25-125">Például, ha a alkalmazáscsomag hello `app_package_dir` directory, a következő parancsok tooupload hello directory használata hello:</span><span class="sxs-lookup"><span data-stu-id="22d25-125">For example, if your application package is in hello `app_package_dir` directory, use hello following commands tooupload hello directory:</span></span>

```azurecli
az sf application upload --path ~/app_package_dir
```

<span data-ttu-id="22d25-126">Nagy alkalmazáscsomagok esetén megadhatja hello `--show-progress` toodisplay hello előrehaladását hello feltöltés lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="22d25-126">For large application packages, you can specify hello `--show-progress` option toodisplay hello progress of hello upload.</span></span>

### <a name="provision-hello-application-type"></a><span data-ttu-id="22d25-127">Kiépítés hello alkalmazás típusa</span><span class="sxs-lookup"><span data-stu-id="22d25-127">Provision hello application type</span></span>

<span data-ttu-id="22d25-128">Hello feltöltés befejezését követően használhatók a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="22d25-128">When hello upload is finished, provision hello application.</span></span> <span data-ttu-id="22d25-129">tooprovision hello alkalmazás, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="22d25-129">tooprovision hello application, use hello following command:</span></span>

```azurecli
az sf application provision --application-type-build-path app_package_dir
```

<span data-ttu-id="22d25-130">a következő hello `application-type-build-path` hello neve, ahol feltöltött alkalmazáscsomag hello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="22d25-130">hello value for `application-type-build-path` is hello name of hello directory where you uploaded your application package.</span></span>

### <a name="create-an-application-from-an-application-type"></a><span data-ttu-id="22d25-131">Alkalmazástípusok az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="22d25-131">Create an application from an application type</span></span>

<span data-ttu-id="22d25-132">Hello alkalmazás kiépítése után a következő parancs tooname hello használja, és az alkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="22d25-132">After you provision hello application, use hello following command tooname and create your application:</span></span>

```azurecli
az sf application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

<span data-ttu-id="22d25-133">`app-name`hello nevét, amelyet az toouse hello alkalmazás példány van.</span><span class="sxs-lookup"><span data-stu-id="22d25-133">`app-name` is hello name that you want toouse for hello application instance.</span></span> <span data-ttu-id="22d25-134">További paraméterek az előzőleg létrehozott alkalmazásjegyzék hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="22d25-134">You can get additional parameters from hello previously provisioned application manifest.</span></span>

<span data-ttu-id="22d25-135">hello alkalmazásnév hello előtaggal kell kezdődnie `fabric:/`.</span><span class="sxs-lookup"><span data-stu-id="22d25-135">hello application name must start with hello prefix `fabric:/`.</span></span>

### <a name="create-services-for-hello-new-application"></a><span data-ttu-id="22d25-136">Szolgáltatások hello új alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="22d25-136">Create services for hello new application</span></span>

<span data-ttu-id="22d25-137">Miután létrehozott egy alkalmazást, a szolgáltatások létrehozására hello alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="22d25-137">After you have created an application, create services from hello application.</span></span> <span data-ttu-id="22d25-138">A következő példa hello hogy hozzon létre egy új állapot nélküli az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="22d25-138">In hello following example, we create a new stateless service from our application.</span></span> <span data-ttu-id="22d25-139">hello szolgáltatásokat hozhat létre egy alkalmazást a korábban kiépített alkalmazáscsomag hello szolgáltatás jegyzékfájl vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="22d25-139">hello services that you can create from an application are defined in a service manifest in hello previously provisioned application package.</span></span>

```azurecli
az sf service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a><span data-ttu-id="22d25-140">Alkalmazások üzembe helyezését és állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="22d25-140">Verify application deployment and health</span></span>

<span data-ttu-id="22d25-141">tooverify, hogy egy alkalmazás és szolgáltatás sikeresen telepített, ellenőrizze, hogy hello alkalmazás és szolgáltatás szerepel:</span><span class="sxs-lookup"><span data-stu-id="22d25-141">tooverify that an application and service were successfully deployed, check that hello application and service are listed:</span></span>

```azurecli
az sf application list
az sf service list --application-list TestApp
```

<span data-ttu-id="22d25-142">tooverify, hogy hello szolgáltatás állapota megfelelő, hasonló parancsok tooretrieve hello állapotfigyelő hello szolgáltatás és alkalmazás hello használata:</span><span class="sxs-lookup"><span data-stu-id="22d25-142">tooverify that hello service is healthy, use similar commands tooretrieve hello health of both hello service and hello application:</span></span>

```azurecli
az sf application health --application-id TestApp
az sf service health --service-id TestApp/TestSvc
```

<span data-ttu-id="22d25-143">Kifogástalan szolgáltatások és alkalmazások egy `HealthState` értékének `Ok`.</span><span class="sxs-lookup"><span data-stu-id="22d25-143">Healthy services and applications have a `HealthState` value of `Ok`.</span></span>

## <a name="remove-an-existing-application"></a><span data-ttu-id="22d25-144">Távolítsa el a meglévő alkalmazás</span><span class="sxs-lookup"><span data-stu-id="22d25-144">Remove an existing application</span></span>

<span data-ttu-id="22d25-145">tooremove egy alkalmazást, a következő feladatok teljes hello.</span><span class="sxs-lookup"><span data-stu-id="22d25-145">tooremove an application, complete hello following tasks.</span></span>

### <a name="delete-hello-application"></a><span data-ttu-id="22d25-146">Hello alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="22d25-146">Delete hello application</span></span>

<span data-ttu-id="22d25-147">toodelete hello alkalmazás, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="22d25-147">toodelete hello application, use hello following command:</span></span>

```azurecli
az sf application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a><span data-ttu-id="22d25-148">Leépíteni a következőt: hello alkalmazás típusa</span><span class="sxs-lookup"><span data-stu-id="22d25-148">Unprovision hello application type</span></span>

<span data-ttu-id="22d25-149">Hello alkalmazás törlése után is leépítése hello alkalmazás típusa, ha már nincs szüksége.</span><span class="sxs-lookup"><span data-stu-id="22d25-149">After you delete hello application, you can unprovision hello application type if you no longer need it.</span></span> <span data-ttu-id="22d25-150">toounprovision hello alkalmazástípusok, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="22d25-150">toounprovision hello application type, use hello following command:</span></span>

```azurecli
az sf application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

<span data-ttu-id="22d25-151">hello típus nevét és típusát meg kell egyeznie hello neve és verziója hello korábban kiosztott alkalmazásjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="22d25-151">hello type name and type version must match hello name and version in hello previously provisioned application manifest.</span></span>

### <a name="delete-hello-application-package"></a><span data-ttu-id="22d25-152">Hello alkalmazáscsomag törlése</span><span class="sxs-lookup"><span data-stu-id="22d25-152">Delete hello application package</span></span>

<span data-ttu-id="22d25-153">Miután hello alkalmazástípus rendelkezik leépítette a következő, törölheti hello alkalmazáscsomag hello lemezképtárolóhoz Ha már nincs szüksége.</span><span class="sxs-lookup"><span data-stu-id="22d25-153">After you have unprovisioned hello application type, you can delete hello application package from hello image store if you no longer need it.</span></span> <span data-ttu-id="22d25-154">Alkalmazás csomagok törlésével szabadítson fel lemezterületet segítségével.</span><span class="sxs-lookup"><span data-stu-id="22d25-154">Deleting application packages helps reclaim disk space.</span></span> 

<span data-ttu-id="22d25-155">hello-alkalmazáscsomag toodelete hello kép Store áruházból, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="22d25-155">toodelete hello application package from hello image store, use hello following command:</span></span>

```azurecli
az sf application package-delete --content-path app_package_dir
```

<span data-ttu-id="22d25-156">`content-path`hello directory hello alkalmazás létrehozásakor feltöltött hello nevének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="22d25-156">`content-path` must be hello name of hello directory that you uploaded when you created hello application.</span></span>

## <a name="related-articles"></a><span data-ttu-id="22d25-157">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="22d25-157">Related articles</span></span>

* [<span data-ttu-id="22d25-158">A Service Fabric és az Azure CLI 2.0 használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="22d25-158">Get started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
* [<span data-ttu-id="22d25-159">A Service Fabric XPlat parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="22d25-159">Get started with Service Fabric XPlat CLI</span></span>](service-fabric-azure-cli.md)

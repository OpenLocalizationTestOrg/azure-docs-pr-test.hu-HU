---
title: "aaaManage Azure Service Fabric-alkalmazások Azure Service Fabric parancssori felület használatával"
description: "Ismerje meg, hogyan toodeploy és eltávolítás alkalmazások az Azure Service Fabric fürt Azure Service Fabric parancssori felület használatával"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: d9f98cee1d70f71a2aab68ff556956619910e4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli"></a><span data-ttu-id="5deec-103">Az Azure Service Fabric-alkalmazás kezelése az Azure Service Fabric parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="5deec-103">Manage an Azure Service Fabric application by using Azure Service Fabric CLI</span></span>

<span data-ttu-id="5deec-104">Megtudhatja, hogyan toocreate és törli az alkalmazást, hogy az Azure Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="5deec-104">Learn how toocreate and delete applications that are running in an Azure Service Fabric cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5deec-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5deec-105">Prerequisites</span></span>

* <span data-ttu-id="5deec-106">A Service Fabric parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="5deec-106">Install Service Fabric CLI.</span></span> <span data-ttu-id="5deec-107">Ezután válassza ki a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="5deec-107">Then, select your Service Fabric cluster.</span></span> <span data-ttu-id="5deec-108">További információkért lásd: [Ismerkedés a Service Fabric CLI](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5deec-108">For more information, see [Get started with Service Fabric CLI](service-fabric-cli.md).</span></span>

* <span data-ttu-id="5deec-109">A Service Fabric application csomag készen toobe telepítve van.</span><span class="sxs-lookup"><span data-stu-id="5deec-109">Have a Service Fabric application package ready toobe deployed.</span></span> <span data-ttu-id="5deec-110">További információ tooauthor és a csomag egy alkalmazást, olvassa el hello [Service Fabric-alkalmazás modell](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="5deec-110">For more information about how tooauthor and package an application, read about hello [Service Fabric application model](service-fabric-application-model.md).</span></span>

## <a name="overview"></a><span data-ttu-id="5deec-111">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5deec-111">Overview</span></span>

<span data-ttu-id="5deec-112">toodeploy egy új alkalmazást, végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5deec-112">toodeploy a new application, complete these steps:</span></span>

1. <span data-ttu-id="5deec-113">Töltse fel az alkalmazás csomag toohello Service Fabric lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="5deec-113">Upload an application package toohello Service Fabric image store.</span></span>
2. <span data-ttu-id="5deec-114">Az alkalmazástípus kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5deec-114">Provision an application type.</span></span>
3. <span data-ttu-id="5deec-115">Adja meg, és hozzon létre egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5deec-115">Specify and create an application.</span></span>
4. <span data-ttu-id="5deec-116">Adja meg, és a szolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="5deec-116">Specify and create services.</span></span>

<span data-ttu-id="5deec-117">tooremove egy meglévő alkalmazást, kövesse ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5deec-117">tooremove an existing application, complete these steps:</span></span>

1. <span data-ttu-id="5deec-118">Hello alkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="5deec-118">Delete hello application.</span></span>
2. <span data-ttu-id="5deec-119">Leépíteni hello társított alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="5deec-119">Unprovision hello associated application type.</span></span>
3. <span data-ttu-id="5deec-120">Hello lemezképet tároló tartalom törlése.</span><span class="sxs-lookup"><span data-stu-id="5deec-120">Delete hello image store content.</span></span>

## <a name="deploy-a-new-application"></a><span data-ttu-id="5deec-121">Egy új alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5deec-121">Deploy a new application</span></span>

<span data-ttu-id="5deec-122">toodeploy egy új alkalmazást, a következő feladatok teljes hello:</span><span class="sxs-lookup"><span data-stu-id="5deec-122">toodeploy a new application, complete hello following tasks:</span></span>

### <a name="upload-a-new-application-package-toohello-image-store"></a><span data-ttu-id="5deec-123">Egy új alkalmazás csomag toohello lemezképtárolóhoz feltöltése</span><span class="sxs-lookup"><span data-stu-id="5deec-123">Upload a new application package toohello image store</span></span>

<span data-ttu-id="5deec-124">Az alkalmazás létrehozása előtt töltse fel a hello alkalmazás csomag toohello Service Fabric lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="5deec-124">Before you create an application, upload hello application package toohello Service Fabric image store.</span></span>

<span data-ttu-id="5deec-125">Például, ha a alkalmazáscsomag hello `app_package_dir` directory, a következő parancsok tooupload hello directory használata hello:</span><span class="sxs-lookup"><span data-stu-id="5deec-125">For example, if your application package is in hello `app_package_dir` directory, use hello following commands tooupload hello directory:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir
```

<span data-ttu-id="5deec-126">Nagy alkalmazáscsomagok esetén megadhatja hello `--show-progress` toodisplay hello előrehaladását hello feltöltés lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5deec-126">For large application packages, you can specify hello `--show-progress` option toodisplay hello progress of hello upload.</span></span>

### <a name="provision-hello-application-type"></a><span data-ttu-id="5deec-127">Kiépítés hello alkalmazás típusa</span><span class="sxs-lookup"><span data-stu-id="5deec-127">Provision hello application type</span></span>

<span data-ttu-id="5deec-128">Hello feltöltés befejezését követően használhatók a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5deec-128">When hello upload is finished, provision hello application.</span></span> <span data-ttu-id="5deec-129">tooprovision hello alkalmazás, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="5deec-129">tooprovision hello application, use hello following command:</span></span>

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

<span data-ttu-id="5deec-130">a következő hello `application-type-build-path` hello neve, ahol feltöltött alkalmazáscsomag hello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="5deec-130">hello value for `application-type-build-path` is hello name of hello directory where you uploaded your application package.</span></span>

### <a name="create-an-application-from-an-application-type"></a><span data-ttu-id="5deec-131">Alkalmazástípusok az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5deec-131">Create an application from an application type</span></span>

<span data-ttu-id="5deec-132">Hello alkalmazás kiépítése után a következő parancs tooname hello használja, és az alkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="5deec-132">After you provision hello application, use hello following command tooname and create your application:</span></span>

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

<span data-ttu-id="5deec-133">`app-name`hello nevét, amelyet az toouse hello alkalmazás példány van.</span><span class="sxs-lookup"><span data-stu-id="5deec-133">`app-name` is hello name that you want toouse for hello application instance.</span></span> <span data-ttu-id="5deec-134">További paraméterek lekérheti az előzőleg létrehozott alkalmazás jegyzékében.</span><span class="sxs-lookup"><span data-stu-id="5deec-134">You can get additional parameters from the previously provisioned application manifest.</span></span>

<span data-ttu-id="5deec-135">hello alkalmazásnév hello előtaggal kell kezdődnie `fabric:/`.</span><span class="sxs-lookup"><span data-stu-id="5deec-135">hello application name must start with hello prefix `fabric:/`.</span></span>

### <a name="create-services-for-hello-new-application"></a><span data-ttu-id="5deec-136">Szolgáltatások hello új alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5deec-136">Create services for hello new application</span></span>

<span data-ttu-id="5deec-137">Miután létrehozott egy alkalmazást, a szolgáltatások létrehozására hello alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="5deec-137">After you have created an application, create services from hello application.</span></span> <span data-ttu-id="5deec-138">A következő példa hello hogy hozzon létre egy új állapot nélküli az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="5deec-138">In hello following example, we create a new stateless service from our application.</span></span> <span data-ttu-id="5deec-139">hello szolgáltatásokat hozhat létre egy alkalmazást a korábban kiépített alkalmazáscsomag hello szolgáltatás jegyzékfájl vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="5deec-139">hello services that you can create from an application are defined in a service manifest in hello previously provisioned application package.</span></span>

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a><span data-ttu-id="5deec-140">Alkalmazások üzembe helyezését és állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5deec-140">Verify application deployment and health</span></span>

<span data-ttu-id="5deec-141">tooverify mindent kifogástalan állapotú, használja a következő állapotfigyelő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="5deec-141">tooverify everything is healthy, use hello following health commands:</span></span>

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

<span data-ttu-id="5deec-142">tooverify, hogy hello szolgáltatás állapota megfelelő, hasonló parancsok tooretrieve hello állapotát a hello szolgáltatás, és az alkalmazás használata:</span><span class="sxs-lookup"><span data-stu-id="5deec-142">tooverify that hello service is healthy, use similar commands tooretrieve hello health of both hello service and the application:</span></span>

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

<span data-ttu-id="5deec-143">Kifogástalan szolgáltatások és alkalmazások egy `HealthState` értékének `Ok`.</span><span class="sxs-lookup"><span data-stu-id="5deec-143">Healthy services and applications have a `HealthState` value of `Ok`.</span></span>

## <a name="remove-an-existing-application"></a><span data-ttu-id="5deec-144">Távolítsa el a meglévő alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5deec-144">Remove an existing application</span></span>

<span data-ttu-id="5deec-145">tooremove egy alkalmazást, a következő feladatok teljes hello:</span><span class="sxs-lookup"><span data-stu-id="5deec-145">tooremove an application, complete hello following tasks:</span></span>

### <a name="delete-hello-application"></a><span data-ttu-id="5deec-146">Hello alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="5deec-146">Delete hello application</span></span>

<span data-ttu-id="5deec-147">toodelete hello alkalmazás, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="5deec-147">toodelete hello application, use hello following command:</span></span>

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a><span data-ttu-id="5deec-148">Leépíteni a következőt: hello alkalmazás típusa</span><span class="sxs-lookup"><span data-stu-id="5deec-148">Unprovision hello application type</span></span>

<span data-ttu-id="5deec-149">Hello alkalmazás törlése után is leépítése hello alkalmazás típusa, ha már nincs szüksége.</span><span class="sxs-lookup"><span data-stu-id="5deec-149">After you delete hello application, you can unprovision hello application type if you no longer need it.</span></span> <span data-ttu-id="5deec-150">toounprovision hello alkalmazástípusok, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="5deec-150">toounprovision hello application type, use hello following command:</span></span>

```azurecli
sfctl application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

<span data-ttu-id="5deec-151">hello típus nevét és típusát meg kell egyeznie hello neve és verziója hello korábban kiosztott alkalmazásjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="5deec-151">hello type name and type version must match hello name and version in hello previously provisioned application manifest.</span></span>

### <a name="delete-hello-application-package"></a><span data-ttu-id="5deec-152">Hello alkalmazáscsomag törlése</span><span class="sxs-lookup"><span data-stu-id="5deec-152">Delete hello application package</span></span>

<span data-ttu-id="5deec-153">Miután hello alkalmazástípus rendelkezik leépítette a következő, törölheti hello alkalmazáscsomag hello lemezképtárolóhoz Ha már nincs szüksége.</span><span class="sxs-lookup"><span data-stu-id="5deec-153">After you have unprovisioned hello application type, you can delete hello application package from hello image store if you no longer need it.</span></span> <span data-ttu-id="5deec-154">Alkalmazás csomagok törlésével szabadítson fel lemezterületet segítségével.</span><span class="sxs-lookup"><span data-stu-id="5deec-154">Deleting application packages helps reclaim disk space.</span></span> 

<span data-ttu-id="5deec-155">hello-alkalmazáscsomag toodelete hello kép Store áruházból, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="5deec-155">toodelete hello application package from hello image store, use hello following command:</span></span>

```azurecli
sfctl store delete --content-path app_package_dir
```

<span data-ttu-id="5deec-156">`content-path`hello directory hello alkalmazás létrehozásakor feltöltött hello nevének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5deec-156">`content-path` must be hello name of hello directory that you uploaded when you created hello application.</span></span>

## <a name="upgrade-application"></a><span data-ttu-id="5deec-157">Alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="5deec-157">Upgrade application</span></span>

<span data-ttu-id="5deec-158">Miután létrehozta az alkalmazást, megismételhető hello ugyanazokat a lépéseket tooprovision az alkalmazás egy másik verziója.</span><span class="sxs-lookup"><span data-stu-id="5deec-158">After creating your application, you can repeat hello same set of steps tooprovision a second version of your application.</span></span> <span data-ttu-id="5deec-159">Ezt követően a Service Fabric-alkalmazás frissítését dönthet, hogy átvált toorunning hello második hello alkalmazás verziója.</span><span class="sxs-lookup"><span data-stu-id="5deec-159">Then, with a Service Fabric application upgrade you can transition toorunning hello second version of hello application.</span></span> <span data-ttu-id="5deec-160">További információ: hello dokumentációja a [Service Fabric az alkalmazásfrissítéseket](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="5deec-160">For more information, see hello documentation on [Service Fabric application upgrades](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="5deec-161">első rendelkezés hello következő verziójára hello alkalmazás használja a frissítés tooperform azt korábban hello ugyanazokat a parancsokat:</span><span class="sxs-lookup"><span data-stu-id="5deec-161">tooperform an upgrade, first provision hello next version of hello application using hello same commands as before:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
```

<span data-ttu-id="5deec-162">Javasoljuk, majd tooperform egy figyelt automatikus frissítését, indítsa el a hello frissítés hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="5deec-162">It is recommended then tooperform a monitored automatic upgrade, launch hello upgrade by running hello following command:</span></span>

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

<span data-ttu-id="5deec-163">Frissítések bírálja felül a meglévő paramétereket bármilyen készlet van megadva.</span><span class="sxs-lookup"><span data-stu-id="5deec-163">Upgrades override existing parameters with whatever set is specified.</span></span> <span data-ttu-id="5deec-164">Alkalmazás paraméterei kell adhatók át argumentumok toohello frissítési parancs, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="5deec-164">Application parameters should be passed as arguments toohello upgrade command, if necessary.</span></span> <span data-ttu-id="5deec-165">Alkalmazás paraméterei kell kódolható egy JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="5deec-165">Application parameters should be encoded as a JSON object.</span></span>

<span data-ttu-id="5deec-166">tooretrieve minden korábban megadott paraméterek, használhatja a hello `sfctl application info` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5deec-166">tooretrieve any parameters previously specified, you can use hello `sfctl application info` command.</span></span>

<span data-ttu-id="5deec-167">Ha egy alkalmazás frissítése folyamatban van, hello állapot lekérhető használatával a `sfctl application upgrade-status` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5deec-167">When an application upgrade is in progress, hello status can be retrieved using the `sfctl application upgrade-status` command.</span></span>

<span data-ttu-id="5deec-168">Végül, ha a frissítés folyamatban van, és meg kell szakítani toobe, használhat hello `sfctl application upgrade-rollback` tooroll vissza hello frissítését.</span><span class="sxs-lookup"><span data-stu-id="5deec-168">Finally, if an upgrade is in progress and needs toobe canceled, you can use hello `sfctl application upgrade-rollback` tooroll back hello upgrade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5deec-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5deec-169">Next steps</span></span>

* [<span data-ttu-id="5deec-170">Service Fabric CLI alapjai</span><span class="sxs-lookup"><span data-stu-id="5deec-170">Service Fabric CLI basics</span></span>](service-fabric-cli.md)
* [<span data-ttu-id="5deec-171">A Service Fabric Linux első lépések</span><span class="sxs-lookup"><span data-stu-id="5deec-171">Getting started with Service Fabric on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="5deec-172">A Service Fabric-alkalmazás frissítés indítása</span><span class="sxs-lookup"><span data-stu-id="5deec-172">Launching a Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

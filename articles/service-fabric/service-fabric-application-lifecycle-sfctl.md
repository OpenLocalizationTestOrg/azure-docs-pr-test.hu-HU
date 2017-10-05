---
title: "Azure Service Fabric parancssori felület használatával Azure Service Fabric-alkalmazások kezelése"
description: "Ismerje meg, hogyan helyezhet üzembe és alkalmazások eltávolítása egy Azure Service Fabric-fürt Azure Service Fabric parancssori felület használatával"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: c3a2eb3e6e54f952ef963bb2a0292d9ad7b53bc5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli"></a><span data-ttu-id="5255a-103">Az Azure Service Fabric-alkalmazás kezelése az Azure Service Fabric parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="5255a-103">Manage an Azure Service Fabric application by using Azure Service Fabric CLI</span></span>

<span data-ttu-id="5255a-104">Ismerje meg, hogyan hozhat létre, és törölnie kell az Azure Service Fabric-fürt futó alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="5255a-104">Learn how to create and delete applications that are running in an Azure Service Fabric cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5255a-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5255a-105">Prerequisites</span></span>

* <span data-ttu-id="5255a-106">A Service Fabric parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="5255a-106">Install Service Fabric CLI.</span></span> <span data-ttu-id="5255a-107">Ezután válassza ki a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="5255a-107">Then, select your Service Fabric cluster.</span></span> <span data-ttu-id="5255a-108">További információkért lásd: [Ismerkedés a Service Fabric CLI](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5255a-108">For more information, see [Get started with Service Fabric CLI](service-fabric-cli.md).</span></span>

* <span data-ttu-id="5255a-109">A Service Fabric alkalmazáscsomagok telepítésre kész rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5255a-109">Have a Service Fabric application package ready to be deployed.</span></span> <span data-ttu-id="5255a-110">Szerző és csomagot alkalmazássá kapcsolatos további információkért olvassa el a [Service Fabric-alkalmazás modell](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="5255a-110">For more information about how to author and package an application, read about the [Service Fabric application model](service-fabric-application-model.md).</span></span>

## <a name="overview"></a><span data-ttu-id="5255a-111">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5255a-111">Overview</span></span>

<span data-ttu-id="5255a-112">Egy új alkalmazás központi telepítése, végezze el ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5255a-112">To deploy a new application, complete these steps:</span></span>

1. <span data-ttu-id="5255a-113">Alkalmazáscsomag feltöltése a Service Fabric lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="5255a-113">Upload an application package to the Service Fabric image store.</span></span>
2. <span data-ttu-id="5255a-114">Az alkalmazástípus kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5255a-114">Provision an application type.</span></span>
3. <span data-ttu-id="5255a-115">Adja meg, és hozzon létre egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5255a-115">Specify and create an application.</span></span>
4. <span data-ttu-id="5255a-116">Adja meg, és a szolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="5255a-116">Specify and create services.</span></span>

<span data-ttu-id="5255a-117">Távolítsa el a meglévő alkalmazás, végezze el ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5255a-117">To remove an existing application, complete these steps:</span></span>

1. <span data-ttu-id="5255a-118">Az alkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="5255a-118">Delete the application.</span></span>
2. <span data-ttu-id="5255a-119">A társított alkalmazások típus leépítése.</span><span class="sxs-lookup"><span data-stu-id="5255a-119">Unprovision the associated application type.</span></span>
3. <span data-ttu-id="5255a-120">A lemezképet tároló tartalom törlése.</span><span class="sxs-lookup"><span data-stu-id="5255a-120">Delete the image store content.</span></span>

## <a name="deploy-a-new-application"></a><span data-ttu-id="5255a-121">Egy új alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5255a-121">Deploy a new application</span></span>

<span data-ttu-id="5255a-122">Új alkalmazás központi telepítéséhez hajtsa végre a következőket:</span><span class="sxs-lookup"><span data-stu-id="5255a-122">To deploy a new application, complete the following tasks:</span></span>

### <a name="upload-a-new-application-package-to-the-image-store"></a><span data-ttu-id="5255a-123">Az image store egy új alkalmazáscsomag feltöltése</span><span class="sxs-lookup"><span data-stu-id="5255a-123">Upload a new application package to the image store</span></span>

<span data-ttu-id="5255a-124">Hoz létre egy alkalmazást, mielőtt az alkalmazáscsomag feltöltése a Service Fabric lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="5255a-124">Before you create an application, upload the application package to the Service Fabric image store.</span></span>

<span data-ttu-id="5255a-125">Például, ha az alkalmazás csomagot a `app_package_dir` könyvtár, töltse fel a könyvtár a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="5255a-125">For example, if your application package is in the `app_package_dir` directory, use the following commands to upload the directory:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir
```

<span data-ttu-id="5255a-126">Nagy alkalmazáscsomagok esetén megadhatja a `--show-progress` a feltöltés állapotának megjelenítése lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5255a-126">For large application packages, you can specify the `--show-progress` option to display the progress of the upload.</span></span>

### <a name="provision-the-application-type"></a><span data-ttu-id="5255a-127">Az alkalmazástípus kiépítése</span><span class="sxs-lookup"><span data-stu-id="5255a-127">Provision the application type</span></span>

<span data-ttu-id="5255a-128">A feltöltés befejeződése után telepíteni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5255a-128">When the upload is finished, provision the application.</span></span> <span data-ttu-id="5255a-129">Az alkalmazás telepítéséhez, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="5255a-129">To provision the application, use the following command:</span></span>

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

<span data-ttu-id="5255a-130">A következő `application-type-build-path` a könyvtárban, ahol az alkalmazáscsomag feltöltött neve.</span><span class="sxs-lookup"><span data-stu-id="5255a-130">The value for `application-type-build-path` is the name of the directory where you uploaded your application package.</span></span>

### <a name="create-an-application-from-an-application-type"></a><span data-ttu-id="5255a-131">Alkalmazástípusok az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5255a-131">Create an application from an application type</span></span>

<span data-ttu-id="5255a-132">Az alkalmazás kiépítése után a következő paranccsal nevet, és az alkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="5255a-132">After you provision the application, use the following command to name and create your application:</span></span>

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

<span data-ttu-id="5255a-133">`app-name`Ez a név az alkalmazáspéldány használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="5255a-133">`app-name` is the name that you want to use for the application instance.</span></span> <span data-ttu-id="5255a-134">További paraméterek lekérheti az előzőleg létrehozott alkalmazás jegyzékében.</span><span class="sxs-lookup"><span data-stu-id="5255a-134">You can get additional parameters from the previously provisioned application manifest.</span></span>

<span data-ttu-id="5255a-135">Az alkalmazás nevét a előtaggal kell kezdődnie `fabric:/`.</span><span class="sxs-lookup"><span data-stu-id="5255a-135">The application name must start with the prefix `fabric:/`.</span></span>

### <a name="create-services-for-the-new-application"></a><span data-ttu-id="5255a-136">Szolgáltatások az új alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5255a-136">Create services for the new application</span></span>

<span data-ttu-id="5255a-137">Miután létrehozott egy alkalmazást, a szolgáltatások létrehozására az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="5255a-137">After you have created an application, create services from the application.</span></span> <span data-ttu-id="5255a-138">A következő példában azt hozzon létre egy új állapotmentes az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="5255a-138">In the following example, we create a new stateless service from our application.</span></span> <span data-ttu-id="5255a-139">A szolgáltatásokat, amelyeket létrehozhat egy alkalmazást a korábban kiépített alkalmazáscsomagban szolgáltatás jegyzékfájl vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="5255a-139">The services that you can create from an application are defined in a service manifest in the previously provisioned application package.</span></span>

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a><span data-ttu-id="5255a-140">Alkalmazások üzembe helyezését és állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5255a-140">Verify application deployment and health</span></span>

<span data-ttu-id="5255a-141">Minden épségének ellenőrzése, hogy az alábbi parancsokkal állapotát:</span><span class="sxs-lookup"><span data-stu-id="5255a-141">To verify everything is healthy, use the following health commands:</span></span>

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

<span data-ttu-id="5255a-142">Győződjön meg arról, hogy a szolgáltatás nem működik megfelelően, hasonló parancsok segítségével a szolgáltatás és az alkalmazás állapotának beolvasása:</span><span class="sxs-lookup"><span data-stu-id="5255a-142">To verify that the service is healthy, use similar commands to retrieve the health of both the service and the application:</span></span>

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

<span data-ttu-id="5255a-143">Kifogástalan szolgáltatások és alkalmazások egy `HealthState` értékének `Ok`.</span><span class="sxs-lookup"><span data-stu-id="5255a-143">Healthy services and applications have a `HealthState` value of `Ok`.</span></span>

## <a name="remove-an-existing-application"></a><span data-ttu-id="5255a-144">Távolítsa el a meglévő alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5255a-144">Remove an existing application</span></span>

<span data-ttu-id="5255a-145">Az alkalmazás eltávolításához hajtsa végre a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="5255a-145">To remove an application, complete the following tasks:</span></span>

### <a name="delete-the-application"></a><span data-ttu-id="5255a-146">Az alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="5255a-146">Delete the application</span></span>

<span data-ttu-id="5255a-147">Az alkalmazás törléséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="5255a-147">To delete the application, use the following command:</span></span>

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a><span data-ttu-id="5255a-148">Az alkalmazástípus leépíteni a következőt:</span><span class="sxs-lookup"><span data-stu-id="5255a-148">Unprovision the application type</span></span>

<span data-ttu-id="5255a-149">Az alkalmazás törlése után is leválasztja az alkalmazástípus, ha már nincs szüksége.</span><span class="sxs-lookup"><span data-stu-id="5255a-149">After you delete the application, you can unprovision the application type if you no longer need it.</span></span> <span data-ttu-id="5255a-150">Az alkalmazástípus leépítése, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="5255a-150">To unprovision the application type, use the following command:</span></span>

```azurecli
sfctl application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

<span data-ttu-id="5255a-151">A típus neve és típusa verziója meg kell egyeznie a nevét és verzióját az előzőleg létrehozott alkalmazásjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="5255a-151">The type name and type version must match the name and version in the previously provisioned application manifest.</span></span>

### <a name="delete-the-application-package"></a><span data-ttu-id="5255a-152">Az alkalmazáscsomag törlése</span><span class="sxs-lookup"><span data-stu-id="5255a-152">Delete the application package</span></span>

<span data-ttu-id="5255a-153">Miután az alkalmazástípus rendelkezik leépítette a következő, törölheti az alkalmazáscsomag az image store Ha már nincs szüksége.</span><span class="sxs-lookup"><span data-stu-id="5255a-153">After you have unprovisioned the application type, you can delete the application package from the image store if you no longer need it.</span></span> <span data-ttu-id="5255a-154">Alkalmazás csomagok törlésével szabadítson fel lemezterületet segítségével.</span><span class="sxs-lookup"><span data-stu-id="5255a-154">Deleting application packages helps reclaim disk space.</span></span> 

<span data-ttu-id="5255a-155">Az image store alkalmazáscsomag törléséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="5255a-155">To delete the application package from the image store, use the following command:</span></span>

```azurecli
sfctl store delete --content-path app_package_dir
```

<span data-ttu-id="5255a-156">`content-path`annak a könyvtárnak az alkalmazás létrehozásakor feltöltött nevének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5255a-156">`content-path` must be the name of the directory that you uploaded when you created the application.</span></span>

## <a name="upgrade-application"></a><span data-ttu-id="5255a-157">Alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="5255a-157">Upgrade application</span></span>

<span data-ttu-id="5255a-158">Miután létrehozta az alkalmazást, ismételje meg ugyanazokat a lépéseket egy második verziója az alkalmazás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5255a-158">After creating your application, you can repeat the same set of steps to provision a second version of your application.</span></span> <span data-ttu-id="5255a-159">Ezt követően a Service Fabric-alkalmazás frissítését, is átmenet az alkalmazás a második verzióját.</span><span class="sxs-lookup"><span data-stu-id="5255a-159">Then, with a Service Fabric application upgrade you can transition to running the second version of the application.</span></span> <span data-ttu-id="5255a-160">További információkért lásd: a dokumentáció a [Service Fabric az alkalmazásfrissítéseket](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="5255a-160">For more information, see the documentation on [Service Fabric application upgrades](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="5255a-161">A frissítés végrehajtásához először az alkalmazás azt korábban az használja ugyanazokat a parancsokat a következő verziójában kiépítése:</span><span class="sxs-lookup"><span data-stu-id="5255a-161">To perform an upgrade, first provision the next version of the application using the same commands as before:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
```

<span data-ttu-id="5255a-162">Ajánlott majd, végezheti el a figyelt automatikus frissítése a frissítés a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="5255a-162">It is recommended then to perform a monitored automatic upgrade, launch the upgrade by running the following command:</span></span>

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

<span data-ttu-id="5255a-163">Frissítések bírálja felül a meglévő paramétereket bármilyen készlet van megadva.</span><span class="sxs-lookup"><span data-stu-id="5255a-163">Upgrades override existing parameters with whatever set is specified.</span></span> <span data-ttu-id="5255a-164">Alkalmazás paraméterei kell átadni argumentumként a frissítési parancs, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="5255a-164">Application parameters should be passed as arguments to the upgrade command, if necessary.</span></span> <span data-ttu-id="5255a-165">Alkalmazás paraméterei kell kódolható egy JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="5255a-165">Application parameters should be encoded as a JSON object.</span></span>

<span data-ttu-id="5255a-166">Korábban a megadott paraméterek lekéréséhez használja a `sfctl application info` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5255a-166">To retrieve any parameters previously specified, you can use the `sfctl application info` command.</span></span>

<span data-ttu-id="5255a-167">Ha egy alkalmazás frissítése folyamatban van, az állapot lekérhető használatával a `sfctl application upgrade-status` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5255a-167">When an application upgrade is in progress, the status can be retrieved using the `sfctl application upgrade-status` command.</span></span>

<span data-ttu-id="5255a-168">Végül, ha egy frissítés folyamatát, és hozzá lehet megszakítani, használhatja a `sfctl application upgrade-rollback` vissza tudják állítani a frissítést.</span><span class="sxs-lookup"><span data-stu-id="5255a-168">Finally, if an upgrade is in progress and needs to be canceled, you can use the `sfctl application upgrade-rollback` to roll back the upgrade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5255a-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5255a-169">Next steps</span></span>

* [<span data-ttu-id="5255a-170">Service Fabric CLI alapjai</span><span class="sxs-lookup"><span data-stu-id="5255a-170">Service Fabric CLI basics</span></span>](service-fabric-cli.md)
* [<span data-ttu-id="5255a-171">A Service Fabric Linux első lépések</span><span class="sxs-lookup"><span data-stu-id="5255a-171">Getting started with Service Fabric on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="5255a-172">A Service Fabric-alkalmazás frissítés indítása</span><span class="sxs-lookup"><span data-stu-id="5255a-172">Launching a Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

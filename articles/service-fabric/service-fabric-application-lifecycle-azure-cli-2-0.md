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
# <a name="manage-an-azure-service-fabric-application-by-using-azure-cli-20"></a>Az Azure Service Fabric-alkalmazás kezelése az Azure CLI 2.0 használatával

Megtudhatja, hogyan toocreate és törli az alkalmazást, hogy az Azure Service Fabric-fürt.

## <a name="prerequisites"></a>Előfeltételek

* Az Azure CLI 2.0 telepítése. Ezután válassza ki a Service Fabric-fürt. További információkért lásd: [Ismerkedés az Azure CLI 2.0](service-fabric-azure-cli-2-0.md).

* A Service Fabric application csomag készen toobe telepítve van. További információ tooauthor és a csomag egy alkalmazást, olvassa el hello [Service Fabric-alkalmazás modell](service-fabric-application-model.md).

## <a name="overview"></a>Áttekintés

toodeploy egy új alkalmazást, végezze el az alábbi lépéseket:

1. Töltse fel az alkalmazás csomag toohello Service Fabric lemezképtárolóhoz.
2. Az alkalmazástípus kiépítéséhez.
3. Adja meg, és hozzon létre egy alkalmazást.
4. Adja meg, és a szolgáltatások létrehozására.

tooremove egy meglévő alkalmazást, kövesse ezeket a lépéseket:

1. Hello alkalmazás törlése.
2. Leépíteni hello társított alkalmazás típusa.
3. Hello lemezképet tároló tartalom törlése.

## <a name="deploy-a-new-application"></a>Egy új alkalmazás központi telepítése

egy új alkalmazást, a következő teljes hello toodeploy feladatok.

### <a name="upload-a-new-application-package-toohello-image-store"></a>Egy új alkalmazás csomag toohello lemezképtárolóhoz feltöltése

Az alkalmazás létrehozása előtt töltse fel a hello alkalmazás csomag toohello Service Fabric lemezképtárolóhoz. 

Például, ha a alkalmazáscsomag hello `app_package_dir` directory, a következő parancsok tooupload hello directory használata hello:

```azurecli
az sf application upload --path ~/app_package_dir
```

Nagy alkalmazáscsomagok esetén megadhatja hello `--show-progress` toodisplay hello előrehaladását hello feltöltés lehetőséget.

### <a name="provision-hello-application-type"></a>Kiépítés hello alkalmazás típusa

Hello feltöltés befejezését követően használhatók a hello alkalmazás. tooprovision hello alkalmazás, a következő parancs használata hello:

```azurecli
az sf application provision --application-type-build-path app_package_dir
```

a következő hello `application-type-build-path` hello neve, ahol feltöltött alkalmazáscsomag hello könyvtár.

### <a name="create-an-application-from-an-application-type"></a>Alkalmazástípusok az alkalmazás létrehozása

Hello alkalmazás kiépítése után a következő parancs tooname hello használja, és az alkalmazás létrehozása:

```azurecli
az sf application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name`hello nevét, amelyet az toouse hello alkalmazás példány van. További paraméterek az előzőleg létrehozott alkalmazásjegyzék hello kérheti le.

hello alkalmazásnév hello előtaggal kell kezdődnie `fabric:/`.

### <a name="create-services-for-hello-new-application"></a>Szolgáltatások hello új alkalmazás létrehozása

Miután létrehozott egy alkalmazást, a szolgáltatások létrehozására hello alkalmazásból. A következő példa hello hogy hozzon létre egy új állapot nélküli az alkalmazásból. hello szolgáltatásokat hozhat létre egy alkalmazást a korábban kiépített alkalmazáscsomag hello szolgáltatás jegyzékfájl vannak definiálva.

```azurecli
az sf service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Alkalmazások üzembe helyezését és állapotának ellenőrzése

tooverify, hogy egy alkalmazás és szolgáltatás sikeresen telepített, ellenőrizze, hogy hello alkalmazás és szolgáltatás szerepel:

```azurecli
az sf application list
az sf service list --application-list TestApp
```

tooverify, hogy hello szolgáltatás állapota megfelelő, hasonló parancsok tooretrieve hello állapotfigyelő hello szolgáltatás és alkalmazás hello használata:

```azurecli
az sf application health --application-id TestApp
az sf service health --service-id TestApp/TestSvc
```

Kifogástalan szolgáltatások és alkalmazások egy `HealthState` értékének `Ok`.

## <a name="remove-an-existing-application"></a>Távolítsa el a meglévő alkalmazás

tooremove egy alkalmazást, a következő feladatok teljes hello.

### <a name="delete-hello-application"></a>Hello alkalmazás törlése

toodelete hello alkalmazás, a következő parancs használata hello:

```azurecli
az sf application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a>Leépíteni a következőt: hello alkalmazás típusa

Hello alkalmazás törlése után is leépítése hello alkalmazás típusa, ha már nincs szüksége. toounprovision hello alkalmazástípusok, a következő parancs használata hello:

```azurecli
az sf application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

hello típus nevét és típusát meg kell egyeznie hello neve és verziója hello korábban kiosztott alkalmazásjegyzékben.

### <a name="delete-hello-application-package"></a>Hello alkalmazáscsomag törlése

Miután hello alkalmazástípus rendelkezik leépítette a következő, törölheti hello alkalmazáscsomag hello lemezképtárolóhoz Ha már nincs szüksége. Alkalmazás csomagok törlésével szabadítson fel lemezterületet segítségével. 

hello-alkalmazáscsomag toodelete hello kép Store áruházból, a következő parancs használata hello:

```azurecli
az sf application package-delete --content-path app_package_dir
```

`content-path`hello directory hello alkalmazás létrehozásakor feltöltött hello nevének kell lennie.

## <a name="related-articles"></a>Kapcsolódó cikkek

* [A Service Fabric és az Azure CLI 2.0 használatának első lépései](service-fabric-azure-cli-2-0.md)
* [A Service Fabric XPlat parancssori felület használatának első lépései](service-fabric-azure-cli.md)

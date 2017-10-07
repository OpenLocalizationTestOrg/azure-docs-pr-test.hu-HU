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
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli"></a>Az Azure Service Fabric-alkalmazás kezelése az Azure Service Fabric parancssori felület használatával

Megtudhatja, hogyan toocreate és törli az alkalmazást, hogy az Azure Service Fabric-fürt.

## <a name="prerequisites"></a>Előfeltételek

* A Service Fabric parancssori felület telepítése. Ezután válassza ki a Service Fabric-fürt. További információkért lásd: [Ismerkedés a Service Fabric CLI](service-fabric-cli.md).

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

toodeploy egy új alkalmazást, a következő feladatok teljes hello:

### <a name="upload-a-new-application-package-toohello-image-store"></a>Egy új alkalmazás csomag toohello lemezképtárolóhoz feltöltése

Az alkalmazás létrehozása előtt töltse fel a hello alkalmazás csomag toohello Service Fabric lemezképtárolóhoz.

Például, ha a alkalmazáscsomag hello `app_package_dir` directory, a következő parancsok tooupload hello directory használata hello:

```azurecli
sfctl application upload --path ~/app_package_dir
```

Nagy alkalmazáscsomagok esetén megadhatja hello `--show-progress` toodisplay hello előrehaladását hello feltöltés lehetőséget.

### <a name="provision-hello-application-type"></a>Kiépítés hello alkalmazás típusa

Hello feltöltés befejezését követően használhatók a hello alkalmazás. tooprovision hello alkalmazás, a következő parancs használata hello:

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

a következő hello `application-type-build-path` hello neve, ahol feltöltött alkalmazáscsomag hello könyvtár.

### <a name="create-an-application-from-an-application-type"></a>Alkalmazástípusok az alkalmazás létrehozása

Hello alkalmazás kiépítése után a következő parancs tooname hello használja, és az alkalmazás létrehozása:

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name`hello nevét, amelyet az toouse hello alkalmazás példány van. További paraméterek lekérheti az előzőleg létrehozott alkalmazás jegyzékében.

hello alkalmazásnév hello előtaggal kell kezdődnie `fabric:/`.

### <a name="create-services-for-hello-new-application"></a>Szolgáltatások hello új alkalmazás létrehozása

Miután létrehozott egy alkalmazást, a szolgáltatások létrehozására hello alkalmazásból. A következő példa hello hogy hozzon létre egy új állapot nélküli az alkalmazásból. hello szolgáltatásokat hozhat létre egy alkalmazást a korábban kiépített alkalmazáscsomag hello szolgáltatás jegyzékfájl vannak definiálva.

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Alkalmazások üzembe helyezését és állapotának ellenőrzése

tooverify mindent kifogástalan állapotú, használja a következő állapotfigyelő parancsok hello:

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

tooverify, hogy hello szolgáltatás állapota megfelelő, hasonló parancsok tooretrieve hello állapotát a hello szolgáltatás, és az alkalmazás használata:

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

Kifogástalan szolgáltatások és alkalmazások egy `HealthState` értékének `Ok`.

## <a name="remove-an-existing-application"></a>Távolítsa el a meglévő alkalmazás

tooremove egy alkalmazást, a következő feladatok teljes hello:

### <a name="delete-hello-application"></a>Hello alkalmazás törlése

toodelete hello alkalmazás, a következő parancs használata hello:

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a>Leépíteni a következőt: hello alkalmazás típusa

Hello alkalmazás törlése után is leépítése hello alkalmazás típusa, ha már nincs szüksége. toounprovision hello alkalmazástípusok, a következő parancs használata hello:

```azurecli
sfctl application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

hello típus nevét és típusát meg kell egyeznie hello neve és verziója hello korábban kiosztott alkalmazásjegyzékben.

### <a name="delete-hello-application-package"></a>Hello alkalmazáscsomag törlése

Miután hello alkalmazástípus rendelkezik leépítette a következő, törölheti hello alkalmazáscsomag hello lemezképtárolóhoz Ha már nincs szüksége. Alkalmazás csomagok törlésével szabadítson fel lemezterületet segítségével. 

hello-alkalmazáscsomag toodelete hello kép Store áruházból, a következő parancs használata hello:

```azurecli
sfctl store delete --content-path app_package_dir
```

`content-path`hello directory hello alkalmazás létrehozásakor feltöltött hello nevének kell lennie.

## <a name="upgrade-application"></a>Alkalmazás frissítése

Miután létrehozta az alkalmazást, megismételhető hello ugyanazokat a lépéseket tooprovision az alkalmazás egy másik verziója. Ezt követően a Service Fabric-alkalmazás frissítését dönthet, hogy átvált toorunning hello második hello alkalmazás verziója. További információ: hello dokumentációja a [Service Fabric az alkalmazásfrissítéseket](service-fabric-application-upgrade.md).

első rendelkezés hello következő verziójára hello alkalmazás használja a frissítés tooperform azt korábban hello ugyanazokat a parancsokat:

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
```

Javasoljuk, majd tooperform egy figyelt automatikus frissítését, indítsa el a hello frissítés hello a következő parancs futtatásával:

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

Frissítések bírálja felül a meglévő paramétereket bármilyen készlet van megadva. Alkalmazás paraméterei kell adhatók át argumentumok toohello frissítési parancs, ha szükséges. Alkalmazás paraméterei kell kódolható egy JSON-objektum.

tooretrieve minden korábban megadott paraméterek, használhatja a hello `sfctl application info` parancsot.

Ha egy alkalmazás frissítése folyamatban van, hello állapot lekérhető használatával a `sfctl application upgrade-status` parancsot.

Végül, ha a frissítés folyamatban van, és meg kell szakítani toobe, használhat hello `sfctl application upgrade-rollback` tooroll vissza hello frissítését.

## <a name="next-steps"></a>Következő lépések

* [Service Fabric CLI alapjai](service-fabric-cli.md)
* [A Service Fabric Linux első lépések](service-fabric-get-started-linux.md)
* [A Service Fabric-alkalmazás frissítés indítása](service-fabric-application-upgrade.md)

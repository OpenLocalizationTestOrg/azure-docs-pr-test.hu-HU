---
title: "az IoT-központ Azure CLI-vel (azure.js) aaaCreate |} Microsoft Docs"
description: "Hogyan egy Azure IoT hub használatával toocreate hello platformfüggetlen Azure CLI (azure.js)."
services: iot-hub
documentationcenter: .net
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: 46a17831-650c-41d9-b228-445c5bb423d3
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: boltean
ms.openlocfilehash: c2a7ea98500b0a0e55a39f4cdfea4605c92add94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-cli"></a>Létrehoz egy IoT-központot hello Azure parancssori felület használatával

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Bevezetés

Használhatja az Azure parancssori felület (azure.js) toocreate és Azure IoT-központok programozott kezelését. Ez a cikk bemutatja, hogyan toouse hello Azure CLI (azure.js) toocreate egy IoT-központot.

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

* Az Azure CLI (azure.js) – hello CLI hello klasszikus és resource management üzembe helyezési modellel ebben a cikkben leírtak szerint.
* [Az Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) -hello következő generációs CLI hello erőforrás management üzembe helyezési modellben.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].
* [Az Azure CLI 0.10.4] [ lnk-CLI-install] vagy újabb. Ha már van telepítve az Azure parancssori felület hello, ellenőrzéséhez hello verziószámának hello parancssorban hello a következő parancsot:

```azurecli
azure --version
```

> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). hello Azure CLI Azure Resource Manager módban kell lennie:
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a>Az Azure-fiók és -előfizetés beállítása

1. Hello parancssorba írja be a bejelentkezési hello a következő parancsot:

   ```azurecli
    azure login
   ```

   Hello javasolt webböngésző és kód tooauthenticate használja.
1. Ha több Azure-előfizetéssel rendelkezik, csatlakozás tooAzure hozzáférést biztosít tooall hello Azure-előfizetéssel társított hitelesítő adatait. Megtekintheti hello Azure-előfizetések, és azok azonosítása, mely még hello alapértelmezés szerint hello parancs használatával:

   ```azurecli
    azure account list
   ```

   tooset hello előfizetési kontextust kérünk toorun hello többi hello parancsokat használja:

   ```azurecli
    azure account set <subscription name>
   ```

1. Ha még nem rendelkezik egy erőforráscsoport, létrehozhat egy nevű **exampleResourceGroup**:

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> hello cikk [használja hello Azure CLI toomanage Azure erőforráscsoport-sablonok és erőforrások] [ lnk-CLI-arm] hogyan toouse hello Azure CLI toomanage Azure további információt nyújt erőforrások.

## <a name="create-an-iot-hub"></a>IoT Hub létrehozása

Kötelező paraméter:

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* **Erőforráscsoport**. hello erőforráscsoport-név. kis-és nagybetűket alfanumerikus, aláhúzásjelet, és kötőjelet tartalmazhat, 1-64 hossza hello formátuma.
* **Név** hello IoT hub toobe létrehozott hello neve. kis-és nagybetűket alfanumerikus, aláhúzásjelet, és kötőjelet tartalmazhat, hossza 3 – 50 hello formátuma.
* **hely**. hello helye (azure régió/adatközpont) tooprovision hello IoT-központot.
* **Termékváltozat**. hello sku, egyik hello neve: [F1, S1, S2, S3]. Hello legújabb teljes listáját tekintse meg a toohello IoT hub árképzést ismertető oldalra.
* **egységek**. hello kiosztott egységek száma. Tartományon: [1-1] F1: S1, S2 [1-200]: [1 – 10] S3. IoT Hub-egységek alapuló tooconnect szeretné az összes üzenet számán és hello eszközök száma.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

toosee összes hello létrehozásához rendelkezésre álló paramétereket, használhatja a hello súgó parancsot a parancssorba:

```azurecli
azure iothub create -h
```

Gyors példa: az IoT-központ nevű toocreate **exampleIoTHubName** hello erőforráscsoportban **exampleResourceGroup**- ben futtassa hello következő parancsot:

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> Az Azure parancssori felület létrehozza az S1 Standard IoT-központ, amelynek kell fizetni. Törölheti a hello IoT-központ **exampleIoTHubName** használatával a következő parancsot:
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a>Következő lépések

IoT Hub, fejlesztésével kapcsolatos további toolearn lásd: hello a következő cikket:

* [Az IoT-SDK][lnk-sdks]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [Az Azure portál toomanage IoT-központ hello használata][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 

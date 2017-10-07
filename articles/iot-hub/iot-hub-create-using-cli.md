---
title: "az IoT-központ Azure CLI-vel (az.py) aaaCreate |} Microsoft Docs"
description: "Hogyan egy Azure IoT hub használatával toocreate hello platformfüggetlen Azure CLI 2.0 (az.py)."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 9c9639235c2ac343e6ceb9578291dafaea26ea24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-cli-20"></a>Létrehoz egy IoT-központot hello Azure CLI 2.0 használatával

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Bevezetés

Használhatja az Azure CLI 2.0 (az.py) toocreate és Azure IoT-központok programozott kezelését. Ez a cikk bemutatja, hogyan toouse hello Azure CLI 2.0 (az.py) toocreate egy IoT-központot.

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

* [Az Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) – hello CLI hello klasszikus és resource management üzembe helyezési modellek.
* Az Azure CLI 2.0 (az.py) – hello következő generációs CLI hello erőforrás felügyeleti telepítési modell ebben a cikkben leírtak szerint.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].
* [Az Azure CLI 2.0][lnk-CLI-install].

## <a name="sign-in-and-set-your-azure-account"></a>Jelentkezzen be, és állítsa be az Azure-fiókjával

Jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését.

1. Hello parancssorban futtassa a hello [bejelentkezési parancs][lnk-login-command]:
    
    ```azurecli
    az login
    ```

    Hajtsa végre a hello utasításokat tooauthenticate hello kód használatával, és jelentkezzen be Azure-fiók egy webböngészőn keresztül tooyour.

2. Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello a hitelesítő adatok társított Azure-fiókra. Hello következő [parancs toolist hello Azure-fiókra] [ lnk-az-account-command] meg toouse érhető el:
    
    ```azurecli
    az account list 
    ```

    A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toocreate az IoT hub hello használata. Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a>IoT Hub létrehozása

Hello Azure CLI toocreate erőforráscsoport használja, és adja hozzá az IoT-központ.

1. Amikor létrehoz egy IoT-központot, erőforráscsoportban kell létrehoznia. Használjon egy meglévő erőforráscsoportot, vagy futtassa a következő hello [parancs toocreate erőforráscsoport][lnk-az-resource-command]:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > hello előző példa hello erőforráscsoport hello USA nyugati régiója helyet hoz létre. Hello parancs futtatásával megtekintheti a rendelkezésre álló helyek listáját `az account list-locations -o table`.
    >
    >

2. Futtassa a következő hello [parancs toocreate az IoT-központ] [ lnk-az-iot-command] az erőforráscsoportban, globálisan egyedi névvel az IoT hub:
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> előző parancs hello az IoT-központ IP-címek, amelynek számlázása hello S1 hoz létre. További információkért lásd: [Azure IoT Hub árképzési][lnk-iot-pricing].
>
>

## <a name="remove-an-iot-hub"></a>Távolítsa el az IoT-központ

Használhatja az Azure parancssori felület hello túl[egyedi erőforrás törlése][lnk-az-resource-command], például egy IoT-központ, vagy törölje az erőforráscsoportot és az ahhoz tartozó összes erőforrást, beleértve az IoT-központok.

az IoT-központ toodelete hello a következő parancsot futtassa:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

toodelete az erőforráscsoportot és a hozzá tartozó összes erőforrásnak, a következő futtatási hello parancsot:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Következő lépések
toolearn IoT-központot, fejlesztésével kapcsolatos további hello a következő cikkekben talál:

* [IoT Hub fejlesztői útmutató][lnk-devguide]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [Az Azure portál toomanage IoT-központ hello használata][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 

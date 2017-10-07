---
title: "egy Azure IoT-központot egy PowerShell-parancsmaggal aaaCreate |} Microsoft Docs"
description: "Hogyan toouse egy PowerShell-parancsmag toocreate egy IoT-központot."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 005cd8d48eb39d2e8b1bfb9ef84330d4aae4658f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-new-azurermiothub-cmdlet"></a>Új-AzureRmIotHub hello parancsmaggal IoT hub létrehozása

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Bevezetés

Használhatja az Azure PowerShell-parancsmagok toocreate és Azure IoT-központok kezelése. Az oktatóanyag bemutatja, hogyan toocreate az IoT-központ a PowerShell használatával.

> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello Azure Resource Manager telepítési modell használatát bemutatja.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Aktív Azure-fiók. <br/>Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].
* [Az Azure PowerShell-parancsmagok][lnk-powershell-install].

## <a name="connect-tooyour-azure-subscription"></a>Csatlakozás Azure-előfizetés tooyour
A PowerShell-parancssort írja be a hello parancs toosign tooyour Azure-előfizetés a következő:

```powershell
Login-AzureRmAccount
```

Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello Azure-előfizetéssel társított hitelesítő adatait. A következő parancs toolist hello Azure-előfizetések elérhető az Ön toouse hello használata:

```powershell
Get-AzureRMSubscription
```

A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toocreate az IoT hub hello használata. Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

## <a name="create-resource-group"></a>Erőforráscsoport létrehozása

Egy erőforrás csoport toodeploy az IoT-központ van szüksége. Használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.

Használhatja az alábbi parancs toodiscover hello helyek telepíthető az IoT-központ hello:

```powershell
((Get-AzureRmResourceProvider `
  -ProviderNamespace Microsoft.Devices).ResourceTypes `
  | Where-Object ResourceTypeName -eq IoTHubs).Locations
```

az IoT hub az IoT-központ, a következő parancs használata hello hello támogatott helyek egyikén erőforráscsoport toocreate. Ebben a példában egy nevű erőforráscsoportot hoz létre **MyIoTRG1** a hello **USA keleti régiója** régió:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>IoT Hub létrehozása

az IoT-központ hello előző lépésben, a következő parancs használata hello létrehozott hello erőforráscsoportban toocreate. Ez a példa létrehoz egy **S1** nevű hub **MyTestIoTHub** a hello **USA keleti régiója** régió:

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

az IoT-központ hello hello nevének egyedinek kell lennie.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


Minden hello IoT-központok az előfizetéshez a következő parancs hello segítségével jeleníthetők meg:

```powershell
Get-AzureRmIotHub
```

hello előző példa ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni. Törölheti a hello IoT-központ hello a következő parancs használatával:

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

Eltávolíthatja az egy erőforráscsoport, és minden hello erőforrásokat tartalmaz, a következő parancs hello használata:

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Következő lépések

Most egy IoT-központot egy PowerShell-parancsmag használatával telepített, akkor érdemes lehet további tooexplore:

* Fedezze fel más [PowerShell-parancsmagok használata az IoT hub][lnk-iothub-cmdlets].
* További információ a hello hello képességeit [IoT-központ erőforrás-szolgáltató REST API][lnk-rest-api].

toolearn IoT-központot, fejlesztésével kapcsolatos további hello a következő cikkekben talál:

* [Bevezetés tooC SDK][lnk-c-sdk]
* [Az Azure IoT SDK-k][lnk-sdks]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [Egy eszköz szimulálva IoT oldala][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-iothub-cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.iothub/
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

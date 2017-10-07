---
title: "az Azure IoT-központ sablon (PowerShell) segítségével aaaCreate |} Microsoft Docs"
description: "Hogyan toouse az Azure Resource Manager sablon toocreate az IoT-központ a PowerShell használatával."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7eade855-c289-4ffb-b5ef-02be8c5f670f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: e98ff5e898200cd727b9326fb3df393e43b021e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Létrehoz egy IoT-központot, Azure Resource Manager sablonnal (PowerShell)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Használhatja az Azure Resource Manager toocreate és Azure IoT-központok programozott kezelését. Az oktatóanyag bemutatja, hogyan toouse az Azure Resource Manager sablon toocreate az IoT-központ a PowerShell használatával.

> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello Azure Resource Manager telepítési modell használatát bemutatja.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Aktív Azure-fiók. <br/>Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].
* [Az Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb.

> [!TIP]
> hello cikk [az Azure PowerShell használata Azure Resource Managerrel] [ lnk-powershell-arm] nyújt további információt toouse PowerShell és az Azure Resource Manager sablonok toocreate Azure erőforrásokat.

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

A következő parancsok toodiscover, ahol az IoT-központ telepítése és hello jelenleg támogatott API-verziók hello használhatja:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Hozzon létre egy erőforrás csoport toocontain az IoT hub, a következő parancs az IoT-központ hello támogatott helyek egyikén hello segítségével. Ebben a példában egy nevű erőforráscsoportot hoz létre **MyIoTRG1**:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-toocreate-an-iot-hub"></a>Küldje el a sablon toocreate az IoT-központ

A JSON-sablon toocreate az IoT-központ használja az erőforráscsoportban. Az Azure Resource Manager sablon toomake módosítások tooan meglévő IoT-központ is használható.

1. Használja a szöveg szerkesztő toocreate Azure Resource Manager-sablonok nevű **template.json** a hello erőforrás-definíció toocreate követően egy új, normál IoT-központot. Ebben a példában az IoT-központ hello helyez hello **USA keleti régiója** régió, létrehoz két felhasználói csoportok (**cg1** és **cg2**) hello Event Hub-kompatibilis végpont, és használja hello **2016-02-03** API-verzió. Ez a sablon is várja meg toopass az IoT-központnév hello paraméterként nevű **hubName**. Hello jelenlegi listája támogatja az IoT Hub-helyeken: [Azure állapot][lnk-status].

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Mentse a hello Azure Resource Manager sablon fájlt a helyi számítógépen. Ebben a példában feltételezzük, hogy nevű mappába menti **c:\templates**.

3. Futtassa a következő parancs toodeploy hello az új IoT hub, amely az IoT hub nevét hello átadott paraméterként. Ebben a példában az IoT-központ hello hello neve nem `abcmyiothub`. az IoT hub hello nevének globálisan egyedi kell lennie:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. hello kimeneti hello kulcsokat hello IoT hub létrehozott jeleníti meg.

5. az alkalmazás hozzáadott tooverify hello új IoT hub, látogasson el hello [Azure-portálon] [ lnk-azure-portal] és tekintse meg az erőforrások listáját. Másik megoldásként használhatja a hello **Get-AzureRmResource** PowerShell-parancsmagot.

> [!NOTE]
> A mintaalkalmazás ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni. Törölheti a hello IoT-központ keresztül hello [Azure-portálon] [ lnk-azure-portal] vagy hello segítségével **Remove-AzureRmResource** PowerShell-parancsmag, amikor elkészült.

## <a name="next-steps"></a>Következő lépések

Most egy IoT-központot, a PowerShell segítségével az Azure Resource Manager-sablon használatával telepített, akkor érdemes lehet további tooexplore:

* További információ a hello hello képességeit [IoT-központ erőforrás-szolgáltató REST API][lnk-rest-api].
* Olvasási [Azure Resource Manager áttekintése] [ lnk-azure-rm-overview] Azure Resource Manager hello képességeivel kapcsolatos további toolearn.

toolearn IoT-központot, fejlesztésével kapcsolatos további hello a következő cikkekben talál:

* [Bevezetés tooC SDK][lnk-c-sdk]
* [Az Azure IoT SDK-k][lnk-sdks]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

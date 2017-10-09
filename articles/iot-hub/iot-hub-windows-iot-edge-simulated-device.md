---
title: "Azure IoT peremhálózati (Windows) rendelkező eszköz aaaSimulate |} Microsoft Docs"
description: "Hogyan toouse Azure IoT peremhálózati a Windows toocreate a szimulált eszköz, amely elküldi az Azure IoT peremhálózati átjáró tooan IoT-központ keresztül telemetriai."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: ddbe85eb956e9934e80e2e80e09f77b24cf54856
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-toosend-device-to-cloud-messages-with-a-simulated-device-windows"></a>Azure IoT peremhálózati toosend eszközről a felhőbe üzenetek használata a szimulált eszköz (Windows)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-toorun-hello-sample"></a>Hogyan toorun hello minta

Hello **build.cmd** parancsfájlt hoz létre a kimeneti hello **build** hello helyi példánya mappájában **iot-edge** tárházba. A kimeneti hello a mintában használt négy IoT peremhálózati modulok tartalmazza.

build script helyek hello a:

* **Logger.dll** a hello **build\\modulok\\naplózó\\Debug** mappát.
* **iothub.dll** a hello **build\\modulok\\IOT hubbal\\Debug** mappát.
* **identitás\_map.dll** a hello **build\\modulok\\identitymap\\Debug** mappát.
* **Szimulált\_device.dll** a hello **build\\modulok\\szimulált\_eszköz\\Debug** mappát.

Az elérési utak használata hello **modul elérési útján** látható módon hello beállításokat JSON-fájlt a következő értékeket:

Szimulált hello\_eszköz\_felhő\_feltöltése\_minta tart hello elérési tooa JSON-konfigurációs fájl parancssori argumentumként. hello alábbi példa JSON-fájl valósul meg a következő SDK-tárházban hello **minták\\szimulált\_eszköz\_felhő\_feltöltése\_minta\\src\\ Szimulált\_eszköz\_felhő\_feltöltése\_minta\_win.json**. A konfigurációs fájl működik, kivéve, ha módosítja a hello build script tooplace hello IoT peremhálózati modulok, vagy végrehajtható fájlok alapértelmezettől eltérő helyeket a mintát.

> [!NOTE]
> hello modul elérési útvonalai hello szimulált ahol relatív toohello directory\_eszköz\_felhő\_feltöltése\_sample.exe helyezkedik el. hello minta JSON-konfigurációs fájlt toowriting too'deviceCloudUploadGatewaylog.log alapértelmezés szerint "az aktuális munkakönyvtárban a.

Egy szövegszerkesztőben nyissa meg a hello fájlt **minták\\szimulált\_eszköz\_felhő\_feltöltése\_minta\\src\\szimulált\_eszköz \_felhő\_feltöltése\_win.json** hello helyi példánya a **iot-edge** tárházba. Ez a fájl hello IoT peremhálózati modulok hello minta átjáró konfigurálja:

* Hello **IOT hubbal** modul tooyour IoT-központ csatlakozik. Konfigurálja toosend adatok tooyour IoT-központot. Pontosabban, a set hello **IoTHubName** az IoT hub nevét toohello értékét, és állítsa be a hello **IoTHubSuffix** érték túl**azure-devices.net**. Set hello **átviteli** az érték tooone: **HTTP**, **AMQP**, vagy **MQTT**. Jelenleg csak **HTTP** közösen használja az összes eszközre küldött üzenetek egy TCP-kapcsolatot. Ha a hello érték túl**AMQP**, vagy **MQTT**, hello átjáró egy külön TCP kapcsolat tooIoT Hub az egyes eszközök tart fenn.
* Hello **leképezési** modul leképezi a szimulált eszköz tooyour IoT-központ eszközazonosítókat hello MAC-címeit. Győződjön meg arról, hogy **deviceId** értékek egyeznek hello azonosítói hello két tooyour IoT-központot, és adott hello hozzáadott eszközök **deviceKey** értékeket tartalmaz, a két eszközök hello kulcsokat.
* Hello **BLE1** és **BLE2** modulokra szimulált hello eszközök. Vegye figyelembe, hogyan hello modul MAC-címek felel meg a hello hello címek **leképezési** modul.
* Hello **naplózó** modul naplózza az átjáró tevékenység tooa fájlt.
* Hello **modul elérési útján** hello a következő példában látható értékek a következők hello szimulált ahol relatív toohello directory\_eszköz\_felhő\_feltöltése\_sample.exe helyezkedik el.
* Hello **hivatkozások** hello JSON-fájl hello alján tömb csatlakozik hello **BLE1** és **BLE2** modulok toohello **leképezési** modul, és hello **leképezési** modul toohello **IOT hubbal** modul. Emellett biztosítja, hogy az összes üzenet hello által naplózott **naplózó** modul.

```json
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01"
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02"
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

Mentés hello módosításokat végzett toohello konfigurációs fájl.

toorun hello minta:

1. A parancssorban lépjen a toohello **build** hello helyi példánya mappájában **iot-edge** tárházba.
2. Futtassa a következő parancs hello:
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. Használhatja a hello [eszköz explorer] [ lnk-device-explorer] vagy [IOT hubbal-explorer] [ lnk-iothub-explorer] IoT-központ fogadja hello toomonitor köszönőüzenetei eszköz átjáró. Például az IOT hubbal-Explorerben figyelheti eszközről a felhőbe üzenetek hello a következő parancs használatával:

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Következő lépések

toogain tájékozottabbak IoT peremhálózati és kísérlet az egyes kódpéldák hello következő látogasson el fejlesztői oktatóanyagok és erőforrások:

* [Eszköz-felhő üzeneteket küldeni egy fizikai eszközről IoT oldala][lnk-physical-device]
* [Az Azure IoT él][lnk-iot-edge]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]
* [Az IoT-megoldásból hello szabad a biztonságos][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
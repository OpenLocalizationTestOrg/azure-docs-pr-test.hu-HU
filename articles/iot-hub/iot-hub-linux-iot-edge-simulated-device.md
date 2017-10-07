---
title: "Azure IoT peremhálózati (Linux) rendelkező eszköz aaaSimulate |} Microsoft Docs"
description: "Hogyan toouse Azure IoT peremhálózati lévő Linux toocreate a szimulált eszköz, amely elküldi telemetriai adatokat az IoT-Edge révén átjáró tooan IoT-központ."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 11e7bf28-ee3d-48d6-a386-eb506c7a31cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: 168fb8eda8671d02c63073bdf36dfcd88b397fe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-toosend-device-to-cloud-messages-with-a-simulated-device-linux"></a>Azure IoT peremhálózati toosend eszközről a felhőbe üzenetek használata a szimulált eszköz (Linux)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-toorun-hello-sample"></a>Hogyan toorun hello minta

Hello **build.sh** parancsfájlt hoz létre a kimeneti hello **build** hello helyi példánya mappájában **iot-edge** tárházba. A kimeneti hello a mintában használt négy IoT peremhálózati modulok tartalmazza.

build script helyek hello a:

* **liblogger.so** a hello **build/modulok/naplózó** mappát.
* **libiothub.so** a hello **build/modulok/IOT hubbal** mappát.
* **lib\_identitás\_map.so** a hello **build/modulok/identitymap** mappa.
* **libsimulated\_device.so** a hello **build, modulok vagy szimulált\_eszköz** mappát.

Az elérési utak használata hello **modul elérési útján** látható módon hello beállításokat JSON-fájlt a következő értékeket:

Szimulált hello\_eszköz\_felhő\_feltöltése\_minta tart hello elérési tooa JSON-konfigurációs fájl parancssori argumentumként. hello alábbi példa JSON-fájl valósul meg a következő SDK-tárházban hello **minták\\szimulált\_eszköz\_felhő\_feltöltése\_minta\\src\\ Szimulált\_eszköz\_felhő\_feltöltése\_minta\_lin.json**. A konfigurációs fájl működik, kivéve, ha módosítja a hello build script tooplace hello IoT peremhálózati modulok, vagy végrehajtható fájlok alapértelmezettől eltérő helyeket a mintát.

> [!NOTE]
> hello modul elérési útvonalai relatív toohello könyvtárat az szimulált hello futtató\_eszköz\_felhő\_feltöltése\_minta végrehajtható fájl, nem hello könyvtárat, amelyben hello végrehajtható fájl található. hello minta JSON-konfigurációs fájlt toowriting too'deviceCloudUploadGatewaylog.log alapértelmezés szerint "az aktuális munkakönyvtárban a.

Egy szövegszerkesztőben nyissa meg a hello fájlt **minták/szimulált\_eszköz\_felhő\_feltöltése\_minta/src/szimulált\_eszköz\_felhő\_feltöltése\_lin.json** hello helyi példánya a **iot-edge** tárházba. Ez a fájl hello IoT peremhálózati modulok hello minta átjáró konfigurálja:

* Hello **IOT hubbal** modul tooyour IoT-központ csatlakozik. Konfigurálja toosend adatok tooyour IoT-központot. Pontosabban, a set hello **IoTHubName** az IoT hub nevét toohello értékét, és állítsa be a hello **IoTHubSuffix** érték túl**azure-devices.net**. Set hello **átviteli** az érték tooone: **HTTP**, **AMQP**, vagy **MQTT**. Jelenleg csak **HTTP** közösen használja az összes eszközre küldött üzenetek egy TCP-kapcsolatot. Ha a hello érték túl**AMQP**, vagy **MQTT**, hello átjáró egy külön TCP kapcsolat tooIoT Hub az egyes eszközök tart fenn.
* Hello **leképezési** modul leképezi a szimulált eszköz tooyour IoT-központ eszközazonosítókat hello MAC-címeit. Győződjön meg arról, hogy **deviceId** értékek egyeznek hello azonosítói hello két tooyour IoT-központot, és adott hello hozzáadott eszközök **deviceKey** értékeket tartalmaz, a két eszközök hello kulcsokat.
* Hello **BLE1** és **BLE2** modulokra szimulált hello eszközök. Vegye figyelembe, hogyan a MAC-cím egyezik hello címek hello **leképezési** modul.
* Hello **naplózó** modul naplózza az átjáró tevékenység tooa fájlt.
* Hello **modul elérési útján** értékek hello példában látható módon azt feltételezik, hogy futtatja hello minta hello **build** hello helyi példánya mappájában **iot-edge** tárházba.
* Hello **hivatkozások** hello JSON-fájl hello alján tömb csatlakozik hello **BLE1** és **BLE2** modulok toohello **leképezési** modul, és hello **leképezési** modul toohello **IOT hubbal** modul. Emellett biztosítja, hogy az összes üzenet hello által naplózott **naplózó** modul.

```json
{
    "modules": [
        {
            "name": "IotHub",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/iothub/libiothub.so"
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
              "module.path": "./modules/identitymap/libidentity_map.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
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
              "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args": {
              "filename": "deviceCloudUploadGatewaylog.log"
            }
          }
    ],
    "links": [
        {
            "source": "*",
            "sink": "Logger"
        },
        {
            "source": "BLE1",
            "sink": "mapping"
        },
        {
            "source": "BLE2",
            "sink": "mapping"
        },
        {
            "source": "mapping",
            "sink": "IotHub"
        }
    ]
}
```

Mentés hello módosításokat végzett toohello konfigurációs fájl.

toorun hello minta:

1. A rendszerhéj, lépjen a toohello **létrehozása, az iot-edge** mappát.
2. Futtassa a következő parancs hello:
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. Használhatja a hello [eszköz explorer] [ lnk-device-explorer] vagy [IOT hubbal-explorer] [ lnk-iothub-explorer] IoT-központ fogadja hello toomonitor köszönőüzenetei eszköz átjáró. Például az IOT hubbal-Explorerben figyelheti eszközről a felhőbe üzenetek hello a következő parancs használatával:

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Következő lépések

toogain tájékozottabbak Azure IoT peremhálózati és kísérlet az egyes kódpéldák hello következő látogasson el fejlesztői oktatóanyagok és erőforrások:

* [Eszköz-felhő üzeneteket küldeni egy fizikai eszközről Azure IoT oldala][lnk-physical-device]
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

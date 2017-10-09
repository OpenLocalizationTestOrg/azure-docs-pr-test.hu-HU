---
title: "SensorTag eszköz & Azure IoT átjáró - rész 3: futtassa a mintaalkalmazást |} Microsoft Docs"
description: "Egy táblázat alkalmazás tooreceive mintaadatok BLA SensorTag és az IoT hub fut."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "bla app, érzékelő alkalmazás monitorozása, érzékelő adatok gyűjtését, érzékelőket, érzékelő adatokat toocloud adatait"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: b33e53a1-1df7-4412-ade1-45185aec5bef
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4a8acdeadd402ffc82d3b766e1ec03a77ddcebb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a>Konfigurálja és BLA mintaalkalmazás futtatása

## <a name="what-you-will-do"></a>Mit fog

- Klónozás hello minta tárházba. 
- Összekapcsolhatók hello SensorTag és Intel NUC. 
- A hello Azure CLI tooget az IoT-központ és a SensorTag információk egy BLA (Bluetooth alacsony energia) mintaalkalmazás. És konfigurálása, valamint futtassa hello BLA mintaalkalmazást. 

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ebből a cikkből megtudhatja:

- Hogyan tooconfigure és futtatási hello BLA mintaalkalmazást.

## <a name="what-you-need"></a>Mi szükséges

Sikeresen végrehajtotta

- [Létrehoz egy IoT-központot, és regisztrálja a SensorTag](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a>Klónozás hello minta tárház toohello gazdaszámítógépen

tooclone hello minta tárház, kövesse az alábbi lépéseket hello állomáson:

1. Nyisson meg egy parancssort a Windows, vagy nyisson meg egy terminál macOS vagy Ubuntu.
2. Futtassa a következő parancsok hello:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-hello-connectivity-between-sensortag-and-intel-nuc"></a>Összekapcsolhatók hello SensorTag és Intel NUC

tooset mentése hello kapcsolatot, kövesse az alábbi lépéseket hello állomáson:

1. Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. Nyissa meg `config-gateway.json` a Visual Studio Code hello a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. Keresse meg a következő kódsort hello, és cserélje le `[device hostname or IP address]` hello IP cím vagy állomásnév Intel NUC annak nevét.
   ![Képernyőkép a config átjáró](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

4. Intel NUC segítő eszközök telepítése hello a következő parancs futtatásával:

   ```bash
   gulp install-tools
   ```

5. Kapcsolja be a SensorTag kép a következő hello és zöld LED kell villogni hello hello főkapcsoló lenyomásával.

   ![Kapcsolja be a Sensor Tag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. Vizsgálat SensorTag eszközökről hello a következő parancsok futtatásával:

   ```bash
   gulp discover-sensortag
   ```

7. Közötti kapcsolat működőképességét hello hello SensorTag és Intel NUC hello a következő parancs futtatásával:

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   Cserélje le `{mac address}` a hello hello előző lépésben beszerzett MAC-címet.

## <a name="get-hello-connection-string-of-sensortag"></a>Hello kapcsolati karakterlánca SensorTag beolvasása

tooget hello Azure IoT hub kapcsolati karakterlánca SensorTag, futtassa a következő parancsot a hello gazdaszámítógépen hello:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`hello IoT-központnév használt van. Az iot-átjáró használatához hello értékeként `{resource group name}` és mydevice hello értékeként `{device id}` nem módosítása hello érték a 2.

## <a name="configure-hello-ble-sample-application"></a>Hello BLA mintaalkalmazás konfigurálása

tooconfigure és futtatási hello BLA mintaalkalmazást, kövesse az alábbi lépéseket hello állomáson:

1. Nyissa meg `config-sensortag.json` a Visual Studio Code hello a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![config sensortag képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. Hajtsa végre a következő hello kód csere hello:
   - Cserélje le `[IoT hub name]` nevű hello IoT hub használt.
   - Cserélje le `[IoT device connection string]` a beszerzett SensorTag hello kapcsolati karakterlánccal.
   - Cserélje le `[device_mac_address]` a hello hello beszerzett SensorTag MAC-címét.

3. Futtassa a hello BLA mintaalkalmazást.

   toorun hello BLA mintaalkalmazást, kövesse az alábbi lépéseket hello állomáson:

   1. Kapcsolja be a SensorTag.

   2. Központi telepítése, és futtassa hello BLA mintaalkalmazás Intel NUC hello a következő parancs futtatásával:
   
      ```bash
      gulp run
      ```

## <a name="verify-that-hello-ble-sample-application-works"></a>Győződjön meg arról, hogy működik-e hello BLA mintaalkalmazás

Ekkor megjelenik egy kimeneti hasonló hello:

![BLA alkalmazás Kimenetminta](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

hello mintaalkalmazás tartja hőmérséklet adatokat gyűjt, és küldje el tooyour IoT-központ. hello mintaalkalmazás automatikusan 40 másodperc elküldése után leáll.

## <a name="summary"></a>Összefoglalás

Hogy sikeresen összekapcsolhatók hello SensorTag és Intel NUC, és adatokat gyűjt és SensorTag tooyour IoT hubról BLA mintaalkalmazás futtatása. Most készen áll a toolearn módját, amely az IoT hub kapott tooverify hello adatokat.

## <a name="next-steps"></a>Következő lépések
[Üzenetek olvasása az IoT Hubról](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)
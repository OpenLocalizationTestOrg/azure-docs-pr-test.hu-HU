---
title: "SensorTag eszköz & Azure IoT átjáró - rész 3: futtassa a mintaalkalmazást |} Microsoft Docs"
description: "Adatok fogadására BLA SensorTag és az IoT hub BLA mintaalkalmazás futtatása."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "bla app, érzékelő alkalmazás monitorozása, érzékelő adatok gyűjtését, érzékelőket, érzékelők adataiból, hogy a felhő adatait"
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
ms.openlocfilehash: f6fa158dbe1d48be7d493efa6217e1e0a759d2f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a>Konfigurálja és BLA mintaalkalmazás futtatása

## <a name="what-you-will-do"></a>Mit fog

- A minta-tárház klónozása. 
- Összekapcsolhatók a SensorTag és Intel NUC. 
- Az Azure parancssori felület használatával az IoT-központ és BLA (Bluetooth alacsony energia) mintaalkalmazás SensorTag adatait. És konfigurálása, és futtassa a BLA mintaalkalmazást. 

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ebből a cikkből megtudhatja:

- Hogyan lehet konfigurálni, és futtassa a BLA mintaalkalmazást.

## <a name="what-you-need"></a>Mi szükséges

Sikeresen végrehajtotta

- [Létrehoz egy IoT-központot, és regisztrálja a SensorTag](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a>A gazdagépnek a minta-tárház klónozása

A minta-tárház klónozása, kövesse az alábbi lépéseket az állomáson:

1. Nyisson meg egy parancssort a Windows, vagy nyisson meg egy terminál macOS vagy Ubuntu.
2. Futtassa az alábbi parancsot:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-the-connectivity-between-sensortag-and-intel-nuc"></a>Összekapcsolhatók a SensorTag és Intel NUC

A kapcsolat beállításához, kövesse az alábbi lépéseket az állomáson:

1. A konfigurációs fájl inicializálása a következő parancsok futtatásával:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. Nyissa meg `config-gateway.json` a Visual Studio Code a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. Keresse meg a következő kódsort, és cserélje le `[device hostname or IP address]` Intel NUC IP cím vagy állomásnév nevével.
   ![Képernyőkép a config átjáró](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

4. Intel NUC segítő eszközök telepítése a következő parancs futtatásával:

   ```bash
   gulp install-tools
   ```

5. A power gombra kattintva a következő képként SensorTag bekapcsolása, és a zöld LED villogni kell.

   ![Kapcsolja be a Sensor Tag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. SensorTag eszközökről ellenőrzése a következő parancsok futtatásával:

   ```bash
   gulp discover-sensortag
   ```

7. A kapcsolatok tesztelése a SensorTag és Intel NUC között a következő parancs futtatásával:

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   Cserélje le `{mac address}` az előző lépésben kapott MAC-címmel.

## <a name="get-the-connection-string-of-sensortag"></a>A kapcsolati karakterlánc SensorTag az beszerzése

Ahhoz, hogy az Azure IoT hub kapcsolati karakterlánca SensorTag, futtassa a következő parancsot az állomáson:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`az IoT-központnév használt van. Az iot-átjáró használatához értékeként `{resource group name}` mydevice használják, értékének `{device id}` Ha nem módosítja az érték a 2.

## <a name="configure-the-ble-sample-application"></a>A táblázat mintaalkalmazás konfigurálása

Konfigurálja, és futtassa a BLA mintaalkalmazást, kövesse az alábbi lépéseket az állomáson:

1. Nyissa meg `config-sensortag.json` a Visual Studio Code a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![config sensortag képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. Végezze el az alábbi új kódot:
   - Cserélje le `[IoT hub name]` nevű az IoT hub használt.
   - Cserélje le `[IoT device connection string]` SensorTag beszerzett kapcsolati karakterlánccal rendelkező.
   - Cserélje le `[device_mac_address]` a beszerzett SensorTag a MAC-címmel.

3. Futtassa a BLA mintaalkalmazást.

   Futtassa a BLA mintaalkalmazást, kövesse az alábbi lépéseket az állomáson:

   1. Kapcsolja be a SensorTag.

   2. Központi telepítése, és a BLA mintaalkalmazás Intel NUC futtassa a következő parancs futtatásával:
   
      ```bash
      gulp run
      ```

## <a name="verify-that-the-ble-sample-application-works"></a>Győződjön meg arról, hogy működik-e a BLA mintaalkalmazás

Most látnia kell a következőhöz hasonló kimenetnek:

![BLA alkalmazás Kimenetminta](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

A mintaalkalmazás tartja hőmérséklet adatokat gyűjt, és elküldi az IoT hub. A mintaalkalmazás automatikusan 40 másodperc elküldése után leáll.

## <a name="summary"></a>Összefoglalás

Hogy sikeresen összekapcsolhatók a SensorTag és Intel NUC, és futtassa a BLA mintaalkalmazást, amely összegyűjti és SensorTag adatokat küld az IoT hub. Készen áll a megtudhatja, hogyan ellenőrizheti, hogy az IoT hub kapott-e az adatokat.

## <a name="next-steps"></a>Következő lépések
[Üzenetek olvasása az IoT Hubról](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)
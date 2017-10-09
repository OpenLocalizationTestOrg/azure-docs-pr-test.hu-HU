---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 3: futtassa a mintaalkalmazást |} Microsoft Docs"
description: "A szimulált eszköz sample app toosend hőmérséklet adatok tooyour IoT-központ futtatása"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: adatok toocloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5d051d99-9749-4150-b3c8-573b0bda9c52
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: bc2c97919e95e4e3977a8b6ac75162bf2b5017be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a>Konfigurálja és a szimulált eszköz mintaalkalmazás futtatása

## <a name="what-you-will-do"></a>Mit fog

- Klónozás hello minta tárházba.
- A hello Azure CLI tooget az IoT-központ és a logikai eszköz információk szimulált eszköz mintaalkalmazáshoz. Konfigurálja, és futtassa a hello szimulált eszköz mintaalkalmazást.

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ebből a cikkből megtudhatja:

- Hogyan tooconfigure és futtatási hello szimulált eszköz mintaalkalmazást.

## <a name="what-you-need"></a>Mi szükséges

Sikeresen végrehajtotta

- [IoT Hub létrehozása és az eszköz regisztrálása](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a>Klónozás hello minta tárház toohello gazdaszámítógépen

tooclone hello minta tárház, kövesse az alábbi lépéseket hello állomáson:

1. Nyisson meg egy parancssort a Windows, vagy nyisson meg egy terminál macOS vagy Ubuntu.
2. Futtassa a következő parancsok hello:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-hello-simulated-device-and-your-nuc"></a>Hello szimulált eszköz és a NUC konfigurálása

1. Nyissa meg hello konfigurációs fájl `config.json` a Visual Studio Code hello a következő parancs futtatásával:

   ```bash
   code config.json
   ```

2. Cserélje le `"has_sensortag": true` a`"has_sensortag": false`

   ![Nem rendelkezik TI SensorTag eszközzel config](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. Nyissa meg `config-gateway.json` a Visual Studio Code hello a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. Keresse meg a következő kódsort hello, és cserélje le `[device hostname or IP address]` az IP-címét vagy állomásnevét kiszolgálónevét hello Intel NUC.
   ![Képernyőkép a config átjáró](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

## <a name="get-hello-connection-string-of-your-iot-hub-logical-device"></a>Az IoT hub logikai eszköz hello kapcsolati karakterlánc beolvasása

tooget hello Azure IoT hub kapcsolati karakterláncot a logikai eszköz, futtassa a következő parancsot a hello gazdaszámítógépen hello:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`hello IoT-központnév használt van. Az iot-átjáró használatához hello értékeként `{resource group name}` és mydevice hello értékeként `{device id}` nem módosítása hello érték a 2.

## <a name="configure-hello-simulated-device-cloud-upload-sample-application"></a>Hello szimulált eszköz felhő feltöltés mintaalkalmazás konfigurálása

tooconfigure és szimulált futási hello eszköz felhő töltse fel a mintaalkalmazást, kövesse az alábbi lépéseket hello állomáson:

1. Nyissa meg `config-sensortag.json` a Visual Studio Code hello a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![config sensortag képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. Hajtsa végre a következő hello kód csere hello:
   - Cserélje le `[IoT hub name]` hello IoT hub névvel.
   - Cserélje le `[IoT device connection string]` az IoT hub logikai eszköz hello kapcsolati karakterlánccal.

3. Hello alkalmazás futtatásához.

   Központi telepítése, és futtassa a hello alkalmazást hello a következő parancs futtatásával:

   ```bash
   gulp run
   ```

## <a name="verify-hello-sample-application-works"></a>Hello minta alkalmazás works ellenőrzése

Mostantól a következő hello hasonló kimenetnek kell megjelennie:

![alkalmazás szimulált eszköz Példakimenet.](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

hello alkalmazás küld hőmérséklet adatok tooyour IoT-központ, amely 40 másodpercig tart.

## <a name="summary"></a>Összefoglalás

Sikeresen konfigurálta, és futtassa hello szimulált eszköz felhő feltöltés mintaalkalmazás el, amely adatok tooyour IoT-központ a szimulált eszköz.

## <a name="next-steps"></a>Következő lépések
[Üzenetek olvasása az IoT Hubról](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)
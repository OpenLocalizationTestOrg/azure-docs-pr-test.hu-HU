---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 3: futtassa a mintaalkalmazást |} Microsoft Docs"
description: "Hőmérséklet-adatok küldését az IoT hub a szimulált eszköz mintaalkalmazás futtatása"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok felhőbe"
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
ms.openlocfilehash: 7df2d730c38a9f715e0fd57b4d436724a5727760
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a>Konfigurálja és a szimulált eszköz mintaalkalmazás futtatása

## <a name="what-you-will-do"></a>Mit fog

- A minta-tárház klónozása.
- Az Azure parancssori felület használatával az IoT-központ és logikai eszköz szimulált eszköz mintaalkalmazás vonatkozó információkat. Konfigurálja, és futtassa a szimulált eszköz mintaalkalmazást.

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ebből a cikkből megtudhatja:

- Hogyan lehet konfigurálni, és futtassa a szimulált eszköz mintaalkalmazást.

## <a name="what-you-need"></a>Mi szükséges

Sikeresen végrehajtotta

- [IoT Hub létrehozása és az eszköz regisztrálása](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a>A gazdagépnek a minta-tárház klónozása

A minta-tárház klónozása, kövesse az alábbi lépéseket az állomáson:

1. Nyisson meg egy parancssort a Windows, vagy nyisson meg egy terminál macOS vagy Ubuntu.
2. Futtassa az alábbi parancsot:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-the-simulated-device-and-your-nuc"></a>A szimulált eszköz és a NUC konfigurálása

1. Nyissa meg a konfigurációs fájl `config.json` a Visual Studio Code a következő parancs futtatásával:

   ```bash
   code config.json
   ```

2. Cserélje le `"has_sensortag": true` a`"has_sensortag": false`

   ![Nem rendelkezik TI SensorTag eszközzel config](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. A konfigurációs fájl inicializálása a következő parancsok futtatásával:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. Nyissa meg `config-gateway.json` a Visual Studio Code a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. Keresse meg a következő kódsort, és cserélje le `[device hostname or IP address]` az IP-címét vagy állomásnevét kiszolgálónevét az Intel NUC.
   ![Képernyőkép a config átjáró](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

## <a name="get-the-connection-string-of-your-iot-hub-logical-device"></a>A kapcsolati karakterlánc az IoT hub logikai eszköz beolvasása

Ahhoz, hogy az Azure IoT hub kapcsolati karakterlánca a logikai eszköz, a következő parancsot az állomáson:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`az IoT-központnév használt van. Az iot-átjáró használatához értékeként `{resource group name}` mydevice használják, értékének `{device id}` Ha nem módosítja az érték a 2.

## <a name="configure-the-simulated-device-cloud-upload-sample-application"></a>A szimulált eszköz felhő feltöltés mintaalkalmazás konfigurálása

Konfigurálja, és futtassa a szimulált eszköz felhő feltöltés mintaalkalmazást, kövesse az alábbi lépéseket az állomáson:

1. Nyissa meg `config-sensortag.json` a Visual Studio Code a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![config sensortag képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. Végezze el az alábbi új kódot:
   - Cserélje le `[IoT hub name]` az IoT hub nevét.
   - Cserélje le `[IoT device connection string]` az IoT hub logikai eszköz kapcsolati karakterlánccal.

3. Futtassa az alkalmazást.

   Telepíthet, és futtassa az alkalmazást a következő parancs futtatásával:

   ```bash
   gulp run
   ```

## <a name="verify-the-sample-application-works"></a>Ellenőrizze a minta alkalmazás működik

Most látnia kell a következőhöz hasonló kimenetnek:

![alkalmazás szimulált eszköz Példakimenet.](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

Az alkalmazás hőmérséklet adatokat küld az IoT hub, amely 40 másodpercig tart.

## <a name="summary"></a>Összefoglalás

Sikeresen konfigurálását és futtassa a szimulált eszköz felhő feltöltés mintaalkalmazást, amelyek adatokat küld az IoT hub szimulált eszköz.

## <a name="next-steps"></a>Következő lépések
[Üzenetek olvasása az IoT Hubról](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)
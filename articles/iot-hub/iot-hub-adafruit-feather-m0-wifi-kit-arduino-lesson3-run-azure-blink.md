---
title: "Connect Arduino (C) tooAzure IoT - lecke 3: a minta futtatásához |} Microsoft Docs"
description: "Regisztrálhat és futtathat egy minta alkalmazás tooAdafruit lágyított M0 Wi-Fi, amely tooyour IoT-központ üzeneteket küld, és villogjon-hello LED-jét."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT felhőalapú szolgáltatás, arduino küldése adatok toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 92cce319-2b17-4c9b-889d-deac959e3e7c
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ddca015a3655f8a1a9de2a00e718ec0d28a5affb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a>Futtassa egy minta alkalmazás toosend eszközről a felhőbe üzenetek
## <a name="what-you-will-do"></a>Mit fog
Ez a cikk bemutatja, hogyan toodeploy és a Adafruit lágyított M0 Wi-Fi Arduino mintaalkalmazás lefuttatva board, hogy küld üzeneteket tooyour IoT-központot.

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Megtudhatja, hogyan toouse hello eszköz toodeploy gulp és hello Arduino mintaalkalmazás futtatása a Arduino táblán.

## <a name="what-you-need"></a>Mi szükséges
* Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazás és a tárolási fiók tooprocess és a tároló IoT-központ üzenetek][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása
eszköz kapcsolati karakterlánc hello használt tooconnect az Arduino Bizottság tooyour IoT hub van. hello IoT hub kapcsolati karakterlánca az IoT hub toohello eszközidentitás a Arduino jelölő board hello IoT-központ a használt tooconnect.

* Az erőforráscsoport az IoT hub listában hello Azure CLI parancs a következő futtatásával:

```bash
az iot hub list -g iot-sample --query [].name
```

Használjon `iot-sample` hello értékeként `{resource group name}` Ha hello érték nem módosítható.

* Hello IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő parancs az Azure parancssori felület hello:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`Ez az IoT hub létrehozása, és a regisztrált a Arduino board megadott hello név.

* Hello eszköz kapcsolati karakterlánc beolvasása hello a következő parancs futtatásával:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

Használjon `mym0wifi` hello értékeként `{device id}` Ha hello érték nem módosítható.
## <a name="configure-hello-device-connection"></a>Hello eszköz kapcsolat konfigurálása
tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:

1. Hello soros port hello eszköz hello eszköz felderítési cli az beszerzése:

   ```bash
   devdisco list --usb
   ```

   Meg kell, amely hasonló toohello következő kimenetnek, és található hello usb COM-portot a Arduino Bizottság:

   ![Eszköz felderítése][device-discovery]

2. Nyissa meg hello fájl `config.json` a hello lecke mappa, és adja hozzá a COM-port száma található hello hello értékét:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > Hello COM-porthoz, a Windows platformra, hello formátuma rendelkezik `COM1, COM2, ...`. MacOS vagy Ubuntu, kezdődik `/dev/`.

3. Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. Nyissa meg hello eszköz konfigurációs fájl `config-arduino.json` a Visual Studio Code hello a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![konfiguráció-arduino.json][config-arduino-json]

5. Ellenőrizze a következő cserékhez a hello hello `config-arduino.json` fájlt:

   * Cserélje le **[Wi-Fi SSID]** rendelkező a Wi-Fi SSID toohello internethez csatlakozó.
   * Cserélje le **[Wi-Fi jelszó]** a Wi-Fi jelszóval. Távolítsa el a hello karakterlánc, ha a Wi-Fi nem követeli meg.
   * Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a hello `device connection string` beszerzett.
   * Cserélje le **[IoT hub kapcsolati karakterlánc]** a hello `iot hub connection string` beszerzett.

   > [!NOTE]
   > Nem kell `azure_storage_connection_string` ebben a cikkben. Tartsa a jelenlegi állapotában.

## <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Telepíthet, és futtassa a hello mintaalkalmazást a Arduino táblán hello a következő parancs futtatásával:

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> hello alapértelmezett gulp feladat futtatása `install-tools` és `run` feladatok egymás után. Ha Ön [hello villogási alkalmazásnak telepítve][deployed-the-blink-app], külön-külön futtatta ezeket a feladatokat.

## <a name="verify-that-hello-sample-application-works"></a>Győződjön meg arról, hogy működik-e hello mintaalkalmazás
Meg kell jelennie a hello GPIO #0 a helyi villogó LED két másodpercenként. Minden alkalommal, amikor villogjon-hello LED-jét, hello mintaalkalmazás üzenet tooyour IoT-központ küld, és ellenőrzi, hogy hello üzenet elküldése megtörtént tooyour IoT-központ. Emellett minden egyes hello IoT-központ által fogadott üzenet nyomtatása hello console ablakban. hello mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.

![A mintaalkalmazás küldött és fogadott üzenetek][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Összefoglalás
Már telepített, és a Arduino board toosend eszköz a felhőbe küldött üzeneteket tooyour IoT hub hello új villogási mintaalkalmazás futtatása. Az üzenetek most szerint toohello tárfiók írás figyelje.

## <a name="next-steps"></a>Következő lépések
[Az Azure Storage megőrzött üzenetek olvasása][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/config-arduino.png
[deployed-the-blink-app]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_run_arduino.png
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md
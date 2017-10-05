---
title: "Az Azure IoT - lecke 3 Connect Arduino (C): a minta futtatásához |} Microsoft Docs"
description: "Regisztrálhat és futtathat a Adafruit lágyított M0 Wi-Fi, amely üzeneteket küld az IoT hub és a LED villogjon mintaalkalmazást."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT felhőalapú szolgáltatás, arduino adatok felhőbe küldése"
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
ms.openlocfilehash: 0c17fe74dbd78abca955f7789a1674ed6333367f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Futtassa a mintaalkalmazást eszközről a felhőbe üzenetek küldése
## <a name="what-you-will-do"></a>Mit fog
Ez a cikk bemutatja, hogyan helyezhet üzembe és futtassa a mintaalkalmazást a Adafruit lágyított M0 Wi-Fi Arduino táblán, amely üzeneteket küld az IoT hub.

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Megtudhatja, hogyan telepítheti, és futtassa a mintaalkalmazást Arduino a Arduino táblán gulp eszközzel.

## <a name="what-you-need"></a>Mi szükséges
* Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazást és egy tárfiókot, feldolgozást és tárolást IoT hub üzenetek][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása
Az eszköz kapcsolati karakterláncát a Arduino board csatlakozni az IoT hub szolgál. Az IoT hub kapcsolati karakterlánc az IoT hub csatlakozni az eszközidentitást az IoT-központ a Arduino testület jelölő szolgál.

* Az erőforráscsoport az IoT hub listán a következő Azure CLI parancs futtatásával:

```bash
az iot hub list -g iot-sample --query [].name
```

Használjon `iot-sample` értékeként `{resource group name}` Ha az érték nem módosítható.

* Az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő Azure CLI-parancsot:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`az Ön által megadott nevét az IoT hub létrehozása, és a a Arduino board regisztrálva van.

* Az eszköz kapcsolati karakterlánc lekéréséhez futtassa a következő parancsot:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

Használjon `mym0wifi` értékeként `{device id}` Ha az érték nem módosítható.
## <a name="configure-the-device-connection"></a>Az eszköz kapcsolat konfigurálása
Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:

1. A soros port az eszköz az eszköz-felderítési cli az beszerzése:

   ```bash
   devdisco list --usb
   ```

   Tekintse meg a következőhöz hasonló kimenetnek kell, és a Arduino kártya COM-portot az usb található:

   ![Eszköz felderítése][device-discovery]

2. Nyissa meg a fájlt `config.json` a lecke mappában, és adja hozzá az érték a tényleges COM-port száma:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > A COM-port, a Windows platformra, formátuma rendelkezik `COM1, COM2, ...`. MacOS vagy Ubuntu, kezdődik `/dev/`.

3. A konfigurációs fájl inicializálása a következő parancsok futtatásával:

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. Nyissa meg az eszköz konfigurációs fájlját `config-arduino.json` a Visual Studio Code a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![konfiguráció-arduino.json][config-arduino-json]

5. Ellenőrizze a következő cserékhez a `config-arduino.json` fájlt:

   * Cserélje le **[Wi-Fi SSID]** rendelkező a Wi-Fi SSID, amely kapcsolódik az internethez.
   * Cserélje le **[Wi-Fi jelszó]** a Wi-Fi jelszóval. Távolítsa el a karakterláncot, ha a Wi-Fi nem követeli meg.
   * Cserélje le **[IoT eszköz kapcsolati karakterlánc]** rendelkező a `device connection string` beszerzett.
   * Cserélje le **[IoT hub kapcsolati karakterlánc]** rendelkező a `iot hub connection string` beszerzett.

   > [!NOTE]
   > Nem kell `azure_storage_connection_string` ebben a cikkben. Tartsa a jelenlegi állapotában.

## <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás
Telepíthet, és futtassa a mintaalkalmazást a Arduino táblán a következő parancs futtatásával:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> Az alapértelmezett gulp feladat futtatása `install-tools` és `run` feladatok egymás után. Ha Ön [a villogási alkalmazásnak telepítve][deployed-the-blink-app], külön-külön futtatta ezeket a feladatokat.

## <a name="verify-that-the-sample-application-works"></a>Győződjön meg arról, hogy működik-e a mintaalkalmazáshoz
Meg kell jelennie a GPIO #0 a helyi LED villogó két másodpercenként. Minden alkalommal, amikor a LED villogjon, a mintaalkalmazást az IoT hub üzenetet küld, és ellenőrzi, hogy az üzenet már sikeresen elküldte az IoT hub. Emellett minden üzenetet az IoT-központ fogadja a konzolablakban nyomtatása. A mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.

![A mintaalkalmazás küldött és fogadott üzenetek][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Összefoglalás
Már telepített, és futtassa a villogási új mintaalkalmazást a Arduino táblán, az IoT hub eszköz a felhőbe küldött üzeneteket küldeni. Az üzenetek most szerint a tárfiók írás figyelje.

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
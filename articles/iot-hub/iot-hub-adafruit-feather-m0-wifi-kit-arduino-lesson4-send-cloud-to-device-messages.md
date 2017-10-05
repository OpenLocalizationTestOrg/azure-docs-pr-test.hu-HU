---
title: "Csatlakozás az Azure IoT - 4. lecke Arduino (C): Felhő eszközre |} Microsoft Docs"
description: "A mintaalkalmazás az IoT hub Adafruit lágyított M0 Wi-Fi és figyeli a bejövő üzenetek futtatja. Új gulp feladat üzeneteket küld az IoT-központ a LED villogni a Adafruit lágyított M0 Wi-Fi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "a webes, a weben keresztül vezetett arduino vezérlő vezetett arduino vezérlő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: a0bf53fb-29fb-485f-ba4a-6c715057b1a2
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b4f4c1d86b10b64c104ac812d1f650d723322be8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a>Futtassa a mintaalkalmazást a felhő-eszközre küldött üzenetek fogadására
Ebben a cikkben a mintaalkalmazást a Adafruit lágyított M0 Wi-Fi Arduino táblán telepít.

A mintaalkalmazás az IoT hub bejövő üzeneteit figyeli. Gulp feladatot az IoT hub a Arduino board küldéséhez a számítógépen is futtathat. A mintaalkalmazás az üzeneteket fogad, ha azt a LED villogjon. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-do"></a>Mit fog
* A mintaalkalmazás az IoT hub való csatlakozás.
* Központi telepítése, és futtassa a mintaalkalmazást.
* Üzenetek küldése az IoT hub számítógépről a Arduino board villogni fog a LED-jét.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Az IoT hub bejövő üzenetek figyelésének módjáról.
* Az IoT hub felhő-eszközre küldött üzenetek küldése a Arduino board módjáról.

## <a name="what-you-need"></a>Mi szükséges
* A Arduino üzenőfalon, állítsa be a használatra. A Arduino board beállításával kapcsolatban a [állítsa be az eszközt][configure-your-device].
* Az IoT-központ, amely az Azure-előfizetése jön létre. Az IoT hub létrehozásával kapcsolatban lásd: [az Azure IoT Hub létrehozása][create-your-azure-iot-hub].

## <a name="connect-the-sample-application-to-your-iot-hub"></a>A mintaalkalmazás az IoT hub való csatlakozáshoz

1. Győződjön meg arról, hogy Ön a tárházban mappában `iot-hub-c-feather-m0-getting-started`.

   Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:

   ```bash
   cd Lesson4
   code .
   ```

   Figyelje meg a `app.ino` fájlt a `app` almappájában. A `app.ino` a forrás-fájl, amely tartalmazza a kódot a figyelheti a bejövő üzenetek az IoT hubból. A `blinkLED` függvény villogjon-e a LED-jét.

   ![A mintaalkalmazás a tárház szerkezete][repo-structure]

2. A soros port az eszköz az eszköz-felderítési cli az beszerzése:

   ```bash
   devdisco list --usb
   ```

   Tekintse meg a következőhöz hasonló kimenetnek kell, és a Arduino kártya COM-portot az usb található:

   ![Eszköz felderítése][device-discovery]

3. Nyissa meg a fájlt `config.json` a lecke mappában, és a bemeneti értékét a tényleges COM-port száma:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > A COM-port, a Windows platformra, formátuma rendelkezik `COM1, COM2, ...`. MacOS vagy Ubuntu, elindul a `/dev/`.

4. A konfigurációs fájl inicializálása a következő parancsok futtatásával:

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. Ellenőrizze a következő cserékhez a `config-arduino.json` fájlt:

   Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] ezen a számítógépen a konfigurációk örökölt, akkor kihagyhatja a történő központi telepítéséhez és futtatásához a mintaalkalmazást a feladatát. Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] egy másik számítógépre, ki kell cserélni a lekérdezésargumentumban lévő helyőrzőket a `config-arduino.json` fájlt. A `config-arduino.json` fájl van-e a saját almappája.

   ![A config-arduino.json fájl tartalma][config-arduino-json]

   * Cserélje le **[Wi-Fi SSID]** rendelkező a Wi-Fi SSID, amely kapcsolódik az internethez.
   * Cserélje le **[Wi-Fi jelszó]** a Wi-Fi jelszóval. Távolítsa el a karakterláncot, ha a Wi-Fi nem követeli meg.
   * Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a eszköz kapcsolati karakterlánccal, amely akkor lekéréséhez futtassa a `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` parancsot.
   * Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a `az iot hub show-connection-string --name {my hub name}` parancsot.

## <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás
Telepíthet, és futtassa a mintaalkalmazást a Arduino táblán a következő parancsok futtatásával:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

A gulp parancs telepíti a mintaalkalmazást a Arduino kártyához. Ezt követően azt futtatja az alkalmazást a Arduino tábla és egy külön feladat 20 villogási üzenetet küldeni a Arduino board-nak az IoT hub a gazdaszámítógépen.

A mintaalkalmazás futtatása után kezdődik, az IoT hub származó üzenetek figyel. Eközben a gulp feladat több "villogni" üzeneteket küld a az IoT hub a Arduino tábla. Minden egyes villogási üzenet, amely megkapja a táblán, a mintaalkalmazást meghívja a `blinkLED` működnek, mint a LED villogni.

Mivel a gulp feladat 20 üzenetet küld az IoT hub a Arduino kártyához kell megjelennie a LED villogási két másodpercenként. Legutóbb egy "stop" üzenetet, amely leállítja az alkalmazás futását.

![A mintaalkalmazás parancs gulp és üzenetek villogni][sample-application]

## <a name="summary"></a>Összefoglalás
Az IoT hub a a Arduino board a LED villogni a sikeresen elküldött üzenetek. A következő feladat nem kötelező: módosítsa a be- és kikapcsolása a LED viselkedését.

## <a name="next-steps"></a>Következő lépések
[A be- és kikapcsolása a LED viselkedését módosítása][change-the-on-and-off-led-behavior]


<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/repo_structure_arduino.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[create-an-azure-function-app-and-storage-account]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/config-arduino.png
[sample-application]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_blink_arduino.png
[change-the-on-and-off-led-behavior]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-change-led-behavior.md
---
title: "Connect Arduino (C) tooAzure IoT - lecke 4: Felhő eszközre |} Microsoft Docs"
description: "A mintaalkalmazás az IoT hub Adafruit lágyított M0 Wi-Fi és figyeli a bejövő üzenetek futtatja. Egy új gulp feladatot az IoT hub tooblink hello LED üzenetek tooAdafruit lágyított M0 Wi-Fi küldi."
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
ms.openlocfilehash: dcddd61ff684f49436103675938d719cb227c409
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a>Futtassa egy minta alkalmazás tooreceive felhő-eszközre küldött üzenetek
Ebben a cikkben a mintaalkalmazást a Adafruit lágyított M0 Wi-Fi Arduino táblán telepít.

hello mintaalkalmazást az IoT hub bejövő üzeneteit figyeli. Akkor is futtassa gulp feladat a számítógép toosend üzenetek tooyour Arduino board az IoT hub. Hello mintaalkalmazás hello üzeneteket fogad, ha azt villogjon-hello LED-jét. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-do"></a>Mit fog
* Csatlakozás hello minta alkalmazás tooyour IoT-központot.
* Regisztrálhat és futtathat hello mintaalkalmazást.
* Az IoT hub tooyour Arduino board tooblink hello LED küldhet üzeneteket.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan toomonitor bejövő az IoT hub érkező üzenetek.
* Hogyan toosend felhő eszközt az IoT hub tooyour Arduino board érkező üzenetek.

## <a name="what-you-need"></a>Mi szükséges
* A Arduino üzenőfalon, állítsa be a használatra. Hogyan fel a Arduino üzenőfalon, tooset: toolearn [állítsa be az eszközt][configure-your-device].
* Az IoT-központ, amely az Azure-előfizetése jön létre. toolearn hogyan toocreate az IoT hub, lásd: [az Azure IoT Hub létrehozása][create-your-azure-iot-hub].

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a>Csatlakozás hello minta alkalmazás tooyour IoT-központ

1. Győződjön meg arról, hogy Ön hello tárház mappában `iot-hub-c-feather-m0-getting-started`.

   Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:

   ```bash
   cd Lesson4
   code .
   ```

   Értesítés hello `app.ino` hello fájlban `app` almappájában. Hello `app.ino` hello kód toomonitor bejövő üzenetek hello IoT hubról tartalmazó hello forrás-fájl. Hello `blinkLED` függvény villogjon hello LED-jét.

   ![Hello mintaalkalmazást a tárház szerkezete][repo-structure]

2. Hello soros port hello eszköz hello eszköz felderítési cli az beszerzése:

   ```bash
   devdisco list --usb
   ```

   Meg kell, amely hasonló toohello következő kimenetnek, és található hello usb COM-portot a Arduino Bizottság:

   ![Eszköz felderítése][device-discovery]

3. Nyissa meg hello fájl `config.json` hello lecke mappában, és a bemeneti hello értékének hello található COM-port száma:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > Hello COM-porthoz, a Windows platformra, hello formátuma rendelkezik `COM1, COM2, ...`. MacOS vagy Ubuntu, elindul a `/dev/`.

4. Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. Ellenőrizze a következő cserékhez a hello hello `config-arduino.json` fájlt:

   Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] ezen a számítógépen minden hello konfigurációk örökölt, így továbbléphet hello lépés toohello feladat történő telepítésének és hello minta alkalmazást futtat. Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] egy másik számítógépen kell tooreplace hello helyőrzőt hello `config-arduino.json` fájlt. Hello `config-arduino.json` fájl van-e a kezdőmappa hello almappája.

   ![Hello config-arduino.json fájl tartalma][config-arduino-json]

   * Cserélje le **[Wi-Fi SSID]** rendelkező a Wi-Fi SSID toohello internethez csatlakozó.
   * Cserélje le **[Wi-Fi jelszó]** a Wi-Fi jelszóval. Távolítsa el a hello karakterlánc, ha a Wi-Fi nem követeli meg.
   * Cserélje le **[IoT eszköz kapcsolati karakterlánc]** hello eszköz kapcsolati karakterlánccal hello futtatásával kapott `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` parancsot.
   * Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterláncot kapott hello futtatásával hello `az iot hub show-connection-string --name {my hub name}` parancsot.

## <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Telepíthet, és futtassa a hello mintaalkalmazást a Arduino táblán hello a következő parancsok futtatásával:

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

hello gulp parancs hello minta alkalmazás tooyour Arduino board telepíti. Ezt követően hello alkalmazás az fut a Arduino tábla és egy külön feladat a gazdagépen futó számítógép toosend 20 villogási üzenetek tooyour Arduino board az IoT hub a.

Hello mintaalkalmazás futtatása után kezdődik toomessages az IoT-központ figyel. Eközben hello gulp feladat több "villogni" üzenet küldi az IoT hub tooyour Arduino tábla. A tábla hello villogási üzenetek fogadását, hello mintaalkalmazás meghívja a hello `blinkLED` függvény tooblink hello LED-jét.

Megtekintheti az hello LED villogni két másodpercenként, hello gulp feladat 20 üzenetet küldi az IoT hub tooyour Arduino tábla. hello utolsó egyik hello alkalmazás futtatását megakadályozó "stop" üzenet.

![A mintaalkalmazás parancs gulp és üzenetek villogni][sample-application]

## <a name="summary"></a>Összefoglalás
Az IoT hub tooyour Arduino board tooblink hello LED a sikeresen elküldött üzenetek. hello következő feladat nem kötelező: hello be- és kikapcsolását hello LED viselkedésének módosítása.

## <a name="next-steps"></a>Következő lépések
[Hello be- és kikapcsolását hello LED viselkedésének módosítása][change-the-on-and-off-led-behavior]


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
---
title: "Csatlakozzon az Azure IoT - 1. lecke Arduino: alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza a mintaalkalmazást Arduino a Githubból, és futtassa ezt az alkalmazást a Adafruit lágyított M0 Wi-Fi telepítendő gulp. A mintaalkalmazás villogjon-e a GPIO"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vezetett projektek, arduino vezetett villogási, vezetett arduino villogási arduino villogási program, a arduino villogási – példa"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: b0a7d076-d580-4686-9f7d-c0712750b615
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4431808ac6182d194e841c087c8f89f1a12b1911
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a>A villogóalkalmazás elkészítése és üzembe helyezése
## <a name="what-you-will-do"></a>Mit fog
Klónozza a mintaalkalmazást Arduino a Githubból, és a Adafruit lágyított M0 Wi-Fi Arduino táblán a mintaalkalmazás telepítése a gulp eszközzel. A minta alkalmazás villogás a GPIO #13 a-barod két másodpercenként vezetett.

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting-page].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
* Hogyan telepítheti, és futtassa a mintaalkalmazást a Arduino táblán.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta a következő műveleteket:

* [Állítsa be az eszközt][configure-your-device]
* [Eszközök][get-the-tools]

## <a name="open-the-sample-application"></a>Nyissa meg a mintaalkalmazás
A mintaalkalmazás megnyitásához kövesse az alábbi lépéseket:

1. A Githubból a minta-tárház klónozása a következő parancs futtatásával:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Tárház szerkezete][repo-structure]

A `app.ino` fájlt a `app` almappa is szabályozhatja a LED kódot tartalmazó kulcs forrásfájl.

### <a name="install-application-dependencies"></a>Telepítse az alkalmazásfüggőségek
A szalagtárak és egyéb modulok a következő parancs futtatásával kell a mintaalkalmazás telepítése:

```bash
npm install
```

## <a name="configure-the-device-connection"></a>Az eszköz kapcsolat konfigurálása
Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:

1. A soros port az eszköz az eszköz-felderítési cli az beszerzése:

   ```bash
   devdisco list --usb
   ```

   Kell egy a következőhöz hasonló kimenetnek és keresése a usb COM-portot a Arduino board: ![eszköz felderítése][device-discovery]

2. Nyissa meg a fájlt `config.json` a lecke mappában, és adja hozzá az érték a tényleges COM-port száma:

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > A COM-port, a Windows platformra, formátuma rendelkezik `COM1, COM2, ...`. MacOS vagy Ubuntu, kezdődik `/dev/`.

## <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás
### <a name="install-the-required-tools-for-your-arduino-board"></a>A szükséges telepíti a Arduino kártya

Az Azure IoT Hub SDK a Arduino kártya telepítése a következő parancs futtatásával:

```bash
gulp install-tools
```

Ez a feladat befejeződik, attól függően, hogy a hálózati kapcsolat hosszú időbe telhet.

> [!NOTE]
> Lépjen ki a futó Arduino IDE-példány, gulp feladatok futtatásakor: `install-tools`, `run`.

### <a name="deploy-and-run-the-sample-app"></a>Központi telepítése és a mintaalkalmazás futtatása
Telepíthet, és futtassa a mintaalkalmazást a következő parancs futtatásával:

```bash
gulp run

# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-the-app-works"></a>Ellenőrizze az alkalmazás akkor működik
Ha nem látja a LED villogó, tekintse meg a [hibaelhárítási útmutatója] [ troubleshooting-page] gyakori problémák megoldásainak.

![LED villogó][led-blinking]

## <a name="summary"></a>Összefoglalás
A Arduino board használható szükséges eszközök telepítése és a LED villogni fog a Arduino kártyához mintaalkalmazás telepítése. Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti a Arduino board Azure IoT Hub az üzeneteket küldjön és fogadjon.

## <a name="next-steps"></a>Következő lépések
[Az Azure eszközök][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
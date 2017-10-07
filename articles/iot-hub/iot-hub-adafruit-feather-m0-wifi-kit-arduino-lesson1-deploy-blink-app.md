---
title: "Csatlakozás Arduino tooAzure IoT - lecke 1: alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza Arduino mintaalkalmazás hello a Githubból, és futtassa a gulp toodeploy az alkalmazás tooyour Adafruit lágyított M0 Wi-Fi. A mintaalkalmazás villogjon hello GPIO"
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
ms.openlocfilehash: 5bf8e4ae88e070aeacf34bfc43b8d2daeeb1a2fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a>Hello villogási alkalmazás létrehozását és telepítését
## <a name="what-you-will-do"></a>Mit fog
Klónozza Arduino mintaalkalmazás hello a Githubból, és hello gulp eszköz toodeploy hello minta alkalmazás tooyour Adafruit lágyított M0 Wi-Fi Arduino tábla használatával. hello minta alkalmazás villogás hello barod GPIO #13 két másodpercenként vezetett.

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting-page].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
* Hogyan toodeploy és futtatási hello mintaalkalmazást a Arduino táblán.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta a következő műveletek hello:

* [Állítsa be az eszközt][configure-your-device]
* [Hello eszközök beszerzése][get-the-tools]

## <a name="open-hello-sample-application"></a>Nyissa meg hello mintaalkalmazás
tooopen hello mintaalkalmazást, kövesse az alábbi lépéseket:

1. A Githubból hello minta tárház klónozása hello a következő parancs futtatásával:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Tárház szerkezete][repo-structure]

Hello `app.ino` hello fájlban `app` almappa is hello kód toocontrol hello LED tartalmazó hello kulcs forrásfájl.

### <a name="install-application-dependencies"></a>Telepítse az alkalmazásfüggőségek
Hello szalagtárak és más modulok hello a következő parancs futtatásával kell hello mintaalkalmazás telepítése:

```bash
npm install
```

## <a name="configure-hello-device-connection"></a>Hello eszköz kapcsolat konfigurálása
tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:

1. Hello soros port hello eszköz hello eszköz felderítési cli az beszerzése:

   ```bash
   devdisco list --usb
   ```

   Kell egy hasonló toohello következő kimenetnek és található hello usb COM-portot a Arduino board: ![eszköz felderítése][device-discovery]

2. Nyissa meg hello fájl `config.json` a hello lecke mappa, és adja hozzá a COM-port száma található hello hello értékét:

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > Hello COM-porthoz, a Windows platformra, hello formátuma rendelkezik `COM1, COM2, ...`. MacOS vagy Ubuntu, kezdődik `/dev/`.

## <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
### <a name="install-hello-required-tools-for-your-arduino-board"></a>A Arduino kártya szükséges hello eszközök telepítése

Hello Azure IoT Hub SDK a Arduino kártya telepítése hello a következő parancs futtatásával:

```bash
gulp install-tools
```

Ez a feladat egy hosszú ideig toocomplete, attól függően, hogy a hálózati kapcsolatot is igénybe vehet.

> [!NOTE]
> Lépjen ki a Arduino IDE-példányt futtató gulp feladatok futtatásakor hello: `install-tools`, `run`.

### <a name="deploy-and-run-hello-sample-app"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Telepíthet, és futtassa a hello mintaalkalmazást hello a következő parancs futtatásával:

```bash
gulp run

# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-hello-app-works"></a>Ellenőrizze a hello az alkalmazás akkor működik
Ha nem lát hello LED villogó, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting-page] a megoldások toocommon problémákat.

![LED villogó][led-blinking]

## <a name="summary"></a>Összefoglalás
Hello szükséges eszközök toowork telepítése a Arduino üzenőfalon, és telepített egy minta alkalmazás tooyour Arduino board tooblink hello LED-jét. Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti a Arduino board tooAzure IoT-központ toosend és üzeneteket fogadni.

## <a name="next-steps"></a>Következő lépések
[Első hello Azure-eszközök][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
---
title: "Intel Edison (csomópont) csatlakozni az Azure IoT - 1. lecke: alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza a C mintaalkalmazást a Githubból, és futtassa a gulp az Intel Edison board az alkalmazás telepítéséhez. A mintaalkalmazás villogjon a kártyához csatlakoztatott két másodpercenként LED-jét."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vezetett projektek, arduino vezetett villogási, vezetett arduino villogási arduino villogási program, a arduino villogási – példa"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: ed2c21d0-c72c-4ac2-9e70-347e9a0711c0
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8490fbbf14183432c665165412f00955d6323580
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a>A villogóalkalmazás elkészítése és üzembe helyezése
## <a name="what-you-will-do"></a>Mit fog
Klónozza a C mintaalkalmazást a Githubból, és a segítségével gulp az Intel Edison minta alkalmazást telepíti. A mintaalkalmazás villogjon a kártyához csatlakoztatott két másodpercenként LED-jét. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
* Hogyan telepítheti, és futtassa a mintaalkalmazást a Edison.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta a következő műveleteket:

* [Állítsa be az eszközt][configure-your-device]
* [Eszközök][get-the-tools]

## <a name="open-the-sample-application"></a>Nyissa meg a mintaalkalmazás
A mintaalkalmazás megnyitásához kövesse az alábbi lépéseket:

1. A Githubból a minta-tárház klónozása a következő parancs futtatásával:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-edison-getting-started.git
   ```
2. Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:

   ```bash
   cd iot-hub-node-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Tárház szerkezete][repo-structure]

A fájl a `app` almappa is szabályozhatja a LED kódot tartalmazó kulcs forrásfájl.

### <a name="install-application-dependencies"></a>Telepítse az alkalmazásfüggőségek
A szalagtárak és egyéb modulok a következő parancs futtatásával kell a mintaalkalmazás telepítése:

```bash
npm install
```

## <a name="configure-the-device-connection"></a>Az eszköz kapcsolat konfigurálása
Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:

1. Az eszköz konfigurációs fájl létrehozása a következő parancs futtatásával:

   ```bash
   gulp init
   ```

   A konfigurációs fájl `config-edison.json` felhasználói hitelesítő adatokkal kell bejelentkezni Edison tartalmazza. A felhasználói hitelesítő adatok memóriavesztés elkerülése érdekében a konfigurációs fájl jön létre, almappájában `.iot-hub-getting-started` az otthoni mappa a számítógépen.

2. Nyissa meg a Visual Studio Code eszköz konfigurációs fájl a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. Cserélje le a helyőrző `[device hostname or IP address]` és `[device password]` a IP-címét és jelszavát, amely korábbi lecke jelölésű.

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

Gratulálunk! Sikeresen létrehozta a Edison első mintaalkalmazást.

## <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás

### <a name="deploy-and-run-the-sample-app"></a>Központi telepítése és a mintaalkalmazás futtatása
Telepíthet, és futtassa a mintaalkalmazást a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a>Ellenőrizze az alkalmazás akkor működik
A mintaalkalmazás automatikusan leáll, miután a LED villogjon-20 alkalommal a. Ha nem látja a LED villogó, tekintse meg a [hibaelhárítási útmutatója] [ troubleshooting] gyakori problémák megoldásainak.

![LED villogó][led-blinking]

## <a name="summary"></a>Összefoglalás
A szükséges eszközök Edison használható telepítése és a LED villogni fog Edison a mintaalkalmazás telepítése. Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti Edison Azure IoT Hub az üzeneteket küldjön és fogadjon.

## <a name="next-steps"></a>Következő lépések
[Az Azure eszközök][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md

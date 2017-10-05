---
title: "Intel Edison (csomópont) csatlakozni az Azure IoT - lecke 4: üzeneteket fogadni |} Microsoft Docs"
description: "A mintaalkalmazás Edison futtatja, és figyeli a bejövő üzenetek az IoT hub. Új gulp feladat üzeneteket küld a az IoT-központ a LED villogni a Edison."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "a webes, a weben keresztül vezetett arduino vezérlő vezetett arduino vezérlő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: bc738bf6-e38d-4024-82d7-39b6c2d4bacb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 76ea59acd848f60663a0c821bff42166aac5823a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a>Futtassa a mintaalkalmazást a felhő-eszközre küldött üzenetek fogadására
Ebben a cikkben az Intel Edison minta alkalmazást telepít központilag. A mintaalkalmazás az IoT hub bejövő üzeneteit figyeli. Gulp feladatot az IoT hub Edison küldéséhez a számítógépen is futtathat. A mintaalkalmazás az üzeneteket fogad, ha azt a LED villogjon. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-do"></a>Mit fog
* A mintaalkalmazás az IoT hub való csatlakozás.
* Központi telepítése, és futtassa a mintaalkalmazást.
* Üzenetek küldése az IoT hub számítógépről Edison villogni fog a LED-jét.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Az IoT hub bejövő üzenetek figyelésének módjáról.
* Az IoT hub felhő-eszközre küldött üzenetek küldése Edison módjáról.

## <a name="what-you-need"></a>Mi szükséges
* Intel Edison, állítsa be a használatra. Edison beállításával kapcsolatban a [állítsa be az eszközt][configure-your-device].
* Az IoT-központ, amely az Azure-előfizetése jön létre. Az IoT hub létrehozásával kapcsolatban lásd: [az Azure IoT Hub létrehozása][create-your-azure-iot-hub].

## <a name="connect-the-sample-application-to-your-iot-hub"></a>A mintaalkalmazás az IoT hub való csatlakozáshoz
1. Győződjön meg arról, hogy Ön a tárházban mappában `iot-hub-node-edison-getting-started`. Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:

   ```bash
   cd Lesson4
   code .
   ```

   A fájl a `app` almappa is figyelheti a bejövő üzenetek az IoT hubból kódot tartalmazó kulcs forrásfájl. A `blinkLED` függvény villogjon-e a LED-jét.

   ![A mintaalkalmazás a tárház szerkezete][repo-structure]
2. A konfigurációs fájl inicializálása a következő parancsok futtatásával:

   ```bash
   npm install
   gulp init
   ```

   Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] ezen a számítógépen a konfigurációk örökölt, akkor kihagyhatja a történő központi telepítéséhez és futtatásához a mintaalkalmazást a feladatát. Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] egy másik számítógépre, ki kell cserélni a lekérdezésargumentumban lévő helyőrzőket a `config-edison.json` fájlt. A `config-edison.json` fájl van-e a saját almappája.

   ![A config-edison.json fájl tartalma](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * Cserélje le **[eszköz állomásnév vagy IP-címe]** az eszköz konfigurálása során az lefelé megjelölt eszköz IP-címmel.
   * Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a eszköz kapcsolati karakterlánccal, amely akkor lekéréséhez futtassa a `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` parancsot.
   * Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a `az iot hub show-connection-string --name {my hub name}` parancsot.

## <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás
Telepíthet, és futtassa a mintaalkalmazást a Edison a következő parancsok futtatásával:

```bash
gulp deploy && gulp run
```

A gulp parancs a mintaalkalmazáshoz történő Edison telepíti. Az alkalmazás ezt követően az Edison és egy külön feladat számítógépen történő üzenetküldéshez 20 villogási Edison az IoT hub a fut.

A mintaalkalmazás futtatása után kezdődik, az IoT hub származó üzenetek figyel. Eközben a gulp feladat több "villogni" üzeneteket küld a az IoT hub Edison. Minden egyes villogási üzenet, amely megkapja a Edison, a mintaalkalmazást meghívja a `blinkLED` működnek, mint a LED villogni.

A gulp feladat 20 üzeneteket küld a az IoT hub Edison, meg kell jelennie a LED villogási két másodpercenként. Legutóbb egy "stop" üzenetet, amely leállítja az alkalmazás futását.

![A mintaalkalmazás parancs gulp és üzenetek villogni][gulp-command-and-blink-messages]

## <a name="summary"></a>Összefoglalás
Az IoT hub a Edison a LED villogni a sikeresen elküldött üzenetek. A következő feladat nem kötelező: módosítsa a be- és kikapcsolása a LED viselkedését.

## <a name="next-steps"></a>Következő lépések
[A be- és kikapcsolása a LED viselkedését módosítása][change-the-on-and-off-behavior-of-the-led]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-node-lesson4-change-led-behavior.md
---
title: "Connect Intel Edison (C) tooAzure IoT - lecke 4: üzeneteket fogadni |} Microsoft Docs"
description: "A mintaalkalmazás Edison futtatja, és figyeli a bejövő üzenetek az IoT hub. Egy új gulp feladatot az IoT hub tooblink hello LED üzenetek tooEdison küldi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "a webes, a weben keresztül vezetett arduino vezérlő vezetett arduino vezérlő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 820d38f3-d3b8-4249-9e2b-f1b9b771e62f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f0424506ff755e0b9514684787b37584d406d320
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a>Futtassa egy minta alkalmazás tooreceive felhő-eszközre küldött üzenetek
Ebben a cikkben az Intel Edison minta alkalmazást telepít központilag. hello mintaalkalmazást az IoT hub bejövő üzeneteit figyeli. Akkor is futtassa gulp feladat a számítógép toosend üzenetek tooEdison az IoT hub. Hello mintaalkalmazás hello üzeneteket fogad, ha azt villogjon-hello LED-jét. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-do"></a>Mit fog
* Csatlakozás hello minta alkalmazás tooyour IoT-központot.
* Regisztrálhat és futtathat hello mintaalkalmazást.
* Az IoT hub tooEdison tooblink hello LED küldhet üzeneteket.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan toomonitor bejövő az IoT hub érkező üzenetek.
* Hogyan toosend felhő eszközt az IoT hub tooEdison érkező üzenetek.

## <a name="what-you-need"></a>Mi szükséges
* Intel Edison, állítsa be a használatra. Hogyan mentése Edison, tooset: toolearn [állítsa be az eszközt][configure-your-device].
* Az IoT-központ, amely az Azure-előfizetése jön létre. toolearn hogyan toocreate az IoT hub, lásd: [az Azure IoT Hub létrehozása][create-your-azure-iot-hub].

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a>Csatlakozás hello minta alkalmazás tooyour IoT-központ
1. Győződjön meg arról, hogy Ön hello tárház mappában `iot-hub-c-edison-getting-started`. Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:

   ```bash
   cd Lesson4
   code .
   ```

   hello hello fájlban `app` almappa is hello kód toomonitor bejövő üzenetek hello IoT hubról tartalmazó hello kulcs forrásfájl. Hello `blinkLED` függvény villogjon hello LED-jét.

   ![Hello mintaalkalmazást a tárház szerkezete][repo-structure]
2. Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:

   ```bash
   npm install
   gulp init
   ```

   Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] ezen a számítógépen minden hello konfigurációk örökölt, így továbbléphet hello lépés toohello feladat történő telepítésének és hello minta alkalmazást futtat. Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] egy másik számítógépen kell tooreplace hello helyőrzőt hello `config-edison.json` fájlt. Hello `config-edison.json` fájl van-e a kezdőmappa hello almappája.

   ![Hello config-edison.json fájl tartalma](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * Cserélje le **[eszköz állomásnév vagy IP-címe]** hello eszköz IP-cím az eszköz konfigurálása során az lefelé jelölésű.
   * Cserélje le **[IoT eszköz kapcsolati karakterlánc]** hello eszköz kapcsolati karakterlánccal hello futtatásával kapott `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` parancsot.
   * Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterláncot kapott hello futtatásával hello `az iot hub show-connection-string --name {my hub name}` parancsot.

   > [!NOTE]
   > Futtatás **gulp az install-eszközök** és, ha még nem meg lecke 1.

## <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Központi telepítése, és futtassa a mintaalkalmazást hello Edison hello a következő parancsok futtatásával:

```bash
gulp deploy && gulp run
```

hello gulp parancs hello minta alkalmazás tooEdison telepíti. Ezt követően azt futtatja hello alkalmazás Edison és a gazdagép egy külön feladat számítógép toosend 20 villogási üzenetek tooEdison az IoT hub.

Hello mintaalkalmazás futtatása után kezdődik toomessages az IoT-központ figyel. Eközben hello gulp feladat több "villogni" üzenetet küldi az IoT hub tooEdison. Minden egyes villogási üzenet, amely megkapja a Edison, hello mintaalkalmazás meghívja a hello `blinkLED` függvény tooblink hello LED-jét.

Feladat küldése 20 üzenetet az IoT hub tooEdison a gulp LED hello villogási hello mint két másodpercenként kell megjelennie. hello utolsó egyik hello alkalmazás futtatását megakadályozó "stop" üzenet.

![A mintaalkalmazás parancs gulp és üzenetek villogni][gulp-command-and-blink-messages]

## <a name="summary"></a>Összefoglalás
Az IoT hub tooEdison tooblink hello LED a sikeresen elküldött üzenetek. hello következő feladat nem kötelező: hello be- és kikapcsolását hello LED viselkedésének módosítása.

## <a name="next-steps"></a>Következő lépések
[Hello be- és kikapcsolását hello LED viselkedésének módosítása][change-the-on-and-off-behavior-of-the-led]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure_c.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink_c.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-c-lesson4-change-led-behavior.md
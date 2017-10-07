---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 4: Felhő eszközre |} Microsoft Docs"
description: "A mintaalkalmazás Pi futtatja, és figyeli a bejövő üzenetek az IoT hub. Egy új gulp feladatot az IoT hub tooblink hello LED üzenetek tooPi küldi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "felhő toodevice, üzenet a felhőből"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: fcbc0dd0-cae3-47b0-8e58-240e4f406f75
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5596bf3a83c21f2bd54b2f83e2a8fdad7a608b94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a>Futtassa egy minta alkalmazás tooreceive felhő-eszközre küldött üzenetek
Ebben a cikkben a málna Pi 3 minta alkalmazást telepít központilag. hello mintaalkalmazást az IoT hub bejövő üzeneteit figyeli. Akkor is futtassa gulp feladat a számítógép toosend üzenetek tooPi az IoT hub. Hello mintaalkalmazás hello üzeneteket fogad, ha azt villogjon-hello LED-jét. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-do"></a>Mit fog
* Csatlakozás hello minta alkalmazás tooyour IoT-központot.
* Regisztrálhat és futtathat hello mintaalkalmazást.
* Az IoT hub tooPi tooblink hello LED küldhet üzeneteket.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan toomonitor bejövő az IoT hub érkező üzenetek.
* Hogyan toosend felhő eszközt az IoT hub tooPi érkező üzenetek.

## <a name="what-you-need"></a>Mi szükséges
* Raspberry Pi 3, állítsa be a használatra. Hogyan tooset fel a Pi: toolearn [állítsa be az eszközt](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).
* Az IoT-központ, amely az Azure-előfizetése jön létre. toolearn hogyan toocreate az IoT hub, lásd: [az IoT hub létrehozni és regisztrálni az málna Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a>Csatlakozás hello minta alkalmazás tooyour IoT-központ
1. Győződjön meg arról, hogy Ön hello tárház mappában `iot-hub-c-raspberrypi-getting-started`. Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:

   ```bash
   cd Lesson4
   code .
   ```

   Értesítés hello `app.c` hello fájlban `app` almappájában. Hello `app.c` hello kód toomonitor bejövő üzenetek hello IoT hubról tartalmazó hello forrás-fájl. Hello `blinkLED` függvény villogjon hello LED-jét.

   ![Hello mintaalkalmazást a tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure_c.png)
2. Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:

   ```bash
   npm install
   gulp init
   ```

   Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) ezen a számítógépen minden hello konfigurációk örökölt, így továbbléphet toostep toohello feladatütemezés központi telepítését és futó mintaalkalmazást hello. Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) egy másik számítógépen kell tooreplace hello helyőrzőt hello `config-raspberrypi.json` fájlt. Hello `config-raspberrypi.json` fájl van-e a kezdőmappa hello almappája.

   ![Hello config-raspberrypi.json fájl tartalma](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* Cserélje le **[eszköz állomásnév vagy IP-címe]** a Pi tartozó IP-cím vagy név hello futtatásával kapott `devdisco list --eth` parancsot.
* Cserélje le **[IoT eszköz kapcsolati karakterlánc]** hello eszköz kapcsolati karakterlánccal hello futtatásával kapott `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` parancsot.
* Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterláncot kapott hello futtatásával hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` parancsot.

> [!NOTE]
> Futtatás **gulp az install-eszközök** és, ha még nem meg lecke 1.

## <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Központi telepítése, és futtassa a mintaalkalmazást hello Pi hello a következő parancsok futtatásával:

```
gulp deploy && gulp run
```

hello gulp parancs futtatása először hello install-eszközök feladat. Majd hello minta alkalmazás tooPi telepíti. Végül azt futtatja hello alkalmazás Pi és a gazdagép egy külön feladat számítógép toosend 20 villogási üzenetek tooPi az IoT hub.

Hello mintaalkalmazás futtatása után kezdődik toomessages az IoT-központ figyel. Eközben hello gulp feladat több "villogni" üzenetet küldi az IoT hub tooPi. Minden egyes villogási üzenet, amely megkapja a Pi, hello mintaalkalmazás meghívja a hello `blinkLED` függvény tooblink hello LED-jét.

Feladat küldése 20 üzenetet az IoT hub tooPi a gulp LED hello villogási hello mint két másodpercenként kell megjelennie. hello utolsó egyik hello alkalmazás futtatását megakadályozó "stop" üzenet.

![A mintaalkalmazás parancs gulp és üzenetek villogni](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink_c.png)

## <a name="summary"></a>Összefoglalás
Az IoT hub tooPi tooblink hello LED a sikeresen elküldött üzenetek. hello következő feladat nem kötelező: hello be- és kikapcsolását hello LED viselkedésének módosítása.

## <a name="next-steps"></a>Következő lépések
[Hello be- és kikapcsolását hello LED viselkedésének módosítása](iot-hub-raspberry-pi-kit-c-lesson4-change-led-behavior.md)

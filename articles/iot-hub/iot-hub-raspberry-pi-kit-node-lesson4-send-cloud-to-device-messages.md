---
featureFlags: usabilla
title: "Csatlakozás az Azure IoT - 4. lecke Raspberry Pi (csomópont): Cloud eszközre |} Microsoft Docs"
description: "A mintaalkalmazás Pi futtatja, és figyeli a bejövő üzenetek az IoT hub. Új gulp feladat üzeneteket küld a az IoT-központ a LED villogni a Pi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "a felhő eszközhöz, message a felhőből"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6ae6539e-1163-4490-8d72-fdf7803e3054
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b8e9e51391f9b6737762b3404658297ab4c82783
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Futtassa a mintaalkalmazást a felhő-eszközre küldött üzenetek fogadására
Ebben a cikkben a málna Pi 3 minta alkalmazást telepít központilag. A mintaalkalmazás az IoT hub bejövő üzeneteit figyeli. Gulp feladatot az IoT hub Pi küldéséhez a számítógépen is futtathat. A mintaalkalmazás az üzeneteket fogad, ha azt a LED villogjon. Ha bármilyen problémába ütközik, a keresés megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-do"></a>Mit fog
* A mintaalkalmazás az IoT hub való csatlakozás.
* Központi telepítése, és futtassa a mintaalkalmazást.
* Üzenetek küldése az IoT hub számítógépről Pi villogni fog a LED-jét.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Az IoT hub bejövő üzenetek figyelése
* Hogyan küldhetők felhő-eszközre küldött üzenetek az IoT hub-Pi tartományban.

## <a name="what-you-need"></a>Mi szükséges
* Raspberry Pi 3, állítsa be a használatra. A Pi beállításával kapcsolatban a [állítsa be az eszközt](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).
* Az IoT-központ, amely az Azure-előfizetése jön létre. Az IoT hub létrehozásával kapcsolatban lásd: [az IoT hub létrehozni és regisztrálni az málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).

## <a name="connect-the-sample-application-to-your-iot-hub"></a>A mintaalkalmazás az IoT hub való csatlakozáshoz
1. Győződjön meg arról, hogy Ön a tárházban mappában `iot-hub-node-raspberrypi-getting-started`. Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:
   
   ```bash
   cd Lesson4
   code .
   ```
   
   Figyelje meg a `app.js` fájlt a `app` almappájában. A `app.js` a forrás-fájl, amely tartalmazza a kódot a figyelheti a bejövő üzenetek az IoT hubból. A `blinkLED` függvény villogjon-e a LED-jét.
   
   ![A mintaalkalmazás a tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)
2. A konfigurációs fájl inicializálása a következő parancsokkal:
   
   ```bash
   npm install
   gulp init
   ```
   
   Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) ezen a számítógépen a konfigurációk örökölt, így továbbléphet a központi telepítéséhez és futtatásához a mintaalkalmazást a feladatát. Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) egy másik számítógépre, ki kell cserélni a lekérdezésargumentumban lévő helyőrzőket a `config-raspberrypi.json` fájlt. A `config-raspberrypi.json` fájl van-e a saját almappája.
   
   ![A config-raspberrypi.json fájl tartalma](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* Cserélje le **[eszköz állomásnév vagy IP-címe]** Pi vagy az állomásnév futtatásával kapott IP-címét a `devdisco list --eth` parancsot.
* Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a eszköz kapcsolati karakterlánccal, amely akkor lekéréséhez futtassa a `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` parancsot.
* Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` parancsot.

> [!NOTE]
> Futtatás **gulp az install-eszközök** és, ha még nem meg lecke 1.

## <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás
Telepíthet, és futtassa a mintaalkalmazást a Pi a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

A parancs telepíti a mintaalkalmazás-Pi tartományban. Az alkalmazás ezt követően az Pi és a számítógépen egy külön feladat 20 villogási üzenetet küldeni Pi-nak az IoT hub fut.

A mintaalkalmazás futtatása után kezdődik, az IoT hub származó üzenetek figyel. Eközben a gulp feladat küld több "villogni" üzenetek az IoT hub-Pi tartományban. Minden egyes villogási üzenet, amely megkapja a Pi, a mintaalkalmazást meghívja a `blinkLED` működnek, mint a LED villogni.

Meg kell jelennie a LED villogási két másodpercenként, mint a gulp feladat 20 üzenetet küld az IoT hub-Pi tartományban. Legutóbb egy "stop" üzenet, amely közli az alkalmazás leáll.

![A mintaalkalmazás parancs gulp és üzenetek villogni](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="summary"></a>Összefoglalás
Az IoT hub a pi értékét a LED villogni sikeresen elküldött üzenetek. A következő feladat nem kötelező: módosítsa a be- és kikapcsolása a LED viselkedését.

## <a name="next-steps"></a>Következő lépések
[A be- és kikapcsolása a LED viselkedését módosítása](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


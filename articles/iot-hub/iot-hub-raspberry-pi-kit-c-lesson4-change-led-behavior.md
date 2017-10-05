---
title: "Csatlakozás az Azure IoT - 4. lecke Raspberry Pi (C): alkalmazás módosítása |} Microsoft Docs"
description: "A LED be- és kikapcsolását viselkedésének módosítása az üzenetek testreszabhatók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vezérlő vezetett raspberry pi raspberry vezetett pi vezérlő raspberry pi vezérléssel vezetett"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 0201b8ed-d5e6-4445-9a4d-1305003d1eff
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b1e441b20e161f4a03d4c2c300b21aca4fedb2a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>A be- és kikapcsolása a LED viselkedését módosítása
## <a name="what-you-will-do"></a>Mit fog
A LED be- és kikapcsolását viselkedésének módosítása az üzenetek testreszabhatók. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
További Node.js funkciók segítségével módosíthatja a LED be- és kikapcsolását viselkedését.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta [futtassa a mintaalkalmazást a felhőből az eszközre küldött üzenetek fogadására málna Pi](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).

## <a name="add-functions-to-mainc-and-gulpfilejs"></a>Funkciók hozzáadása main.c és gulpfile.js
1. Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:

   ```bash
   cd Lesson4
   code .
   ```
2. Nyissa meg a `main.c` fájlt, és adja hozzá a következő funkciók blinkLED() függvény után:

   ```c
   static void turnOnLED()
   {
     digitalWrite(LED_PIN, HIGH);
   }

   static void turnOffLED()
   {
     digitalWrite(LED_PIN, LOW);
   }
   ```

   ![a hozzáadott funkciók Main.c fájl](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_c.png)
3. Adja hozzá a következő feltételek előtt az alapértelmezett a `if` blokkolja a `receiveMessageCallback` függvény:

   ```c
   else if (0 == strcmp((const char*)value, "\"on\""))
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\""))
   {
       turnOffLED();
   }
   ```

   Most már konfigurálta az válaszolni üzenetekben további információkat a mintaalkalmazáshoz. Az "on" utasítás bekapcsolja a LED-jét, és az "off" utasítás kikapcsolja a LED-jét.
4. Nyissa meg a gulpfile.js fájlt, és adja hozzá az új függvény előtt a függvény `sendMessage`:

   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   }
   ```

   ![A hozzáadott funkcióval Gulpfile.js fájl](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile_c.png)
5. Az a `sendMessage` működik, cserélje le a sor `var message = buildMessage(sentMessageCount);` a új sorral a következő kódrészletben látható:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. A módosítások mentéséhez.

### <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás
Telepíthet, és futtassa a mintaalkalmazást a Pi a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

A két másodpercen bekapcsolása LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie. Az utolsó "stop" üzenet leállítja a mintaalkalmazás futtatását.

![Mintaalkalmazást a be- és kikapcsolását üzenetek](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off_c.png)

Gratulálunk! Sikeresen testre szabta az IoT hub-a-pi tartományban küldött állapotüzenetek.

### <a name="summary"></a>Összefoglalás
Ez nem kötelező a szakasz bemutatja, hogyan kell az üzenetek testreszabhatók, hogy a mintaalkalmazás képes kezelni a be és ki a LED viselkedését eltérő módon.

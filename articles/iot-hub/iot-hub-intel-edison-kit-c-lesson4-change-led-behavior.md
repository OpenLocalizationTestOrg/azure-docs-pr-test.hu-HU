---
title: 'Intel Edison (C) csatlakozni az Azure IoT - lecke 4: a LED villogni |} Microsoft Docs'
description: "A LED be- és kikapcsolását viselkedésének módosítása az üzenetek testreszabhatók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "a arduino vezetett vezérlő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 9826c55a-0e24-4296-ae54-29b7fe66436a
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4852b1cca4c6186ef4857b903b771f76cc20adb8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>A be- és kikapcsolása a LED viselkedését módosítása
## <a name="what-you-will-do"></a>Mit fog
A LED be- és kikapcsolását viselkedésének módosítása az üzenetek testreszabhatók. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
További funkciók segítségével módosíthatja a LED be- és kikapcsolását viselkedését.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta [futtassa a mintaalkalmazást a felhőből az eszközre küldött üzenetek fogadására Intel Edison][receive-cloud-to-device-messages].

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
     mraa_gpio_write(context, 1);
   }

   static void turnOffLED()
   {
     mraa_gpio_write(context, 0);
   }
   ```

   ![a hozzáadott funkciók Main.c fájl](media/iot-hub-intel-edison-lessons/lesson4/updated_app_c.png)

3. Adja meg az alábbi feltételeket előtt a `else if` a letiltása a `receiveMessageCallback` függvény:

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

   ![A hozzáadott funkcióval Gulpfile.js fájl][gulpfile]
5. Az a `sendMessage` működik, cserélje le a sor `var message = buildMessage(sentMessageCount);` a új sorral a következő kódrészletben látható:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. A módosítások mentéséhez.

### <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás
Telepíthet, és futtassa a mintaalkalmazást a Edison a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

A két másodpercen bekapcsolása LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie. Az utolsó "stop" üzenet leállítja a mintaalkalmazás futtatását.

![be- és kikapcsolása][on-and-off]

Gratulálunk! Az IoT hub a Edison küldött üzenetek sikeresen szabta.

### <a name="summary"></a>Összefoglalás
Ez nem kötelező a szakasz bemutatja, hogyan kell az üzenetek testreszabhatók, hogy a mintaalkalmazás képes kezelni a be és ki a LED viselkedését eltérő módon.

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_c.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_c.png

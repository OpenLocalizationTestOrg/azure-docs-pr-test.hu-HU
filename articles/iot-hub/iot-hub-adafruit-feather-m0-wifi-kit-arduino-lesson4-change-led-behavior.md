---
title: "Connect Arduino (C) tooAzure IoT - lecke 4: alkalmazás módosítása |} Microsoft Docs"
description: "Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "a arduino vezetett vezérlő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: d7a25430-450e-43c4-a3ed-1eed995b8b7e
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8cc438650f01ae4335d91c94df6a29e0ffbdc508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Hello be- és kikapcsolását hello LED viselkedésének módosítása
## <a name="what-you-will-do"></a>Mit fog
Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) a Adafruit lágyított M0 Wi-Fi Arduino kártya.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
További Arduino funkciók toochange LED tartozó be- és kikapcsolását viselkedés hello használata.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta [toodevice üzenetek futtassa a mintaalkalmazást a Arduino board tooreceive felhő][receive-cloud-to-device-messages].

## <a name="add-functions-toomainc-and-gulpfilejs"></a>Funkciók toomain.c és gulpfile.js hozzáadása
1. Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:

   ```bash
   cd Lesson4
   code .
   ```
2. Nyissa meg hello `app.ino` fájlt, és adja hozzá a következő funkciók blinkLED() függvény után hello:

   ```arduino
   static void turnOnLED()
   {
     digitalWrite(LED_PIN, HIGH);
   }

   static void turnOffLED()
   {
     digitalWrite(LED_PIN, LOW);
   }
   ```

   ![a hozzáadott funkciók App.ino fájl][app-ino-file]
3. Adja hozzá a következő feltételek előtt hello hello `else if` hello adatblokk `receiveMessageCallback` függvény:

   ```arduino
   else if (strcmp((const char*)value, "\"on\"") == 0)
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\"") == 0)
   {
       turnOffLED();
   }
   ```

   Most már konfigurálta az hello alkalmazás toorespond toomore vonatkozó példautasításokat üzenetekben. hello "on" utasítás bekapcsolja a hello LED-jét, és hello "off" utasítás kikapcsolása hello LED-jét.
4. Nyissa meg a hello gulpfile.js fájlt, és adja hozzá a hello függvény előtt egy új funkció `sendMessage`:

   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   };
   ```

   ![A hozzáadott funkcióval Gulpfile.js fájl][gulp-file-js]
5. A hello `sendMessage` működik, cserélje le a hello sor `var message = buildMessage(sentMessageCount);` hello új sorral hello a következő kódrészletben látható:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Minden hello módosítások mentéséhez.

### <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Telepíthet, és futtassa a hello mintaalkalmazást a Arduino táblán hello a következő parancs futtatásával:

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

Két másodpercen bekapcsolása hello LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie. utolsó "stop" üdvözlőüzenetére hello mintaalkalmazás futtatását leáll.

![be- és kikapcsolása][on-and-off]

Gratulálunk! Az IoT hub tooyour Arduino board küldi hello üzenetek sikeresen szabta.

### <a name="summary"></a>Összefoglalás
A választható szakasz azt mutatja be, hogyan toocustomize üzenetek, hogy hello mintaalkalmazás eltérő módon szabályozhatja hello be- és kikapcsolását hello LED viselkedését.

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png
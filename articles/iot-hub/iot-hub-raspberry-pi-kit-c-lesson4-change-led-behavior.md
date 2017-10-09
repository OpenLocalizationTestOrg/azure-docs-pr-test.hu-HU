---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 4: alkalmazás módosítása |} Microsoft Docs"
description: "Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása."
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
ms.openlocfilehash: f4739c4e9a58b4b0fe964b5c3c81e5918982099f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Hello be- és kikapcsolását hello LED viselkedésének módosítása
## <a name="what-you-will-do"></a>Mit fog
Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
További Node.js funkciók toochange LED tartozó be- és kikapcsolását viselkedés hello használata.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta [toodevice üzenetek futtassa a mintaalkalmazást málna Pi tooreceive felhő](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).

## <a name="add-functions-toomainc-and-gulpfilejs"></a>Funkciók toomain.c és gulpfile.js hozzáadása
1. Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:

   ```bash
   cd Lesson4
   code .
   ```
2. Nyissa meg hello `main.c` fájlt, és adja hozzá a következő funkciók blinkLED() függvény után hello:

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
3. Adja hozzá a hello alapértelmezett kikötéseket követő hello hello `if` hello adatblokk `receiveMessageCallback` függvény:

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
   }
   ```

   ![A hozzáadott funkcióval Gulpfile.js fájl](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile_c.png)
5. A hello `sendMessage` működik, cserélje le a hello sor `var message = buildMessage(sentMessageCount);` hello új sorral hello a következő kódrészletben látható:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Minden hello módosítások mentéséhez.

### <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Központi telepítése, és futtassa a mintaalkalmazást hello Pi hello a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

Két másodpercen bekapcsolása hello LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie. utolsó "stop" üdvözlőüzenetére hello mintaalkalmazás futtatását leáll.

![Mintaalkalmazást a be- és kikapcsolását üzenetek](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off_c.png)

Gratulálunk! Sikeresen testre szabta az IoT hub tooPi küldi hello üzeneteket.

### <a name="summary"></a>Összefoglalás
A választható szakasz azt mutatja be, hogyan toocustomize üzenetek, hogy hello mintaalkalmazás eltérő módon szabályozhatja hello be- és kikapcsolását hello LED viselkedését.

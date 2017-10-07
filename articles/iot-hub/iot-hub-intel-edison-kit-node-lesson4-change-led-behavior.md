---
title: "Csatlakozás Intel Edison (csomópont) tooAzure IoT - lecke 4: hello LED villogni |} Microsoft Docs"
description: "Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "a arduino vezetett vezérlő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 387cd97e-b05e-43c4-b252-f68ad45d524a
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: caeabe311fd1698f298c6d2b4a203ecad80ef7df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Hello be- és kikapcsolását hello LED viselkedésének módosítása
## <a name="what-you-will-do"></a>Mit fog
Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
További funkciók toochange hello LED tartozó be- és kikapcsolását viselkedés használja.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta [toodevice üzenetek futtassa a mintaalkalmazást Intel Edison tooreceive felhő][receive-cloud-to-device-messages].

## <a name="add-functions-tooappjs-and-gulpfilejs"></a>Funkciók tooapp.js és gulpfile.js hozzáadása
1. Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:

   ```bash
   cd Lesson4
   code .
   ```
2. Nyissa meg hello `app.js` fájlt, és adja hozzá a következő funkciók blinkLED() függvény után hello:

   ```javascript
   function turnOnLED() {
     myLed.write(1);
   }

   function turnOffLED() {
     myLed.write(0);
   }
   ```

   ![a hozzáadott funkciók App.js fájlban](media/iot-hub-intel-edison-lessons/lesson4/updated_app_node.png)
3. Adja hozzá a hello hello előtt az alábbi feltételek "villogni" hello kapcsoló blokkméretet hello nagybetűkkel `receiveMessageCallback` függvény:

   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
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

   ![A hozzáadott funkcióval Gulpfile.js fájl][gulpfile]
5. A hello `sendMessage` működik, cserélje le a hello sor `var message = buildMessage(sentMessageCount);` hello új sorral hello a következő kódrészletben látható:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Minden hello módosítások mentéséhez.

### <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Központi telepítése, és futtassa a mintaalkalmazást hello Edison hello a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

Két másodpercen bekapcsolása hello LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie. utolsó "stop" üdvözlőüzenetére hello mintaalkalmazás futtatását leáll.

![be- és kikapcsolása][on-and-off]

Gratulálunk! Sikeresen testre szabta az IoT hub tooEdison küldi hello üzeneteket.

### <a name="summary"></a>Összefoglalás
A választható szakasz azt mutatja be, hogyan toocustomize üzenetek, hogy hello mintaalkalmazás eltérő módon szabályozhatja hello be- és kikapcsolását hello LED viselkedését.

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-node-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_node.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_node.png

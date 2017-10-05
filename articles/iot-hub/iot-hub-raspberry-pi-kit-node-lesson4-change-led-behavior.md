---
title: "Csatlakozás az Azure IoT - 4. lecke Raspberry Pi (csomópont): alkalmazások módosítása |} Microsoft Docs"
description: "A LED be- és kikapcsolását viselkedésének módosítása az üzenetek testreszabhatók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "vezérlő vezetett raspberry pi raspberry vezetett pi vezérlő raspberry pi vezérléssel vezetett"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 3b42a4ad-0197-42f6-8ca9-04c883e879e8
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b2ae23ac9cc1723936c4b4e1900b95cdcde744df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>A be- és kikapcsolása a LED viselkedését módosítása
## <a name="what-you-will-do"></a>Mit fog
A LED be- és kikapcsolását viselkedésének módosítása az üzenetek testreszabhatók. Ha bármilyen problémába ütközik, a keresés megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
További Node.js funkciók segítségével módosíthatja a LED be- és kikapcsolását viselkedését.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta [futtassa a mintaalkalmazást a felhő-eszközre küldött üzenetek fogadására málna Pi](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="add-nodejs-functions"></a>Node.js funkciók hozzáadása
1. Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:
   
   ```bash
   cd Lesson4
   code .
   ```
2. Nyissa meg a `app.js` fájlt, és adja hozzá a következő funkciók végén:
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![a hozzáadott funkciók App.js fájlban](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)
3. Adja meg az alábbi feltételeket az alapértelmezett a kapcsoló blokk, mielőtt a `receiveMessageCallback` függvény:
   
   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
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
   
   ![A hozzáadott funkcióval Gulpfile.js fájl](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)
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

![Mintaalkalmazást a be- és kikapcsolását üzenetek](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Gratulálunk! Sikeresen testre szabta az IoT hub-a-pi tartományban küldött állapotüzenetek.

### <a name="summary"></a>Összefoglalás
Ez nem kötelező a szakasz bemutatja, hogyan kell az üzenetek testreszabhatók, hogy a mintaalkalmazás képes kezelni a be és ki a LED viselkedését eltérő módon.


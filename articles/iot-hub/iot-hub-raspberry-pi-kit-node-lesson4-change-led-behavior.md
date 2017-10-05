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
# <a name="change-the-on-and-off-behavior-of-the-led"></a><span data-ttu-id="6b2dd-104">A be- és kikapcsolása a LED viselkedését módosítása</span><span class="sxs-lookup"><span data-stu-id="6b2dd-104">Change the on and off behavior of the LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="6b2dd-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="6b2dd-105">What you will do</span></span>
<span data-ttu-id="6b2dd-106">A LED be- és kikapcsolását viselkedésének módosítása az üzenetek testreszabhatók.</span><span class="sxs-lookup"><span data-stu-id="6b2dd-106">Customize the messages to change the LED’s on and off behavior.</span></span> <span data-ttu-id="6b2dd-107">Ha bármilyen problémába ütközik, a keresés megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="6b2dd-107">If you have any problems, seek solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6b2dd-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="6b2dd-108">What you will learn</span></span>
<span data-ttu-id="6b2dd-109">További Node.js funkciók segítségével módosíthatja a LED be- és kikapcsolását viselkedését.</span><span class="sxs-lookup"><span data-stu-id="6b2dd-109">Use additional Node.js functions to change the LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6b2dd-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="6b2dd-110">What you need</span></span>
<span data-ttu-id="6b2dd-111">Sikeresen végrehajtotta [futtassa a mintaalkalmazást a felhő-eszközre küldött üzenetek fogadására málna Pi](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).</span><span class="sxs-lookup"><span data-stu-id="6b2dd-111">You must have successfully completed [Run a sample application on Raspberry Pi to receive cloud-to-device messages](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).</span></span>

## <a name="add-nodejs-functions"></a><span data-ttu-id="6b2dd-112">Node.js funkciók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6b2dd-112">Add Node.js functions</span></span>
1. <span data-ttu-id="6b2dd-113">Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6b2dd-113">Open the sample application in Visual Studio code by running the following commands:</span></span>
   
   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="6b2dd-114">Nyissa meg a `app.js` fájlt, és adja hozzá a következő funkciók végén:</span><span class="sxs-lookup"><span data-stu-id="6b2dd-114">Open the `app.js` file, and then add the following functions at the end:</span></span>
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![a hozzáadott funkciók App.js fájlban](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)
3. <span data-ttu-id="6b2dd-116">Adja meg az alábbi feltételeket az alapértelmezett a kapcsoló blokk, mielőtt a `receiveMessageCallback` függvény:</span><span class="sxs-lookup"><span data-stu-id="6b2dd-116">Add the following conditions before the default one in the switch-case block of the `receiveMessageCallback` function:</span></span>
   
   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```
   
   <span data-ttu-id="6b2dd-117">Most már konfigurálta az válaszolni üzenetekben további információkat a mintaalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="6b2dd-117">Now you’ve configured the sample application to respond to more instructions through messages.</span></span> <span data-ttu-id="6b2dd-118">Az "on" utasítás bekapcsolja a LED-jét, és az "off" utasítás kikapcsolja a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="6b2dd-118">The "on" instruction turns on the LED, and the "off" instruction turns off the LED.</span></span>
4. <span data-ttu-id="6b2dd-119">Nyissa meg a gulpfile.js fájlt, és adja hozzá az új függvény előtt a függvény `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="6b2dd-119">Open the gulpfile.js file, and then add a new function before the function `sendMessage`:</span></span>
   
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
5. <span data-ttu-id="6b2dd-121">Az a `sendMessage` működik, cserélje le a sor `var message = buildMessage(sentMessageCount);` a új sorral a következő kódrészletben látható:</span><span class="sxs-lookup"><span data-stu-id="6b2dd-121">In the `sendMessage` function, replace the line `var message = buildMessage(sentMessageCount);` with the new line shown in the following snippet:</span></span>
   
   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="6b2dd-122">A módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="6b2dd-122">Save all the changes.</span></span>

### <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="6b2dd-123">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="6b2dd-123">Deploy and run the sample application</span></span>
<span data-ttu-id="6b2dd-124">Telepíthet, és futtassa a mintaalkalmazást a Pi a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6b2dd-124">Deploy and run the sample application on Pi by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="6b2dd-125">A két másodpercen bekapcsolása LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="6b2dd-125">You should see the LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="6b2dd-126">Az utolsó "stop" üzenet leállítja a mintaalkalmazás futtatását.</span><span class="sxs-lookup"><span data-stu-id="6b2dd-126">The last "stop" message stops the sample application from running.</span></span>

![Mintaalkalmazást a be- és kikapcsolását üzenetek](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

<span data-ttu-id="6b2dd-128">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="6b2dd-128">Congratulations!</span></span> <span data-ttu-id="6b2dd-129">Sikeresen testre szabta az IoT hub-a-pi tartományban küldött állapotüzenetek.</span><span class="sxs-lookup"><span data-stu-id="6b2dd-129">You’ve successfully customized the messages that are sent to Pi from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="6b2dd-130">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="6b2dd-130">Summary</span></span>
<span data-ttu-id="6b2dd-131">Ez nem kötelező a szakasz bemutatja, hogyan kell az üzenetek testreszabhatók, hogy a mintaalkalmazás képes kezelni a be és ki a LED viselkedését eltérő módon.</span><span class="sxs-lookup"><span data-stu-id="6b2dd-131">This optional section demonstrates how to customize messages so that the sample application can control the on and off behavior of the LED in a different way.</span></span>


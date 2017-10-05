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
# <a name="change-the-on-and-off-behavior-of-the-led"></a><span data-ttu-id="80271-104">A be- és kikapcsolása a LED viselkedését módosítása</span><span class="sxs-lookup"><span data-stu-id="80271-104">Change the on and off behavior of the LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="80271-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="80271-105">What you will do</span></span>
<span data-ttu-id="80271-106">A LED be- és kikapcsolását viselkedésének módosítása az üzenetek testreszabhatók.</span><span class="sxs-lookup"><span data-stu-id="80271-106">Customize the messages to change the LED’s on and off behavior.</span></span> <span data-ttu-id="80271-107">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="80271-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="80271-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="80271-108">What you will learn</span></span>
<span data-ttu-id="80271-109">További funkciók segítségével módosíthatja a LED be- és kikapcsolását viselkedését.</span><span class="sxs-lookup"><span data-stu-id="80271-109">Use additional functions to change the LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="80271-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="80271-110">What you need</span></span>
<span data-ttu-id="80271-111">Sikeresen végrehajtotta [futtassa a mintaalkalmazást a felhőből az eszközre küldött üzenetek fogadására Intel Edison][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="80271-111">You must have successfully completed [Run a sample application on Intel Edison to receive cloud to device messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-to-mainc-and-gulpfilejs"></a><span data-ttu-id="80271-112">Funkciók hozzáadása main.c és gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="80271-112">Add functions to main.c and gulpfile.js</span></span>
1. <span data-ttu-id="80271-113">Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="80271-113">Open the sample application in Visual Studio code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="80271-114">Nyissa meg a `main.c` fájlt, és adja hozzá a következő funkciók blinkLED() függvény után:</span><span class="sxs-lookup"><span data-stu-id="80271-114">Open the `main.c` file, and then add the following functions after blinkLED() function:</span></span>

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

3. <span data-ttu-id="80271-116">Adja meg az alábbi feltételeket előtt a `else if` a letiltása a `receiveMessageCallback` függvény:</span><span class="sxs-lookup"><span data-stu-id="80271-116">Add the following conditions before the `else if` block of the `receiveMessageCallback` function:</span></span>

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

   <span data-ttu-id="80271-117">Most már konfigurálta az válaszolni üzenetekben további információkat a mintaalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="80271-117">Now you’ve configured the sample application to respond to more instructions through messages.</span></span> <span data-ttu-id="80271-118">Az "on" utasítás bekapcsolja a LED-jét, és az "off" utasítás kikapcsolja a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="80271-118">The "on" instruction turns on the LED, and the "off" instruction turns off the LED.</span></span>
4. <span data-ttu-id="80271-119">Nyissa meg a gulpfile.js fájlt, és adja hozzá az új függvény előtt a függvény `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="80271-119">Open the gulpfile.js file, and then add a new function before the function `sendMessage`:</span></span>

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
5. <span data-ttu-id="80271-121">Az a `sendMessage` működik, cserélje le a sor `var message = buildMessage(sentMessageCount);` a új sorral a következő kódrészletben látható:</span><span class="sxs-lookup"><span data-stu-id="80271-121">In the `sendMessage` function, replace the line `var message = buildMessage(sentMessageCount);` with the new line shown in the following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="80271-122">A módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="80271-122">Save all the changes.</span></span>

### <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="80271-123">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="80271-123">Deploy and run the sample application</span></span>
<span data-ttu-id="80271-124">Telepíthet, és futtassa a mintaalkalmazást a Edison a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="80271-124">Deploy and run the sample application on Edison by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="80271-125">A két másodpercen bekapcsolása LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="80271-125">You should see the LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="80271-126">Az utolsó "stop" üzenet leállítja a mintaalkalmazás futtatását.</span><span class="sxs-lookup"><span data-stu-id="80271-126">The last "stop" message stops the sample application from running.</span></span>

![be- és kikapcsolása][on-and-off]

<span data-ttu-id="80271-128">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="80271-128">Congratulations!</span></span> <span data-ttu-id="80271-129">Az IoT hub a Edison küldött üzenetek sikeresen szabta.</span><span class="sxs-lookup"><span data-stu-id="80271-129">You’ve successfully customized the messages that are sent to Edison from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="80271-130">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="80271-130">Summary</span></span>
<span data-ttu-id="80271-131">Ez nem kötelező a szakasz bemutatja, hogyan kell az üzenetek testreszabhatók, hogy a mintaalkalmazás képes kezelni a be és ki a LED viselkedését eltérő módon.</span><span class="sxs-lookup"><span data-stu-id="80271-131">This optional section demonstrates how to customize messages so that the sample application can control the on and off behavior of the LED in a different way.</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_c.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_c.png

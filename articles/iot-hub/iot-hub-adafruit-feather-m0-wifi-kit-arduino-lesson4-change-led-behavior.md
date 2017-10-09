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
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="c1b88-104">Hello be- és kikapcsolását hello LED viselkedésének módosítása</span><span class="sxs-lookup"><span data-stu-id="c1b88-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="c1b88-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="c1b88-105">What you will do</span></span>
<span data-ttu-id="c1b88-106">Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása.</span><span class="sxs-lookup"><span data-stu-id="c1b88-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="c1b88-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) a Adafruit lágyított M0 Wi-Fi Arduino kártya.</span><span class="sxs-lookup"><span data-stu-id="c1b88-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="c1b88-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="c1b88-108">What you will learn</span></span>
<span data-ttu-id="c1b88-109">További Arduino funkciók toochange LED tartozó be- és kikapcsolását viselkedés hello használata.</span><span class="sxs-lookup"><span data-stu-id="c1b88-109">Use additional Arduino functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c1b88-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="c1b88-110">What you need</span></span>
<span data-ttu-id="c1b88-111">Sikeresen végrehajtotta [toodevice üzenetek futtassa a mintaalkalmazást a Arduino board tooreceive felhő][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="c1b88-111">You must have successfully completed [Run a sample application on your Arduino board tooreceive cloud toodevice messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-toomainc-and-gulpfilejs"></a><span data-ttu-id="c1b88-112">Funkciók toomain.c és gulpfile.js hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c1b88-112">Add functions toomain.c and gulpfile.js</span></span>
1. <span data-ttu-id="c1b88-113">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c1b88-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="c1b88-114">Nyissa meg hello `app.ino` fájlt, és adja hozzá a következő funkciók blinkLED() függvény után hello:</span><span class="sxs-lookup"><span data-stu-id="c1b88-114">Open hello `app.ino` file, and then add hello following functions after blinkLED() function:</span></span>

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
3. <span data-ttu-id="c1b88-116">Adja hozzá a következő feltételek előtt hello hello `else if` hello adatblokk `receiveMessageCallback` függvény:</span><span class="sxs-lookup"><span data-stu-id="c1b88-116">Add hello following conditions before hello `else if` block of hello `receiveMessageCallback` function:</span></span>

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

   <span data-ttu-id="c1b88-117">Most már konfigurálta az hello alkalmazás toorespond toomore vonatkozó példautasításokat üzenetekben.</span><span class="sxs-lookup"><span data-stu-id="c1b88-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="c1b88-118">hello "on" utasítás bekapcsolja a hello LED-jét, és hello "off" utasítás kikapcsolása hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="c1b88-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="c1b88-119">Nyissa meg a hello gulpfile.js fájlt, és adja hozzá a hello függvény előtt egy új funkció `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="c1b88-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>

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
5. <span data-ttu-id="c1b88-121">A hello `sendMessage` működik, cserélje le a hello sor `var message = buildMessage(sentMessageCount);` hello új sorral hello a következő kódrészletben látható:</span><span class="sxs-lookup"><span data-stu-id="c1b88-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="c1b88-122">Minden hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="c1b88-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="c1b88-123">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="c1b88-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="c1b88-124">Telepíthet, és futtassa a hello mintaalkalmazást a Arduino táblán hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c1b88-124">Deploy and run hello sample application on your Arduino board by running hello following command:</span></span>

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="c1b88-125">Két másodpercen bekapcsolása hello LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="c1b88-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="c1b88-126">utolsó "stop" üdvözlőüzenetére hello mintaalkalmazás futtatását leáll.</span><span class="sxs-lookup"><span data-stu-id="c1b88-126">hello last "stop" message stops hello sample application from running.</span></span>

![be- és kikapcsolása][on-and-off]

<span data-ttu-id="c1b88-128">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="c1b88-128">Congratulations!</span></span> <span data-ttu-id="c1b88-129">Az IoT hub tooyour Arduino board küldi hello üzenetek sikeresen szabta.</span><span class="sxs-lookup"><span data-stu-id="c1b88-129">You’ve successfully customized hello messages that are sent tooyour Arduino board from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="c1b88-130">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="c1b88-130">Summary</span></span>
<span data-ttu-id="c1b88-131">A választható szakasz azt mutatja be, hogyan toocustomize üzenetek, hogy hello mintaalkalmazás eltérő módon szabályozhatja hello be- és kikapcsolását hello LED viselkedését.</span><span class="sxs-lookup"><span data-stu-id="c1b88-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png
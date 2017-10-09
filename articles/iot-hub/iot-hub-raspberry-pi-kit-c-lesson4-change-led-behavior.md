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
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="a177d-104">Hello be- és kikapcsolását hello LED viselkedésének módosítása</span><span class="sxs-lookup"><span data-stu-id="a177d-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="a177d-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a177d-105">What you will do</span></span>
<span data-ttu-id="a177d-106">Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása.</span><span class="sxs-lookup"><span data-stu-id="a177d-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="a177d-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a177d-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a177d-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a177d-108">What you will learn</span></span>
<span data-ttu-id="a177d-109">További Node.js funkciók toochange LED tartozó be- és kikapcsolását viselkedés hello használata.</span><span class="sxs-lookup"><span data-stu-id="a177d-109">Use additional Node.js functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a177d-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a177d-110">What you need</span></span>
<span data-ttu-id="a177d-111">Sikeresen végrehajtotta [toodevice üzenetek futtassa a mintaalkalmazást málna Pi tooreceive felhő](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).</span><span class="sxs-lookup"><span data-stu-id="a177d-111">You must have successfully completed [Run a sample application on Raspberry Pi tooreceive cloud toodevice messages](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).</span></span>

## <a name="add-functions-toomainc-and-gulpfilejs"></a><span data-ttu-id="a177d-112">Funkciók toomain.c és gulpfile.js hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a177d-112">Add functions toomain.c and gulpfile.js</span></span>
1. <span data-ttu-id="a177d-113">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a177d-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="a177d-114">Nyissa meg hello `main.c` fájlt, és adja hozzá a következő funkciók blinkLED() függvény után hello:</span><span class="sxs-lookup"><span data-stu-id="a177d-114">Open hello `main.c` file, and then add hello following functions after blinkLED() function:</span></span>

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
3. <span data-ttu-id="a177d-116">Adja hozzá a hello alapértelmezett kikötéseket követő hello hello `if` hello adatblokk `receiveMessageCallback` függvény:</span><span class="sxs-lookup"><span data-stu-id="a177d-116">Add hello following conditions before hello default one in hello `if` block of hello `receiveMessageCallback` function:</span></span>

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

   <span data-ttu-id="a177d-117">Most már konfigurálta az hello alkalmazás toorespond toomore vonatkozó példautasításokat üzenetekben.</span><span class="sxs-lookup"><span data-stu-id="a177d-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="a177d-118">hello "on" utasítás bekapcsolja a hello LED-jét, és hello "off" utasítás kikapcsolása hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="a177d-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="a177d-119">Nyissa meg a hello gulpfile.js fájlt, és adja hozzá a hello függvény előtt egy új funkció `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="a177d-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>

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
5. <span data-ttu-id="a177d-121">A hello `sendMessage` működik, cserélje le a hello sor `var message = buildMessage(sentMessageCount);` hello új sorral hello a következő kódrészletben látható:</span><span class="sxs-lookup"><span data-stu-id="a177d-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="a177d-122">Minden hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="a177d-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="a177d-123">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="a177d-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="a177d-124">Központi telepítése, és futtassa a mintaalkalmazást hello Pi hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a177d-124">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="a177d-125">Két másodpercen bekapcsolása hello LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="a177d-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="a177d-126">utolsó "stop" üdvözlőüzenetére hello mintaalkalmazás futtatását leáll.</span><span class="sxs-lookup"><span data-stu-id="a177d-126">hello last "stop" message stops hello sample application from running.</span></span>

![Mintaalkalmazást a be- és kikapcsolását üzenetek](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off_c.png)

<span data-ttu-id="a177d-128">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="a177d-128">Congratulations!</span></span> <span data-ttu-id="a177d-129">Sikeresen testre szabta az IoT hub tooPi küldi hello üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="a177d-129">You’ve successfully customized hello messages that are sent tooPi from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="a177d-130">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a177d-130">Summary</span></span>
<span data-ttu-id="a177d-131">A választható szakasz azt mutatja be, hogyan toocustomize üzenetek, hogy hello mintaalkalmazás eltérő módon szabályozhatja hello be- és kikapcsolását hello LED viselkedését.</span><span class="sxs-lookup"><span data-stu-id="a177d-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>

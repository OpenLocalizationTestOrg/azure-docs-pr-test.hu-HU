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
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="87c1f-104">Hello be- és kikapcsolását hello LED viselkedésének módosítása</span><span class="sxs-lookup"><span data-stu-id="87c1f-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="87c1f-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="87c1f-105">What you will do</span></span>
<span data-ttu-id="87c1f-106">Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása.</span><span class="sxs-lookup"><span data-stu-id="87c1f-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="87c1f-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="87c1f-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="87c1f-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="87c1f-108">What you will learn</span></span>
<span data-ttu-id="87c1f-109">További funkciók toochange hello LED tartozó be- és kikapcsolását viselkedés használja.</span><span class="sxs-lookup"><span data-stu-id="87c1f-109">Use additional functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="87c1f-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="87c1f-110">What you need</span></span>
<span data-ttu-id="87c1f-111">Sikeresen végrehajtotta [toodevice üzenetek futtassa a mintaalkalmazást Intel Edison tooreceive felhő][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="87c1f-111">You must have successfully completed [Run a sample application on Intel Edison tooreceive cloud toodevice messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-tooappjs-and-gulpfilejs"></a><span data-ttu-id="87c1f-112">Funkciók tooapp.js és gulpfile.js hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87c1f-112">Add functions tooapp.js and gulpfile.js</span></span>
1. <span data-ttu-id="87c1f-113">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="87c1f-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="87c1f-114">Nyissa meg hello `app.js` fájlt, és adja hozzá a következő funkciók blinkLED() függvény után hello:</span><span class="sxs-lookup"><span data-stu-id="87c1f-114">Open hello `app.js` file, and then add hello following functions after blinkLED() function:</span></span>

   ```javascript
   function turnOnLED() {
     myLed.write(1);
   }

   function turnOffLED() {
     myLed.write(0);
   }
   ```

   ![a hozzáadott funkciók App.js fájlban](media/iot-hub-intel-edison-lessons/lesson4/updated_app_node.png)
3. <span data-ttu-id="87c1f-116">Adja hozzá a hello hello előtt az alábbi feltételek "villogni" hello kapcsoló blokkméretet hello nagybetűkkel `receiveMessageCallback` függvény:</span><span class="sxs-lookup"><span data-stu-id="87c1f-116">Add hello following conditions before hello 'blink' case in hello switch-case block of hello `receiveMessageCallback` function:</span></span>

   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```

   <span data-ttu-id="87c1f-117">Most már konfigurálta az hello alkalmazás toorespond toomore vonatkozó példautasításokat üzenetekben.</span><span class="sxs-lookup"><span data-stu-id="87c1f-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="87c1f-118">hello "on" utasítás bekapcsolja a hello LED-jét, és hello "off" utasítás kikapcsolása hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="87c1f-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="87c1f-119">Nyissa meg a hello gulpfile.js fájlt, és adja hozzá a hello függvény előtt egy új funkció `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="87c1f-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>

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
5. <span data-ttu-id="87c1f-121">A hello `sendMessage` működik, cserélje le a hello sor `var message = buildMessage(sentMessageCount);` hello új sorral hello a következő kódrészletben látható:</span><span class="sxs-lookup"><span data-stu-id="87c1f-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="87c1f-122">Minden hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="87c1f-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="87c1f-123">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="87c1f-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="87c1f-124">Központi telepítése, és futtassa a mintaalkalmazást hello Edison hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="87c1f-124">Deploy and run hello sample application on Edison by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="87c1f-125">Két másodpercen bekapcsolása hello LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="87c1f-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="87c1f-126">utolsó "stop" üdvözlőüzenetére hello mintaalkalmazás futtatását leáll.</span><span class="sxs-lookup"><span data-stu-id="87c1f-126">hello last "stop" message stops hello sample application from running.</span></span>

![be- és kikapcsolása][on-and-off]

<span data-ttu-id="87c1f-128">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="87c1f-128">Congratulations!</span></span> <span data-ttu-id="87c1f-129">Sikeresen testre szabta az IoT hub tooEdison küldi hello üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="87c1f-129">You’ve successfully customized hello messages that are sent tooEdison from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="87c1f-130">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="87c1f-130">Summary</span></span>
<span data-ttu-id="87c1f-131">A választható szakasz azt mutatja be, hogyan toocustomize üzenetek, hogy hello mintaalkalmazás eltérő módon szabályozhatja hello be- és kikapcsolását hello LED viselkedését.</span><span class="sxs-lookup"><span data-stu-id="87c1f-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-node-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_node.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_node.png

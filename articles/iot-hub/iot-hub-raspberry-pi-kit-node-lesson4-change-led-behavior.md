---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 4: alkalmazás módosítása |} Microsoft Docs"
description: "Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása."
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
ms.openlocfilehash: 99b542fcb8639add0f5a0f7a49dd8abd0e224a51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="92a51-104">Hello be- és kikapcsolását hello LED viselkedésének módosítása</span><span class="sxs-lookup"><span data-stu-id="92a51-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="92a51-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="92a51-105">What you will do</span></span>
<span data-ttu-id="92a51-106">Hello üzenetek toochange hello LED tartozó be- és kikapcsolását viselkedés testreszabása.</span><span class="sxs-lookup"><span data-stu-id="92a51-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="92a51-107">Ha bármilyen problémába ütközik, a keresési hello megoldások [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="92a51-107">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="92a51-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="92a51-108">What you will learn</span></span>
<span data-ttu-id="92a51-109">További Node.js funkciók toochange LED tartozó be- és kikapcsolását viselkedés hello használata.</span><span class="sxs-lookup"><span data-stu-id="92a51-109">Use additional Node.js functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="92a51-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="92a51-110">What you need</span></span>
<span data-ttu-id="92a51-111">Sikeresen végrehajtotta [mintaalkalmazás futtatása málna Pi tooreceive a felhő-eszközre küldött üzenetek](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).</span><span class="sxs-lookup"><span data-stu-id="92a51-111">You must have successfully completed [Run a sample application on Raspberry Pi tooreceive cloud-to-device messages](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).</span></span>

## <a name="add-nodejs-functions"></a><span data-ttu-id="92a51-112">Node.js funkciók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="92a51-112">Add Node.js functions</span></span>
1. <span data-ttu-id="92a51-113">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="92a51-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>
   
   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="92a51-114">Nyissa meg hello `app.js` fájlt, és adja hozzá a következő funkciók hello végén hello:</span><span class="sxs-lookup"><span data-stu-id="92a51-114">Open hello `app.js` file, and then add hello following functions at hello end:</span></span>
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![a hozzáadott funkciók App.js fájlban](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)
3. <span data-ttu-id="92a51-116">Adja hozzá a következő hello alapértelmezett kikötéseket hello kapcsoló ideig tartó hello hello `receiveMessageCallback` függvény:</span><span class="sxs-lookup"><span data-stu-id="92a51-116">Add hello following conditions before hello default one in hello switch-case block of hello `receiveMessageCallback` function:</span></span>
   
   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```
   
   <span data-ttu-id="92a51-117">Most már konfigurálta az hello alkalmazás toorespond toomore vonatkozó példautasításokat üzenetekben.</span><span class="sxs-lookup"><span data-stu-id="92a51-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="92a51-118">hello "on" utasítás bekapcsolja a hello LED-jét, és hello "off" utasítás kikapcsolása hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="92a51-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="92a51-119">Nyissa meg a hello gulpfile.js fájlt, és adja hozzá a hello függvény előtt egy új funkció `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="92a51-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>
   
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
5. <span data-ttu-id="92a51-121">A hello `sendMessage` működik, cserélje le a hello sor `var message = buildMessage(sentMessageCount);` hello új sorral hello a következő kódrészletben látható:</span><span class="sxs-lookup"><span data-stu-id="92a51-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>
   
   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="92a51-122">Minden hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="92a51-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="92a51-123">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="92a51-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="92a51-124">Központi telepítése, és futtassa a mintaalkalmazást hello Pi hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="92a51-124">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="92a51-125">Két másodpercen bekapcsolása hello LED-jét, és ezután kapcsolja ki a másik két másodpercen kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="92a51-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="92a51-126">utolsó "stop" üdvözlőüzenetére hello mintaalkalmazás futtatását leáll.</span><span class="sxs-lookup"><span data-stu-id="92a51-126">hello last "stop" message stops hello sample application from running.</span></span>

![Mintaalkalmazást a be- és kikapcsolását üzenetek](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

<span data-ttu-id="92a51-128">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="92a51-128">Congratulations!</span></span> <span data-ttu-id="92a51-129">Sikeresen testre szabta az IoT hub tooPi küldi hello üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="92a51-129">You’ve successfully customized hello messages that are sent tooPi from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="92a51-130">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="92a51-130">Summary</span></span>
<span data-ttu-id="92a51-131">A választható szakasz azt mutatja be, hogyan toocustomize üzenetek, hogy hello mintaalkalmazás eltérő módon szabályozhatja hello be- és kikapcsolását hello LED viselkedését.</span><span class="sxs-lookup"><span data-stu-id="92a51-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>


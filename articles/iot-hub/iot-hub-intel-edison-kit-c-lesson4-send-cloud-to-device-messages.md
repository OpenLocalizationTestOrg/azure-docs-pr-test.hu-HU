---
title: "Connect Intel Edison (C) tooAzure IoT - lecke 4: üzeneteket fogadni |} Microsoft Docs"
description: "A mintaalkalmazás Edison futtatja, és figyeli a bejövő üzenetek az IoT hub. Egy új gulp feladatot az IoT hub tooblink hello LED üzenetek tooEdison küldi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "a webes, a weben keresztül vezetett arduino vezérlő vezetett arduino vezérlő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 820d38f3-d3b8-4249-9e2b-f1b9b771e62f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f0424506ff755e0b9514684787b37584d406d320
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="1c22d-105">Futtassa egy minta alkalmazás tooreceive felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="1c22d-105">Run a sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="1c22d-106">Ebben a cikkben az Intel Edison minta alkalmazást telepít központilag.</span><span class="sxs-lookup"><span data-stu-id="1c22d-106">In this article, you deploy a sample application on Intel Edison.</span></span> <span data-ttu-id="1c22d-107">hello mintaalkalmazást az IoT hub bejövő üzeneteit figyeli.</span><span class="sxs-lookup"><span data-stu-id="1c22d-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="1c22d-108">Akkor is futtassa gulp feladat a számítógép toosend üzenetek tooEdison az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1c22d-108">You also run a gulp task on your computer toosend messages tooEdison from your IoT hub.</span></span> <span data-ttu-id="1c22d-109">Hello mintaalkalmazás hello üzeneteket fogad, ha azt villogjon-hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="1c22d-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="1c22d-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="1c22d-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="1c22d-111">Mit fog</span><span class="sxs-lookup"><span data-stu-id="1c22d-111">What you will do</span></span>
* <span data-ttu-id="1c22d-112">Csatlakozás hello minta alkalmazás tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="1c22d-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="1c22d-113">Regisztrálhat és futtathat hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1c22d-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="1c22d-114">Az IoT hub tooEdison tooblink hello LED küldhet üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="1c22d-114">Send messages from your IoT hub tooEdison tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="1c22d-115">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="1c22d-115">What you will learn</span></span>
<span data-ttu-id="1c22d-116">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="1c22d-116">In this article, you will learn:</span></span>
* <span data-ttu-id="1c22d-117">Hogyan toomonitor bejövő az IoT hub érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="1c22d-117">How toomonitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="1c22d-118">Hogyan toosend felhő eszközt az IoT hub tooEdison érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="1c22d-118">How toosend cloud-to-device messages from your IoT hub tooEdison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="1c22d-119">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="1c22d-119">What you need</span></span>
* <span data-ttu-id="1c22d-120">Intel Edison, állítsa be a használatra.</span><span class="sxs-lookup"><span data-stu-id="1c22d-120">Intel Edison, set up for use.</span></span> <span data-ttu-id="1c22d-121">Hogyan mentése Edison, tooset: toolearn [állítsa be az eszközt][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="1c22d-121">toolearn how tooset up Edison, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="1c22d-122">Az IoT-központ, amely az Azure-előfizetése jön létre.</span><span class="sxs-lookup"><span data-stu-id="1c22d-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="1c22d-123">toolearn hogyan toocreate az IoT hub, lásd: [az Azure IoT Hub létrehozása][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="1c22d-123">toolearn how toocreate your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="1c22d-124">Csatlakozás hello minta alkalmazás tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="1c22d-124">Connect hello sample application tooyour IoT hub</span></span>
1. <span data-ttu-id="1c22d-125">Győződjön meg arról, hogy Ön hello tárház mappában `iot-hub-c-edison-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="1c22d-125">Make sure that you're in hello repo folder `iot-hub-c-edison-getting-started`.</span></span> <span data-ttu-id="1c22d-126">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1c22d-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="1c22d-127">hello hello fájlban `app` almappa is hello kód toomonitor bejövő üzenetek hello IoT hubról tartalmazó hello kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="1c22d-127">hello file in hello `app` subfolder is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="1c22d-128">Hello `blinkLED` függvény villogjon hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="1c22d-128">hello `blinkLED` function blinks hello LED.</span></span>

   ![Hello mintaalkalmazást a tárház szerkezete][repo-structure]
2. <span data-ttu-id="1c22d-130">Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1c22d-130">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

   <span data-ttu-id="1c22d-131">Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] ezen a számítógépen minden hello konfigurációk örökölt, így továbbléphet hello lépés toohello feladat történő telepítésének és hello minta alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="1c22d-131">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all hello configurations are inherited, so you can skip hello step toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="1c22d-132">Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] egy másik számítógépen kell tooreplace hello helyőrzőt hello `config-edison.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="1c22d-132">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need tooreplace hello placeholders in hello `config-edison.json` file.</span></span> <span data-ttu-id="1c22d-133">Hello `config-edison.json` fájl van-e a kezdőmappa hello almappája.</span><span class="sxs-lookup"><span data-stu-id="1c22d-133">hello `config-edison.json` file is in hello subfolder of your home folder.</span></span>

   ![Hello config-edison.json fájl tartalma](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * <span data-ttu-id="1c22d-135">Cserélje le **[eszköz állomásnév vagy IP-címe]** hello eszköz IP-cím az eszköz konfigurálása során az lefelé jelölésű.</span><span class="sxs-lookup"><span data-stu-id="1c22d-135">Replace **[device hostname or IP address]** with hello device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="1c22d-136">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** hello eszköz kapcsolati karakterlánccal hello futtatásával kapott `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="1c22d-136">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="1c22d-137">Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterláncot kapott hello futtatásával hello `az iot hub show-connection-string --name {my hub name}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="1c22d-137">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name}` command.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1c22d-138">Futtatás **gulp az install-eszközök** és, ha még nem meg lecke 1.</span><span class="sxs-lookup"><span data-stu-id="1c22d-138">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="1c22d-139">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="1c22d-139">Deploy and run hello sample application</span></span>
<span data-ttu-id="1c22d-140">Központi telepítése, és futtassa a mintaalkalmazást hello Edison hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1c22d-140">Deploy and run hello sample application on Edison by running hello following commands:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="1c22d-141">hello gulp parancs hello minta alkalmazás tooEdison telepíti.</span><span class="sxs-lookup"><span data-stu-id="1c22d-141">hello gulp command deploys hello sample application tooEdison.</span></span> <span data-ttu-id="1c22d-142">Ezt követően azt futtatja hello alkalmazás Edison és a gazdagép egy külön feladat számítógép toosend 20 villogási üzenetek tooEdison az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1c22d-142">Then, it runs hello application on Edison and a separate task on your host computer toosend 20 blink messages tooEdison from your IoT hub.</span></span>

<span data-ttu-id="1c22d-143">Hello mintaalkalmazás futtatása után kezdődik toomessages az IoT-központ figyel.</span><span class="sxs-lookup"><span data-stu-id="1c22d-143">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="1c22d-144">Eközben hello gulp feladat több "villogni" üzenetet küldi az IoT hub tooEdison.</span><span class="sxs-lookup"><span data-stu-id="1c22d-144">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooEdison.</span></span> <span data-ttu-id="1c22d-145">Minden egyes villogási üzenet, amely megkapja a Edison, hello mintaalkalmazás meghívja a hello `blinkLED` függvény tooblink hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="1c22d-145">For each blink message that Edison receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="1c22d-146">Feladat küldése 20 üzenetet az IoT hub tooEdison a gulp LED hello villogási hello mint két másodpercenként kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="1c22d-146">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooEdison.</span></span> <span data-ttu-id="1c22d-147">hello utolsó egyik hello alkalmazás futtatását megakadályozó "stop" üzenet.</span><span class="sxs-lookup"><span data-stu-id="1c22d-147">hello last one is a "stop" message that stops hello application from running.</span></span>

![A mintaalkalmazás parancs gulp és üzenetek villogni][gulp-command-and-blink-messages]

## <a name="summary"></a><span data-ttu-id="1c22d-149">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="1c22d-149">Summary</span></span>
<span data-ttu-id="1c22d-150">Az IoT hub tooEdison tooblink hello LED a sikeresen elküldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="1c22d-150">You’ve successfully sent messages from your IoT hub tooEdison tooblink hello LED.</span></span> <span data-ttu-id="1c22d-151">hello következő feladat nem kötelező: hello be- és kikapcsolását hello LED viselkedésének módosítása.</span><span class="sxs-lookup"><span data-stu-id="1c22d-151">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c22d-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1c22d-152">Next steps</span></span>
<span data-ttu-id="1c22d-153">[Hello be- és kikapcsolását hello LED viselkedésének módosítása][change-the-on-and-off-behavior-of-the-led]</span><span class="sxs-lookup"><span data-stu-id="1c22d-153">[Change hello on and off behavior of hello LED][change-the-on-and-off-behavior-of-the-led]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure_c.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink_c.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-c-lesson4-change-led-behavior.md
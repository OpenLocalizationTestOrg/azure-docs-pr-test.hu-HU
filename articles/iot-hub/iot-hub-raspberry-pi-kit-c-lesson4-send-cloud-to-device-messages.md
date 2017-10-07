---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 4: Felhő eszközre |} Microsoft Docs"
description: "A mintaalkalmazás Pi futtatja, és figyeli a bejövő üzenetek az IoT hub. Egy új gulp feladatot az IoT hub tooblink hello LED üzenetek tooPi küldi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "felhő toodevice, üzenet a felhőből"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: fcbc0dd0-cae3-47b0-8e58-240e4f406f75
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5596bf3a83c21f2bd54b2f83e2a8fdad7a608b94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="15013-105">Futtassa egy minta alkalmazás tooreceive felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="15013-105">Run a sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="15013-106">Ebben a cikkben a málna Pi 3 minta alkalmazást telepít központilag.</span><span class="sxs-lookup"><span data-stu-id="15013-106">In this article, you deploy a sample application on Raspberry Pi 3.</span></span> <span data-ttu-id="15013-107">hello mintaalkalmazást az IoT hub bejövő üzeneteit figyeli.</span><span class="sxs-lookup"><span data-stu-id="15013-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="15013-108">Akkor is futtassa gulp feladat a számítógép toosend üzenetek tooPi az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="15013-108">You also run a gulp task on your computer toosend messages tooPi from your IoT hub.</span></span> <span data-ttu-id="15013-109">Hello mintaalkalmazás hello üzeneteket fogad, ha azt villogjon-hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="15013-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="15013-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="15013-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="15013-111">Mit fog</span><span class="sxs-lookup"><span data-stu-id="15013-111">What you will do</span></span>
* <span data-ttu-id="15013-112">Csatlakozás hello minta alkalmazás tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="15013-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="15013-113">Regisztrálhat és futtathat hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="15013-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="15013-114">Az IoT hub tooPi tooblink hello LED küldhet üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="15013-114">Send messages from your IoT hub tooPi tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="15013-115">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="15013-115">What you will learn</span></span>
<span data-ttu-id="15013-116">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="15013-116">In this article, you will learn:</span></span>
* <span data-ttu-id="15013-117">Hogyan toomonitor bejövő az IoT hub érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="15013-117">How toomonitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="15013-118">Hogyan toosend felhő eszközt az IoT hub tooPi érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="15013-118">How toosend cloud-to-device messages from your IoT hub tooPi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="15013-119">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="15013-119">What you need</span></span>
* <span data-ttu-id="15013-120">Raspberry Pi 3, állítsa be a használatra.</span><span class="sxs-lookup"><span data-stu-id="15013-120">Raspberry Pi 3, set up for use.</span></span> <span data-ttu-id="15013-121">Hogyan tooset fel a Pi: toolearn [állítsa be az eszközt](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).</span><span class="sxs-lookup"><span data-stu-id="15013-121">toolearn how tooset up Pi, see [Configure your device](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).</span></span>
* <span data-ttu-id="15013-122">Az IoT-központ, amely az Azure-előfizetése jön létre.</span><span class="sxs-lookup"><span data-stu-id="15013-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="15013-123">toolearn hogyan toocreate az IoT hub, lásd: [az IoT hub létrehozni és regisztrálni az málna Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="15013-123">toolearn how toocreate your IoT hub, see [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="15013-124">Csatlakozás hello minta alkalmazás tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="15013-124">Connect hello sample application tooyour IoT hub</span></span>
1. <span data-ttu-id="15013-125">Győződjön meg arról, hogy Ön hello tárház mappában `iot-hub-c-raspberrypi-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="15013-125">Make sure that you're in hello repo folder `iot-hub-c-raspberrypi-getting-started`.</span></span> <span data-ttu-id="15013-126">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="15013-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="15013-127">Értesítés hello `app.c` hello fájlban `app` almappájában.</span><span class="sxs-lookup"><span data-stu-id="15013-127">Notice hello `app.c` file in hello `app` subfolder.</span></span> <span data-ttu-id="15013-128">Hello `app.c` hello kód toomonitor bejövő üzenetek hello IoT hubról tartalmazó hello forrás-fájl.</span><span class="sxs-lookup"><span data-stu-id="15013-128">hello `app.c` file is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="15013-129">Hello `blinkLED` függvény villogjon hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="15013-129">hello `blinkLED` function blinks hello LED.</span></span>

   ![Hello mintaalkalmazást a tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure_c.png)
2. <span data-ttu-id="15013-131">Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="15013-131">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

   <span data-ttu-id="15013-132">Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) ezen a számítógépen minden hello konfigurációk örökölt, így továbbléphet toostep toohello feladatütemezés központi telepítését és futó mintaalkalmazást hello.</span><span class="sxs-lookup"><span data-stu-id="15013-132">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) on this computer, all hello configurations are inherited, so you can skip toostep toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="15013-133">Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) egy másik számítógépen kell tooreplace hello helyőrzőt hello `config-raspberrypi.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="15013-133">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) on a different computer, you need tooreplace hello placeholders in hello `config-raspberrypi.json` file.</span></span> <span data-ttu-id="15013-134">Hello `config-raspberrypi.json` fájl van-e a kezdőmappa hello almappája.</span><span class="sxs-lookup"><span data-stu-id="15013-134">hello `config-raspberrypi.json` file is in hello subfolder of your home folder.</span></span>

   ![Hello config-raspberrypi.json fájl tartalma](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* <span data-ttu-id="15013-136">Cserélje le **[eszköz állomásnév vagy IP-címe]** a Pi tartozó IP-cím vagy név hello futtatásával kapott `devdisco list --eth` parancsot.</span><span class="sxs-lookup"><span data-stu-id="15013-136">Replace **[device hostname or IP address]** with Pi’s IP address or host name that you get by running hello `devdisco list --eth` command.</span></span>
* <span data-ttu-id="15013-137">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** hello eszköz kapcsolati karakterlánccal hello futtatásával kapott `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="15013-137">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` command.</span></span>
* <span data-ttu-id="15013-138">Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterláncot kapott hello futtatásával hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="15013-138">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` command.</span></span>

> [!NOTE]
> <span data-ttu-id="15013-139">Futtatás **gulp az install-eszközök** és, ha még nem meg lecke 1.</span><span class="sxs-lookup"><span data-stu-id="15013-139">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="15013-140">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="15013-140">Deploy and run hello sample application</span></span>
<span data-ttu-id="15013-141">Központi telepítése, és futtassa a mintaalkalmazást hello Pi hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="15013-141">Deploy and run hello sample application on Pi by running hello following commands:</span></span>

```
gulp deploy && gulp run
```

<span data-ttu-id="15013-142">hello gulp parancs futtatása először hello install-eszközök feladat.</span><span class="sxs-lookup"><span data-stu-id="15013-142">hello gulp command runs hello install-tools task first.</span></span> <span data-ttu-id="15013-143">Majd hello minta alkalmazás tooPi telepíti.</span><span class="sxs-lookup"><span data-stu-id="15013-143">Then it deploys hello sample application tooPi.</span></span> <span data-ttu-id="15013-144">Végül azt futtatja hello alkalmazás Pi és a gazdagép egy külön feladat számítógép toosend 20 villogási üzenetek tooPi az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="15013-144">Finally, it runs hello application on Pi and a separate task on your host computer toosend 20 blink messages tooPi from your IoT hub.</span></span>

<span data-ttu-id="15013-145">Hello mintaalkalmazás futtatása után kezdődik toomessages az IoT-központ figyel.</span><span class="sxs-lookup"><span data-stu-id="15013-145">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="15013-146">Eközben hello gulp feladat több "villogni" üzenetet küldi az IoT hub tooPi.</span><span class="sxs-lookup"><span data-stu-id="15013-146">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooPi.</span></span> <span data-ttu-id="15013-147">Minden egyes villogási üzenet, amely megkapja a Pi, hello mintaalkalmazás meghívja a hello `blinkLED` függvény tooblink hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="15013-147">For each blink message that Pi receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="15013-148">Feladat küldése 20 üzenetet az IoT hub tooPi a gulp LED hello villogási hello mint két másodpercenként kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="15013-148">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooPi.</span></span> <span data-ttu-id="15013-149">hello utolsó egyik hello alkalmazás futtatását megakadályozó "stop" üzenet.</span><span class="sxs-lookup"><span data-stu-id="15013-149">hello last one is a "stop" message that stops hello application from running.</span></span>

![A mintaalkalmazás parancs gulp és üzenetek villogni](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink_c.png)

## <a name="summary"></a><span data-ttu-id="15013-151">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="15013-151">Summary</span></span>
<span data-ttu-id="15013-152">Az IoT hub tooPi tooblink hello LED a sikeresen elküldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="15013-152">You’ve successfully sent messages from your IoT hub tooPi tooblink hello LED.</span></span> <span data-ttu-id="15013-153">hello következő feladat nem kötelező: hello be- és kikapcsolását hello LED viselkedésének módosítása.</span><span class="sxs-lookup"><span data-stu-id="15013-153">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15013-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15013-154">Next steps</span></span>
[<span data-ttu-id="15013-155">Hello be- és kikapcsolását hello LED viselkedésének módosítása</span><span class="sxs-lookup"><span data-stu-id="15013-155">Change hello on and off behavior of hello LED</span></span>](iot-hub-raspberry-pi-kit-c-lesson4-change-led-behavior.md)

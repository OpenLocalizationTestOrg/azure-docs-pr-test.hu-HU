---
featureFlags: usabilla
title: "Csatlakozás az Azure IoT - 4. lecke Raspberry Pi (csomópont): Cloud eszközre |} Microsoft Docs"
description: "A mintaalkalmazás Pi futtatja, és figyeli a bejövő üzenetek az IoT hub. Új gulp feladat üzeneteket küld a az IoT-központ a LED villogni a Pi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "a felhő eszközhöz, message a felhőből"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6ae6539e-1163-4490-8d72-fdf7803e3054
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b8e9e51391f9b6737762b3404658297ab4c82783
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a><span data-ttu-id="f0af8-105">Futtassa a mintaalkalmazást a felhő-eszközre küldött üzenetek fogadására</span><span class="sxs-lookup"><span data-stu-id="f0af8-105">Run the sample application to receive cloud-to-device messages</span></span>
<span data-ttu-id="f0af8-106">Ebben a cikkben a málna Pi 3 minta alkalmazást telepít központilag.</span><span class="sxs-lookup"><span data-stu-id="f0af8-106">In this article, you deploy a sample application on Raspberry Pi 3.</span></span> <span data-ttu-id="f0af8-107">A mintaalkalmazás az IoT hub bejövő üzeneteit figyeli.</span><span class="sxs-lookup"><span data-stu-id="f0af8-107">The sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="f0af8-108">Gulp feladatot az IoT hub Pi küldéséhez a számítógépen is futtathat.</span><span class="sxs-lookup"><span data-stu-id="f0af8-108">You also run a gulp task on your computer to send messages to Pi from your IoT hub.</span></span> <span data-ttu-id="f0af8-109">A mintaalkalmazás az üzeneteket fogad, ha azt a LED villogjon.</span><span class="sxs-lookup"><span data-stu-id="f0af8-109">When the sample application receives the messages, it blinks the LED.</span></span> <span data-ttu-id="f0af8-110">Ha bármilyen problémába ütközik, a keresés megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f0af8-110">If you have any problems, seek solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="f0af8-111">Mit fog</span><span class="sxs-lookup"><span data-stu-id="f0af8-111">What you will do</span></span>
* <span data-ttu-id="f0af8-112">A mintaalkalmazás az IoT hub való csatlakozás.</span><span class="sxs-lookup"><span data-stu-id="f0af8-112">Connect the sample application to your IoT hub.</span></span>
* <span data-ttu-id="f0af8-113">Központi telepítése, és futtassa a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f0af8-113">Deploy and run the sample application.</span></span>
* <span data-ttu-id="f0af8-114">Üzenetek küldése az IoT hub számítógépről Pi villogni fog a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="f0af8-114">Send messages from your IoT hub to Pi to blink the LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f0af8-115">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="f0af8-115">What you will learn</span></span>
<span data-ttu-id="f0af8-116">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="f0af8-116">In this article, you will learn:</span></span>
* <span data-ttu-id="f0af8-117">Az IoT hub bejövő üzenetek figyelése</span><span class="sxs-lookup"><span data-stu-id="f0af8-117">How to monitor incoming messages from your IoT hub</span></span>
* <span data-ttu-id="f0af8-118">Hogyan küldhetők felhő-eszközre küldött üzenetek az IoT hub-Pi tartományban.</span><span class="sxs-lookup"><span data-stu-id="f0af8-118">How to send cloud-to-device messages from your IoT hub to Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f0af8-119">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="f0af8-119">What you need</span></span>
* <span data-ttu-id="f0af8-120">Raspberry Pi 3, állítsa be a használatra.</span><span class="sxs-lookup"><span data-stu-id="f0af8-120">Raspberry Pi 3, set up for use.</span></span> <span data-ttu-id="f0af8-121">A Pi beállításával kapcsolatban a [állítsa be az eszközt](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).</span><span class="sxs-lookup"><span data-stu-id="f0af8-121">To learn how to set up Pi, see [Configure your device](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).</span></span>
* <span data-ttu-id="f0af8-122">Az IoT-központ, amely az Azure-előfizetése jön létre.</span><span class="sxs-lookup"><span data-stu-id="f0af8-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="f0af8-123">Az IoT hub létrehozásával kapcsolatban lásd: [az IoT hub létrehozni és regisztrálni az málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="f0af8-123">To learn how to create your IoT hub, see [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span></span>

## <a name="connect-the-sample-application-to-your-iot-hub"></a><span data-ttu-id="f0af8-124">A mintaalkalmazás az IoT hub való csatlakozáshoz</span><span class="sxs-lookup"><span data-stu-id="f0af8-124">Connect the sample application to your IoT hub</span></span>
1. <span data-ttu-id="f0af8-125">Győződjön meg arról, hogy Ön a tárházban mappában `iot-hub-node-raspberrypi-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="f0af8-125">Make sure that you're in the repo folder `iot-hub-node-raspberrypi-getting-started`.</span></span> <span data-ttu-id="f0af8-126">Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f0af8-126">Open the sample application in Visual Studio Code by running the following commands:</span></span>
   
   ```bash
   cd Lesson4
   code .
   ```
   
   <span data-ttu-id="f0af8-127">Figyelje meg a `app.js` fájlt a `app` almappájában.</span><span class="sxs-lookup"><span data-stu-id="f0af8-127">Notice the `app.js` file in the `app` subfolder.</span></span> <span data-ttu-id="f0af8-128">A `app.js` a forrás-fájl, amely tartalmazza a kódot a figyelheti a bejövő üzenetek az IoT hubból.</span><span class="sxs-lookup"><span data-stu-id="f0af8-128">The `app.js` file is the key source file that contains the code to monitor incoming messages from the IoT hub.</span></span> <span data-ttu-id="f0af8-129">A `blinkLED` függvény villogjon-e a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="f0af8-129">The `blinkLED` function blinks the LED.</span></span>
   
   ![A mintaalkalmazás a tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)
2. <span data-ttu-id="f0af8-131">A konfigurációs fájl inicializálása a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="f0af8-131">Initialize the configuration file by using the following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```
   
   <span data-ttu-id="f0af8-132">Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) ezen a számítógépen a konfigurációk örökölt, így továbbléphet a központi telepítéséhez és futtatásához a mintaalkalmazást a feladatát.</span><span class="sxs-lookup"><span data-stu-id="f0af8-132">If you completed the steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) on this computer, all the configurations are inherited, so you can skip to the task of deploying and running the sample application.</span></span> <span data-ttu-id="f0af8-133">Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) egy másik számítógépre, ki kell cserélni a lekérdezésargumentumban lévő helyőrzőket a `config-raspberrypi.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="f0af8-133">If you completed the steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) on a different computer, you need to replace the placeholders in the `config-raspberrypi.json` file.</span></span> <span data-ttu-id="f0af8-134">A `config-raspberrypi.json` fájl van-e a saját almappája.</span><span class="sxs-lookup"><span data-stu-id="f0af8-134">The `config-raspberrypi.json` file is in the subfolder of your home folder.</span></span>
   
   ![A config-raspberrypi.json fájl tartalma](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* <span data-ttu-id="f0af8-136">Cserélje le **[eszköz állomásnév vagy IP-címe]** Pi vagy az állomásnév futtatásával kapott IP-címét a `devdisco list --eth` parancsot.</span><span class="sxs-lookup"><span data-stu-id="f0af8-136">Replace **[device hostname or IP address]** with the IP address of Pi or the host name that you get by running the `devdisco list --eth` command.</span></span>
* <span data-ttu-id="f0af8-137">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a eszköz kapcsolati karakterlánccal, amely akkor lekéréséhez futtassa a `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="f0af8-137">Replace **[IoT device connection string]** with the device connection string that you get by running the `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` command.</span></span>
* <span data-ttu-id="f0af8-138">Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="f0af8-138">Replace **[IoT hub connection string]** with the IoT hub connection string that you get by running the `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` command.</span></span>

> [!NOTE]
> <span data-ttu-id="f0af8-139">Futtatás **gulp az install-eszközök** és, ha még nem meg lecke 1.</span><span class="sxs-lookup"><span data-stu-id="f0af8-139">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="f0af8-140">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="f0af8-140">Deploy and run the sample application</span></span>
<span data-ttu-id="f0af8-141">Telepíthet, és futtassa a mintaalkalmazást a Pi a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f0af8-141">Deploy and run the sample application on Pi by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="f0af8-142">A parancs telepíti a mintaalkalmazás-Pi tartományban.</span><span class="sxs-lookup"><span data-stu-id="f0af8-142">The command deploys the sample application to Pi.</span></span> <span data-ttu-id="f0af8-143">Az alkalmazás ezt követően az Pi és a számítógépen egy külön feladat 20 villogási üzenetet küldeni Pi-nak az IoT hub fut.</span><span class="sxs-lookup"><span data-stu-id="f0af8-143">Then, it runs the application on Pi and a separate task on your host computer to send 20 blink messages to Pi from your IoT hub.</span></span>

<span data-ttu-id="f0af8-144">A mintaalkalmazás futtatása után kezdődik, az IoT hub származó üzenetek figyel.</span><span class="sxs-lookup"><span data-stu-id="f0af8-144">After the sample application runs, it starts listening to messages from your IoT hub.</span></span> <span data-ttu-id="f0af8-145">Eközben a gulp feladat küld több "villogni" üzenetek az IoT hub-Pi tartományban.</span><span class="sxs-lookup"><span data-stu-id="f0af8-145">Meanwhile, the gulp task sends several "blink" messages from your IoT hub to Pi.</span></span> <span data-ttu-id="f0af8-146">Minden egyes villogási üzenet, amely megkapja a Pi, a mintaalkalmazást meghívja a `blinkLED` működnek, mint a LED villogni.</span><span class="sxs-lookup"><span data-stu-id="f0af8-146">For each blink message that Pi receives, the sample application calls the `blinkLED` function to blink the LED.</span></span>

<span data-ttu-id="f0af8-147">Meg kell jelennie a LED villogási két másodpercenként, mint a gulp feladat 20 üzenetet küld az IoT hub-Pi tartományban.</span><span class="sxs-lookup"><span data-stu-id="f0af8-147">You should see the LED blink every two seconds as the gulp task sends 20 messages from your IoT hub to Pi.</span></span> <span data-ttu-id="f0af8-148">Legutóbb egy "stop" üzenet, amely közli az alkalmazás leáll.</span><span class="sxs-lookup"><span data-stu-id="f0af8-148">The last one is a "stop" message that tells the application to stop running.</span></span>

![A mintaalkalmazás parancs gulp és üzenetek villogni](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="summary"></a><span data-ttu-id="f0af8-150">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="f0af8-150">Summary</span></span>
<span data-ttu-id="f0af8-151">Az IoT hub a pi értékét a LED villogni sikeresen elküldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="f0af8-151">You’ve successfully sent messages from your IoT hub to Pi to blink the LED.</span></span> <span data-ttu-id="f0af8-152">A következő feladat nem kötelező: módosítsa a be- és kikapcsolása a LED viselkedését.</span><span class="sxs-lookup"><span data-stu-id="f0af8-152">The next task is optional: change the on and off behavior of the LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0af8-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0af8-153">Next steps</span></span>
[<span data-ttu-id="f0af8-154">A be- és kikapcsolása a LED viselkedését módosítása</span><span class="sxs-lookup"><span data-stu-id="f0af8-154">Change the on and off behavior of the LED</span></span>](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


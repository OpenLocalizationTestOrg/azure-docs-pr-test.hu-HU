---
title: "Intel Edison (csomópont) csatlakozni az Azure IoT - lecke 4: üzeneteket fogadni |} Microsoft Docs"
description: "A mintaalkalmazás Edison futtatja, és figyeli a bejövő üzenetek az IoT hub. Új gulp feladat üzeneteket küld a az IoT-központ a LED villogni a Edison."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "a webes, a weben keresztül vezetett arduino vezérlő vezetett arduino vezérlő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: bc738bf6-e38d-4024-82d7-39b6c2d4bacb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 76ea59acd848f60663a0c821bff42166aac5823a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a><span data-ttu-id="b5fff-105">Futtassa a mintaalkalmazást a felhő-eszközre küldött üzenetek fogadására</span><span class="sxs-lookup"><span data-stu-id="b5fff-105">Run a sample application to receive cloud-to-device messages</span></span>
<span data-ttu-id="b5fff-106">Ebben a cikkben az Intel Edison minta alkalmazást telepít központilag.</span><span class="sxs-lookup"><span data-stu-id="b5fff-106">In this article, you deploy a sample application on Intel Edison.</span></span> <span data-ttu-id="b5fff-107">A mintaalkalmazás az IoT hub bejövő üzeneteit figyeli.</span><span class="sxs-lookup"><span data-stu-id="b5fff-107">The sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="b5fff-108">Gulp feladatot az IoT hub Edison küldéséhez a számítógépen is futtathat.</span><span class="sxs-lookup"><span data-stu-id="b5fff-108">You also run a gulp task on your computer to send messages to Edison from your IoT hub.</span></span> <span data-ttu-id="b5fff-109">A mintaalkalmazás az üzeneteket fogad, ha azt a LED villogjon.</span><span class="sxs-lookup"><span data-stu-id="b5fff-109">When the sample application receives the messages, it blinks the LED.</span></span> <span data-ttu-id="b5fff-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="b5fff-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="b5fff-111">Mit fog</span><span class="sxs-lookup"><span data-stu-id="b5fff-111">What you will do</span></span>
* <span data-ttu-id="b5fff-112">A mintaalkalmazás az IoT hub való csatlakozás.</span><span class="sxs-lookup"><span data-stu-id="b5fff-112">Connect the sample application to your IoT hub.</span></span>
* <span data-ttu-id="b5fff-113">Központi telepítése, és futtassa a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5fff-113">Deploy and run the sample application.</span></span>
* <span data-ttu-id="b5fff-114">Üzenetek küldése az IoT hub számítógépről Edison villogni fog a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="b5fff-114">Send messages from your IoT hub to Edison to blink the LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="b5fff-115">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="b5fff-115">What you will learn</span></span>
<span data-ttu-id="b5fff-116">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="b5fff-116">In this article, you will learn:</span></span>
* <span data-ttu-id="b5fff-117">Az IoT hub bejövő üzenetek figyelésének módjáról.</span><span class="sxs-lookup"><span data-stu-id="b5fff-117">How to monitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="b5fff-118">Az IoT hub felhő-eszközre küldött üzenetek küldése Edison módjáról.</span><span class="sxs-lookup"><span data-stu-id="b5fff-118">How to send cloud-to-device messages from your IoT hub to Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="b5fff-119">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="b5fff-119">What you need</span></span>
* <span data-ttu-id="b5fff-120">Intel Edison, állítsa be a használatra.</span><span class="sxs-lookup"><span data-stu-id="b5fff-120">Intel Edison, set up for use.</span></span> <span data-ttu-id="b5fff-121">Edison beállításával kapcsolatban a [állítsa be az eszközt][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="b5fff-121">To learn how to set up Edison, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="b5fff-122">Az IoT-központ, amely az Azure-előfizetése jön létre.</span><span class="sxs-lookup"><span data-stu-id="b5fff-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="b5fff-123">Az IoT hub létrehozásával kapcsolatban lásd: [az Azure IoT Hub létrehozása][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="b5fff-123">To learn how to create your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-the-sample-application-to-your-iot-hub"></a><span data-ttu-id="b5fff-124">A mintaalkalmazás az IoT hub való csatlakozáshoz</span><span class="sxs-lookup"><span data-stu-id="b5fff-124">Connect the sample application to your IoT hub</span></span>
1. <span data-ttu-id="b5fff-125">Győződjön meg arról, hogy Ön a tárházban mappában `iot-hub-node-edison-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="b5fff-125">Make sure that you're in the repo folder `iot-hub-node-edison-getting-started`.</span></span> <span data-ttu-id="b5fff-126">Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b5fff-126">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="b5fff-127">A fájl a `app` almappa is figyelheti a bejövő üzenetek az IoT hubból kódot tartalmazó kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="b5fff-127">The file in the `app` subfolder is the key source file that contains the code to monitor incoming messages from the IoT hub.</span></span> <span data-ttu-id="b5fff-128">A `blinkLED` függvény villogjon-e a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="b5fff-128">The `blinkLED` function blinks the LED.</span></span>

   ![A mintaalkalmazás a tárház szerkezete][repo-structure]
2. <span data-ttu-id="b5fff-130">A konfigurációs fájl inicializálása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b5fff-130">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

   <span data-ttu-id="b5fff-131">Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] ezen a számítógépen a konfigurációk örökölt, akkor kihagyhatja a történő központi telepítéséhez és futtatásához a mintaalkalmazást a feladatát.</span><span class="sxs-lookup"><span data-stu-id="b5fff-131">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all the configurations are inherited, so you can skip the step to the task of deploying and running the sample application.</span></span> <span data-ttu-id="b5fff-132">Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] egy másik számítógépre, ki kell cserélni a lekérdezésargumentumban lévő helyőrzőket a `config-edison.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="b5fff-132">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need to replace the placeholders in the `config-edison.json` file.</span></span> <span data-ttu-id="b5fff-133">A `config-edison.json` fájl van-e a saját almappája.</span><span class="sxs-lookup"><span data-stu-id="b5fff-133">The `config-edison.json` file is in the subfolder of your home folder.</span></span>

   ![A config-edison.json fájl tartalma](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * <span data-ttu-id="b5fff-135">Cserélje le **[eszköz állomásnév vagy IP-címe]** az eszköz konfigurálása során az lefelé megjelölt eszköz IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="b5fff-135">Replace **[device hostname or IP address]** with the device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="b5fff-136">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a eszköz kapcsolati karakterlánccal, amely akkor lekéréséhez futtassa a `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b5fff-136">Replace **[IoT device connection string]** with the device connection string that you get by running the `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="b5fff-137">Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a `az iot hub show-connection-string --name {my hub name}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b5fff-137">Replace **[IoT hub connection string]** with the IoT hub connection string that you get by running the `az iot hub show-connection-string --name {my hub name}` command.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="b5fff-138">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="b5fff-138">Deploy and run the sample application</span></span>
<span data-ttu-id="b5fff-139">Telepíthet, és futtassa a mintaalkalmazást a Edison a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b5fff-139">Deploy and run the sample application on Edison by running the following commands:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="b5fff-140">A gulp parancs a mintaalkalmazáshoz történő Edison telepíti.</span><span class="sxs-lookup"><span data-stu-id="b5fff-140">The gulp command deploys the sample application to Edison.</span></span> <span data-ttu-id="b5fff-141">Az alkalmazás ezt követően az Edison és egy külön feladat számítógépen történő üzenetküldéshez 20 villogási Edison az IoT hub a fut.</span><span class="sxs-lookup"><span data-stu-id="b5fff-141">Then, it runs the application on Edison and a separate task on your host computer to send 20 blink messages to Edison from your IoT hub.</span></span>

<span data-ttu-id="b5fff-142">A mintaalkalmazás futtatása után kezdődik, az IoT hub származó üzenetek figyel.</span><span class="sxs-lookup"><span data-stu-id="b5fff-142">After the sample application runs, it starts listening to messages from your IoT hub.</span></span> <span data-ttu-id="b5fff-143">Eközben a gulp feladat több "villogni" üzeneteket küld a az IoT hub Edison.</span><span class="sxs-lookup"><span data-stu-id="b5fff-143">Meanwhile, the gulp task sends several "blink" messages from your IoT hub to Edison.</span></span> <span data-ttu-id="b5fff-144">Minden egyes villogási üzenet, amely megkapja a Edison, a mintaalkalmazást meghívja a `blinkLED` működnek, mint a LED villogni.</span><span class="sxs-lookup"><span data-stu-id="b5fff-144">For each blink message that Edison receives, the sample application calls the `blinkLED` function to blink the LED.</span></span>

<span data-ttu-id="b5fff-145">A gulp feladat 20 üzeneteket küld a az IoT hub Edison, meg kell jelennie a LED villogási két másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="b5fff-145">You should see the LED blink every two seconds as the gulp task sends 20 messages from your IoT hub to Edison.</span></span> <span data-ttu-id="b5fff-146">Legutóbb egy "stop" üzenetet, amely leállítja az alkalmazás futását.</span><span class="sxs-lookup"><span data-stu-id="b5fff-146">The last one is a "stop" message that stops the application from running.</span></span>

![A mintaalkalmazás parancs gulp és üzenetek villogni][gulp-command-and-blink-messages]

## <a name="summary"></a><span data-ttu-id="b5fff-148">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="b5fff-148">Summary</span></span>
<span data-ttu-id="b5fff-149">Az IoT hub a Edison a LED villogni a sikeresen elküldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="b5fff-149">You’ve successfully sent messages from your IoT hub to Edison to blink the LED.</span></span> <span data-ttu-id="b5fff-150">A következő feladat nem kötelező: módosítsa a be- és kikapcsolása a LED viselkedését.</span><span class="sxs-lookup"><span data-stu-id="b5fff-150">The next task is optional: change the on and off behavior of the LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5fff-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b5fff-151">Next steps</span></span>
<span data-ttu-id="b5fff-152">[A be- és kikapcsolása a LED viselkedését módosítása][change-the-on-and-off-behavior-of-the-led]</span><span class="sxs-lookup"><span data-stu-id="b5fff-152">[Change the on and off behavior of the LED][change-the-on-and-off-behavior-of-the-led]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-node-lesson4-change-led-behavior.md
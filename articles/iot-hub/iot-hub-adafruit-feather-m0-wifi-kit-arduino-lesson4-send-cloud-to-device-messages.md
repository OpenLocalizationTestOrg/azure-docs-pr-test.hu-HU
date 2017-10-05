---
title: "Csatlakozás az Azure IoT - 4. lecke Arduino (C): Felhő eszközre |} Microsoft Docs"
description: "A mintaalkalmazás az IoT hub Adafruit lágyított M0 Wi-Fi és figyeli a bejövő üzenetek futtatja. Új gulp feladat üzeneteket küld az IoT-központ a LED villogni a Adafruit lágyított M0 Wi-Fi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "a webes, a weben keresztül vezetett arduino vezérlő vezetett arduino vezérlő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: a0bf53fb-29fb-485f-ba4a-6c715057b1a2
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b4f4c1d86b10b64c104ac812d1f650d723322be8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a><span data-ttu-id="e1167-105">Futtassa a mintaalkalmazást a felhő-eszközre küldött üzenetek fogadására</span><span class="sxs-lookup"><span data-stu-id="e1167-105">Run a sample application to receive cloud-to-device messages</span></span>
<span data-ttu-id="e1167-106">Ebben a cikkben a mintaalkalmazást a Adafruit lágyított M0 Wi-Fi Arduino táblán telepít.</span><span class="sxs-lookup"><span data-stu-id="e1167-106">In this article, you deploy a sample application on your Adafruit Feather M0 WiFi Arduino board.</span></span>

<span data-ttu-id="e1167-107">A mintaalkalmazás az IoT hub bejövő üzeneteit figyeli.</span><span class="sxs-lookup"><span data-stu-id="e1167-107">The sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="e1167-108">Gulp feladatot az IoT hub a Arduino board küldéséhez a számítógépen is futtathat.</span><span class="sxs-lookup"><span data-stu-id="e1167-108">You also run a gulp task on your computer to send messages to your Arduino board from your IoT hub.</span></span> <span data-ttu-id="e1167-109">A mintaalkalmazás az üzeneteket fogad, ha azt a LED villogjon.</span><span class="sxs-lookup"><span data-stu-id="e1167-109">When the sample application receives the messages, it blinks the LED.</span></span> <span data-ttu-id="e1167-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="e1167-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e1167-111">Mit fog</span><span class="sxs-lookup"><span data-stu-id="e1167-111">What you will do</span></span>
* <span data-ttu-id="e1167-112">A mintaalkalmazás az IoT hub való csatlakozás.</span><span class="sxs-lookup"><span data-stu-id="e1167-112">Connect the sample application to your IoT hub.</span></span>
* <span data-ttu-id="e1167-113">Központi telepítése, és futtassa a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e1167-113">Deploy and run the sample application.</span></span>
* <span data-ttu-id="e1167-114">Üzenetek küldése az IoT hub számítógépről a Arduino board villogni fog a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="e1167-114">Send messages from your IoT hub to your Arduino board to blink the LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e1167-115">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="e1167-115">What you will learn</span></span>
<span data-ttu-id="e1167-116">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="e1167-116">In this article, you will learn:</span></span>
* <span data-ttu-id="e1167-117">Az IoT hub bejövő üzenetek figyelésének módjáról.</span><span class="sxs-lookup"><span data-stu-id="e1167-117">How to monitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="e1167-118">Az IoT hub felhő-eszközre küldött üzenetek küldése a Arduino board módjáról.</span><span class="sxs-lookup"><span data-stu-id="e1167-118">How to send cloud-to-device messages from your IoT hub to your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e1167-119">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="e1167-119">What you need</span></span>
* <span data-ttu-id="e1167-120">A Arduino üzenőfalon, állítsa be a használatra.</span><span class="sxs-lookup"><span data-stu-id="e1167-120">Your Arduino board, set up for use.</span></span> <span data-ttu-id="e1167-121">A Arduino board beállításával kapcsolatban a [állítsa be az eszközt][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="e1167-121">To learn how to set up your Arduino board, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="e1167-122">Az IoT-központ, amely az Azure-előfizetése jön létre.</span><span class="sxs-lookup"><span data-stu-id="e1167-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="e1167-123">Az IoT hub létrehozásával kapcsolatban lásd: [az Azure IoT Hub létrehozása][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="e1167-123">To learn how to create your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-the-sample-application-to-your-iot-hub"></a><span data-ttu-id="e1167-124">A mintaalkalmazás az IoT hub való csatlakozáshoz</span><span class="sxs-lookup"><span data-stu-id="e1167-124">Connect the sample application to your IoT hub</span></span>

1. <span data-ttu-id="e1167-125">Győződjön meg arról, hogy Ön a tárházban mappában `iot-hub-c-feather-m0-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="e1167-125">Make sure that you're in the repo folder `iot-hub-c-feather-m0-getting-started`.</span></span>

   <span data-ttu-id="e1167-126">Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e1167-126">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="e1167-127">Figyelje meg a `app.ino` fájlt a `app` almappájában.</span><span class="sxs-lookup"><span data-stu-id="e1167-127">Notice the `app.ino` file in the `app` subfolder.</span></span> <span data-ttu-id="e1167-128">A `app.ino` a forrás-fájl, amely tartalmazza a kódot a figyelheti a bejövő üzenetek az IoT hubból.</span><span class="sxs-lookup"><span data-stu-id="e1167-128">The `app.ino` file is the key source file that contains the code to monitor incoming messages from the IoT hub.</span></span> <span data-ttu-id="e1167-129">A `blinkLED` függvény villogjon-e a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="e1167-129">The `blinkLED` function blinks the LED.</span></span>

   ![A mintaalkalmazás a tárház szerkezete][repo-structure]

2. <span data-ttu-id="e1167-131">A soros port az eszköz az eszköz-felderítési cli az beszerzése:</span><span class="sxs-lookup"><span data-stu-id="e1167-131">Obtain the serial port of the device with the device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="e1167-132">Tekintse meg a következőhöz hasonló kimenetnek kell, és a Arduino kártya COM-portot az usb található:</span><span class="sxs-lookup"><span data-stu-id="e1167-132">You should see an output that is similar to the following and find the usb COM port for your Arduino board:</span></span>

   ![Eszköz felderítése][device-discovery]

3. <span data-ttu-id="e1167-134">Nyissa meg a fájlt `config.json` a lecke mappában, és a bemeneti értékét a tényleges COM-port száma:</span><span class="sxs-lookup"><span data-stu-id="e1167-134">Open the file `config.json` in the lesson folder and input the value of the found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="e1167-136">A COM-port, a Windows platformra, formátuma rendelkezik `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="e1167-136">For the COM port, on Windows platform, it has the format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="e1167-137">MacOS vagy Ubuntu, elindul a `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="e1167-137">On macOS or Ubuntu, it will start with `/dev/`.</span></span>

4. <span data-ttu-id="e1167-138">A konfigurációs fájl inicializálása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e1167-138">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. <span data-ttu-id="e1167-139">Ellenőrizze a következő cserékhez a `config-arduino.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="e1167-139">Make the following replacements in the `config-arduino.json` file:</span></span>

   <span data-ttu-id="e1167-140">Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] ezen a számítógépen a konfigurációk örökölt, akkor kihagyhatja a történő központi telepítéséhez és futtatásához a mintaalkalmazást a feladatát.</span><span class="sxs-lookup"><span data-stu-id="e1167-140">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all the configurations are inherited, so you can skip the step to the task of deploying and running the sample application.</span></span> <span data-ttu-id="e1167-141">Ha végrehajtotta a lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] egy másik számítógépre, ki kell cserélni a lekérdezésargumentumban lévő helyőrzőket a `config-arduino.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="e1167-141">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need to replace the placeholders in the `config-arduino.json` file.</span></span> <span data-ttu-id="e1167-142">A `config-arduino.json` fájl van-e a saját almappája.</span><span class="sxs-lookup"><span data-stu-id="e1167-142">The `config-arduino.json` file is in the subfolder of your home folder.</span></span>

   ![A config-arduino.json fájl tartalma][config-arduino-json]

   * <span data-ttu-id="e1167-144">Cserélje le **[Wi-Fi SSID]** rendelkező a Wi-Fi SSID, amely kapcsolódik az internethez.</span><span class="sxs-lookup"><span data-stu-id="e1167-144">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected to the Internet.</span></span>
   * <span data-ttu-id="e1167-145">Cserélje le **[Wi-Fi jelszó]** a Wi-Fi jelszóval.</span><span class="sxs-lookup"><span data-stu-id="e1167-145">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="e1167-146">Távolítsa el a karakterláncot, ha a Wi-Fi nem követeli meg.</span><span class="sxs-lookup"><span data-stu-id="e1167-146">Remove the string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="e1167-147">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a eszköz kapcsolati karakterlánccal, amely akkor lekéréséhez futtassa a `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e1167-147">Replace **[IoT device connection string]** with the device connection string that you get by running the `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="e1167-148">Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a `az iot hub show-connection-string --name {my hub name}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e1167-148">Replace **[IoT hub connection string]** with the IoT hub connection string that you get by running the `az iot hub show-connection-string --name {my hub name}` command.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="e1167-149">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="e1167-149">Deploy and run the sample application</span></span>
<span data-ttu-id="e1167-150">Telepíthet, és futtassa a mintaalkalmazást a Arduino táblán a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e1167-150">Deploy and run the sample application on your Arduino board by running the following commands:</span></span>

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="e1167-151">A gulp parancs telepíti a mintaalkalmazást a Arduino kártyához.</span><span class="sxs-lookup"><span data-stu-id="e1167-151">The gulp command deploys the sample application to your Arduino board.</span></span> <span data-ttu-id="e1167-152">Ezt követően azt futtatja az alkalmazást a Arduino tábla és egy külön feladat 20 villogási üzenetet küldeni a Arduino board-nak az IoT hub a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="e1167-152">Then, it runs the application on your Arduino board and a separate task on your host computer to send 20 blink messages to your Arduino board from your IoT hub.</span></span>

<span data-ttu-id="e1167-153">A mintaalkalmazás futtatása után kezdődik, az IoT hub származó üzenetek figyel.</span><span class="sxs-lookup"><span data-stu-id="e1167-153">After the sample application runs, it starts listening to messages from your IoT hub.</span></span> <span data-ttu-id="e1167-154">Eközben a gulp feladat több "villogni" üzeneteket küld a az IoT hub a Arduino tábla.</span><span class="sxs-lookup"><span data-stu-id="e1167-154">Meanwhile, the gulp task sends several "blink" messages from your IoT hub to your Arduino board.</span></span> <span data-ttu-id="e1167-155">Minden egyes villogási üzenet, amely megkapja a táblán, a mintaalkalmazást meghívja a `blinkLED` működnek, mint a LED villogni.</span><span class="sxs-lookup"><span data-stu-id="e1167-155">For each blink message that the board receives, the sample application calls the `blinkLED` function to blink the LED.</span></span>

<span data-ttu-id="e1167-156">Mivel a gulp feladat 20 üzenetet küld az IoT hub a Arduino kártyához kell megjelennie a LED villogási két másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="e1167-156">You should see the LED blink every two seconds as the gulp task sends 20 messages from your IoT hub to your Arduino board.</span></span> <span data-ttu-id="e1167-157">Legutóbb egy "stop" üzenetet, amely leállítja az alkalmazás futását.</span><span class="sxs-lookup"><span data-stu-id="e1167-157">The last one is a "stop" message that stops the application from running.</span></span>

![A mintaalkalmazás parancs gulp és üzenetek villogni][sample-application]

## <a name="summary"></a><span data-ttu-id="e1167-159">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e1167-159">Summary</span></span>
<span data-ttu-id="e1167-160">Az IoT hub a a Arduino board a LED villogni a sikeresen elküldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="e1167-160">You’ve successfully sent messages from your IoT hub to your Arduino board to blink the LED.</span></span> <span data-ttu-id="e1167-161">A következő feladat nem kötelező: módosítsa a be- és kikapcsolása a LED viselkedését.</span><span class="sxs-lookup"><span data-stu-id="e1167-161">The next task is optional: change the on and off behavior of the LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1167-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1167-162">Next steps</span></span>
<span data-ttu-id="e1167-163">[A be- és kikapcsolása a LED viselkedését módosítása][change-the-on-and-off-led-behavior]</span><span class="sxs-lookup"><span data-stu-id="e1167-163">[Change the on and off behavior of the LED][change-the-on-and-off-led-behavior]</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/repo_structure_arduino.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[create-an-azure-function-app-and-storage-account]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/config-arduino.png
[sample-application]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_blink_arduino.png
[change-the-on-and-off-led-behavior]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-change-led-behavior.md
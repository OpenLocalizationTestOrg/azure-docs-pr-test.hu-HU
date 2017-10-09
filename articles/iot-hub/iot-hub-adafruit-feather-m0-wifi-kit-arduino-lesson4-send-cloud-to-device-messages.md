---
title: "Connect Arduino (C) tooAzure IoT - lecke 4: Felhő eszközre |} Microsoft Docs"
description: "A mintaalkalmazás az IoT hub Adafruit lágyított M0 Wi-Fi és figyeli a bejövő üzenetek futtatja. Egy új gulp feladatot az IoT hub tooblink hello LED üzenetek tooAdafruit lágyított M0 Wi-Fi küldi."
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
ms.openlocfilehash: dcddd61ff684f49436103675938d719cb227c409
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="a4bc5-105">Futtassa egy minta alkalmazás tooreceive felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="a4bc5-105">Run a sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="a4bc5-106">Ebben a cikkben a mintaalkalmazást a Adafruit lágyított M0 Wi-Fi Arduino táblán telepít.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-106">In this article, you deploy a sample application on your Adafruit Feather M0 WiFi Arduino board.</span></span>

<span data-ttu-id="a4bc5-107">hello mintaalkalmazást az IoT hub bejövő üzeneteit figyeli.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="a4bc5-108">Akkor is futtassa gulp feladat a számítógép toosend üzenetek tooyour Arduino board az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-108">You also run a gulp task on your computer toosend messages tooyour Arduino board from your IoT hub.</span></span> <span data-ttu-id="a4bc5-109">Hello mintaalkalmazás hello üzeneteket fogad, ha azt villogjon-hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="a4bc5-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="a4bc5-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="a4bc5-111">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a4bc5-111">What you will do</span></span>
* <span data-ttu-id="a4bc5-112">Csatlakozás hello minta alkalmazás tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="a4bc5-113">Regisztrálhat és futtathat hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="a4bc5-114">Az IoT hub tooyour Arduino board tooblink hello LED küldhet üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-114">Send messages from your IoT hub tooyour Arduino board tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a4bc5-115">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a4bc5-115">What you will learn</span></span>
<span data-ttu-id="a4bc5-116">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="a4bc5-116">In this article, you will learn:</span></span>
* <span data-ttu-id="a4bc5-117">Hogyan toomonitor bejövő az IoT hub érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-117">How toomonitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="a4bc5-118">Hogyan toosend felhő eszközt az IoT hub tooyour Arduino board érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-118">How toosend cloud-to-device messages from your IoT hub tooyour Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a4bc5-119">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a4bc5-119">What you need</span></span>
* <span data-ttu-id="a4bc5-120">A Arduino üzenőfalon, állítsa be a használatra.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-120">Your Arduino board, set up for use.</span></span> <span data-ttu-id="a4bc5-121">Hogyan fel a Arduino üzenőfalon, tooset: toolearn [állítsa be az eszközt][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="a4bc5-121">toolearn how tooset up your Arduino board, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="a4bc5-122">Az IoT-központ, amely az Azure-előfizetése jön létre.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="a4bc5-123">toolearn hogyan toocreate az IoT hub, lásd: [az Azure IoT Hub létrehozása][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="a4bc5-123">toolearn how toocreate your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="a4bc5-124">Csatlakozás hello minta alkalmazás tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="a4bc5-124">Connect hello sample application tooyour IoT hub</span></span>

1. <span data-ttu-id="a4bc5-125">Győződjön meg arról, hogy Ön hello tárház mappában `iot-hub-c-feather-m0-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-125">Make sure that you're in hello repo folder `iot-hub-c-feather-m0-getting-started`.</span></span>

   <span data-ttu-id="a4bc5-126">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a4bc5-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="a4bc5-127">Értesítés hello `app.ino` hello fájlban `app` almappájában.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-127">Notice hello `app.ino` file in hello `app` subfolder.</span></span> <span data-ttu-id="a4bc5-128">Hello `app.ino` hello kód toomonitor bejövő üzenetek hello IoT hubról tartalmazó hello forrás-fájl.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-128">hello `app.ino` file is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="a4bc5-129">Hello `blinkLED` függvény villogjon hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-129">hello `blinkLED` function blinks hello LED.</span></span>

   ![Hello mintaalkalmazást a tárház szerkezete][repo-structure]

2. <span data-ttu-id="a4bc5-131">Hello soros port hello eszköz hello eszköz felderítési cli az beszerzése:</span><span class="sxs-lookup"><span data-stu-id="a4bc5-131">Obtain hello serial port of hello device with hello device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="a4bc5-132">Meg kell, amely hasonló toohello következő kimenetnek, és található hello usb COM-portot a Arduino Bizottság:</span><span class="sxs-lookup"><span data-stu-id="a4bc5-132">You should see an output that is similar toohello following and find hello usb COM port for your Arduino board:</span></span>

   ![Eszköz felderítése][device-discovery]

3. <span data-ttu-id="a4bc5-134">Nyissa meg hello fájl `config.json` hello lecke mappában, és a bemeneti hello értékének hello található COM-port száma:</span><span class="sxs-lookup"><span data-stu-id="a4bc5-134">Open hello file `config.json` in hello lesson folder and input hello value of hello found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="a4bc5-136">Hello COM-porthoz, a Windows platformra, hello formátuma rendelkezik `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-136">For hello COM port, on Windows platform, it has hello format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="a4bc5-137">MacOS vagy Ubuntu, elindul a `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-137">On macOS or Ubuntu, it will start with `/dev/`.</span></span>

4. <span data-ttu-id="a4bc5-138">Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a4bc5-138">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. <span data-ttu-id="a4bc5-139">Ellenőrizze a következő cserékhez a hello hello `config-arduino.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="a4bc5-139">Make hello following replacements in hello `config-arduino.json` file:</span></span>

   <span data-ttu-id="a4bc5-140">Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] ezen a számítógépen minden hello konfigurációk örökölt, így továbbléphet hello lépés toohello feladat történő telepítésének és hello minta alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-140">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all hello configurations are inherited, so you can skip hello step toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="a4bc5-141">Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása] [ create-an-azure-function-app-and-storage-account] egy másik számítógépen kell tooreplace hello helyőrzőt hello `config-arduino.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-141">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need tooreplace hello placeholders in hello `config-arduino.json` file.</span></span> <span data-ttu-id="a4bc5-142">Hello `config-arduino.json` fájl van-e a kezdőmappa hello almappája.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-142">hello `config-arduino.json` file is in hello subfolder of your home folder.</span></span>

   ![Hello config-arduino.json fájl tartalma][config-arduino-json]

   * <span data-ttu-id="a4bc5-144">Cserélje le **[Wi-Fi SSID]** rendelkező a Wi-Fi SSID toohello internethez csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-144">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected toohello Internet.</span></span>
   * <span data-ttu-id="a4bc5-145">Cserélje le **[Wi-Fi jelszó]** a Wi-Fi jelszóval.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-145">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="a4bc5-146">Távolítsa el a hello karakterlánc, ha a Wi-Fi nem követeli meg.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-146">Remove hello string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="a4bc5-147">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** hello eszköz kapcsolati karakterlánccal hello futtatásával kapott `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-147">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="a4bc5-148">Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterláncot kapott hello futtatásával hello `az iot hub show-connection-string --name {my hub name}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-148">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name}` command.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="a4bc5-149">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="a4bc5-149">Deploy and run hello sample application</span></span>
<span data-ttu-id="a4bc5-150">Telepíthet, és futtassa a hello mintaalkalmazást a Arduino táblán hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a4bc5-150">Deploy and run hello sample application on your Arduino board by running hello following commands:</span></span>

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="a4bc5-151">hello gulp parancs hello minta alkalmazás tooyour Arduino board telepíti.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-151">hello gulp command deploys hello sample application tooyour Arduino board.</span></span> <span data-ttu-id="a4bc5-152">Ezt követően hello alkalmazás az fut a Arduino tábla és egy külön feladat a gazdagépen futó számítógép toosend 20 villogási üzenetek tooyour Arduino board az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-152">Then, it runs hello application on your Arduino board and a separate task on your host computer toosend 20 blink messages tooyour Arduino board from your IoT hub.</span></span>

<span data-ttu-id="a4bc5-153">Hello mintaalkalmazás futtatása után kezdődik toomessages az IoT-központ figyel.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-153">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="a4bc5-154">Eközben hello gulp feladat több "villogni" üzenet küldi az IoT hub tooyour Arduino tábla.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-154">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooyour Arduino board.</span></span> <span data-ttu-id="a4bc5-155">A tábla hello villogási üzenetek fogadását, hello mintaalkalmazás meghívja a hello `blinkLED` függvény tooblink hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-155">For each blink message that hello board receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="a4bc5-156">Megtekintheti az hello LED villogni két másodpercenként, hello gulp feladat 20 üzenetet küldi az IoT hub tooyour Arduino tábla.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-156">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooyour Arduino board.</span></span> <span data-ttu-id="a4bc5-157">hello utolsó egyik hello alkalmazás futtatását megakadályozó "stop" üzenet.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-157">hello last one is a "stop" message that stops hello application from running.</span></span>

![A mintaalkalmazás parancs gulp és üzenetek villogni][sample-application]

## <a name="summary"></a><span data-ttu-id="a4bc5-159">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a4bc5-159">Summary</span></span>
<span data-ttu-id="a4bc5-160">Az IoT hub tooyour Arduino board tooblink hello LED a sikeresen elküldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-160">You’ve successfully sent messages from your IoT hub tooyour Arduino board tooblink hello LED.</span></span> <span data-ttu-id="a4bc5-161">hello következő feladat nem kötelező: hello be- és kikapcsolását hello LED viselkedésének módosítása.</span><span class="sxs-lookup"><span data-stu-id="a4bc5-161">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4bc5-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a4bc5-162">Next steps</span></span>
<span data-ttu-id="a4bc5-163">[Hello be- és kikapcsolását hello LED viselkedésének módosítása][change-the-on-and-off-led-behavior]</span><span class="sxs-lookup"><span data-stu-id="a4bc5-163">[Change hello on and off behavior of hello LED][change-the-on-and-off-led-behavior]</span></span>


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
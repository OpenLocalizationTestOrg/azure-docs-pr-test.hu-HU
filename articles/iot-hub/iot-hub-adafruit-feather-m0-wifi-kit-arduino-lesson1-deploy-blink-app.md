---
title: "Csatlakozzon az Azure IoT - 1. lecke Arduino: alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza a mintaalkalmazást Arduino a Githubból, és futtassa ezt az alkalmazást a Adafruit lágyított M0 Wi-Fi telepítendő gulp. A mintaalkalmazás villogjon-e a GPIO"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vezetett projektek, arduino vezetett villogási, vezetett arduino villogási arduino villogási program, a arduino villogási – példa"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: b0a7d076-d580-4686-9f7d-c0712750b615
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4431808ac6182d194e841c087c8f89f1a12b1911
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="1f973-105">A villogóalkalmazás elkészítése és üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="1f973-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="1f973-106">Mit fog</span><span class="sxs-lookup"><span data-stu-id="1f973-106">What you will do</span></span>
<span data-ttu-id="1f973-107">Klónozza a mintaalkalmazást Arduino a Githubból, és a Adafruit lágyított M0 Wi-Fi Arduino táblán a mintaalkalmazás telepítése a gulp eszközzel.</span><span class="sxs-lookup"><span data-stu-id="1f973-107">Clone the sample Arduino application from GitHub, and use the gulp tool to deploy the sample application to your Adafruit Feather M0 WiFi Arduino board.</span></span> <span data-ttu-id="1f973-108">A minta alkalmazás villogás a GPIO #13 a-barod két másodpercenként vezetett.</span><span class="sxs-lookup"><span data-stu-id="1f973-108">The sample application blinks the GPIO #13 on-barod LED every two seconds.</span></span>

<span data-ttu-id="1f973-109">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting-page].</span><span class="sxs-lookup"><span data-stu-id="1f973-109">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting-page].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="1f973-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="1f973-110">What you will learn</span></span>
* <span data-ttu-id="1f973-111">Hogyan telepítheti, és futtassa a mintaalkalmazást a Arduino táblán.</span><span class="sxs-lookup"><span data-stu-id="1f973-111">How to deploy and run the sample application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="1f973-112">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="1f973-112">What you need</span></span>
<span data-ttu-id="1f973-113">Sikeresen végrehajtotta a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="1f973-113">You must have successfully completed the following operations:</span></span>

* <span data-ttu-id="1f973-114">[Állítsa be az eszközt][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="1f973-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="1f973-115">[Eszközök][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="1f973-115">[Get the tools][get-the-tools]</span></span>

## <a name="open-the-sample-application"></a><span data-ttu-id="1f973-116">Nyissa meg a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="1f973-116">Open the sample application</span></span>
<span data-ttu-id="1f973-117">A mintaalkalmazás megnyitásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1f973-117">To open the sample application, follow these steps:</span></span>

1. <span data-ttu-id="1f973-118">A Githubból a minta-tárház klónozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1f973-118">Clone the sample repository from GitHub by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. <span data-ttu-id="1f973-119">Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1f973-119">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Tárház szerkezete][repo-structure]

<span data-ttu-id="1f973-121">A `app.ino` fájlt a `app` almappa is szabályozhatja a LED kódot tartalmazó kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="1f973-121">The `app.ino` file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="1f973-122">Telepítse az alkalmazásfüggőségek</span><span class="sxs-lookup"><span data-stu-id="1f973-122">Install application dependencies</span></span>
<span data-ttu-id="1f973-123">A szalagtárak és egyéb modulok a következő parancs futtatásával kell a mintaalkalmazás telepítése:</span><span class="sxs-lookup"><span data-stu-id="1f973-123">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="1f973-124">Az eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1f973-124">Configure the device connection</span></span>
<span data-ttu-id="1f973-125">Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1f973-125">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="1f973-126">A soros port az eszköz az eszköz-felderítési cli az beszerzése:</span><span class="sxs-lookup"><span data-stu-id="1f973-126">Obtain the serial port of the device with the device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="1f973-127">Kell egy a következőhöz hasonló kimenetnek és keresése a usb COM-portot a Arduino board: ![eszköz felderítése][device-discovery]</span><span class="sxs-lookup"><span data-stu-id="1f973-127">You should see an output that is similar to the following and find the usb COM port for your Arduino board: ![Device discovery][device-discovery]</span></span>

2. <span data-ttu-id="1f973-128">Nyissa meg a fájlt `config.json` a lecke mappában, és adja hozzá az érték a tényleges COM-port száma:</span><span class="sxs-lookup"><span data-stu-id="1f973-128">Open the file `config.json` in the lesson folder and add the value of the found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > <span data-ttu-id="1f973-130">A COM-port, a Windows platformra, formátuma rendelkezik `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="1f973-130">For the COM port, on Windows platform, it has the format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="1f973-131">MacOS vagy Ubuntu, kezdődik `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="1f973-131">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="1f973-132">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="1f973-132">Deploy and run the sample application</span></span>
### <a name="install-the-required-tools-for-your-arduino-board"></a><span data-ttu-id="1f973-133">A szükséges telepíti a Arduino kártya</span><span class="sxs-lookup"><span data-stu-id="1f973-133">Install the required tools for your Arduino board</span></span>

<span data-ttu-id="1f973-134">Az Azure IoT Hub SDK a Arduino kártya telepítése a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1f973-134">Install the Azure IoT Hub SDK for your Arduino board by running the following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="1f973-135">Ez a feladat befejeződik, attól függően, hogy a hálózati kapcsolat hosszú időbe telhet.</span><span class="sxs-lookup"><span data-stu-id="1f973-135">This task might take a long time to complete, depending on your network connection.</span></span>

> [!NOTE]
> <span data-ttu-id="1f973-136">Lépjen ki a futó Arduino IDE-példány, gulp feladatok futtatásakor: `install-tools`, `run`.</span><span class="sxs-lookup"><span data-stu-id="1f973-136">Please exit the running Arduino IDE instance when running gulp tasks: `install-tools`, `run`.</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="1f973-137">Központi telepítése és a mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="1f973-137">Deploy and run the sample app</span></span>
<span data-ttu-id="1f973-138">Telepíthet, és futtassa a mintaalkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1f973-138">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp run

# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-the-app-works"></a><span data-ttu-id="1f973-139">Ellenőrizze az alkalmazás akkor működik</span><span class="sxs-lookup"><span data-stu-id="1f973-139">Verify the app works</span></span>
<span data-ttu-id="1f973-140">Ha nem látja a LED villogó, tekintse meg a [hibaelhárítási útmutatója] [ troubleshooting-page] gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="1f973-140">If you don’t see the LED blinking, see the [troubleshooting guide][troubleshooting-page] for solutions to common problems.</span></span>

![LED villogó][led-blinking]

## <a name="summary"></a><span data-ttu-id="1f973-142">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="1f973-142">Summary</span></span>
<span data-ttu-id="1f973-143">A Arduino board használható szükséges eszközök telepítése és a LED villogni fog a Arduino kártyához mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="1f973-143">You've installed the required tools to work with your Arduino board and deployed a sample application to your Arduino board to blink the LED.</span></span> <span data-ttu-id="1f973-144">Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti a Arduino board Azure IoT Hub az üzeneteket küldjön és fogadjon.</span><span class="sxs-lookup"><span data-stu-id="1f973-144">You can now create, deploy, and run another sample application that connects your Arduino board to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f973-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1f973-145">Next steps</span></span>
<span data-ttu-id="1f973-146">[Az Azure eszközök][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="1f973-146">[Get the Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
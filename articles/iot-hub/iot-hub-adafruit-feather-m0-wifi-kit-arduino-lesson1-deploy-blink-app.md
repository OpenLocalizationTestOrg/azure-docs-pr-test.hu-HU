---
title: "Csatlakozás Arduino tooAzure IoT - lecke 1: alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza Arduino mintaalkalmazás hello a Githubból, és futtassa a gulp toodeploy az alkalmazás tooyour Adafruit lágyított M0 Wi-Fi. A mintaalkalmazás villogjon hello GPIO"
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
ms.openlocfilehash: 5bf8e4ae88e070aeacf34bfc43b8d2daeeb1a2fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="54467-105">Hello villogási alkalmazás létrehozását és telepítését</span><span class="sxs-lookup"><span data-stu-id="54467-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="54467-106">Mit fog</span><span class="sxs-lookup"><span data-stu-id="54467-106">What you will do</span></span>
<span data-ttu-id="54467-107">Klónozza Arduino mintaalkalmazás hello a Githubból, és hello gulp eszköz toodeploy hello minta alkalmazás tooyour Adafruit lágyított M0 Wi-Fi Arduino tábla használatával.</span><span class="sxs-lookup"><span data-stu-id="54467-107">Clone hello sample Arduino application from GitHub, and use hello gulp tool toodeploy hello sample application tooyour Adafruit Feather M0 WiFi Arduino board.</span></span> <span data-ttu-id="54467-108">hello minta alkalmazás villogás hello barod GPIO #13 két másodpercenként vezetett.</span><span class="sxs-lookup"><span data-stu-id="54467-108">hello sample application blinks hello GPIO #13 on-barod LED every two seconds.</span></span>

<span data-ttu-id="54467-109">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting-page].</span><span class="sxs-lookup"><span data-stu-id="54467-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting-page].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="54467-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="54467-110">What you will learn</span></span>
* <span data-ttu-id="54467-111">Hogyan toodeploy és futtatási hello mintaalkalmazást a Arduino táblán.</span><span class="sxs-lookup"><span data-stu-id="54467-111">How toodeploy and run hello sample application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="54467-112">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="54467-112">What you need</span></span>
<span data-ttu-id="54467-113">Sikeresen végrehajtotta a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="54467-113">You must have successfully completed hello following operations:</span></span>

* <span data-ttu-id="54467-114">[Állítsa be az eszközt][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="54467-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="54467-115">[Hello eszközök beszerzése][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="54467-115">[Get hello tools][get-the-tools]</span></span>

## <a name="open-hello-sample-application"></a><span data-ttu-id="54467-116">Nyissa meg hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="54467-116">Open hello sample application</span></span>
<span data-ttu-id="54467-117">tooopen hello mintaalkalmazást, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="54467-117">tooopen hello sample application, follow these steps:</span></span>

1. <span data-ttu-id="54467-118">A Githubból hello minta tárház klónozása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="54467-118">Clone hello sample repository from GitHub by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. <span data-ttu-id="54467-119">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="54467-119">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Tárház szerkezete][repo-structure]

<span data-ttu-id="54467-121">Hello `app.ino` hello fájlban `app` almappa is hello kód toocontrol hello LED tartalmazó hello kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="54467-121">hello `app.ino` file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="54467-122">Telepítse az alkalmazásfüggőségek</span><span class="sxs-lookup"><span data-stu-id="54467-122">Install application dependencies</span></span>
<span data-ttu-id="54467-123">Hello szalagtárak és más modulok hello a következő parancs futtatásával kell hello mintaalkalmazás telepítése:</span><span class="sxs-lookup"><span data-stu-id="54467-123">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="54467-124">Hello eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54467-124">Configure hello device connection</span></span>
<span data-ttu-id="54467-125">tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="54467-125">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="54467-126">Hello soros port hello eszköz hello eszköz felderítési cli az beszerzése:</span><span class="sxs-lookup"><span data-stu-id="54467-126">Obtain hello serial port of hello device with hello device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="54467-127">Kell egy hasonló toohello következő kimenetnek és található hello usb COM-portot a Arduino board: ![eszköz felderítése][device-discovery]</span><span class="sxs-lookup"><span data-stu-id="54467-127">You should see an output that is similar toohello following and find hello usb COM port for your Arduino board: ![Device discovery][device-discovery]</span></span>

2. <span data-ttu-id="54467-128">Nyissa meg hello fájl `config.json` a hello lecke mappa, és adja hozzá a COM-port száma található hello hello értékét:</span><span class="sxs-lookup"><span data-stu-id="54467-128">Open hello file `config.json` in hello lesson folder and add hello value of hello found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > <span data-ttu-id="54467-130">Hello COM-porthoz, a Windows platformra, hello formátuma rendelkezik `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="54467-130">For hello COM port, on Windows platform, it has hello format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="54467-131">MacOS vagy Ubuntu, kezdődik `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="54467-131">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="54467-132">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="54467-132">Deploy and run hello sample application</span></span>
### <a name="install-hello-required-tools-for-your-arduino-board"></a><span data-ttu-id="54467-133">A Arduino kártya szükséges hello eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="54467-133">Install hello required tools for your Arduino board</span></span>

<span data-ttu-id="54467-134">Hello Azure IoT Hub SDK a Arduino kártya telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="54467-134">Install hello Azure IoT Hub SDK for your Arduino board by running hello following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="54467-135">Ez a feladat egy hosszú ideig toocomplete, attól függően, hogy a hálózati kapcsolatot is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="54467-135">This task might take a long time toocomplete, depending on your network connection.</span></span>

> [!NOTE]
> <span data-ttu-id="54467-136">Lépjen ki a Arduino IDE-példányt futtató gulp feladatok futtatásakor hello: `install-tools`, `run`.</span><span class="sxs-lookup"><span data-stu-id="54467-136">Please exit hello running Arduino IDE instance when running gulp tasks: `install-tools`, `run`.</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="54467-137">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="54467-137">Deploy and run hello sample app</span></span>
<span data-ttu-id="54467-138">Telepíthet, és futtassa a hello mintaalkalmazást hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="54467-138">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp run

# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="54467-139">Ellenőrizze a hello az alkalmazás akkor működik</span><span class="sxs-lookup"><span data-stu-id="54467-139">Verify hello app works</span></span>
<span data-ttu-id="54467-140">Ha nem lát hello LED villogó, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting-page] a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="54467-140">If you don’t see hello LED blinking, see hello [troubleshooting guide][troubleshooting-page] for solutions toocommon problems.</span></span>

![LED villogó][led-blinking]

## <a name="summary"></a><span data-ttu-id="54467-142">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="54467-142">Summary</span></span>
<span data-ttu-id="54467-143">Hello szükséges eszközök toowork telepítése a Arduino üzenőfalon, és telepített egy minta alkalmazás tooyour Arduino board tooblink hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="54467-143">You've installed hello required tools toowork with your Arduino board and deployed a sample application tooyour Arduino board tooblink hello LED.</span></span> <span data-ttu-id="54467-144">Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti a Arduino board tooAzure IoT-központ toosend és üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="54467-144">You can now create, deploy, and run another sample application that connects your Arduino board tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54467-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="54467-145">Next steps</span></span>
<span data-ttu-id="54467-146">[Első hello Azure-eszközök][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="54467-146">[Get hello Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
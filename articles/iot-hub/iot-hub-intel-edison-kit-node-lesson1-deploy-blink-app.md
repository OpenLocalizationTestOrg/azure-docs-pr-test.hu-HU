---
title: "Intel Edison (csomópont) csatlakozni az Azure IoT - 1. lecke: alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza a C mintaalkalmazást a Githubból, és futtassa a gulp az Intel Edison board az alkalmazás telepítéséhez. A mintaalkalmazás villogjon a kártyához csatlakoztatott két másodpercenként LED-jét."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vezetett projektek, arduino vezetett villogási, vezetett arduino villogási arduino villogási program, a arduino villogási – példa"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: ed2c21d0-c72c-4ac2-9e70-347e9a0711c0
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8490fbbf14183432c665165412f00955d6323580
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="dbf61-105">A villogóalkalmazás elkészítése és üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="dbf61-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="dbf61-106">Mit fog</span><span class="sxs-lookup"><span data-stu-id="dbf61-106">What you will do</span></span>
<span data-ttu-id="dbf61-107">Klónozza a C mintaalkalmazást a Githubból, és a segítségével gulp az Intel Edison minta alkalmazást telepíti.</span><span class="sxs-lookup"><span data-stu-id="dbf61-107">Clone the sample C application from GitHub, and use the gulp tool to deploy the sample application to Intel Edison.</span></span> <span data-ttu-id="dbf61-108">A mintaalkalmazás villogjon a kártyához csatlakoztatott két másodpercenként LED-jét.</span><span class="sxs-lookup"><span data-stu-id="dbf61-108">The sample application blinks the LED connected to the board every two seconds.</span></span> <span data-ttu-id="dbf61-109">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="dbf61-109">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="dbf61-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="dbf61-110">What you will learn</span></span>
* <span data-ttu-id="dbf61-111">Hogyan telepítheti, és futtassa a mintaalkalmazást a Edison.</span><span class="sxs-lookup"><span data-stu-id="dbf61-111">How to deploy and run the sample application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="dbf61-112">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="dbf61-112">What you need</span></span>
<span data-ttu-id="dbf61-113">Sikeresen végrehajtotta a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="dbf61-113">You must have successfully completed the following operations:</span></span>

* <span data-ttu-id="dbf61-114">[Állítsa be az eszközt][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="dbf61-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="dbf61-115">[Eszközök][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="dbf61-115">[Get the tools][get-the-tools]</span></span>

## <a name="open-the-sample-application"></a><span data-ttu-id="dbf61-116">Nyissa meg a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="dbf61-116">Open the sample application</span></span>
<span data-ttu-id="dbf61-117">A mintaalkalmazás megnyitásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dbf61-117">To open the sample application, follow these steps:</span></span>

1. <span data-ttu-id="dbf61-118">A Githubból a minta-tárház klónozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="dbf61-118">Clone the sample repository from GitHub by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-edison-getting-started.git
   ```
2. <span data-ttu-id="dbf61-119">Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="dbf61-119">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd iot-hub-node-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Tárház szerkezete][repo-structure]

<span data-ttu-id="dbf61-121">A fájl a `app` almappa is szabályozhatja a LED kódot tartalmazó kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="dbf61-121">The file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="dbf61-122">Telepítse az alkalmazásfüggőségek</span><span class="sxs-lookup"><span data-stu-id="dbf61-122">Install application dependencies</span></span>
<span data-ttu-id="dbf61-123">A szalagtárak és egyéb modulok a következő parancs futtatásával kell a mintaalkalmazás telepítése:</span><span class="sxs-lookup"><span data-stu-id="dbf61-123">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="dbf61-124">Az eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dbf61-124">Configure the device connection</span></span>
<span data-ttu-id="dbf61-125">Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dbf61-125">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="dbf61-126">Az eszköz konfigurációs fájl létrehozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="dbf61-126">Generate the device configuration file by running the following command:</span></span>

   ```bash
   gulp init
   ```

   <span data-ttu-id="dbf61-127">A konfigurációs fájl `config-edison.json` felhasználói hitelesítő adatokkal kell bejelentkezni Edison tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dbf61-127">The configuration file `config-edison.json` contains the user credentials you use to log in to Edison.</span></span> <span data-ttu-id="dbf61-128">A felhasználói hitelesítő adatok memóriavesztés elkerülése érdekében a konfigurációs fájl jön létre, almappájában `.iot-hub-getting-started` az otthoni mappa a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="dbf61-128">To avoid the leak of user credentials, the configuration file is generated in the subfolder `.iot-hub-getting-started` of the home folder on your computer.</span></span>

2. <span data-ttu-id="dbf61-129">Nyissa meg a Visual Studio Code eszköz konfigurációs fájl a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="dbf61-129">Open the device configuration file in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. <span data-ttu-id="dbf61-130">Cserélje le a helyőrző `[device hostname or IP address]` és `[device password]` a IP-címét és jelszavát, amely korábbi lecke jelölésű.</span><span class="sxs-lookup"><span data-stu-id="dbf61-130">Replace the placeholder `[device hostname or IP address]` and `[device password]` with the IP address and password that you marked down in previous lesson.</span></span>

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

<span data-ttu-id="dbf61-132">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="dbf61-132">Congratulations!</span></span> <span data-ttu-id="dbf61-133">Sikeresen létrehozta a Edison első mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dbf61-133">You've successfully created the first sample application for Edison.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="dbf61-134">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="dbf61-134">Deploy and run the sample application</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="dbf61-135">Központi telepítése és a mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="dbf61-135">Deploy and run the sample app</span></span>
<span data-ttu-id="dbf61-136">Telepíthet, és futtassa a mintaalkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="dbf61-136">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a><span data-ttu-id="dbf61-137">Ellenőrizze az alkalmazás akkor működik</span><span class="sxs-lookup"><span data-stu-id="dbf61-137">Verify the app works</span></span>
<span data-ttu-id="dbf61-138">A mintaalkalmazás automatikusan leáll, miután a LED villogjon-20 alkalommal a.</span><span class="sxs-lookup"><span data-stu-id="dbf61-138">The sample application terminates automatically after the LED blinks for 20 times.</span></span> <span data-ttu-id="dbf61-139">Ha nem látja a LED villogó, tekintse meg a [hibaelhárítási útmutatója] [ troubleshooting] gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="dbf61-139">If you don’t see the LED blinking, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

![LED villogó][led-blinking]

## <a name="summary"></a><span data-ttu-id="dbf61-141">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="dbf61-141">Summary</span></span>
<span data-ttu-id="dbf61-142">A szükséges eszközök Edison használható telepítése és a LED villogni fog Edison a mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="dbf61-142">You've installed the required tools to work with Edison and deployed a sample application to Edison to blink the LED.</span></span> <span data-ttu-id="dbf61-143">Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti Edison Azure IoT Hub az üzeneteket küldjön és fogadjon.</span><span class="sxs-lookup"><span data-stu-id="dbf61-143">You can now create, deploy, and run another sample application that connects Edison to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dbf61-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dbf61-144">Next steps</span></span>
<span data-ttu-id="dbf61-145">[Az Azure eszközök][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="dbf61-145">[Get the Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md

---
title: "Intel Edison (csomópont) tooAzure IoT - lecke 1 Kapcsolódás: alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza a C mintaalkalmazás hello a Githubból, és futtassa gulp toodeploy az alkalmazás tooyour Intel Edison board. A mintaalkalmazás villogjon hello csatlakoztatott LED toohello board két másodpercenként."
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
ms.openlocfilehash: bc03c7e45bd1ba9e9b2c8f2fec70a1be647e96b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="2ac83-105">Hello villogási alkalmazás létrehozását és telepítését</span><span class="sxs-lookup"><span data-stu-id="2ac83-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="2ac83-106">Mit fog</span><span class="sxs-lookup"><span data-stu-id="2ac83-106">What you will do</span></span>
<span data-ttu-id="2ac83-107">Klónozza a C mintaalkalmazás hello a Githubból, és hello gulp eszköz toodeploy hello minta alkalmazás tooIntel Edison használja.</span><span class="sxs-lookup"><span data-stu-id="2ac83-107">Clone hello sample C application from GitHub, and use hello gulp tool toodeploy hello sample application tooIntel Edison.</span></span> <span data-ttu-id="2ac83-108">hello mintaalkalmazás villogjon hello csatlakoztatott LED toohello board két másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="2ac83-108">hello sample application blinks hello LED connected toohello board every two seconds.</span></span> <span data-ttu-id="2ac83-109">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="2ac83-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="2ac83-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="2ac83-110">What you will learn</span></span>
* <span data-ttu-id="2ac83-111">Hogyan toodeploy és futtatási hello mintaalkalmazást a Edison.</span><span class="sxs-lookup"><span data-stu-id="2ac83-111">How toodeploy and run hello sample application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2ac83-112">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="2ac83-112">What you need</span></span>
<span data-ttu-id="2ac83-113">Sikeresen végrehajtotta a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="2ac83-113">You must have successfully completed hello following operations:</span></span>

* <span data-ttu-id="2ac83-114">[Állítsa be az eszközt][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="2ac83-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="2ac83-115">[Hello eszközök beszerzése][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="2ac83-115">[Get hello tools][get-the-tools]</span></span>

## <a name="open-hello-sample-application"></a><span data-ttu-id="2ac83-116">Nyissa meg hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2ac83-116">Open hello sample application</span></span>
<span data-ttu-id="2ac83-117">tooopen hello mintaalkalmazást, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2ac83-117">tooopen hello sample application, follow these steps:</span></span>

1. <span data-ttu-id="2ac83-118">A Githubból hello minta tárház klónozása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2ac83-118">Clone hello sample repository from GitHub by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-edison-getting-started.git
   ```
2. <span data-ttu-id="2ac83-119">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2ac83-119">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd iot-hub-node-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Tárház szerkezete][repo-structure]

<span data-ttu-id="2ac83-121">hello hello fájlban `app` almappa is hello kód toocontrol hello LED tartalmazó hello kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="2ac83-121">hello file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="2ac83-122">Telepítse az alkalmazásfüggőségek</span><span class="sxs-lookup"><span data-stu-id="2ac83-122">Install application dependencies</span></span>
<span data-ttu-id="2ac83-123">Hello szalagtárak és más modulok hello a következő parancs futtatásával kell hello mintaalkalmazás telepítése:</span><span class="sxs-lookup"><span data-stu-id="2ac83-123">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="2ac83-124">Hello eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2ac83-124">Configure hello device connection</span></span>
<span data-ttu-id="2ac83-125">tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2ac83-125">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="2ac83-126">Hello eszköz konfigurációs fájl létrehozása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2ac83-126">Generate hello device configuration file by running hello following command:</span></span>

   ```bash
   gulp init
   ```

   <span data-ttu-id="2ac83-127">hello konfigurációs fájl `config-edison.json` hello segítségével toolog tooEdison felhasználói hitelesítő adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2ac83-127">hello configuration file `config-edison.json` contains hello user credentials you use toolog in tooEdison.</span></span> <span data-ttu-id="2ac83-128">tooavoid hello szivárgásával járnak a felhasználói hitelesítő adatok, hello konfigurációs fájl jön létre hello almappájában `.iot-hub-getting-started` hello otthoni mappa a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2ac83-128">tooavoid hello leak of user credentials, hello configuration file is generated in hello subfolder `.iot-hub-getting-started` of hello home folder on your computer.</span></span>

2. <span data-ttu-id="2ac83-129">Nyissa meg a Visual Studio Code hello eszköz konfigurációs fájl hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2ac83-129">Open hello device configuration file in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. <span data-ttu-id="2ac83-130">Cserélje le a hello helyőrző `[device hostname or IP address]` és `[device password]` hello IP-címét és jelszavát, amely korábbi lecke jelölésű.</span><span class="sxs-lookup"><span data-stu-id="2ac83-130">Replace hello placeholder `[device hostname or IP address]` and `[device password]` with hello IP address and password that you marked down in previous lesson.</span></span>

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

<span data-ttu-id="2ac83-132">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="2ac83-132">Congratulations!</span></span> <span data-ttu-id="2ac83-133">Sikeresen létrehozta a hello első mintaalkalmazás Edison.</span><span class="sxs-lookup"><span data-stu-id="2ac83-133">You've successfully created hello first sample application for Edison.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="2ac83-134">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2ac83-134">Deploy and run hello sample application</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="2ac83-135">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2ac83-135">Deploy and run hello sample app</span></span>
<span data-ttu-id="2ac83-136">Telepíthet, és futtassa a hello mintaalkalmazást hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2ac83-136">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="2ac83-137">Ellenőrizze a hello az alkalmazás akkor működik</span><span class="sxs-lookup"><span data-stu-id="2ac83-137">Verify hello app works</span></span>
<span data-ttu-id="2ac83-138">hello mintaalkalmazás automatikusan leáll, miután hello LED villogjon-20 alkalommal a.</span><span class="sxs-lookup"><span data-stu-id="2ac83-138">hello sample application terminates automatically after hello LED blinks for 20 times.</span></span> <span data-ttu-id="2ac83-139">Ha nem lát hello LED villogó, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="2ac83-139">If you don’t see hello LED blinking, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

![LED villogó][led-blinking]

## <a name="summary"></a><span data-ttu-id="2ac83-141">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="2ac83-141">Summary</span></span>
<span data-ttu-id="2ac83-142">A Edison hello szükséges eszközök toowork telepítése és telepített egy minta alkalmazás tooEdison tooblink hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="2ac83-142">You've installed hello required tools toowork with Edison and deployed a sample application tooEdison tooblink hello LED.</span></span> <span data-ttu-id="2ac83-143">Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti az IoT-központ toosend Edison tooAzure és üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="2ac83-143">You can now create, deploy, and run another sample application that connects Edison tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2ac83-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2ac83-144">Next steps</span></span>
<span data-ttu-id="2ac83-145">[Első hello Azure-eszközök][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="2ac83-145">[Get hello Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md

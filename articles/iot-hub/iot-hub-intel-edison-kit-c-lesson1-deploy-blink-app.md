---
title: "Connect Intel Edison (C) tooAzure IoT - lecke 1: alkalmazás központi telepítése |} Microsoft Docs"
description: "Klónozza a C mintaalkalmazás hello a Githubból, és futtassa gulp toodeploy az alkalmazás tooyour Intel Edison board. A mintaalkalmazás villogjon hello csatlakoztatott LED toohello board két másodpercenként."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vezetett projektek, arduino vezetett villogási, vezetett arduino villogási arduino villogási program, a arduino villogási – példa"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: b02dfd3f-28fd-4b52-8775-eb0eaf74d707
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: fa84fae812dd742a2ad4997a5e213c8e40e6fcf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="4382e-105">Hello villogási alkalmazás létrehozását és telepítését</span><span class="sxs-lookup"><span data-stu-id="4382e-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="4382e-106">Mit fog</span><span class="sxs-lookup"><span data-stu-id="4382e-106">What you will do</span></span>
<span data-ttu-id="4382e-107">Klónozza a C mintaalkalmazás hello a Githubból, és hello gulp eszköz toodeploy hello minta alkalmazás tooIntel Edison használja.</span><span class="sxs-lookup"><span data-stu-id="4382e-107">Clone hello sample C application from GitHub, and use hello gulp tool toodeploy hello sample application tooIntel Edison.</span></span> <span data-ttu-id="4382e-108">hello mintaalkalmazás villogjon hello csatlakoztatott LED toohello board két másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="4382e-108">hello sample application blinks hello LED connected toohello board every two seconds.</span></span> <span data-ttu-id="4382e-109">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="4382e-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4382e-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="4382e-110">What you will learn</span></span>
* <span data-ttu-id="4382e-111">Hogyan toodeploy és futtatási hello mintaalkalmazást a Edison.</span><span class="sxs-lookup"><span data-stu-id="4382e-111">How toodeploy and run hello sample application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4382e-112">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="4382e-112">What you need</span></span>
<span data-ttu-id="4382e-113">Sikeresen végrehajtotta a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="4382e-113">You must have successfully completed hello following operations:</span></span>

* <span data-ttu-id="4382e-114">[Állítsa be az eszközt][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="4382e-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="4382e-115">[Hello eszközök beszerzése][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="4382e-115">[Get hello tools][get-the-tools]</span></span>

## <a name="open-hello-sample-application"></a><span data-ttu-id="4382e-116">Nyissa meg hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="4382e-116">Open hello sample application</span></span>
<span data-ttu-id="4382e-117">tooopen hello mintaalkalmazást, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4382e-117">tooopen hello sample application, follow these steps:</span></span>

1. <span data-ttu-id="4382e-118">A Githubból hello minta tárház klónozása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4382e-118">Clone hello sample repository from GitHub by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-edison-getting-started.git
   ```
2. <span data-ttu-id="4382e-119">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4382e-119">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd iot-hub-c-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Tárház szerkezete][repo-structure]

<span data-ttu-id="4382e-121">hello hello fájlban `app` almappa is hello kód toocontrol hello LED tartalmazó hello kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="4382e-121">hello file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="4382e-122">Telepítse az alkalmazásfüggőségek</span><span class="sxs-lookup"><span data-stu-id="4382e-122">Install application dependencies</span></span>
<span data-ttu-id="4382e-123">Hello szalagtárak és más modulok hello a következő parancs futtatásával kell hello mintaalkalmazás telepítése:</span><span class="sxs-lookup"><span data-stu-id="4382e-123">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="4382e-124">Hello eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4382e-124">Configure hello device connection</span></span>
<span data-ttu-id="4382e-125">tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4382e-125">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="4382e-126">Hello eszköz konfigurációs fájl létrehozása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4382e-126">Generate hello device configuration file by running hello following command:</span></span>

   ```bash
   gulp init
   ```

   <span data-ttu-id="4382e-127">hello konfigurációs fájl `config-edison.json` hello segítségével toolog tooEdison felhasználói hitelesítő adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4382e-127">hello configuration file `config-edison.json` contains hello user credentials you use toolog in tooEdison.</span></span> <span data-ttu-id="4382e-128">tooavoid hello szivárgásával járnak a felhasználói hitelesítő adatok, hello konfigurációs fájl jön létre hello almappájában `.iot-hub-getting-started` hello otthoni mappa a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4382e-128">tooavoid hello leak of user credentials, hello configuration file is generated in hello subfolder `.iot-hub-getting-started` of hello home folder on your computer.</span></span>

2. <span data-ttu-id="4382e-129">Nyissa meg a Visual Studio Code hello eszköz konfigurációs fájl hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4382e-129">Open hello device configuration file in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. <span data-ttu-id="4382e-130">Cserélje le a hello helyőrző `[device hostname or IP address]` és `[device password]` hello IP-címét és jelszavát, amely korábbi lecke jelölésű.</span><span class="sxs-lookup"><span data-stu-id="4382e-130">Replace hello placeholder `[device hostname or IP address]` and `[device password]` with hello IP address and password that you marked down in previous lesson.</span></span>

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

<span data-ttu-id="4382e-132">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="4382e-132">Congratulations!</span></span> <span data-ttu-id="4382e-133">Sikeresen létrehozta a hello első mintaalkalmazás Edison.</span><span class="sxs-lookup"><span data-stu-id="4382e-133">You've successfully created hello first sample application for Edison.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="4382e-134">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="4382e-134">Deploy and run hello sample application</span></span>
### <a name="install-hello-azure-iot-hub-sdk-on-edison"></a><span data-ttu-id="4382e-135">Edison hello Azure IoT Hub SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="4382e-135">Install hello Azure IoT Hub SDK on Edison</span></span>
<span data-ttu-id="4382e-136">Hello Azure IoT Hub SDK telepítése Edison hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4382e-136">Install hello Azure IoT Hub SDK on Edison by running hello following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="4382e-137">Ez a feladat egy hosszú ideig toocomplete, attól függően, hogy a hálózati kapcsolatot is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="4382e-137">This task might take a long time toocomplete, depending on your network connection.</span></span> <span data-ttu-id="4382e-138">Az igények toobe csak egyszeri futtatás egy Edison.</span><span class="sxs-lookup"><span data-stu-id="4382e-138">It needs toobe run only once for one Edison.</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="4382e-139">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="4382e-139">Deploy and run hello sample app</span></span>
<span data-ttu-id="4382e-140">Telepíthet, és futtassa a hello mintaalkalmazást hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4382e-140">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="4382e-141">Ellenőrizze a hello az alkalmazás akkor működik</span><span class="sxs-lookup"><span data-stu-id="4382e-141">Verify hello app works</span></span>
<span data-ttu-id="4382e-142">hello mintaalkalmazás automatikusan leáll, miután hello LED villogjon-20 alkalommal a.</span><span class="sxs-lookup"><span data-stu-id="4382e-142">hello sample application terminates automatically after hello LED blinks for 20 times.</span></span> <span data-ttu-id="4382e-143">Ha nem lát hello LED villogó, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="4382e-143">If you don’t see hello LED blinking, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

![LED villogó][led-blinking]

## <a name="summary"></a><span data-ttu-id="4382e-145">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="4382e-145">Summary</span></span>
<span data-ttu-id="4382e-146">A Edison hello szükséges eszközök toowork telepítése és telepített egy minta alkalmazás tooEdison tooblink hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="4382e-146">You've installed hello required tools toowork with Edison and deployed a sample application tooEdison tooblink hello LED.</span></span> <span data-ttu-id="4382e-147">Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti az IoT-központ toosend Edison tooAzure és üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="4382e-147">You can now create, deploy, and run another sample application that connects Edison tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4382e-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4382e-148">Next steps</span></span>
<span data-ttu-id="4382e-149">[Első hello Azure-eszközök][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="4382e-149">[Get hello Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure_c.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking_c.jpg
[get-the-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md

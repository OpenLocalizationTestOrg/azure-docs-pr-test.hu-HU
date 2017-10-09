---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 3: futtassa a mintaalkalmazást |} Microsoft Docs"
description: "A szimulált eszköz sample app toosend hőmérséklet adatok tooyour IoT-központ futtatása"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: adatok toocloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5d051d99-9749-4150-b3c8-573b0bda9c52
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: bc2c97919e95e4e3977a8b6ac75162bf2b5017be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a><span data-ttu-id="a3373-104">Konfigurálja és a szimulált eszköz mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a3373-104">Configure and run a simulated device sample app</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="a3373-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a3373-105">What you will do</span></span>

- <span data-ttu-id="a3373-106">Klónozás hello minta tárházba.</span><span class="sxs-lookup"><span data-stu-id="a3373-106">Clone hello sample repository.</span></span>
- <span data-ttu-id="a3373-107">A hello Azure CLI tooget az IoT-központ és a logikai eszköz információk szimulált eszköz mintaalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="a3373-107">Use hello Azure CLI tooget your IoT hub and logical device information for simulated device sample application.</span></span> <span data-ttu-id="a3373-108">Konfigurálja, és futtassa a hello szimulált eszköz mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a3373-108">Configure and run hello simulated device sample application.</span></span>

<span data-ttu-id="a3373-109">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a3373-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a3373-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a3373-110">What you will learn</span></span>

<span data-ttu-id="a3373-111">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="a3373-111">In this article, you will learn:</span></span>

- <span data-ttu-id="a3373-112">Hogyan tooconfigure és futtatási hello szimulált eszköz mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a3373-112">How tooconfigure and run hello simulated device sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a3373-113">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a3373-113">What you need</span></span>

<span data-ttu-id="a3373-114">Sikeresen végrehajtotta</span><span class="sxs-lookup"><span data-stu-id="a3373-114">You must have successfully completed</span></span>

- [<span data-ttu-id="a3373-115">IoT Hub létrehozása és az eszköz regisztrálása</span><span class="sxs-lookup"><span data-stu-id="a3373-115">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a><span data-ttu-id="a3373-116">Klónozás hello minta tárház toohello gazdaszámítógépen</span><span class="sxs-lookup"><span data-stu-id="a3373-116">Clone hello sample repository toohello host computer</span></span>

<span data-ttu-id="a3373-117">tooclone hello minta tárház, kövesse az alábbi lépéseket hello állomáson:</span><span class="sxs-lookup"><span data-stu-id="a3373-117">tooclone hello sample repository, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="a3373-118">Nyisson meg egy parancssort a Windows, vagy nyisson meg egy terminál macOS vagy Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="a3373-118">Open a Command Prompt in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="a3373-119">Futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="a3373-119">Run hello following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-hello-simulated-device-and-your-nuc"></a><span data-ttu-id="a3373-120">Hello szimulált eszköz és a NUC konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3373-120">Configure hello simulated device and your NUC</span></span>

1. <span data-ttu-id="a3373-121">Nyissa meg hello konfigurációs fájl `config.json` a Visual Studio Code hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3373-121">Open hello configuration file `config.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   code config.json
   ```

2. <span data-ttu-id="a3373-122">Cserélje le `"has_sensortag": true` a`"has_sensortag": false`</span><span class="sxs-lookup"><span data-stu-id="a3373-122">Replace `"has_sensortag": true` with `"has_sensortag": false`</span></span>

   ![Nem rendelkezik TI SensorTag eszközzel config](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. <span data-ttu-id="a3373-124">Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3373-124">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. <span data-ttu-id="a3373-125">Nyissa meg `config-gateway.json` a Visual Studio Code hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3373-125">Open `config-gateway.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. <span data-ttu-id="a3373-126">Keresse meg a következő kódsort hello, és cserélje le `[device hostname or IP address]` az IP-címét vagy állomásnevét kiszolgálónevét hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="a3373-126">Locate hello following line of code and replace `[device hostname or IP address]` with IP address or host name of hello Intel NUC.</span></span>
   <span data-ttu-id="a3373-127">![Képernyőkép a config átjáró](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="a3373-127">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

## <a name="get-hello-connection-string-of-your-iot-hub-logical-device"></a><span data-ttu-id="a3373-128">Az IoT hub logikai eszköz hello kapcsolati karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="a3373-128">Get hello connection string of your IoT hub logical device</span></span>

<span data-ttu-id="a3373-129">tooget hello Azure IoT hub kapcsolati karakterláncot a logikai eszköz, futtassa a következő parancsot a hello gazdaszámítógépen hello:</span><span class="sxs-lookup"><span data-stu-id="a3373-129">tooget hello Azure IoT hub connection string of your logical device, run hello following command on hello host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="a3373-130">`{IoT hub name}`hello IoT-központnév használt van.</span><span class="sxs-lookup"><span data-stu-id="a3373-130">`{IoT hub name}` is hello IoT hub name that you used.</span></span> <span data-ttu-id="a3373-131">Az iot-átjáró használatához hello értékeként `{resource group name}` és mydevice hello értékeként `{device id}` nem módosítása hello érték a 2.</span><span class="sxs-lookup"><span data-stu-id="a3373-131">Use iot-gateway as hello value of `{resource group name}` and use mydevice as hello value of `{device id}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="configure-hello-simulated-device-cloud-upload-sample-application"></a><span data-ttu-id="a3373-132">Hello szimulált eszköz felhő feltöltés mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3373-132">Configure hello simulated device cloud upload sample application</span></span>

<span data-ttu-id="a3373-133">tooconfigure és szimulált futási hello eszköz felhő töltse fel a mintaalkalmazást, kövesse az alábbi lépéseket hello állomáson:</span><span class="sxs-lookup"><span data-stu-id="a3373-133">tooconfigure and run hello simulated device cloud upload sample application, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="a3373-134">Nyissa meg `config-sensortag.json` a Visual Studio Code hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3373-134">Open `config-sensortag.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![config sensortag képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. <span data-ttu-id="a3373-136">Hajtsa végre a következő hello kód csere hello:</span><span class="sxs-lookup"><span data-stu-id="a3373-136">Make hello following replacements in hello code:</span></span>
   - <span data-ttu-id="a3373-137">Cserélje le `[IoT hub name]` hello IoT hub névvel.</span><span class="sxs-lookup"><span data-stu-id="a3373-137">Replace `[IoT hub name]` with hello IoT hub name.</span></span>
   - <span data-ttu-id="a3373-138">Cserélje le `[IoT device connection string]` az IoT hub logikai eszköz hello kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="a3373-138">Replace `[IoT device connection string]` with hello connection string of your IoT hub logical device.</span></span>

3. <span data-ttu-id="a3373-139">Hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a3373-139">Run hello application.</span></span>

   <span data-ttu-id="a3373-140">Központi telepítése, és futtassa a hello alkalmazást hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3373-140">Deploy and run hello application by running hello following command:</span></span>

   ```bash
   gulp run
   ```

## <a name="verify-hello-sample-application-works"></a><span data-ttu-id="a3373-141">Hello minta alkalmazás works ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a3373-141">Verify hello sample application works</span></span>

<span data-ttu-id="a3373-142">Mostantól a következő hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a3373-142">You should now see output like hello following:</span></span>

![alkalmazás szimulált eszköz Példakimenet.](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

<span data-ttu-id="a3373-144">hello alkalmazás küld hőmérséklet adatok tooyour IoT-központ, amely 40 másodpercig tart.</span><span class="sxs-lookup"><span data-stu-id="a3373-144">hello application sends temperature data tooyour IoT hub, which lasts for 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="a3373-145">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a3373-145">Summary</span></span>

<span data-ttu-id="a3373-146">Sikeresen konfigurálta, és futtassa hello szimulált eszköz felhő feltöltés mintaalkalmazás el, amely adatok tooyour IoT-központ a szimulált eszköz.</span><span class="sxs-lookup"><span data-stu-id="a3373-146">You've successfully configured and run hello simulated device cloud upload sample application which sends data tooyour IoT hub with simulated device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3373-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3373-147">Next steps</span></span>
[<span data-ttu-id="a3373-148">Üzenetek olvasása az IoT Hubról</span><span class="sxs-lookup"><span data-stu-id="a3373-148">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)
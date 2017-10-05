---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 3: futtassa a mintaalkalmazást |} Microsoft Docs"
description: "Hőmérséklet-adatok küldését az IoT hub a szimulált eszköz mintaalkalmazás futtatása"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok felhőbe"
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
ms.openlocfilehash: 7df2d730c38a9f715e0fd57b4d436724a5727760
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a><span data-ttu-id="a3dff-104">Konfigurálja és a szimulált eszköz mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a3dff-104">Configure and run a simulated device sample app</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="a3dff-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a3dff-105">What you will do</span></span>

- <span data-ttu-id="a3dff-106">A minta-tárház klónozása.</span><span class="sxs-lookup"><span data-stu-id="a3dff-106">Clone the sample repository.</span></span>
- <span data-ttu-id="a3dff-107">Az Azure parancssori felület használatával az IoT-központ és logikai eszköz szimulált eszköz mintaalkalmazás vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="a3dff-107">Use the Azure CLI to get your IoT hub and logical device information for simulated device sample application.</span></span> <span data-ttu-id="a3dff-108">Konfigurálja, és futtassa a szimulált eszköz mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a3dff-108">Configure and run the simulated device sample application.</span></span>

<span data-ttu-id="a3dff-109">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a3dff-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a3dff-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a3dff-110">What you will learn</span></span>

<span data-ttu-id="a3dff-111">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="a3dff-111">In this article, you will learn:</span></span>

- <span data-ttu-id="a3dff-112">Hogyan lehet konfigurálni, és futtassa a szimulált eszköz mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a3dff-112">How to configure and run the simulated device sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a3dff-113">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a3dff-113">What you need</span></span>

<span data-ttu-id="a3dff-114">Sikeresen végrehajtotta</span><span class="sxs-lookup"><span data-stu-id="a3dff-114">You must have successfully completed</span></span>

- [<span data-ttu-id="a3dff-115">IoT Hub létrehozása és az eszköz regisztrálása</span><span class="sxs-lookup"><span data-stu-id="a3dff-115">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a><span data-ttu-id="a3dff-116">A gazdagépnek a minta-tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="a3dff-116">Clone the sample repository to the host computer</span></span>

<span data-ttu-id="a3dff-117">A minta-tárház klónozása, kövesse az alábbi lépéseket az állomáson:</span><span class="sxs-lookup"><span data-stu-id="a3dff-117">To clone the sample repository, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="a3dff-118">Nyisson meg egy parancssort a Windows, vagy nyisson meg egy terminál macOS vagy Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="a3dff-118">Open a Command Prompt in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="a3dff-119">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="a3dff-119">Run the following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-the-simulated-device-and-your-nuc"></a><span data-ttu-id="a3dff-120">A szimulált eszköz és a NUC konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3dff-120">Configure the simulated device and your NUC</span></span>

1. <span data-ttu-id="a3dff-121">Nyissa meg a konfigurációs fájl `config.json` a Visual Studio Code a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3dff-121">Open the configuration file `config.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   code config.json
   ```

2. <span data-ttu-id="a3dff-122">Cserélje le `"has_sensortag": true` a`"has_sensortag": false`</span><span class="sxs-lookup"><span data-stu-id="a3dff-122">Replace `"has_sensortag": true` with `"has_sensortag": false`</span></span>

   ![Nem rendelkezik TI SensorTag eszközzel config](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. <span data-ttu-id="a3dff-124">A konfigurációs fájl inicializálása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3dff-124">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. <span data-ttu-id="a3dff-125">Nyissa meg `config-gateway.json` a Visual Studio Code a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3dff-125">Open `config-gateway.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. <span data-ttu-id="a3dff-126">Keresse meg a következő kódsort, és cserélje le `[device hostname or IP address]` az IP-címét vagy állomásnevét kiszolgálónevét az Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="a3dff-126">Locate the following line of code and replace `[device hostname or IP address]` with IP address or host name of the Intel NUC.</span></span>
   <span data-ttu-id="a3dff-127">![Képernyőkép a config átjáró](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="a3dff-127">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

## <a name="get-the-connection-string-of-your-iot-hub-logical-device"></a><span data-ttu-id="a3dff-128">A kapcsolati karakterlánc az IoT hub logikai eszköz beolvasása</span><span class="sxs-lookup"><span data-stu-id="a3dff-128">Get the connection string of your IoT hub logical device</span></span>

<span data-ttu-id="a3dff-129">Ahhoz, hogy az Azure IoT hub kapcsolati karakterlánca a logikai eszköz, a következő parancsot az állomáson:</span><span class="sxs-lookup"><span data-stu-id="a3dff-129">To get the Azure IoT hub connection string of your logical device, run the following command on the host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="a3dff-130">`{IoT hub name}`az IoT-központnév használt van.</span><span class="sxs-lookup"><span data-stu-id="a3dff-130">`{IoT hub name}` is the IoT hub name that you used.</span></span> <span data-ttu-id="a3dff-131">Az iot-átjáró használatához értékeként `{resource group name}` mydevice használják, értékének `{device id}` Ha nem módosítja az érték a 2.</span><span class="sxs-lookup"><span data-stu-id="a3dff-131">Use iot-gateway as the value of `{resource group name}` and use mydevice as the value of `{device id}` if you didn't change the value in Lesson 2.</span></span>

## <a name="configure-the-simulated-device-cloud-upload-sample-application"></a><span data-ttu-id="a3dff-132">A szimulált eszköz felhő feltöltés mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3dff-132">Configure the simulated device cloud upload sample application</span></span>

<span data-ttu-id="a3dff-133">Konfigurálja, és futtassa a szimulált eszköz felhő feltöltés mintaalkalmazást, kövesse az alábbi lépéseket az állomáson:</span><span class="sxs-lookup"><span data-stu-id="a3dff-133">To configure and run the simulated device cloud upload sample application, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="a3dff-134">Nyissa meg `config-sensortag.json` a Visual Studio Code a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3dff-134">Open `config-sensortag.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![config sensortag képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. <span data-ttu-id="a3dff-136">Végezze el az alábbi új kódot:</span><span class="sxs-lookup"><span data-stu-id="a3dff-136">Make the following replacements in the code:</span></span>
   - <span data-ttu-id="a3dff-137">Cserélje le `[IoT hub name]` az IoT hub nevét.</span><span class="sxs-lookup"><span data-stu-id="a3dff-137">Replace `[IoT hub name]` with the IoT hub name.</span></span>
   - <span data-ttu-id="a3dff-138">Cserélje le `[IoT device connection string]` az IoT hub logikai eszköz kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="a3dff-138">Replace `[IoT device connection string]` with the connection string of your IoT hub logical device.</span></span>

3. <span data-ttu-id="a3dff-139">Futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a3dff-139">Run the application.</span></span>

   <span data-ttu-id="a3dff-140">Telepíthet, és futtassa az alkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3dff-140">Deploy and run the application by running the following command:</span></span>

   ```bash
   gulp run
   ```

## <a name="verify-the-sample-application-works"></a><span data-ttu-id="a3dff-141">Ellenőrizze a minta alkalmazás működik</span><span class="sxs-lookup"><span data-stu-id="a3dff-141">Verify the sample application works</span></span>

<span data-ttu-id="a3dff-142">Most látnia kell a következőhöz hasonló kimenetnek:</span><span class="sxs-lookup"><span data-stu-id="a3dff-142">You should now see output like the following:</span></span>

![alkalmazás szimulált eszköz Példakimenet.](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

<span data-ttu-id="a3dff-144">Az alkalmazás hőmérséklet adatokat küld az IoT hub, amely 40 másodpercig tart.</span><span class="sxs-lookup"><span data-stu-id="a3dff-144">The application sends temperature data to your IoT hub, which lasts for 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="a3dff-145">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a3dff-145">Summary</span></span>

<span data-ttu-id="a3dff-146">Sikeresen konfigurálását és futtassa a szimulált eszköz felhő feltöltés mintaalkalmazást, amelyek adatokat küld az IoT hub szimulált eszköz.</span><span class="sxs-lookup"><span data-stu-id="a3dff-146">You've successfully configured and run the simulated device cloud upload sample application which sends data to your IoT hub with simulated device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3dff-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3dff-147">Next steps</span></span>
[<span data-ttu-id="a3dff-148">Üzenetek olvasása az IoT Hubról</span><span class="sxs-lookup"><span data-stu-id="a3dff-148">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)
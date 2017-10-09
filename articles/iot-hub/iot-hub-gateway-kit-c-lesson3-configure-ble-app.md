---
title: "SensorTag eszköz & Azure IoT átjáró - rész 3: futtassa a mintaalkalmazást |} Microsoft Docs"
description: "Egy táblázat alkalmazás tooreceive mintaadatok BLA SensorTag és az IoT hub fut."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "bla app, érzékelő alkalmazás monitorozása, érzékelő adatok gyűjtését, érzékelőket, érzékelő adatokat toocloud adatait"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: b33e53a1-1df7-4412-ade1-45185aec5bef
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4a8acdeadd402ffc82d3b766e1ec03a77ddcebb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a><span data-ttu-id="4228b-104">Konfigurálja és BLA mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="4228b-104">Configure and run a BLE sample application</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="4228b-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="4228b-105">What you will do</span></span>

- <span data-ttu-id="4228b-106">Klónozás hello minta tárházba.</span><span class="sxs-lookup"><span data-stu-id="4228b-106">Clone hello sample repository.</span></span> 
- <span data-ttu-id="4228b-107">Összekapcsolhatók hello SensorTag és Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4228b-107">Set up hello connectivity between SensorTag and Intel NUC.</span></span> 
- <span data-ttu-id="4228b-108">A hello Azure CLI tooget az IoT-központ és a SensorTag információk egy BLA (Bluetooth alacsony energia) mintaalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4228b-108">Use hello Azure CLI tooget your IoT hub and SensorTag information for a BLE(Bluetooth Low Energy) sample application.</span></span> <span data-ttu-id="4228b-109">És konfigurálása, valamint futtassa hello BLA mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4228b-109">And configure and run hello BLE sample application.</span></span> 

<span data-ttu-id="4228b-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="4228b-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4228b-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="4228b-111">What you will learn</span></span>

<span data-ttu-id="4228b-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="4228b-112">In this article, you will learn:</span></span>

- <span data-ttu-id="4228b-113">Hogyan tooconfigure és futtatási hello BLA mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4228b-113">How tooconfigure and run hello BLE sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4228b-114">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="4228b-114">What you need</span></span>

<span data-ttu-id="4228b-115">Sikeresen végrehajtotta</span><span class="sxs-lookup"><span data-stu-id="4228b-115">You must have successfully completed</span></span>

- [<span data-ttu-id="4228b-116">Létrehoz egy IoT-központot, és regisztrálja a SensorTag</span><span class="sxs-lookup"><span data-stu-id="4228b-116">Create an IoT hub and register SensorTag</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a><span data-ttu-id="4228b-117">Klónozás hello minta tárház toohello gazdaszámítógépen</span><span class="sxs-lookup"><span data-stu-id="4228b-117">Clone hello sample repository toohello host computer</span></span>

<span data-ttu-id="4228b-118">tooclone hello minta tárház, kövesse az alábbi lépéseket hello állomáson:</span><span class="sxs-lookup"><span data-stu-id="4228b-118">tooclone hello sample repository, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="4228b-119">Nyisson meg egy parancssort a Windows, vagy nyisson meg egy terminál macOS vagy Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="4228b-119">Open a Command Prompt window in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="4228b-120">Futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="4228b-120">Run hello following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-hello-connectivity-between-sensortag-and-intel-nuc"></a><span data-ttu-id="4228b-121">Összekapcsolhatók hello SensorTag és Intel NUC</span><span class="sxs-lookup"><span data-stu-id="4228b-121">Set up hello connectivity between SensorTag and Intel NUC</span></span>

<span data-ttu-id="4228b-122">tooset mentése hello kapcsolatot, kövesse az alábbi lépéseket hello állomáson:</span><span class="sxs-lookup"><span data-stu-id="4228b-122">tooset up hello connectivity, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="4228b-123">Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4228b-123">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. <span data-ttu-id="4228b-124">Nyissa meg `config-gateway.json` a Visual Studio Code hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4228b-124">Open `config-gateway.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. <span data-ttu-id="4228b-125">Keresse meg a következő kódsort hello, és cserélje le `[device hostname or IP address]` hello IP cím vagy állomásnév Intel NUC annak nevét.</span><span class="sxs-lookup"><span data-stu-id="4228b-125">Locate hello following line of code and replace `[device hostname or IP address]` with hello IP address or host name of Intel NUC.</span></span>
   <span data-ttu-id="4228b-126">![Képernyőkép a config átjáró](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="4228b-126">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

4. <span data-ttu-id="4228b-127">Intel NUC segítő eszközök telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4228b-127">Install helper tools on Intel NUC by running hello following command:</span></span>

   ```bash
   gulp install-tools
   ```

5. <span data-ttu-id="4228b-128">Kapcsolja be a SensorTag kép a következő hello és zöld LED kell villogni hello hello főkapcsoló lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="4228b-128">Turn on SensorTag by pressing hello power button as hello following picture, and hello green LED should blink.</span></span>

   ![Kapcsolja be a Sensor Tag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. <span data-ttu-id="4228b-130">Vizsgálat SensorTag eszközökről hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4228b-130">Scan SensorTag devices by running hello following commands:</span></span>

   ```bash
   gulp discover-sensortag
   ```

7. <span data-ttu-id="4228b-131">Közötti kapcsolat működőképességét hello hello SensorTag és Intel NUC hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4228b-131">Test hello connectivity between hello SensorTag and Intel NUC by running hello following command:</span></span>

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   <span data-ttu-id="4228b-132">Cserélje le `{mac address}` a hello hello előző lépésben beszerzett MAC-címet.</span><span class="sxs-lookup"><span data-stu-id="4228b-132">Replace `{mac address}` with hello MAC address that you obtained in hello previous step.</span></span>

## <a name="get-hello-connection-string-of-sensortag"></a><span data-ttu-id="4228b-133">Hello kapcsolati karakterlánca SensorTag beolvasása</span><span class="sxs-lookup"><span data-stu-id="4228b-133">Get hello connection string of SensorTag</span></span>

<span data-ttu-id="4228b-134">tooget hello Azure IoT hub kapcsolati karakterlánca SensorTag, futtassa a következő parancsot a hello gazdaszámítógépen hello:</span><span class="sxs-lookup"><span data-stu-id="4228b-134">tooget hello Azure IoT hub connection string of SensorTag, run hello following command on hello host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="4228b-135">`{IoT hub name}`hello IoT-központnév használt van.</span><span class="sxs-lookup"><span data-stu-id="4228b-135">`{IoT hub name}` is hello IoT hub name that you used.</span></span> <span data-ttu-id="4228b-136">Az iot-átjáró használatához hello értékeként `{resource group name}` és mydevice hello értékeként `{device id}` nem módosítása hello érték a 2.</span><span class="sxs-lookup"><span data-stu-id="4228b-136">Use iot-gateway as hello value of `{resource group name}` and use mydevice as hello value of `{device id}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="configure-hello-ble-sample-application"></a><span data-ttu-id="4228b-137">Hello BLA mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4228b-137">Configure hello BLE sample application</span></span>

<span data-ttu-id="4228b-138">tooconfigure és futtatási hello BLA mintaalkalmazást, kövesse az alábbi lépéseket hello állomáson:</span><span class="sxs-lookup"><span data-stu-id="4228b-138">tooconfigure and run hello BLE sample application, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="4228b-139">Nyissa meg `config-sensortag.json` a Visual Studio Code hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4228b-139">Open `config-sensortag.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![config sensortag képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. <span data-ttu-id="4228b-141">Hajtsa végre a következő hello kód csere hello:</span><span class="sxs-lookup"><span data-stu-id="4228b-141">Make hello following replacements in hello code:</span></span>
   - <span data-ttu-id="4228b-142">Cserélje le `[IoT hub name]` nevű hello IoT hub használt.</span><span class="sxs-lookup"><span data-stu-id="4228b-142">Replace `[IoT hub name]` with hello IoT hub name that you used.</span></span>
   - <span data-ttu-id="4228b-143">Cserélje le `[IoT device connection string]` a beszerzett SensorTag hello kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="4228b-143">Replace `[IoT device connection string]` with hello connection string of SensorTag that you obtained.</span></span>
   - <span data-ttu-id="4228b-144">Cserélje le `[device_mac_address]` a hello hello beszerzett SensorTag MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="4228b-144">Replace `[device_mac_address]` with hello MAC address of hello SensorTag that you obtained.</span></span>

3. <span data-ttu-id="4228b-145">Futtassa a hello BLA mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4228b-145">Run hello BLE sample application.</span></span>

   <span data-ttu-id="4228b-146">toorun hello BLA mintaalkalmazást, kövesse az alábbi lépéseket hello állomáson:</span><span class="sxs-lookup"><span data-stu-id="4228b-146">toorun hello BLE sample application, follow these steps on hello host computer:</span></span>

   1. <span data-ttu-id="4228b-147">Kapcsolja be a SensorTag.</span><span class="sxs-lookup"><span data-stu-id="4228b-147">Turn on SensorTag.</span></span>

   2. <span data-ttu-id="4228b-148">Központi telepítése, és futtassa hello BLA mintaalkalmazás Intel NUC hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4228b-148">Deploy and run hello BLE sample application on Intel NUC by running hello following command:</span></span>
   
      ```bash
      gulp run
      ```

## <a name="verify-that-hello-ble-sample-application-works"></a><span data-ttu-id="4228b-149">Győződjön meg arról, hogy működik-e hello BLA mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="4228b-149">Verify that hello BLE sample application works</span></span>

<span data-ttu-id="4228b-150">Ekkor megjelenik egy kimeneti hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="4228b-150">You should now see an output like hello following:</span></span>

![BLA alkalmazás Kimenetminta](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

<span data-ttu-id="4228b-152">hello mintaalkalmazás tartja hőmérséklet adatokat gyűjt, és küldje el tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="4228b-152">hello sample application keeps collecting temperature data and sent it tooyour IoT hub.</span></span> <span data-ttu-id="4228b-153">hello mintaalkalmazás automatikusan 40 másodperc elküldése után leáll.</span><span class="sxs-lookup"><span data-stu-id="4228b-153">hello sample application terminates automatically after sending 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="4228b-154">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="4228b-154">Summary</span></span>

<span data-ttu-id="4228b-155">Hogy sikeresen összekapcsolhatók hello SensorTag és Intel NUC, és adatokat gyűjt és SensorTag tooyour IoT hubról BLA mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="4228b-155">You've successfully set up hello connectivity between SensorTag and Intel NUC, and run a BLE sample application which collects and sends data from SensorTag tooyour IoT hub.</span></span> <span data-ttu-id="4228b-156">Most készen áll a toolearn módját, amely az IoT hub kapott tooverify hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="4228b-156">You're ready toolearn how tooverify that your IoT hub has received hello data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4228b-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4228b-157">Next steps</span></span>
[<span data-ttu-id="4228b-158">Üzenetek olvasása az IoT Hubról</span><span class="sxs-lookup"><span data-stu-id="4228b-158">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)
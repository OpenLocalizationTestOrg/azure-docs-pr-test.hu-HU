---
title: "SensorTag eszköz & Azure IoT átjáró - rész 3: futtassa a mintaalkalmazást |} Microsoft Docs"
description: "Adatok fogadására BLA SensorTag és az IoT hub BLA mintaalkalmazás futtatása."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "bla app, érzékelő alkalmazás monitorozása, érzékelő adatok gyűjtését, érzékelőket, érzékelők adataiból, hogy a felhő adatait"
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
ms.openlocfilehash: f6fa158dbe1d48be7d493efa6217e1e0a759d2f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a><span data-ttu-id="a5e8b-104">Konfigurálja és BLA mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a5e8b-104">Configure and run a BLE sample application</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="a5e8b-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a5e8b-105">What you will do</span></span>

- <span data-ttu-id="a5e8b-106">A minta-tárház klónozása.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-106">Clone the sample repository.</span></span> 
- <span data-ttu-id="a5e8b-107">Összekapcsolhatók a SensorTag és Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-107">Set up the connectivity between SensorTag and Intel NUC.</span></span> 
- <span data-ttu-id="a5e8b-108">Az Azure parancssori felület használatával az IoT-központ és BLA (Bluetooth alacsony energia) mintaalkalmazás SensorTag adatait.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-108">Use the Azure CLI to get your IoT hub and SensorTag information for a BLE(Bluetooth Low Energy) sample application.</span></span> <span data-ttu-id="a5e8b-109">És konfigurálása, és futtassa a BLA mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-109">And configure and run the BLE sample application.</span></span> 

<span data-ttu-id="a5e8b-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a5e8b-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a5e8b-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a5e8b-111">What you will learn</span></span>

<span data-ttu-id="a5e8b-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-112">In this article, you will learn:</span></span>

- <span data-ttu-id="a5e8b-113">Hogyan lehet konfigurálni, és futtassa a BLA mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-113">How to configure and run the BLE sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a5e8b-114">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a5e8b-114">What you need</span></span>

<span data-ttu-id="a5e8b-115">Sikeresen végrehajtotta</span><span class="sxs-lookup"><span data-stu-id="a5e8b-115">You must have successfully completed</span></span>

- [<span data-ttu-id="a5e8b-116">Létrehoz egy IoT-központot, és regisztrálja a SensorTag</span><span class="sxs-lookup"><span data-stu-id="a5e8b-116">Create an IoT hub and register SensorTag</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a><span data-ttu-id="a5e8b-117">A gazdagépnek a minta-tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="a5e8b-117">Clone the sample repository to the host computer</span></span>

<span data-ttu-id="a5e8b-118">A minta-tárház klónozása, kövesse az alábbi lépéseket az állomáson:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-118">To clone the sample repository, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="a5e8b-119">Nyisson meg egy parancssort a Windows, vagy nyisson meg egy terminál macOS vagy Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-119">Open a Command Prompt window in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="a5e8b-120">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-120">Run the following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-the-connectivity-between-sensortag-and-intel-nuc"></a><span data-ttu-id="a5e8b-121">Összekapcsolhatók a SensorTag és Intel NUC</span><span class="sxs-lookup"><span data-stu-id="a5e8b-121">Set up the connectivity between SensorTag and Intel NUC</span></span>

<span data-ttu-id="a5e8b-122">A kapcsolat beállításához, kövesse az alábbi lépéseket az állomáson:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-122">To set up the connectivity, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="a5e8b-123">A konfigurációs fájl inicializálása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-123">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. <span data-ttu-id="a5e8b-124">Nyissa meg `config-gateway.json` a Visual Studio Code a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-124">Open `config-gateway.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. <span data-ttu-id="a5e8b-125">Keresse meg a következő kódsort, és cserélje le `[device hostname or IP address]` Intel NUC IP cím vagy állomásnév nevével.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-125">Locate the following line of code and replace `[device hostname or IP address]` with the IP address or host name of Intel NUC.</span></span>
   <span data-ttu-id="a5e8b-126">![Képernyőkép a config átjáró](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="a5e8b-126">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

4. <span data-ttu-id="a5e8b-127">Intel NUC segítő eszközök telepítése a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-127">Install helper tools on Intel NUC by running the following command:</span></span>

   ```bash
   gulp install-tools
   ```

5. <span data-ttu-id="a5e8b-128">A power gombra kattintva a következő képként SensorTag bekapcsolása, és a zöld LED villogni kell.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-128">Turn on SensorTag by pressing the power button as the following picture, and the green LED should blink.</span></span>

   ![Kapcsolja be a Sensor Tag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. <span data-ttu-id="a5e8b-130">SensorTag eszközökről ellenőrzése a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-130">Scan SensorTag devices by running the following commands:</span></span>

   ```bash
   gulp discover-sensortag
   ```

7. <span data-ttu-id="a5e8b-131">A kapcsolatok tesztelése a SensorTag és Intel NUC között a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-131">Test the connectivity between the SensorTag and Intel NUC by running the following command:</span></span>

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   <span data-ttu-id="a5e8b-132">Cserélje le `{mac address}` az előző lépésben kapott MAC-címmel.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-132">Replace `{mac address}` with the MAC address that you obtained in the previous step.</span></span>

## <a name="get-the-connection-string-of-sensortag"></a><span data-ttu-id="a5e8b-133">A kapcsolati karakterlánc SensorTag az beszerzése</span><span class="sxs-lookup"><span data-stu-id="a5e8b-133">Get the connection string of SensorTag</span></span>

<span data-ttu-id="a5e8b-134">Ahhoz, hogy az Azure IoT hub kapcsolati karakterlánca SensorTag, futtassa a következő parancsot az állomáson:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-134">To get the Azure IoT hub connection string of SensorTag, run the following command on the host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="a5e8b-135">`{IoT hub name}`az IoT-központnév használt van.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-135">`{IoT hub name}` is the IoT hub name that you used.</span></span> <span data-ttu-id="a5e8b-136">Az iot-átjáró használatához értékeként `{resource group name}` mydevice használják, értékének `{device id}` Ha nem módosítja az érték a 2.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-136">Use iot-gateway as the value of `{resource group name}` and use mydevice as the value of `{device id}` if you didn't change the value in Lesson 2.</span></span>

## <a name="configure-the-ble-sample-application"></a><span data-ttu-id="a5e8b-137">A táblázat mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a5e8b-137">Configure the BLE sample application</span></span>

<span data-ttu-id="a5e8b-138">Konfigurálja, és futtassa a BLA mintaalkalmazást, kövesse az alábbi lépéseket az állomáson:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-138">To configure and run the BLE sample application, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="a5e8b-139">Nyissa meg `config-sensortag.json` a Visual Studio Code a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-139">Open `config-sensortag.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![config sensortag képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. <span data-ttu-id="a5e8b-141">Végezze el az alábbi új kódot:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-141">Make the following replacements in the code:</span></span>
   - <span data-ttu-id="a5e8b-142">Cserélje le `[IoT hub name]` nevű az IoT hub használt.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-142">Replace `[IoT hub name]` with the IoT hub name that you used.</span></span>
   - <span data-ttu-id="a5e8b-143">Cserélje le `[IoT device connection string]` SensorTag beszerzett kapcsolati karakterlánccal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-143">Replace `[IoT device connection string]` with the connection string of SensorTag that you obtained.</span></span>
   - <span data-ttu-id="a5e8b-144">Cserélje le `[device_mac_address]` a beszerzett SensorTag a MAC-címmel.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-144">Replace `[device_mac_address]` with the MAC address of the SensorTag that you obtained.</span></span>

3. <span data-ttu-id="a5e8b-145">Futtassa a BLA mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-145">Run the BLE sample application.</span></span>

   <span data-ttu-id="a5e8b-146">Futtassa a BLA mintaalkalmazást, kövesse az alábbi lépéseket az állomáson:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-146">To run the BLE sample application, follow these steps on the host computer:</span></span>

   1. <span data-ttu-id="a5e8b-147">Kapcsolja be a SensorTag.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-147">Turn on SensorTag.</span></span>

   2. <span data-ttu-id="a5e8b-148">Központi telepítése, és a BLA mintaalkalmazás Intel NUC futtassa a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-148">Deploy and run the BLE sample application on Intel NUC by running the following command:</span></span>
   
      ```bash
      gulp run
      ```

## <a name="verify-that-the-ble-sample-application-works"></a><span data-ttu-id="a5e8b-149">Győződjön meg arról, hogy működik-e a BLA mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="a5e8b-149">Verify that the BLE sample application works</span></span>

<span data-ttu-id="a5e8b-150">Most látnia kell a következőhöz hasonló kimenetnek:</span><span class="sxs-lookup"><span data-stu-id="a5e8b-150">You should now see an output like the following:</span></span>

![BLA alkalmazás Kimenetminta](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

<span data-ttu-id="a5e8b-152">A mintaalkalmazás tartja hőmérséklet adatokat gyűjt, és elküldi az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-152">The sample application keeps collecting temperature data and sent it to your IoT hub.</span></span> <span data-ttu-id="a5e8b-153">A mintaalkalmazás automatikusan 40 másodperc elküldése után leáll.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-153">The sample application terminates automatically after sending 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="a5e8b-154">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a5e8b-154">Summary</span></span>

<span data-ttu-id="a5e8b-155">Hogy sikeresen összekapcsolhatók a SensorTag és Intel NUC, és futtassa a BLA mintaalkalmazást, amely összegyűjti és SensorTag adatokat küld az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-155">You've successfully set up the connectivity between SensorTag and Intel NUC, and run a BLE sample application which collects and sends data from SensorTag to your IoT hub.</span></span> <span data-ttu-id="a5e8b-156">Készen áll a megtudhatja, hogyan ellenőrizheti, hogy az IoT hub kapott-e az adatokat.</span><span class="sxs-lookup"><span data-stu-id="a5e8b-156">You're ready to learn how to verify that your IoT hub has received the data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5e8b-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a5e8b-157">Next steps</span></span>
[<span data-ttu-id="a5e8b-158">Üzenetek olvasása az IoT Hubról</span><span class="sxs-lookup"><span data-stu-id="a5e8b-158">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)
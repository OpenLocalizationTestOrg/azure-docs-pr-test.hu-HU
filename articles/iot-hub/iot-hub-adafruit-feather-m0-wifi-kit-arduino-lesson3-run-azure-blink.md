---
title: "Az Azure IoT - lecke 3 Connect Arduino (C): a minta futtatásához |} Microsoft Docs"
description: "Regisztrálhat és futtathat a Adafruit lágyított M0 Wi-Fi, amely üzeneteket küld az IoT hub és a LED villogjon mintaalkalmazást."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT felhőalapú szolgáltatás, arduino adatok felhőbe küldése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 92cce319-2b17-4c9b-889d-deac959e3e7c
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0c17fe74dbd78abca955f7789a1674ed6333367f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a><span data-ttu-id="22cf1-104">Futtassa a mintaalkalmazást eszközről a felhőbe üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="22cf1-104">Run a sample application to send device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="22cf1-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="22cf1-105">What you will do</span></span>
<span data-ttu-id="22cf1-106">Ez a cikk bemutatja, hogyan helyezhet üzembe és futtassa a mintaalkalmazást a Adafruit lágyított M0 Wi-Fi Arduino táblán, amely üzeneteket küld az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="22cf1-106">This article will show you how to deploy and run a sample application on your Adafruit Feather M0 WiFi Arduino board that sends messages to your IoT hub.</span></span>

<span data-ttu-id="22cf1-107">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="22cf1-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="22cf1-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="22cf1-108">What you will learn</span></span>
<span data-ttu-id="22cf1-109">Megtudhatja, hogyan telepítheti, és futtassa a mintaalkalmazást Arduino a Arduino táblán gulp eszközzel.</span><span class="sxs-lookup"><span data-stu-id="22cf1-109">You will learn how to use the gulp tool to deploy and run the sample Arduino application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="22cf1-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="22cf1-110">What you need</span></span>
* <span data-ttu-id="22cf1-111">Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazást és egy tárfiókot, feldolgozást és tárolást IoT hub üzenetek][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="22cf1-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account to process and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="22cf1-112">Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="22cf1-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="22cf1-113">Az eszköz kapcsolati karakterláncát a Arduino board csatlakozni az IoT hub szolgál.</span><span class="sxs-lookup"><span data-stu-id="22cf1-113">The device connection string is used to connect your Arduino board to your IoT hub.</span></span> <span data-ttu-id="22cf1-114">Az IoT hub kapcsolati karakterlánc az IoT hub csatlakozni az eszközidentitást az IoT-központ a Arduino testület jelölő szolgál.</span><span class="sxs-lookup"><span data-stu-id="22cf1-114">The IoT hub connection string is used to connect your IoT hub to the device identity that represents your Arduino board in the IoT hub.</span></span>

* <span data-ttu-id="22cf1-115">Az erőforráscsoport az IoT hub listán a következő Azure CLI parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="22cf1-115">List all your IoT hubs in your resource group by running the following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="22cf1-116">Használjon `iot-sample` értékeként `{resource group name}` Ha az érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="22cf1-116">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>

* <span data-ttu-id="22cf1-117">Az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő Azure CLI-parancsot:</span><span class="sxs-lookup"><span data-stu-id="22cf1-117">Get the IoT hub connection string by running the following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="22cf1-118">`{my hub name}`az Ön által megadott nevét az IoT hub létrehozása, és a a Arduino board regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="22cf1-118">`{my hub name}` is the name that you specified when you created your IoT hub and registered your Arduino board.</span></span>

* <span data-ttu-id="22cf1-119">Az eszköz kapcsolati karakterlánc lekéréséhez futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="22cf1-119">Get the device connection string by running the following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

<span data-ttu-id="22cf1-120">Használjon `mym0wifi` értékeként `{device id}` Ha az érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="22cf1-120">Use `mym0wifi` as the value of `{device id}` if you didn't change the value.</span></span>
## <a name="configure-the-device-connection"></a><span data-ttu-id="22cf1-121">Az eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="22cf1-121">Configure the device connection</span></span>
<span data-ttu-id="22cf1-122">Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="22cf1-122">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="22cf1-123">A soros port az eszköz az eszköz-felderítési cli az beszerzése:</span><span class="sxs-lookup"><span data-stu-id="22cf1-123">Obtain the serial port of the device with the device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="22cf1-124">Tekintse meg a következőhöz hasonló kimenetnek kell, és a Arduino kártya COM-portot az usb található:</span><span class="sxs-lookup"><span data-stu-id="22cf1-124">You should see an output that is similar to the following and find the usb COM port for your Arduino board:</span></span>

   ![Eszköz felderítése][device-discovery]

2. <span data-ttu-id="22cf1-126">Nyissa meg a fájlt `config.json` a lecke mappában, és adja hozzá az érték a tényleges COM-port száma:</span><span class="sxs-lookup"><span data-stu-id="22cf1-126">Open the file `config.json` in the lesson folder and add the value of the found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="22cf1-128">A COM-port, a Windows platformra, formátuma rendelkezik `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="22cf1-128">For the COM port, on Windows platform, it has the format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="22cf1-129">MacOS vagy Ubuntu, kezdődik `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="22cf1-129">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

3. <span data-ttu-id="22cf1-130">A konfigurációs fájl inicializálása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="22cf1-130">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. <span data-ttu-id="22cf1-131">Nyissa meg az eszköz konfigurációs fájlját `config-arduino.json` a Visual Studio Code a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="22cf1-131">Open the device configuration file `config-arduino.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![konfiguráció-arduino.json][config-arduino-json]

5. <span data-ttu-id="22cf1-133">Ellenőrizze a következő cserékhez a `config-arduino.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="22cf1-133">Make the following replacements in the `config-arduino.json` file:</span></span>

   * <span data-ttu-id="22cf1-134">Cserélje le **[Wi-Fi SSID]** rendelkező a Wi-Fi SSID, amely kapcsolódik az internethez.</span><span class="sxs-lookup"><span data-stu-id="22cf1-134">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected to the Internet.</span></span>
   * <span data-ttu-id="22cf1-135">Cserélje le **[Wi-Fi jelszó]** a Wi-Fi jelszóval.</span><span class="sxs-lookup"><span data-stu-id="22cf1-135">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="22cf1-136">Távolítsa el a karakterláncot, ha a Wi-Fi nem követeli meg.</span><span class="sxs-lookup"><span data-stu-id="22cf1-136">Remove the string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="22cf1-137">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** rendelkező a `device connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="22cf1-137">Replace **[IoT device connection string]** with the `device connection string` you obtained.</span></span>
   * <span data-ttu-id="22cf1-138">Cserélje le **[IoT hub kapcsolati karakterlánc]** rendelkező a `iot hub connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="22cf1-138">Replace **[IoT hub connection string]** with the `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="22cf1-139">Nem kell `azure_storage_connection_string` ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="22cf1-139">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="22cf1-140">Tartsa a jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="22cf1-140">Keep it as is.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="22cf1-141">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="22cf1-141">Deploy and run the sample application</span></span>
<span data-ttu-id="22cf1-142">Telepíthet, és futtassa a mintaalkalmazást a Arduino táblán a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="22cf1-142">Deploy and run the sample application on your Arduino board by running the following command:</span></span>

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> <span data-ttu-id="22cf1-143">Az alapértelmezett gulp feladat futtatása `install-tools` és `run` feladatok egymás után.</span><span class="sxs-lookup"><span data-stu-id="22cf1-143">The default gulp task runs `install-tools` and `run` tasks sequentially.</span></span> <span data-ttu-id="22cf1-144">Ha Ön [a villogási alkalmazásnak telepítve][deployed-the-blink-app], külön-külön futtatta ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="22cf1-144">When you [deployed the blink app][deployed-the-blink-app], you ran these tasks separately.</span></span>

## <a name="verify-that-the-sample-application-works"></a><span data-ttu-id="22cf1-145">Győződjön meg arról, hogy működik-e a mintaalkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="22cf1-145">Verify that the sample application works</span></span>
<span data-ttu-id="22cf1-146">Meg kell jelennie a GPIO #0 a helyi LED villogó két másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="22cf1-146">You should see the GPIO #0 on-board LED blinking every two seconds.</span></span> <span data-ttu-id="22cf1-147">Minden alkalommal, amikor a LED villogjon, a mintaalkalmazást az IoT hub üzenetet küld, és ellenőrzi, hogy az üzenet már sikeresen elküldte az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="22cf1-147">Every time the LED blinks, the sample application sends a message to your IoT hub and verifies that the message has been successfully sent to your IoT hub.</span></span> <span data-ttu-id="22cf1-148">Emellett minden üzenetet az IoT-központ fogadja a konzolablakban nyomtatása.</span><span class="sxs-lookup"><span data-stu-id="22cf1-148">In addition, each message received by the IoT hub is printed in the console window.</span></span> <span data-ttu-id="22cf1-149">A mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.</span><span class="sxs-lookup"><span data-stu-id="22cf1-149">The sample application terminates automatically after sending 20 messages.</span></span>

![A mintaalkalmazás küldött és fogadott üzenetek][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="22cf1-151">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="22cf1-151">Summary</span></span>
<span data-ttu-id="22cf1-152">Már telepített, és futtassa a villogási új mintaalkalmazást a Arduino táblán, az IoT hub eszköz a felhőbe küldött üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="22cf1-152">You've deployed and run the new blink sample application on your Arduino board to send device-to-cloud messages to your IoT hub.</span></span> <span data-ttu-id="22cf1-153">Az üzenetek most szerint a tárfiók írás figyelje.</span><span class="sxs-lookup"><span data-stu-id="22cf1-153">You now monitor your messages as they are written to the storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="22cf1-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="22cf1-154">Next steps</span></span>
<span data-ttu-id="22cf1-155">[Az Azure Storage megőrzött üzenetek olvasása][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="22cf1-155">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/config-arduino.png
[deployed-the-blink-app]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_run_arduino.png
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md
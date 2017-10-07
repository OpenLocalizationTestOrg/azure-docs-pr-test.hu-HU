---
title: "Connect Arduino (C) tooAzure IoT - lecke 3: a minta futtatásához |} Microsoft Docs"
description: "Regisztrálhat és futtathat egy minta alkalmazás tooAdafruit lágyított M0 Wi-Fi, amely tooyour IoT-központ üzeneteket küld, és villogjon-hello LED-jét."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT felhőalapú szolgáltatás, arduino küldése adatok toocloud"
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
ms.openlocfilehash: ddca015a3655f8a1a9de2a00e718ec0d28a5affb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a><span data-ttu-id="2cf37-104">Futtassa egy minta alkalmazás toosend eszközről a felhőbe üzenetek</span><span class="sxs-lookup"><span data-stu-id="2cf37-104">Run a sample application toosend device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="2cf37-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="2cf37-105">What you will do</span></span>
<span data-ttu-id="2cf37-106">Ez a cikk bemutatja, hogyan toodeploy és a Adafruit lágyított M0 Wi-Fi Arduino mintaalkalmazás lefuttatva board, hogy küld üzeneteket tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="2cf37-106">This article will show you how toodeploy and run a sample application on your Adafruit Feather M0 WiFi Arduino board that sends messages tooyour IoT hub.</span></span>

<span data-ttu-id="2cf37-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="2cf37-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="2cf37-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="2cf37-108">What you will learn</span></span>
<span data-ttu-id="2cf37-109">Megtudhatja, hogyan toouse hello eszköz toodeploy gulp és hello Arduino mintaalkalmazás futtatása a Arduino táblán.</span><span class="sxs-lookup"><span data-stu-id="2cf37-109">You will learn how toouse hello gulp tool toodeploy and run hello sample Arduino application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2cf37-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="2cf37-110">What you need</span></span>
* <span data-ttu-id="2cf37-111">Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazás és a tárolási fiók tooprocess és a tároló IoT-központ üzenetek][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="2cf37-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="2cf37-112">Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="2cf37-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="2cf37-113">eszköz kapcsolati karakterlánc hello használt tooconnect az Arduino Bizottság tooyour IoT hub van.</span><span class="sxs-lookup"><span data-stu-id="2cf37-113">hello device connection string is used tooconnect your Arduino board tooyour IoT hub.</span></span> <span data-ttu-id="2cf37-114">hello IoT hub kapcsolati karakterlánca az IoT hub toohello eszközidentitás a Arduino jelölő board hello IoT-központ a használt tooconnect.</span><span class="sxs-lookup"><span data-stu-id="2cf37-114">hello IoT hub connection string is used tooconnect your IoT hub toohello device identity that represents your Arduino board in hello IoT hub.</span></span>

* <span data-ttu-id="2cf37-115">Az erőforráscsoport az IoT hub listában hello Azure CLI parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2cf37-115">List all your IoT hubs in your resource group by running hello following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="2cf37-116">Használjon `iot-sample` hello értékeként `{resource group name}` Ha hello érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="2cf37-116">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>

* <span data-ttu-id="2cf37-117">Hello IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő parancs az Azure parancssori felület hello:</span><span class="sxs-lookup"><span data-stu-id="2cf37-117">Get hello IoT hub connection string by running hello following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="2cf37-118">`{my hub name}`Ez az IoT hub létrehozása, és a regisztrált a Arduino board megadott hello név.</span><span class="sxs-lookup"><span data-stu-id="2cf37-118">`{my hub name}` is hello name that you specified when you created your IoT hub and registered your Arduino board.</span></span>

* <span data-ttu-id="2cf37-119">Hello eszköz kapcsolati karakterlánc beolvasása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2cf37-119">Get hello device connection string by running hello following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

<span data-ttu-id="2cf37-120">Használjon `mym0wifi` hello értékeként `{device id}` Ha hello érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="2cf37-120">Use `mym0wifi` as hello value of `{device id}` if you didn't change hello value.</span></span>
## <a name="configure-hello-device-connection"></a><span data-ttu-id="2cf37-121">Hello eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2cf37-121">Configure hello device connection</span></span>
<span data-ttu-id="2cf37-122">tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2cf37-122">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="2cf37-123">Hello soros port hello eszköz hello eszköz felderítési cli az beszerzése:</span><span class="sxs-lookup"><span data-stu-id="2cf37-123">Obtain hello serial port of hello device with hello device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="2cf37-124">Meg kell, amely hasonló toohello következő kimenetnek, és található hello usb COM-portot a Arduino Bizottság:</span><span class="sxs-lookup"><span data-stu-id="2cf37-124">You should see an output that is similar toohello following and find hello usb COM port for your Arduino board:</span></span>

   ![Eszköz felderítése][device-discovery]

2. <span data-ttu-id="2cf37-126">Nyissa meg hello fájl `config.json` a hello lecke mappa, és adja hozzá a COM-port száma található hello hello értékét:</span><span class="sxs-lookup"><span data-stu-id="2cf37-126">Open hello file `config.json` in hello lesson folder and add hello value of hello found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="2cf37-128">Hello COM-porthoz, a Windows platformra, hello formátuma rendelkezik `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="2cf37-128">For hello COM port, on Windows platform, it has hello format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="2cf37-129">MacOS vagy Ubuntu, kezdődik `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="2cf37-129">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

3. <span data-ttu-id="2cf37-130">Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2cf37-130">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. <span data-ttu-id="2cf37-131">Nyissa meg hello eszköz konfigurációs fájl `config-arduino.json` a Visual Studio Code hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2cf37-131">Open hello device configuration file `config-arduino.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![konfiguráció-arduino.json][config-arduino-json]

5. <span data-ttu-id="2cf37-133">Ellenőrizze a következő cserékhez a hello hello `config-arduino.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="2cf37-133">Make hello following replacements in hello `config-arduino.json` file:</span></span>

   * <span data-ttu-id="2cf37-134">Cserélje le **[Wi-Fi SSID]** rendelkező a Wi-Fi SSID toohello internethez csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="2cf37-134">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected toohello Internet.</span></span>
   * <span data-ttu-id="2cf37-135">Cserélje le **[Wi-Fi jelszó]** a Wi-Fi jelszóval.</span><span class="sxs-lookup"><span data-stu-id="2cf37-135">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="2cf37-136">Távolítsa el a hello karakterlánc, ha a Wi-Fi nem követeli meg.</span><span class="sxs-lookup"><span data-stu-id="2cf37-136">Remove hello string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="2cf37-137">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a hello `device connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="2cf37-137">Replace **[IoT device connection string]** with hello `device connection string` you obtained.</span></span>
   * <span data-ttu-id="2cf37-138">Cserélje le **[IoT hub kapcsolati karakterlánc]** a hello `iot hub connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="2cf37-138">Replace **[IoT hub connection string]** with hello `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2cf37-139">Nem kell `azure_storage_connection_string` ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="2cf37-139">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="2cf37-140">Tartsa a jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="2cf37-140">Keep it as is.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="2cf37-141">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2cf37-141">Deploy and run hello sample application</span></span>
<span data-ttu-id="2cf37-142">Telepíthet, és futtassa a hello mintaalkalmazást a Arduino táblán hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2cf37-142">Deploy and run hello sample application on your Arduino board by running hello following command:</span></span>

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> <span data-ttu-id="2cf37-143">hello alapértelmezett gulp feladat futtatása `install-tools` és `run` feladatok egymás után.</span><span class="sxs-lookup"><span data-stu-id="2cf37-143">hello default gulp task runs `install-tools` and `run` tasks sequentially.</span></span> <span data-ttu-id="2cf37-144">Ha Ön [hello villogási alkalmazásnak telepítve][deployed-the-blink-app], külön-külön futtatta ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="2cf37-144">When you [deployed hello blink app][deployed-the-blink-app], you ran these tasks separately.</span></span>

## <a name="verify-that-hello-sample-application-works"></a><span data-ttu-id="2cf37-145">Győződjön meg arról, hogy működik-e hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2cf37-145">Verify that hello sample application works</span></span>
<span data-ttu-id="2cf37-146">Meg kell jelennie a hello GPIO #0 a helyi villogó LED két másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="2cf37-146">You should see hello GPIO #0 on-board LED blinking every two seconds.</span></span> <span data-ttu-id="2cf37-147">Minden alkalommal, amikor villogjon-hello LED-jét, hello mintaalkalmazás üzenet tooyour IoT-központ küld, és ellenőrzi, hogy hello üzenet elküldése megtörtént tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="2cf37-147">Every time hello LED blinks, hello sample application sends a message tooyour IoT hub and verifies that hello message has been successfully sent tooyour IoT hub.</span></span> <span data-ttu-id="2cf37-148">Emellett minden egyes hello IoT-központ által fogadott üzenet nyomtatása hello console ablakban.</span><span class="sxs-lookup"><span data-stu-id="2cf37-148">In addition, each message received by hello IoT hub is printed in hello console window.</span></span> <span data-ttu-id="2cf37-149">hello mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.</span><span class="sxs-lookup"><span data-stu-id="2cf37-149">hello sample application terminates automatically after sending 20 messages.</span></span>

![A mintaalkalmazás küldött és fogadott üzenetek][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="2cf37-151">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="2cf37-151">Summary</span></span>
<span data-ttu-id="2cf37-152">Már telepített, és a Arduino board toosend eszköz a felhőbe küldött üzeneteket tooyour IoT hub hello új villogási mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="2cf37-152">You've deployed and run hello new blink sample application on your Arduino board toosend device-to-cloud messages tooyour IoT hub.</span></span> <span data-ttu-id="2cf37-153">Az üzenetek most szerint toohello tárfiók írás figyelje.</span><span class="sxs-lookup"><span data-stu-id="2cf37-153">You now monitor your messages as they are written toohello storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2cf37-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2cf37-154">Next steps</span></span>
<span data-ttu-id="2cf37-155">[Az Azure Storage megőrzött üzenetek olvasása][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="2cf37-155">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/config-arduino.png
[deployed-the-blink-app]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_run_arduino.png
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md
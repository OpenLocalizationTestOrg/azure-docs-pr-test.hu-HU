---
title: "Az Azure IoT - lecke 3 Connect Raspberry pi (C): a minta futtatásához |} Microsoft Docs"
description: "Központi telepítése, és futtassa a mintaalkalmazást, amely üzeneteket küld az IoT hub és a LED villogjon málna Pi 3."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "villogási kereslet az olyan felhő pi, villogási vezetett a felhőből"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: e38df29f-f77f-435f-9add-46814297564f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: e583ba455a94f9afcc7b31e49425b518d7968919
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a><span data-ttu-id="99458-104">Futtassa a mintaalkalmazást eszközről a felhőbe üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="99458-104">Run a sample application to send device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="99458-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="99458-105">What you will do</span></span>
<span data-ttu-id="99458-106">Ez a cikk bemutatja, hogyan helyezhet üzembe és futtassa a mintaalkalmazást, amely üzeneteket küld az IoT hub málna Pi 3.</span><span class="sxs-lookup"><span data-stu-id="99458-106">This article will show you how to deploy and run a sample application on Raspberry Pi 3 that sends messages to your IoT hub.</span></span> <span data-ttu-id="99458-107">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="99458-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="99458-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="99458-108">What you will learn</span></span>
<span data-ttu-id="99458-109">Megtudhatja, hogyan telepítheti, és futtassa a minta Node.js alkalmazást a Pi gulp eszközzel.</span><span class="sxs-lookup"><span data-stu-id="99458-109">You will learn how to use the gulp tool to deploy and run the sample Node.js application on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="99458-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="99458-110">What you need</span></span>
* <span data-ttu-id="99458-111">Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazást és egy tárfiókot, feldolgozást és tárolást IoT hub üzenetek](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="99458-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account to process and store IoT hub messages](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="99458-112">Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="99458-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="99458-113">Az eszköz kapcsolati karakterláncát a Pi használják az IoT hub való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="99458-113">The device connection string is used by your Pi to connect to your IoT hub.</span></span> <span data-ttu-id="99458-114">Az IoT hub kapcsolati karakterlánc biztosít az identitásjegyzékhez az IoT hub kezelheti az eszközöket, amelyek használata engedélyezett az IoT hub csatlakozni a csatlakozáshoz használt.</span><span class="sxs-lookup"><span data-stu-id="99458-114">The IoT hub connection string is used to connect to the identity registry in your IoT hub to manage the devices that are allowed to connect to your IoT hub.</span></span> 

* <span data-ttu-id="99458-115">Az erőforráscsoport az IoT hub listán a következő Azure CLI parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="99458-115">List all your IoT hubs in your resource group by running the following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="99458-116">Használjon `iot-sample` értékeként `{resource group name}` Ha az érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="99458-116">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>

* <span data-ttu-id="99458-117">Az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő Azure CLI-parancsot:</span><span class="sxs-lookup"><span data-stu-id="99458-117">Get the IoT hub connection string by running the following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

<span data-ttu-id="99458-118">`{my hub name}`az Ön által megadott nevét az IoT hub létrehozása, és ha a Pi regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="99458-118">`{my hub name}` is the name that you specified when you created your IoT hub and registered Pi.</span></span>

* <span data-ttu-id="99458-119">Az eszköz kapcsolati karakterlánc lekéréséhez futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="99458-119">Get the device connection string by running the following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

<span data-ttu-id="99458-120">Használjon `myraspberrypi` értékeként `{device id}` Ha az érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="99458-120">Use `myraspberrypi` as the value of `{device id}` if you didn't change the value.</span></span>

## <a name="configure-the-device-connection"></a><span data-ttu-id="99458-121">Az eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="99458-121">Configure the device connection</span></span>
1. <span data-ttu-id="99458-122">A konfigurációs fájl inicializálása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="99458-122">Initialize the configuration file by running the following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```

> [!NOTE]
> <span data-ttu-id="99458-123">Futtatás **gulp az install-eszközök** és, ha még nem meg lecke 1.</span><span class="sxs-lookup"><span data-stu-id="99458-123">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

2. <span data-ttu-id="99458-124">Nyissa meg az eszköz konfigurációs fájlját `config-raspberrypi.json` a Visual Studio Code a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="99458-124">Open the device configuration file `config-raspberrypi.json` in Visual Studio Code by running the following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. <span data-ttu-id="99458-126">Ellenőrizze a következő cserékhez a `config-raspberrypi.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="99458-126">Make the following replacements in the `config-raspberrypi.json` file:</span></span>
   
   * <span data-ttu-id="99458-127">Cserélje le **[eszköz állomásnév vagy IP-címe]** nevű eszköz IP cím vagy állomásnév során kapott azonosítóértékeket `device-discovery-cli` vagy az eszköz konfigurálása során örökölt értékkel.</span><span class="sxs-lookup"><span data-stu-id="99458-127">Replace **[device hostname or IP address]** with the device IP address or host name you got from `device-discovery-cli` or with the value inherited when you configured your device.</span></span>
   * <span data-ttu-id="99458-128">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** rendelkező a `device connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="99458-128">Replace **[IoT device connection string]** with the `device connection string` you obtained.</span></span>
   * <span data-ttu-id="99458-129">Cserélje le **[IoT hub kapcsolati karakterlánc]** rendelkező a `iot hub connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="99458-129">Replace **[IoT hub connection string]** with the `iot hub connection string` you obtained.</span></span>

> [!NOTE]
> <span data-ttu-id="99458-130">Nem kell `azure_storage_connection_string` ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="99458-130">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="99458-131">Tartsa a jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="99458-131">Keep it as is.</span></span>

<span data-ttu-id="99458-132">Frissítés a `config-raspberrypi.json` fájlt úgy, hogy a számítógépről a mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="99458-132">Update the `config-raspberrypi.json` file so that you can deploy the sample application from your computer.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="99458-133">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="99458-133">Deploy and run the sample application</span></span>
<span data-ttu-id="99458-134">Telepíthet, és futtassa a mintaalkalmazást a Pi a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="99458-134">Deploy and run the sample application on Pi by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a><span data-ttu-id="99458-135">Győződjön meg arról, hogy működik-e a mintaalkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="99458-135">Verify that the sample application works</span></span>
<span data-ttu-id="99458-136">A Pi két másodpercenként villogó kapcsolódó LED kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="99458-136">You should see the LED that is connected to Pi blinking every two seconds.</span></span> <span data-ttu-id="99458-137">Minden alkalommal, amikor a LED villogjon, a mintaalkalmazást az IoT hub üzenetet küld, és ellenőrzi, hogy az üzenet már sikeresen elküldte az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="99458-137">Every time the LED blinks, the sample application sends a message to your IoT hub and verifies that the message has been successfully sent to your IoT hub.</span></span> <span data-ttu-id="99458-138">Emellett minden üzenetet az IoT-központ fogadja a konzolablakban nyomtatása.</span><span class="sxs-lookup"><span data-stu-id="99458-138">In addition, each message received by the IoT hub is printed in the console window.</span></span> <span data-ttu-id="99458-139">A mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.</span><span class="sxs-lookup"><span data-stu-id="99458-139">The sample application terminates automatically after sending 20 messages.</span></span>

![A mintaalkalmazás küldött és fogadott üzenetek](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run_c.png)

## <a name="summary"></a><span data-ttu-id="99458-141">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="99458-141">Summary</span></span>
<span data-ttu-id="99458-142">Már telepített, és futtassa a villogási új mintaalkalmazást az eszközről a felhőbe üzeneteket küldhet az IoT hub Pi.</span><span class="sxs-lookup"><span data-stu-id="99458-142">You've deployed and run the new blink sample application on Pi to send device-to-cloud messages to your IoT hub.</span></span> <span data-ttu-id="99458-143">Az üzenetek most szerint a tárfiók írás figyelje.</span><span class="sxs-lookup"><span data-stu-id="99458-143">You now monitor your messages as they are written to the storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99458-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="99458-144">Next steps</span></span>
[<span data-ttu-id="99458-145">Az Azure Storage megőrzött üzenetek olvasása</span><span class="sxs-lookup"><span data-stu-id="99458-145">Read messages persisted in Azure Storage</span></span>](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md)


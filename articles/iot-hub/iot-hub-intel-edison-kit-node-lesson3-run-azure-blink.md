---
title: "Intel Edison (csomópont) csatlakozni az Azure IoT - lecke 3: üzenetek küldése |} Microsoft Docs"
description: "Regisztrálhat és futtathat a mintaalkalmazás, amely üzeneteket küld az IoT hub és a LED villogjon Intel Edison számára."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT felhőalapú szolgáltatás, arduino adatok felhőbe küldése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 1b3b1074-f4d4-42ac-b32c-55f18b304b44
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d4b520b9a1852a285b1e10b5b35447a54313af9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a><span data-ttu-id="67dce-104">Futtassa a mintaalkalmazást eszközről a felhőbe üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="67dce-104">Run a sample application to send device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="67dce-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="67dce-105">What you will do</span></span>
<span data-ttu-id="67dce-106">Ez a cikk bemutatja, hogyan helyezhet üzembe és futtassa a mintaalkalmazást, amely üzeneteket küld az IoT hub Intel Edison.</span><span class="sxs-lookup"><span data-stu-id="67dce-106">This article will show you how to deploy and run a sample application on Intel Edison that sends messages to your IoT hub.</span></span> <span data-ttu-id="67dce-107">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="67dce-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="67dce-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="67dce-108">What you will learn</span></span>
<span data-ttu-id="67dce-109">Megtudhatja, hogyan telepítheti, és futtassa a mintaalkalmazást C Edison gulp eszközzel.</span><span class="sxs-lookup"><span data-stu-id="67dce-109">You will learn how to use the gulp tool to deploy and run the sample C application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="67dce-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="67dce-110">What you need</span></span>
* <span data-ttu-id="67dce-111">Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazást és egy tárfiókot, feldolgozást és tárolást IoT hub üzenetek][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="67dce-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account to process and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="67dce-112">Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="67dce-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="67dce-113">Az eszköz kapcsolati karakterláncát Edison csatlakozni az IoT hub szolgál.</span><span class="sxs-lookup"><span data-stu-id="67dce-113">The device connection string is used to connect Edison to your IoT hub.</span></span> <span data-ttu-id="67dce-114">Az IoT hub kapcsolati karakterlánc az IoT hub csatlakozni az eszközidentitást az IoT hub Edison jelölő szolgál.</span><span class="sxs-lookup"><span data-stu-id="67dce-114">The IoT hub connection string is used to connect your IoT hub to the device identity that represents Edison in the IoT hub.</span></span>

* <span data-ttu-id="67dce-115">Az erőforráscsoport az IoT hub listán a következő Azure CLI parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="67dce-115">List all your IoT hubs in your resource group by running the following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="67dce-116">Használjon `iot-sample` értékeként `{resource group name}` Ha az érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="67dce-116">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>

* <span data-ttu-id="67dce-117">Az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő Azure CLI-parancsot:</span><span class="sxs-lookup"><span data-stu-id="67dce-117">Get the IoT hub connection string by running the following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="67dce-118">`{my hub name}`az Ön által megadott nevét az IoT hub létrehozása, és ha a Edison regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="67dce-118">`{my hub name}` is the name that you specified when you created your IoT hub and registered Edison.</span></span>

* <span data-ttu-id="67dce-119">Az eszköz kapcsolati karakterlánc lekéréséhez futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="67dce-119">Get the device connection string by running the following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

<span data-ttu-id="67dce-120">Használjon `myinteledison` értékeként `{device id}` Ha az érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="67dce-120">Use `myinteledison` as the value of `{device id}` if you didn't change the value.</span></span>

## <a name="configure-the-device-connection"></a><span data-ttu-id="67dce-121">Az eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="67dce-121">Configure the device connection</span></span>
1. <span data-ttu-id="67dce-122">A konfigurációs fájl inicializálása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="67dce-122">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

2. <span data-ttu-id="67dce-123">Nyissa meg az eszköz konfigurációs fájlját `config-edison.json` a Visual Studio Code a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="67dce-123">Open the device configuration file `config-edison.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)
3. <span data-ttu-id="67dce-125">Ellenőrizze a következő cserékhez a `config-edison.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="67dce-125">Make the following replacements in the `config-edison.json` file:</span></span>

   * <span data-ttu-id="67dce-126">Cserélje le **[eszköz állomásnév vagy IP-címe]** az eszköz konfigurálása során az lefelé megjelölt eszköz IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="67dce-126">Replace **[device hostname or IP address]** with the device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="67dce-127">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** rendelkező a `device connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="67dce-127">Replace **[IoT device connection string]** with the `device connection string` you obtained.</span></span>
   * <span data-ttu-id="67dce-128">Cserélje le **[IoT hub kapcsolati karakterlánc]** rendelkező a `iot hub connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="67dce-128">Replace **[IoT hub connection string]** with the `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="67dce-129">Nem kell `azure_storage_connection_string` ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="67dce-129">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="67dce-130">Tartsa a jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="67dce-130">Keep it as is.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="67dce-131">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="67dce-131">Deploy and run the sample application</span></span>
<span data-ttu-id="67dce-132">Telepíthet, és futtassa a mintaalkalmazást a Edison a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="67dce-132">Deploy and run the sample application on Edison by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a><span data-ttu-id="67dce-133">Győződjön meg arról, hogy működik-e a mintaalkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="67dce-133">Verify that the sample application works</span></span>
<span data-ttu-id="67dce-134">Két másodpercenként villogó Edison kapcsolódó LED kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="67dce-134">You should see the LED that is connected to Edison blinking every two seconds.</span></span> <span data-ttu-id="67dce-135">Minden alkalommal, amikor a LED villogjon, a mintaalkalmazást az IoT hub üzenetet küld, és ellenőrzi, hogy az üzenet már sikeresen elküldte az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="67dce-135">Every time the LED blinks, the sample application sends a message to your IoT hub and verifies that the message has been successfully sent to your IoT hub.</span></span> <span data-ttu-id="67dce-136">Emellett minden üzenetet az IoT-központ fogadja a konzolablakban nyomtatása.</span><span class="sxs-lookup"><span data-stu-id="67dce-136">In addition, each message received by the IoT hub is printed in the console window.</span></span> <span data-ttu-id="67dce-137">A mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.</span><span class="sxs-lookup"><span data-stu-id="67dce-137">The sample application terminates automatically after sending 20 messages.</span></span>

![A mintaalkalmazás küldött és fogadott üzenetek][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="67dce-139">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="67dce-139">Summary</span></span>
<span data-ttu-id="67dce-140">Már telepített, és futtassa a villogási új mintaalkalmazást az eszközről a felhőbe üzeneteket küldhet az IoT hub Edison.</span><span class="sxs-lookup"><span data-stu-id="67dce-140">You've deployed and run the new blink sample application on Edison to send device-to-cloud messages to your IoT hub.</span></span> <span data-ttu-id="67dce-141">Az üzenetek most szerint a tárfiók írás figyelje.</span><span class="sxs-lookup"><span data-stu-id="67dce-141">You now monitor your messages as they are written to the storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="67dce-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="67dce-142">Next steps</span></span>
<span data-ttu-id="67dce-143">[Az Azure Storage megőrzött üzenetek olvasása][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="67dce-143">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-node-lesson3-read-table-storage.md
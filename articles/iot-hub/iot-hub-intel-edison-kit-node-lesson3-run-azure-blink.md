---
title: "Csatlakozás Intel Edison (csomópont) tooAzure IoT - lecke 3: üzenetek küldése |} Microsoft Docs"
description: "Regisztrálhat és futtathat egy minta alkalmazás tooIntel Edison, amely tooyour IoT-központ üzeneteket küld, és villogjon-hello LED-jét."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT felhőalapú szolgáltatás, arduino küldése adatok toocloud"
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
ms.openlocfilehash: ebd4c7558544d64086fb4cd615cee546aeed2fc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a><span data-ttu-id="97385-104">Futtassa egy minta alkalmazás toosend eszközről a felhőbe üzenetek</span><span class="sxs-lookup"><span data-stu-id="97385-104">Run a sample application toosend device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="97385-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="97385-105">What you will do</span></span>
<span data-ttu-id="97385-106">Ez a cikk bemutatja, hogyan toodeploy, és futtassa a mintaalkalmazást az Intel Edison által küldött üzenetek tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="97385-106">This article will show you how toodeploy and run a sample application on Intel Edison that sends messages tooyour IoT hub.</span></span> <span data-ttu-id="97385-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="97385-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="97385-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="97385-108">What you will learn</span></span>
<span data-ttu-id="97385-109">Megtudhatja, hogyan toouse hello gulp eszköz toodeploy lesz, és C mintaalkalmazás hello futtatnak Edison.</span><span class="sxs-lookup"><span data-stu-id="97385-109">You will learn how toouse hello gulp tool toodeploy and run hello sample C application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="97385-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="97385-110">What you need</span></span>
* <span data-ttu-id="97385-111">Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazás és a tárolási fiók tooprocess és a tároló IoT-központ üzenetek][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="97385-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="97385-112">Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="97385-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="97385-113">hello eszköz kapcsolati karakterlánca használt tooconnect Edison tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="97385-113">hello device connection string is used tooconnect Edison tooyour IoT hub.</span></span> <span data-ttu-id="97385-114">az IoT hub kapcsolati karakterlánc hello használt tooconnect az IoT hub toohello eszközidentitás hello IoT-központ Edison jelölő van.</span><span class="sxs-lookup"><span data-stu-id="97385-114">hello IoT hub connection string is used tooconnect your IoT hub toohello device identity that represents Edison in hello IoT hub.</span></span>

* <span data-ttu-id="97385-115">Az erőforráscsoport az IoT hub listában hello Azure CLI parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="97385-115">List all your IoT hubs in your resource group by running hello following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="97385-116">Használjon `iot-sample` hello értékeként `{resource group name}` Ha hello érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="97385-116">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>

* <span data-ttu-id="97385-117">Hello IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő parancs az Azure parancssori felület hello:</span><span class="sxs-lookup"><span data-stu-id="97385-117">Get hello IoT hub connection string by running hello following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="97385-118">`{my hub name}`Ez az IoT hub létrehozása, és ha a Edison regisztrált megadott hello név.</span><span class="sxs-lookup"><span data-stu-id="97385-118">`{my hub name}` is hello name that you specified when you created your IoT hub and registered Edison.</span></span>

* <span data-ttu-id="97385-119">Hello eszköz kapcsolati karakterlánc beolvasása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="97385-119">Get hello device connection string by running hello following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

<span data-ttu-id="97385-120">Használjon `myinteledison` hello értékeként `{device id}` Ha hello érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="97385-120">Use `myinteledison` as hello value of `{device id}` if you didn't change hello value.</span></span>

## <a name="configure-hello-device-connection"></a><span data-ttu-id="97385-121">Hello eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="97385-121">Configure hello device connection</span></span>
1. <span data-ttu-id="97385-122">Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="97385-122">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

2. <span data-ttu-id="97385-123">Nyissa meg hello eszköz konfigurációs fájl `config-edison.json` a Visual Studio Code hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="97385-123">Open hello device configuration file `config-edison.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)
3. <span data-ttu-id="97385-125">Ellenőrizze a következő cserékhez a hello hello `config-edison.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="97385-125">Make hello following replacements in hello `config-edison.json` file:</span></span>

   * <span data-ttu-id="97385-126">Cserélje le **[eszköz állomásnév vagy IP-címe]** hello eszköz IP-cím az eszköz konfigurálása során az lefelé jelölésű.</span><span class="sxs-lookup"><span data-stu-id="97385-126">Replace **[device hostname or IP address]** with hello device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="97385-127">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a hello `device connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="97385-127">Replace **[IoT device connection string]** with hello `device connection string` you obtained.</span></span>
   * <span data-ttu-id="97385-128">Cserélje le **[IoT hub kapcsolati karakterlánc]** a hello `iot hub connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="97385-128">Replace **[IoT hub connection string]** with hello `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="97385-129">Nem kell `azure_storage_connection_string` ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="97385-129">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="97385-130">Tartsa a jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="97385-130">Keep it as is.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="97385-131">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="97385-131">Deploy and run hello sample application</span></span>
<span data-ttu-id="97385-132">Központi telepítése, és futtassa a mintaalkalmazást hello Edison hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="97385-132">Deploy and run hello sample application on Edison by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a><span data-ttu-id="97385-133">Győződjön meg arról, hogy működik-e hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="97385-133">Verify that hello sample application works</span></span>
<span data-ttu-id="97385-134">Hello LED-jét, amely két másodpercenként villogó csatlakoztatott tooEdison kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="97385-134">You should see hello LED that is connected tooEdison blinking every two seconds.</span></span> <span data-ttu-id="97385-135">Minden alkalommal, amikor villogjon-hello LED-jét, hello mintaalkalmazás üzenet tooyour IoT-központ küld, és ellenőrzi, hogy hello üzenet elküldése megtörtént tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="97385-135">Every time hello LED blinks, hello sample application sends a message tooyour IoT hub and verifies that hello message has been successfully sent tooyour IoT hub.</span></span> <span data-ttu-id="97385-136">Emellett minden egyes hello IoT-központ által fogadott üzenet nyomtatása hello console ablakban.</span><span class="sxs-lookup"><span data-stu-id="97385-136">In addition, each message received by hello IoT hub is printed in hello console window.</span></span> <span data-ttu-id="97385-137">hello mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.</span><span class="sxs-lookup"><span data-stu-id="97385-137">hello sample application terminates automatically after sending 20 messages.</span></span>

![A mintaalkalmazás küldött és fogadott üzenetek][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="97385-139">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="97385-139">Summary</span></span>
<span data-ttu-id="97385-140">Már telepített, és új villogási mintaalkalmazás hello futtatnak Edison toosend eszköz a felhőbe küldött üzeneteket tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="97385-140">You've deployed and run hello new blink sample application on Edison toosend device-to-cloud messages tooyour IoT hub.</span></span> <span data-ttu-id="97385-141">Az üzenetek most szerint toohello tárfiók írás figyelje.</span><span class="sxs-lookup"><span data-stu-id="97385-141">You now monitor your messages as they are written toohello storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97385-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97385-142">Next steps</span></span>
<span data-ttu-id="97385-143">[Az Azure Storage megőrzött üzenetek olvasása][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="97385-143">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-node-lesson3-read-table-storage.md
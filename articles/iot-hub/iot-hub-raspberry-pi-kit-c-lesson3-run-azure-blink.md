---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 3: a minta futtatásához |} Microsoft Docs"
description: "Regisztrálhat és futtathat egy minta alkalmazás tooRaspberry Pi 3, amely tooyour IoT-központ üzeneteket küld, és villogjon-hello LED-jét."
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
ms.openlocfilehash: c484beb2e2d3a3cf19f071f2ba87b9a4fe41c1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a><span data-ttu-id="b473d-104">Futtassa egy minta alkalmazás toosend eszközről a felhőbe üzenetek</span><span class="sxs-lookup"><span data-stu-id="b473d-104">Run a sample application toosend device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="b473d-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="b473d-105">What you will do</span></span>
<span data-ttu-id="b473d-106">Ez a cikk bemutatja, hogyan toodeploy, és futtassa a mintaalkalmazást málna Pi 3 által küldött üzenetek tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="b473d-106">This article will show you how toodeploy and run a sample application on Raspberry Pi 3 that sends messages tooyour IoT hub.</span></span> <span data-ttu-id="b473d-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="b473d-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="b473d-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="b473d-108">What you will learn</span></span>
<span data-ttu-id="b473d-109">Megtudhatja, hogyan toouse hello eszköz toodeploy gulp és Pi hello minta Node.js-alkalmazást futtatnak.</span><span class="sxs-lookup"><span data-stu-id="b473d-109">You will learn how toouse hello gulp tool toodeploy and run hello sample Node.js application on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="b473d-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="b473d-110">What you need</span></span>
* <span data-ttu-id="b473d-111">Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazás és a tárolási fiók tooprocess és a tároló IoT-központ üzenetek](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="b473d-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account tooprocess and store IoT hub messages](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="b473d-112">Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása</span><span class="sxs-lookup"><span data-stu-id="b473d-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="b473d-113">a Pi tooconnect tooyour IoT-központ hello eszköz kapcsolati karakterlánc használja.</span><span class="sxs-lookup"><span data-stu-id="b473d-113">hello device connection string is used by your Pi tooconnect tooyour IoT hub.</span></span> <span data-ttu-id="b473d-114">az IoT hub kapcsolati karakterlánc hello használt tooconnect toohello identitásjegyzékhez az IoT hub toomanage hello eszközök, amelyek számára engedélyezett tooconnect tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="b473d-114">hello IoT hub connection string is used tooconnect toohello identity registry in your IoT hub toomanage hello devices that are allowed tooconnect tooyour IoT hub.</span></span> 

* <span data-ttu-id="b473d-115">Az erőforráscsoport az IoT hub listában hello Azure CLI parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b473d-115">List all your IoT hubs in your resource group by running hello following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="b473d-116">Használjon `iot-sample` hello értékeként `{resource group name}` Ha hello érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="b473d-116">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>

* <span data-ttu-id="b473d-117">Hello IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő parancs az Azure parancssori felület hello:</span><span class="sxs-lookup"><span data-stu-id="b473d-117">Get hello IoT hub connection string by running hello following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

<span data-ttu-id="b473d-118">`{my hub name}`Ez az IoT hub létrehozása, és ha a Pi regisztrált megadott hello név.</span><span class="sxs-lookup"><span data-stu-id="b473d-118">`{my hub name}` is hello name that you specified when you created your IoT hub and registered Pi.</span></span>

* <span data-ttu-id="b473d-119">Hello eszköz kapcsolati karakterlánc beolvasása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b473d-119">Get hello device connection string by running hello following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

<span data-ttu-id="b473d-120">Használjon `myraspberrypi` hello értékeként `{device id}` Ha hello érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="b473d-120">Use `myraspberrypi` as hello value of `{device id}` if you didn't change hello value.</span></span>

## <a name="configure-hello-device-connection"></a><span data-ttu-id="b473d-121">Hello eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b473d-121">Configure hello device connection</span></span>
1. <span data-ttu-id="b473d-122">Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b473d-122">Initialize hello configuration file by running hello following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```

> [!NOTE]
> <span data-ttu-id="b473d-123">Futtatás **gulp az install-eszközök** és, ha még nem meg lecke 1.</span><span class="sxs-lookup"><span data-stu-id="b473d-123">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

2. <span data-ttu-id="b473d-124">Nyissa meg hello eszköz konfigurációs fájl `config-raspberrypi.json` a Visual Studio Code hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b473d-124">Open hello device configuration file `config-raspberrypi.json` in Visual Studio Code by running hello following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. <span data-ttu-id="b473d-126">Ellenőrizze a következő cserékhez a hello hello `config-raspberrypi.json` fájlt:</span><span class="sxs-lookup"><span data-stu-id="b473d-126">Make hello following replacements in hello `config-raspberrypi.json` file:</span></span>
   
   * <span data-ttu-id="b473d-127">Cserélje le **[eszköz állomásnév vagy IP-címe]** hello IP cím vagy állomásnév eszköznévvel során kapott azonosítóértékeket `device-discovery-cli` vagy az eszköz konfigurálása során örökölt hello értékkel.</span><span class="sxs-lookup"><span data-stu-id="b473d-127">Replace **[device hostname or IP address]** with hello device IP address or host name you got from `device-discovery-cli` or with hello value inherited when you configured your device.</span></span>
   * <span data-ttu-id="b473d-128">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a hello `device connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="b473d-128">Replace **[IoT device connection string]** with hello `device connection string` you obtained.</span></span>
   * <span data-ttu-id="b473d-129">Cserélje le **[IoT hub kapcsolati karakterlánc]** a hello `iot hub connection string` beszerzett.</span><span class="sxs-lookup"><span data-stu-id="b473d-129">Replace **[IoT hub connection string]** with hello `iot hub connection string` you obtained.</span></span>

> [!NOTE]
> <span data-ttu-id="b473d-130">Nem kell `azure_storage_connection_string` ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="b473d-130">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="b473d-131">Tartsa a jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="b473d-131">Keep it as is.</span></span>

<span data-ttu-id="b473d-132">Frissítés hello `config-raspberrypi.json` fájlt úgy, hogy a számítógépről hello mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="b473d-132">Update hello `config-raspberrypi.json` file so that you can deploy hello sample application from your computer.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="b473d-133">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="b473d-133">Deploy and run hello sample application</span></span>
<span data-ttu-id="b473d-134">Központi telepítése, és futtassa a mintaalkalmazást hello Pi hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b473d-134">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a><span data-ttu-id="b473d-135">Győződjön meg arról, hogy működik-e hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="b473d-135">Verify that hello sample application works</span></span>
<span data-ttu-id="b473d-136">Hello LED-jét, amely két másodpercenként villogó csatlakoztatott tooPi kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="b473d-136">You should see hello LED that is connected tooPi blinking every two seconds.</span></span> <span data-ttu-id="b473d-137">Minden alkalommal, amikor villogjon-hello LED-jét, hello mintaalkalmazás üzenet tooyour IoT-központ küld, és ellenőrzi, hogy hello üzenet elküldése megtörtént tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="b473d-137">Every time hello LED blinks, hello sample application sends a message tooyour IoT hub and verifies that hello message has been successfully sent tooyour IoT hub.</span></span> <span data-ttu-id="b473d-138">Emellett minden egyes hello IoT-központ által fogadott üzenet nyomtatása hello console ablakban.</span><span class="sxs-lookup"><span data-stu-id="b473d-138">In addition, each message received by hello IoT hub is printed in hello console window.</span></span> <span data-ttu-id="b473d-139">hello mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.</span><span class="sxs-lookup"><span data-stu-id="b473d-139">hello sample application terminates automatically after sending 20 messages.</span></span>

![A mintaalkalmazás küldött és fogadott üzenetek](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run_c.png)

## <a name="summary"></a><span data-ttu-id="b473d-141">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="b473d-141">Summary</span></span>
<span data-ttu-id="b473d-142">Már telepítve és Pi toosend eszköz a felhőbe küldött üzeneteket tooyour IoT hub hello új villogási mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="b473d-142">You've deployed and run hello new blink sample application on Pi toosend device-to-cloud messages tooyour IoT hub.</span></span> <span data-ttu-id="b473d-143">Az üzenetek most szerint toohello tárfiók írás figyelje.</span><span class="sxs-lookup"><span data-stu-id="b473d-143">You now monitor your messages as they are written toohello storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b473d-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b473d-144">Next steps</span></span>
[<span data-ttu-id="b473d-145">Az Azure Storage megőrzött üzenetek olvasása</span><span class="sxs-lookup"><span data-stu-id="b473d-145">Read messages persisted in Azure Storage</span></span>](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md)


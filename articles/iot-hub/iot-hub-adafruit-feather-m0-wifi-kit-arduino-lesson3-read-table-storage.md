---
title: 'Connect Arduino (C) tooAzure IoT - lecke 3: Table storage |} Microsoft Docs'
description: "Szerint tooyour Azure Table storage megírásának módjától, figyelje a köszönőüzenetei eszközről a felhőbe."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok hello felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 386083e0-0dbb-48c0-9ac2-4f8fb4590772
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9fef18bc9e780e78d95f0c643a5f193125130a5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a><span data-ttu-id="d8682-104">Az Azure Storage megőrzött üzenetek olvasása</span><span class="sxs-lookup"><span data-stu-id="d8682-104">Read messages persisted in Azure Storage</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="d8682-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="d8682-105">What you will do</span></span>
<span data-ttu-id="d8682-106">A figyelő hello eszközről a felhőbe küldött állapotüzenetek az Adafruit lágyított M0 Wi-Fi Arduino board tooyour IoT hub a hello üzenetekként tooyour Azure Table storage készültek.</span><span class="sxs-lookup"><span data-stu-id="d8682-106">Monitor hello device-to-cloud messages that are sent from your Adafruit Feather M0 WiFi Arduino board tooyour IoT hub as hello messages are written tooyour Azure Table storage.</span></span>

<span data-ttu-id="d8682-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="d8682-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="d8682-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="d8682-108">What you will learn</span></span>
<span data-ttu-id="d8682-109">Ebből a cikkből megtudhatja, hogyan toouse gulp üzenet olvasása tevékenység tooread köszönőüzenetei megőrzött az Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="d8682-109">In this article, you will learn how toouse hello gulp read-message task tooread messages persisted in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="d8682-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="d8682-110">What you need</span></span>
<span data-ttu-id="d8682-111">Ez a folyamat megkezdése előtt kell sikeresen befejeződött [hello Azure villogási mintaalkalmazás futtatása a Arduino táblán][run-blink-application].</span><span class="sxs-lookup"><span data-stu-id="d8682-111">Before starting this process, you must have successfully completed [Run hello Azure blink sample application on your Arduino board][run-blink-application].</span></span>

## <a name="read-new-messages-from-your-storage-account"></a><span data-ttu-id="d8682-112">Új üzenetek olvasásakor a tárfiók</span><span class="sxs-lookup"><span data-stu-id="d8682-112">Read new messages from your storage account</span></span>
<span data-ttu-id="d8682-113">Hello előző cikkben a mintaalkalmazást a Arduino táblán futtatta.</span><span class="sxs-lookup"><span data-stu-id="d8682-113">In hello previous article, you ran a sample application on your Arduino board.</span></span> <span data-ttu-id="d8682-114">hello mintaalkalmazás küldött üzenetek tooyour Azure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="d8682-114">hello sample application sent messages tooyour Azure IoT hub.</span></span> <span data-ttu-id="d8682-115">köszönőüzenetei tooyour IoT-központ küldött be a Azure Table storage hello Azure függvény app keresztül tárolja.</span><span class="sxs-lookup"><span data-stu-id="d8682-115">hello messages sent tooyour IoT hub are stored into your Azure Table storage via hello Azure function app.</span></span> <span data-ttu-id="d8682-116">Az az Azure Table storage Azure storage kapcsolati karakterlánc tooread köszönőüzenetei van szüksége.</span><span class="sxs-lookup"><span data-stu-id="d8682-116">You need hello Azure storage connection string tooread messages from your Azure Table storage.</span></span>

<span data-ttu-id="d8682-117">az Azure Table storage-ban tárolt tooread üzenetek kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d8682-117">tooread messages stored in your Azure Table storage, follow these steps:</span></span>

1. <span data-ttu-id="d8682-118">Hello kapcsolati karakterlánc lekéréséhez futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="d8682-118">Get hello connection string by running hello following commands:</span></span>

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   <span data-ttu-id="d8682-119">hello első parancs segítségével lekérdezhető hello `storage name` használt hello második parancs tooget hello kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="d8682-119">hello first command retrieves hello `storage name` that is used in hello second command tooget hello connection string.</span></span> <span data-ttu-id="d8682-120">Használjon `iot-sample` hello értékeként `{resource group name}` Ha hello érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="d8682-120">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>
2. <span data-ttu-id="d8682-121">Nyissa meg hello konfigurációs fájl `config-arduino.json` a Visual Studio Code hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="d8682-121">Open hello configuration file `config-arduino.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```
3. <span data-ttu-id="d8682-122">Cserélje le `[Azure storage connection string]` az 1. lépésben kapott hello kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="d8682-122">Replace `[Azure storage connection string]` with hello connection string you got in step 1.</span></span>
4. <span data-ttu-id="d8682-123">Mentse a hello `config-arduino.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d8682-123">Save hello `config-arduino.json` file.</span></span>
5. <span data-ttu-id="d8682-124">Küldje el újra az üzeneteket, és olvasni őket az Azure Table storage hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="d8682-124">Send messages again and read them from your Azure Table storage by running hello following command:</span></span>

   ```bash
   gulp run --read-storage

   # You can monitor hello serial port by running listen task:
   gulp listen

   # Or you can combine above two gulp tasks into one:
   gulp run --read-storage --listen
   ```

   <span data-ttu-id="d8682-125">hello programot az Azure Table storage olvasásra van hello `azure-table.js` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d8682-125">hello logic for reading from Azure Table storage is in hello `azure-table.js` file.</span></span>

   ![gulp futásra – olvasás-tároló][gulp-run]

## <a name="summary"></a><span data-ttu-id="d8682-127">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="d8682-127">Summary</span></span>
<span data-ttu-id="d8682-128">Hogy sikeresen csatlakozott az Arduino Bizottság tooyour IoT hub hello felhőben és villogási minta alkalmazás toosend eszközről a felhőbe köszönőüzenetei használt.</span><span class="sxs-lookup"><span data-stu-id="d8682-128">You've successfully connected your Arduino board tooyour IoT hub in hello cloud and used hello blink sample application toosend device-to-cloud messages.</span></span> <span data-ttu-id="d8682-129">Hello Azure függvény app toostore bejövő IoT hub üzenetek tooyour Azure Table storage is használt.</span><span class="sxs-lookup"><span data-stu-id="d8682-129">You also used hello Azure function app toostore incoming IoT hub messages tooyour Azure Table storage.</span></span> <span data-ttu-id="d8682-130">Most már az IoT hub tooyour Arduino board küldhet felhő-eszközre küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="d8682-130">You can now send cloud-to-device messages from your IoT hub tooyour Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8682-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d8682-131">Next steps</span></span>
<span data-ttu-id="d8682-132">[Felhő-eszközre küldött üzenetek küldése][send-cloud-to-device-messages]</span><span class="sxs-lookup"><span data-stu-id="d8682-132">[Send cloud-to-device messages][send-cloud-to-device-messages]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[run-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md
[gulp-run]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_read_message_arduino.png
[send-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
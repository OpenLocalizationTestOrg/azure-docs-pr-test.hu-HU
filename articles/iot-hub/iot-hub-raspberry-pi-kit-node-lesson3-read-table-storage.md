---
featureFlags: usabilla
title: "Csatlakozás Azure IoT - lecke 3 málna Pi (csomópont): Table storage |} Microsoft Docs"
description: "Mivel az Azure Table storage írás az eszközről a felhőbe üzenetek figyelése"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "adatok beolvasása a felhőből, iot-felhőszolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 9965bd54-61da-479b-aaad-5604850a2be5
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 60084906c05ff9e5396f8e2378d73f7ac939d8df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a><span data-ttu-id="daf3d-104">Az Azure Storage megőrzött üzenetek olvasása</span><span class="sxs-lookup"><span data-stu-id="daf3d-104">Read messages persisted in Azure Storage</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="daf3d-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="daf3d-105">What you will do</span></span>
<span data-ttu-id="daf3d-106">Az eszközről a felhőbe küldött üzenetek málna Pi 3 az IoT hubhoz, az üzenetek kerülnek az Azure Table storage figyelése.</span><span class="sxs-lookup"><span data-stu-id="daf3d-106">Monitor the device-to-cloud messages that are sent from Raspberry Pi 3 to your IoT hub as the messages are written to your Azure Table storage.</span></span> <span data-ttu-id="daf3d-107">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="daf3d-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="daf3d-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="daf3d-108">What you will learn</span></span>
<span data-ttu-id="daf3d-109">Ebből a cikkből megtudhatja hogyan használható a gulp üzenet olvasása tevékenység olvassa el az Azure Table storage-ban tárolt üzenetek.</span><span class="sxs-lookup"><span data-stu-id="daf3d-109">In this article, you will learn how to use the gulp read-message task to read messages persisted in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="daf3d-110">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="daf3d-110">What you need</span></span>
<span data-ttu-id="daf3d-111">Ez a folyamat megkezdése előtt kell sikeresen befejeződött [futtassa az Azure villogási mintaalkalmazást málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).</span><span class="sxs-lookup"><span data-stu-id="daf3d-111">Before starting this process, you must have successfully completed [Run the Azure blink sample application on Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).</span></span>

## <a name="read-new-messages-from-your-storage-account"></a><span data-ttu-id="daf3d-112">Új üzenetek olvasásakor a tárfiók</span><span class="sxs-lookup"><span data-stu-id="daf3d-112">Read new messages from your storage account</span></span>
<span data-ttu-id="daf3d-113">Az előző cikkben a mintaalkalmazást a Pi futtatta.</span><span class="sxs-lookup"><span data-stu-id="daf3d-113">In the previous article, you ran a sample application on Pi.</span></span> <span data-ttu-id="daf3d-114">A minta alkalmazás küldött üzenetek az Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="daf3d-114">The sample application sent messages to your Azure IoT hub.</span></span> <span data-ttu-id="daf3d-115">Az IoT hub küldött üzenetek tárolja azokat az Azure Table storage segítségével az Azure-függvény alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="daf3d-115">The messages sent to your IoT hub are stored into your Azure Table storage via the Azure function app.</span></span> <span data-ttu-id="daf3d-116">Az Azure tárolási kapcsolati karakterlánc üzeneteket beolvasni az Azure Table storage van szüksége.</span><span class="sxs-lookup"><span data-stu-id="daf3d-116">You need the Azure storage connection string to read messages from your Azure Table storage.</span></span>

<span data-ttu-id="daf3d-117">Olvassa el az Azure Table storage-ban tárolt üzenetek, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="daf3d-117">To read messages stored in your Azure Table storage, follow these steps:</span></span>

1. <span data-ttu-id="daf3d-118">A kapcsolati karakterlánc beolvasása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="daf3d-118">Get the connection string by running the following commands:</span></span>

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   <span data-ttu-id="daf3d-119">Az első parancs segítségével lekérdezhető a `storage name` , amelynek használatával a második parancs a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="daf3d-119">The first command retrieves the `storage name` that is used in the second command to get the connection string.</span></span> <span data-ttu-id="daf3d-120">Használjon `iot-sample` értékeként `{resource group name}` Ha az érték nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="daf3d-120">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>
2. <span data-ttu-id="daf3d-121">Nyissa meg a konfigurációs fájl `config-raspberrypi.json` a Visual Studio Code a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="daf3d-121">Open the configuration file `config-raspberrypi.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
3. <span data-ttu-id="daf3d-122">Cserélje le `[Azure storage connection string]` az 1. lépésben kapott kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="daf3d-122">Replace `[Azure storage connection string]` with the connection string you got in step 1.</span></span>
4. <span data-ttu-id="daf3d-123">Mentse a `config-raspberrypi.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="daf3d-123">Save the `config-raspberrypi.json` file.</span></span>
5. <span data-ttu-id="daf3d-124">Küldje el újra az üzeneteket, és olvasni őket az Azure Table storage a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="daf3d-124">Send messages again and read them from your Azure Table storage by running the following command:</span></span>
   
   ```bash
   gulp run --read-storage
   ```
   
   <span data-ttu-id="daf3d-125">Az Azure Table storage-ből történő olvasáshoz logika van a `azure-table.js` fájlt.</span><span class="sxs-lookup"><span data-stu-id="daf3d-125">The logic for reading from Azure Table storage is in the `azure-table.js` file.</span></span>
   
    ![gulp futásra – olvasás-tároló](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="summary"></a><span data-ttu-id="daf3d-127">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="daf3d-127">Summary</span></span>
<span data-ttu-id="daf3d-128">Hogy sikeresen Pi csatlakozik az IoT hub a felhőben és a villogási mintaalkalmazás eszközről a felhőbe üzenetek küldéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="daf3d-128">You've successfully connected Pi to your IoT hub in the cloud and used the blink sample application to send device-to-cloud messages.</span></span> <span data-ttu-id="daf3d-129">Az Azure-függvény alkalmazás bejövő IoT hub üzenetek tárolására az Azure Table Storage is használt.</span><span class="sxs-lookup"><span data-stu-id="daf3d-129">You also used the Azure function app to store incoming IoT hub messages to your Azure Table storage.</span></span> <span data-ttu-id="daf3d-130">Most küldhet felhő-eszközre küldött üzenetek az IoT hub a Pi.</span><span class="sxs-lookup"><span data-stu-id="daf3d-130">You can now send cloud-to-device messages from your IoT hub to Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="daf3d-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="daf3d-131">Next steps</span></span>
[<span data-ttu-id="daf3d-132">Futtassa a mintaalkalmazást a felhő-eszközre küldött üzenetek fogadására</span><span class="sxs-lookup"><span data-stu-id="daf3d-132">Run the sample application to receive cloud-to-device messages</span></span>](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)


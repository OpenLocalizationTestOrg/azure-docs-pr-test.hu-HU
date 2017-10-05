---
title: "Az IOT hubbal-explorer üzenetküldési Azure IoT Hub felhőalapú eszközök felügyelete |} Microsoft Docs"
description: "Megtudhatja, hogyan eszközzel az IOT hubbal-explorer CLI figyelő eszközre cloud (D2C) üzenetek és felhő küldeni az Azure IoT Hub eszköz (C2D) üzeneteket."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT hubbal explorer, üzenetküldés, iot hub felhő eszközre, az eszköz üzenetkezelés felhő felhőalapú eszköz"
ms.assetid: 04521658-35d3-4503-ae48-51d6ad3c62cc
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: 30151b7bdc544bc36e959cc3528d37897198fc7e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-iothub-explorer-to-send-and-receive-messages-between-your-device-and-iot-hub"></a><span data-ttu-id="485c5-104">Az eszköz és az IoT-központ között üzeneteket küldjön és fogadjon IOT hubbal-kezelő használatával</span><span class="sxs-lookup"><span data-stu-id="485c5-104">Use iothub-explorer to send and receive messages between your device and IoT Hub</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="485c5-106">[IOT hubbal-explorer](https://github.com/azure/iothub-explorer) rendelkezik, amely egyszerűbbé teszi az IoT-központ felügyeleti néhány olyan parancsok.</span><span class="sxs-lookup"><span data-stu-id="485c5-106">[iothub-explorer](https://github.com/azure/iothub-explorer) has a handful of commands that makes IoT Hub management easier.</span></span> <span data-ttu-id="485c5-107">Ez az oktatóanyag az IOT hubbal-kezelő használata az eszköz és az IoT hub között üzeneteket küldjön és fogadjon összpontosít.</span><span class="sxs-lookup"><span data-stu-id="485c5-107">This tutorial focuses on how to use iothub-explorer to send and receive messages between your device and your IoT hub.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="485c5-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="485c5-108">What you will learn</span></span>

<span data-ttu-id="485c5-109">Megismerheti, hogyan kezelővel IOT hubbal-eszköz a felhőbe küldött üzeneteket figyelése és a felhő-eszközre küldött üzenetek küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="485c5-109">You learn how to use iothub-explorer to monitor device-to-cloud messages and to send cloud-to-device messages.</span></span> <span data-ttu-id="485c5-110">Eszköz a felhőbe küldött üzeneteket érzékelőadatait, amely az eszköz összegyűjti, és ezután elküldi az IoT hub lehet.</span><span class="sxs-lookup"><span data-stu-id="485c5-110">Device-to-cloud messages could be sensor data that your device collects and then sends to your IoT hub.</span></span> <span data-ttu-id="485c5-111">Felhő-eszközre küldött üzenetek parancsokat, amelyek az IoT hub küld az eszközre, amely az eszköz csatlakozik LED villogni lehet.</span><span class="sxs-lookup"><span data-stu-id="485c5-111">Cloud-to-device messages could be commands that your IoT hub sends to your device to blink an LED that is connected to your device.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="485c5-112">Mit fog</span><span class="sxs-lookup"><span data-stu-id="485c5-112">What you will do</span></span>

- <span data-ttu-id="485c5-113">IOT hubbal-explorer segítségével figyelheti az eszköz a felhőbe küldött üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="485c5-113">Use iothub-explorer to monitor device-to-cloud messages.</span></span>
- <span data-ttu-id="485c5-114">IOT hubbal-explorer segítségével felhő eszközre üzenetek.</span><span class="sxs-lookup"><span data-stu-id="485c5-114">Use iothub-explorer to send cloud-to-device messages.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="485c5-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="485c5-115">What you need</span></span>

- <span data-ttu-id="485c5-116">Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve az alábbi követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="485c5-116">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  - <span data-ttu-id="485c5-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="485c5-117">An active Azure subscription.</span></span>
  - <span data-ttu-id="485c5-118">Az előfizetéshez tartozó Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="485c5-118">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="485c5-119">Egy ügyfélalkalmazást, amely üzeneteket küld az Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="485c5-119">A client application that sends messages to your Azure IoT hub.</span></span>
- <span data-ttu-id="485c5-120">IOT hubbal-explorer.</span><span class="sxs-lookup"><span data-stu-id="485c5-120">iothub-explorer.</span></span> <span data-ttu-id="485c5-121">([Telepítse az IOT hubbal-explorer](https://github.com/azure/iothub-explorer))</span><span class="sxs-lookup"><span data-stu-id="485c5-121">([Install iothub-explorer](https://github.com/azure/iothub-explorer))</span></span>

## <a name="monitor-device-to-cloud-messages"></a><span data-ttu-id="485c5-122">Eszköz a felhőbe küldött üzeneteket figyelése</span><span class="sxs-lookup"><span data-stu-id="485c5-122">Monitor device-to-cloud messages</span></span>

<span data-ttu-id="485c5-123">Az IoT hub az eszközről küldött üzenetek figyeléséhez, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="485c5-123">To monitor messages that are sent from your device to your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="485c5-124">Nyissa meg a konzol ablakot.</span><span class="sxs-lookup"><span data-stu-id="485c5-124">Open a console window.</span></span>
1. <span data-ttu-id="485c5-125">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="485c5-125">Run the following command:</span></span>

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > <span data-ttu-id="485c5-126">Első `<device-id>` és `<IoTHubConnectionString>` az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="485c5-126">Get `<device-id>` and `<IoTHubConnectionString>` from your IoT hub.</span></span> <span data-ttu-id="485c5-127">Győződjön meg arról, hogy az előző oktatóanyag befejezése után.</span><span class="sxs-lookup"><span data-stu-id="485c5-127">Make sure you've finished the previous tutorial.</span></span> <span data-ttu-id="485c5-128">Vagy próbálja meg használni `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` Ha `HostName`, `SharedAccessKeyName` és `SharedAccessKey`.</span><span class="sxs-lookup"><span data-stu-id="485c5-128">Or you can try to use `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` if you have `HostName`, `SharedAccessKeyName` and `SharedAccessKey`.</span></span>

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="485c5-129">Üzenetküldés a felhőből az eszközökre</span><span class="sxs-lookup"><span data-stu-id="485c5-129">Send cloud-to-device messages</span></span>

<span data-ttu-id="485c5-130">Az IoT hub egy üzenetet küldeni az eszközre, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="485c5-130">To send a message from your IoT hub to your device, follow these steps:</span></span>

1. <span data-ttu-id="485c5-131">Nyissa meg a konzol ablakot.</span><span class="sxs-lookup"><span data-stu-id="485c5-131">Open a console window.</span></span>
1. <span data-ttu-id="485c5-132">Az IoT hub a munkamenetet indítani a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="485c5-132">Start a session on your IoT hub by running the following command:</span></span>

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. <span data-ttu-id="485c5-133">Üzenet küldése az eszköz a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="485c5-133">Send a message to your device by running the following command:</span></span>

   ```bash
   iothub-explorer send <device-id> <message>
   ```

<span data-ttu-id="485c5-134">A parancs villogjon, amely az eszköz csatlakozik, és az üzenetet küld az eszköz a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="485c5-134">The command blinks the LED that is connected to your device and sends the message to your device.</span></span>

> [!Note]
> <span data-ttu-id="485c5-135">Nincs szükség az eszköz egy külön ack parancs küldése a az IoT hub, az üzenet fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="485c5-135">There is no need for the device to send a separate ack command back to your IoT hub upon receiving the message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="485c5-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="485c5-136">Next steps</span></span>

<span data-ttu-id="485c5-137">Hogy megismerte az eszköz a felhőbe küldött üzeneteket figyelése, és az IoT-eszközök és az Azure IoT-központ között felhő eszközre üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="485c5-137">You’ve learned how to monitor device-to-cloud messages and send cloud-to-device messages between your IoT device and Azure IoT Hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

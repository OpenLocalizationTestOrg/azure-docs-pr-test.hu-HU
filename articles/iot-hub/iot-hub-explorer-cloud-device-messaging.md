---
title: "az IOT hubbal-explorer üzenetküldési aaaManage Azure IoT Hub felhő eszköz |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello IOT hubbal-explorer CLI eszköz toomonitor toocloud (D2C) üzeneteket, és Azure IoT Hub felhő toodevice (C2D) üzenetek küldése."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT hubbal explorer, a felhő eszköz üzenetküldés, iot hub felhő toodevice, a felhő toodevice üzenetkezelés"
ms.assetid: 04521658-35d3-4503-ae48-51d6ad3c62cc
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: 741383b5631714cc2fef3309541fd2117aa7824c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-toosend-and-receive-messages-between-your-device-and-iot-hub"></a><span data-ttu-id="f3cd7-104">Használja az IOT hubbal-explorer toosend és az eszköz és az IoT-központ között üzeneteket fogadni</span><span class="sxs-lookup"><span data-stu-id="f3cd7-104">Use iothub-explorer toosend and receive messages between your device and IoT Hub</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="f3cd7-106">[IOT hubbal-explorer](https://github.com/azure/iothub-explorer) rendelkezik, amely egyszerűbbé teszi az IoT-központ felügyeleti néhány olyan parancsok.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-106">[iothub-explorer](https://github.com/azure/iothub-explorer) has a handful of commands that makes IoT Hub management easier.</span></span> <span data-ttu-id="f3cd7-107">Ez az oktatóanyag témája módja toouse IOT hubbal-explorer toosend és az eszköz és az IoT hub között üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-107">This tutorial focuses on how toouse iothub-explorer toosend and receive messages between your device and your IoT hub.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f3cd7-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="f3cd7-108">What you will learn</span></span>

<span data-ttu-id="f3cd7-109">Megismerheti, hogyan toouse IOT hubbal-explorer toomonitor eszközről a felhőbe üzenetek és toosend felhő eszközre üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-109">You learn how toouse iothub-explorer toomonitor device-to-cloud messages and toosend cloud-to-device messages.</span></span> <span data-ttu-id="f3cd7-110">Eszköz a felhőbe küldött üzeneteket lehet, hogy az eszköz összegyűjti, és ezután elküldi az IoT-központ tooyour érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-110">Device-to-cloud messages could be sensor data that your device collects and then sends tooyour IoT hub.</span></span> <span data-ttu-id="f3cd7-111">Felhő-eszközre küldött üzenetek oka az lehet, hogy az IoT hub küld tooyour eszköz tooblink, amelyek csatlakoztatott tooyour eszköz LED parancsok.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-111">Cloud-to-device messages could be commands that your IoT hub sends tooyour device tooblink an LED that is connected tooyour device.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="f3cd7-112">Mit fog</span><span class="sxs-lookup"><span data-stu-id="f3cd7-112">What you will do</span></span>

- <span data-ttu-id="f3cd7-113">Használja az IOT hubbal-explorer toomonitor eszköz-a-felhőbe küldött üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-113">Use iothub-explorer toomonitor device-to-cloud messages.</span></span>
- <span data-ttu-id="f3cd7-114">Használja az IOT hubbal-explorer toosend felhő eszközre üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-114">Use iothub-explorer toosend cloud-to-device messages.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f3cd7-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="f3cd7-115">What you need</span></span>

- <span data-ttu-id="f3cd7-116">Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve hello követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="f3cd7-116">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="f3cd7-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-117">An active Azure subscription.</span></span>
  - <span data-ttu-id="f3cd7-118">Az előfizetéshez tartozó Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-118">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="f3cd7-119">Egy ügyfélalkalmazás által küldött üzenetek tooyour Azure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-119">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="f3cd7-120">IOT hubbal-explorer.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-120">iothub-explorer.</span></span> <span data-ttu-id="f3cd7-121">([Telepítse az IOT hubbal-explorer](https://github.com/azure/iothub-explorer))</span><span class="sxs-lookup"><span data-stu-id="f3cd7-121">([Install iothub-explorer](https://github.com/azure/iothub-explorer))</span></span>

## <a name="monitor-device-to-cloud-messages"></a><span data-ttu-id="f3cd7-122">Eszköz a felhőbe küldött üzeneteket figyelése</span><span class="sxs-lookup"><span data-stu-id="f3cd7-122">Monitor device-to-cloud messages</span></span>

<span data-ttu-id="f3cd7-123">az eszköz tooyour IoT hub érkező üzenetek toomonitor kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f3cd7-123">toomonitor messages that are sent from your device tooyour IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="f3cd7-124">Nyissa meg a konzol ablakot.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-124">Open a console window.</span></span>
1. <span data-ttu-id="f3cd7-125">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="f3cd7-125">Run hello following command:</span></span>

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > <span data-ttu-id="f3cd7-126">Első `<device-id>` és `<IoTHubConnectionString>` az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-126">Get `<device-id>` and `<IoTHubConnectionString>` from your IoT hub.</span></span> <span data-ttu-id="f3cd7-127">Győződjön meg arról, hogy hello előző oktatóanyag befejezése után.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-127">Make sure you've finished hello previous tutorial.</span></span> <span data-ttu-id="f3cd7-128">Vagy megpróbálhatja toouse `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` Ha `HostName`, `SharedAccessKeyName` és `SharedAccessKey`.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-128">Or you can try toouse `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` if you have `HostName`, `SharedAccessKeyName` and `SharedAccessKey`.</span></span>

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="f3cd7-129">Üzenetküldés a felhőből az eszközökre</span><span class="sxs-lookup"><span data-stu-id="f3cd7-129">Send cloud-to-device messages</span></span>

<span data-ttu-id="f3cd7-130">egy üzenetet az IoT hub tooyour eszközről toosend kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f3cd7-130">toosend a message from your IoT hub tooyour device, follow these steps:</span></span>

1. <span data-ttu-id="f3cd7-131">Nyissa meg a konzol ablakot.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-131">Open a console window.</span></span>
1. <span data-ttu-id="f3cd7-132">Az IoT hub a munkamenet indításához hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f3cd7-132">Start a session on your IoT hub by running hello following command:</span></span>

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. <span data-ttu-id="f3cd7-133">Egy üzenet tooyour eszköz küldése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f3cd7-133">Send a message tooyour device by running hello following command:</span></span>

   ```bash
   iothub-explorer send <device-id> <message>
   ```

<span data-ttu-id="f3cd7-134">hello parancs villogjon-hello LED-jét, csatlakoztatott tooyour eszközt, és elküldi azt hello üzenet tooyour eszköz.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-134">hello command blinks hello LED that is connected tooyour device and sends hello message tooyour device.</span></span>

> [!Note]
> <span data-ttu-id="f3cd7-135">Nincs szükség a hello eszköz toosend egy külön ack parancs hátsó tooyour IoT-központ hello üzenet fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-135">There is no need for hello device toosend a separate ack command back tooyour IoT hub upon receiving hello message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3cd7-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3cd7-136">Next steps</span></span>

<span data-ttu-id="f3cd7-137">Hogy mér megismerte hogyan toomonitor eszköz-felhő üzeneteket, és az IoT-eszközök és az Azure IoT-központ között felhő eszközre üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="f3cd7-137">You’ve learned how toomonitor device-to-cloud messages and send cloud-to-device messages between your IoT device and Azure IoT Hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

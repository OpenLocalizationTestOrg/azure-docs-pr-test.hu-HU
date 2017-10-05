---
title: "Azure IoT Hub egyéni végpontokat megértése |} Microsoft Docs"
description: "Fejlesztői útmutató - útválasztási szabályok használatával egyéni végpontokkal való eszközről a felhőbe üzenetek továbbításához."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: a21f1c61f344f96e2e03422e41fd8c5f7f841a0c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a><span data-ttu-id="61e86-103">Üzenet útvonalak és egyéni végpontokat használja az eszköz a felhőbe küldött üzeneteket</span><span class="sxs-lookup"><span data-stu-id="61e86-103">Use message routes and custom endpoints for device-to-cloud messages</span></span>

<span data-ttu-id="61e86-104">Az IoT-központ lehetővé teszi a továbbításához [eszköz a felhőbe küldött üzeneteket] [ lnk-device-to-cloud] az IoT-központ szolgáltatás felé néző végpontok üzenet tulajdonságai alapján.</span><span class="sxs-lookup"><span data-stu-id="61e86-104">IoT Hub enables you to route [device-to-cloud messages][lnk-device-to-cloud] to IoT Hub service-facing endpoints based on message properties.</span></span> <span data-ttu-id="61e86-105">Útválasztási szabályokat csoportjai üzenetek küldéséhez, hová kell Ugrás a további szolgáltatások nélkül folyamat üzenetek vagy kód írása a rugalmasságot.</span><span class="sxs-lookup"><span data-stu-id="61e86-105">Routing rules give you the flexibility to send messages where they need to go without the need for additional services to process messages or to write additional code.</span></span> <span data-ttu-id="61e86-106">Egyes útválasztási szabályokat konfigurál a következő tulajdonságokkal rendelkeznek:</span><span class="sxs-lookup"><span data-stu-id="61e86-106">Each routing rule you configure has the following properties:</span></span>

| <span data-ttu-id="61e86-107">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="61e86-107">Property</span></span>      | <span data-ttu-id="61e86-108">Leírás</span><span class="sxs-lookup"><span data-stu-id="61e86-108">Description</span></span> |
| ------------- | ----------- |
| <span data-ttu-id="61e86-109">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="61e86-109">**Name**</span></span>      | <span data-ttu-id="61e86-110">A szabály azonosító egyedi név.</span><span class="sxs-lookup"><span data-stu-id="61e86-110">The unique name that identifies the rule.</span></span> |
| <span data-ttu-id="61e86-111">**Forrás**</span><span class="sxs-lookup"><span data-stu-id="61e86-111">**Source**</span></span>    | <span data-ttu-id="61e86-112">Az adatfolyamot kell bírálni a forrása.</span><span class="sxs-lookup"><span data-stu-id="61e86-112">The origin of the data stream to be acted upon.</span></span> <span data-ttu-id="61e86-113">Például telemetriát.</span><span class="sxs-lookup"><span data-stu-id="61e86-113">For example, device telemetry.</span></span> |
| <span data-ttu-id="61e86-114">**Az állapot**</span><span class="sxs-lookup"><span data-stu-id="61e86-114">**Condition**</span></span> | <span data-ttu-id="61e86-115">Az útválasztási szabály, amely az üzenet fejlécek és body és határozzák meg, hogy a végpont egyezés használt lekérdezési kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="61e86-115">The query expression for the routing rule that is run against the message's headers and body and used to determine whether it is a match for the endpoint.</span></span> <span data-ttu-id="61e86-116">Útvonal feltétel létrehozásával kapcsolatos további információkért tekintse meg a [referencia - lekérdezési nyelv eszköz twins és feladatok][lnk-devguide-query-language].</span><span class="sxs-lookup"><span data-stu-id="61e86-116">For more information about constructing a route condition, see the [Reference - query language for device twins and jobs][lnk-devguide-query-language].</span></span> |
| <span data-ttu-id="61e86-117">**Végpont**</span><span class="sxs-lookup"><span data-stu-id="61e86-117">**Endpoint**</span></span>  | <span data-ttu-id="61e86-118">A végpont, ahol az IoT-központ elküldi a feltételnek megfelelő üzenetek neve.</span><span class="sxs-lookup"><span data-stu-id="61e86-118">The name of the endpoint where IoT Hub sends messages that match the condition.</span></span> <span data-ttu-id="61e86-119">Végpontok az IoT hub ugyanabban a régióban kell lennie, ellenkező esetben meg kell felszámítani kereszt-régió írási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="61e86-119">Endpoints should be in the same region as the IoT hub, otherwise you may be charged for cross-region writes.</span></span> |

<span data-ttu-id="61e86-120">Egyetlen üzenet előfordulhat, hogy felel meg a feltétel több útválasztási szabályokat, amelyben eset az IoT-központ kézbesíti az üzenetet minden egyező szabályt társított végpont.</span><span class="sxs-lookup"><span data-stu-id="61e86-120">A single message may match the condition on multiple routing rules, in which case IoT Hub delivers the message to the endpoint associated with each matched rule.</span></span> <span data-ttu-id="61e86-121">Az IoT-központ is automatikusan deduplicates üzenetkézbesítést, így ha egy állapotüzenet megfelel a több szabályok azonos céllal rendelkező, csak írás célhoz egyszer.</span><span class="sxs-lookup"><span data-stu-id="61e86-121">IoT Hub also automatically deduplicates message delivery, so if a message matches multiple rules that all have the same destination, it is only written to that destination once.</span></span>

<span data-ttu-id="61e86-122">Az IoT-központ rendelkezik egy alapértelmezett [beépített végpont][lnk-built-in].</span><span class="sxs-lookup"><span data-stu-id="61e86-122">An IoT hub has a default [built-in endpoint][lnk-built-in].</span></span> <span data-ttu-id="61e86-123">Egyéni végpontokat üzeneteknek az előfizetésében szereplő más szolgáltatások létrehozhatja, ha szeretne az elosztóhoz hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="61e86-123">You can create custom endpoints to route messages to by linking other services in your subscription to the hub.</span></span> <span data-ttu-id="61e86-124">Az IoT-központ jelenleg Eseményközpontokhoz, a Service Bus-üzenetsorok és a Service Bus-üzenettémakörök egyéni végpontként.</span><span class="sxs-lookup"><span data-stu-id="61e86-124">IoT Hub currently supports Event Hubs, Service Bus queues, and Service Bus topics as custom endpoints.</span></span>

> [!WARNING]
> <span data-ttu-id="61e86-125">Service Bus-üzenetsorok és témakörök a **munkamenetek** vagy **ismétlődő észlelési** engedélyezve van az egyéni végpontok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="61e86-125">Service Bus queues and topics with **Sessions** or **Duplicate Detection** enabled are not supported as custom endpoints.</span></span>

<span data-ttu-id="61e86-126">Egyéni végpontokat az IoT hubon létrehozásával kapcsolatos további információkért lásd: [IoT-központok végpontjai][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="61e86-126">For more information about creating custom endpoints in IoT Hub, see [IoT Hub endpoints][lnk-devguide-endpoints].</span></span>

<span data-ttu-id="61e86-127">A egyéni végpontok olvasását kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="61e86-127">For more information about reading from custom endpoints, see:</span></span>

* <span data-ttu-id="61e86-128">A olvasásakor [az Event Hubs][lnk-getstarted-eh].</span><span class="sxs-lookup"><span data-stu-id="61e86-128">Reading from [Event Hubs][lnk-getstarted-eh].</span></span>
* <span data-ttu-id="61e86-129">A olvasásakor [Service Bus-üzenetsorok][lnk-getstarted-queue].</span><span class="sxs-lookup"><span data-stu-id="61e86-129">Reading from [Service Bus queues][lnk-getstarted-queue].</span></span>
* <span data-ttu-id="61e86-130">A olvasásakor [Service Bus-üzenettémakörök][lnk-getstarted-topic].</span><span class="sxs-lookup"><span data-stu-id="61e86-130">Reading from [Service Bus topics][lnk-getstarted-topic].</span></span>

### <a name="next-steps"></a><span data-ttu-id="61e86-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61e86-131">Next steps</span></span>

<span data-ttu-id="61e86-132">IoT-központok végpontjai kapcsolatos további információkért lásd: [IoT-központok végpontjai][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="61e86-132">For more information about IoT Hub endpoints, see [IoT Hub endpoints][lnk-devguide-endpoints].</span></span>

<span data-ttu-id="61e86-133">Útválasztási szabályok meghatározásához használja a lekérdezési nyelv kapcsolatos további információkért lásd: [IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási][lnk-devguide-query-language].</span><span class="sxs-lookup"><span data-stu-id="61e86-133">For more information about the query language you use to define routing rules, see [IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query-language].</span></span>

<span data-ttu-id="61e86-134">A [folyamat IoT Hub eszközről a felhőbe üzenetek útvonalak] [ lnk-d2c-tutorial] oktatóanyag bemutatja, hogyan használható az útválasztási szabályokat és az egyéni végpontokat.</span><span class="sxs-lookup"><span data-stu-id="61e86-134">The [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial shows you how to use routing rules and custom endpoints.</span></span>

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md

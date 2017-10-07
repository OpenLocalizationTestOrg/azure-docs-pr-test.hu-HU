---
title: "aaaUnderstand Azure IoT Hub egyéni végpontokat |} Microsoft Docs"
description: "Fejlesztői útmutató - szabályok alkalmazásával a útválasztási tooroute eszköz a felhőbe küldött üzeneteket toocustom végpontok."
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
ms.openlocfilehash: daa9cfb35d0853e316bbf469b034d4dadbd4e85d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a><span data-ttu-id="8f44b-103">Üzenet útvonalak és egyéni végpontokat használja az eszköz a felhőbe küldött üzeneteket</span><span class="sxs-lookup"><span data-stu-id="8f44b-103">Use message routes and custom endpoints for device-to-cloud messages</span></span>

<span data-ttu-id="8f44b-104">Az IoT-központ lehetővé teszi a tooroute [eszköz a felhőbe küldött üzeneteket] [ lnk-device-to-cloud] tooIoT központ szolgáltatás felé néző végpontok üzenet tulajdonságai alapján.</span><span class="sxs-lookup"><span data-stu-id="8f44b-104">IoT Hub enables you tooroute [device-to-cloud messages][lnk-device-to-cloud] tooIoT Hub service-facing endpoints based on message properties.</span></span> <span data-ttu-id="8f44b-105">Üzenetek, hová kell toogo hello nélkül további szolgáltatások tooprocess üzenetek vagy toowrite kód útválasztási szabályok adja meg azt a rugalmasságot toosend hello szüksége.</span><span class="sxs-lookup"><span data-stu-id="8f44b-105">Routing rules give you hello flexibility toosend messages where they need toogo without hello need for additional services tooprocess messages or toowrite additional code.</span></span> <span data-ttu-id="8f44b-106">Egyes útválasztási szabályokat konfigurál a következő tulajdonságai hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="8f44b-106">Each routing rule you configure has hello following properties:</span></span>

| <span data-ttu-id="8f44b-107">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8f44b-107">Property</span></span>      | <span data-ttu-id="8f44b-108">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f44b-108">Description</span></span> |
| ------------- | ----------- |
| <span data-ttu-id="8f44b-109">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="8f44b-109">**Name**</span></span>      | <span data-ttu-id="8f44b-110">hello egyedi nevet, amely azonosítja a hello szabály.</span><span class="sxs-lookup"><span data-stu-id="8f44b-110">hello unique name that identifies hello rule.</span></span> |
| <span data-ttu-id="8f44b-111">**Forrás**</span><span class="sxs-lookup"><span data-stu-id="8f44b-111">**Source**</span></span>    | <span data-ttu-id="8f44b-112">hello adatok származásának hello reakciót toobe adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="8f44b-112">hello origin of hello data stream toobe acted upon.</span></span> <span data-ttu-id="8f44b-113">Például telemetriát.</span><span class="sxs-lookup"><span data-stu-id="8f44b-113">For example, device telemetry.</span></span> |
| <span data-ttu-id="8f44b-114">**Az állapot**</span><span class="sxs-lookup"><span data-stu-id="8f44b-114">**Condition**</span></span> | <span data-ttu-id="8f44b-115">hello lekérdezési kifejezésben hello útválasztási szabály hello üzenet fejlécek és body futtatni, és a használt toodetermine, hogy hello végpont egyezés-e.</span><span class="sxs-lookup"><span data-stu-id="8f44b-115">hello query expression for hello routing rule that is run against hello message's headers and body and used toodetermine whether it is a match for hello endpoint.</span></span> <span data-ttu-id="8f44b-116">Útvonal feltétel létrehozásával kapcsolatos további információkért lásd: hello [referencia - lekérdezési nyelv eszköz twins és feladatok][lnk-devguide-query-language].</span><span class="sxs-lookup"><span data-stu-id="8f44b-116">For more information about constructing a route condition, see hello [Reference - query language for device twins and jobs][lnk-devguide-query-language].</span></span> |
| <span data-ttu-id="8f44b-117">**Végpont**</span><span class="sxs-lookup"><span data-stu-id="8f44b-117">**Endpoint**</span></span>  | <span data-ttu-id="8f44b-118">ahol az IoT-központ küldi hello feltételének üzenetek hello végpont hello nevét.</span><span class="sxs-lookup"><span data-stu-id="8f44b-118">hello name of hello endpoint where IoT Hub sends messages that match hello condition.</span></span> <span data-ttu-id="8f44b-119">Végpontok kell hello hello IoT hub és ugyanabban a régióban, ellenkező esetben akkor számítható a kereszt-régió írja.</span><span class="sxs-lookup"><span data-stu-id="8f44b-119">Endpoints should be in hello same region as hello IoT hub, otherwise you may be charged for cross-region writes.</span></span> |

<span data-ttu-id="8f44b-120">Egyetlen üzenet előfordulhat, hogy megfelelő hello feltétel több útválasztási szabályok eset az IoT-központ szolgáltatás nyújt hello üzenet toohello végponthoz társított minden egyező szabályt.</span><span class="sxs-lookup"><span data-stu-id="8f44b-120">A single message may match hello condition on multiple routing rules, in which case IoT Hub delivers hello message toohello endpoint associated with each matched rule.</span></span> <span data-ttu-id="8f44b-121">Az IoT-központ is automatikusan deduplicates üzenetkézbesítést, így ha egy állapotüzenet megfelel a több szabályok hello rendelkező ugyanarra a célgépre, csak írás toothat cél egyszer.</span><span class="sxs-lookup"><span data-stu-id="8f44b-121">IoT Hub also automatically deduplicates message delivery, so if a message matches multiple rules that all have hello same destination, it is only written toothat destination once.</span></span>

<span data-ttu-id="8f44b-122">Az IoT-központ rendelkezik egy alapértelmezett [beépített végpont][lnk-built-in].</span><span class="sxs-lookup"><span data-stu-id="8f44b-122">An IoT hub has a default [built-in endpoint][lnk-built-in].</span></span> <span data-ttu-id="8f44b-123">Egyéni végpontokat tooroute üzenetek tooby létrehozhatja, ha az előfizetés toohello központban más szolgáltatások is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="8f44b-123">You can create custom endpoints tooroute messages tooby linking other services in your subscription toohello hub.</span></span> <span data-ttu-id="8f44b-124">Az IoT-központ jelenleg Eseményközpontokhoz, a Service Bus-üzenetsorok és a Service Bus-üzenettémakörök egyéni végpontként.</span><span class="sxs-lookup"><span data-stu-id="8f44b-124">IoT Hub currently supports Event Hubs, Service Bus queues, and Service Bus topics as custom endpoints.</span></span>

> [!WARNING]
> <span data-ttu-id="8f44b-125">Service Bus-üzenetsorok és témakörök a **munkamenetek** vagy **ismétlődő észlelési** engedélyezve van az egyéni végpontok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="8f44b-125">Service Bus queues and topics with **Sessions** or **Duplicate Detection** enabled are not supported as custom endpoints.</span></span>

<span data-ttu-id="8f44b-126">Egyéni végpontokat az IoT hubon létrehozásával kapcsolatos további információkért lásd: [IoT-központok végpontjai][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="8f44b-126">For more information about creating custom endpoints in IoT Hub, see [IoT Hub endpoints][lnk-devguide-endpoints].</span></span>

<span data-ttu-id="8f44b-127">A egyéni végpontok olvasását kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="8f44b-127">For more information about reading from custom endpoints, see:</span></span>

* <span data-ttu-id="8f44b-128">A olvasásakor [az Event Hubs][lnk-getstarted-eh].</span><span class="sxs-lookup"><span data-stu-id="8f44b-128">Reading from [Event Hubs][lnk-getstarted-eh].</span></span>
* <span data-ttu-id="8f44b-129">A olvasásakor [Service Bus-üzenetsorok][lnk-getstarted-queue].</span><span class="sxs-lookup"><span data-stu-id="8f44b-129">Reading from [Service Bus queues][lnk-getstarted-queue].</span></span>
* <span data-ttu-id="8f44b-130">A olvasásakor [Service Bus-üzenettémakörök][lnk-getstarted-topic].</span><span class="sxs-lookup"><span data-stu-id="8f44b-130">Reading from [Service Bus topics][lnk-getstarted-topic].</span></span>

### <a name="next-steps"></a><span data-ttu-id="8f44b-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f44b-131">Next steps</span></span>

<span data-ttu-id="8f44b-132">IoT-központok végpontjai kapcsolatos további információkért lásd: [IoT-központok végpontjai][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="8f44b-132">For more information about IoT Hub endpoints, see [IoT Hub endpoints][lnk-devguide-endpoints].</span></span>

<span data-ttu-id="8f44b-133">Hello lekérdezési nyelv kapcsolatos további információk toodefine útválasztási szabályokat használ, tekintse meg [IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási][lnk-devguide-query-language].</span><span class="sxs-lookup"><span data-stu-id="8f44b-133">For more information about hello query language you use toodefine routing rules, see [IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query-language].</span></span>

<span data-ttu-id="8f44b-134">Hello [folyamat IoT Hub eszközről a felhőbe üzenetek útvonalak] [ lnk-d2c-tutorial] oktatóanyag bemutatja, hogyan toouse útválasztási szabályokat és egyéni végpontokat.</span><span class="sxs-lookup"><span data-stu-id="8f44b-134">hello [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial shows you how toouse routing rules and custom endpoints.</span></span>

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md

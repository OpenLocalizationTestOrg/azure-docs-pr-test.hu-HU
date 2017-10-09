---
title: "aaaUnderstand Azure IoT Hub eszközről a felhőbe üzenetküldési |} Microsoft Docs"
description: "Fejlesztői útmutató - hogyan toouse eszköz-felhő IoT-központ az üzenetküldési. A telemetriai adatokat, és nem telemtry adatok küldését, és útválasztási toodeliver üzenetek használatával kapcsolatos adatokat tartalmaz."
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
ms.openlocfilehash: 07dc8a6be747365c7efbc528ab2762b0d9790758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-device-to-cloud-messages-tooiot-hub"></a><span data-ttu-id="5ad37-104">Eszköz a felhőbe küldött üzeneteket tooIoT Hub küldése</span><span class="sxs-lookup"><span data-stu-id="5ad37-104">Send device-to-cloud messages tooIoT Hub</span></span>

<span data-ttu-id="5ad37-105">toosend idősorozat telemetriai adatokat és az eszközök tooyour megoldás háttérrendszeréhez, a riasztások a eszközről a felhőbe üzeneteket küldhet az eszköz tooyour IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="5ad37-105">toosend time-series telemetry and alerts from your devices tooyour solution back end, send device-to-cloud messages from your device tooyour IoT hub.</span></span> <span data-ttu-id="5ad37-106">Az IoT-központ által támogatott eszközről a felhőbe lehetőségeket tárgyalását lásd: [eszközről a felhőbe kommunikációs útmutatást][lnk-d2c-guidance].</span><span class="sxs-lookup"><span data-stu-id="5ad37-106">For a discussion of other device-to-cloud options supported by IoT Hub, see [Device-to-cloud communications guidance][lnk-d2c-guidance].</span></span>

<span data-ttu-id="5ad37-107">Egy eszköz felé néző végpont keresztül eszközről a felhőbe üzeneteket küld (**/devices/ {deviceId} / üzenetek/események**).</span><span class="sxs-lookup"><span data-stu-id="5ad37-107">You send device-to-cloud messages through a device-facing endpoint (**/devices/{deviceId}/messages/events**).</span></span> <span data-ttu-id="5ad37-108">Útválasztási szabályokat, majd az üzenetek tooone hello szolgáltatás felé néző végpontok irányításához az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5ad37-108">Routing rules then route your messages tooone of hello service-facing endpoints on your IoT hub.</span></span> <span data-ttu-id="5ad37-109">Útválasztási szabályok használata hello fejlécek és törzsének átfutó a központ toodetermine eszközről a felhőbe köszönőüzenetei ahol tooroute őket.</span><span class="sxs-lookup"><span data-stu-id="5ad37-109">Routing rules use hello headers and body of hello device-to-cloud messages flowing through your hub toodetermine where tooroute them.</span></span> <span data-ttu-id="5ad37-110">Üzenetek alapesetben irányított toohello beépített szolgáltatás felé néző végpont (**üzenetek/események**), amely kompatibilis a [Event Hubs][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="5ad37-110">By default, messages are routed toohello built-in service-facing endpoint (**messages/events**), that is compatible with [Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="5ad37-111">Ezért, használhatja a szabványos [Event Hubs-integrációval és SDK-k] [ lnk-compatible-endpoint] háttér tooreceive eszköz-a-felhőbe küldött üzeneteket a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="5ad37-111">Therefore, you can use standard [Event Hubs integration and SDKs][lnk-compatible-endpoint] tooreceive device-to-cloud messages in your solution back end.</span></span>

<span data-ttu-id="5ad37-112">Az IoT-központ megvalósítja az eszközről a felhőbe üzenetküldési adatfolyam üzenetkezelési minta használatával.</span><span class="sxs-lookup"><span data-stu-id="5ad37-112">IoT Hub implements device-to-cloud messaging using a streaming messaging pattern.</span></span> <span data-ttu-id="5ad37-113">Az IoT-központ eszközről a felhőbe üzenetek mint [Event Hubs] [ lnk-event-hubs] *események* mint [Service Bus] [ lnk-servicebus] *üzenetek* abban, hogy nagyszámú események, hogy át kellene haladnia hello szolgáltatás, amely több olvasók által is olvasható.</span><span class="sxs-lookup"><span data-stu-id="5ad37-113">IoT Hub's device-to-cloud messages are more like [Event Hubs][lnk-event-hubs] *events* than [Service Bus][lnk-servicebus] *messages* in that there is a high volume of events passing through hello service that can be read by multiple readers.</span></span>

<span data-ttu-id="5ad37-114">Eszköz-felhő IoT-központ az üzenetküldési hello a következő jellemzőkkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="5ad37-114">Device-to-cloud messaging with IoT Hub has hello following characteristics:</span></span>

* <span data-ttu-id="5ad37-115">Eszköz a felhőbe küldött üzeneteket a következők tartós és az IoT-központ alapértelmezett megtartott **üzenetek/események** tooseven nap fel végpontot.</span><span class="sxs-lookup"><span data-stu-id="5ad37-115">Device-to-cloud messages are durable and retained in an IoT hub's default **messages/events** endpoint for up tooseven days.</span></span>
* <span data-ttu-id="5ad37-116">Eszköz a felhőbe küldött üzeneteket legfeljebb 256 KB, és toooptimize küld kötegekben csoportosíthatók.</span><span class="sxs-lookup"><span data-stu-id="5ad37-116">Device-to-cloud messages can be at most 256 KB, and can be grouped in batches toooptimize sends.</span></span> <span data-ttu-id="5ad37-117">Kötegek legfeljebb 256 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="5ad37-117">Batches can be at most 256 KB.</span></span>
* <span data-ttu-id="5ad37-118">Hello leírtak [vezérlő hozzáférés tooIoT Hub] [ lnk-devguide-security] szakaszban, IoT-központ lehetővé teszi, hogy a eszközönkénti hitelesítési és hozzáférés-vezérlés.</span><span class="sxs-lookup"><span data-stu-id="5ad37-118">As explained in hello [Control access tooIoT Hub][lnk-devguide-security] section, IoT Hub enables per-device authentication and access control.</span></span>
* <span data-ttu-id="5ad37-119">Az IoT-központ lehetővé teszi a toocreate too10 egyéni végpontokat fel.</span><span class="sxs-lookup"><span data-stu-id="5ad37-119">IoT Hub allows you toocreate up too10 custom endpoints.</span></span> <span data-ttu-id="5ad37-120">Üzenetek kézbesítési útvonalakat az IoT hub konfigurált alapján toohello végpontok.</span><span class="sxs-lookup"><span data-stu-id="5ad37-120">Messages are delivered toohello endpoints based on routes configured on your IoT hub.</span></span> <span data-ttu-id="5ad37-121">További információkért lásd: [útválasztási szabályok](#routing-rules).</span><span class="sxs-lookup"><span data-stu-id="5ad37-121">For more information, see [Routing rules](#routing-rules).</span></span>
* <span data-ttu-id="5ad37-122">Az IoT-központ lehetővé teszi, hogy egyidejűleg csatlakoztatott eszközök millióira (lásd: [kvóták és sávszélesség-szabályozási][lnk-quotas]).</span><span class="sxs-lookup"><span data-stu-id="5ad37-122">IoT Hub enables millions of simultaneously connected devices (see [Quotas and throttling][lnk-quotas]).</span></span>
* <span data-ttu-id="5ad37-123">Az IoT-központ nem engedélyezi a tetszőleges particionálást.</span><span class="sxs-lookup"><span data-stu-id="5ad37-123">IoT Hub does not allow arbitrary partitioning.</span></span> <span data-ttu-id="5ad37-124">Eszköz a felhőbe küldött üzeneteket particionáltak, azok származó alapján **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="5ad37-124">Device-to-cloud messages are partitioned based on their originating **deviceId**.</span></span>

<span data-ttu-id="5ad37-125">Hello IoT-központ és az Event Hubs szolgáltatások hello különbségei kapcsolatos további információkért lásd: [összehasonlítása az Azure IoT-központ és az Azure Event Hubs][lnk-comparison].</span><span class="sxs-lookup"><span data-stu-id="5ad37-125">For more information about hello differences between hello IoT Hub and Event Hubs services, see [Comparison of Azure IoT Hub and Azure Event Hubs][lnk-comparison].</span></span>

## <a name="send-non-telemetry-traffic"></a><span data-ttu-id="5ad37-126">Nem telemetriai forgalom küldése</span><span class="sxs-lookup"><span data-stu-id="5ad37-126">Send non-telemetry traffic</span></span>

<span data-ttu-id="5ad37-127">Gyakran továbbá tootelemetry adatpontok, eszközök üzeneteket küldhet, és külön végrehajtási és kezelését a hello megoldás háttérrendszerének igénylő kérelmek.</span><span class="sxs-lookup"><span data-stu-id="5ad37-127">Often, in addition tootelemetry data points, devices send messages and requests that require separate execution and handling in hello solution back end.</span></span> <span data-ttu-id="5ad37-128">Például egy adott művelet hello kell kiváltani a kritikus riasztások háttér.</span><span class="sxs-lookup"><span data-stu-id="5ad37-128">For example, critical alerts that must trigger a specific action in hello back end.</span></span> <span data-ttu-id="5ad37-129">A felhasználó egyszerűen készíthet egy [útválasztási szabály] [ lnk-devguide-custom] toosend ilyen típusú üzenetek tooan végpont dedikált tootheir feldolgozási üdvözlőüzenetére vagy a fejlécet vagy hello üzenettörzsbe érték alapján.</span><span class="sxs-lookup"><span data-stu-id="5ad37-129">You can easily write a [routing rule][lnk-devguide-custom] toosend these types of messages tooan endpoint dedicated tootheir processing based on either a header on hello message or a value in hello message body.</span></span>

<span data-ttu-id="5ad37-130">Hello legjobb módja tooprocess a kind üzenet kapcsolatos további információkért lásd: hello [oktatóanyag: hogyan tooprocess IoT Hub eszköz-a-felhőbe küldött üzeneteket] [ lnk-d2c-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="5ad37-130">For more information about hello best way tooprocess this kind of message, see hello [Tutorial: How tooprocess IoT Hub device-to-cloud messages][lnk-d2c-tutorial] tutorial.</span></span>

## <a name="route-device-to-cloud-messages"></a><span data-ttu-id="5ad37-131">Eszköz-felhő üzenetek</span><span class="sxs-lookup"><span data-stu-id="5ad37-131">Route device-to-cloud messages</span></span>

<span data-ttu-id="5ad37-132">Az útválasztási eszköz a felhőbe küldött üzeneteket tooyour háttér-alkalmazások két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="5ad37-132">You have two options for routing device-to-cloud messages tooyour back-end apps:</span></span>

* <span data-ttu-id="5ad37-133">Használja a hello beépített [Event Hub-kompatibilis végpont] [ lnk-compatible-endpoint] tooenable háttér-alkalmazások tooread hello eszközről a felhőbe által fogadott üzenetek hello központ.</span><span class="sxs-lookup"><span data-stu-id="5ad37-133">Use hello built-in [Event Hub-compatible endpoint][lnk-compatible-endpoint] tooenable back-end apps tooread hello device-to-cloud messages received by hello hub.</span></span> <span data-ttu-id="5ad37-134">toolearn hello beépített Event Hub-kompatibilis végpont kapcsolatban lásd: [eszköz a felhőbe küldött üzeneteket beolvasni hello beépített végpont][lnk-devguide-builtin].</span><span class="sxs-lookup"><span data-stu-id="5ad37-134">toolearn about hello built-in Event Hub-compatible endpoint, see [Read device-to-cloud messages from hello built-in endpoint][lnk-devguide-builtin].</span></span>
* <span data-ttu-id="5ad37-135">Az IoT hub útválasztási szabályok toosend üzenetek toocustom végpontok használja.</span><span class="sxs-lookup"><span data-stu-id="5ad37-135">Use routing rules toosend messages toocustom endpoints in your IoT hub.</span></span> <span data-ttu-id="5ad37-136">Egyéni végpontokat engedélyezése a háttér-alkalmazások tooread eszközről a felhőbe az Event Hubs, a Service Bus-üzenetsorok vagy a Service Bus-üzenettémakörök használata.</span><span class="sxs-lookup"><span data-stu-id="5ad37-136">Custom endpoints enable your back-end apps tooread device-to-cloud messages using Event Hubs, Service Bus queues, or Service Bus topics.</span></span> <span data-ttu-id="5ad37-137">útválasztási és egyéni végpont, kapcsolatos toolearn lásd [egyéni végpontokat és útválasztási szabályokat használja az eszköz a felhőbe küldött üzeneteket][lnk-devguide-custom].</span><span class="sxs-lookup"><span data-stu-id="5ad37-137">toolearn about routing and custom endpoints, see [Use custom endpoints and routing rules for device-to-cloud messages][lnk-devguide-custom].</span></span>

## <a name="anti-spoofing-properties"></a><span data-ttu-id="5ad37-138">Hamisításszűrés tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="5ad37-138">Anti-spoofing properties</span></span>

<span data-ttu-id="5ad37-139">az eszközről a felhőbe üzenetekben, IoT-központ címhamisítást következő tulajdonságai hello üzenetek bélyegzők tooavoid eszköz:</span><span class="sxs-lookup"><span data-stu-id="5ad37-139">tooavoid device spoofing in device-to-cloud messages, IoT Hub stamps all messages with hello following properties:</span></span>

* <span data-ttu-id="5ad37-140">**ConnectionDeviceId**</span><span class="sxs-lookup"><span data-stu-id="5ad37-140">**ConnectionDeviceId**</span></span>
* <span data-ttu-id="5ad37-141">**ConnectionDeviceGenerationId**</span><span class="sxs-lookup"><span data-stu-id="5ad37-141">**ConnectionDeviceGenerationId**</span></span>
* <span data-ttu-id="5ad37-142">**ConnectionAuthMethod**</span><span class="sxs-lookup"><span data-stu-id="5ad37-142">**ConnectionAuthMethod**</span></span>

<span data-ttu-id="5ad37-143">hello először két tartalmaznia hello **deviceId** és **generationId** az eszköz, származó megfelelően hello [identitás eszköztulajdonságok][lnk-device-properties].</span><span class="sxs-lookup"><span data-stu-id="5ad37-143">hello first two contain hello **deviceId** and **generationId** of hello originating device, as per [Device identity properties][lnk-device-properties].</span></span>

<span data-ttu-id="5ad37-144">Hello **ConnectionAuthMethod** tulajdonság egy szerializált JSON-objektum tartalmazza a következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="5ad37-144">hello **ConnectionAuthMethod** property contains a JSON serialized object, with hello following properties:</span></span>

```json
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a><span data-ttu-id="5ad37-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ad37-145">Next steps</span></span>

<span data-ttu-id="5ad37-146">Információ a hello SDK-k használata a toosend eszköz-a-felhőbe küldött üzeneteket, hogy [Azure IoT SDK-k][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="5ad37-146">For information about hello SDKs you can use toosend device-to-cloud messages, see [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="5ad37-147">Hello [Ismerkedés] [ lnk-get-started] oktatóprogramok bemutatják, hogyan toosend eszközről-a-felhőbe a szimulált és a fizikai eszközök érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="5ad37-147">hello [Get Started][lnk-get-started] tutorials show you how toosend device-to-cloud messages from both simulated and physical devices.</span></span> <span data-ttu-id="5ad37-148">További részletekért lásd: hello [folyamat IoT Hub eszközről a felhőbe üzenetek útvonalak] [ lnk-d2c-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="5ad37-148">For more detail, see hello [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial.</span></span>

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: iot-hub-get-started.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

---
title: "aaaAzure IoT-központ méretezése |} Microsoft Docs"
description: "Hogyan tooscale az IoT hub toosupport a várható üzenet átviteli sebességet. Az egyes rétegekhez hello támogatott átviteli és horizontális skálázási beállításokat összegzését tartalmazza."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3b8bf6c44631c65b34b69752d9043c21db24bb01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-your-iot-hub-solution"></a><span data-ttu-id="0a825-104">Az IoT hub megoldás méretezése</span><span class="sxs-lookup"><span data-stu-id="0a825-104">Scale your IoT hub solution</span></span>
<span data-ttu-id="0a825-105">Az Azure IoT Hub tooa millió egyidejűleg csatlakoztatott eszközöket támogatja.</span><span class="sxs-lookup"><span data-stu-id="0a825-105">Azure IoT Hub can support up tooa million simultaneously connected devices.</span></span> <span data-ttu-id="0a825-106">További információkért lásd: [árképzési IoT-központ][lnk-pricing].</span><span class="sxs-lookup"><span data-stu-id="0a825-106">For more information, see [IoT Hub pricing][lnk-pricing].</span></span> <span data-ttu-id="0a825-107">IoT Hub-egységenként lehetővé teszi, hogy egy bizonyos napi üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="0a825-107">Each IoT Hub unit allows a certain number of daily messages.</span></span>

<span data-ttu-id="0a825-108">tooproperly a megoldás méretezése, fontolja meg az IoT-központ alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="0a825-108">tooproperly scale your solution, consider your particular use of IoT Hub.</span></span> <span data-ttu-id="0a825-109">Különösen vegye figyelembe a következő kategóriák műveletek hello szükséges hello maximális átviteli sebesség:</span><span class="sxs-lookup"><span data-stu-id="0a825-109">In particular, consider hello required peak throughput for hello following categories of operations:</span></span>

* <span data-ttu-id="0a825-110">Az eszközről a felhőbe irányuló üzenetek</span><span class="sxs-lookup"><span data-stu-id="0a825-110">Device-to-cloud messages</span></span>
* <span data-ttu-id="0a825-111">Felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="0a825-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="0a825-112">Identitásjegyzék műveletei</span><span class="sxs-lookup"><span data-stu-id="0a825-112">Identity registry operations</span></span>

<span data-ttu-id="0a825-113">Toothis átviteli tájékoztatást, a következő látható: [IoT-központ kvóták és szabályozások] [ IoT Hub quotas and throttles] és ennek megfelelően a megoldás megtervezése.</span><span class="sxs-lookup"><span data-stu-id="0a825-113">In addition toothis throughput information, see [IoT Hub quotas and throttles][IoT Hub quotas and throttles] and design your solution accordingly.</span></span>

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a><span data-ttu-id="0a825-114">Eszköz-felhő és a felhő eszközre üzenet átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="0a825-114">Device-to-cloud and cloud-to-device message throughput</span></span>
<span data-ttu-id="0a825-115">hello legjobb módja toosize egy IoT-központ megoldás tooevaluate hello forgalmat egy egységenként.</span><span class="sxs-lookup"><span data-stu-id="0a825-115">hello best way toosize an IoT Hub solution is tooevaluate hello traffic on a per-unit basis.</span></span>

<span data-ttu-id="0a825-116">Eszköz a felhőbe küldött üzeneteket az alábbiakra fenntartható.</span><span class="sxs-lookup"><span data-stu-id="0a825-116">Device-to-cloud messages follow these sustained throughput guidelines.</span></span>

| <span data-ttu-id="0a825-117">Szint</span><span class="sxs-lookup"><span data-stu-id="0a825-117">Tier</span></span> | <span data-ttu-id="0a825-118">Fenntartható</span><span class="sxs-lookup"><span data-stu-id="0a825-118">Sustained throughput</span></span> | <span data-ttu-id="0a825-119">Tartós küldések</span><span class="sxs-lookup"><span data-stu-id="0a825-119">Sustained send rate</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0a825-120">S1</span><span class="sxs-lookup"><span data-stu-id="0a825-120">S1</span></span> |<span data-ttu-id="0a825-121">Too1111 KB/perc egységenként mentése</span><span class="sxs-lookup"><span data-stu-id="0a825-121">Up too1111 KB/minute per unit</span></span><br/><span data-ttu-id="0a825-122">(1,5 GB/nap/egység)</span><span class="sxs-lookup"><span data-stu-id="0a825-122">(1.5 GB/day/unit)</span></span> |<span data-ttu-id="0a825-123">278 üzenetek/másodperc egységenként átlaga</span><span class="sxs-lookup"><span data-stu-id="0a825-123">Average of 278 messages/minute per unit</span></span><br/><span data-ttu-id="0a825-124">(400 000 üzenetek/nap / egység)</span><span class="sxs-lookup"><span data-stu-id="0a825-124">(400,000 messages/day per unit)</span></span> |
| <span data-ttu-id="0a825-125">S2</span><span class="sxs-lookup"><span data-stu-id="0a825-125">S2</span></span> |<span data-ttu-id="0a825-126">Too16 MB percenként egységenként mentése</span><span class="sxs-lookup"><span data-stu-id="0a825-126">Up too16 MB/minute per unit</span></span><br/><span data-ttu-id="0a825-127">(22.8 GB/nap/egység)</span><span class="sxs-lookup"><span data-stu-id="0a825-127">(22.8 GB/day/unit)</span></span> |<span data-ttu-id="0a825-128">4,167 üzenetek/másodperc egységenként átlaga</span><span class="sxs-lookup"><span data-stu-id="0a825-128">Average of 4,167 messages/minute per unit</span></span><br/><span data-ttu-id="0a825-129">(6 millió üzenetek/nap / egység)</span><span class="sxs-lookup"><span data-stu-id="0a825-129">(6 million messages/day per unit)</span></span> |
| <span data-ttu-id="0a825-130">S3</span><span class="sxs-lookup"><span data-stu-id="0a825-130">S3</span></span> |<span data-ttu-id="0a825-131">Too814 MB percenként egységenként mentése</span><span class="sxs-lookup"><span data-stu-id="0a825-131">Up too814 MB/minute per unit</span></span><br/><span data-ttu-id="0a825-132">(1144.4 GB/nap/egység)</span><span class="sxs-lookup"><span data-stu-id="0a825-132">(1144.4 GB/day/unit)</span></span> |<span data-ttu-id="0a825-133">208,333 üzenetek/másodperc egységenként átlaga</span><span class="sxs-lookup"><span data-stu-id="0a825-133">Average of 208,333 messages/minute per unit</span></span><br/><span data-ttu-id="0a825-134">(300 millió üzenetek/nap / egység)</span><span class="sxs-lookup"><span data-stu-id="0a825-134">(300 million messages/day per unit)</span></span> |

## <a name="identity-registry-operation-throughput"></a><span data-ttu-id="0a825-135">Identitás beállításjegyzék művelet átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="0a825-135">Identity registry operation throughput</span></span>
<span data-ttu-id="0a825-136">Az IoT-központ identitás kapcsolatos műveletek nem kellene toobe futásidejű műveletek, mivel ezek többnyire kapcsolódó toodevice kiépítés.</span><span class="sxs-lookup"><span data-stu-id="0a825-136">IoT Hub identity registry operations are not supposed toobe run-time operations, as they are mostly related toodevice provisioning.</span></span>

<span data-ttu-id="0a825-137">Adott kapacitásnövelés számok, lásd: [IoT-központ kvóták és szabályozások][IoT Hub quotas and throttles].</span><span class="sxs-lookup"><span data-stu-id="0a825-137">For specific burst performance numbers, see [IoT Hub quotas and throttles][IoT Hub quotas and throttles].</span></span>

## <a name="sharding"></a><span data-ttu-id="0a825-138">Horizontális</span><span class="sxs-lookup"><span data-stu-id="0a825-138">Sharding</span></span>
<span data-ttu-id="0a825-139">Amíg egy egyetlen IoT hub méretezhető toomillions az eszközök, a megoldás specifikus teljesítményt nyújt, amely egyetlen IoT-központ nem garantálható, hogy szükség.</span><span class="sxs-lookup"><span data-stu-id="0a825-139">While a single IoT hub can scale toomillions of devices, sometimes your solution requires specific performance characteristics that a single IoT hub cannot guarantee.</span></span> <span data-ttu-id="0a825-140">Ebben az esetben ajánlott, hogy az IoT-központok több partíció az eszközök.</span><span class="sxs-lookup"><span data-stu-id="0a825-140">In that case, it is recommended that you partition your devices into multiple IoT hubs.</span></span> <span data-ttu-id="0a825-141">Több IoT-központok forgalom felszakadásáig sima, és szerezze be a szükséges átviteli hello vagy művelet díjszabás szükséges.</span><span class="sxs-lookup"><span data-stu-id="0a825-141">Multiple IoT hubs smooth traffic bursts and obtain hello required throughput or operation rates that are required.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a825-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a825-142">Next steps</span></span>
<span data-ttu-id="0a825-143">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="0a825-143">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="0a825-144">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="0a825-144">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="0a825-145">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="0a825-145">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

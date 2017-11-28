---
title: "Az Azure IoT-központ méretezése |} Microsoft Docs"
description: "A várható üzenet átviteli támogatásához az IoT hub méretezési módját. Az egyes rétegekhez a támogatott átviteli sebesség és a horizontális skálázási beállításokat összegzését tartalmazza."
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
ms.openlocfilehash: 2cb263103da05b10c24aab71d81c43eb25987565
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="scale-your-iot-hub-solution"></a><span data-ttu-id="0b4a1-104">Az IoT hub megoldás méretezése</span><span class="sxs-lookup"><span data-stu-id="0b4a1-104">Scale your IoT hub solution</span></span>
<span data-ttu-id="0b4a1-105">Az Azure IoT Hub legfeljebb egy millió egyidejűleg csatlakoztatott eszközt támogathat.</span><span class="sxs-lookup"><span data-stu-id="0b4a1-105">Azure IoT Hub can support up to a million simultaneously connected devices.</span></span> <span data-ttu-id="0b4a1-106">További információkért lásd: [árképzési IoT-központ][lnk-pricing].</span><span class="sxs-lookup"><span data-stu-id="0b4a1-106">For more information, see [IoT Hub pricing][lnk-pricing].</span></span> <span data-ttu-id="0b4a1-107">IoT Hub-egységenként lehetővé teszi, hogy egy bizonyos napi üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="0b4a1-107">Each IoT Hub unit allows a certain number of daily messages.</span></span>

<span data-ttu-id="0b4a1-108">A megoldás megfelelően méretezhető, fontolja meg az IoT-központ alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="0b4a1-108">To properly scale your solution, consider your particular use of IoT Hub.</span></span> <span data-ttu-id="0b4a1-109">Különösen fontolja meg a szükséges maximális átviteli sebesség műveletek az alábbi kategóriákban:</span><span class="sxs-lookup"><span data-stu-id="0b4a1-109">In particular, consider the required peak throughput for the following categories of operations:</span></span>

* <span data-ttu-id="0b4a1-110">Az eszközről a felhőbe irányuló üzenetek</span><span class="sxs-lookup"><span data-stu-id="0b4a1-110">Device-to-cloud messages</span></span>
* <span data-ttu-id="0b4a1-111">Felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="0b4a1-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="0b4a1-112">Identitásjegyzék műveletei</span><span class="sxs-lookup"><span data-stu-id="0b4a1-112">Identity registry operations</span></span>

<span data-ttu-id="0b4a1-113">Lásd: az átviteli információk mellett [IoT-központ kvóták és szabályozások] [ IoT Hub quotas and throttles] és ennek megfelelően a megoldás megtervezése.</span><span class="sxs-lookup"><span data-stu-id="0b4a1-113">In addition to this throughput information, see [IoT Hub quotas and throttles][IoT Hub quotas and throttles] and design your solution accordingly.</span></span>

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a><span data-ttu-id="0b4a1-114">Eszköz-felhő és a felhő eszközre üzenet átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="0b4a1-114">Device-to-cloud and cloud-to-device message throughput</span></span>
<span data-ttu-id="0b4a1-115">A legjobb módszer a következő méretre egy IoT-központ megoldás a forgalom egy egységenként kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="0b4a1-115">The best way to size an IoT Hub solution is to evaluate the traffic on a per-unit basis.</span></span>

<span data-ttu-id="0b4a1-116">Eszköz a felhőbe küldött üzeneteket az alábbiakra fenntartható.</span><span class="sxs-lookup"><span data-stu-id="0b4a1-116">Device-to-cloud messages follow these sustained throughput guidelines.</span></span>

| <span data-ttu-id="0b4a1-117">Szint</span><span class="sxs-lookup"><span data-stu-id="0b4a1-117">Tier</span></span> | <span data-ttu-id="0b4a1-118">Fenntartható</span><span class="sxs-lookup"><span data-stu-id="0b4a1-118">Sustained throughput</span></span> | <span data-ttu-id="0b4a1-119">Tartós küldések</span><span class="sxs-lookup"><span data-stu-id="0b4a1-119">Sustained send rate</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0b4a1-120">S1</span><span class="sxs-lookup"><span data-stu-id="0b4a1-120">S1</span></span> |<span data-ttu-id="0b4a1-121">1111 KB percenként egységenként legfeljebb</span><span class="sxs-lookup"><span data-stu-id="0b4a1-121">Up to 1111 KB/minute per unit</span></span><br/><span data-ttu-id="0b4a1-122">(1,5 GB/nap/egység)</span><span class="sxs-lookup"><span data-stu-id="0b4a1-122">(1.5 GB/day/unit)</span></span> |<span data-ttu-id="0b4a1-123">278 üzenetek/másodperc egységenként átlaga</span><span class="sxs-lookup"><span data-stu-id="0b4a1-123">Average of 278 messages/minute per unit</span></span><br/><span data-ttu-id="0b4a1-124">(400 000 üzenetek/nap / egység)</span><span class="sxs-lookup"><span data-stu-id="0b4a1-124">(400,000 messages/day per unit)</span></span> |
| <span data-ttu-id="0b4a1-125">S2</span><span class="sxs-lookup"><span data-stu-id="0b4a1-125">S2</span></span> |<span data-ttu-id="0b4a1-126">Legfeljebb 16 MB percenként egység</span><span class="sxs-lookup"><span data-stu-id="0b4a1-126">Up to 16 MB/minute per unit</span></span><br/><span data-ttu-id="0b4a1-127">(22.8 GB/nap/egység)</span><span class="sxs-lookup"><span data-stu-id="0b4a1-127">(22.8 GB/day/unit)</span></span> |<span data-ttu-id="0b4a1-128">4,167 üzenetek/másodperc egységenként átlaga</span><span class="sxs-lookup"><span data-stu-id="0b4a1-128">Average of 4,167 messages/minute per unit</span></span><br/><span data-ttu-id="0b4a1-129">(6 millió üzenetek/nap / egység)</span><span class="sxs-lookup"><span data-stu-id="0b4a1-129">(6 million messages/day per unit)</span></span> |
| <span data-ttu-id="0b4a1-130">S3</span><span class="sxs-lookup"><span data-stu-id="0b4a1-130">S3</span></span> |<span data-ttu-id="0b4a1-131">814 MB percenként egységenként legfeljebb</span><span class="sxs-lookup"><span data-stu-id="0b4a1-131">Up to 814 MB/minute per unit</span></span><br/><span data-ttu-id="0b4a1-132">(1144.4 GB/nap/egység)</span><span class="sxs-lookup"><span data-stu-id="0b4a1-132">(1144.4 GB/day/unit)</span></span> |<span data-ttu-id="0b4a1-133">208,333 üzenetek/másodperc egységenként átlaga</span><span class="sxs-lookup"><span data-stu-id="0b4a1-133">Average of 208,333 messages/minute per unit</span></span><br/><span data-ttu-id="0b4a1-134">(300 millió üzenetek/nap / egység)</span><span class="sxs-lookup"><span data-stu-id="0b4a1-134">(300 million messages/day per unit)</span></span> |

## <a name="identity-registry-operation-throughput"></a><span data-ttu-id="0b4a1-135">Identitás beállításjegyzék művelet átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="0b4a1-135">Identity registry operation throughput</span></span>
<span data-ttu-id="0b4a1-136">IoT Hub identitás kapcsolatos műveletek nem elvileg futásidejű műveletek, mivel ezek többnyire kapcsolódó eszközök kiépítését.</span><span class="sxs-lookup"><span data-stu-id="0b4a1-136">IoT Hub identity registry operations are not supposed to be run-time operations, as they are mostly related to device provisioning.</span></span>

<span data-ttu-id="0b4a1-137">Adott kapacitásnövelés számok, lásd: [IoT-központ kvóták és szabályozások][IoT Hub quotas and throttles].</span><span class="sxs-lookup"><span data-stu-id="0b4a1-137">For specific burst performance numbers, see [IoT Hub quotas and throttles][IoT Hub quotas and throttles].</span></span>

## <a name="sharding"></a><span data-ttu-id="0b4a1-138">Horizontális</span><span class="sxs-lookup"><span data-stu-id="0b4a1-138">Sharding</span></span>
<span data-ttu-id="0b4a1-139">Egyetlen IoT-központ eszközök millióira méretezhetők, amíg a megoldás specifikus teljesítményt nyújt, amely egyetlen IoT-központ nem garantálható, hogy szükség.</span><span class="sxs-lookup"><span data-stu-id="0b4a1-139">While a single IoT hub can scale to millions of devices, sometimes your solution requires specific performance characteristics that a single IoT hub cannot guarantee.</span></span> <span data-ttu-id="0b4a1-140">Ebben az esetben ajánlott, hogy az IoT-központok több partíció az eszközök.</span><span class="sxs-lookup"><span data-stu-id="0b4a1-140">In that case, it is recommended that you partition your devices into multiple IoT hubs.</span></span> <span data-ttu-id="0b4a1-141">Több IoT-központok forgalom felszakadásáig sima, és szerezze be a szükséges átviteli vagy a művelet díjszabás szükséges.</span><span class="sxs-lookup"><span data-stu-id="0b4a1-141">Multiple IoT hubs smooth traffic bursts and obtain the required throughput or operation rates that are required.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b4a1-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b4a1-142">Next steps</span></span>
<span data-ttu-id="0b4a1-143">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="0b4a1-143">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="0b4a1-144">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="0b4a1-144">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="0b4a1-145">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="0b4a1-145">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

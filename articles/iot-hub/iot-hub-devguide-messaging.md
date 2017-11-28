---
title: "aaaUnderstand Azure IoT Hub üzenetküldési |} Microsoft Docs"
description: "Fejlesztői útmutató - eszközről a felhőbe és a felhő-eszközt az IoT-központ az üzenetküldési. Üzenetformátumok és a támogatott kommunikációs protokollok kapcsolatos adatokat tartalmaz."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: a610741e23e243f392f1c042f9ab4a00d42f734f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a><span data-ttu-id="a3a43-104">Eszköz-felhő és a felhő-eszközt az IoT-központ az üzenetküldési</span><span class="sxs-lookup"><span data-stu-id="a3a43-104">Device-to-cloud and cloud-to-device messaging with IoT Hub</span></span>

<span data-ttu-id="a3a43-105">Az IoT-központ toocommunicate üzenetküldés az eszközök által használja:</span><span class="sxs-lookup"><span data-stu-id="a3a43-105">Use IoT Hub messaging toocommunicate with your devices by:</span></span>

* <span data-ttu-id="a3a43-106">Küldés [eszközről a felhőbe] [ lnk-d2c] üzeneteket az eszközök tooyour megoldásból háttér.</span><span class="sxs-lookup"><span data-stu-id="a3a43-106">Sending [device-to-cloud][lnk-d2c] messages from your devices tooyour solution back end.</span></span>
* <span data-ttu-id="a3a43-107">Küldés [felhő eszközre] [ lnk-c2d] hello megoldás háttérrendszere üzeneteit end tooyour eszközök.</span><span class="sxs-lookup"><span data-stu-id="a3a43-107">Sending [cloud-to-device][lnk-c2d] messages from hello solution back end tooyour devices.</span></span>

<span data-ttu-id="a3a43-108">Az IoT-központ üzenetküldési funkció Core tulajdonságai, hello megbízhatóság és a tartós üzenetek.</span><span class="sxs-lookup"><span data-stu-id="a3a43-108">Core properties of IoT Hub messaging functionality are hello reliability and durability of messages.</span></span> <span data-ttu-id="a3a43-109">Ezek a Tulajdonságok hello eszköz ügyféloldali rugalmasság toointermittent kapcsolódásának engedélyezése, és tooload Eseményfeldolgozási hello felhő oldalán a napra.</span><span class="sxs-lookup"><span data-stu-id="a3a43-109">These properties enable resilience toointermittent connectivity on hello device side, and tooload spikes in event processing on hello cloud side.</span></span> <span data-ttu-id="a3a43-110">Az IoT-központ megvalósítja *legalább egyszer* kézbesítési biztosítja, hogy az eszközről a felhőbe és a felhő eszközre üzenetek.</span><span class="sxs-lookup"><span data-stu-id="a3a43-110">IoT Hub implements *at least once* delivery guarantees for both device-to-cloud and cloud-to-device messaging.</span></span>

<span data-ttu-id="a3a43-111">Az IoT-központ bemutatása toohello szolgáltatásait, lásd: hello cikkek [Azure és az eszközök internetes hálózatát] [ lnk-azure-iot] és [hello Azure IoT-központ szolgáltatás áttekintése] [lnk-iot-hub-overview].</span><span class="sxs-lookup"><span data-stu-id="a3a43-111">For an introduction toohello capabilities of IoT Hub, see hello articles [Azure and Internet of Things][lnk-azure-iot] and [Overview of hello Azure IoT Hub service][lnk-iot-hub-overview].</span></span>

## <a name="when-toouse-iot-hub-messaging"></a><span data-ttu-id="a3a43-112">Ha toouse IoT-központ üzenetkezelés</span><span class="sxs-lookup"><span data-stu-id="a3a43-112">When toouse IoT Hub messaging</span></span>

<span data-ttu-id="a3a43-113">Használja az eszközről a felhőbe üzenetek küldésének ideje adatsorozat telemetriai adatok és riasztások az eszköz alkalmazásból, és a felhő-eszközre küldött üzenetek egyirányú értesítések tooyour eszköz alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a3a43-113">Use device-to-cloud messages for sending time series telemetry and alerts from your device app, and cloud-to-device messages for one-way notifications tooyour device app.</span></span>

* <span data-ttu-id="a3a43-114">Tekintse meg a túl[eszközről a felhőbe kommunikációs útmutatást] [ lnk-d2c-guidance] Ha bizonytalan az eszköz a felhőbe küldött üzeneteket, a jelentett tulajdonságok vagy a fájl feltöltése között.</span><span class="sxs-lookup"><span data-stu-id="a3a43-114">Refer too[Device-to-cloud communication guidance][lnk-d2c-guidance] if in doubt between using device-to-cloud messages, reported properties, or file upload.</span></span>
* <span data-ttu-id="a3a43-115">Tekintse meg a túl[felhő eszközre kommunikációs útmutatást] [ lnk-c2d-guidance] Ha kétségei vannak, a felhő-eszközre küldött üzenetek, kívánt tulajdonságokkal vagy közvetlen módszerek között.</span><span class="sxs-lookup"><span data-stu-id="a3a43-115">Refer too[Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using cloud-to-device messages, desired properties, or direct methods.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3a43-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3a43-116">Next steps</span></span>

* <span data-ttu-id="a3a43-117">További tudnivalók az IoT-központ [eszközről a felhőbe üzenetküldési][lnk-d2c].</span><span class="sxs-lookup"><span data-stu-id="a3a43-117">Learn about IoT Hub [device-to-cloud messaging][lnk-d2c].</span></span>
* <span data-ttu-id="a3a43-118">További tudnivalók az IoT-központ [eszközre cloud messaging][lnk-c2d].</span><span class="sxs-lookup"><span data-stu-id="a3a43-118">Learn about IoT Hub [cloud-to-device messaging][lnk-c2d].</span></span>

[lnk-azure-iot]: iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
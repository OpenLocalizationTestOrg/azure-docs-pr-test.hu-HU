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
# <a name="scale-your-iot-hub-solution"></a>Az IoT hub megoldás méretezése
Az Azure IoT Hub tooa millió egyidejűleg csatlakoztatott eszközöket támogatja. További információkért lásd: [árképzési IoT-központ][lnk-pricing]. IoT Hub-egységenként lehetővé teszi, hogy egy bizonyos napi üzenetek száma.

tooproperly a megoldás méretezése, fontolja meg az IoT-központ alkalmazását. Különösen vegye figyelembe a következő kategóriák műveletek hello szükséges hello maximális átviteli sebesség:

* Az eszközről a felhőbe irányuló üzenetek
* Felhő-eszközre küldött üzenetek
* Identitásjegyzék műveletei

Toothis átviteli tájékoztatást, a következő látható: [IoT-központ kvóták és szabályozások] [ IoT Hub quotas and throttles] és ennek megfelelően a megoldás megtervezése.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Eszköz-felhő és a felhő eszközre üzenet átviteli sebesség
hello legjobb módja toosize egy IoT-központ megoldás tooevaluate hello forgalmat egy egységenként.

Eszköz a felhőbe küldött üzeneteket az alábbiakra fenntartható.

| Szint | Fenntartható | Tartós küldések |
| --- | --- | --- |
| S1 |Too1111 KB/perc egységenként mentése<br/>(1,5 GB/nap/egység) |278 üzenetek/másodperc egységenként átlaga<br/>(400 000 üzenetek/nap / egység) |
| S2 |Too16 MB percenként egységenként mentése<br/>(22.8 GB/nap/egység) |4,167 üzenetek/másodperc egységenként átlaga<br/>(6 millió üzenetek/nap / egység) |
| S3 |Too814 MB percenként egységenként mentése<br/>(1144.4 GB/nap/egység) |208,333 üzenetek/másodperc egységenként átlaga<br/>(300 millió üzenetek/nap / egység) |

## <a name="identity-registry-operation-throughput"></a>Identitás beállításjegyzék művelet átviteli sebesség
Az IoT-központ identitás kapcsolatos műveletek nem kellene toobe futásidejű műveletek, mivel ezek többnyire kapcsolódó toodevice kiépítés.

Adott kapacitásnövelés számok, lásd: [IoT-központ kvóták és szabályozások][IoT Hub quotas and throttles].

## <a name="sharding"></a>Horizontális
Amíg egy egyetlen IoT hub méretezhető toomillions az eszközök, a megoldás specifikus teljesítményt nyújt, amely egyetlen IoT-központ nem garantálható, hogy szükség. Ebben az esetben ajánlott, hogy az IoT-központok több partíció az eszközök. Több IoT-központok forgalom felszakadásáig sima, és szerezze be a szükséges átviteli hello vagy művelet díjszabás szükséges.

## <a name="next-steps"></a>Következő lépések
toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]
* [Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

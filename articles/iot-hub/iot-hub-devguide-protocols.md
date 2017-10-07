---
title: "az IoT-központ kommunikációs aaaAzure protokollok és portok |} Microsoft Docs"
description: "Fejlesztői útmutató - ismerteti hello támogatott kommunikációs protokollok a eszközről a felhőbe és a felhő-eszköz- és hello portszámok, amelyek nyitva kell lennie."
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
ms.openlocfilehash: 31cb948f1d30edd87edb13ad0dd859c02bcc3239
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Olyan kommunikációs protokollt hivatkozhat - a

Az IoT-központ lehetővé teszi, hogy az eszközök toouse [MQTT][lnk-mqtt], MQTT keresztül websocket elemeket, [AMQP][lnk-amqp], websocket elemeket, és a HTTP-alapú AMQP az eszközoldali protokollok kommunikáció. Ezeket a protokollokat hogyan támogatják a különböző funkciók IoT-központ kapcsolatos információkért lásd: [eszközről a felhőbe kommunikációs útmutatást] [ lnk-d2c-guidance] és [felhő eszközre kommunikációs útmutatást] [lnk-c2d-guidance].

hello következő tábla ajánlásokat hello magas szintű a kiválasztott protokoll:

| Protokoll | Ha ez a protokoll válasszon |
| --- | --- |
| MQTT <br> MQTT WebSocket keresztül |Minden eszközön, amelyre nincs szükség tooconnect több eszközzel (minden saját eszköz hitelesítő adatokkal rendelkező) keresztül hello ugyanazt a TLS-kapcsolatot. |
| AMQP <br> AMQP WebSocket keresztül |Használja a mezőt, és a felhő átjárók tootake előnyeit multiplexáló eszközök közötti kapcsolat. |
| HTTP |Használjon más protokollt nem támogató eszközökön. |

Vegye figyelembe a következő pontok, ha úgy dönt, hogy a eszközoldali kommunikációs protokollja hello:

* **Felhő eszközre mintát**. A HTTP nem rendelkezik egy hatékony módját tooimplement kiszolgáló leküldéses. Ilyen HTTP használatakor eszközök kérdezze le az IoT-központ az felhő-eszközre küldött üzenetek. Ez a módszer nem hatékony hello eszköz és az IoT-központ. Az aktuális HTTP irányelveit minden egyes eszköz kell kérdezze le az üzenetek 25 percenként vagy több. A hello ugyanakkor, MQTT és AMQP támogatja kiszolgáló leküldéses a felhő-eszközre küldött üzenetek fogadásakor. Az IoT-központ toohello eszközről üzenetek azonnali leküldéses értesítések lehetővé teszik. Kézbesítési késés fontos, ha a MQTT vagy AMQP hello legjobb protokollok toouse. Ritkán csatlakoztatott eszközök esetén a HTTP is működik.
* **Átjárók mezőben**. MQTT és a HTTP használata esetén nem lehet csatlakoztatni (minden saját eszköz hitelesítő adatokkal rendelkező) több eszköz használatával hello ugyanazt a TLS-kapcsolatot. Ebből kifolyólag a [mezőben az átjáró alkalmazásának típusai][lnk-azure-gateway-guidance], ezeket a protokollokat optimálisnál, mert azok megkövetelik hello mező átjáró és az IoT-központ között egy TLS-kapcsolat, minden eszközön csatlakoztatott toohello mező átjáró.
* **Kevés erőforrás eszközök**. hello MQTT és a HTTP-könyvtárak rendelkezik egy hello AMQP szalagtárak-nál kisebb erőforrásigényét. Ilyen, ha hello eszköz csak korlátozott erőforrásokat (például, kevesebb mint 1 MB RAM) ezeket a protokollokat, akkor lehetséges, hello kizárólag protokollmegvalósítás érhető el.
* **A hálózati átviteli**. hello szabványos AMQP protokoll port: 5671, amíg MQTT porton figyel 8883, amelyek problémákat okozhatnak hálózatokban, amelyek a lezárt toonon HTTP protokoll. Websocket elemek keresztül MQTT, websocket elemeket, és a HTTP-alapú AMQP érhető el ebben a forgatókönyvben használt toobe.
* **Terhelés méretének**. MQTT és AMQP a bináris protokollok, amelyek tömörebb hasznos adat található HTTP-nál.

> [!WARNING]
> HTTP használata esetén minden egyes eszköz kell a lekérésének felhő-eszközre küldött üzenetek 25 percenként vagy több. Azonban során fejlesztési, a rendszer elfogadható toopoll gyakrabban 25 percenként.

## Portszámok

Eszközök képesek kommunikálni az Azure-ban a különböző protokollok IoT-központot. Hello választott protokoll általában hello konkrét követelmények hello megoldás vezérlik. hello következő táblázat hello kimenő meg kell nyitni egy eszköz toobe képes toouse egy adott protokoll:

| Protokoll | Port |
| --- | --- |
| MQTT |8883 |
| MQTT websocket elemek keresztül |443 |
| AMQP |5671 |
| Az AMQP keresztül websocket elemek |443 |
| HTTP |443 |

Miután létrehozta az IoT-központ Azure-régióban, hello IoT hub tartja hello azonos IP-címet, hogy az IoT-központ hello élettartamát. Azonban toomaintain szolgáltatásminőség, ha a Microsoft hello IoT hub tooa különböző méretezési egység, akkor az új IP-cím van hozzárendelve.


## Következő lépések

toolearn hogyan megvalósítja az IoT-központ a hello MQTT protokoll kapcsolatos további információkért lásd: [kommunikáljon az IoT hubbal hello MQTT protokoll használatával][lnk-mqtt-support].

[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-mqtt-support]: iot-hub-mqtt-support.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways

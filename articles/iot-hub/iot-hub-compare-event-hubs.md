---
title: aaaCompare Azure IoT Hub tooAzure Event Hubs |} Microsoft Docs
description: "Hello IoT-központ és az Event Hubs Azure-szolgáltatások használati esetek és működési különbségeket kiemelés összehasonlítása. hello összehasonlítását tartalmazza a támogatott protokollok, kezelés, figyelés, és fájl feltöltését."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: e5f546b54e29860498d540abfc86a41c4662c0df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Az Azure IoT-központ és az Azure Event Hubs összehasonlítása
Az IoT-központ fő alkalmazási helyzetei hello egyik toogather telemetriai eszközökről. Emiatt az IoT-központ gyakran képest túl[Azure Event Hubs][Azure Event Hubs]. Az IoT-központ, mint az Event Hubs egy Eseményfeldolgozási szolgáltatás, amely lehetővé teszi az esemény-és telemetriabevitelt toohello felhő nagy méretű, alacsony késéssel és nagy megbízhatósággal.

Hello szolgáltatások azonban sok különbségek, amely részletes leírást talál a következő táblázat hello rendelkezik:

| Terület | IoT Hub | Event Hubs |
| --- | --- | --- |
| Kommunikációs minták | Lehetővé teszi, hogy [eszközről a felhőbe kommunikációs] [ lnk-d2c-guidance] (üzenettovábbítás, fájl, feltöltések és jelentett Tulajdonságok) és [felhő eszközre kommunikációs] [ lnk-c2d-guidance] (közvetlen módszerek, üzenetküldési kívánt tulajdonságai). |Csak lehetővé teszi, hogy esemény érkező (eszköz-felhő forgatókönyvek általában számít). |
| Eszköz állapota adatai | [Eszköz twins] [ lnk-twins] tárolására és a lekérdezési eszköz állapotadatait. | Eszköz állapota adatokat tárolhatja. |
| Eszköz protokoll támogatása |MQTT, MQTT websocket elemeket, AMQP, websocket elemeket, és a HTTP-alapú AMQP keresztül támogatja. Továbbá az IoT-központ hello együttműködve [Azure IoT protokoll-átjáró][lnk-azure-protocol-gateway], egy testre szabható protokoll átjáró megvalósítási toosupport egyéni protokollok. |Támogatja az AMQP, AMQP websocket elemeket, és a HTTP-alapú. |
| Biztonság |Biztosít az eszközönkénti identitás- és visszavonható hozzáférés-vezérlés. Lásd: hello [hello IoT Hub fejlesztői útmutató szakasza biztonsági]. |Event Hubs kiterjedő biztosít [megosztott hozzáférési házirendek][Event Hubs - security], és a korlátozott visszahívás támogatja segítségével [publisher házirendek][Event Hubs publisher policies]. Az IoT-megoldások általában szükséges tooimplement egy egyéni megoldás toosupport eszközönkénti hitelesítő adatok és mérőszámok hamisításszűrés. |
| Műveletek figyelése |Lehetővé teszi, hogy az IoT megoldások toosubscribe tooa sokféle eszköz identity management és a csatlakozási esemény, például egyes eszköz hitelesítési hibák, a sávszélesség-szabályozás és a hibás formátumú kivételek. Ezek az események lehetővé teszik a tooquickly hello egyéni eszköz szinten kapcsolódási problémák azonosításához. |Csak összesített metrikák mutatja. |
| Méretezés |Optimalizált toosupport több millió egyidejűleg csatlakoztatott eszközön van. |Mérőszámok hello megfelelően kapcsolatok [Azure Event Hubs kvóták][Azure Event Hubs quotas]. A hello ugyanakkor, az Event Hubs lehetővé teszi az egyes üzenetsorokra küldött üzeneteket toospecify hello partíció. |
| Eszközoldali SDK-k |Itt [eszköz SDK-k] [ Azure IoT SDKs] számos platformon és programnyelven, továbbá toodirect MQTT, AMQP és HTTP API-k esetében. |Támogatott .NET, Java és a C, továbbá tooAMQP és HTTP küldési felületek. |
| Fájl feltöltése |Lehetővé teszi, hogy az IoT megoldások tooupload fájlok eszközök toohello felhőből. A munkafolyamat-integráció és egy támogatási hibakereséshez kategória figyelési műveletek egy fájl értesítési tartozó végpontot tartalmaz. | Nem támogatott. |
| Útvonal-üzenetek toomultiple végpontok | Too10 be egyéni végpontokat támogatottak. A szabályok határozzák meg, hogyan történik az üzenetek a irányított toocustom végpontok. További információkért lásd: [üzeneteket küldjön és fogadjon IoT hubbal][lnk-devguide-messaging]. | Szükséges kód toobe írt fut, és az üzenetek terjesztéséhez. |

Összefoglalva akkor is, ha hello csak használati eset telemetriai eszközről a felhőbe érkező, az IoT-központ biztosít egy szolgáltatás, amely úgy van kialakítva, IoT-eszköz kapcsolatot. Folyamatosan tooexpand hello értéknövelő az IoT-specifikus funkcióival forgatókönyvek esetén. Az Event Hubs egy nagy méretű, mindkét többek datacenter és intra-datacenter forgatókönyvek hello környezetében esemény érkező készült.

Nincs ritka toouse IoT-központot, mind az Event Hubs hello ugyanahhoz a megoldáshoz. Az IoT-központ hello eszközről a felhőbe kommunikációt kezeli, és az Event Hubs későbbi szakaszban esemény érkező kezeli a valós idejű feldolgozórendszerekkel.

### <a name="next-steps"></a>Következő lépések
További információ az IoT-központ telepítésének tervezése toolearn lásd [méretezés, a magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási][lnk-scaling].

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]
* [Egy eszköz szimulálva IoT oldala][lnk-iotedge]

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[hello IoT Hub fejlesztői útmutató szakasza biztonsági]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-features.md#event-publishers
[Azure Event Hubs quotas]: ../event-hubs/event-hubs-quotas.md
[Azure IoT SDKs]: https://github.com/Azure/azure-iot-sdks
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md

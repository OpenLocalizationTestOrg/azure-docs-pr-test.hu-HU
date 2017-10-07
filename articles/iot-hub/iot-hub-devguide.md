---
title: "Azure IoT Hub aaaDeveloper útmutatója |} Microsoft Docs"
description: "hello Azure IoT Hub fejlesztői útmutató végpontok, biztonsági, hello identitásjegyzékhez, kezelés, közvetlen módszerek, eszköz twins, fájlfeltöltéseket, feladatok, hello IoT-központ lekérdező nyelv, és üzenetküldési beszélgetéseket tartalmazza."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: d534ff9d-2de5-4995-bb2d-84a02693cb2e
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 2d3f18399e4cef6f9c4850a5caeb170a2d091a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-developer-guide"></a>Az Azure IoT Hub fejlesztői útmutató

Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amellyel eszközök millióira közötti megbízható és biztonságos kétirányú kommunikáció engedélyezése és a megoldás háttérrendszere.

Az Azure IoT-központ biztosítja:

* A biztonságos kommunikáció eszközönkénti biztonsági hitelesítő adatok használatával, és hozzáférés-vezérlést.
* Több eszköz-felhő és a felhő eszközre kapacitású kommunikációs lehetőségekről.
* Eszközönkénti állapotadatokat és a metaadatok tárolása lekérdezhető.
* Egyszerű eszközkapcsolatok eszköz könyvtárakkal hello legnépszerűbb nyelvekhez és platformokhoz.

Az IoT Hub fejlesztői útmutató a következő cikkek hello tartalmazza:

* [Eszköz-felhő kommunikációs útmutatást] [ lnk-d2c-guidance] segítségével választhat eszköz a felhőbe küldött üzeneteket, az eszköz iker jelentett tulajdonságok és a fájl feltöltése között.
* [Útmutatás felhő-és eszköz közötti kommunikációt] [ lnk-c2d-guidance] segítségével választhat a közvetlen módszer eszköz iker kívánt tulajdonságokkal és felhő-eszközre küldött üzenetek között.
* [Eszköz-felhő és a felhő-eszközt az IoT-központ az üzenetküldési] [ devguide-messaging] hello üzenetküldési funkcióknak (eszközről a felhőbe és felhő eszközre), amely az IoT-központ elérhetővé teszi ismerteti.
  * [Eszköz a felhőbe küldött üzeneteket tooIoT Hub küldése][devguide-messages-d2c].
  * [Eszköz a felhőbe küldött üzeneteket beolvasni hello beépített végpont][devguide-builtin].
  * [Egyéni végpontokat és útválasztási szabályokat használja az eszköz a felhőbe küldött üzeneteket][devguide-custom].
  * [Felhő-eszközre küldött üzenetek küldése az IoT-központ][devguide-messages-c2d].
  * [Hozzon létre, és az IoT-központ olvashatja][devguide-format].
* [Az eszközről tölt fel] [ devguide-upload] ismerteti, hogyan tölthet fájlok az eszközről. hello cikket is küldhet értesítéseket hello feltöltési folyamat többek között a hello kapcsolatos információkat.
* [Az IoT Hub eszköz identitáskezelést] [ devguide-identities] milyen információkat ismerteti, minden IoT-központ beállításjegyzék identitástárolók, és hogyan elérheti és módosíthatja azt.
* [Szabályozhatja a hozzáférést tooIoT Hub] [ devguide-security] funkcióit mutatja be hello biztonsági modell toogrant hozzáférés tooIoT Hub eszközöket és a felhő összetevőket. hello cikk jogkivonatokat és X.509-tanúsítványokat és hello engedélyeket biztosíthat a részleteit használatával kapcsolatos információt tartalmaz.
* [Az eszköz twins toosynchronize állapotát és konfigurációkat használják] [ devguide-device-twins] hello ismerteti *eszköz iker* koncepciót és hello funkcióit érheti el például az eszköz szinkronizálása az eszköz kettős. hello cikk egy eszköz iker tárolt hello adatokra vonatkozó információt tartalmaz.
* [Az eszközön közvetlen metódus] [ devguide-directmethods] hello életciklus közvetlen módszer, hogyan tooinvoke metódusai a háttér-alkalmazás és a leíró hello eszközről a közvetlen módszer az eszközön információkat ismerteti.
* [Több eszközön feladatok ütemezése] [ devguide-jobs] ismerteti, hogyan több eszközön futó feladatot is ütemezheti. hello cikkből megtudhatja, hogyan toosubmit feladatok feladatokat végrehajtó egy közvetlen módszer, egy eszközt egy eszköz iker frissítése. Azt is bemutatja, hogyan tooquery hello egy feladat állapota.
* [Olyan kommunikációs protokollt hivatkozhat - a] [ devguide-protocol] ismerteti hello kommunikációs protokollt, hogy az IoT-Központ támogatja az eszközök közötti kommunikáció, és listák hello portok nyitva kell lennie.
* [Referencia - IoT-központok végpontjai] [ devguide-endpoints] ismerteti, hogy minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontok hello. hello is bemutatja, hogyan hozhat létre további végpontokat az IoT hub, és hogyan toouse mező átjáró tooenable eszközök kapcsolat tooyour IoT-központ végpontok nem szabványos forgatókönyvekben.
* [Referencia - IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ devguide-query] , hogy az IoT-központ lekérdező nyelv, amely lehetővé teszi tooretrieve származó információkat a központ az eszköz twins és feladatok ismertetése.
* [Referencia - kvóták és sávszélesség-szabályozási] [ devguide-quotas] hello IoT-központ szolgáltatás és a sávszélesség-szabályozás toosee túllépi a kvótát esetén várható viselkedést hello hello kvóták foglalja össze.
* [Hivatkozás - díjszabás] [ devguide-pricing] különböző Termékváltozatai és az IoT-központot, és hogyan IoT-központ által küldött üzenetek hello különböző IoT-központ funkciók mérése, a részletek díjszabása kapcsolatos általános tudnivalókat tartalmazza.
* [Referencia - eszköz és az SDK szolgáltatás] [ devguide-sdks] listák hello Azure IoT SDK-k toodevelop is használhatja az eszköz és a szolgáltatás olyan alkalmazásokat, amelyek az IoT hub kommunikál. hello cikkből hivatkozások tooonline API dokumentációját.
* [Referencia - IoT Hub MQTT támogatási] [ devguide-mqtt] részletes információt nyújt arról, hogyan IoT-központ hello MQTT protokoll használatát támogatja. hello cikk ismerteti a hello hello MQTT protokoll beépített toohello Azure IoT SDK-k támogatása, és közvetlenül a hello MQTT protokoll használatával kapcsolatban nyújt információkat.
* [Szószedet] [ devguide-glossary] általános IoT Hub-mal kapcsolatos kifejezések listája.

[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md
[devguide-pricing]: iot-hub-devguide-pricing.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[devguide-messages-d2c]: iot-hub-devguide-messages-d2c.md
[devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[devguide-custom]: iot-hub-devguide-messages-read-custom.md
[devguide-messages-c2d]: iot-hub-devguide-messages-c2d.md
[devguide-format]: iot-hub-devguide-messages-construct.md
[devguide-protocol]: iot-hub-devguide-protocols.md

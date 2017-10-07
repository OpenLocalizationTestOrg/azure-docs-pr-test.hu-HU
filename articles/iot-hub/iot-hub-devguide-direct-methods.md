---
title: "Azure IoT Hub aaaUnderstand közvetlen módszerek |} Microsoft Docs"
description: "Fejlesztői útmutató - használható közvetlen módszerek tooinvoke kódot az eszközök egy szolgáltatás alkalmazásból."
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d15d44a0c3e1d1cda1669c1ed011c2f932e3d92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Ismertetés és az IoT-központ közvetlen metódusok
## <a name="overview"></a>Áttekintés
Az IoT-központ lehetővé teszi az képességét tooinvoke közvetlen módszerek hello felhőből eszközökön. Közvetlen módszerek határoz meg egy eszköz hasonló tooan HTTP hívható meg abban, hogy sikeres legyen, vagy közvetlenül (felhasználó által meghatározott időtúllépési) után sikertelen a kérelem-válasz interakció. Ez akkor hasznos, a forgatókönyvekben, ahol hello intézkedés azonnali hello eszköz volt-e képes toorespond, például egy SMS-ébresztési tooa eszköz küldése, ha egy eszköz kapcsolat nélküli (SMS drágább, mint egy metódus hívása folyamatban) függően eltérnek.

Minden eszköz metódus egyetlen eszközt célozza. [Feladatok] [ lnk-devguide-jobs] biztosítanak egy módon tooinvoke közvetlen módszerek több eszközön, és ütemezés szerinti metódushívás leválasztott eszközökhöz.

Bárki, aki **service csatlakozás** IoT-központ engedélyeinek indít el egy metódust az eszközön.

### <a name="when-toouse"></a>Ha toouse
Közvetlen módszerek hajtsa végre a kérelem-válasz mintát, és úgy van kialakítva, az eredmény, általában interaktív vezérléshez hello eszköz, például egy ventilátor a tooturn azonnali megerősítését igénylő kommunikációhoz.

Tekintse meg a túl[felhő eszközre kommunikációs útmutatást] [ lnk-c2d-guidance] bizonytalan kívánt tulajdonságai között, ha a közvetlen módszer vagy a felhő-eszközre küldött üzenetek.

## <a name="method-lifecycle"></a>Módszer életciklusa
Közvetlen módszerek hello eszközön megvalósított, és előfordulhat, hogy nulla vagy több bemenetében benne hello módszer hasznos toocorrectly hozható létre. A közvetlen módszer keresztül elérhető szolgáltatás URI indításakor (`{iot hub}/twins/{device id}/methods/`). Egy eszköz megkapja közvetlen metódusok eszközspecifikus MQTT témakör keresztül (`$iothub/methods/POST/{method name}/`). A további eszközoldali hálózati protokollok a jövőbeli hello közvetlen módszerek előfordulhat, hogy támogatott.

> [!NOTE]
> Az eszközön közvetlen metódus meghívásakor nevét és értékeit csak tartalmazhat US-ASCII nyomtatható alfanumerikus, kivéve a következő set hello: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Szinkron módszerek és vagy sikeres közvetlen vagy követően hello időkorlát (alapértelmezett: 30 másodperces, állítható be too3600 másodperc). Közvetlen módszereket kívánt egy eszköz tooact csak ha hello eszköz online és a fogadó parancsok, például egy világos telefonos bekapcsolásával interaktív esetekben hasznos. Ezekben az esetekben érdemes toosee egy azonnali sikeres vagy sikertelen volt, hello felhőalapú szolgáltatás működhet hello eredménye a lehető leghamarabb. hello eszköz térhetnek vissza néhány üzenettörzs hello metódus miatt, de ez nem szükséges hello metódus toodo így. Van a rendezés nem garantálja, vagy a metódushívások bármely párhuzamossági szemantikáját.

Közvetlen módszer a következők: csak HTTP hello felhő oldaláról, és csak MQTT hello eszköz oldaláról.

hello hasznos módszer kérelem és válasz egy JSON-dokumentum mentése too8KB.

## <a name="reference-topics"></a>Referencia-témaköreit:
hello következő referencia-témakörökre biztosít közvetlen módszerek használatáról további információkat.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>A háttér-alkalmazásból közvetlen metódus
### <a name="method-invocation"></a>A metódushívás
Az eszközön a közvetlen módszer meghívásához HTTP hívások, amely tartalmazza:

* Hello *URI* adott toohello eszköz (`{iot hub}/twins/{device id}/methods/`)
* hello POST *módszer*
* *Fejlécek* amely tartalmaz hello engedélyezési, a kérelem azonosítója, tartalomtípus, és a tartalom kódolása
* Transzparens JSON *törzs* a hello a következő formátumban:

```
{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

Időtúllépés másodpercben van. Ha időtúllépési nincs beállítva, alapértelmezett too30 másodperc.

### <a name="response"></a>Válasz
hello háttér-alkalmazást magában foglaló választ kap:

* *HTTP-állapotkód*, amellyel az IoT-központ hello érkező hibák, beleértve az eszközök 404 hibaüzenetet jelenleg nem csatlakoztatott
* *Fejlécek* amely hello ETag tartalmaz, a kérelem azonosítója, tartalomtípus, és a tartalom kódolása
* A JSON *törzs* a hello a következő formátumban:

```
{
    "status" : 201,
    "payload" : {...}
}
```

   Mindkét `status` és `body` hello eszköz által biztosított és toorespond használt hello eszköz saját állapotkód és/vagy leírása.

## <a name="handle-a-direct-method-on-a-device"></a>Kezeli az eszközön a közvetlen módszer
### <a name="method-invocation"></a>A metódushívás
Eszköz megkapja a közvetlen módszer kérelmeket a hello MQTT témakör:`$iothub/methods/POST/{method name}/?$rid={request id}`

mely hello eszköz megkapja hello törzs hello a következő formátumban kell megadni:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

A metóduskérelmek QoS 0.

### <a name="response"></a>Válasz
hello eszköz küldi a válaszokat túl`$iothub/methods/res/{status}/?$rid={request id}`, ahol:

* Hello `status` tulajdonsága hello metódus végrehajtása a megadott eszköz állapotát.
* Hello `$rid` tulajdonsága az IoT-központ kapott hello metódushívás hello Kérelemazonosító.

hello szervezet hello eszköz állítja be, és bármely állapota lehet.

## <a name="additional-reference-material"></a>További referenciaanyag
Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:

* [IoT-központok végpontjai] [ lnk-endpoints] ismerteti, hogy minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontok hello.
* [Sávszélesség-szabályozási és kvóták] [ lnk-quotas] toohello IoT-központ szolgáltatás és szabályozási viselkedés tooexpect hello hello szolgáltatás használatakor hello kvóták ismerteti.
* [Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] listák hello különböző nyelvi használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor SDK-k.
* [Az IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ lnk-query] hello tooretrieve IoT Hub-ből származó adataival az eszköz twins és feladatok is használhatja az IoT-központ lekérdezési nyelv ismerteti.
* [Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] hello MQTT protokoll IoT-központ támogatásával kapcsolatos további információkat biztosít.

## <a name="next-steps"></a>Következő lépések
Most megtanulta, hogyan toouse közvetlen módszerek, esetleg következő IoT Hub fejlesztői útmutató témakör hello iránt érdeklődik:

* [Több eszközön feladatok ütemezése][lnk-devguide-jobs]

Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ az oktatóanyag következő hello iránt érdeklődik:

* [Közvetlen módszerekkel][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md

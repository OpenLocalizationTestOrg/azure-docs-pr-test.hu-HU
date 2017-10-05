---
title: "Azure IoT Hub feladatok ismertetése |} Microsoft Docs"
description: "Az IoT hub fejlesztői útmutató - feladatütemezés több eszközökön való futtatására csatlakoztatva. Feladatok címkék és a kívánt tulajdonságok frissítése, és több eszközön közvetlen metódusok."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: fe78458f-4f14-4358-ac83-4f7bd14ee8da
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: abb7f80662650efa8f158f32125ebc5350cb4f62
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="schedule-jobs-on-multiple-devices"></a>Feladatok ütemezése több eszközön
## <a name="overview"></a>Áttekintés
Szerint az előző cikket, Azure IoT-központ lehetővé teszi, hogy számos egyéb építőelemekből ([iker eszköztulajdonságok és címkék] [ lnk-twin-devguide] és [módszerek közvetlen][lnk-dev-methods]).  Háttér-alkalmazások általában eszközadminisztrátorok és operátorok frissítése, és az IoT-eszközök tömeges és ütemezett időpontban interaktívan tesznek lehetővé.  Feladatok ütemezés egyszerre iker eszközfrissítésekhez és szemben az eszközök közvetlen metódusok végrehajtása foglalják magukban.  Például egy olyan operátort használja egy háttér-alkalmazást, amely ehhez kezdeményezése és nyomon követheti a feladat újraindítja az eszközök a 43 és emelet 3 felépítése nem lenne az épület műveletekre zavaró egyszerre.

### <a name="when-to-use"></a>A következő esetekben használja
Érdemes lehet használatával feladatokkal: a megoldás háttérrendszere végén kell ütemezni, és nyomon követni egy csoportján eszköz a következő tevékenységek bármelyike:

* Eszköz kívánt tulajdonságainak frissítése
* Címkék frissítése
* Közvetlen metódusok

## <a name="job-lifecycle"></a>Feladat életciklusa
Feladatok a megoldás háttérrendszeréhez által kezdeményezett, és az IoT-központ által kezelt.  Egy feladat keresztül elérhető szolgáltatás URI is kezdeményezhető (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) és folyamatban van a egy végrehajtás alatt álló feladat keresztül elérhető szolgáltatás URI-lekérdezés (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).  Egy feladat indítható, miután a feladatok lekérdezése lehetővé teszi, hogy a háttér-alkalmazásnak, hogy a futó feladatok állapotának frissítése.

> [!NOTE]
> Elindít egy feladatot, nevét és értékeit tartalmazhatnak US-ASCII nyomtatható alfanumerikus, kivéve a következő set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

## <a name="reference-topics"></a>Referencia-témaköreit:
A következő témaköröket feladatok használatáról további információkat biztosít.

## <a name="jobs-to-execute-direct-methods"></a>Közvetlen módszerek végrehajtandó feladatok
Az alábbiakban található a HTTP 1.1 kérelemrészletek hajthatók végre olyan [közvetlen módszer] [ lnk-dev-methods] meg az eszközök feladat használatával:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-11-14

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            responseTimeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
A lekérdezés feltétel is lehet egyetlen eszközt azonosító vagy eszközazonosítók alább látható módon listája

**Példák**
```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
[Az IoT Hub lekérdezési nyelv] [ lnk-query] IoT-központ lekérdezési nyelv további részletesen ismerteti.

## <a name="jobs-to-update-device-twin-properties"></a>Feladatok eszköz iker tulajdonságainak módosítása
A következő egy feladat használatával kettős eszköztulajdonságok frissítése a HTTP 1.1 kérelem részletei:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-11-14
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Folyamatban van a feladatok lekérdezése
Az alábbiakban található a HTTP 1.1 kérelem részletes [feladatok lekérdezése][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```

A válaszban szereplő a continuationToken valósul meg.  

## <a name="jobs-properties"></a>Feladatok tulajdonságai
A következő tulajdonságokat és a megfelelő leírását, amely lekérdezésekor feladatok vagy a feladat eredményeinek listája látható.

| Tulajdonság | Leírás |
| --- | --- |
| **a JobId értékének** |Alkalmazás azonosítója megadva a feladathoz. |
| **Kezdő időpont** |Alkalmazás által biztosított a feladat kezdési időpontja (ISO 8601). |
| **Befejezés időpontja** |Az IoT-központ dátuma (ISO 8601) Ha a feladat befejeződött-e megadva. Csak azt követően a feladat eléri a "kész" állapot érvényes. |
| **típusa** |Feladatok típusai: |
| **scheduledUpdateTwin**: egy kívánt tulajdonságokkal vagy címkék frissítésére szolgáló feladatot. | |
| **scheduledDeviceMethod**: egy eszköz twins a megfelelő eszköz metódus hívásához használt feladat. | |
| **állapot** |A feladat jelenlegi állapota. Az állapotot a lehetséges értékek: |
| **függőben lévő** : ütemezett és váró észlelnie kell a feladat szolgáltatás. | |
| **ütemezett** : jövőbeli időpontra ütemezve. | |
| **futó** : jelenleg aktív feladat. | |
| **megszakítva** : feladat meg lett szakítva. | |
| **nem sikerült** : feladat sikertelen volt. | |
| **befejeződött** : feladat befejeződött. | |
| **deviceJobStatistics** |A feladat végrehajtása statisztikája. |

**deviceJobStatistics** tulajdonságait.

| Tulajdonság | Leírás |
| --- | --- |
| **deviceJobStatistics.deviceCount** |A feladat eszközök számát. |
| **deviceJobStatistics.failedCount** |Ha a feladat nem sikerült eszközök számát. |
| **deviceJobStatistics.succeededCount** |Ha a feladat sikeresen befejeződött eszközök számát. |
| **deviceJobStatistics.runningCount** |A feladat éppen futó eszközök számát. |
| **deviceJobStatistics.pendingCount** |A feladat futtatásához függőben lévő eszközök száma. |

### <a name="additional-reference-material"></a>További referenciaanyag
Az IoT Hub fejlesztői útmutató más hivatkozás témaköröket tartalmazza:

* [IoT-központok végpontjai] [ lnk-endpoints] ismerteti a különböző végpontok, amelyek minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek.
* [Sávszélesség-szabályozási és kvóták] [ lnk-quotas] ismerteti a kvótákat, az IoT-központ szolgáltatás és a sávszélesség-szabályozási viselkedését történik, ha a szolgáltatás használatához.
* [Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] felsorolja a különböző nyelvi SDK-k, egy eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor használja.
* [Az IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ lnk-query] az IoT-központ lekérdezési nyelv segítségével adatok lekérését az IoT-központ az eszköz twins és feladatok ismertetése.
* [Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] IoT-központ támogatásával kapcsolatos további információkat biztosít a MQTT protokoll.

## <a name="next-steps"></a>Következő lépések
Ha azt szeretné, hogy próbálja ki azokat a jelen cikkben ismertetett fogalmakat, esetleg megváltozása a következő IoT Hub-oktatóanyag:

* [Ütemezés és a feladatok][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md

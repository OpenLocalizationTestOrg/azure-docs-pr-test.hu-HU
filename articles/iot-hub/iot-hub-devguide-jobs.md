---
title: aaaUnderstand Azure IoT Hub feladatok |} Microsoft Docs
description: "Fejlesztői útmutató - tooyour IoT-központ feladatok toorun több eszközön ütemezés csatlakoztatva. Feladatok címkék és a kívánt tulajdonságok frissítése, és több eszközön közvetlen metódusok."
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
ms.openlocfilehash: 8be134e6c379feae5087df8f562a74505c57afee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-jobs-on-multiple-devices"></a>Feladatok ütemezése több eszközön
## <a name="overview"></a>Áttekintés
Szerint az előző cikket, Azure IoT-központ lehetővé teszi, hogy számos egyéb építőelemekből ([iker eszköztulajdonságok és címkék] [ lnk-twin-devguide] és [módszerek közvetlen][lnk-dev-methods]).  Háttér-alkalmazások általában engedélyezése az eszköz a rendszergazdák és a kezelők tooupdate, és az IoT-eszközök tömeges és ütemezett időpontban módosításának.  Feladatok ütemezése egyszerre iker eszközfrissítésekhez és az eszközök elleni közvetlen módszerek hello végrehajtási foglalják magukban.  Például egy olyan operátort használja egy háttér-alkalmazást, amely ehhez kezdeményezése és nyomon követheti a feladat tooreboot az eszközök a 43 és emelet 3 felépítése nem lenne zavaró toohello műveletek hello épület egyszerre.

### <a name="when-toouse"></a>Ha toouse
Érdemes lehet használatával feladatokkal: egy megoldás háttér igények tooschedule és nyomon követése előrehaladás eszköz a megfelelő tevékenységek következő hello bármelyikét:

* Eszköz kívánt tulajdonságainak frissítése
* Címkék frissítése
* Közvetlen metódusok

## <a name="job-lifecycle"></a>Feladat életciklusa
Feladatok hello megoldás háttérrendszerének által kezdeményezett, és az IoT-központ által kezelt.  Egy feladat keresztül elérhető szolgáltatás URI is kezdeményezhető (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) és folyamatban van a egy végrehajtás alatt álló feladat keresztül elérhető szolgáltatás URI-lekérdezés (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).  Egy feladat indítható, miután a feladatok lekérdezése lehetővé teszi, hogy hello háttér-alkalmazás toorefresh hello állapotának futó feladatok.

> [!NOTE]
> Elindít egy feladatot, nevét és értékeit tartalmazhatnak US-ASCII nyomtatható alfanumerikus, kivéve a következő set hello: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

## <a name="reference-topics"></a>Referencia-témaköreit:
a következő témaköröket hello feladatok használatáról további információkat biztosít.

## <a name="jobs-tooexecute-direct-methods"></a>Feladatok tooexecute közvetlen módszer
hello az alábbiakban látható a HTTP 1.1 hello kérelemrészletek hajthatók végre olyan [közvetlen módszer] [ lnk-dev-methods] meg az eszközök feladat használatával:

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
hello lekérdezés feltétel is lehet egyetlen eszközt azonosító vagy eszközazonosítókat listája alább látható módon

**Példák**
```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
[Az IoT Hub lekérdezési nyelv] [ lnk-query] IoT-központ lekérdezési nyelv további részletesen ismerteti.

## <a name="jobs-tooupdate-device-twin-properties"></a>Feladatok tooupdate iker tulajdonságai
hello az alábbiakban látható a frissítési feladat használatával kettős eszköztulajdonságok hello HTTP 1.1 részletei:

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
hello az alábbiakban látható hello a HTTP 1.1 kérelemrészletek [feladatok lekérdezése][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```

a válaszban hello hello continuationToken valósul meg.  

## <a name="jobs-properties"></a>Feladatok tulajdonságai
hello tulajdonságok és a megfelelő leírását, amely lekérdezésekor feladatok vagy a feladat eredményeinek listája látható.

| Tulajdonság | Leírás |
| --- | --- |
| **a JobId értékének** |Alkalmazás azonosítója előírt hello feladat. |
| **Kezdő időpont** |Alkalmazás által biztosított hello feladat kezdési időpontja (ISO 8601). |
| **Befejezés időpontja** |Az IoT-központ megadott dátum (ISO 8601) hello feladat elvégzésekor. Csak azt követően hello feladat eléri 'tölteni' hello állapot érvényes. |
| **típusa** |Feladatok típusai: |
| **scheduledUpdateTwin**: A feladat használt tooupdate kívánt tulajdonságokkal vagy címkék. | |
| **scheduledDeviceMethod**: A feladat használt tooinvoke eszköz twins a megfelelő eszköz metódus. | |
| **állapot** |Hello feladat jelenlegi állapota. Az állapotot a lehetséges értékek: |
| **függőben lévő** : ütemezett és hello job szolgáltatás észlelnie várakozási toobe. | |
| **ütemezett** : hello későbbi időpontra ütemezve. | |
| **futó** : jelenleg aktív feladat. | |
| **megszakítva** : feladat meg lett szakítva. | |
| **nem sikerült** : feladat sikertelen volt. | |
| **befejeződött** : feladat befejeződött. | |
| **deviceJobStatistics** |Hello feladat végrehajtása statisztikája. |

**deviceJobStatistics** tulajdonságait.

| Tulajdonság | Leírás |
| --- | --- |
| **deviceJobStatistics.deviceCount** |Hello feladat eszközök számát. |
| **deviceJobStatistics.failedCount** |Ahol a hello feladat végrehajtása nem sikerült eszközök számát. |
| **deviceJobStatistics.succeededCount** |Ha a hello feladat sikeresen befejeződött eszközök számát. |
| **deviceJobStatistics.runningCount** |Hello feladat éppen futó eszközök számát. |
| **deviceJobStatistics.pendingCount** |Toorun hello feldolgozás alatt álló eszközök száma. |

### <a name="additional-reference-material"></a>További referenciaanyag
Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:

* [IoT-központok végpontjai] [ lnk-endpoints] ismerteti, hogy minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontok hello.
* [Sávszélesség-szabályozási és kvóták] [ lnk-quotas] toohello IoT-központ szolgáltatás és szabályozási viselkedés tooexpect hello hello szolgáltatás használatakor hello kvóták ismerteti.
* [Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] listák hello különböző nyelvi SDK-k, egy eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor használja.
* [Az IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ lnk-query] hello tooretrieve IoT Hub-ből származó adataival az eszköz twins és feladatok is használhatja az IoT-központ lekérdezési nyelv ismerteti.
* [Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] hello MQTT protokoll IoT-központ támogatásával kapcsolatos további információkat biztosít.

## <a name="next-steps"></a>Következő lépések
Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ az oktatóanyag következő hello iránt érdeklődik:

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

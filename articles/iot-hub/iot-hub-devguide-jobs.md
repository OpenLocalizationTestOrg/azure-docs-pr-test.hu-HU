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
# <a name="schedule-jobs-on-multiple-devices"></a><span data-ttu-id="ea8ee-104">Feladatok ütemezése több eszközön</span><span class="sxs-lookup"><span data-stu-id="ea8ee-104">Schedule jobs on multiple devices</span></span>
## <a name="overview"></a><span data-ttu-id="ea8ee-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ea8ee-105">Overview</span></span>
<span data-ttu-id="ea8ee-106">Szerint az előző cikket, Azure IoT-központ lehetővé teszi, hogy számos egyéb építőelemekből ([iker eszköztulajdonságok és címkék] [ lnk-twin-devguide] és [módszerek közvetlen][lnk-dev-methods]).</span><span class="sxs-lookup"><span data-stu-id="ea8ee-106">As described by previous articles, Azure IoT Hub enables a number of building blocks ([device twin properties and tags][lnk-twin-devguide] and [direct methods][lnk-dev-methods]).</span></span>  <span data-ttu-id="ea8ee-107">Háttér-alkalmazások általában engedélyezése az eszköz a rendszergazdák és a kezelők tooupdate, és az IoT-eszközök tömeges és ütemezett időpontban módosításának.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-107">Typically, back-end apps enable device administrators and operators tooupdate and interact with IoT devices in bulk and at a scheduled time.</span></span>  <span data-ttu-id="ea8ee-108">Feladatok ütemezése egyszerre iker eszközfrissítésekhez és az eszközök elleni közvetlen módszerek hello végrehajtási foglalják magukban.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-108">Jobs encapsulate hello execution of device twin updates and direct methods against a set of devices at a schedule time.</span></span>  <span data-ttu-id="ea8ee-109">Például egy olyan operátort használja egy háttér-alkalmazást, amely ehhez kezdeményezése és nyomon követheti a feladat tooreboot az eszközök a 43 és emelet 3 felépítése nem lenne zavaró toohello műveletek hello épület egyszerre.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-109">For example, an operator would use a back-end app that would initiate and track a job tooreboot a set of devices in building 43 and floor 3 at a time that would not be disruptive toohello operations of hello building.</span></span>

### <a name="when-toouse"></a><span data-ttu-id="ea8ee-110">Ha toouse</span><span class="sxs-lookup"><span data-stu-id="ea8ee-110">When toouse</span></span>
<span data-ttu-id="ea8ee-111">Érdemes lehet használatával feladatokkal: egy megoldás háttér igények tooschedule és nyomon követése előrehaladás eszköz a megfelelő tevékenységek következő hello bármelyikét:</span><span class="sxs-lookup"><span data-stu-id="ea8ee-111">Consider using jobs when: a solution back end needs tooschedule and track progress any of hello following activities on a set of device:</span></span>

* <span data-ttu-id="ea8ee-112">Eszköz kívánt tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="ea8ee-112">Update desired properties</span></span>
* <span data-ttu-id="ea8ee-113">Címkék frissítése</span><span class="sxs-lookup"><span data-stu-id="ea8ee-113">Update tags</span></span>
* <span data-ttu-id="ea8ee-114">Közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="ea8ee-114">Invoke direct methods</span></span>

## <a name="job-lifecycle"></a><span data-ttu-id="ea8ee-115">Feladat életciklusa</span><span class="sxs-lookup"><span data-stu-id="ea8ee-115">Job lifecycle</span></span>
<span data-ttu-id="ea8ee-116">Feladatok hello megoldás háttérrendszerének által kezdeményezett, és az IoT-központ által kezelt.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-116">Jobs are initiated by hello solution back end and maintained by IoT Hub.</span></span>  <span data-ttu-id="ea8ee-117">Egy feladat keresztül elérhető szolgáltatás URI is kezdeményezhető (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) és folyamatban van a egy végrehajtás alatt álló feladat keresztül elérhető szolgáltatás URI-lekérdezés (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).</span><span class="sxs-lookup"><span data-stu-id="ea8ee-117">You can initiate a job through a service-facing URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) and query for progress on an executing job through a service-facing URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).</span></span>  <span data-ttu-id="ea8ee-118">Egy feladat indítható, miután a feladatok lekérdezése lehetővé teszi, hogy hello háttér-alkalmazás toorefresh hello állapotának futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-118">Once a job is initiated, querying for jobs enables hello back-end app toorefresh hello status of running jobs.</span></span>

> [!NOTE]
> <span data-ttu-id="ea8ee-119">Elindít egy feladatot, nevét és értékeit tartalmazhatnak US-ASCII nyomtatható alfanumerikus, kivéve a következő set hello: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-119">When you initiate a job, property names and values can only contain US-ASCII printable alphanumeric, except any in hello following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="ea8ee-120">Referencia-témaköreit:</span><span class="sxs-lookup"><span data-stu-id="ea8ee-120">Reference topics:</span></span>
<span data-ttu-id="ea8ee-121">a következő témaköröket hello feladatok használatáról további információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-121">hello following reference topics provide you with more information about using jobs.</span></span>

## <a name="jobs-tooexecute-direct-methods"></a><span data-ttu-id="ea8ee-122">Feladatok tooexecute közvetlen módszer</span><span class="sxs-lookup"><span data-stu-id="ea8ee-122">Jobs tooexecute direct methods</span></span>
<span data-ttu-id="ea8ee-123">hello az alábbiakban látható a HTTP 1.1 hello kérelemrészletek hajthatók végre olyan [közvetlen módszer] [ lnk-dev-methods] meg az eszközök feladat használatával:</span><span class="sxs-lookup"><span data-stu-id="ea8ee-123">hello following is hello HTTP 1.1 request details for executing a [direct method][lnk-dev-methods] on a set of devices using a job:</span></span>

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
<span data-ttu-id="ea8ee-124">hello lekérdezés feltétel is lehet egyetlen eszközt azonosító vagy eszközazonosítókat listája alább látható módon</span><span class="sxs-lookup"><span data-stu-id="ea8ee-124">hello query condition can also be on a single device Id or on a list of device Ids as shown below</span></span>

<span data-ttu-id="ea8ee-125">**Példák**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-125">**Examples**</span></span>
```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
<span data-ttu-id="ea8ee-126">[Az IoT Hub lekérdezési nyelv] [ lnk-query] IoT-központ lekérdezési nyelv további részletesen ismerteti.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-126">[IoT Hub Query Language][lnk-query] covers IoT Hub query language in additional detail.</span></span>

## <a name="jobs-tooupdate-device-twin-properties"></a><span data-ttu-id="ea8ee-127">Feladatok tooupdate iker tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ea8ee-127">Jobs tooupdate device twin properties</span></span>
<span data-ttu-id="ea8ee-128">hello az alábbiakban látható a frissítési feladat használatával kettős eszköztulajdonságok hello HTTP 1.1 részletei:</span><span class="sxs-lookup"><span data-stu-id="ea8ee-128">hello following is hello HTTP 1.1 request details for updating device twin properties using a job:</span></span>

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

## <a name="querying-for-progress-on-jobs"></a><span data-ttu-id="ea8ee-129">Folyamatban van a feladatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="ea8ee-129">Querying for progress on jobs</span></span>
<span data-ttu-id="ea8ee-130">hello az alábbiakban látható hello a HTTP 1.1 kérelemrészletek [feladatok lekérdezése][lnk-query]:</span><span class="sxs-lookup"><span data-stu-id="ea8ee-130">hello following is hello HTTP 1.1 request details for [querying for jobs][lnk-query]:</span></span>

    ```
    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```

<span data-ttu-id="ea8ee-131">a válaszban hello hello continuationToken valósul meg.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-131">hello continuationToken is provided from hello response.</span></span>  

## <a name="jobs-properties"></a><span data-ttu-id="ea8ee-132">Feladatok tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ea8ee-132">Jobs Properties</span></span>
<span data-ttu-id="ea8ee-133">hello tulajdonságok és a megfelelő leírását, amely lekérdezésekor feladatok vagy a feladat eredményeinek listája látható.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-133">hello following is a list of properties and corresponding descriptions, which can be used when querying for jobs or job results.</span></span>

| <span data-ttu-id="ea8ee-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ea8ee-134">Property</span></span> | <span data-ttu-id="ea8ee-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="ea8ee-135">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ea8ee-136">**a JobId értékének**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-136">**jobId**</span></span> |<span data-ttu-id="ea8ee-137">Alkalmazás azonosítója előírt hello feladat.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-137">Application provided ID for hello job.</span></span> |
| <span data-ttu-id="ea8ee-138">**Kezdő időpont**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-138">**startTime**</span></span> |<span data-ttu-id="ea8ee-139">Alkalmazás által biztosított hello feladat kezdési időpontja (ISO 8601).</span><span class="sxs-lookup"><span data-stu-id="ea8ee-139">Application provided start time (ISO-8601) for hello job.</span></span> |
| <span data-ttu-id="ea8ee-140">**Befejezés időpontja**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-140">**endTime**</span></span> |<span data-ttu-id="ea8ee-141">Az IoT-központ megadott dátum (ISO 8601) hello feladat elvégzésekor.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-141">IoT Hub provided date (ISO-8601) for when hello job completed.</span></span> <span data-ttu-id="ea8ee-142">Csak azt követően hello feladat eléri 'tölteni' hello állapot érvényes.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-142">Valid only after hello job reaches hello 'completed' state.</span></span> |
| <span data-ttu-id="ea8ee-143">**típusa**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-143">**type**</span></span> |<span data-ttu-id="ea8ee-144">Feladatok típusai:</span><span class="sxs-lookup"><span data-stu-id="ea8ee-144">Types of jobs:</span></span> |
| <span data-ttu-id="ea8ee-145">**scheduledUpdateTwin**: A feladat használt tooupdate kívánt tulajdonságokkal vagy címkék.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-145">**scheduledUpdateTwin**: A job used tooupdate a set of desired properties or tags.</span></span> | |
| <span data-ttu-id="ea8ee-146">**scheduledDeviceMethod**: A feladat használt tooinvoke eszköz twins a megfelelő eszköz metódus.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-146">**scheduledDeviceMethod**: A job used tooinvoke a device method on a set of device twins.</span></span> | |
| <span data-ttu-id="ea8ee-147">**állapot**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-147">**status**</span></span> |<span data-ttu-id="ea8ee-148">Hello feladat jelenlegi állapota.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-148">Current state of hello job.</span></span> <span data-ttu-id="ea8ee-149">Az állapotot a lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="ea8ee-149">Possible values for status:</span></span> |
| <span data-ttu-id="ea8ee-150">**függőben lévő** : ütemezett és hello job szolgáltatás észlelnie várakozási toobe.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-150">**pending** : Scheduled and waiting toobe picked up by hello job service.</span></span> | |
| <span data-ttu-id="ea8ee-151">**ütemezett** : hello későbbi időpontra ütemezve.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-151">**scheduled** : Scheduled for a time in hello future.</span></span> | |
| <span data-ttu-id="ea8ee-152">**futó** : jelenleg aktív feladat.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-152">**running** : Currently active job.</span></span> | |
| <span data-ttu-id="ea8ee-153">**megszakítva** : feladat meg lett szakítva.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-153">**cancelled** : Job has been cancelled.</span></span> | |
| <span data-ttu-id="ea8ee-154">**nem sikerült** : feladat sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-154">**failed** : Job failed.</span></span> | |
| <span data-ttu-id="ea8ee-155">**befejeződött** : feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-155">**completed** : Job has completed.</span></span> | |
| <span data-ttu-id="ea8ee-156">**deviceJobStatistics**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-156">**deviceJobStatistics**</span></span> |<span data-ttu-id="ea8ee-157">Hello feladat végrehajtása statisztikája.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-157">Statistics about hello job's execution.</span></span> |

<span data-ttu-id="ea8ee-158">**deviceJobStatistics** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-158">**deviceJobStatistics** properties.</span></span>

| <span data-ttu-id="ea8ee-159">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ea8ee-159">Property</span></span> | <span data-ttu-id="ea8ee-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="ea8ee-160">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ea8ee-161">**deviceJobStatistics.deviceCount**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-161">**deviceJobStatistics.deviceCount**</span></span> |<span data-ttu-id="ea8ee-162">Hello feladat eszközök számát.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-162">Number of devices in hello job.</span></span> |
| <span data-ttu-id="ea8ee-163">**deviceJobStatistics.failedCount**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-163">**deviceJobStatistics.failedCount**</span></span> |<span data-ttu-id="ea8ee-164">Ahol a hello feladat végrehajtása nem sikerült eszközök számát.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-164">Number of devices where hello job failed.</span></span> |
| <span data-ttu-id="ea8ee-165">**deviceJobStatistics.succeededCount**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-165">**deviceJobStatistics.succeededCount**</span></span> |<span data-ttu-id="ea8ee-166">Ha a hello feladat sikeresen befejeződött eszközök számát.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-166">Number of devices where hello job succeeded.</span></span> |
| <span data-ttu-id="ea8ee-167">**deviceJobStatistics.runningCount**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-167">**deviceJobStatistics.runningCount**</span></span> |<span data-ttu-id="ea8ee-168">Hello feladat éppen futó eszközök számát.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-168">Number of devices that are currently running hello job.</span></span> |
| <span data-ttu-id="ea8ee-169">**deviceJobStatistics.pendingCount**</span><span class="sxs-lookup"><span data-stu-id="ea8ee-169">**deviceJobStatistics.pendingCount**</span></span> |<span data-ttu-id="ea8ee-170">Toorun hello feldolgozás alatt álló eszközök száma.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-170">Number of devices that are pending toorun hello job.</span></span> |

### <a name="additional-reference-material"></a><span data-ttu-id="ea8ee-171">További referenciaanyag</span><span class="sxs-lookup"><span data-stu-id="ea8ee-171">Additional reference material</span></span>
<span data-ttu-id="ea8ee-172">Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ea8ee-172">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="ea8ee-173">[IoT-központok végpontjai] [ lnk-endpoints] ismerteti, hogy minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontok hello.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-173">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="ea8ee-174">[Sávszélesség-szabályozási és kvóták] [ lnk-quotas] toohello IoT-központ szolgáltatás és szabályozási viselkedés tooexpect hello hello szolgáltatás használatakor hello kvóták ismerteti.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-174">[Throttling and quotas][lnk-quotas] describes hello quotas that apply toohello IoT Hub service and hello throttling behavior tooexpect when you use hello service.</span></span>
* <span data-ttu-id="ea8ee-175">[Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] listák hello különböző nyelvi SDK-k, egy eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor használja.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-175">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you an use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="ea8ee-176">[Az IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ lnk-query] hello tooretrieve IoT Hub-ből származó adataival az eszköz twins és feladatok is használhatja az IoT-központ lekérdezési nyelv ismerteti.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-176">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes hello IoT Hub query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="ea8ee-177">[Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] hello MQTT protokoll IoT-központ támogatásával kapcsolatos további információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="ea8ee-177">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea8ee-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ea8ee-178">Next steps</span></span>
<span data-ttu-id="ea8ee-179">Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ az oktatóanyag következő hello iránt érdeklődik:</span><span class="sxs-lookup"><span data-stu-id="ea8ee-179">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorial:</span></span>

* <span data-ttu-id="ea8ee-180">[Ütemezés és a feladatok][lnk-jobs-tutorial]</span><span class="sxs-lookup"><span data-stu-id="ea8ee-180">[Schedule and broadcast jobs][lnk-jobs-tutorial]</span></span>

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

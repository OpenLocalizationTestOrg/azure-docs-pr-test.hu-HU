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
# <a name="schedule-jobs-on-multiple-devices"></a><span data-ttu-id="fe3f1-104">Feladatok ütemezése több eszközön</span><span class="sxs-lookup"><span data-stu-id="fe3f1-104">Schedule jobs on multiple devices</span></span>
## <a name="overview"></a><span data-ttu-id="fe3f1-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fe3f1-105">Overview</span></span>
<span data-ttu-id="fe3f1-106">Szerint az előző cikket, Azure IoT-központ lehetővé teszi, hogy számos egyéb építőelemekből ([iker eszköztulajdonságok és címkék] [ lnk-twin-devguide] és [módszerek közvetlen][lnk-dev-methods]).</span><span class="sxs-lookup"><span data-stu-id="fe3f1-106">As described by previous articles, Azure IoT Hub enables a number of building blocks ([device twin properties and tags][lnk-twin-devguide] and [direct methods][lnk-dev-methods]).</span></span>  <span data-ttu-id="fe3f1-107">Háttér-alkalmazások általában eszközadminisztrátorok és operátorok frissítése, és az IoT-eszközök tömeges és ütemezett időpontban interaktívan tesznek lehetővé.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-107">Typically, back-end apps enable device administrators and operators to update and interact with IoT devices in bulk and at a scheduled time.</span></span>  <span data-ttu-id="fe3f1-108">Feladatok ütemezés egyszerre iker eszközfrissítésekhez és szemben az eszközök közvetlen metódusok végrehajtása foglalják magukban.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-108">Jobs encapsulate the execution of device twin updates and direct methods against a set of devices at a schedule time.</span></span>  <span data-ttu-id="fe3f1-109">Például egy olyan operátort használja egy háttér-alkalmazást, amely ehhez kezdeményezése és nyomon követheti a feladat újraindítja az eszközök a 43 és emelet 3 felépítése nem lenne az épület műveletekre zavaró egyszerre.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-109">For example, an operator would use a back-end app that would initiate and track a job to reboot a set of devices in building 43 and floor 3 at a time that would not be disruptive to the operations of the building.</span></span>

### <a name="when-to-use"></a><span data-ttu-id="fe3f1-110">A következő esetekben használja</span><span class="sxs-lookup"><span data-stu-id="fe3f1-110">When to use</span></span>
<span data-ttu-id="fe3f1-111">Érdemes lehet használatával feladatokkal: a megoldás háttérrendszere végén kell ütemezni, és nyomon követni egy csoportján eszköz a következő tevékenységek bármelyike:</span><span class="sxs-lookup"><span data-stu-id="fe3f1-111">Consider using jobs when: a solution back end needs to schedule and track progress any of the following activities on a set of device:</span></span>

* <span data-ttu-id="fe3f1-112">Eszköz kívánt tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="fe3f1-112">Update desired properties</span></span>
* <span data-ttu-id="fe3f1-113">Címkék frissítése</span><span class="sxs-lookup"><span data-stu-id="fe3f1-113">Update tags</span></span>
* <span data-ttu-id="fe3f1-114">Közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="fe3f1-114">Invoke direct methods</span></span>

## <a name="job-lifecycle"></a><span data-ttu-id="fe3f1-115">Feladat életciklusa</span><span class="sxs-lookup"><span data-stu-id="fe3f1-115">Job lifecycle</span></span>
<span data-ttu-id="fe3f1-116">Feladatok a megoldás háttérrendszeréhez által kezdeményezett, és az IoT-központ által kezelt.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-116">Jobs are initiated by the solution back end and maintained by IoT Hub.</span></span>  <span data-ttu-id="fe3f1-117">Egy feladat keresztül elérhető szolgáltatás URI is kezdeményezhető (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) és folyamatban van a egy végrehajtás alatt álló feladat keresztül elérhető szolgáltatás URI-lekérdezés (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).</span><span class="sxs-lookup"><span data-stu-id="fe3f1-117">You can initiate a job through a service-facing URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) and query for progress on an executing job through a service-facing URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).</span></span>  <span data-ttu-id="fe3f1-118">Egy feladat indítható, miután a feladatok lekérdezése lehetővé teszi, hogy a háttér-alkalmazásnak, hogy a futó feladatok állapotának frissítése.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-118">Once a job is initiated, querying for jobs enables the back-end app to refresh the status of running jobs.</span></span>

> [!NOTE]
> <span data-ttu-id="fe3f1-119">Elindít egy feladatot, nevét és értékeit tartalmazhatnak US-ASCII nyomtatható alfanumerikus, kivéve a következő set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-119">When you initiate a job, property names and values can only contain US-ASCII printable alphanumeric, except any in the following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="fe3f1-120">Referencia-témaköreit:</span><span class="sxs-lookup"><span data-stu-id="fe3f1-120">Reference topics:</span></span>
<span data-ttu-id="fe3f1-121">A következő témaköröket feladatok használatáról további információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-121">The following reference topics provide you with more information about using jobs.</span></span>

## <a name="jobs-to-execute-direct-methods"></a><span data-ttu-id="fe3f1-122">Közvetlen módszerek végrehajtandó feladatok</span><span class="sxs-lookup"><span data-stu-id="fe3f1-122">Jobs to execute direct methods</span></span>
<span data-ttu-id="fe3f1-123">Az alábbiakban található a HTTP 1.1 kérelemrészletek hajthatók végre olyan [közvetlen módszer] [ lnk-dev-methods] meg az eszközök feladat használatával:</span><span class="sxs-lookup"><span data-stu-id="fe3f1-123">The following is the HTTP 1.1 request details for executing a [direct method][lnk-dev-methods] on a set of devices using a job:</span></span>

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
<span data-ttu-id="fe3f1-124">A lekérdezés feltétel is lehet egyetlen eszközt azonosító vagy eszközazonosítók alább látható módon listája</span><span class="sxs-lookup"><span data-stu-id="fe3f1-124">The query condition can also be on a single device Id or on a list of device Ids as shown below</span></span>

<span data-ttu-id="fe3f1-125">**Példák**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-125">**Examples**</span></span>
```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
<span data-ttu-id="fe3f1-126">[Az IoT Hub lekérdezési nyelv] [ lnk-query] IoT-központ lekérdezési nyelv további részletesen ismerteti.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-126">[IoT Hub Query Language][lnk-query] covers IoT Hub query language in additional detail.</span></span>

## <a name="jobs-to-update-device-twin-properties"></a><span data-ttu-id="fe3f1-127">Feladatok eszköz iker tulajdonságainak módosítása</span><span class="sxs-lookup"><span data-stu-id="fe3f1-127">Jobs to update device twin properties</span></span>
<span data-ttu-id="fe3f1-128">A következő egy feladat használatával kettős eszköztulajdonságok frissítése a HTTP 1.1 kérelem részletei:</span><span class="sxs-lookup"><span data-stu-id="fe3f1-128">The following is the HTTP 1.1 request details for updating device twin properties using a job:</span></span>

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

## <a name="querying-for-progress-on-jobs"></a><span data-ttu-id="fe3f1-129">Folyamatban van a feladatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="fe3f1-129">Querying for progress on jobs</span></span>
<span data-ttu-id="fe3f1-130">Az alábbiakban található a HTTP 1.1 kérelem részletes [feladatok lekérdezése][lnk-query]:</span><span class="sxs-lookup"><span data-stu-id="fe3f1-130">The following is the HTTP 1.1 request details for [querying for jobs][lnk-query]:</span></span>

    ```
    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```

<span data-ttu-id="fe3f1-131">A válaszban szereplő a continuationToken valósul meg.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-131">The continuationToken is provided from the response.</span></span>  

## <a name="jobs-properties"></a><span data-ttu-id="fe3f1-132">Feladatok tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="fe3f1-132">Jobs Properties</span></span>
<span data-ttu-id="fe3f1-133">A következő tulajdonságokat és a megfelelő leírását, amely lekérdezésekor feladatok vagy a feladat eredményeinek listája látható.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-133">The following is a list of properties and corresponding descriptions, which can be used when querying for jobs or job results.</span></span>

| <span data-ttu-id="fe3f1-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="fe3f1-134">Property</span></span> | <span data-ttu-id="fe3f1-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="fe3f1-135">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fe3f1-136">**a JobId értékének**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-136">**jobId**</span></span> |<span data-ttu-id="fe3f1-137">Alkalmazás azonosítója megadva a feladathoz.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-137">Application provided ID for the job.</span></span> |
| <span data-ttu-id="fe3f1-138">**Kezdő időpont**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-138">**startTime**</span></span> |<span data-ttu-id="fe3f1-139">Alkalmazás által biztosított a feladat kezdési időpontja (ISO 8601).</span><span class="sxs-lookup"><span data-stu-id="fe3f1-139">Application provided start time (ISO-8601) for the job.</span></span> |
| <span data-ttu-id="fe3f1-140">**Befejezés időpontja**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-140">**endTime**</span></span> |<span data-ttu-id="fe3f1-141">Az IoT-központ dátuma (ISO 8601) Ha a feladat befejeződött-e megadva.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-141">IoT Hub provided date (ISO-8601) for when the job completed.</span></span> <span data-ttu-id="fe3f1-142">Csak azt követően a feladat eléri a "kész" állapot érvényes.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-142">Valid only after the job reaches the 'completed' state.</span></span> |
| <span data-ttu-id="fe3f1-143">**típusa**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-143">**type**</span></span> |<span data-ttu-id="fe3f1-144">Feladatok típusai:</span><span class="sxs-lookup"><span data-stu-id="fe3f1-144">Types of jobs:</span></span> |
| <span data-ttu-id="fe3f1-145">**scheduledUpdateTwin**: egy kívánt tulajdonságokkal vagy címkék frissítésére szolgáló feladatot.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-145">**scheduledUpdateTwin**: A job used to update a set of desired properties or tags.</span></span> | |
| <span data-ttu-id="fe3f1-146">**scheduledDeviceMethod**: egy eszköz twins a megfelelő eszköz metódus hívásához használt feladat.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-146">**scheduledDeviceMethod**: A job used to invoke a device method on a set of device twins.</span></span> | |
| <span data-ttu-id="fe3f1-147">**állapot**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-147">**status**</span></span> |<span data-ttu-id="fe3f1-148">A feladat jelenlegi állapota.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-148">Current state of the job.</span></span> <span data-ttu-id="fe3f1-149">Az állapotot a lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="fe3f1-149">Possible values for status:</span></span> |
| <span data-ttu-id="fe3f1-150">**függőben lévő** : ütemezett és váró észlelnie kell a feladat szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-150">**pending** : Scheduled and waiting to be picked up by the job service.</span></span> | |
| <span data-ttu-id="fe3f1-151">**ütemezett** : jövőbeli időpontra ütemezve.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-151">**scheduled** : Scheduled for a time in the future.</span></span> | |
| <span data-ttu-id="fe3f1-152">**futó** : jelenleg aktív feladat.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-152">**running** : Currently active job.</span></span> | |
| <span data-ttu-id="fe3f1-153">**megszakítva** : feladat meg lett szakítva.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-153">**cancelled** : Job has been cancelled.</span></span> | |
| <span data-ttu-id="fe3f1-154">**nem sikerült** : feladat sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-154">**failed** : Job failed.</span></span> | |
| <span data-ttu-id="fe3f1-155">**befejeződött** : feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-155">**completed** : Job has completed.</span></span> | |
| <span data-ttu-id="fe3f1-156">**deviceJobStatistics**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-156">**deviceJobStatistics**</span></span> |<span data-ttu-id="fe3f1-157">A feladat végrehajtása statisztikája.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-157">Statistics about the job's execution.</span></span> |

<span data-ttu-id="fe3f1-158">**deviceJobStatistics** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-158">**deviceJobStatistics** properties.</span></span>

| <span data-ttu-id="fe3f1-159">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="fe3f1-159">Property</span></span> | <span data-ttu-id="fe3f1-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="fe3f1-160">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fe3f1-161">**deviceJobStatistics.deviceCount**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-161">**deviceJobStatistics.deviceCount**</span></span> |<span data-ttu-id="fe3f1-162">A feladat eszközök számát.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-162">Number of devices in the job.</span></span> |
| <span data-ttu-id="fe3f1-163">**deviceJobStatistics.failedCount**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-163">**deviceJobStatistics.failedCount**</span></span> |<span data-ttu-id="fe3f1-164">Ha a feladat nem sikerült eszközök számát.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-164">Number of devices where the job failed.</span></span> |
| <span data-ttu-id="fe3f1-165">**deviceJobStatistics.succeededCount**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-165">**deviceJobStatistics.succeededCount**</span></span> |<span data-ttu-id="fe3f1-166">Ha a feladat sikeresen befejeződött eszközök számát.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-166">Number of devices where the job succeeded.</span></span> |
| <span data-ttu-id="fe3f1-167">**deviceJobStatistics.runningCount**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-167">**deviceJobStatistics.runningCount**</span></span> |<span data-ttu-id="fe3f1-168">A feladat éppen futó eszközök számát.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-168">Number of devices that are currently running the job.</span></span> |
| <span data-ttu-id="fe3f1-169">**deviceJobStatistics.pendingCount**</span><span class="sxs-lookup"><span data-stu-id="fe3f1-169">**deviceJobStatistics.pendingCount**</span></span> |<span data-ttu-id="fe3f1-170">A feladat futtatásához függőben lévő eszközök száma.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-170">Number of devices that are pending to run the job.</span></span> |

### <a name="additional-reference-material"></a><span data-ttu-id="fe3f1-171">További referenciaanyag</span><span class="sxs-lookup"><span data-stu-id="fe3f1-171">Additional reference material</span></span>
<span data-ttu-id="fe3f1-172">Az IoT Hub fejlesztői útmutató más hivatkozás témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="fe3f1-172">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="fe3f1-173">[IoT-központok végpontjai] [ lnk-endpoints] ismerteti a különböző végpontok, amelyek minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-173">[IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="fe3f1-174">[Sávszélesség-szabályozási és kvóták] [ lnk-quotas] ismerteti a kvótákat, az IoT-központ szolgáltatás és a sávszélesség-szabályozási viselkedését történik, ha a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-174">[Throttling and quotas][lnk-quotas] describes the quotas that apply to the IoT Hub service and the throttling behavior to expect when you use the service.</span></span>
* <span data-ttu-id="fe3f1-175">[Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] felsorolja a különböző nyelvi SDK-k, egy eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor használja.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-175">[Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you an use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="fe3f1-176">[Az IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ lnk-query] az IoT-központ lekérdezési nyelv segítségével adatok lekérését az IoT-központ az eszköz twins és feladatok ismertetése.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-176">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes the IoT Hub query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="fe3f1-177">[Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] IoT-központ támogatásával kapcsolatos további információkat biztosít a MQTT protokoll.</span><span class="sxs-lookup"><span data-stu-id="fe3f1-177">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe3f1-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe3f1-178">Next steps</span></span>
<span data-ttu-id="fe3f1-179">Ha azt szeretné, hogy próbálja ki azokat a jelen cikkben ismertetett fogalmakat, esetleg megváltozása a következő IoT Hub-oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="fe3f1-179">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorial:</span></span>

* <span data-ttu-id="fe3f1-180">[Ütemezés és a feladatok][lnk-jobs-tutorial]</span><span class="sxs-lookup"><span data-stu-id="fe3f1-180">[Schedule and broadcast jobs][lnk-jobs-tutorial]</span></span>

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

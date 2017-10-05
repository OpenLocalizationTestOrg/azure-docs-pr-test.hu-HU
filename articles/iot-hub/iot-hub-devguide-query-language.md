---
title: "Az Azure IoT Hub lekérdezési nyelv megismerése |} Microsoft Docs"
description: "Fejlesztői útmutató – az SQL-szerű IoT Hub lekérdezési nyelv eszköz twins és feladatok kapcsolatos információkat kérdezi le az IoT hub leírása."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 851a9ed3-b69e-422e-8a5d-1d79f91ddf15
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/17
ms.author: elioda
ms.openlocfilehash: a7650104eda58923558892f6f0f6666d16dbce28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a><span data-ttu-id="aa2de-103">Referencia - IoT-központ lekérdezési nyelv eszköz twins, a feladatok és az üzenet-útválasztás</span><span class="sxs-lookup"><span data-stu-id="aa2de-103">Reference - IoT Hub query language for device twins, jobs, and message routing</span></span>

<span data-ttu-id="aa2de-104">Az IoT-központ biztosít egy hatékony SQL-szerű nyelv való adatbeolvasás vonatkozó [eszköz twins] [ lnk-twins] és [feladatok][lnk-jobs], és [üzenet útválasztási][lnk-devguide-messaging-routes].</span><span class="sxs-lookup"><span data-stu-id="aa2de-104">IoT Hub provides a powerful SQL-like language to retrieve information regarding [device twins][lnk-twins] and [jobs][lnk-jobs], and [message routing][lnk-devguide-messaging-routes].</span></span> <span data-ttu-id="aa2de-105">Ez a cikk mutatja be:</span><span class="sxs-lookup"><span data-stu-id="aa2de-105">This article presents:</span></span>

* <span data-ttu-id="aa2de-106">Az IoT-központ lekérdező nyelv, a fő szolgáltatásainak bemutatása és</span><span class="sxs-lookup"><span data-stu-id="aa2de-106">An introduction to the major features of the IoT Hub query language, and</span></span>
* <span data-ttu-id="aa2de-107">A nyelv részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="aa2de-107">The detailed description of the language.</span></span>

## <a name="get-started-with-device-twin-queries"></a><span data-ttu-id="aa2de-108">Az eszköz iker lekérdezések első lépései</span><span class="sxs-lookup"><span data-stu-id="aa2de-108">Get started with device twin queries</span></span>
<span data-ttu-id="aa2de-109">[Eszköz twins] [ lnk-twins] tetszőleges JSON-objektumok címkék és a tulajdonságok is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="aa2de-109">[Device twins][lnk-twins] can contain arbitrary JSON objects as both tags and properties.</span></span> <span data-ttu-id="aa2de-110">Az IoT-központ lehetővé teszi lekérdezés eszköz twins JSON-dokumentumként egyetlen tartalmazó összes iker eszközadatokat.</span><span class="sxs-lookup"><span data-stu-id="aa2de-110">IoT Hub enables you to query device twins as a single JSON document containing all device twin information.</span></span>
<span data-ttu-id="aa2de-111">Tegyük fel például, hogy az IoT hub eszköz twins rendelkezik-e az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="aa2de-111">Assume, for instance, that your IoT hub device twins have the following structure:</span></span>

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        "location": {
            "region": "US",
            "plant": "Redmond43"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300
            },
            "$metadata": {
            ...
            },
            "$version": 4
        },
        "reported": {
            "connectivity": {
                "type": "cellular"
            },
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300,
                "status": "Success"
            },
            "$metadata": {
            ...
            },
            "$version": 7
        }
    }
}
```

<span data-ttu-id="aa2de-112">Az IoT-központ nevű dokumentum gyűjteményként elérhetővé teszi az eszköz twins **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="aa2de-112">IoT Hub exposes the device twins as a document collection called **devices**.</span></span>
<span data-ttu-id="aa2de-113">Ezért az alábbi lekérdezés lekéri az eszköz twins teljes készletét:</span><span class="sxs-lookup"><span data-stu-id="aa2de-113">So the following query retrieves the whole set of device twins:</span></span>

```sql
SELECT * FROM devices
```

> [!NOTE]
> <span data-ttu-id="aa2de-114">[Azure IoT SDK-k] [ lnk-hub-sdks] támogatja a nagyméretű eredmények lapozást.</span><span class="sxs-lookup"><span data-stu-id="aa2de-114">[Azure IoT SDKs][lnk-hub-sdks] support paging of large results.</span></span>

<span data-ttu-id="aa2de-115">Az IoT-központ lehetővé teszi tetszőleges feltételek szűrésének eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="aa2de-115">IoT Hub allows you to retrieve device twins filtering with arbitrary conditions.</span></span> <span data-ttu-id="aa2de-116">Például</span><span class="sxs-lookup"><span data-stu-id="aa2de-116">For instance,</span></span>

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

<span data-ttu-id="aa2de-117">beolvassa az eszköz twins rendelkező a **location.region** címke értéke **USA**.</span><span class="sxs-lookup"><span data-stu-id="aa2de-117">retrieves the device twins with the **location.region** tag set to **US**.</span></span>
<span data-ttu-id="aa2de-118">Logikai operátorok és aritmetikai összehasonlítások is támogatott, például</span><span class="sxs-lookup"><span data-stu-id="aa2de-118">Boolean operators and arithmetic comparisons are supported as well, for example</span></span>

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

<span data-ttu-id="aa2de-119">az Amerikai Egyesült Államokban legalább percenként telemetriai adatokat küldhet a konfigurált található összes eszköz twins kéri le.</span><span class="sxs-lookup"><span data-stu-id="aa2de-119">retrieves all device twins located in the US configured to send telemetry less often than every minute.</span></span> <span data-ttu-id="aa2de-120">A könnyebb is lehetőség az állandókat használja a **IN** és **nA** (nem szereplő) operátor.</span><span class="sxs-lookup"><span data-stu-id="aa2de-120">As a convenience, it is also possible to use array constants with the **IN** and **NIN** (not in) operators.</span></span> <span data-ttu-id="aa2de-121">Például</span><span class="sxs-lookup"><span data-stu-id="aa2de-121">For instance,</span></span>

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

<span data-ttu-id="aa2de-122">lekéri az összes eszköz twins jelentett Wi-Fi, vagy a vezetékes kapcsolati.</span><span class="sxs-lookup"><span data-stu-id="aa2de-122">retrieves all device twins that reported WiFi or wired connectivity.</span></span> <span data-ttu-id="aa2de-123">Legtöbbször az összes eszköz twins, amelyek tartalmaznak egy adott tulajdonságra azonosításához.</span><span class="sxs-lookup"><span data-stu-id="aa2de-123">It is often necessary to identify all device twins that contain a specific property.</span></span> <span data-ttu-id="aa2de-124">Az IoT-Központ támogatja a függvény `is_defined()` erre a célra.</span><span class="sxs-lookup"><span data-stu-id="aa2de-124">IoT Hub supports the function `is_defined()` for this purpose.</span></span> <span data-ttu-id="aa2de-125">Például</span><span class="sxs-lookup"><span data-stu-id="aa2de-125">For instance,</span></span>

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

<span data-ttu-id="aa2de-126">minden eszköz twins meghatározó beolvasni a `connectivity` jelentett tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="aa2de-126">retrieved all device twins that define the `connectivity` reported property.</span></span> <span data-ttu-id="aa2de-127">Tekintse meg a [WHERE záradék] [ lnk-query-where] a szűrési képességek a teljes referencia szakasza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-127">Refer to the [WHERE clause][lnk-query-where] section for the full reference of the filtering capabilities.</span></span>

<span data-ttu-id="aa2de-128">Csoportosítás és az aggregációhoz is támogatottak.</span><span class="sxs-lookup"><span data-stu-id="aa2de-128">Grouping and aggregations are also supported.</span></span> <span data-ttu-id="aa2de-129">Például</span><span class="sxs-lookup"><span data-stu-id="aa2de-129">For instance,</span></span>

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

<span data-ttu-id="aa2de-130">minden telemetriai állapotot az eszközök számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-130">returns the count of the devices in each telemetry configuration status.</span></span>

```json
[
    {
        "numberOfDevices": 3,
        "status": "Success"
    },
    {
        "numberOfDevices": 2,
        "status": "Pending"
    },
    {
        "numberOfDevices": 1,
        "status": "Error"
    }
]
```

<span data-ttu-id="aa2de-131">Az előző példában a helyzet, ahol három eszközöket jelentett sikeres konfigurációhoz, két továbbra is a konfiguráció alkalmazását, és egy hibát jelentett mutatja be.</span><span class="sxs-lookup"><span data-stu-id="aa2de-131">The preceding example illustrates a situation where three devices reported successful configuration, two are still applying the configuration, and one reported an error.</span></span>

### <a name="c-example"></a><span data-ttu-id="aa2de-132">C# – példa</span><span class="sxs-lookup"><span data-stu-id="aa2de-132">C# example</span></span>
<span data-ttu-id="aa2de-133">A lekérdezési funkciókat tesz elérhetővé a [C# szolgáltatás SDK] [ lnk-hub-sdks] a a **RegistryManager** osztály.</span><span class="sxs-lookup"><span data-stu-id="aa2de-133">The query functionality is exposed by the [C# service SDK][lnk-hub-sdks] in the **RegistryManager** class.</span></span>
<span data-ttu-id="aa2de-134">Íme egy példa egy egyszerű lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="aa2de-134">Here is an example of a simple query:</span></span>

```csharp
var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
while (query.HasMoreResults)
{
    var page = await query.GetNextAsTwinAsync();
    foreach (var twin in page)
    {
        // do work on twin object
    }
}
```

<span data-ttu-id="aa2de-135">Megjegyzés: az **lekérdezés** objektum létrejön az oldalméretet (legfeljebb 1000), és majd a több oldalra hívásával kérhető a **GetNextAsTwinAsync** módszerek több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="aa2de-135">Note how the **query** object is instantiated with a page size (up to 1000), and then multiple pages can be retrieved by calling the **GetNextAsTwinAsync** methods multiple times.</span></span>
<span data-ttu-id="aa2de-136">Vegye figyelembe, hogy a lekérdezés vezérlőnek több **következő\***, attól függően, a deszerializálás beállítást igényel, a lekérdezés, például a kettős vagy feladat eszközobjektumok, vagy az egyszerű JSON leképezések használata esetén használható.</span><span class="sxs-lookup"><span data-stu-id="aa2de-136">Note that the query object exposes multiple **Next\***, depending on the deserialization option required by the query, such as device twin or job objects, or plain JSON to be used when using projections.</span></span>

### <a name="nodejs-example"></a><span data-ttu-id="aa2de-137">NODE.js – példa</span><span class="sxs-lookup"><span data-stu-id="aa2de-137">Node.js example</span></span>
<span data-ttu-id="aa2de-138">A lekérdezési funkciókat tesz elérhetővé a [Azure IoT szolgáltatás SDK for Node.js] [ lnk-hub-sdks] a a **beállításjegyzék** objektum.</span><span class="sxs-lookup"><span data-stu-id="aa2de-138">The query functionality is exposed by the [Azure IoT service SDK for Node.js][lnk-hub-sdks] in the **Registry** object.</span></span>
<span data-ttu-id="aa2de-139">Íme egy példa egy egyszerű lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="aa2de-139">Here is an example of a simple query:</span></span>

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed to fetch the results: ' + err.message);
    } else {
        // Do something with the results
        results.forEach(function(twin) {
            console.log(twin.deviceId);
        });

        if (query.hasMoreResults) {
            query.nextAsTwin(onResults);
        }
    }
};
query.nextAsTwin(onResults);
```

<span data-ttu-id="aa2de-140">Megjegyzés: az **lekérdezés** objektum létrejön az oldalméretet (legfeljebb 1000), és majd a több oldalra hívásával kérhető a **nextAsTwin** módszerek több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="aa2de-140">Note how the **query** object is instantiated with a page size (up to 1000), and then multiple pages can be retrieved by calling the **nextAsTwin** methods multiple times.</span></span>
<span data-ttu-id="aa2de-141">Vegye figyelembe, hogy a lekérdezés vezérlőnek több **következő\***, attól függően, a deszerializálás beállítást igényel, a lekérdezés, például a kettős vagy feladat eszközobjektumok, vagy az egyszerű JSON leképezések használata esetén használható.</span><span class="sxs-lookup"><span data-stu-id="aa2de-141">Note that the query object exposes multiple **next\***, depending on the deserialization option required by the query, such as device twin or job objects, or plain JSON to be used when using projections.</span></span>

### <a name="limitations"></a><span data-ttu-id="aa2de-142">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="aa2de-142">Limitations</span></span>
> [!IMPORTANT]
> <span data-ttu-id="aa2de-143">Lekérdezés eredményei eszköz twins lehet néhány perc múlva késedelem vonatkozóan a legutóbbi értékét.</span><span class="sxs-lookup"><span data-stu-id="aa2de-143">Query results can have a few minutes of delay with respect to the latest values in device twins.</span></span> <span data-ttu-id="aa2de-144">Ha a lekérdezése egyes eszköz twins-azonosító szerint, célszerű mindig történő lekérése eszköz iker API, amely mindig a legfrissebb értéket tartalmaz, és magasabb szabályozás korlátok.</span><span class="sxs-lookup"><span data-stu-id="aa2de-144">If querying individual device twins by id, it is always preferable to use the retrieve device twin API, which always contains the latest values and has higher throttling limits.</span></span>

<span data-ttu-id="aa2de-145">Jelenleg összehasonlítások csak között támogatott primitív típusok (nincs objektumok), például `... WHERE properties.desired.config = properties.reported.config` csak támogatott, ha ezek a tulajdonságok egyszerű értékűek.</span><span class="sxs-lookup"><span data-stu-id="aa2de-145">Currently, comparisons are supported only between primitive types (no objects), for instance `... WHERE properties.desired.config = properties.reported.config` is supported only if those properties have primitive values.</span></span>

## <a name="get-started-with-jobs-queries"></a><span data-ttu-id="aa2de-146">Ismerkedés a feladatok lekérdezések</span><span class="sxs-lookup"><span data-stu-id="aa2de-146">Get started with jobs queries</span></span>
<span data-ttu-id="aa2de-147">[Feladatok] [ lnk-jobs] lehetőséget nyújtanak olyan meg az eszközök-műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="aa2de-147">[Jobs][lnk-jobs] provide a way to execute operations on sets of devices.</span></span> <span data-ttu-id="aa2de-148">Minden eszköz iker tartalmazza, amely egy nevű gyűjtemény részét képezi a feladatok **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="aa2de-148">Each device twin contains the information of the jobs of which it is part in a collection called **jobs**.</span></span>
<span data-ttu-id="aa2de-149">Logikailag,</span><span class="sxs-lookup"><span data-stu-id="aa2de-149">Logically,</span></span>

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        ...
    },
    "properties": {
        ...
    },
    "jobs": [
        {
            "deviceId": "myDeviceId",
            "jobId": "myJobId",
            "jobType": "scheduleTwinUpdate",
            "status": "completed",
            "startTimeUtc": "2016-09-29T18:18:52.7418462",
            "endTimeUtc": "2016-09-29T18:20:52.7418462",
            "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
            "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
            "outcome": {
                "deviceMethodResponse": null
            }
        },
        ...
    ]
}
```

<span data-ttu-id="aa2de-150">Ez a gyűjtemény jelenleg lekérdezhető mint **devices.jobs** az IoT-központ a lekérdezési nyelv.</span><span class="sxs-lookup"><span data-stu-id="aa2de-150">Currently, this collection is queryable as **devices.jobs** in the IoT Hub query language.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aa2de-151">A feladatok tulajdonság jelenleg nem vissza, ha lekérdezése eszköz twins (Ez azt jelenti, hogy lekérdezések tartalmaz "eszközök").</span><span class="sxs-lookup"><span data-stu-id="aa2de-151">Currently, the jobs property is never returned when querying device twins (that is, queries that contains 'FROM devices').</span></span> <span data-ttu-id="aa2de-152">Csak használható közvetlenül a lekérdezések használatával `FROM devices.jobs`.</span><span class="sxs-lookup"><span data-stu-id="aa2de-152">It can only be accessed directly with queries using `FROM devices.jobs`.</span></span>
>
>

<span data-ttu-id="aa2de-153">Például ahhoz, hogy az összes feladat (elmúlt és ütemezett), amely egyetlen eszközt érintik, használhatja a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="aa2de-153">For instance, to get all jobs (past and scheduled) that affect a single device, you can use the following query:</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

<span data-ttu-id="aa2de-154">Vegye figyelembe, hogy ez a lekérdezés biztosítja az eszközspecifikus állapota (és valószínűleg a közvetlen módszer válasz) minden visszaadott feladat.</span><span class="sxs-lookup"><span data-stu-id="aa2de-154">Note how this query provides the device-specific status (and possibly the direct method response) of each job returned.</span></span>
<span data-ttu-id="aa2de-155">Akkor is az összes objektum tulajdonság tetszőleges logikai feltételeknek szűrése a **devices.jobs** gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="aa2de-155">It is also possible to filter with arbitrary Boolean conditions on all object properties in the **devices.jobs** collection.</span></span>
<span data-ttu-id="aa2de-156">Például a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="aa2de-156">For instance, the following query:</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

<span data-ttu-id="aa2de-157">lekéri az összes befejezett eszköz két frissítési feladatok eszköz **myDeviceId** után 2016 szeptemberétől hozott létre.</span><span class="sxs-lookup"><span data-stu-id="aa2de-157">retrieves all completed device twin update jobs for device **myDeviceId** that were created after September 2016.</span></span>

<span data-ttu-id="aa2de-158">Akkor is egyetlen feladat eszközönkénti eredményeit beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aa2de-158">It is also possible to retrieve the per-device outcomes of a single job.</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a><span data-ttu-id="aa2de-159">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="aa2de-159">Limitations</span></span>
<span data-ttu-id="aa2de-160">A jelenleg, lekérdezi **devices.jobs** nem támogatják:</span><span class="sxs-lookup"><span data-stu-id="aa2de-160">Currently, queries on **devices.jobs** do not support:</span></span>

* <span data-ttu-id="aa2de-161">Leképezések, ezért csak `SELECT *` lehetséges.</span><span class="sxs-lookup"><span data-stu-id="aa2de-161">Projections, therefore only `SELECT *` is possible.</span></span>
* <span data-ttu-id="aa2de-162">Tekintse meg a feladat tulajdonságait (lásd az előző szakaszban) mellett az eszköz két feltételnek.</span><span class="sxs-lookup"><span data-stu-id="aa2de-162">Conditions that refer to the device twin in addition to job properties (see the preceding section).</span></span>
* <span data-ttu-id="aa2de-163">Összesítéseket, például a száma, avg, a csoportosítás alapját végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="aa2de-163">Performing aggregations, such as count, avg, group by.</span></span>

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a><span data-ttu-id="aa2de-164">Ismerkedés az eszközről a felhőbe üzenet útvonalak lekérdezési kifejezések</span><span class="sxs-lookup"><span data-stu-id="aa2de-164">Get started with device-to-cloud message routes query expressions</span></span>

<span data-ttu-id="aa2de-165">Használatával [eszközről a felhőbe útvonalak][lnk-devguide-messaging-routes], konfigurálhatja az IoT-központ átirányítani az egyes üzeneteket értékelni kifejezések alapján különböző végpontokhoz üzenetek eszközről a felhőbe.</span><span class="sxs-lookup"><span data-stu-id="aa2de-165">Using [device-to-cloud routes][lnk-devguide-messaging-routes], you can configure IoT Hub to dispatch device-to-cloud messages to different endpoints based on expressions evaluated against individual messages.</span></span>

<span data-ttu-id="aa2de-166">Az útvonal [feltétel] [ lnk-query-expressions] ugyanazt az IoT-központ a lekérdezés nyelvet használja, mint a feltételek iker és feladat lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="aa2de-166">The route [condition][lnk-query-expressions] uses the same IoT Hub query language as conditions in twin and job queries.</span></span> <span data-ttu-id="aa2de-167">Útvonal feltételek értékelésének üzenetfejlécek és törzse.</span><span class="sxs-lookup"><span data-stu-id="aa2de-167">Route conditions are evaluated on the message headers and body.</span></span> <span data-ttu-id="aa2de-168">Az útválasztási lekérdezési kifejezésben járhatnak csak üzenetfejlécek, csak az üzenettörzs vagy üzenet fejlécek és üzenet törzse.</span><span class="sxs-lookup"><span data-stu-id="aa2de-168">Your routing query expression may involve only message headers, only the message body, or both message headers and message body.</span></span> <span data-ttu-id="aa2de-169">Az IoT-központ azt feltételezi, hogy a fejlécek és az üzenet törzse egy adott séma útválasztásához üzeneteket, és a következő szakaszok ismertetik a megfelelő irányításához az IoT-központ szükséges:</span><span class="sxs-lookup"><span data-stu-id="aa2de-169">IoT Hub assumes a specific schema for the headers and message body in order to route messages, and the following sections describe what is required for IoT Hub to properly route:</span></span>

### <a name="routing-on-message-headers"></a><span data-ttu-id="aa2de-170">Fejlécek az Útválasztás</span><span class="sxs-lookup"><span data-stu-id="aa2de-170">Routing on message headers</span></span>

<span data-ttu-id="aa2de-171">Az IoT-központ azt feltételezi, hogy a következő JSON-ábrázolását üzenetfejlécek üzenet útválasztás:</span><span class="sxs-lookup"><span data-stu-id="aa2de-171">IoT Hub assumes the following JSON representation of message headers for message routing:</span></span>

```json
{
    "$messageId": "",
    "$enqueuedTime": "",
    "$to": "",
    "$expiryTimeUtc": "",
    "$correlationId": "",
    "$userId": "",
    "$ack": "",
    "$connectionDeviceId": "",
    "$connectionDeviceGenerationId": "",
    "$connectionAuthMethod": "",
    "$content-type": "",
    "$content-encoding": "",

    "userProperty1": "",
    "userProperty2": ""
}
```

<span data-ttu-id="aa2de-172">Üzenet Rendszertulajdonságok fűzve előtagként a `'$'` szimbólum.</span><span class="sxs-lookup"><span data-stu-id="aa2de-172">Message system properties are prefixed with the `'$'` symbol.</span></span>
<span data-ttu-id="aa2de-173">Felhasználói tulajdonságok a nevükkel mindig érhetők el.</span><span class="sxs-lookup"><span data-stu-id="aa2de-173">User properties are always accessed with their name.</span></span> <span data-ttu-id="aa2de-174">Ha egy felhasználó tulajdonságnév történik-e a rendszer tulajdonság egybe (például `$to`), a felhasználó tulajdonság veszi a a `$to` kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aa2de-174">If a user property name happens to coincide with a system property (such as `$to`), the user property will be retrieved with the `$to` expression.</span></span>
<span data-ttu-id="aa2de-175">A rendszer tulajdonság használatával zárójeleket mindig elérhető `{}`: például a kifejezés használható `{$to}` eléréséhez a rendszer tulajdonság `to`.</span><span class="sxs-lookup"><span data-stu-id="aa2de-175">You can always access the system property using brackets `{}`: for instance, you can use the expression `{$to}` to access the system property `to`.</span></span> <span data-ttu-id="aa2de-176">Zárójeles tulajdonságnevek mindig a megfelelő rendszer tulajdonság beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aa2de-176">Bracketed property names always retrieve the corresponding system property.</span></span>

<span data-ttu-id="aa2de-177">Ne feledje, hogy tulajdonságnevek megkülönböztetik a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="aa2de-177">Remember that property names are case insensitive.</span></span>

> [!NOTE]
> <span data-ttu-id="aa2de-178">Minden üzenet tulajdonságai olyan karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="aa2de-178">All message properties are strings.</span></span> <span data-ttu-id="aa2de-179">Rendszer tulajdonságai, lásd: a [– útmutató fejlesztőknek][lnk-devguide-messaging-format], jelenleg nem használható lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="aa2de-179">System properties, as described in the [developer guide][lnk-devguide-messaging-format], are currently not available to use in queries.</span></span>
>

<span data-ttu-id="aa2de-180">Ha például egy `messageType` tulajdonság, érdemes egy végpontot, és egy másik végpont az összes riasztás irányíthatja az összes telemetriai adat.</span><span class="sxs-lookup"><span data-stu-id="aa2de-180">For example, if you use a `messageType` property, you might want to route all telemetry to one endpoint, and all alerts to another endpoint.</span></span> <span data-ttu-id="aa2de-181">Írhat a telemetriai adatok továbbításához a következő kifejezést:</span><span class="sxs-lookup"><span data-stu-id="aa2de-181">You can write the following expression to route the telemetry:</span></span>

```sql
messageType = 'telemetry'
```

<span data-ttu-id="aa2de-182">És a figyelmeztető üzenetek a következő kifejezést:</span><span class="sxs-lookup"><span data-stu-id="aa2de-182">And the following expression to route the alert messages:</span></span>

```sql
messageType = 'alert'
```

<span data-ttu-id="aa2de-183">Logikai kifejezésen, és a funkciók is támogatottak.</span><span class="sxs-lookup"><span data-stu-id="aa2de-183">Boolean expressions and functions are also supported.</span></span> <span data-ttu-id="aa2de-184">Ez a funkció lehetővé teszi, hogy alapján megkülönböztetheti a súlyossági szintet, például:</span><span class="sxs-lookup"><span data-stu-id="aa2de-184">This feature enables you to distinguish between severity level, for example:</span></span>

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

<span data-ttu-id="aa2de-185">Tekintse meg a [kifejezés és feltételek] [ lnk-query-expressions] támogatott operátorok és funkciók teljes listája szakaszában.</span><span class="sxs-lookup"><span data-stu-id="aa2de-185">Refer to the [Expression and conditions][lnk-query-expressions] section for the full list of supported operators and functions.</span></span>

### <a name="routing-on-message-bodies"></a><span data-ttu-id="aa2de-186">Az üzenet törzse Útválasztás</span><span class="sxs-lookup"><span data-stu-id="aa2de-186">Routing on message bodies</span></span>

<span data-ttu-id="aa2de-187">Az IoT-központ csak irányíthatja a üzenettörzs alapján tartalmát, ha az üzenet törzse nem megfelelően formázott JSON-kódolású UTF-8, UTF-16 vagy UTF-32.</span><span class="sxs-lookup"><span data-stu-id="aa2de-187">IoT Hub can only route based on message body contents if the message body is properly formed JSON encoded in either UTF-8, UTF-16, or UTF-32.</span></span> <span data-ttu-id="aa2de-188">Meg kell adni az üzenet tartalomtípusa `application/json` és az IoT-központ az üzenet törzse tartalma alapján engedélyezi a üzenetfejlécek a támogatott UTF kódolások egyikét tartalmának kódolását.</span><span class="sxs-lookup"><span data-stu-id="aa2de-188">You must set the content type of the message to `application/json` and the content encoding to one of the supported UTF encodings in the message headers to allow IoT Hub to route the message based on the body contents.</span></span> <span data-ttu-id="aa2de-189">Ha a fejlécek egyikét nincs megadva, az IoT-központ nem kísérli meg bármely lekérdezési kifejezés használata esetén a szervezet az üzeneten való kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="aa2de-189">If either of the headers is not specified, IoT Hub will not attempt to evaluate any query expression involving the body against the message.</span></span> <span data-ttu-id="aa2de-190">Ha az üzenet nem egy JSON-üzenetet, vagy ha az üzenet nem adja meg a tartalom típusa és a tartalmának kódolását, Ön továbbra is használhatja üzenet-útválasztás a fejlécek alapján üzenet.</span><span class="sxs-lookup"><span data-stu-id="aa2de-190">If your message is not a JSON message, or if the message does not specify the content type and content encoding, you may still use message routing to route the message based on the message headers.</span></span>

<span data-ttu-id="aa2de-191">Használhat `$body` az üzenet a lekérdezési kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="aa2de-191">You can use `$body` in the query expression to route the message.</span></span> <span data-ttu-id="aa2de-192">Használhatja egyszerű törzs hivatkozást, törzs tömb referencia vagy több szervezet hivatkozást a lekérdezési kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="aa2de-192">You can use a simple body reference, body array reference, or multiple body references in the query expression.</span></span> <span data-ttu-id="aa2de-193">A lekérdezési kifejezésben kombinálhatja üzenet fejlécének hivatkozással törzs hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="aa2de-193">Your query expression can also combine a body reference with a message header reference.</span></span> <span data-ttu-id="aa2de-194">Például a következők minden érvényes lekérdezési kifejezések:</span><span class="sxs-lookup"><span data-stu-id="aa2de-194">For example, the following are all valid query expressions:</span></span>

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a><span data-ttu-id="aa2de-195">Az IoT-központ lekérdezést alapjai</span><span class="sxs-lookup"><span data-stu-id="aa2de-195">Basics of an IoT Hub query</span></span>
<span data-ttu-id="aa2de-196">Minden egyes IoT-központ lekérdezés áll egy jelöljön ki és FROM záradék használata nem kötelező HELYÉT és a GROUP BY záradékban.</span><span class="sxs-lookup"><span data-stu-id="aa2de-196">Every IoT Hub query consists of a SELECT and FROM clauses and by optional WHERE and GROUP BY clauses.</span></span> <span data-ttu-id="aa2de-197">A JSON-dokumentumok, például az eszköz twins gyűjteménye minden egyes lekérdezés futtatható.</span><span class="sxs-lookup"><span data-stu-id="aa2de-197">Every query is run on a collection of JSON documents, for example device twins.</span></span> <span data-ttu-id="aa2de-198">A FROM záradék azt jelzi, a dokumentum gyűjteményt, amelyben többször is meg kell (**eszközök** vagy **devices.jobs**).</span><span class="sxs-lookup"><span data-stu-id="aa2de-198">The FROM clause indicates the document collection to be iterated on (**devices** or **devices.jobs**).</span></span> <span data-ttu-id="aa2de-199">Ezt követően a WHERE záradékban a szűrő alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="aa2de-199">Then, the filter in the WHERE clause is applied.</span></span> <span data-ttu-id="aa2de-200">Az összesítéseket, ez a lépés szerint vannak csoportosítva sor jön létre a GROUP BY záradékban, és minden egyes csoport megadva, a SELECT záradékban megadottak szerint.</span><span class="sxs-lookup"><span data-stu-id="aa2de-200">With aggregations, the results of this step are grouped as specified in the GROUP BY clause and, for each group, a row is generated as specified in the SELECT clause.</span></span>

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a><span data-ttu-id="aa2de-201">FROM záradékban</span><span class="sxs-lookup"><span data-stu-id="aa2de-201">FROM clause</span></span>
<span data-ttu-id="aa2de-202">A **< from_specification > a** záradék csak két érték feltételezheti: **ESZKÖZÖKRŐL**, eszköz twins, lekérdezése vagy **devices.jobs a**, feladat eszközönkénti lekérdezése részletes adatokat.</span><span class="sxs-lookup"><span data-stu-id="aa2de-202">The **FROM <from_specification>** clause can assume only two values: **FROM devices**, to query device twins, or **FROM devices.jobs**, to query job per-device details.</span></span>

## <a name="where-clause"></a><span data-ttu-id="aa2de-203">A WHERE záradék</span><span class="sxs-lookup"><span data-stu-id="aa2de-203">WHERE clause</span></span>
<span data-ttu-id="aa2de-204">A **ahol < filter_condition >** záradék használata nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="aa2de-204">The **WHERE <filter_condition>** clause is optional.</span></span> <span data-ttu-id="aa2de-205">Meghatározza, hogy a JSON-dokumentumok FROM gyűjtemény egy vagy több feltételt meg kell felelnie a eredményének része.</span><span class="sxs-lookup"><span data-stu-id="aa2de-205">It specifies one or more conditions that the JSON documents in the FROM collection must satisfy to be included as part of the result.</span></span> <span data-ttu-id="aa2de-206">Bármely JSON-dokumentum ki kell értékelnie, hogy a megadott feltételeknek, az eredmény szerepeltetni a "true".</span><span class="sxs-lookup"><span data-stu-id="aa2de-206">Any JSON document must evaluate the specified conditions to "true" to be included in the result.</span></span>

<span data-ttu-id="aa2de-207">Az engedélyezett feltételek részében leírt [kifejezések és a kikötések][lnk-query-expressions].</span><span class="sxs-lookup"><span data-stu-id="aa2de-207">The allowed conditions are described in section [Expressions and conditions][lnk-query-expressions].</span></span>

## <a name="select-clause"></a><span data-ttu-id="aa2de-208">SELECT záradékban</span><span class="sxs-lookup"><span data-stu-id="aa2de-208">SELECT clause</span></span>
<span data-ttu-id="aa2de-209">A SELECT záradékban (**VÁLASSZA < select_list >**) megadása kötelező, és határozza meg, milyen értékeket olvassa be a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="aa2de-209">The SELECT clause (**SELECT <select_list>**) is mandatory and specifies what values are retrieved from the query.</span></span> <span data-ttu-id="aa2de-210">Azt adja meg az új JSON-objektumok létrehozásához használt JSON értékeket.</span><span class="sxs-lookup"><span data-stu-id="aa2de-210">It specifies the JSON values to be used to generate new JSON objects.</span></span>
<span data-ttu-id="aa2de-211">A FROM gyűjtemény szűrt (és nem kötelezően csoportosított) részhalmazát minden egyes elemhez a leképezés fázis állít elő, egy új JSON-objektum, a SELECT záradékban megadott értékek kialakítani.</span><span class="sxs-lookup"><span data-stu-id="aa2de-211">For each element of the filtered (and optionally grouped) subset of the FROM collection, the projection phase generates a new JSON object, constructed with the values specified in the SELECT clause.</span></span>

<span data-ttu-id="aa2de-212">Az alábbiakban látható a SELECT záradékban nyelvtani:</span><span class="sxs-lookup"><span data-stu-id="aa2de-212">Following is the grammar of the SELECT clause:</span></span>

```
SELECT [TOP <max number>] <projection list>

<projection_list> ::=
    '*'
    | <projection_element> AS alias [, <projection_element> AS alias]+

<projection_element> :==
    attribute_name
    | <projection_element> '.' attribute_name
    | <aggregate>

<aggregate> :==
    count()
    | avg(<projection_element>)
    | sum(<projection_element>)
    | min(<projection_element>)
    | max(<projection_element>)
```

<span data-ttu-id="aa2de-213">Ha **attribute_name** a JSON-dokumentum FROM gyűjtemény egyik tulajdonságnak sem hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="aa2de-213">where **attribute_name** refers to any property of the JSON document in the FROM collection.</span></span> <span data-ttu-id="aa2de-214">Néhány példa a SELECT záradékban található a [Ismerkedés az eszköz iker lekérdezések] [ lnk-query-getstarted] szakasz.</span><span class="sxs-lookup"><span data-stu-id="aa2de-214">Some examples of SELECT clauses can be found in the [Getting started with device twin queries][lnk-query-getstarted] section.</span></span>

<span data-ttu-id="aa2de-215">Jelenleg kijelölt záradékot eltérő **válasszon \***  csak az eszköz twins összesített lekérdezései támogat.</span><span class="sxs-lookup"><span data-stu-id="aa2de-215">Currently, selection clauses different than **SELECT \*** are only supported in aggregate queries on device twins.</span></span>

## <a name="group-by-clause"></a><span data-ttu-id="aa2de-216">GROUP BY záradékban</span><span class="sxs-lookup"><span data-stu-id="aa2de-216">GROUP BY clause</span></span>
<span data-ttu-id="aa2de-217">A **GROUP BY < group_specification >** záradék egy opcionális lépés, amely után a szűrő megadott szerepel a WHERE záradékban, és a leképezés kiválasztása megadott előtt hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="aa2de-217">The **GROUP BY <group_specification>** clause is an optional step that can be executed after the filter specified in the WHERE clause, and before the projection specified in the SELECT.</span></span> <span data-ttu-id="aa2de-218">Dokumentumok egy attribútum alapján csoportosítja azt.</span><span class="sxs-lookup"><span data-stu-id="aa2de-218">It groups documents based on the value of an attribute.</span></span> <span data-ttu-id="aa2de-219">Ezek a csoportok a SELECT záradékban megadott összesített értékek generálásához használt.</span><span class="sxs-lookup"><span data-stu-id="aa2de-219">These groups are used to generate aggregated values as specified in the SELECT clause.</span></span>

<span data-ttu-id="aa2de-220">A GROUP BY lekérdezést példa, hogy:</span><span class="sxs-lookup"><span data-stu-id="aa2de-220">An example of a query using GROUP BY is:</span></span>

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

<span data-ttu-id="aa2de-221">A formális GROUP BY szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="aa2de-221">The formal syntax for GROUP BY is:</span></span>

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

<span data-ttu-id="aa2de-222">Ha **attribute_name** a JSON-dokumentum FROM gyűjtemény egyik tulajdonságnak sem hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="aa2de-222">where **attribute_name** refers to any property of the JSON document in the FROM collection.</span></span>

<span data-ttu-id="aa2de-223">Jelenleg a GROUP BY záradék csak támogatott eszköz twins lekérdezésekor.</span><span class="sxs-lookup"><span data-stu-id="aa2de-223">Currently, the GROUP BY clause is only supported when querying device twins.</span></span>

## <a name="expressions-and-conditions"></a><span data-ttu-id="aa2de-224">Kifejezések és a feltételek</span><span class="sxs-lookup"><span data-stu-id="aa2de-224">Expressions and conditions</span></span>
<span data-ttu-id="aa2de-225">Magas szinten egy *kifejezés*:</span><span class="sxs-lookup"><span data-stu-id="aa2de-225">At a high level, an *expression*:</span></span>

* <span data-ttu-id="aa2de-226">(Például a logikai érték, számot, karakterlánc, a tömb vagy objektum), egy JSON-típus egy példánya értékelődik ki és</span><span class="sxs-lookup"><span data-stu-id="aa2de-226">Evaluates to an instance of a JSON type (such as Boolean, number, string, array, or object), and</span></span>
* <span data-ttu-id="aa2de-227">Határozza meg a JSON-dokumentum eszköz és a beépített operátorok és függvények használata állandók származó adatok kezelésére.</span><span class="sxs-lookup"><span data-stu-id="aa2de-227">Is defined by manipulating data coming from the device JSON document and constants using built-in operators and functions.</span></span>

<span data-ttu-id="aa2de-228">*Feltételek* kifejezések, amelyek kiértékelik logikai értékként.</span><span class="sxs-lookup"><span data-stu-id="aa2de-228">*Conditions* are expressions that evaluate to a Boolean.</span></span> <span data-ttu-id="aa2de-229">Bármely állandó eltér a logikai **igaz** minősül **hamis** (beleértve a **null**, **nem definiált**, bármely objektum vagy tömb példány bármilyen karakterlánc, és egyértelműen a logikai **hamis**).</span><span class="sxs-lookup"><span data-stu-id="aa2de-229">Any constant different than Boolean **true** is considered as **false** (including **null**, **undefined**, any object or array instance, any string, and clearly the Boolean **false**).</span></span>

<span data-ttu-id="aa2de-230">A kifejezés szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="aa2de-230">The syntax for expressions is:</span></span>

```
<expression> ::=
    <constant> |
    attribute_name |
    <function_call> |
    <expression> binary_operator <expression> |
    <create_array_expression> |
    '(' <expression> ')'

<function_call> ::=
    <function_name> '(' expression ')'

<constant> ::=
    <undefined_constant>
    | <null_constant>
    | <number_constant>
    | <string_constant>
    | <array_constant>

<undefined_constant> ::= undefined
<null_constant> ::= null
<number_constant> ::= decimal_literal | hexadecimal_literal
<string_constant> ::= string_literal
<array_constant> ::= '[' <constant> [, <constant>]+ ']'
```

<span data-ttu-id="aa2de-231">Ahol:</span><span class="sxs-lookup"><span data-stu-id="aa2de-231">where:</span></span>

| <span data-ttu-id="aa2de-232">Szimbólum</span><span class="sxs-lookup"><span data-stu-id="aa2de-232">Symbol</span></span> | <span data-ttu-id="aa2de-233">Meghatározás</span><span class="sxs-lookup"><span data-stu-id="aa2de-233">Definition</span></span> |
| --- | --- |
| <span data-ttu-id="aa2de-234">attribute_name</span><span class="sxs-lookup"><span data-stu-id="aa2de-234">attribute_name</span></span> | <span data-ttu-id="aa2de-235">A JSON-dokumentum található bármely tulajdonságát a **FROM** gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="aa2de-235">Any property of the JSON document in the **FROM** collection.</span></span> |
| <span data-ttu-id="aa2de-236">binary_operator</span><span class="sxs-lookup"><span data-stu-id="aa2de-236">binary_operator</span></span> | <span data-ttu-id="aa2de-237">A bináris operátor szerepel a [operátorok](#operators) szakasz.</span><span class="sxs-lookup"><span data-stu-id="aa2de-237">Any binary operator listed in the [Operators](#operators) section.</span></span> |
| <span data-ttu-id="aa2de-238">function_name</span><span class="sxs-lookup"><span data-stu-id="aa2de-238">function_name</span></span>| <span data-ttu-id="aa2de-239">Bármely függvény szerepel a [funkciók](#functions) szakasz.</span><span class="sxs-lookup"><span data-stu-id="aa2de-239">Any function listed in the [Functions](#functions) section.</span></span> |
| <span data-ttu-id="aa2de-240">decimal_literal</span><span class="sxs-lookup"><span data-stu-id="aa2de-240">decimal_literal</span></span> |<span data-ttu-id="aa2de-241">Egy lebegőpontos decimális jelöléssel kifejezve.</span><span class="sxs-lookup"><span data-stu-id="aa2de-241">A float expressed in decimal notation.</span></span> |
| <span data-ttu-id="aa2de-242">hexadecimal_literal</span><span class="sxs-lookup"><span data-stu-id="aa2de-242">hexadecimal_literal</span></span> |<span data-ttu-id="aa2de-243">Egy szám, a karakterlánc a "0 x" hexadecimális számjegyeket tartalmazó karakterlánc követ kifejezve.</span><span class="sxs-lookup"><span data-stu-id="aa2de-243">A number expressed by the string ‘0x’ followed by a string of hexadecimal digits.</span></span> |
| <span data-ttu-id="aa2de-244">string_literal</span><span class="sxs-lookup"><span data-stu-id="aa2de-244">string_literal</span></span> |<span data-ttu-id="aa2de-245">A szövegkonstansok olyan Unicode karakterláncok sorozatát nulla vagy több Unicode-karaktereket vagy escape-karaktersorozatokat.</span><span class="sxs-lookup"><span data-stu-id="aa2de-245">String literals are Unicode strings represented by a sequence of zero or more Unicode characters or escape sequences.</span></span> <span data-ttu-id="aa2de-246">A szövegkonstansok vannak szimpla zárójelek között (aposztróf: ") vagy dupla idézőjel (idézőjel:").</span><span class="sxs-lookup"><span data-stu-id="aa2de-246">String literals are enclosed in single quotes (apostrophe: ' ) or double quotes (quotation mark: ").</span></span> <span data-ttu-id="aa2de-247">Kilépés engedélyezett: `\'`, `\"`, `\\`, `\uXXXX` az Unicode karaktereket 4 hexadecimális számjegy határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="aa2de-247">Allowed escapes: `\'`, `\"`, `\\`, `\uXXXX` for Unicode characters defined by 4 hexadecimal digits.</span></span> |

### <a name="operators"></a><span data-ttu-id="aa2de-248">Operátorok</span><span class="sxs-lookup"><span data-stu-id="aa2de-248">Operators</span></span>
<span data-ttu-id="aa2de-249">Az alábbi műveleteket támogatja:</span><span class="sxs-lookup"><span data-stu-id="aa2de-249">The following operators are supported:</span></span>

| <span data-ttu-id="aa2de-250">Termékcsalád</span><span class="sxs-lookup"><span data-stu-id="aa2de-250">Family</span></span> | <span data-ttu-id="aa2de-251">Operátorok</span><span class="sxs-lookup"><span data-stu-id="aa2de-251">Operators</span></span> |
| --- | --- |
| <span data-ttu-id="aa2de-252">Aritmetikai</span><span class="sxs-lookup"><span data-stu-id="aa2de-252">Arithmetic</span></span> |<span data-ttu-id="aa2de-253">+,-,*,/,%</span><span class="sxs-lookup"><span data-stu-id="aa2de-253">+,-,*,/,%</span></span> |
| <span data-ttu-id="aa2de-254">Logikai</span><span class="sxs-lookup"><span data-stu-id="aa2de-254">Logical</span></span> |<span data-ttu-id="aa2de-255">ÉS, VAGY SEM</span><span class="sxs-lookup"><span data-stu-id="aa2de-255">AND, OR, NOT</span></span> |
| <span data-ttu-id="aa2de-256">Összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="aa2de-256">Comparison</span></span> |<span data-ttu-id="aa2de-257">=, !=, <, >, <=, >=, <></span><span class="sxs-lookup"><span data-stu-id="aa2de-257">=, !=, <, >, <=, >=, <></span></span> |

### <a name="functions"></a><span data-ttu-id="aa2de-258">Functions</span><span class="sxs-lookup"><span data-stu-id="aa2de-258">Functions</span></span>
<span data-ttu-id="aa2de-259">Twins és az egyetlen támogatott feladatok lekérdezésekor függvény van:</span><span class="sxs-lookup"><span data-stu-id="aa2de-259">When querying twins and jobs the only supported function is:</span></span>

| <span data-ttu-id="aa2de-260">Függvény</span><span class="sxs-lookup"><span data-stu-id="aa2de-260">Function</span></span> | <span data-ttu-id="aa2de-261">Leírás</span><span class="sxs-lookup"><span data-stu-id="aa2de-261">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="aa2de-262">IS_DEFINED(property)</span><span class="sxs-lookup"><span data-stu-id="aa2de-262">IS_DEFINED(property)</span></span> | <span data-ttu-id="aa2de-263">Jelző, ha a tulajdonság van rendelve egy érték logikai érték beolvasása (beleértve a `null`).</span><span class="sxs-lookup"><span data-stu-id="aa2de-263">Returns a Boolean indicating if the property has been assigned a value (including `null`).</span></span> |

<span data-ttu-id="aa2de-264">Útvonalak feltételek mellett a következő matematikai-funkciók támogatottak:</span><span class="sxs-lookup"><span data-stu-id="aa2de-264">In routes conditions, the following math functions are supported:</span></span>

| <span data-ttu-id="aa2de-265">Függvény</span><span class="sxs-lookup"><span data-stu-id="aa2de-265">Function</span></span> | <span data-ttu-id="aa2de-266">Leírás</span><span class="sxs-lookup"><span data-stu-id="aa2de-266">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="aa2de-267">ABS(x)</span><span class="sxs-lookup"><span data-stu-id="aa2de-267">ABS(x)</span></span> | <span data-ttu-id="aa2de-268">A megadott numerikus kifejezés (pozitív) abszolút értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-268">Returns the absolute (positive) value of the specified numeric expression.</span></span> |
| <span data-ttu-id="aa2de-269">Exp(x)</span><span class="sxs-lookup"><span data-stu-id="aa2de-269">EXP(x)</span></span> | <span data-ttu-id="aa2de-270">Az exponenciális a megadott numerikus kifejezés értékét adja vissza (e ^ x).</span><span class="sxs-lookup"><span data-stu-id="aa2de-270">Returns the exponential value of the specified numeric expression (e^x).</span></span> |
| <span data-ttu-id="aa2de-271">Power(x,y)</span><span class="sxs-lookup"><span data-stu-id="aa2de-271">POWER(x,y)</span></span> | <span data-ttu-id="aa2de-272">A megadott kifejezés értékét adja vissza a megadott hatványra (x ^ y).</span><span class="sxs-lookup"><span data-stu-id="aa2de-272">Returns the value of the specified expression to the specified power (x^y).</span></span>|
| <span data-ttu-id="aa2de-273">Square(x)</span><span class="sxs-lookup"><span data-stu-id="aa2de-273">SQUARE(x)</span></span> | <span data-ttu-id="aa2de-274">Kiszámítja a megadott numerikus érték.</span><span class="sxs-lookup"><span data-stu-id="aa2de-274">Returns the square of the specified numeric value.</span></span> |
| <span data-ttu-id="aa2de-275">CEILING(x)</span><span class="sxs-lookup"><span data-stu-id="aa2de-275">CEILING(x)</span></span> | <span data-ttu-id="aa2de-276">A legkisebb egész értéket ad vissza, nagyobb vagy egyenlő a megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aa2de-276">Returns the smallest integer value greater than, or equal to, the specified numeric expression.</span></span> |
| <span data-ttu-id="aa2de-277">FLOOR(x)</span><span class="sxs-lookup"><span data-stu-id="aa2de-277">FLOOR(x)</span></span> | <span data-ttu-id="aa2de-278">A legnagyobb egész számot ad vissza kisebb vagy egyenlő, mint a megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aa2de-278">Returns the largest integer less than or equal to the specified numeric expression.</span></span> |
| <span data-ttu-id="aa2de-279">SIGN(x)</span><span class="sxs-lookup"><span data-stu-id="aa2de-279">SIGN(x)</span></span> | <span data-ttu-id="aa2de-280">A pozitív (+ 1), nulla (0) vagy a megadott numerikus kifejezés mínuszjel (-1) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-280">Returns the positive (+1), zero (0), or negative (-1) sign of the specified numeric expression.</span></span>|
| <span data-ttu-id="aa2de-281">Sqrt(x)</span><span class="sxs-lookup"><span data-stu-id="aa2de-281">SQRT(x)</span></span> | <span data-ttu-id="aa2de-282">Kiszámítja a megadott numerikus érték.</span><span class="sxs-lookup"><span data-stu-id="aa2de-282">Returns the square of the specified numeric value.</span></span> |

<span data-ttu-id="aa2de-283">Útvonalak állapotától függően a következő típus ellenőrzése és adattípusokról funkciók támogatottak:</span><span class="sxs-lookup"><span data-stu-id="aa2de-283">In routes conditions, the following type checking and casting functions are supported:</span></span>

| <span data-ttu-id="aa2de-284">Függvény</span><span class="sxs-lookup"><span data-stu-id="aa2de-284">Function</span></span> | <span data-ttu-id="aa2de-285">Leírás</span><span class="sxs-lookup"><span data-stu-id="aa2de-285">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="aa2de-286">AS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="aa2de-286">AS_NUMBER</span></span> | <span data-ttu-id="aa2de-287">A bemeneti karakterlánc alakít egy számot.</span><span class="sxs-lookup"><span data-stu-id="aa2de-287">Converts the input string to a number.</span></span> <span data-ttu-id="aa2de-288">`noop`Ha a bemeneti érték egy szám; `Undefined` Ha karakterlánc nem felel meg egy számot.</span><span class="sxs-lookup"><span data-stu-id="aa2de-288">`noop` if input is a number; `Undefined` if string does not represent a number.</span></span>|
| <span data-ttu-id="aa2de-289">IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="aa2de-289">IS_ARRAY</span></span> | <span data-ttu-id="aa2de-290">Azt jelzi, hogy ha a megadott kifejezés típusú tömb egy logikai értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-290">Returns a Boolean value indicating if the type of the specified expression is an array.</span></span> |
| <span data-ttu-id="aa2de-291">IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="aa2de-291">IS_BOOL</span></span> | <span data-ttu-id="aa2de-292">Azt jelzi, hogy ha a megadott kifejezés típusa olyan logikai érték logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aa2de-292">Returns a Boolean value indicating if the type of the specified expression is a Boolean.</span></span> |
| <span data-ttu-id="aa2de-293">IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="aa2de-293">IS_DEFINED</span></span> | <span data-ttu-id="aa2de-294">Jelzi, ha a tulajdonság van rendelve egy érték logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="aa2de-294">Returns a Boolean indicating if the property has been assigned a value.</span></span> |
| <span data-ttu-id="aa2de-295">IS_NULL</span><span class="sxs-lookup"><span data-stu-id="aa2de-295">IS_NULL</span></span> | <span data-ttu-id="aa2de-296">Visszaad egy logikai értéket, amely azt jelzi, ha a megadott kifejezés típusa null.</span><span class="sxs-lookup"><span data-stu-id="aa2de-296">Returns a Boolean value indicating if the type of the specified expression is null.</span></span> |
| <span data-ttu-id="aa2de-297">IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="aa2de-297">IS_NUMBER</span></span> | <span data-ttu-id="aa2de-298">Azt jelzi, hogy ha a típus a megadott kifejezés több olyan logikai értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-298">Returns a Boolean value indicating if the type of the specified expression is a number.</span></span> |
| <span data-ttu-id="aa2de-299">IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="aa2de-299">IS_OBJECT</span></span> | <span data-ttu-id="aa2de-300">Azt jelzi, hogy ha a megadott kifejezés típusa egy JSON-objektum egy logikai értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-300">Returns a Boolean value indicating if the type of the specified expression is a JSON object.</span></span> |
| <span data-ttu-id="aa2de-301">IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="aa2de-301">IS_PRIMITIVE</span></span> | <span data-ttu-id="aa2de-302">Azt jelzi, hogy ha a megadott kifejezés típusa egy primitív egy logikai értéket ad vissza (string, Boolean, numerikus vagy `null`).</span><span class="sxs-lookup"><span data-stu-id="aa2de-302">Returns a Boolean value indicating if the type of the specified expression is a primitive (string, Boolean, numeric or `null`).</span></span> |
| <span data-ttu-id="aa2de-303">IS_STRING</span><span class="sxs-lookup"><span data-stu-id="aa2de-303">IS_STRING</span></span> | <span data-ttu-id="aa2de-304">Azt jelzi, hogy ha a megadott kifejezés típusa karakterlánc egy logikai értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-304">Returns a Boolean value indicating if the type of the specified expression is a string.</span></span> |

<span data-ttu-id="aa2de-305">Útvonalak feltételek a következő karakterlánc-funkciók támogatottak:</span><span class="sxs-lookup"><span data-stu-id="aa2de-305">In routes conditions, the following string functions are supported:</span></span>

| <span data-ttu-id="aa2de-306">Függvény</span><span class="sxs-lookup"><span data-stu-id="aa2de-306">Function</span></span> | <span data-ttu-id="aa2de-307">Leírás</span><span class="sxs-lookup"><span data-stu-id="aa2de-307">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="aa2de-308">CONCAT(x,...)</span><span class="sxs-lookup"><span data-stu-id="aa2de-308">CONCAT(x, …)</span></span> | <span data-ttu-id="aa2de-309">Karakterlánc, amely legalább két karakterlánc-értékek hozzáfűzésével eredményét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-309">Returns a string that is the result of concatenating two or more string values.</span></span> |
| <span data-ttu-id="aa2de-310">LENGTH(x)</span><span class="sxs-lookup"><span data-stu-id="aa2de-310">LENGTH(x)</span></span> | <span data-ttu-id="aa2de-311">A megadott karakterlánc-kifejezés karakterek számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-311">Returns the number of characters of the specified string expression.</span></span>|
| <span data-ttu-id="aa2de-312">Lower(x)</span><span class="sxs-lookup"><span data-stu-id="aa2de-312">LOWER(x)</span></span> | <span data-ttu-id="aa2de-313">Egy karakterlánc-kifejezés után nagybetűt adatok kisbetűssé alakításával adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-313">Returns a string expression after converting uppercase character data to lowercase.</span></span> |
| <span data-ttu-id="aa2de-314">Upper(x)</span><span class="sxs-lookup"><span data-stu-id="aa2de-314">UPPER(x)</span></span> | <span data-ttu-id="aa2de-315">Egy karakterlánc-kifejezés után kisbetűt adatok nagybetűssé alakításával adja vissza.</span><span class="sxs-lookup"><span data-stu-id="aa2de-315">Returns a string expression after converting lowercase character data to uppercase.</span></span> |
| <span data-ttu-id="aa2de-316">SUBSTRING (karakterlánc, start [, hossz])</span><span class="sxs-lookup"><span data-stu-id="aa2de-316">SUBSTRING(string, start [, length])</span></span> | <span data-ttu-id="aa2de-317">A megadott karakter nulla pozíciótól kezdődően karakterlánc-kifejezés részét adja vissza, és továbbra is fennáll, a megadott időtartam, illetve a karakterlánc végén.</span><span class="sxs-lookup"><span data-stu-id="aa2de-317">Returns part of a string expression starting at the specified character zero-based position and continues to the specified length, or to the end of the string.</span></span> |
| <span data-ttu-id="aa2de-318">(Karakterlánc, töredék) INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="aa2de-318">INDEX_OF(string, fragment)</span></span> | <span data-ttu-id="aa2de-319">A második első előfordulásának kezdőpozícióját adja vissza karakterlánc-kifejezés az első megadott karakterlánc-kifejezés vagy -1, ha a karakterlánc nem található.</span><span class="sxs-lookup"><span data-stu-id="aa2de-319">Returns the starting position of the first occurrence of the second string expression within the first specified string expression, or -1 if the string is not found.</span></span>|
| <span data-ttu-id="aa2de-320">STARTS_WITH (x, y)</span><span class="sxs-lookup"><span data-stu-id="aa2de-320">STARTS_WITH(x, y)</span></span> | <span data-ttu-id="aa2de-321">Visszaadja egy logikai, amely jelzi, hogy az első karakterlánc-kifejezés kezdődik-e a második.</span><span class="sxs-lookup"><span data-stu-id="aa2de-321">Returns a Boolean indicating whether the first string expression starts with the second.</span></span> |
| <span data-ttu-id="aa2de-322">ENDS_WITH (x, y)</span><span class="sxs-lookup"><span data-stu-id="aa2de-322">ENDS_WITH(x, y)</span></span> | <span data-ttu-id="aa2de-323">Adja vissza egy logikai, amely jelzi, hogy az első karakterlánc-kifejezés a második végződik.</span><span class="sxs-lookup"><span data-stu-id="aa2de-323">Returns a Boolean indicating whether the first string expression ends with the second.</span></span> |
| <span data-ttu-id="aa2de-324">CONTAINS(x,y)</span><span class="sxs-lookup"><span data-stu-id="aa2de-324">CONTAINS(x,y)</span></span> | <span data-ttu-id="aa2de-325">Visszaadja egy logikai, amely jelzi, hogy az első karakterlánc-kifejezés tartalmazza a második.</span><span class="sxs-lookup"><span data-stu-id="aa2de-325">Returns a Boolean indicating whether the first string expression contains the second.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="aa2de-326">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa2de-326">Next steps</span></span>
<span data-ttu-id="aa2de-327">Megtudhatja, hogyan hajtsa végre a lekérdezéseket az alkalmazások a [Azure IoT SDK-k][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="aa2de-327">Learn how to execute queries in your apps using [Azure IoT SDKs][lnk-hub-sdks].</span></span>

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#get-started-with-device-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging-routes]: iot-hub-devguide-messages-read-custom.md
[lnk-devguide-messaging-format]: iot-hub-devguide-messages-construct.md
[lnk-devguide-messaging-routes]: ./iot-hub-devguide-messages-read-custom.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md

---
title: "aaaUnderstand hello Azure IoT Hub lekérdezési nyelv |} Microsoft Docs"
description: "Fejlesztői útmutató - hello SQL-szerű IoT Hub lekérdezési nyelv leírása használt eszköz twins és az IoT hub-feladatok tooretrieve információt."
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
ms.openlocfilehash: 01a7c8ffdf44c6c27b834739d02c8fef1dd3d3fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a><span data-ttu-id="02d35-103">Referencia - IoT-központ lekérdezési nyelv eszköz twins, a feladatok és az üzenet-útválasztás</span><span class="sxs-lookup"><span data-stu-id="02d35-103">Reference - IoT Hub query language for device twins, jobs, and message routing</span></span>

<span data-ttu-id="02d35-104">Az IoT-központ tájékoztatást ad a hatékony SQL-szerű nyelv tooretrieve vonatkozó [eszköz twins] [ lnk-twins] és [feladatok][lnk-jobs], és [üzenet útválasztási][lnk-devguide-messaging-routes].</span><span class="sxs-lookup"><span data-stu-id="02d35-104">IoT Hub provides a powerful SQL-like language tooretrieve information regarding [device twins][lnk-twins] and [jobs][lnk-jobs], and [message routing][lnk-devguide-messaging-routes].</span></span> <span data-ttu-id="02d35-105">Ez a cikk mutatja be:</span><span class="sxs-lookup"><span data-stu-id="02d35-105">This article presents:</span></span>

* <span data-ttu-id="02d35-106">Egy bevezető toohello főbb hello IoT-központ lekérdező nyelv, szolgáltatásainak és</span><span class="sxs-lookup"><span data-stu-id="02d35-106">An introduction toohello major features of hello IoT Hub query language, and</span></span>
* <span data-ttu-id="02d35-107">hello részletes hello nyelvi leírása.</span><span class="sxs-lookup"><span data-stu-id="02d35-107">hello detailed description of hello language.</span></span>

## <a name="get-started-with-device-twin-queries"></a><span data-ttu-id="02d35-108">Az eszköz iker lekérdezések első lépései</span><span class="sxs-lookup"><span data-stu-id="02d35-108">Get started with device twin queries</span></span>
<span data-ttu-id="02d35-109">[Eszköz twins] [ lnk-twins] tetszőleges JSON-objektumok címkék és a tulajdonságok is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="02d35-109">[Device twins][lnk-twins] can contain arbitrary JSON objects as both tags and properties.</span></span> <span data-ttu-id="02d35-110">Az IoT-központ JSON-dokumentumként egyetlen összes iker Eszközadatokat tartalmazó lehetővé teszi tooquery eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="02d35-110">IoT Hub enables you tooquery device twins as a single JSON document containing all device twin information.</span></span>
<span data-ttu-id="02d35-111">Tegyük fel például, hogy az IoT hub eszköz twins rendelkezik-e a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="02d35-111">Assume, for instance, that your IoT hub device twins have hello following structure:</span></span>

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

<span data-ttu-id="02d35-112">Az IoT-központ hello eszköz twins mutatja egy nevű dokumentumgyűjteményt **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="02d35-112">IoT Hub exposes hello device twins as a document collection called **devices**.</span></span>
<span data-ttu-id="02d35-113">Ezért a következő lekérdezés hello lekéri hello teljes készletének eszközt twins:</span><span class="sxs-lookup"><span data-stu-id="02d35-113">So hello following query retrieves hello whole set of device twins:</span></span>

```sql
SELECT * FROM devices
```

> [!NOTE]
> <span data-ttu-id="02d35-114">[Azure IoT SDK-k] [ lnk-hub-sdks] támogatja a nagyméretű eredmények lapozást.</span><span class="sxs-lookup"><span data-stu-id="02d35-114">[Azure IoT SDKs][lnk-hub-sdks] support paging of large results.</span></span>

<span data-ttu-id="02d35-115">Az IoT-központ lehetővé teszi tetszőleges feltételek szűrésének tooretrieve eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="02d35-115">IoT Hub allows you tooretrieve device twins filtering with arbitrary conditions.</span></span> <span data-ttu-id="02d35-116">Például</span><span class="sxs-lookup"><span data-stu-id="02d35-116">For instance,</span></span>

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

<span data-ttu-id="02d35-117">lekéri a hello az eszköz twins hello **location.region** címke beállítása túl**USA**.</span><span class="sxs-lookup"><span data-stu-id="02d35-117">retrieves hello device twins with hello **location.region** tag set too**US**.</span></span>
<span data-ttu-id="02d35-118">Logikai operátorok és aritmetikai összehasonlítások is támogatott, például</span><span class="sxs-lookup"><span data-stu-id="02d35-118">Boolean operators and arithmetic comparisons are supported as well, for example</span></span>

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

<span data-ttu-id="02d35-119">hello VELÜNK konfigurált toosend telemetriai kisebb gyakran percenként található összes eszköz twins kéri le.</span><span class="sxs-lookup"><span data-stu-id="02d35-119">retrieves all device twins located in hello US configured toosend telemetry less often than every minute.</span></span> <span data-ttu-id="02d35-120">A könnyebb egyben a hello lehetséges toouse állandókat **IN** és **nA** (nem szereplő) operátor.</span><span class="sxs-lookup"><span data-stu-id="02d35-120">As a convenience, it is also possible toouse array constants with hello **IN** and **NIN** (not in) operators.</span></span> <span data-ttu-id="02d35-121">Például</span><span class="sxs-lookup"><span data-stu-id="02d35-121">For instance,</span></span>

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

<span data-ttu-id="02d35-122">lekéri az összes eszköz twins jelentett Wi-Fi, vagy a vezetékes kapcsolati.</span><span class="sxs-lookup"><span data-stu-id="02d35-122">retrieves all device twins that reported WiFi or wired connectivity.</span></span> <span data-ttu-id="02d35-123">Ez gyakran szükséges tooidentify minden eszköz twins, amelyek tartalmaznak egy adott tulajdonságra van.</span><span class="sxs-lookup"><span data-stu-id="02d35-123">It is often necessary tooidentify all device twins that contain a specific property.</span></span> <span data-ttu-id="02d35-124">Az IoT-Központ támogatja hello függvény `is_defined()` erre a célra.</span><span class="sxs-lookup"><span data-stu-id="02d35-124">IoT Hub supports hello function `is_defined()` for this purpose.</span></span> <span data-ttu-id="02d35-125">Például</span><span class="sxs-lookup"><span data-stu-id="02d35-125">For instance,</span></span>

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

<span data-ttu-id="02d35-126">minden eszköz twins hello meghatározó beolvasott `connectivity` jelentett tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="02d35-126">retrieved all device twins that define hello `connectivity` reported property.</span></span> <span data-ttu-id="02d35-127">Tekintse meg a toohello [WHERE záradék] [ lnk-query-where] szűrési lehetőségek hello szakasza hello teljes referenciaként.</span><span class="sxs-lookup"><span data-stu-id="02d35-127">Refer toohello [WHERE clause][lnk-query-where] section for hello full reference of hello filtering capabilities.</span></span>

<span data-ttu-id="02d35-128">Csoportosítás és az aggregációhoz is támogatottak.</span><span class="sxs-lookup"><span data-stu-id="02d35-128">Grouping and aggregations are also supported.</span></span> <span data-ttu-id="02d35-129">Például</span><span class="sxs-lookup"><span data-stu-id="02d35-129">For instance,</span></span>

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

<span data-ttu-id="02d35-130">minden telemetriai konfiguráció állapota hello eszközök hello számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="02d35-130">returns hello count of hello devices in each telemetry configuration status.</span></span>

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

<span data-ttu-id="02d35-131">hello előző példa azt mutatja be olyan helyzet, ahol három eszközöket jelentett sikeres konfigurációhoz, továbbra is alkalmazzák, két hello konfigurációs és egy hibát jelzett.</span><span class="sxs-lookup"><span data-stu-id="02d35-131">hello preceding example illustrates a situation where three devices reported successful configuration, two are still applying hello configuration, and one reported an error.</span></span>

### <a name="c-example"></a><span data-ttu-id="02d35-132">C# – példa</span><span class="sxs-lookup"><span data-stu-id="02d35-132">C# example</span></span>
<span data-ttu-id="02d35-133">hello lekérdezési funkciókat tesz elérhetővé hello [C# szolgáltatás SDK] [ lnk-hub-sdks] a hello **RegistryManager** osztály.</span><span class="sxs-lookup"><span data-stu-id="02d35-133">hello query functionality is exposed by hello [C# service SDK][lnk-hub-sdks] in hello **RegistryManager** class.</span></span>
<span data-ttu-id="02d35-134">Íme egy példa egy egyszerű lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="02d35-134">Here is an example of a simple query:</span></span>

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

<span data-ttu-id="02d35-135">Megjegyzés: hogyan hello **lekérdezés** (felfelé too1000) lap méretű objektum létrejön, és majd lekérhetők több lapokat hívó hello **GetNextAsTwinAsync** módszerek többször.</span><span class="sxs-lookup"><span data-stu-id="02d35-135">Note how hello **query** object is instantiated with a page size (up too1000), and then multiple pages can be retrieved by calling hello **GetNextAsTwinAsync** methods multiple times.</span></span>
<span data-ttu-id="02d35-136">Vegye figyelembe, hogy hello lekérdezés vezérlőnek több **következő\***, attól függően, hogy hello deszerializálása beállítás szükséges eszköz iker vagy feladat objektumok, például a hello lekérdezés vagy egyszerű JSON toobe leképezések használni.</span><span class="sxs-lookup"><span data-stu-id="02d35-136">Note that hello query object exposes multiple **Next\***, depending on hello deserialization option required by hello query, such as device twin or job objects, or plain JSON toobe used when using projections.</span></span>

### <a name="nodejs-example"></a><span data-ttu-id="02d35-137">NODE.js – példa</span><span class="sxs-lookup"><span data-stu-id="02d35-137">Node.js example</span></span>
<span data-ttu-id="02d35-138">hello lekérdezési funkciókat tesz elérhetővé hello [Azure IoT szolgáltatás SDK for Node.js] [ lnk-hub-sdks] a hello **beállításjegyzék** objektum.</span><span class="sxs-lookup"><span data-stu-id="02d35-138">hello query functionality is exposed by hello [Azure IoT service SDK for Node.js][lnk-hub-sdks] in hello **Registry** object.</span></span>
<span data-ttu-id="02d35-139">Íme egy példa egy egyszerű lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="02d35-139">Here is an example of a simple query:</span></span>

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed toofetch hello results: ' + err.message);
    } else {
        // Do something with hello results
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

<span data-ttu-id="02d35-140">Megjegyzés: hogyan hello **lekérdezés** (felfelé too1000) lap méretű objektum létrejön, és majd lekérhetők több lapokat hívó hello **nextAsTwin** módszerek többször.</span><span class="sxs-lookup"><span data-stu-id="02d35-140">Note how hello **query** object is instantiated with a page size (up too1000), and then multiple pages can be retrieved by calling hello **nextAsTwin** methods multiple times.</span></span>
<span data-ttu-id="02d35-141">Vegye figyelembe, hogy hello lekérdezés vezérlőnek több **következő\***, attól függően, hogy hello deszerializálása beállítás szükséges eszköz iker vagy feladat objektumok, például a hello lekérdezés vagy egyszerű JSON toobe leképezések használni.</span><span class="sxs-lookup"><span data-stu-id="02d35-141">Note that hello query object exposes multiple **next\***, depending on hello deserialization option required by hello query, such as device twin or job objects, or plain JSON toobe used when using projections.</span></span>

### <a name="limitations"></a><span data-ttu-id="02d35-142">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="02d35-142">Limitations</span></span>
> [!IMPORTANT]
> <span data-ttu-id="02d35-143">Lekérdezés eredményei eszköz twins tekintetben toohello legújabb értékekkel késedelem néhány perc is lehet.</span><span class="sxs-lookup"><span data-stu-id="02d35-143">Query results can have a few minutes of delay with respect toohello latest values in device twins.</span></span> <span data-ttu-id="02d35-144">Ha lekérdezése egyes eszköz twins-azonosító szerint, a rendszer mindig előnyösebb toouse hello eszköz iker API, amely mindig hello legújabb értékeket tartalmaz, és magasabb szabályozás korlátok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="02d35-144">If querying individual device twins by id, it is always preferable toouse hello retrieve device twin API, which always contains hello latest values and has higher throttling limits.</span></span>

<span data-ttu-id="02d35-145">Jelenleg összehasonlítások csak között támogatott primitív típusok (nincs objektumok), például `... WHERE properties.desired.config = properties.reported.config` csak támogatott, ha ezek a tulajdonságok egyszerű értékűek.</span><span class="sxs-lookup"><span data-stu-id="02d35-145">Currently, comparisons are supported only between primitive types (no objects), for instance `... WHERE properties.desired.config = properties.reported.config` is supported only if those properties have primitive values.</span></span>

## <a name="get-started-with-jobs-queries"></a><span data-ttu-id="02d35-146">Ismerkedés a feladatok lekérdezések</span><span class="sxs-lookup"><span data-stu-id="02d35-146">Get started with jobs queries</span></span>
<span data-ttu-id="02d35-147">[Feladatok] [ lnk-jobs] egy módon tooexecute meg az eszközök műveleteket biztosítsanak.</span><span class="sxs-lookup"><span data-stu-id="02d35-147">[Jobs][lnk-jobs] provide a way tooexecute operations on sets of devices.</span></span> <span data-ttu-id="02d35-148">Minden eszköz iker hello feladatok, amelyek nevű gyűjtemény részét képezi hello információkat tartalmaz **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="02d35-148">Each device twin contains hello information of hello jobs of which it is part in a collection called **jobs**.</span></span>
<span data-ttu-id="02d35-149">Logikailag,</span><span class="sxs-lookup"><span data-stu-id="02d35-149">Logically,</span></span>

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

<span data-ttu-id="02d35-150">Ez a gyűjtemény jelenleg lekérdezhető mint **devices.jobs** az IoT-központ lekérdezési nyelv hello.</span><span class="sxs-lookup"><span data-stu-id="02d35-150">Currently, this collection is queryable as **devices.jobs** in hello IoT Hub query language.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="02d35-151">Hello feladatok tulajdonság jelenleg, soha nem vissza, ha az eszköz twins (Ez azt jelenti, hogy lekérdezések "ESZKÖZÖKRŐL" tartalmazó) lekérdezését.</span><span class="sxs-lookup"><span data-stu-id="02d35-151">Currently, hello jobs property is never returned when querying device twins (that is, queries that contains 'FROM devices').</span></span> <span data-ttu-id="02d35-152">Csak használható közvetlenül a lekérdezések használatával `FROM devices.jobs`.</span><span class="sxs-lookup"><span data-stu-id="02d35-152">It can only be accessed directly with queries using `FROM devices.jobs`.</span></span>
>
>

<span data-ttu-id="02d35-153">Például tooget összes feladatot (elmúlt és ütemezett), amely egyetlen eszközt érintik, a következő lekérdezés hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="02d35-153">For instance, tooget all jobs (past and scheduled) that affect a single device, you can use hello following query:</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

<span data-ttu-id="02d35-154">Vegye figyelembe, hogy ez a lekérdezés biztosítja a hello eszközspecifikus állapota (és valószínűleg hello közvetlen metódusra adott válasz) minden visszaadott feladat.</span><span class="sxs-lookup"><span data-stu-id="02d35-154">Note how this query provides hello device-specific status (and possibly hello direct method response) of each job returned.</span></span>
<span data-ttu-id="02d35-155">Egyúttal a tetszőleges logikai feltétellel hello szereplő összes objektum tulajdonság lehetséges toofilter **devices.jobs** gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="02d35-155">It is also possible toofilter with arbitrary Boolean conditions on all object properties in hello **devices.jobs** collection.</span></span>
<span data-ttu-id="02d35-156">Például hello a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="02d35-156">For instance, hello following query:</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

<span data-ttu-id="02d35-157">lekéri az összes befejezett eszköz két frissítési feladatok eszköz **myDeviceId** után 2016 szeptemberétől hozott létre.</span><span class="sxs-lookup"><span data-stu-id="02d35-157">retrieves all completed device twin update jobs for device **myDeviceId** that were created after September 2016.</span></span>

<span data-ttu-id="02d35-158">Akkor is lehetséges tooretrieve hello eszközönkénti eredményeit egyetlen feladat.</span><span class="sxs-lookup"><span data-stu-id="02d35-158">It is also possible tooretrieve hello per-device outcomes of a single job.</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a><span data-ttu-id="02d35-159">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="02d35-159">Limitations</span></span>
<span data-ttu-id="02d35-160">A jelenleg, lekérdezi **devices.jobs** nem támogatják:</span><span class="sxs-lookup"><span data-stu-id="02d35-160">Currently, queries on **devices.jobs** do not support:</span></span>

* <span data-ttu-id="02d35-161">Leképezések, ezért csak `SELECT *` lehetséges.</span><span class="sxs-lookup"><span data-stu-id="02d35-161">Projections, therefore only `SELECT *` is possible.</span></span>
* <span data-ttu-id="02d35-162">Az feltételeket, amelyek toohello eszköz iker hozzáadása toojob tulajdonságai (lásd a szakasz fenti hello) hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="02d35-162">Conditions that refer toohello device twin in addition toojob properties (see hello preceding section).</span></span>
* <span data-ttu-id="02d35-163">Összesítéseket, például a száma, avg, a csoportosítás alapját végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="02d35-163">Performing aggregations, such as count, avg, group by.</span></span>

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a><span data-ttu-id="02d35-164">Ismerkedés az eszközről a felhőbe üzenet útvonalak lekérdezési kifejezések</span><span class="sxs-lookup"><span data-stu-id="02d35-164">Get started with device-to-cloud message routes query expressions</span></span>

<span data-ttu-id="02d35-165">Használatával [eszközről a felhőbe útvonalak][lnk-devguide-messaging-routes], IoT-központ toodispatch eszközről-a-felhőbe üzenetek az egyes üzeneteket értékelni kifejezések alapján toodifferent végpontok is beállíthat.</span><span class="sxs-lookup"><span data-stu-id="02d35-165">Using [device-to-cloud routes][lnk-devguide-messaging-routes], you can configure IoT Hub toodispatch device-to-cloud messages toodifferent endpoints based on expressions evaluated against individual messages.</span></span>

<span data-ttu-id="02d35-166">hello útvonal [feltétel] [ lnk-query-expressions] használja a két-és feladat feltételek megegyező IoT-központ lekérdezés nyelvű hello.</span><span class="sxs-lookup"><span data-stu-id="02d35-166">hello route [condition][lnk-query-expressions] uses hello same IoT Hub query language as conditions in twin and job queries.</span></span> <span data-ttu-id="02d35-167">Útvonal feltételek értékelésének hello üzenetfejlécek és törzse.</span><span class="sxs-lookup"><span data-stu-id="02d35-167">Route conditions are evaluated on hello message headers and body.</span></span> <span data-ttu-id="02d35-168">Az útválasztási lekérdezési kifejezésben csak üzenetfejlécek, csak egy hello üzenettörzs járhatnak vagy üzenet fejlécek és üzenet törzse.</span><span class="sxs-lookup"><span data-stu-id="02d35-168">Your routing query expression may involve only message headers, only hello message body, or both message headers and message body.</span></span> <span data-ttu-id="02d35-169">Az IoT-központ azt feltételezi, hogy egy adott séma hello fejlécek és az üzenet törzse rendelés tooroute üzenetek, és hello a következő szakaszok ismertetik az IoT-központ tooproperly útvonal szükséges:</span><span class="sxs-lookup"><span data-stu-id="02d35-169">IoT Hub assumes a specific schema for hello headers and message body in order tooroute messages, and hello following sections describe what is required for IoT Hub tooproperly route:</span></span>

### <a name="routing-on-message-headers"></a><span data-ttu-id="02d35-170">Fejlécek az Útválasztás</span><span class="sxs-lookup"><span data-stu-id="02d35-170">Routing on message headers</span></span>

<span data-ttu-id="02d35-171">Az IoT-központ azt feltételezi, hogy a következő üzenet irányításához üzenetfejlécek JSON-ábrázolását hello:</span><span class="sxs-lookup"><span data-stu-id="02d35-171">IoT Hub assumes hello following JSON representation of message headers for message routing:</span></span>

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

<span data-ttu-id="02d35-172">Üzenet Rendszertulajdonságok fűzve előtagként hello `'$'` szimbólum.</span><span class="sxs-lookup"><span data-stu-id="02d35-172">Message system properties are prefixed with hello `'$'` symbol.</span></span>
<span data-ttu-id="02d35-173">Felhasználói tulajdonságok a nevükkel mindig érhetők el.</span><span class="sxs-lookup"><span data-stu-id="02d35-173">User properties are always accessed with their name.</span></span> <span data-ttu-id="02d35-174">Ha egy felhasználó tulajdonságnév történik-e a rendszer tulajdonsággal toocoincide (például `$to`), a hello veszi hello felhasználói tulajdonság `$to` kifejezés.</span><span class="sxs-lookup"><span data-stu-id="02d35-174">If a user property name happens toocoincide with a system property (such as `$to`), hello user property will be retrieved with hello `$to` expression.</span></span>
<span data-ttu-id="02d35-175">Mindig elérhető hello rendszer tulajdonságon keresztül zárójeleket `{}`: például hello kifejezés használható `{$to}` tooaccess hello rendszer tulajdonság `to`.</span><span class="sxs-lookup"><span data-stu-id="02d35-175">You can always access hello system property using brackets `{}`: for instance, you can use hello expression `{$to}` tooaccess hello system property `to`.</span></span> <span data-ttu-id="02d35-176">Zárójeles tulajdonságnevek mindig hello megfelelő rendszer tulajdonság beolvasása.</span><span class="sxs-lookup"><span data-stu-id="02d35-176">Bracketed property names always retrieve hello corresponding system property.</span></span>

<span data-ttu-id="02d35-177">Ne feledje, hogy tulajdonságnevek megkülönböztetik a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="02d35-177">Remember that property names are case insensitive.</span></span>

> [!NOTE]
> <span data-ttu-id="02d35-178">Minden üzenet tulajdonságai olyan karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="02d35-178">All message properties are strings.</span></span> <span data-ttu-id="02d35-179">Rendszer tulajdonságai, a hello [– útmutató fejlesztőknek][lnk-devguide-messaging-format], jelenleg nem érhető el toouse lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="02d35-179">System properties, as described in hello [developer guide][lnk-devguide-messaging-format], are currently not available toouse in queries.</span></span>
>

<span data-ttu-id="02d35-180">Ha például egy `messageType` tulajdonság, érdemes tooroute összes telemetriai tooone végpont, és minden riasztások tooanother végpont.</span><span class="sxs-lookup"><span data-stu-id="02d35-180">For example, if you use a `messageType` property, you might want tooroute all telemetry tooone endpoint, and all alerts tooanother endpoint.</span></span> <span data-ttu-id="02d35-181">A következő kifejezés tooroute hello telemetriai hello írhat be:</span><span class="sxs-lookup"><span data-stu-id="02d35-181">You can write hello following expression tooroute hello telemetry:</span></span>

```sql
messageType = 'telemetry'
```

<span data-ttu-id="02d35-182">És a következő kifejezés tooroute hello figyelmeztető üzenetek hello:</span><span class="sxs-lookup"><span data-stu-id="02d35-182">And hello following expression tooroute hello alert messages:</span></span>

```sql
messageType = 'alert'
```

<span data-ttu-id="02d35-183">Logikai kifejezésen, és a funkciók is támogatottak.</span><span class="sxs-lookup"><span data-stu-id="02d35-183">Boolean expressions and functions are also supported.</span></span> <span data-ttu-id="02d35-184">Ez a funkció lehetővé teszi, hogy toodistinguish közötti súlyossági szintet, például:</span><span class="sxs-lookup"><span data-stu-id="02d35-184">This feature enables you toodistinguish between severity level, for example:</span></span>

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

<span data-ttu-id="02d35-185">Tekintse meg a toohello [kifejezés és feltételek] [ lnk-query-expressions] hello teljes lista szakasza támogatott operátort és függvényt.</span><span class="sxs-lookup"><span data-stu-id="02d35-185">Refer toohello [Expression and conditions][lnk-query-expressions] section for hello full list of supported operators and functions.</span></span>

### <a name="routing-on-message-bodies"></a><span data-ttu-id="02d35-186">Az üzenet törzse Útválasztás</span><span class="sxs-lookup"><span data-stu-id="02d35-186">Routing on message bodies</span></span>

<span data-ttu-id="02d35-187">Az IoT-központ csak irányíthatja a üzenettörzs alapján tartalmát, ha hello az üzenet törzse nem megfelelően formázott JSON-kódolású UTF-8, UTF-16 vagy UTF-32.</span><span class="sxs-lookup"><span data-stu-id="02d35-187">IoT Hub can only route based on message body contents if hello message body is properly formed JSON encoded in either UTF-8, UTF-16, or UTF-32.</span></span> <span data-ttu-id="02d35-188">Be kell állítani az üdvözlő üzenet tartalomtípusa hello túl`application/json` és a hello kódolási tooone tartalom hello támogatott UTF kódolások hello üzenet fejlécek tooallow IoT-központ tooroute üdvözlőüzenetére hello törzs tartalma alapján.</span><span class="sxs-lookup"><span data-stu-id="02d35-188">You must set hello content type of hello message too`application/json` and hello content encoding tooone of hello supported UTF encodings in hello message headers tooallow IoT Hub tooroute hello message based on hello body contents.</span></span> <span data-ttu-id="02d35-189">Ha hello fejlécek egyikét nincs megadva, az IoT-központ nem kísérli meg tooevaluate bármely hello törzs elleni üdvözlőüzenetére érintő lekérdezési kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="02d35-189">If either of hello headers is not specified, IoT Hub will not attempt tooevaluate any query expression involving hello body against hello message.</span></span> <span data-ttu-id="02d35-190">Ha az üzenet nem egy JSON-üzenetet, vagy üdvözlőüzenetére nem adja meg a hello tartalomtípus és tartalmának kódolását, Ön továbbra is használhatja üzenettovábbítás tooroute üdvözlőüzenetére hello fejlécek alapján.</span><span class="sxs-lookup"><span data-stu-id="02d35-190">If your message is not a JSON message, or if hello message does not specify hello content type and content encoding, you may still use message routing tooroute hello message based on hello message headers.</span></span>

<span data-ttu-id="02d35-191">Használhat `$body` hello lekérdezési kifejezés tooroute hello üzenetben.</span><span class="sxs-lookup"><span data-stu-id="02d35-191">You can use `$body` in hello query expression tooroute hello message.</span></span> <span data-ttu-id="02d35-192">Használhatja egyszerű törzs hivatkozást, törzs tömb referencia vagy több szervezet hivatkozást hello a lekérdezési kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="02d35-192">You can use a simple body reference, body array reference, or multiple body references in hello query expression.</span></span> <span data-ttu-id="02d35-193">A lekérdezési kifejezésben kombinálhatja üzenet fejlécének hivatkozással törzs hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="02d35-193">Your query expression can also combine a body reference with a message header reference.</span></span> <span data-ttu-id="02d35-194">Például hello az alábbiakban az összes érvényes lekérdezési kifejezések:</span><span class="sxs-lookup"><span data-stu-id="02d35-194">For example, hello following are all valid query expressions:</span></span>

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a><span data-ttu-id="02d35-195">Az IoT-központ lekérdezést alapjai</span><span class="sxs-lookup"><span data-stu-id="02d35-195">Basics of an IoT Hub query</span></span>
<span data-ttu-id="02d35-196">Minden egyes IoT-központ lekérdezés áll egy jelöljön ki és FROM záradék használata nem kötelező HELYÉT és a GROUP BY záradékban.</span><span class="sxs-lookup"><span data-stu-id="02d35-196">Every IoT Hub query consists of a SELECT and FROM clauses and by optional WHERE and GROUP BY clauses.</span></span> <span data-ttu-id="02d35-197">A JSON-dokumentumok, például az eszköz twins gyűjteménye minden egyes lekérdezés futtatható.</span><span class="sxs-lookup"><span data-stu-id="02d35-197">Every query is run on a collection of JSON documents, for example device twins.</span></span> <span data-ttu-id="02d35-198">hello FROM záradék azt jelzi, hello dokumentum gyűjtemény toobe többször is meg (**eszközök** vagy **devices.jobs**).</span><span class="sxs-lookup"><span data-stu-id="02d35-198">hello FROM clause indicates hello document collection toobe iterated on (**devices** or **devices.jobs**).</span></span> <span data-ttu-id="02d35-199">Ezt követően hello szűrő a hello amikor záradék van érvényben.</span><span class="sxs-lookup"><span data-stu-id="02d35-199">Then, hello filter in hello WHERE clause is applied.</span></span> <span data-ttu-id="02d35-200">Az összesítéseket, a megadott hello GROUP BY záradékban, a ebben a lépésben hello eredményeit vannak csoportosítva, és az egyes csoportok sor jön létre a SELECT záradékban hello megadottak szerint.</span><span class="sxs-lookup"><span data-stu-id="02d35-200">With aggregations, hello results of this step are grouped as specified in hello GROUP BY clause and, for each group, a row is generated as specified in hello SELECT clause.</span></span>

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a><span data-ttu-id="02d35-201">FROM záradékban</span><span class="sxs-lookup"><span data-stu-id="02d35-201">FROM clause</span></span>
<span data-ttu-id="02d35-202">Hello **a < from_specification >** záradék csak két értéket veheti fel: **ESZKÖZÖKRŐL**, tooquery eszköz twins, vagy **devices.jobs a**, tooquery feladat eszközönként részletes adatokat.</span><span class="sxs-lookup"><span data-stu-id="02d35-202">hello **FROM <from_specification>** clause can assume only two values: **FROM devices**, tooquery device twins, or **FROM devices.jobs**, tooquery job per-device details.</span></span>

## <a name="where-clause"></a><span data-ttu-id="02d35-203">A WHERE záradék</span><span class="sxs-lookup"><span data-stu-id="02d35-203">WHERE clause</span></span>
<span data-ttu-id="02d35-204">Hello **ahol < filter_condition >** záradék használata nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="02d35-204">hello **WHERE <filter_condition>** clause is optional.</span></span> <span data-ttu-id="02d35-205">Meghatározza, hogy hello JSON-dokumentumok hello FROM gyűjtemény hello eredmény részét képező toobe meg kell felelnie egy vagy több feltételt.</span><span class="sxs-lookup"><span data-stu-id="02d35-205">It specifies one or more conditions that hello JSON documents in hello FROM collection must satisfy toobe included as part of hello result.</span></span> <span data-ttu-id="02d35-206">Ki kell értékelnie minden bármely JSON-dokumentum hello megadott feltételek túl "true" hello eredményben toobe.</span><span class="sxs-lookup"><span data-stu-id="02d35-206">Any JSON document must evaluate hello specified conditions too"true" toobe included in hello result.</span></span>

<span data-ttu-id="02d35-207">hello engedélyezett feltételek részében leírt [kifejezések és a kikötések][lnk-query-expressions].</span><span class="sxs-lookup"><span data-stu-id="02d35-207">hello allowed conditions are described in section [Expressions and conditions][lnk-query-expressions].</span></span>

## <a name="select-clause"></a><span data-ttu-id="02d35-208">SELECT záradékban</span><span class="sxs-lookup"><span data-stu-id="02d35-208">SELECT clause</span></span>
<span data-ttu-id="02d35-209">hello SELECT záradékban (**VÁLASSZA < select_list >**) megadása kötelező, és határozza meg, milyen értékeket lekért hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="02d35-209">hello SELECT clause (**SELECT <select_list>**) is mandatory and specifies what values are retrieved from hello query.</span></span> <span data-ttu-id="02d35-210">Azt adja meg, hogy hello JSON értékek toobe használt toogenerate új JSON-objektumok.</span><span class="sxs-lookup"><span data-stu-id="02d35-210">It specifies hello JSON values toobe used toogenerate new JSON objects.</span></span>
<span data-ttu-id="02d35-211">Az egyes elemeinek hello hello FROM gyűjtemény szűrt (és nem kötelezően csoportosított) részhalmazát, hello leképezése fázis hoz létre egy új JSON-objektum, a hello SELECT záradékban megadott hello értékek kialakítani.</span><span class="sxs-lookup"><span data-stu-id="02d35-211">For each element of hello filtered (and optionally grouped) subset of hello FROM collection, hello projection phase generates a new JSON object, constructed with hello values specified in hello SELECT clause.</span></span>

<span data-ttu-id="02d35-212">Az alábbiakban látható a SELECT záradékban hello hello nyelvtan:</span><span class="sxs-lookup"><span data-stu-id="02d35-212">Following is hello grammar of hello SELECT clause:</span></span>

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

<span data-ttu-id="02d35-213">Ha **attribute_name** hello JSON-dokumentum hello FROM gyűjtemény tooany tulajdonsága hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="02d35-213">where **attribute_name** refers tooany property of hello JSON document in hello FROM collection.</span></span> <span data-ttu-id="02d35-214">Néhány példa a SELECT záradékban található hello [Ismerkedés az eszköz iker lekérdezések] [ lnk-query-getstarted] szakasz.</span><span class="sxs-lookup"><span data-stu-id="02d35-214">Some examples of SELECT clauses can be found in hello [Getting started with device twin queries][lnk-query-getstarted] section.</span></span>

<span data-ttu-id="02d35-215">Jelenleg kijelölt záradékot eltérő **válasszon \***  csak az eszköz twins összesített lekérdezései támogat.</span><span class="sxs-lookup"><span data-stu-id="02d35-215">Currently, selection clauses different than **SELECT \*** are only supported in aggregate queries on device twins.</span></span>

## <a name="group-by-clause"></a><span data-ttu-id="02d35-216">GROUP BY záradékban</span><span class="sxs-lookup"><span data-stu-id="02d35-216">GROUP BY clause</span></span>
<span data-ttu-id="02d35-217">Hello **GROUP BY < group_specification >** záradék egy opcionális lépés után hello szűrő hello WHERE záradékban megadott hajtható végre, és hello megadott hello leképezése előtt válassza ki.</span><span class="sxs-lookup"><span data-stu-id="02d35-217">hello **GROUP BY <group_specification>** clause is an optional step that can be executed after hello filter specified in hello WHERE clause, and before hello projection specified in hello SELECT.</span></span> <span data-ttu-id="02d35-218">Az attribútum értékének hello alapján dokumentumok felsorolását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="02d35-218">It groups documents based on hello value of an attribute.</span></span> <span data-ttu-id="02d35-219">Ezek a csoportok értékeket összesítve használt toogenerate hello SELECT záradékban megadott.</span><span class="sxs-lookup"><span data-stu-id="02d35-219">These groups are used toogenerate aggregated values as specified in hello SELECT clause.</span></span>

<span data-ttu-id="02d35-220">A GROUP BY lekérdezést példa, hogy:</span><span class="sxs-lookup"><span data-stu-id="02d35-220">An example of a query using GROUP BY is:</span></span>

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

<span data-ttu-id="02d35-221">hello formális GROUP BY szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="02d35-221">hello formal syntax for GROUP BY is:</span></span>

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

<span data-ttu-id="02d35-222">Ha **attribute_name** hello JSON-dokumentum hello FROM gyűjtemény tooany tulajdonsága hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="02d35-222">where **attribute_name** refers tooany property of hello JSON document in hello FROM collection.</span></span>

<span data-ttu-id="02d35-223">Jelenleg hello GROUP BY záradék használata csak támogatott eszköz twins lekérdezésekor.</span><span class="sxs-lookup"><span data-stu-id="02d35-223">Currently, hello GROUP BY clause is only supported when querying device twins.</span></span>

## <a name="expressions-and-conditions"></a><span data-ttu-id="02d35-224">Kifejezések és a feltételek</span><span class="sxs-lookup"><span data-stu-id="02d35-224">Expressions and conditions</span></span>
<span data-ttu-id="02d35-225">Magas szinten egy *kifejezés*:</span><span class="sxs-lookup"><span data-stu-id="02d35-225">At a high level, an *expression*:</span></span>

* <span data-ttu-id="02d35-226">Kiértékeli tooan típusú példány JSON (például a logikai érték, számot, karakterlánc, a tömb vagy objektum), és</span><span class="sxs-lookup"><span data-stu-id="02d35-226">Evaluates tooan instance of a JSON type (such as Boolean, number, string, array, or object), and</span></span>
* <span data-ttu-id="02d35-227">Határozza meg hello eszköz JSON-dokumentum és a beépített operátorok és függvények használata állandók származó adatok kezelésére.</span><span class="sxs-lookup"><span data-stu-id="02d35-227">Is defined by manipulating data coming from hello device JSON document and constants using built-in operators and functions.</span></span>

<span data-ttu-id="02d35-228">*Feltételek* kifejezések, amelyek kiértékelik tooa logikai érték.</span><span class="sxs-lookup"><span data-stu-id="02d35-228">*Conditions* are expressions that evaluate tooa Boolean.</span></span> <span data-ttu-id="02d35-229">Bármely állandó eltér a logikai **igaz** minősül **hamis** (beleértve a **null**, **nem definiált**, bármely objektum vagy tömb példány bármilyen karakterlánc, és egyértelműen hello logikai **hamis**).</span><span class="sxs-lookup"><span data-stu-id="02d35-229">Any constant different than Boolean **true** is considered as **false** (including **null**, **undefined**, any object or array instance, any string, and clearly hello Boolean **false**).</span></span>

<span data-ttu-id="02d35-230">hello kifejezések szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="02d35-230">hello syntax for expressions is:</span></span>

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

<span data-ttu-id="02d35-231">Ahol:</span><span class="sxs-lookup"><span data-stu-id="02d35-231">where:</span></span>

| <span data-ttu-id="02d35-232">Szimbólum</span><span class="sxs-lookup"><span data-stu-id="02d35-232">Symbol</span></span> | <span data-ttu-id="02d35-233">Meghatározás</span><span class="sxs-lookup"><span data-stu-id="02d35-233">Definition</span></span> |
| --- | --- |
| <span data-ttu-id="02d35-234">attribute_name</span><span class="sxs-lookup"><span data-stu-id="02d35-234">attribute_name</span></span> | <span data-ttu-id="02d35-235">Bármely tulajdonságát a hello hello JSON-dokumentum **FROM** gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="02d35-235">Any property of hello JSON document in hello **FROM** collection.</span></span> |
| <span data-ttu-id="02d35-236">binary_operator</span><span class="sxs-lookup"><span data-stu-id="02d35-236">binary_operator</span></span> | <span data-ttu-id="02d35-237">A bináris operátor szerepel a hello [operátorok](#operators) szakasz.</span><span class="sxs-lookup"><span data-stu-id="02d35-237">Any binary operator listed in hello [Operators](#operators) section.</span></span> |
| <span data-ttu-id="02d35-238">function_name</span><span class="sxs-lookup"><span data-stu-id="02d35-238">function_name</span></span>| <span data-ttu-id="02d35-239">Bármely függvény szerepel hello [funkciók](#functions) szakasz.</span><span class="sxs-lookup"><span data-stu-id="02d35-239">Any function listed in hello [Functions](#functions) section.</span></span> |
| <span data-ttu-id="02d35-240">decimal_literal</span><span class="sxs-lookup"><span data-stu-id="02d35-240">decimal_literal</span></span> |<span data-ttu-id="02d35-241">Egy lebegőpontos decimális jelöléssel kifejezve.</span><span class="sxs-lookup"><span data-stu-id="02d35-241">A float expressed in decimal notation.</span></span> |
| <span data-ttu-id="02d35-242">hexadecimal_literal</span><span class="sxs-lookup"><span data-stu-id="02d35-242">hexadecimal_literal</span></span> |<span data-ttu-id="02d35-243">Egy szám kifejezni hello karakterlánc "0 x" hexadecimális számjegyeket tartalmazó karakterlánc követ.</span><span class="sxs-lookup"><span data-stu-id="02d35-243">A number expressed by hello string ‘0x’ followed by a string of hexadecimal digits.</span></span> |
| <span data-ttu-id="02d35-244">string_literal</span><span class="sxs-lookup"><span data-stu-id="02d35-244">string_literal</span></span> |<span data-ttu-id="02d35-245">A szövegkonstansok olyan Unicode karakterláncok sorozatát nulla vagy több Unicode-karaktereket vagy escape-karaktersorozatokat.</span><span class="sxs-lookup"><span data-stu-id="02d35-245">String literals are Unicode strings represented by a sequence of zero or more Unicode characters or escape sequences.</span></span> <span data-ttu-id="02d35-246">A szövegkonstansok vannak szimpla zárójelek között (aposztróf: ") vagy dupla idézőjel (idézőjel:").</span><span class="sxs-lookup"><span data-stu-id="02d35-246">String literals are enclosed in single quotes (apostrophe: ' ) or double quotes (quotation mark: ").</span></span> <span data-ttu-id="02d35-247">Kilépés engedélyezett: `\'`, `\"`, `\\`, `\uXXXX` az Unicode karaktereket 4 hexadecimális számjegy határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="02d35-247">Allowed escapes: `\'`, `\"`, `\\`, `\uXXXX` for Unicode characters defined by 4 hexadecimal digits.</span></span> |

### <a name="operators"></a><span data-ttu-id="02d35-248">Operátorok</span><span class="sxs-lookup"><span data-stu-id="02d35-248">Operators</span></span>
<span data-ttu-id="02d35-249">a következő operátor hello támogatottak:</span><span class="sxs-lookup"><span data-stu-id="02d35-249">hello following operators are supported:</span></span>

| <span data-ttu-id="02d35-250">Termékcsalád</span><span class="sxs-lookup"><span data-stu-id="02d35-250">Family</span></span> | <span data-ttu-id="02d35-251">Operátorok</span><span class="sxs-lookup"><span data-stu-id="02d35-251">Operators</span></span> |
| --- | --- |
| <span data-ttu-id="02d35-252">Aritmetikai</span><span class="sxs-lookup"><span data-stu-id="02d35-252">Arithmetic</span></span> |<span data-ttu-id="02d35-253">+,-,*,/,%</span><span class="sxs-lookup"><span data-stu-id="02d35-253">+,-,*,/,%</span></span> |
| <span data-ttu-id="02d35-254">Logikai</span><span class="sxs-lookup"><span data-stu-id="02d35-254">Logical</span></span> |<span data-ttu-id="02d35-255">ÉS, VAGY SEM</span><span class="sxs-lookup"><span data-stu-id="02d35-255">AND, OR, NOT</span></span> |
| <span data-ttu-id="02d35-256">Összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="02d35-256">Comparison</span></span> |<span data-ttu-id="02d35-257">=, !=, <, >, <=, >=, <></span><span class="sxs-lookup"><span data-stu-id="02d35-257">=, !=, <, >, <=, >=, <></span></span> |

### <a name="functions"></a><span data-ttu-id="02d35-258">Functions</span><span class="sxs-lookup"><span data-stu-id="02d35-258">Functions</span></span>
<span data-ttu-id="02d35-259">Csak a támogatott twins és feladatok hello lekérdezésekor függvény van:</span><span class="sxs-lookup"><span data-stu-id="02d35-259">When querying twins and jobs hello only supported function is:</span></span>

| <span data-ttu-id="02d35-260">Függvény</span><span class="sxs-lookup"><span data-stu-id="02d35-260">Function</span></span> | <span data-ttu-id="02d35-261">Leírás</span><span class="sxs-lookup"><span data-stu-id="02d35-261">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="02d35-262">IS_DEFINED(property)</span><span class="sxs-lookup"><span data-stu-id="02d35-262">IS_DEFINED(property)</span></span> | <span data-ttu-id="02d35-263">Jelző, ha hello tulajdonság van rendelve egy érték logikai érték beolvasása (beleértve a `null`).</span><span class="sxs-lookup"><span data-stu-id="02d35-263">Returns a Boolean indicating if hello property has been assigned a value (including `null`).</span></span> |

<span data-ttu-id="02d35-264">Útvonalak állapotától függően a következő matematikai függvények hello támogat:</span><span class="sxs-lookup"><span data-stu-id="02d35-264">In routes conditions, hello following math functions are supported:</span></span>

| <span data-ttu-id="02d35-265">Függvény</span><span class="sxs-lookup"><span data-stu-id="02d35-265">Function</span></span> | <span data-ttu-id="02d35-266">Leírás</span><span class="sxs-lookup"><span data-stu-id="02d35-266">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="02d35-267">ABS(x)</span><span class="sxs-lookup"><span data-stu-id="02d35-267">ABS(x)</span></span> | <span data-ttu-id="02d35-268">Értéket ad vissza hello abszolút (pozitív) hello a megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="02d35-268">Returns hello absolute (positive) value of hello specified numeric expression.</span></span> |
| <span data-ttu-id="02d35-269">Exp(x)</span><span class="sxs-lookup"><span data-stu-id="02d35-269">EXP(x)</span></span> | <span data-ttu-id="02d35-270">Értéket ad vissza hello exponenciális hello a megadott numerikus kifejezés (e ^ x).</span><span class="sxs-lookup"><span data-stu-id="02d35-270">Returns hello exponential value of hello specified numeric expression (e^x).</span></span> |
| <span data-ttu-id="02d35-271">Power(x,y)</span><span class="sxs-lookup"><span data-stu-id="02d35-271">POWER(x,y)</span></span> | <span data-ttu-id="02d35-272">Értéket ad vissza a megadott hello értékének hello kifejezés toohello megadott power (x ^ y).</span><span class="sxs-lookup"><span data-stu-id="02d35-272">Returns hello value of hello specified expression toohello specified power (x^y).</span></span>|
| <span data-ttu-id="02d35-273">Square(x)</span><span class="sxs-lookup"><span data-stu-id="02d35-273">SQUARE(x)</span></span> | <span data-ttu-id="02d35-274">Beolvasása hello négyzetes hello a megadott numerikus érték.</span><span class="sxs-lookup"><span data-stu-id="02d35-274">Returns hello square of hello specified numeric value.</span></span> |
| <span data-ttu-id="02d35-275">CEILING(x)</span><span class="sxs-lookup"><span data-stu-id="02d35-275">CEILING(x)</span></span> | <span data-ttu-id="02d35-276">Beolvasása hello legkisebb egész szám nagyobb értékre, vagy egyenlő, hello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="02d35-276">Returns hello smallest integer value greater than, or equal to, hello specified numeric expression.</span></span> |
| <span data-ttu-id="02d35-277">FLOOR(x)</span><span class="sxs-lookup"><span data-stu-id="02d35-277">FLOOR(x)</span></span> | <span data-ttu-id="02d35-278">Visszaadja hello legnagyobb egész szám kisebb vagy egyenlő, mint a toohello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="02d35-278">Returns hello largest integer less than or equal toohello specified numeric expression.</span></span> |
| <span data-ttu-id="02d35-279">SIGN(x)</span><span class="sxs-lookup"><span data-stu-id="02d35-279">SIGN(x)</span></span> | <span data-ttu-id="02d35-280">Beolvasása hello pozitív (+ 1), nulla (0), vagy a hello mínuszjel (-1) megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="02d35-280">Returns hello positive (+1), zero (0), or negative (-1) sign of hello specified numeric expression.</span></span>|
| <span data-ttu-id="02d35-281">Sqrt(x)</span><span class="sxs-lookup"><span data-stu-id="02d35-281">SQRT(x)</span></span> | <span data-ttu-id="02d35-282">Beolvasása hello négyzetes hello a megadott numerikus érték.</span><span class="sxs-lookup"><span data-stu-id="02d35-282">Returns hello square of hello specified numeric value.</span></span> |

<span data-ttu-id="02d35-283">Az útvonalak feltételek a következő típus ellenőrzése és a funkciók leadó hello támogatottak:</span><span class="sxs-lookup"><span data-stu-id="02d35-283">In routes conditions, hello following type checking and casting functions are supported:</span></span>

| <span data-ttu-id="02d35-284">Függvény</span><span class="sxs-lookup"><span data-stu-id="02d35-284">Function</span></span> | <span data-ttu-id="02d35-285">Leírás</span><span class="sxs-lookup"><span data-stu-id="02d35-285">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="02d35-286">AS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="02d35-286">AS_NUMBER</span></span> | <span data-ttu-id="02d35-287">Hello bemeneti karakterlánc tooa számot konvertálja.</span><span class="sxs-lookup"><span data-stu-id="02d35-287">Converts hello input string tooa number.</span></span> <span data-ttu-id="02d35-288">`noop`Ha a bemeneti érték egy szám; `Undefined` Ha karakterlánc nem felel meg egy számot.</span><span class="sxs-lookup"><span data-stu-id="02d35-288">`noop` if input is a number; `Undefined` if string does not represent a number.</span></span>|
| <span data-ttu-id="02d35-289">IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="02d35-289">IS_ARRAY</span></span> | <span data-ttu-id="02d35-290">Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték egy tömb.</span><span class="sxs-lookup"><span data-stu-id="02d35-290">Returns a Boolean value indicating if hello type of hello specified expression is an array.</span></span> |
| <span data-ttu-id="02d35-291">IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="02d35-291">IS_BOOL</span></span> | <span data-ttu-id="02d35-292">Adja vissza egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés olyan logikai érték.</span><span class="sxs-lookup"><span data-stu-id="02d35-292">Returns a Boolean value indicating if hello type of hello specified expression is a Boolean.</span></span> |
| <span data-ttu-id="02d35-293">IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="02d35-293">IS_DEFINED</span></span> | <span data-ttu-id="02d35-294">Jelzi, ha hello tulajdonság van rendelve egy érték logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="02d35-294">Returns a Boolean indicating if hello property has been assigned a value.</span></span> |
| <span data-ttu-id="02d35-295">IS_NULL</span><span class="sxs-lookup"><span data-stu-id="02d35-295">IS_NULL</span></span> | <span data-ttu-id="02d35-296">Visszaadja egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés értéke null.</span><span class="sxs-lookup"><span data-stu-id="02d35-296">Returns a Boolean value indicating if hello type of hello specified expression is null.</span></span> |
| <span data-ttu-id="02d35-297">IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="02d35-297">IS_NUMBER</span></span> | <span data-ttu-id="02d35-298">Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték egy szám.</span><span class="sxs-lookup"><span data-stu-id="02d35-298">Returns a Boolean value indicating if hello type of hello specified expression is a number.</span></span> |
| <span data-ttu-id="02d35-299">IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="02d35-299">IS_OBJECT</span></span> | <span data-ttu-id="02d35-300">Adja vissza egy logikai érték, amely jelzi, ha hello hello típusú megadott kifejezés egy JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="02d35-300">Returns a Boolean value indicating if hello type of hello specified expression is a JSON object.</span></span> |
| <span data-ttu-id="02d35-301">IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="02d35-301">IS_PRIMITIVE</span></span> | <span data-ttu-id="02d35-302">Ha hello hello megadva kifejezés egy egyszerű jelző logikai érték beolvasása (string, Boolean, numerikus vagy `null`).</span><span class="sxs-lookup"><span data-stu-id="02d35-302">Returns a Boolean value indicating if hello type of hello specified expression is a primitive (string, Boolean, numeric or `null`).</span></span> |
| <span data-ttu-id="02d35-303">IS_STRING</span><span class="sxs-lookup"><span data-stu-id="02d35-303">IS_STRING</span></span> | <span data-ttu-id="02d35-304">Adja vissza, ha hello hello típusú megadott kifejezés jelző logikai érték: karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="02d35-304">Returns a Boolean value indicating if hello type of hello specified expression is a string.</span></span> |

<span data-ttu-id="02d35-305">Útvonalak feltételek, a karakterlánc a következő hello támogatottak:</span><span class="sxs-lookup"><span data-stu-id="02d35-305">In routes conditions, hello following string functions are supported:</span></span>

| <span data-ttu-id="02d35-306">Függvény</span><span class="sxs-lookup"><span data-stu-id="02d35-306">Function</span></span> | <span data-ttu-id="02d35-307">Leírás</span><span class="sxs-lookup"><span data-stu-id="02d35-307">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="02d35-308">CONCAT(x,...)</span><span class="sxs-lookup"><span data-stu-id="02d35-308">CONCAT(x, …)</span></span> | <span data-ttu-id="02d35-309">Egy karakterlánc, amely legalább két karakterlánc-értékek hozzáfűzésével hello eredményét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="02d35-309">Returns a string that is hello result of concatenating two or more string values.</span></span> |
| <span data-ttu-id="02d35-310">LENGTH(x)</span><span class="sxs-lookup"><span data-stu-id="02d35-310">LENGTH(x)</span></span> | <span data-ttu-id="02d35-311">A megadott karakterlánc-kifejezés hello karakterét hello számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="02d35-311">Returns hello number of characters of hello specified string expression.</span></span>|
| <span data-ttu-id="02d35-312">Lower(x)</span><span class="sxs-lookup"><span data-stu-id="02d35-312">LOWER(x)</span></span> | <span data-ttu-id="02d35-313">Egy karakterlánc-kifejezés nagybetűt adatok toolowercase átalakítása után adja vissza.</span><span class="sxs-lookup"><span data-stu-id="02d35-313">Returns a string expression after converting uppercase character data toolowercase.</span></span> |
| <span data-ttu-id="02d35-314">Upper(x)</span><span class="sxs-lookup"><span data-stu-id="02d35-314">UPPER(x)</span></span> | <span data-ttu-id="02d35-315">Egy karakterlánc-kifejezés kisbetűt adatok toouppercase átalakítása után adja vissza.</span><span class="sxs-lookup"><span data-stu-id="02d35-315">Returns a string expression after converting lowercase character data toouppercase.</span></span> |
| <span data-ttu-id="02d35-316">SUBSTRING (karakterlánc, start [, hossz])</span><span class="sxs-lookup"><span data-stu-id="02d35-316">SUBSTRING(string, start [, length])</span></span> | <span data-ttu-id="02d35-317">Hello kezdődő karakterlánc-kifejezés részét adja vissza a megadott karakter nulláról indulva számolt helyzetét, és folytatja a toohello megadott hosszúság vagy hello karakterlánc toohello végét.</span><span class="sxs-lookup"><span data-stu-id="02d35-317">Returns part of a string expression starting at hello specified character zero-based position and continues toohello specified length, or toohello end of hello string.</span></span> |
| <span data-ttu-id="02d35-318">(Karakterlánc, töredék) INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="02d35-318">INDEX_OF(string, fragment)</span></span> | <span data-ttu-id="02d35-319">Hello hello második karakterlánc-kifejezés hello első megadott karakterlánc-kifejezés vagy-1 érték első előfordulásának pozícióját a indítása, ha hello karakterlánc nem található hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="02d35-319">Returns hello starting position of hello first occurrence of hello second string expression within hello first specified string expression, or -1 if hello string is not found.</span></span>|
| <span data-ttu-id="02d35-320">STARTS_WITH (x, y)</span><span class="sxs-lookup"><span data-stu-id="02d35-320">STARTS_WITH(x, y)</span></span> | <span data-ttu-id="02d35-321">Hogy hello első karakterlánc-kifejezés indításakor hello második jelző logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="02d35-321">Returns a Boolean indicating whether hello first string expression starts with hello second.</span></span> |
| <span data-ttu-id="02d35-322">ENDS_WITH (x, y)</span><span class="sxs-lookup"><span data-stu-id="02d35-322">ENDS_WITH(x, y)</span></span> | <span data-ttu-id="02d35-323">E hello első karaktersorozat végződik hello második jelző logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="02d35-323">Returns a Boolean indicating whether hello first string expression ends with hello second.</span></span> |
| <span data-ttu-id="02d35-324">CONTAINS(x,y)</span><span class="sxs-lookup"><span data-stu-id="02d35-324">CONTAINS(x,y)</span></span> | <span data-ttu-id="02d35-325">Hogy hello első karakterlánc-kifejezés második tartalmaz-e hello jelző logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="02d35-325">Returns a Boolean indicating whether hello first string expression contains hello second.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="02d35-326">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="02d35-326">Next steps</span></span>
<span data-ttu-id="02d35-327">Ismerje meg, hogyan tooexecute lekérdezi az alkalmazások a [Azure IoT SDK-k][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="02d35-327">Learn how tooexecute queries in your apps using [Azure IoT SDKs][lnk-hub-sdks].</span></span>

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

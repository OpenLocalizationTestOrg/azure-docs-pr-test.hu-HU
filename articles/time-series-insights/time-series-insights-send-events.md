---
title: "aaaSend események tooAzure idő adatsorozat Insights környezetben |} Microsoft Docs"
description: "Ez az oktatóanyag ismerteti hello lépéseket toopush események tooyour idő adatsorozat Insights környezet"
keywords: 
services: tsi
documentationcenter: 
author: venkatgct
manager: jhubbard
editor: 
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: venkatja
ms.openlocfilehash: dbccc23f61351a0033cd48c1a02fb3841b45d560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooa-time-series-insights-environment-using-event-hub"></a><span data-ttu-id="e8e5c-103">Küldés események tooa idő adatsorozat Insights környezet az event hubs használatával</span><span class="sxs-lookup"><span data-stu-id="e8e5c-103">Send events tooa Time Series Insights environment using event hub</span></span>

<span data-ttu-id="e8e5c-104">Ez az oktatóanyag azt ismerteti, hogyan toocreate és az event hubs konfigurálása, és futtasson egy minta alkalmazás toopush események.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-104">This tutorial explains how toocreate and configure event hub and run a sample application toopush events.</span></span> <span data-ttu-id="e8e5c-105">Ha már van JSON formátumú eseményeket tartalmazó eseményközpontja, ugorja át ezt az oktatóanyagot, és tekintse meg a környezetet a [Time Series Insightsban](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e8e5c-105">If you have an existing event hub with events in JSON format, skip this tutorial and view your environment in [time series insights](https://insights.timeseries.azure.com).</span></span>

## <a name="configure-an-event-hub"></a><span data-ttu-id="e8e5c-106">Eseményközpont konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e8e5c-106">Configure an event hub</span></span>
1. <span data-ttu-id="e8e5c-107">toocreate eseményközpontban, kövesse az utasításokat az Eseményközpont hello [dokumentáció](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).</span><span class="sxs-lookup"><span data-stu-id="e8e5c-107">toocreate an event hub, follow instructions from hello Event Hub [documentation](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).</span></span>

2. <span data-ttu-id="e8e5c-108">Olyan fogyasztói csoportot hozzon létre, amelyet csak a Time Series Insights-eseményforrás használ.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-108">Make sure you create a consumer group that is used exclusively by your Time Series Insights event source.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="e8e5c-109">Ügyeljen arra, hogy ezt a fogyasztói csoportot ne használja másik szolgáltatás (például Stream Analytics-feladat vagy másik Time Series Insights-környezet).</span><span class="sxs-lookup"><span data-stu-id="e8e5c-109">Make sure this consumer group is not used by any other service (such as Stream Analytics job or another Time Series Insights environment).</span></span> <span data-ttu-id="e8e5c-110">Fogyasztói csoportot más szolgáltatások használata esetén olvassa el a művelet negatívan befolyásolja az ebben a környezetben, és más szolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-110">If consumer group is used by other services, read operation is negatively affected for this environment and hello other services.</span></span> <span data-ttu-id="e8e5c-111">Ha a "$Default" fogyasztói csoportot hello használ, előfordulhat toopotential újbóli más olvasók által.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-111">If you are using “$Default” as hello consumer group, it could lead toopotential reuse by other readers.</span></span>

  ![Az eseményközpont fogyasztói csoportjának kiválasztása](media/send-events/consumer-group.png)

3. <span data-ttu-id="e8e5c-113">Hello eseményközpontok felé, hozzon létre "MySendPolicy" hello csharp mintát, amely használt toosend események.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-113">On hello event hub, create “MySendPolicy” that is used toosend events in hello csharp sample.</span></span>

  ![A Megosztott elérési házirendek kiválasztása, majd kattintás a Hozzáadás gombra](media/send-events/shared-access-policy.png)  

  ![Új megosztott elérési házirend hozzáadása](media/send-events/shared-access-policy-2.png)  

## <a name="create-time-series-insights-event-source"></a><span data-ttu-id="e8e5c-116">Time Series Insights-eseményforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8e5c-116">Create Time Series Insights event source</span></span>
1. <span data-ttu-id="e8e5c-117">Ha még nem hozott létre az eseményforrás, hajtsa végre az [ezeket az utasításokat](time-series-insights-add-event-source.md) toocreate egy esemény forrását.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-117">If you haven't created an event source, follow [these instructions](time-series-insights-add-event-source.md) toocreate an event source.</span></span>

2. <span data-ttu-id="e8e5c-118">Adja meg a "deviceTimestamp" hello időbélyeg-tulajdonság neve – ezzel a tulajdonsággal, a tényleges időbélyeg hello csharp mintában hello.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-118">Specify “deviceTimestamp” as hello timestamp property name – this property is used as hello actual timestamp in hello csharp sample.</span></span> <span data-ttu-id="e8e5c-119">hello időbélyeg-tulajdonság neve a kis-és nagybetűket, és értékek hello formátumot kell követnie __éééé-hh-nnTóó: pp:. FFFFFFFK__ , JSON tooevent hub elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-119">hello timestamp property name is case-sensitive and values must follow hello format __yyyy-MM-ddTHH:mm:ss.FFFFFFFK__ when sent as JSON tooevent hub.</span></span> <span data-ttu-id="e8e5c-120">Ha hello tulajdonság nem létezik a hello esemény, majd hello event hub várólistán lévő idő szolgál.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-120">If hello property does not exist in hello event, then hello event hub enqueued time is used.</span></span>

  ![Eseményforrás létrehozása](media/send-events/event-source-1.png)

## <a name="sample-code-toopush-events"></a><span data-ttu-id="e8e5c-122">A minta kód toopush események</span><span class="sxs-lookup"><span data-stu-id="e8e5c-122">Sample code toopush events</span></span>
1. <span data-ttu-id="e8e5c-123">Nyissa meg toohello event hub házirend "MySendPolicy", és másolja a hello kapcsolati karakterláncot hello házirend kulccsal.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-123">Go toohello event hub policy “MySendPolicy” and copy hello connection string with hello policy key.</span></span>

  ![A MySendPolicy kapcsolati karakterlánc másolása](media/send-events/sample-code-connection-string.png)

2. <span data-ttu-id="e8e5c-125">Futtassa a következő kód hello adott toosend 600 események (Event) hello három eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-125">Run hello following code that toosend 600 events per each of hello three devices.</span></span> <span data-ttu-id="e8e5c-126">Frissítse az `eventHubConnectionString` elemet a kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-126">Update `eventHubConnectionString` with your connection string.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON tooevent hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## <a name="supported-json-shapes"></a><span data-ttu-id="e8e5c-127">Támogatott JSON-alakzatok</span><span class="sxs-lookup"><span data-stu-id="e8e5c-127">Supported JSON shapes</span></span>
### <a name="sample-1"></a><span data-ttu-id="e8e5c-128">1. példa</span><span class="sxs-lookup"><span data-stu-id="e8e5c-128">Sample 1</span></span>

#### <a name="input"></a><span data-ttu-id="e8e5c-129">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="e8e5c-129">Input</span></span>

<span data-ttu-id="e8e5c-130">Egyszerű JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-130">A simple JSON object.</span></span>

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---1-event"></a><span data-ttu-id="e8e5c-131">Kimenet – 1 esemény</span><span class="sxs-lookup"><span data-stu-id="e8e5c-131">Output - 1 event</span></span>

|<span data-ttu-id="e8e5c-132">id</span><span class="sxs-lookup"><span data-stu-id="e8e5c-132">id</span></span>|<span data-ttu-id="e8e5c-133">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="e8e5c-133">timestamp</span></span>|
|--------|---------------|
|<span data-ttu-id="e8e5c-134">device1</span><span class="sxs-lookup"><span data-stu-id="e8e5c-134">device1</span></span>|<span data-ttu-id="e8e5c-135">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="e8e5c-135">2016-01-08T01:08:00Z</span></span>|

### <a name="sample-2"></a><span data-ttu-id="e8e5c-136">2. példa</span><span class="sxs-lookup"><span data-stu-id="e8e5c-136">Sample 2</span></span>

#### <a name="input"></a><span data-ttu-id="e8e5c-137">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="e8e5c-137">Input</span></span>
<span data-ttu-id="e8e5c-138">JSON-tömb két JSON-objektummal.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-138">A JSON array with two JSON objects.</span></span> <span data-ttu-id="e8e5c-139">Minden JSON-objektum lesz konvertált tooan esemény.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-139">Each JSON object will be converted tooan event.</span></span>
```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### <a name="output---2-events"></a><span data-ttu-id="e8e5c-140">Kimenet – 2 esemény</span><span class="sxs-lookup"><span data-stu-id="e8e5c-140">Output - 2 Events</span></span>

|<span data-ttu-id="e8e5c-141">id</span><span class="sxs-lookup"><span data-stu-id="e8e5c-141">id</span></span>|<span data-ttu-id="e8e5c-142">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="e8e5c-142">timestamp</span></span>|
|--------|---------------|
|<span data-ttu-id="e8e5c-143">device1</span><span class="sxs-lookup"><span data-stu-id="e8e5c-143">device1</span></span>|<span data-ttu-id="e8e5c-144">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="e8e5c-144">2016-01-08T01:08:00Z</span></span>|
|<span data-ttu-id="e8e5c-145">device2</span><span class="sxs-lookup"><span data-stu-id="e8e5c-145">device2</span></span>|<span data-ttu-id="e8e5c-146">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="e8e5c-146">2016-01-08T01:17:00Z</span></span>|
### <a name="sample-3"></a><span data-ttu-id="e8e5c-147">3. példa</span><span class="sxs-lookup"><span data-stu-id="e8e5c-147">Sample 3</span></span>

#### <a name="input"></a><span data-ttu-id="e8e5c-148">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="e8e5c-148">Input</span></span>

<span data-ttu-id="e8e5c-149">Két JSON-objektumot tartalmazó beágyazott JSON-tömbbel rendelkező JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-149">A JSON object with a nested JSON array containing two JSON objects.</span></span>
```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### <a name="output---2-events"></a><span data-ttu-id="e8e5c-150">Kimenet – 2 esemény</span><span class="sxs-lookup"><span data-stu-id="e8e5c-150">Output - 2 Events</span></span>
<span data-ttu-id="e8e5c-151">Vegye figyelembe, hogy hello tulajdonság "hely" hello esemény tooeach keresztül másolja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-151">Note that hello property "location" is copied over tooeach of hello event.</span></span>

|<span data-ttu-id="e8e5c-152">location</span><span class="sxs-lookup"><span data-stu-id="e8e5c-152">location</span></span>|<span data-ttu-id="e8e5c-153">events.id</span><span class="sxs-lookup"><span data-stu-id="e8e5c-153">events.id</span></span>|<span data-ttu-id="e8e5c-154">events.timestamp</span><span class="sxs-lookup"><span data-stu-id="e8e5c-154">events.timestamp</span></span>|
|--------|---------------|----------------------|
|<span data-ttu-id="e8e5c-155">WestUs</span><span class="sxs-lookup"><span data-stu-id="e8e5c-155">WestUs</span></span>|<span data-ttu-id="e8e5c-156">device1</span><span class="sxs-lookup"><span data-stu-id="e8e5c-156">device1</span></span>|<span data-ttu-id="e8e5c-157">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="e8e5c-157">2016-01-08T01:08:00Z</span></span>|
|<span data-ttu-id="e8e5c-158">WestUs</span><span class="sxs-lookup"><span data-stu-id="e8e5c-158">WestUs</span></span>|<span data-ttu-id="e8e5c-159">device2</span><span class="sxs-lookup"><span data-stu-id="e8e5c-159">device2</span></span>|<span data-ttu-id="e8e5c-160">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="e8e5c-160">2016-01-08T01:17:00Z</span></span>|

### <a name="sample-4"></a><span data-ttu-id="e8e5c-161">4. példa</span><span class="sxs-lookup"><span data-stu-id="e8e5c-161">Sample 4</span></span>

#### <a name="input"></a><span data-ttu-id="e8e5c-162">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="e8e5c-162">Input</span></span>

<span data-ttu-id="e8e5c-163">Két JSON-objektumot tartalmazó beágyazott JSON-tömbbel rendelkező JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-163">A JSON object with a nested JSON array containing two JSON objects.</span></span> <span data-ttu-id="e8e5c-164">A bemeneti mutatja be, hogy hello globális tulajdonságok képviselheti hello összetett JSON-objektumból.</span><span class="sxs-lookup"><span data-stu-id="e8e5c-164">This input demonstrates that hello global properties may be represented by hello complex JSON object.</span></span>

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### <a name="output---2-events"></a><span data-ttu-id="e8e5c-165">Kimenet – 2 esemény</span><span class="sxs-lookup"><span data-stu-id="e8e5c-165">Output - 2 Events</span></span>

|<span data-ttu-id="e8e5c-166">location</span><span class="sxs-lookup"><span data-stu-id="e8e5c-166">location</span></span>|<span data-ttu-id="e8e5c-167">manufacturer.name</span><span class="sxs-lookup"><span data-stu-id="e8e5c-167">manufacturer.name</span></span>|<span data-ttu-id="e8e5c-168">manufacturer.location</span><span class="sxs-lookup"><span data-stu-id="e8e5c-168">manufacturer.location</span></span>|<span data-ttu-id="e8e5c-169">events.id</span><span class="sxs-lookup"><span data-stu-id="e8e5c-169">events.id</span></span>|<span data-ttu-id="e8e5c-170">events.timestamp</span><span class="sxs-lookup"><span data-stu-id="e8e5c-170">events.timestamp</span></span>|<span data-ttu-id="e8e5c-171">events.data.type</span><span class="sxs-lookup"><span data-stu-id="e8e5c-171">events.data.type</span></span>|<span data-ttu-id="e8e5c-172">events.data.units</span><span class="sxs-lookup"><span data-stu-id="e8e5c-172">events.data.units</span></span>|<span data-ttu-id="e8e5c-173">events.data.value</span><span class="sxs-lookup"><span data-stu-id="e8e5c-173">events.data.value</span></span>|
|---|---|---|---|---|---|---|---|
|<span data-ttu-id="e8e5c-174">WestUs</span><span class="sxs-lookup"><span data-stu-id="e8e5c-174">WestUs</span></span>|<span data-ttu-id="e8e5c-175">manufacturer1</span><span class="sxs-lookup"><span data-stu-id="e8e5c-175">manufacturer1</span></span>|<span data-ttu-id="e8e5c-176">EastUs</span><span class="sxs-lookup"><span data-stu-id="e8e5c-176">EastUs</span></span>|<span data-ttu-id="e8e5c-177">device1</span><span class="sxs-lookup"><span data-stu-id="e8e5c-177">device1</span></span>|<span data-ttu-id="e8e5c-178">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="e8e5c-178">2016-01-08T01:08:00Z</span></span>|<span data-ttu-id="e8e5c-179">pressure</span><span class="sxs-lookup"><span data-stu-id="e8e5c-179">pressure</span></span>|<span data-ttu-id="e8e5c-180">psi</span><span class="sxs-lookup"><span data-stu-id="e8e5c-180">psi</span></span>|<span data-ttu-id="e8e5c-181">108.09</span><span class="sxs-lookup"><span data-stu-id="e8e5c-181">108.09</span></span>|
|<span data-ttu-id="e8e5c-182">WestUs</span><span class="sxs-lookup"><span data-stu-id="e8e5c-182">WestUs</span></span>|<span data-ttu-id="e8e5c-183">manufacturer1</span><span class="sxs-lookup"><span data-stu-id="e8e5c-183">manufacturer1</span></span>|<span data-ttu-id="e8e5c-184">EastUs</span><span class="sxs-lookup"><span data-stu-id="e8e5c-184">EastUs</span></span>|<span data-ttu-id="e8e5c-185">device2</span><span class="sxs-lookup"><span data-stu-id="e8e5c-185">device2</span></span>|<span data-ttu-id="e8e5c-186">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="e8e5c-186">2016-01-08T01:17:00Z</span></span>|<span data-ttu-id="e8e5c-187">vibration</span><span class="sxs-lookup"><span data-stu-id="e8e5c-187">vibration</span></span>|<span data-ttu-id="e8e5c-188">abs G</span><span class="sxs-lookup"><span data-stu-id="e8e5c-188">abs G</span></span>|<span data-ttu-id="e8e5c-189">217.09</span><span class="sxs-lookup"><span data-stu-id="e8e5c-189">217.09</span></span>|

## <a name="next-steps"></a><span data-ttu-id="e8e5c-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8e5c-190">Next steps</span></span>

* <span data-ttu-id="e8e5c-191">A környezet megtekintése a [Time Series Insights portálon](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="e8e5c-191">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>

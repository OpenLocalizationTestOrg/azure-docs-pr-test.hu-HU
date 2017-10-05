---
title: "Események küldése Azure Time Series Insights-környezetbe | Microsoft Docs"
description: "Ez az oktatóanyag bemutatja az események a Time Series Insights-környezetbe való küldéséhez szükséges lépéseket"
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
ms.openlocfilehash: b4ef96a045393f28b3cd750068fe82a5a8411afa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a><span data-ttu-id="65a22-103">Események küldése Time Series Insights-környezetbe eseményközponton keresztül</span><span class="sxs-lookup"><span data-stu-id="65a22-103">Send events to a Time Series Insights environment using event hub</span></span>

<span data-ttu-id="65a22-104">Az oktatóanyag elmagyarázza, hogyan hozhat létre és konfigurálhat egy eseményközpontot, és hogyan futtathat egy mintaalkalmazást események leküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="65a22-104">This tutorial explains how to create and configure event hub and run a sample application to push events.</span></span> <span data-ttu-id="65a22-105">Ha már van JSON formátumú eseményeket tartalmazó eseményközpontja, ugorja át ezt az oktatóanyagot, és tekintse meg a környezetet a [Time Series Insightsban](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="65a22-105">If you have an existing event hub with events in JSON format, skip this tutorial and view your environment in [time series insights](https://insights.timeseries.azure.com).</span></span>

## <a name="configure-an-event-hub"></a><span data-ttu-id="65a22-106">Eseményközpont konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65a22-106">Configure an event hub</span></span>
1. <span data-ttu-id="65a22-107">Eseményközpont létrehozásához kövesse az Event Hubs [dokumentációjában](https://docs.microsoft.com/azure/event-hubs/event-hubs-create) foglalt utasításokat.</span><span class="sxs-lookup"><span data-stu-id="65a22-107">To create an event hub, follow instructions from the Event Hub [documentation](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).</span></span>

2. <span data-ttu-id="65a22-108">Olyan fogyasztói csoportot hozzon létre, amelyet csak a Time Series Insights-eseményforrás használ.</span><span class="sxs-lookup"><span data-stu-id="65a22-108">Make sure you create a consumer group that is used exclusively by your Time Series Insights event source.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="65a22-109">Ügyeljen arra, hogy ezt a fogyasztói csoportot ne használja másik szolgáltatás (például Stream Analytics-feladat vagy másik Time Series Insights-környezet).</span><span class="sxs-lookup"><span data-stu-id="65a22-109">Make sure this consumer group is not used by any other service (such as Stream Analytics job or another Time Series Insights environment).</span></span> <span data-ttu-id="65a22-110">Ha a fogyasztói csoportot más szolgáltatások is használják, az zavarhatja az olvasási műveleteket ebben a környezetben és a többi szolgáltatásban is.</span><span class="sxs-lookup"><span data-stu-id="65a22-110">If consumer group is used by other services, read operation is negatively affected for this environment and the other services.</span></span> <span data-ttu-id="65a22-111">Ha a „$Default” elemet használja a fogyasztói csoportként, előfordulhat, hogy más olvasók újra fel fogják használni a csoportot.</span><span class="sxs-lookup"><span data-stu-id="65a22-111">If you are using “$Default” as the consumer group, it could lead to potential reuse by other readers.</span></span>

  ![Az eseményközpont fogyasztói csoportjának kiválasztása](media/send-events/consumer-group.png)

3. <span data-ttu-id="65a22-113">Az eseményközpontban hozza létre a „MySendPolicy” elnevezésű szabályzatot, amelyet az alábbi C#-példában az események küldésére használunk majd.</span><span class="sxs-lookup"><span data-stu-id="65a22-113">On the event hub, create “MySendPolicy” that is used to send events in the csharp sample.</span></span>

  ![A Megosztott elérési házirendek kiválasztása, majd kattintás a Hozzáadás gombra](media/send-events/shared-access-policy.png)  

  ![Új megosztott elérési házirend hozzáadása](media/send-events/shared-access-policy-2.png)  

## <a name="create-time-series-insights-event-source"></a><span data-ttu-id="65a22-116">Time Series Insights-eseményforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="65a22-116">Create Time Series Insights event source</span></span>
1. <span data-ttu-id="65a22-117">Ha még nem hozott létre eseményforrást, tegye ezt meg [ezeket az utasításokat](time-series-insights-add-event-source.md) követve.</span><span class="sxs-lookup"><span data-stu-id="65a22-117">If you haven't created an event source, follow [these instructions](time-series-insights-add-event-source.md) to create an event source.</span></span>

2. <span data-ttu-id="65a22-118">Adja meg a „deviceTimestamp” értéket az időbélyegző-tulajdonság neveként – ezt a tulajdonságot használja a rendszer a tényleges időbélyegzőként a C#-példában.</span><span class="sxs-lookup"><span data-stu-id="65a22-118">Specify “deviceTimestamp” as the timestamp property name – this property is used as the actual timestamp in the csharp sample.</span></span> <span data-ttu-id="65a22-119">Az időbélyegző-tulajdonság neve megkülönbözteti a kis- és nagybetűket, és az értékeknek __éééé-HH-nnTÓÓ:pp:mm.FFFFFFFK__ formátumban kell lenniük, ha JSON formátumban lesznek elküldve az eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="65a22-119">The timestamp property name is case-sensitive and values must follow the format __yyyy-MM-ddTHH:mm:ss.FFFFFFFK__ when sent as JSON to event hub.</span></span> <span data-ttu-id="65a22-120">Ha a tulajdonság nem létezik az eseményben, akkor a rendszer azt az időpontot használja, amikor az eseményt sorba helyezték az eseményközpontban.</span><span class="sxs-lookup"><span data-stu-id="65a22-120">If the property does not exist in the event, then the event hub enqueued time is used.</span></span>

  ![Eseményforrás létrehozása](media/send-events/event-source-1.png)

## <a name="sample-code-to-push-events"></a><span data-ttu-id="65a22-122">Mintakód események leküldéséhez</span><span class="sxs-lookup"><span data-stu-id="65a22-122">Sample code to push events</span></span>
1. <span data-ttu-id="65a22-123">Lépjen a „MySendPolicy” eseményközpont-házirendhez, és másolja a házirendkulccsal rendelkező kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="65a22-123">Go to the event hub policy “MySendPolicy” and copy the connection string with the policy key.</span></span>

  ![A MySendPolicy kapcsolati karakterlánc másolása](media/send-events/sample-code-connection-string.png)

2. <span data-ttu-id="65a22-125">A következő kód futtatásával 600 eseményt küld mindhárom eszközre.</span><span class="sxs-lookup"><span data-stu-id="65a22-125">Run the following code that to send 600 events per each of the three devices.</span></span> <span data-ttu-id="65a22-126">Frissítse az `eventHubConnectionString` elemet a kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="65a22-126">Update `eventHubConnectionString` with your connection string.</span></span>

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

                // Send JSON to event hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## <a name="supported-json-shapes"></a><span data-ttu-id="65a22-127">Támogatott JSON-alakzatok</span><span class="sxs-lookup"><span data-stu-id="65a22-127">Supported JSON shapes</span></span>
### <a name="sample-1"></a><span data-ttu-id="65a22-128">1. példa</span><span class="sxs-lookup"><span data-stu-id="65a22-128">Sample 1</span></span>

#### <a name="input"></a><span data-ttu-id="65a22-129">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="65a22-129">Input</span></span>

<span data-ttu-id="65a22-130">Egyszerű JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="65a22-130">A simple JSON object.</span></span>

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---1-event"></a><span data-ttu-id="65a22-131">Kimenet – 1 esemény</span><span class="sxs-lookup"><span data-stu-id="65a22-131">Output - 1 event</span></span>

|<span data-ttu-id="65a22-132">id</span><span class="sxs-lookup"><span data-stu-id="65a22-132">id</span></span>|<span data-ttu-id="65a22-133">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="65a22-133">timestamp</span></span>|
|--------|---------------|
|<span data-ttu-id="65a22-134">device1</span><span class="sxs-lookup"><span data-stu-id="65a22-134">device1</span></span>|<span data-ttu-id="65a22-135">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="65a22-135">2016-01-08T01:08:00Z</span></span>|

### <a name="sample-2"></a><span data-ttu-id="65a22-136">2. példa</span><span class="sxs-lookup"><span data-stu-id="65a22-136">Sample 2</span></span>

#### <a name="input"></a><span data-ttu-id="65a22-137">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="65a22-137">Input</span></span>
<span data-ttu-id="65a22-138">JSON-tömb két JSON-objektummal.</span><span class="sxs-lookup"><span data-stu-id="65a22-138">A JSON array with two JSON objects.</span></span> <span data-ttu-id="65a22-139">Minden JSON-objektum eseménnyé lesz átalakítva.</span><span class="sxs-lookup"><span data-stu-id="65a22-139">Each JSON object will be converted to an event.</span></span>
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
#### <a name="output---2-events"></a><span data-ttu-id="65a22-140">Kimenet – 2 esemény</span><span class="sxs-lookup"><span data-stu-id="65a22-140">Output - 2 Events</span></span>

|<span data-ttu-id="65a22-141">id</span><span class="sxs-lookup"><span data-stu-id="65a22-141">id</span></span>|<span data-ttu-id="65a22-142">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="65a22-142">timestamp</span></span>|
|--------|---------------|
|<span data-ttu-id="65a22-143">device1</span><span class="sxs-lookup"><span data-stu-id="65a22-143">device1</span></span>|<span data-ttu-id="65a22-144">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="65a22-144">2016-01-08T01:08:00Z</span></span>|
|<span data-ttu-id="65a22-145">device2</span><span class="sxs-lookup"><span data-stu-id="65a22-145">device2</span></span>|<span data-ttu-id="65a22-146">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="65a22-146">2016-01-08T01:17:00Z</span></span>|
### <a name="sample-3"></a><span data-ttu-id="65a22-147">3. példa</span><span class="sxs-lookup"><span data-stu-id="65a22-147">Sample 3</span></span>

#### <a name="input"></a><span data-ttu-id="65a22-148">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="65a22-148">Input</span></span>

<span data-ttu-id="65a22-149">Két JSON-objektumot tartalmazó beágyazott JSON-tömbbel rendelkező JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="65a22-149">A JSON object with a nested JSON array containing two JSON objects.</span></span>
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
#### <a name="output---2-events"></a><span data-ttu-id="65a22-150">Kimenet – 2 esemény</span><span class="sxs-lookup"><span data-stu-id="65a22-150">Output - 2 Events</span></span>
<span data-ttu-id="65a22-151">A „location” tulajdonság mindegyik eseménybe át van másolva.</span><span class="sxs-lookup"><span data-stu-id="65a22-151">Note that the property "location" is copied over to each of the event.</span></span>

|<span data-ttu-id="65a22-152">location</span><span class="sxs-lookup"><span data-stu-id="65a22-152">location</span></span>|<span data-ttu-id="65a22-153">events.id</span><span class="sxs-lookup"><span data-stu-id="65a22-153">events.id</span></span>|<span data-ttu-id="65a22-154">events.timestamp</span><span class="sxs-lookup"><span data-stu-id="65a22-154">events.timestamp</span></span>|
|--------|---------------|----------------------|
|<span data-ttu-id="65a22-155">WestUs</span><span class="sxs-lookup"><span data-stu-id="65a22-155">WestUs</span></span>|<span data-ttu-id="65a22-156">device1</span><span class="sxs-lookup"><span data-stu-id="65a22-156">device1</span></span>|<span data-ttu-id="65a22-157">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="65a22-157">2016-01-08T01:08:00Z</span></span>|
|<span data-ttu-id="65a22-158">WestUs</span><span class="sxs-lookup"><span data-stu-id="65a22-158">WestUs</span></span>|<span data-ttu-id="65a22-159">device2</span><span class="sxs-lookup"><span data-stu-id="65a22-159">device2</span></span>|<span data-ttu-id="65a22-160">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="65a22-160">2016-01-08T01:17:00Z</span></span>|

### <a name="sample-4"></a><span data-ttu-id="65a22-161">4. példa</span><span class="sxs-lookup"><span data-stu-id="65a22-161">Sample 4</span></span>

#### <a name="input"></a><span data-ttu-id="65a22-162">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="65a22-162">Input</span></span>

<span data-ttu-id="65a22-163">Két JSON-objektumot tartalmazó beágyazott JSON-tömbbel rendelkező JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="65a22-163">A JSON object with a nested JSON array containing two JSON objects.</span></span> <span data-ttu-id="65a22-164">Ez a bemenet azt szemlélteti, hogy a komplex JSON-objektumban a globális tulajdonságok is szerepelhetnek.</span><span class="sxs-lookup"><span data-stu-id="65a22-164">This input demonstrates that the global properties may be represented by the complex JSON object.</span></span>

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
#### <a name="output---2-events"></a><span data-ttu-id="65a22-165">Kimenet – 2 esemény</span><span class="sxs-lookup"><span data-stu-id="65a22-165">Output - 2 Events</span></span>

|<span data-ttu-id="65a22-166">location</span><span class="sxs-lookup"><span data-stu-id="65a22-166">location</span></span>|<span data-ttu-id="65a22-167">manufacturer.name</span><span class="sxs-lookup"><span data-stu-id="65a22-167">manufacturer.name</span></span>|<span data-ttu-id="65a22-168">manufacturer.location</span><span class="sxs-lookup"><span data-stu-id="65a22-168">manufacturer.location</span></span>|<span data-ttu-id="65a22-169">events.id</span><span class="sxs-lookup"><span data-stu-id="65a22-169">events.id</span></span>|<span data-ttu-id="65a22-170">events.timestamp</span><span class="sxs-lookup"><span data-stu-id="65a22-170">events.timestamp</span></span>|<span data-ttu-id="65a22-171">events.data.type</span><span class="sxs-lookup"><span data-stu-id="65a22-171">events.data.type</span></span>|<span data-ttu-id="65a22-172">events.data.units</span><span class="sxs-lookup"><span data-stu-id="65a22-172">events.data.units</span></span>|<span data-ttu-id="65a22-173">events.data.value</span><span class="sxs-lookup"><span data-stu-id="65a22-173">events.data.value</span></span>|
|---|---|---|---|---|---|---|---|
|<span data-ttu-id="65a22-174">WestUs</span><span class="sxs-lookup"><span data-stu-id="65a22-174">WestUs</span></span>|<span data-ttu-id="65a22-175">manufacturer1</span><span class="sxs-lookup"><span data-stu-id="65a22-175">manufacturer1</span></span>|<span data-ttu-id="65a22-176">EastUs</span><span class="sxs-lookup"><span data-stu-id="65a22-176">EastUs</span></span>|<span data-ttu-id="65a22-177">device1</span><span class="sxs-lookup"><span data-stu-id="65a22-177">device1</span></span>|<span data-ttu-id="65a22-178">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="65a22-178">2016-01-08T01:08:00Z</span></span>|<span data-ttu-id="65a22-179">pressure</span><span class="sxs-lookup"><span data-stu-id="65a22-179">pressure</span></span>|<span data-ttu-id="65a22-180">psi</span><span class="sxs-lookup"><span data-stu-id="65a22-180">psi</span></span>|<span data-ttu-id="65a22-181">108.09</span><span class="sxs-lookup"><span data-stu-id="65a22-181">108.09</span></span>|
|<span data-ttu-id="65a22-182">WestUs</span><span class="sxs-lookup"><span data-stu-id="65a22-182">WestUs</span></span>|<span data-ttu-id="65a22-183">manufacturer1</span><span class="sxs-lookup"><span data-stu-id="65a22-183">manufacturer1</span></span>|<span data-ttu-id="65a22-184">EastUs</span><span class="sxs-lookup"><span data-stu-id="65a22-184">EastUs</span></span>|<span data-ttu-id="65a22-185">device2</span><span class="sxs-lookup"><span data-stu-id="65a22-185">device2</span></span>|<span data-ttu-id="65a22-186">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="65a22-186">2016-01-08T01:17:00Z</span></span>|<span data-ttu-id="65a22-187">vibration</span><span class="sxs-lookup"><span data-stu-id="65a22-187">vibration</span></span>|<span data-ttu-id="65a22-188">abs G</span><span class="sxs-lookup"><span data-stu-id="65a22-188">abs G</span></span>|<span data-ttu-id="65a22-189">217.09</span><span class="sxs-lookup"><span data-stu-id="65a22-189">217.09</span></span>|

## <a name="next-steps"></a><span data-ttu-id="65a22-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65a22-190">Next steps</span></span>

* <span data-ttu-id="65a22-191">A környezet megtekintése a [Time Series Insights portálon](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="65a22-191">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>

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
# <a name="send-events-tooa-time-series-insights-environment-using-event-hub"></a>Küldés események tooa idő adatsorozat Insights környezet az event hubs használatával

Ez az oktatóanyag azt ismerteti, hogyan toocreate és az event hubs konfigurálása, és futtasson egy minta alkalmazás toopush események. Ha már van JSON formátumú eseményeket tartalmazó eseményközpontja, ugorja át ezt az oktatóanyagot, és tekintse meg a környezetet a [Time Series Insightsban](https://insights.timeseries.azure.com).

## <a name="configure-an-event-hub"></a>Eseményközpont konfigurálása
1. toocreate eseményközpontban, kövesse az utasításokat az Eseményközpont hello [dokumentáció](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

2. Olyan fogyasztói csoportot hozzon létre, amelyet csak a Time Series Insights-eseményforrás használ.

  > [!IMPORTANT]
  > Ügyeljen arra, hogy ezt a fogyasztói csoportot ne használja másik szolgáltatás (például Stream Analytics-feladat vagy másik Time Series Insights-környezet). Fogyasztói csoportot más szolgáltatások használata esetén olvassa el a művelet negatívan befolyásolja az ebben a környezetben, és más szolgáltatások hello. Ha a "$Default" fogyasztói csoportot hello használ, előfordulhat toopotential újbóli más olvasók által.

  ![Az eseményközpont fogyasztói csoportjának kiválasztása](media/send-events/consumer-group.png)

3. Hello eseményközpontok felé, hozzon létre "MySendPolicy" hello csharp mintát, amely használt toosend események.

  ![A Megosztott elérési házirendek kiválasztása, majd kattintás a Hozzáadás gombra](media/send-events/shared-access-policy.png)  

  ![Új megosztott elérési házirend hozzáadása](media/send-events/shared-access-policy-2.png)  

## <a name="create-time-series-insights-event-source"></a>Time Series Insights-eseményforrás létrehozása
1. Ha még nem hozott létre az eseményforrás, hajtsa végre az [ezeket az utasításokat](time-series-insights-add-event-source.md) toocreate egy esemény forrását.

2. Adja meg a "deviceTimestamp" hello időbélyeg-tulajdonság neve – ezzel a tulajdonsággal, a tényleges időbélyeg hello csharp mintában hello. hello időbélyeg-tulajdonság neve a kis-és nagybetűket, és értékek hello formátumot kell követnie __éééé-hh-nnTóó: pp:. FFFFFFFK__ , JSON tooevent hub elküldésekor. Ha hello tulajdonság nem létezik a hello esemény, majd hello event hub várólistán lévő idő szolgál.

  ![Eseményforrás létrehozása](media/send-events/event-source-1.png)

## <a name="sample-code-toopush-events"></a>A minta kód toopush események
1. Nyissa meg toohello event hub házirend "MySendPolicy", és másolja a hello kapcsolati karakterláncot hello házirend kulccsal.

  ![A MySendPolicy kapcsolati karakterlánc másolása](media/send-events/sample-code-connection-string.png)

2. Futtassa a következő kód hello adott toosend 600 események (Event) hello három eszközökhöz. Frissítse az `eventHubConnectionString` elemet a kapcsolati karakterlánccal.

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
## <a name="supported-json-shapes"></a>Támogatott JSON-alakzatok
### <a name="sample-1"></a>1. példa

#### <a name="input"></a>Input (Bemenet)

Egyszerű JSON-objektum.

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---1-event"></a>Kimenet – 1 esemény

|id|időbélyeg|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|

### <a name="sample-2"></a>2. példa

#### <a name="input"></a>Input (Bemenet)
JSON-tömb két JSON-objektummal. Minden JSON-objektum lesz konvertált tooan esemény.
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
#### <a name="output---2-events"></a>Kimenet – 2 esemény

|id|időbélyeg|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|
### <a name="sample-3"></a>3. példa

#### <a name="input"></a>Input (Bemenet)

Két JSON-objektumot tartalmazó beágyazott JSON-tömbbel rendelkező JSON-objektum.
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
#### <a name="output---2-events"></a>Kimenet – 2 esemény
Vegye figyelembe, hogy hello tulajdonság "hely" hello esemény tooeach keresztül másolja a rendszer.

|location|events.id|events.timestamp|
|--------|---------------|----------------------|
|WestUs|device1|2016-01-08T01:08:00Z|
|WestUs|device2|2016-01-08T01:17:00Z|

### <a name="sample-4"></a>4. példa

#### <a name="input"></a>Input (Bemenet)

Két JSON-objektumot tartalmazó beágyazott JSON-tömbbel rendelkező JSON-objektum. A bemeneti mutatja be, hogy hello globális tulajdonságok képviselheti hello összetett JSON-objektumból.

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
#### <a name="output---2-events"></a>Kimenet – 2 esemény

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Következő lépések

* A környezet megtekintése a [Time Series Insights portálon](https://insights.timeseries.azure.com)

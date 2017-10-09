---
title: "aaaAzure funkciók Event Hubs kötések |} Microsoft Docs"
description: "Megértéséhez hogyan toouse Azure Event Hubs kötések Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/20/2017
ms.author: wesmc
ms.openlocfilehash: e864f032ad5ac58d318c9843c3844b5642733a70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-event-hubs-bindings"></a>Az Azure Functions az Event Hubs kötések
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk azt ismerteti, hogyan tooconfigure és [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) Azure Functions kötéseit.
Az Azure Functions támogatja indítható el, és az Event Hubs kötései kimeneti.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Ha új tooAzure Event Hubs, lásd: hello [Event Hubs – áttekintés](../event-hubs/event-hubs-what-is-event-hubs.md).

<a name="trigger"></a>

## <a name="event-hub-trigger"></a>Hub eseményindító
Használjon hello Event Hubs tooan event hub eseményfelhasználó küldött toorespond tooan esemény következik be. Olvasási hozzáférés toohello event hub tooset hello trigger mentése kell rendelkeznie.

hello Event Hubs függvény eseményindító használja a következő JSON-objektum a hello hello `bindings` function.json tömbje:

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of hello event hub>",
    "consumerGroup": "Consumer group toouse - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

`consumerGroup`van egy nem kötelező tulajdonság használt tooset hello [fogyasztói csoportot](../event-hubs/event-hubs-features.md#event-consumers) toosubscribe tooevents használt hello központban. Ha nincs megadva, hello `$Default` fogyasztói csoportot használja.  
`connection`hello kapcsolati karakterlánc toohello event hub névtér tartalmazó Alkalmazásbeállítás hello nevének kell lennie.
Másolja a kapcsolati karakterláncot hello kattintva **kapcsolatadatok** hello gombra *névtér*, nem hello eseményközpont magát. Ez a kapcsolati karakterlánc rendelkeznie kell legalább olvasási engedélyek tooactivate hello eseményindító.

[További beállítások](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) megadott egy host.json a fájl toofurther finom Event Hubs eseményindítók hangolására lehet.  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Eseményindító kihasználtsága
Az Event Hubs funkció aktiválása esetén, amely elindítja az üdvözlőüzenetére átad a hello függvény egy karakterlánc.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Eseményindító minta
Tegyük fel, hogy rendelkezik a következő Event Hubs trigger hello hello `bindings` function.json tömbje:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Lásd: hello nyelvspecifikus minta, amelyre bejelentkezik hello hub eseményindító hello üzenet törzsét.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>A C# eseményindító minta #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Akkor is jelentkezhet hello esemény egy [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objektum, amely lehetővé teszi az éri toohello esemény metaadatait.

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

tooreceive események kötegekben, módosítsa a hello metódus-aláírás túl`string[]` vagy `EventData[]`.

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Az F # eseményindító minta #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>A node.js eseményindító minta

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a>Az Event Hubs kimeneti kötése
Az Event Hubs használja hello kimeneti kötés toowrite események tooan event hub eseménystreambe. Küldési engedéllyel tooan event hub toowrite események tooit kell rendelkeznie.

hello kimeneti kötés használja a következő JSON-objektum a hello hello `bindings` function.json tömbje:

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

`connection`hello kapcsolati karakterlánc toohello event hub névtér tartalmazó Alkalmazásbeállítás hello nevének kell lennie.
Másolja a kapcsolati karakterláncot hello kattintva **kapcsolatadatok** hello gombra *névtér*, nem hello eseményközpont magát. Ez a kapcsolati karakterlánc küldési engedéllyel toosend hello üzenet toohello eseményfelhasználó kell rendelkeznie.

## <a name="output-usage"></a>Kimeneti használata
Ez a szakasz bemutatja, hogyan toouse az Eseményközpontok kimeneti kötelező a funkciókódot.

A következő paraméter típusa hello a kimenetre küldheti üzenetek konfigurált toohello eseményközpont:

* `out string`
* `ICollector<string>`(toooutput több üzenetek)
* `IAsyncCollector<string>`(aszinkron verzióját `ICollector<T>`)

<a name="outputsample"></a>

## <a name="output-sample"></a>Minta kimenet
Tegyük fel, hogy rendelkezik hello következő Event Hubs kimeneti hello kötések `bindings` function.json tömbje:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Lásd: hello nyelvspecifikus minta egy toohello még akkor is, eseményfelhasználó írja.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>A C# kimeneti minta #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Vagy toocreate több üzenetet:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Az F # kimeneti minta #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a>A Node.js kimeneti minta

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Vagy toosend több üzenetet

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

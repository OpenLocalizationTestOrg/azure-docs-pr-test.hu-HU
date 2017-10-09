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
# <a name="azure-functions-event-hubs-bindings"></a><span data-ttu-id="1dde1-104">Az Azure Functions az Event Hubs kötések</span><span class="sxs-lookup"><span data-stu-id="1dde1-104">Azure Functions Event Hubs bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="1dde1-105">Ez a cikk azt ismerteti, hogyan tooconfigure és [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) Azure Functions kötéseit.</span><span class="sxs-lookup"><span data-stu-id="1dde1-105">This article explains how tooconfigure and use [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindings for Azure Functions.</span></span>
<span data-ttu-id="1dde1-106">Az Azure Functions támogatja indítható el, és az Event Hubs kötései kimeneti.</span><span class="sxs-lookup"><span data-stu-id="1dde1-106">Azure Functions supports trigger and output bindings for Event Hubs.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="1dde1-107">Ha új tooAzure Event Hubs, lásd: hello [Event Hubs – áttekintés](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="1dde1-107">If you are new tooAzure Event Hubs, see hello [Event Hubs overview](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

<a name="trigger"></a>

## <a name="event-hub-trigger"></a><span data-ttu-id="1dde1-108">Hub eseményindító</span><span class="sxs-lookup"><span data-stu-id="1dde1-108">Event hub trigger</span></span>
<span data-ttu-id="1dde1-109">Használjon hello Event Hubs tooan event hub eseményfelhasználó küldött toorespond tooan esemény következik be.</span><span class="sxs-lookup"><span data-stu-id="1dde1-109">Use hello Event Hubs trigger toorespond tooan event sent tooan event hub event stream.</span></span> <span data-ttu-id="1dde1-110">Olvasási hozzáférés toohello event hub tooset hello trigger mentése kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1dde1-110">You must have read access toohello event hub tooset up hello trigger.</span></span>

<span data-ttu-id="1dde1-111">hello Event Hubs függvény eseményindító használja a következő JSON-objektum a hello hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="1dde1-111">hello Event Hubs function trigger uses hello following JSON object in hello `bindings` array of function.json:</span></span>

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

<span data-ttu-id="1dde1-112">`consumerGroup`van egy nem kötelező tulajdonság használt tooset hello [fogyasztói csoportot](../event-hubs/event-hubs-features.md#event-consumers) toosubscribe tooevents használt hello központban.</span><span class="sxs-lookup"><span data-stu-id="1dde1-112">`consumerGroup` is an optional property used tooset hello [consumer group](../event-hubs/event-hubs-features.md#event-consumers) used toosubscribe tooevents in hello hub.</span></span> <span data-ttu-id="1dde1-113">Ha nincs megadva, hello `$Default` fogyasztói csoportot használja.</span><span class="sxs-lookup"><span data-stu-id="1dde1-113">If omitted, hello `$Default` consumer group is used.</span></span>  
<span data-ttu-id="1dde1-114">`connection`hello kapcsolati karakterlánc toohello event hub névtér tartalmazó Alkalmazásbeállítás hello nevének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1dde1-114">`connection` must be hello name of an app setting that contains hello connection string toohello event hub's namespace.</span></span>
<span data-ttu-id="1dde1-115">Másolja a kapcsolati karakterláncot hello kattintva **kapcsolatadatok** hello gombra *névtér*, nem hello eseményközpont magát.</span><span class="sxs-lookup"><span data-stu-id="1dde1-115">Copy this connection string by clicking hello **Connection Information** button for hello *namespace*, not hello event hub itself.</span></span> <span data-ttu-id="1dde1-116">Ez a kapcsolati karakterlánc rendelkeznie kell legalább olvasási engedélyek tooactivate hello eseményindító.</span><span class="sxs-lookup"><span data-stu-id="1dde1-116">This connection string must have at least read permissions tooactivate hello trigger.</span></span>

<span data-ttu-id="1dde1-117">[További beállítások](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) megadott egy host.json a fájl toofurther finom Event Hubs eseményindítók hangolására lehet.</span><span class="sxs-lookup"><span data-stu-id="1dde1-117">[Additional settings](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) can be provided in a host.json file toofurther fine tune Event Hubs triggers.</span></span>  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="1dde1-118">Eseményindító kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="1dde1-118">Trigger usage</span></span>
<span data-ttu-id="1dde1-119">Az Event Hubs funkció aktiválása esetén, amely elindítja az üdvözlőüzenetére átad a hello függvény egy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1dde1-119">When an Event Hubs trigger function is triggered, hello message that triggers it is passed into hello function as a string.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="1dde1-120">Eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="1dde1-120">Trigger sample</span></span>
<span data-ttu-id="1dde1-121">Tegyük fel, hogy rendelkezik a következő Event Hubs trigger hello hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="1dde1-121">Suppose you have hello following Event Hubs trigger in hello `bindings` array of function.json:</span></span>

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

<span data-ttu-id="1dde1-122">Lásd: hello nyelvspecifikus minta, amelyre bejelentkezik hello hub eseményindító hello üzenet törzsét.</span><span class="sxs-lookup"><span data-stu-id="1dde1-122">See hello language-specific sample that logs hello message body of hello event hub trigger.</span></span>

* [<span data-ttu-id="1dde1-123">C#</span><span class="sxs-lookup"><span data-stu-id="1dde1-123">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="1dde1-124">F#</span><span class="sxs-lookup"><span data-stu-id="1dde1-124">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="1dde1-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="1dde1-125">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="1dde1-126">A C# eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="1dde1-126">Trigger sample in C#</span></span> #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="1dde1-127">Akkor is jelentkezhet hello esemény egy [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objektum, amely lehetővé teszi az éri toohello esemény metaadatait.</span><span class="sxs-lookup"><span data-stu-id="1dde1-127">You can also receive hello event as an [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object, which gives you access toohello event metadata.</span></span>

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

<span data-ttu-id="1dde1-128">tooreceive események kötegekben, módosítsa a hello metódus-aláírás túl`string[]` vagy `EventData[]`.</span><span class="sxs-lookup"><span data-stu-id="1dde1-128">tooreceive events in a batch, change hello method signature too`string[]` or `EventData[]`.</span></span>

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

### <a name="trigger-sample-in-f"></a><span data-ttu-id="1dde1-129">Az F # eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="1dde1-129">Trigger sample in F#</span></span> #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="1dde1-130">A node.js eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="1dde1-130">Trigger sample in Node.js</span></span>

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a><span data-ttu-id="1dde1-131">Az Event Hubs kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="1dde1-131">Event Hubs output binding</span></span>
<span data-ttu-id="1dde1-132">Az Event Hubs használja hello kimeneti kötés toowrite események tooan event hub eseménystreambe.</span><span class="sxs-lookup"><span data-stu-id="1dde1-132">Use hello Event Hubs output binding toowrite events tooan event hub event stream.</span></span> <span data-ttu-id="1dde1-133">Küldési engedéllyel tooan event hub toowrite események tooit kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1dde1-133">You must have send permission tooan event hub toowrite events tooit.</span></span>

<span data-ttu-id="1dde1-134">hello kimeneti kötés használja a következő JSON-objektum a hello hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="1dde1-134">hello output binding uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

<span data-ttu-id="1dde1-135">`connection`hello kapcsolati karakterlánc toohello event hub névtér tartalmazó Alkalmazásbeállítás hello nevének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1dde1-135">`connection` must be hello name of an app setting that contains hello connection string toohello event hub's namespace.</span></span>
<span data-ttu-id="1dde1-136">Másolja a kapcsolati karakterláncot hello kattintva **kapcsolatadatok** hello gombra *névtér*, nem hello eseményközpont magát.</span><span class="sxs-lookup"><span data-stu-id="1dde1-136">Copy this connection string by clicking hello **Connection Information** button for hello *namespace*, not hello event hub itself.</span></span> <span data-ttu-id="1dde1-137">Ez a kapcsolati karakterlánc küldési engedéllyel toosend hello üzenet toohello eseményfelhasználó kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1dde1-137">This connection string must have send permissions toosend hello message toohello event stream.</span></span>

## <a name="output-usage"></a><span data-ttu-id="1dde1-138">Kimeneti használata</span><span class="sxs-lookup"><span data-stu-id="1dde1-138">Output usage</span></span>
<span data-ttu-id="1dde1-139">Ez a szakasz bemutatja, hogyan toouse az Eseményközpontok kimeneti kötelező a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="1dde1-139">This section shows you how toouse your Event Hubs output binding in your function code.</span></span>

<span data-ttu-id="1dde1-140">A következő paraméter típusa hello a kimenetre küldheti üzenetek konfigurált toohello eseményközpont:</span><span class="sxs-lookup"><span data-stu-id="1dde1-140">You can output messages toohello configured event hub with hello following parameter types:</span></span>

* `out string`
* <span data-ttu-id="1dde1-141">`ICollector<string>`(toooutput több üzenetek)</span><span class="sxs-lookup"><span data-stu-id="1dde1-141">`ICollector<string>` (toooutput multiple messages)</span></span>
* <span data-ttu-id="1dde1-142">`IAsyncCollector<string>`(aszinkron verzióját `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="1dde1-142">`IAsyncCollector<string>` (async version of `ICollector<T>`)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="1dde1-143">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="1dde1-143">Output sample</span></span>
<span data-ttu-id="1dde1-144">Tegyük fel, hogy rendelkezik hello következő Event Hubs kimeneti hello kötések `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="1dde1-144">Suppose you have hello following Event Hubs output binding in hello `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

<span data-ttu-id="1dde1-145">Lásd: hello nyelvspecifikus minta egy toohello még akkor is, eseményfelhasználó írja.</span><span class="sxs-lookup"><span data-stu-id="1dde1-145">See hello language-specific sample that writes an event toohello even stream.</span></span>

* [<span data-ttu-id="1dde1-146">C#</span><span class="sxs-lookup"><span data-stu-id="1dde1-146">C#</span></span>](#outcsharp)
* [<span data-ttu-id="1dde1-147">F#</span><span class="sxs-lookup"><span data-stu-id="1dde1-147">F#</span></span>](#outfsharp)
* [<span data-ttu-id="1dde1-148">Node.js</span><span class="sxs-lookup"><span data-stu-id="1dde1-148">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="1dde1-149">A C# kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="1dde1-149">Output sample in C#</span></span> #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

<span data-ttu-id="1dde1-150">Vagy toocreate több üzenetet:</span><span class="sxs-lookup"><span data-stu-id="1dde1-150">Or, toocreate multiple messages:</span></span>

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

### <a name="output-sample-in-f"></a><span data-ttu-id="1dde1-151">Az F # kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="1dde1-151">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a><span data-ttu-id="1dde1-152">A Node.js kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="1dde1-152">Output sample for Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

<span data-ttu-id="1dde1-153">Vagy toosend több üzenetet</span><span class="sxs-lookup"><span data-stu-id="1dde1-153">Or, toosend multiple messages,</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="1dde1-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1dde1-154">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

---
title: "Az Azure Functions az Event Hubs kötések |} Microsoft Docs"
description: "Azure Event Hubs kötések az Azure Functions használatának megismerése."
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
ms.openlocfilehash: 19021bef8b7156b3049f43b0275c0ed0c6b22514
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-event-hubs-bindings"></a><span data-ttu-id="da46e-104">Az Azure Functions az Event Hubs kötések</span><span class="sxs-lookup"><span data-stu-id="da46e-104">Azure Functions Event Hubs bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="da46e-105">Ez a cikk azt ismerteti, hogyan konfigurálhatja és használhatja [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) Azure Functions kötéseit.</span><span class="sxs-lookup"><span data-stu-id="da46e-105">This article explains how to configure and use [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindings for Azure Functions.</span></span>
<span data-ttu-id="da46e-106">Az Azure Functions támogatja indítható el, és az Event Hubs kötései kimeneti.</span><span class="sxs-lookup"><span data-stu-id="da46e-106">Azure Functions supports trigger and output bindings for Event Hubs.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="da46e-107">Ha most ismerkedik az Azure Event Hubs, tekintse meg a [Event Hubs – áttekintés](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="da46e-107">If you are new to Azure Event Hubs, see the [Event Hubs overview](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

<a name="trigger"></a>

## <a name="event-hub-trigger"></a><span data-ttu-id="da46e-108">Hub eseményindító</span><span class="sxs-lookup"><span data-stu-id="da46e-108">Event hub trigger</span></span>
<span data-ttu-id="da46e-109">Az Event Hubs eseményindító segítségével egy event hub eseményfelhasználó küldött esemény válaszolni.</span><span class="sxs-lookup"><span data-stu-id="da46e-109">Use the Event Hubs trigger to respond to an event sent to an event hub event stream.</span></span> <span data-ttu-id="da46e-110">Az event hubs az eseményindító beállítása olvasási hozzáféréssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="da46e-110">You must have read access to the event hub to set up the trigger.</span></span>

<span data-ttu-id="da46e-111">Az Event Hubs függvény eseményindító használja a következő JSON-objektum a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="da46e-111">The Event Hubs function trigger uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of the event hub>",
    "consumerGroup": "Consumer group to use - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

<span data-ttu-id="da46e-112">`consumerGroup`egy nem kötelező tulajdonság beállításához használja a [fogyasztói csoportot](../event-hubs/event-hubs-features.md#event-consumers) használt események központban előfizetni.</span><span class="sxs-lookup"><span data-stu-id="da46e-112">`consumerGroup` is an optional property used to set the [consumer group](../event-hubs/event-hubs-features.md#event-consumers) used to subscribe to events in the hub.</span></span> <span data-ttu-id="da46e-113">Ha nincs megadva, a `$Default` fogyasztói csoportot használja.</span><span class="sxs-lookup"><span data-stu-id="da46e-113">If omitted, the `$Default` consumer group is used.</span></span>  
<span data-ttu-id="da46e-114">`connection`egy Alkalmazásbeállítás, amely tartalmazza a kapcsolati karakterláncot az event hubs névtér nevének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="da46e-114">`connection` must be the name of an app setting that contains the connection string to the event hub's namespace.</span></span>
<span data-ttu-id="da46e-115">Másolja a kapcsolati karakterláncot kattintva a **kapcsolatadatok** gombra kattint, az a *névtér*, nem magát az eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="da46e-115">Copy this connection string by clicking the **Connection Information** button for the *namespace*, not the event hub itself.</span></span> <span data-ttu-id="da46e-116">Ez a kapcsolati karakterlánc kell rendelkeznie legalább olvasási engedéllyel az eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="da46e-116">This connection string must have at least read permissions to activate the trigger.</span></span>

<span data-ttu-id="da46e-117">[További beállítások](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) egy host.json fájlban, és további Event Hubs eseményindítók megadható.</span><span class="sxs-lookup"><span data-stu-id="da46e-117">[Additional settings](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) can be provided in a host.json file to further fine tune Event Hubs triggers.</span></span>  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="da46e-118">Eseményindító kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="da46e-118">Trigger usage</span></span>
<span data-ttu-id="da46e-119">Az Event Hubs funkció aktiválása esetén az üzenet, amely elindítja az átad a függvény egy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="da46e-119">When an Event Hubs trigger function is triggered, the message that triggers it is passed into the function as a string.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="da46e-120">Eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="da46e-120">Trigger sample</span></span>
<span data-ttu-id="da46e-121">Tegyük fel, hogy a következő Event Hubs trigger a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="da46e-121">Suppose you have the following Event Hubs trigger in the `bindings` array of function.json:</span></span>

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

<span data-ttu-id="da46e-122">Tekintse meg a nyelvspecifikus mintát, amelyre bejelentkezik az eseményindító hub üzenet törzsét.</span><span class="sxs-lookup"><span data-stu-id="da46e-122">See the language-specific sample that logs the message body of the event hub trigger.</span></span>

* [<span data-ttu-id="da46e-123">C#</span><span class="sxs-lookup"><span data-stu-id="da46e-123">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="da46e-124">F#</span><span class="sxs-lookup"><span data-stu-id="da46e-124">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="da46e-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="da46e-125">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="da46e-126">A C# eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="da46e-126">Trigger sample in C#</span></span> #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="da46e-127">Az esemény akkor is jelentkezhet egy [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objektum, amely hozzáférést biztosít az esemény-metaadatok.</span><span class="sxs-lookup"><span data-stu-id="da46e-127">You can also receive the event as an [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object, which gives you access to the event metadata.</span></span>

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

<span data-ttu-id="da46e-128">Események fogadásához egy kötegben, módosítsa a metódus-aláírás `string[]` vagy `EventData[]`.</span><span class="sxs-lookup"><span data-stu-id="da46e-128">To receive events in a batch, change the method signature to `string[]` or `EventData[]`.</span></span>

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

### <a name="trigger-sample-in-f"></a><span data-ttu-id="da46e-129">Az F # eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="da46e-129">Trigger sample in F#</span></span> #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="da46e-130">A node.js eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="da46e-130">Trigger sample in Node.js</span></span>

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a><span data-ttu-id="da46e-131">Az Event Hubs kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="da46e-131">Event Hubs output binding</span></span>
<span data-ttu-id="da46e-132">Az Event Hubs kimeneti kötése beírni az eseményeket az event hub eseményfelhasználó használja.</span><span class="sxs-lookup"><span data-stu-id="da46e-132">Use the Event Hubs output binding to write events to an event hub event stream.</span></span> <span data-ttu-id="da46e-133">Események írhat az eseményközpontba a küldési engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="da46e-133">You must have send permission to an event hub to write events to it.</span></span>

<span data-ttu-id="da46e-134">A kimeneti kötés használja a következő JSON-objektum a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="da46e-134">The output binding uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

<span data-ttu-id="da46e-135">`connection`egy Alkalmazásbeállítás, amely tartalmazza a kapcsolati karakterláncot az event hubs névtér nevének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="da46e-135">`connection` must be the name of an app setting that contains the connection string to the event hub's namespace.</span></span>
<span data-ttu-id="da46e-136">Másolja a kapcsolati karakterláncot kattintva a **kapcsolatadatok** gombra kattint, az a *névtér*, nem magát az eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="da46e-136">Copy this connection string by clicking the **Connection Information** button for the *namespace*, not the event hub itself.</span></span> <span data-ttu-id="da46e-137">Ez a kapcsolati karakterlánc az üzenetet küldeni az eseménystream küldési engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="da46e-137">This connection string must have send permissions to send the message to the event stream.</span></span>

## <a name="output-usage"></a><span data-ttu-id="da46e-138">Kimeneti használata</span><span class="sxs-lookup"><span data-stu-id="da46e-138">Output usage</span></span>
<span data-ttu-id="da46e-139">Ez a szakasz bemutatja, hogyan használható az Event Hubs kimeneti a funkciókódot kötelező.</span><span class="sxs-lookup"><span data-stu-id="da46e-139">This section shows you how to use your Event Hubs output binding in your function code.</span></span>

<span data-ttu-id="da46e-140">A következő paraméter típusú kimenetre küldheti a konfigurált eseményközpontba üzenetek:</span><span class="sxs-lookup"><span data-stu-id="da46e-140">You can output messages to the configured event hub with the following parameter types:</span></span>

* `out string`
* <span data-ttu-id="da46e-141">`ICollector<string>`(a kimeneti több üzenetek)</span><span class="sxs-lookup"><span data-stu-id="da46e-141">`ICollector<string>` (to output multiple messages)</span></span>
* <span data-ttu-id="da46e-142">`IAsyncCollector<string>`(aszinkron verzióját `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="da46e-142">`IAsyncCollector<string>` (async version of `ICollector<T>`)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="da46e-143">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="da46e-143">Output sample</span></span>
<span data-ttu-id="da46e-144">Tegyük fel, hogy a következő Event Hubs kimeneti kötések a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="da46e-144">Suppose you have the following Event Hubs output binding in the `bindings` array of function.json:</span></span>

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

<span data-ttu-id="da46e-145">Tekintse meg a nyelvspecifikus mintát, amely egy eseményt ír a még akkor is, az adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="da46e-145">See the language-specific sample that writes an event to the even stream.</span></span>

* [<span data-ttu-id="da46e-146">C#</span><span class="sxs-lookup"><span data-stu-id="da46e-146">C#</span></span>](#outcsharp)
* [<span data-ttu-id="da46e-147">F#</span><span class="sxs-lookup"><span data-stu-id="da46e-147">F#</span></span>](#outfsharp)
* [<span data-ttu-id="da46e-148">Node.js</span><span class="sxs-lookup"><span data-stu-id="da46e-148">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="da46e-149">A C# kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="da46e-149">Output sample in C#</span></span> #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

<span data-ttu-id="da46e-150">Vagy, hozzon létre több üzenetet:</span><span class="sxs-lookup"><span data-stu-id="da46e-150">Or, to create multiple messages:</span></span>

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

### <a name="output-sample-in-f"></a><span data-ttu-id="da46e-151">Az F # kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="da46e-151">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a><span data-ttu-id="da46e-152">A Node.js kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="da46e-152">Output sample for Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

<span data-ttu-id="da46e-153">Vagy több üzenetet küldeni.</span><span class="sxs-lookup"><span data-stu-id="da46e-153">Or, to send multiple messages,</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="da46e-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da46e-154">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

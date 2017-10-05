---
title: "Az Azure Functions a Service Bus-eseményindítók és kötések |} Microsoft Docs"
description: "Azure Service Bus-eseményindítók és kötések az Azure Functions használatának megismerése."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/01/2017
ms.author: glenga
ms.openlocfilehash: b3ee306cd37ebf88dc9369ccc2dc6c670557fd5a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-service-bus-bindings"></a><span data-ttu-id="37b41-104">Az Azure Functions a Service Bus kötések</span><span class="sxs-lookup"><span data-stu-id="37b41-104">Azure Functions Service Bus bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="37b41-105">Ez a cikk azt ismerteti, hogyan konfigurálását és az Azure Functions Azure Service Bus kötések használatát.</span><span class="sxs-lookup"><span data-stu-id="37b41-105">This article explains how to configure and work with Azure Service Bus bindings in Azure Functions.</span></span> 

<span data-ttu-id="37b41-106">Az Azure Functions támogatja aktiválhatja és Service Bus-üzenetsorok és témakörök kimeneti.</span><span class="sxs-lookup"><span data-stu-id="37b41-106">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a><span data-ttu-id="37b41-107">A Service Bus eseményindító</span><span class="sxs-lookup"><span data-stu-id="37b41-107">Service Bus trigger</span></span>
<span data-ttu-id="37b41-108">A Service Bus eseményindító használatával a Service Bus-üzenetsor vagy témakör üzenetek válaszolni.</span><span class="sxs-lookup"><span data-stu-id="37b41-108">Use the Service Bus trigger to respond to messages from a Service Bus queue or topic.</span></span> 

<span data-ttu-id="37b41-109">A Service Bus-üzenetsor és a témakör eseményindítók határozzák meg a következő JSON-objektumok a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="37b41-109">The Service Bus queue and topic triggers are defined by the following JSON objects in the `bindings` array of function.json:</span></span>

* <span data-ttu-id="37b41-110">*várólista* eseményindító:</span><span class="sxs-lookup"><span data-stu-id="37b41-110">*queue* trigger:</span></span>

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* <span data-ttu-id="37b41-111">*a témakör* eseményindító:</span><span class="sxs-lookup"><span data-stu-id="37b41-111">*topic* trigger:</span></span>

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

<span data-ttu-id="37b41-112">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="37b41-112">Note the following:</span></span>

* <span data-ttu-id="37b41-113">A `connection`, [alkalmazásbeállítás létrehozása az függvény alkalmazásban](functions-how-to-use-azure-function-app-settings.md) , amely tartalmazza a Service Bus-névtér a kapcsolati karakterláncot, majd adja meg a az Alkalmazásbeállítás nevét a `connection` az eseményindító tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="37b41-113">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains the connection string to your Service Bus namespace, then specify the name of the app setting in the `connection` property in your trigger.</span></span> <span data-ttu-id="37b41-114">Szerezze be a kapcsolati karakterláncot a jelenik meg a lépéseket követve [a felügyeleti hitelesítő adatok beszerzése](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="37b41-114">You obtain the connection string by following the steps shown at [Obtain the management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="37b41-115">A kapcsolati karakterláncnak kell lennie, a Service Bus-névtér, nem kizárólagosan az adott üzenetsor vagy témakör.</span><span class="sxs-lookup"><span data-stu-id="37b41-115">The connection string must be for a Service Bus namespace, not limited to a specific queue or topic.</span></span>
  <span data-ttu-id="37b41-116">Ha nem adja meg `connection` üres, az eseményindító azt feltételezi, hogy egy alapértelmezett Service Bus kapcsolati karakterlánc van megadva egy alkalmazás nevű beállításával `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="37b41-116">If you leave `connection` empty, the trigger assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="37b41-117">A `accessRights`, elérhető értékek a következők `manage` és `listen`.</span><span class="sxs-lookup"><span data-stu-id="37b41-117">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="37b41-118">Az alapértelmezett érték `manage`, amely azt jelzi, hogy a `connection` rendelkezik a **kezelése** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="37b41-118">The default is `manage`, which indicates that the `connection` has the **Manage** permission.</span></span> <span data-ttu-id="37b41-119">Ha használja a kapcsolati karakterláncot, amely nem rendelkezik a **kezelése** engedély, `accessRights` való `listen`.</span><span class="sxs-lookup"><span data-stu-id="37b41-119">If you use a connection string that does not have the **Manage** permission, set `accessRights` to `listen`.</span></span> <span data-ttu-id="37b41-120">Ellenkező esetben a futásidejű meghiúsulhat igénylő műveletek végrehajtását megkísérlő funkciók jogosultságaik kezelését.</span><span class="sxs-lookup"><span data-stu-id="37b41-120">Otherwise, the Functions runtime might fail trying to do operations that require manage rights.</span></span>

## <a name="trigger-behavior"></a><span data-ttu-id="37b41-121">Eseményindító viselkedése</span><span class="sxs-lookup"><span data-stu-id="37b41-121">Trigger behavior</span></span>
* <span data-ttu-id="37b41-122">**Single-threading** - alapértelmezés szerint a funkciók futásidejű folyamatok több üzenetet párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="37b41-122">**Single-threading** - By default, the Functions runtime processes multiple messages concurrently.</span></span> <span data-ttu-id="37b41-123">Állítsa át tudja irányítani a futtatókörnyezet egyszerre csak egyetlen üzenetsor vagy témakör üzenet feldolgozásához, `serviceBus.maxConcurrentCalls` 1 *host.json*.</span><span class="sxs-lookup"><span data-stu-id="37b41-123">To direct the runtime to process only a single queue or topic message at a time, set `serviceBus.maxConcurrentCalls` to 1 in *host.json*.</span></span> 
  <span data-ttu-id="37b41-124">További információ *host.json*, lásd: [mappaszerkezet](functions-reference.md#folder-structure) és [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="37b41-124">For information about *host.json*, see [Folder Structure](functions-reference.md#folder-structure) and [host.json](https://github .com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>
* <span data-ttu-id="37b41-125">**Elhalt üzenetek kezelésének** -Service Bus does saját elhalt üzenetek kezelésének, nem szabályozott, vagy az Azure Functions konfiguráció vagy a kód konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="37b41-125">**Poison message handling** - Service Bus does its own poison message handling, which can't be controlled or configured in Azure Functions configuration or code.</span></span> 
* <span data-ttu-id="37b41-126">**PeekLock viselkedés** -a Functions futtatókörnyezete az üzenetet kap [ `PeekLock` mód](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) és hívások `Complete` az üzenetet, ha a függvény futtatása sikeresen befejeződött, vagy a hívások `Abandon` Ha a parancs nem működik.</span><span class="sxs-lookup"><span data-stu-id="37b41-126">**PeekLock behavior** - The Functions runtime receives a message in [`PeekLock` mode](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) and calls `Complete` on the message if the function finishes successfully, or calls `Abandon` if the function fails.</span></span> 
  <span data-ttu-id="37b41-127">Ha a függvény futásakor hosszabb, mint a `PeekLock` automatikusan megújítják időtúllépés, a zárolás.</span><span class="sxs-lookup"><span data-stu-id="37b41-127">If the function runs longer than the `PeekLock` timeout, the lock is automatically renewed.</span></span>

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="37b41-128">Eseményindító kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="37b41-128">Trigger usage</span></span>
<span data-ttu-id="37b41-129">Ez a szakasz bemutatja, hogyan használható a Service Bus eseményindító a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="37b41-129">This section shows you how to use your Service Bus trigger in your function code.</span></span> 

<span data-ttu-id="37b41-130">A C# és F # a Service Bus eseményindító üzenet a következő bemeneti típusok bármelyikéhez deszerializálható:</span><span class="sxs-lookup"><span data-stu-id="37b41-130">In C# and F#, the Service Bus trigger message can be deserialized to any of the following input types:</span></span>

* <span data-ttu-id="37b41-131">`string`-hasznosak lehetnek a karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="37b41-131">`string` - useful for string messages</span></span>
* <span data-ttu-id="37b41-132">`byte[]`-hasznosak lehetnek a bináris adatok</span><span class="sxs-lookup"><span data-stu-id="37b41-132">`byte[]` - useful for binary data</span></span>
* <span data-ttu-id="37b41-133">Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) - hasznosak lehetnek a JSON-adatokból.</span><span class="sxs-lookup"><span data-stu-id="37b41-133">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized data.</span></span>
  <span data-ttu-id="37b41-134">Egy egyéni bemeneti típus, például a deklarálható `CustomType`, az Azure Functions megkísérli a JSON-adatok deszerializálása be a megadott típus.</span><span class="sxs-lookup"><span data-stu-id="37b41-134">If you declare a custom input type, such as `CustomType`, Azure Functions tries to deserialize the JSON data into your specified type.</span></span>
* <span data-ttu-id="37b41-135">`BrokeredMessage`-a deszerializált üzenet lehetővé teszi a [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="37b41-135">`BrokeredMessage` - gives you the deserialized message with the [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) method.</span></span>

<span data-ttu-id="37b41-136">A node.js a Service Bus eseményindító üzenet át a funkciót egy karakterlánc- vagy JSON-objektumból.</span><span class="sxs-lookup"><span data-stu-id="37b41-136">In Node.js, the Service Bus trigger message is passed into the function as either a string or JSON object.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="37b41-137">Eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="37b41-137">Trigger sample</span></span>
<span data-ttu-id="37b41-138">Tegyük fel, a következő function.json:</span><span class="sxs-lookup"><span data-stu-id="37b41-138">Suppose you have the following function.json:</span></span>

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

<span data-ttu-id="37b41-139">Tekintse meg a nyelvspecifikus mintát, amely egy Service Bus üzenetsor-üzenetet feldolgozza.</span><span class="sxs-lookup"><span data-stu-id="37b41-139">See the language-specific sample that processes a Service Bus queue message.</span></span>

* [<span data-ttu-id="37b41-140">C#</span><span class="sxs-lookup"><span data-stu-id="37b41-140">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="37b41-141">F#</span><span class="sxs-lookup"><span data-stu-id="37b41-141">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="37b41-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="37b41-142">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="37b41-143">A C# eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="37b41-143">Trigger sample in C#</span></span> #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a><span data-ttu-id="37b41-144">Az F # eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="37b41-144">Trigger sample in F#</span></span> #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="37b41-145">A node.js eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="37b41-145">Trigger sample in Node.js</span></span>

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a><span data-ttu-id="37b41-146">A Service Bus kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="37b41-146">Service Bus output binding</span></span>
<span data-ttu-id="37b41-147">A Service Bus várólista és ez a témakör kimeneti függvény használata a következő JSON-objektumok a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="37b41-147">The Service Bus queue and topic output for a function use the following JSON objects in the `bindings` array of function.json:</span></span>

* <span data-ttu-id="37b41-148">*várólista* kimenete:</span><span class="sxs-lookup"><span data-stu-id="37b41-148">*queue* output:</span></span>

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* <span data-ttu-id="37b41-149">*a témakör* kimenete:</span><span class="sxs-lookup"><span data-stu-id="37b41-149">*topic* output:</span></span>

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

<span data-ttu-id="37b41-150">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="37b41-150">Note the following:</span></span>

* <span data-ttu-id="37b41-151">A `connection`, [alkalmazásbeállítás létrehozása az függvény alkalmazásban](functions-how-to-use-azure-function-app-settings.md) , amely tartalmazza a Service Bus-névtér a kapcsolati karakterláncot, majd adja meg a az Alkalmazásbeállítás nevét a `connection` tulajdonság a kimeneti kötés.</span><span class="sxs-lookup"><span data-stu-id="37b41-151">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains the connection string to your Service Bus namespace, then specify the name of the app setting in the `connection` property in your output binding.</span></span> <span data-ttu-id="37b41-152">Szerezze be a kapcsolati karakterláncot a jelenik meg a lépéseket követve [a felügyeleti hitelesítő adatok beszerzése](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="37b41-152">You obtain the connection string by following the steps shown at [Obtain the management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="37b41-153">A kapcsolati karakterláncnak kell lennie, a Service Bus-névtér, nem kizárólagosan az adott üzenetsor vagy témakör.</span><span class="sxs-lookup"><span data-stu-id="37b41-153">The connection string must be for a Service Bus namespace, not limited to a specific queue or topic.</span></span>
  <span data-ttu-id="37b41-154">Ha nem adja meg `connection` üres, a kimeneti kötés azt feltételezi, hogy egy alapértelmezett Service Bus kapcsolati karakterlánc van megadva egy alkalmazás nevű beállításával `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="37b41-154">If you leave `connection` empty, the output binding assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="37b41-155">A `accessRights`, elérhető értékek a következők `manage` és `listen`.</span><span class="sxs-lookup"><span data-stu-id="37b41-155">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="37b41-156">Az alapértelmezett érték `manage`, amely azt jelzi, hogy a `connection` rendelkezik a **kezelése** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="37b41-156">The default is `manage`, which indicates that the `connection` has the **Manage** permission.</span></span> <span data-ttu-id="37b41-157">Ha használja a kapcsolati karakterláncot, amely nem rendelkezik a **kezelése** engedély, `accessRights` való `listen`.</span><span class="sxs-lookup"><span data-stu-id="37b41-157">If you use a connection string that does not have the **Manage** permission, set `accessRights` to `listen`.</span></span> <span data-ttu-id="37b41-158">Ellenkező esetben a futásidejű meghiúsulhat igénylő műveletek végrehajtását megkísérlő funkciók jogosultságaik kezelését.</span><span class="sxs-lookup"><span data-stu-id="37b41-158">Otherwise, the Functions runtime might fail trying to do operations that require manage rights.</span></span>

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="37b41-159">Kimeneti használata</span><span class="sxs-lookup"><span data-stu-id="37b41-159">Output usage</span></span>
<span data-ttu-id="37b41-160">C# és F #, az Azure Functions létrehozhat egy Service Bus-üzenetsor a következő típusok:</span><span class="sxs-lookup"><span data-stu-id="37b41-160">In C# and F#, Azure Functions can create a Service Bus queue message from any of the following types:</span></span>

* <span data-ttu-id="37b41-161">Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) -paraméterek definícióját a következőképpen néz `out T paramName` (C#).</span><span class="sxs-lookup"><span data-stu-id="37b41-161">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - Parameter definition looks like `out T paramName` (C#).</span></span>
  <span data-ttu-id="37b41-162">Funkciók deserializes az objektum JSON üzenetbe.</span><span class="sxs-lookup"><span data-stu-id="37b41-162">Functions deserializes the object into a JSON message.</span></span> <span data-ttu-id="37b41-163">Ha a kimeneti érték értéke null, ha a függvény kilép, funkciók az üzenet null objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="37b41-163">If the output value is null when the function exits, Functions creates the message with a null object.</span></span>
* <span data-ttu-id="37b41-164">`string`-Paraméterek definícióját a következőképpen néz `out string paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="37b41-164">`string` - Parameter definition looks like `out string paraName` (C#).</span></span> <span data-ttu-id="37b41-165">Ha a paraméter értéke nem null értékű, amikor a függvény kilép, a funkciók létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="37b41-165">If the parameter value is non-null when the function exits, Functions creates a message.</span></span>
* <span data-ttu-id="37b41-166">`byte[]`-Paraméterek definícióját a következőképpen néz `out byte[] paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="37b41-166">`byte[]` - Parameter definition looks like `out byte[] paraName` (C#).</span></span> <span data-ttu-id="37b41-167">Ha a paraméter értéke nem null értékű, amikor a függvény kilép, a funkciók létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="37b41-167">If the parameter value is non-null when the function exits, Functions creates a message.</span></span>
* <span data-ttu-id="37b41-168">`BrokeredMessage`Paraméterek definícióját a következőképpen néz `out BrokeredMessage paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="37b41-168">`BrokeredMessage` Parameter definition looks like `out BrokeredMessage paraName` (C#).</span></span> <span data-ttu-id="37b41-169">Ha a paraméter értéke nem null értékű, amikor a függvény kilép, a funkciók létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="37b41-169">If the parameter value is non-null when the function exits, Functions creates a message.</span></span>

<span data-ttu-id="37b41-170">A több üzenet létrehozása a C# függvényben, használhat `ICollector<T>` vagy `IAsyncCollector<T>`.</span><span class="sxs-lookup"><span data-stu-id="37b41-170">For creating multiple messages in a C# function, you can use `ICollector<T>` or `IAsyncCollector<T>`.</span></span> <span data-ttu-id="37b41-171">Egy üzenet jön létre, ha meghívja a `Add` metódust.</span><span class="sxs-lookup"><span data-stu-id="37b41-171">A message is created when you call the `Add` method.</span></span>

<span data-ttu-id="37b41-172">A node.js, rendelhet egy karakterlánc, egy bájttömböt vagy egy Javascript-objektum (deszerializálni a JSON-ba) `context.binding.<paramName>`.</span><span class="sxs-lookup"><span data-stu-id="37b41-172">In Node.js, you can assign a string, a byte array, or a Javascript object (deserialized into JSON) to `context.binding.<paramName>`.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="37b41-173">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="37b41-173">Output sample</span></span>
<span data-ttu-id="37b41-174">Tegyük fel, amely meghatározza egy Service Bus várólista kimenete a következő function.json:</span><span class="sxs-lookup"><span data-stu-id="37b41-174">Suppose you have the following function.json, that defines a Service Bus queue output:</span></span>

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

<span data-ttu-id="37b41-175">Tekintse meg a nyelvspecifikus mintát, amely egy üzenetet küld a service bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="37b41-175">See the language-specific sample that sends a message to the service bus queue.</span></span>

* [<span data-ttu-id="37b41-176">C#</span><span class="sxs-lookup"><span data-stu-id="37b41-176">C#</span></span>](#outcsharp)
* [<span data-ttu-id="37b41-177">F#</span><span class="sxs-lookup"><span data-stu-id="37b41-177">F#</span></span>](#outfsharp)
* [<span data-ttu-id="37b41-178">Node.js</span><span class="sxs-lookup"><span data-stu-id="37b41-178">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="37b41-179">A C# kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="37b41-179">Output sample in C#</span></span> #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

<span data-ttu-id="37b41-180">Vagy, hozzon létre több üzenetet:</span><span class="sxs-lookup"><span data-stu-id="37b41-180">Or, to create multiple messages:</span></span>

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a><span data-ttu-id="37b41-181">Az F # kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="37b41-181">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="37b41-182">Kimeneti minta node.js</span><span class="sxs-lookup"><span data-stu-id="37b41-182">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

<span data-ttu-id="37b41-183">Vagy, hozzon létre több üzenetet:</span><span class="sxs-lookup"><span data-stu-id="37b41-183">Or, to create multiple messages:</span></span>

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = [];
    context.bindings.outputSbQueueMsg.push("1 " + message);
    context.bindings.outputSbQueueMsg.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="37b41-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="37b41-184">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


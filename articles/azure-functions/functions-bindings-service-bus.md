---
title: "aaaAzure funkciók a Service Bus eseményindítók és kötések |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Service Bus eseményindítók és kötések az Azure Functions."
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
ms.openlocfilehash: dff9e89bd3840b8c11f91cae41e13502afc7aa60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-service-bus-bindings"></a><span data-ttu-id="ac95f-104">Az Azure Functions a Service Bus kötések</span><span class="sxs-lookup"><span data-stu-id="ac95f-104">Azure Functions Service Bus bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="ac95f-105">Ez a cikk azt ismerteti, hogyan tooconfigure és az Azure Functions Azure Service Bus kötések munkahelyi.</span><span class="sxs-lookup"><span data-stu-id="ac95f-105">This article explains how tooconfigure and work with Azure Service Bus bindings in Azure Functions.</span></span> 

<span data-ttu-id="ac95f-106">Az Azure Functions támogatja aktiválhatja és Service Bus-üzenetsorok és témakörök kimeneti.</span><span class="sxs-lookup"><span data-stu-id="ac95f-106">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a><span data-ttu-id="ac95f-107">A Service Bus eseményindító</span><span class="sxs-lookup"><span data-stu-id="ac95f-107">Service Bus trigger</span></span>
<span data-ttu-id="ac95f-108">Hello Service Bus eseményindító toorespond toomessages a Service Bus-üzenetsor vagy témakör használja.</span><span class="sxs-lookup"><span data-stu-id="ac95f-108">Use hello Service Bus trigger toorespond toomessages from a Service Bus queue or topic.</span></span> 

<span data-ttu-id="ac95f-109">hello Service Bus-üzenetsor és a témakör eseményindítók határozzák meg a következő hello a JSON-objektumok hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="ac95f-109">hello Service Bus queue and topic triggers are defined by hello following JSON objects in hello `bindings` array of function.json:</span></span>

* <span data-ttu-id="ac95f-110">*várólista* eseményindító:</span><span class="sxs-lookup"><span data-stu-id="ac95f-110">*queue* trigger:</span></span>

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of hello queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* <span data-ttu-id="ac95f-111">*a témakör* eseményindító:</span><span class="sxs-lookup"><span data-stu-id="ac95f-111">*topic* trigger:</span></span>

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of hello topic>",
        "subscriptionName" : "<Name of hello subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

<span data-ttu-id="ac95f-112">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="ac95f-112">Note hello following:</span></span>

* <span data-ttu-id="ac95f-113">A `connection`, [alkalmazásbeállítás létrehozása az függvény alkalmazásban](functions-how-to-use-azure-function-app-settings.md) tartalmazó hello kapcsolati karakterlánc tooyour Service Bus-névtér, majd meg kell adnia hello nevet hello Alkalmazásbeállítás hello `connection` az eseményindító tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="ac95f-113">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains hello connection string tooyour Service Bus namespace, then specify hello name of hello app setting in hello `connection` property in your trigger.</span></span> <span data-ttu-id="ac95f-114">Hello kapcsolati karakterlánc látható hello lépéseket követve szerezze be [hello felügyeleti hitelesítő adatok beszerzése](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="ac95f-114">You obtain hello connection string by following hello steps shown at [Obtain hello management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="ac95f-115">hello kapcsolati karakterláncnak kell lennie, a Service Bus névtér, korlátozás nélkül tooa adott üzenetsor vagy témakör.</span><span class="sxs-lookup"><span data-stu-id="ac95f-115">hello connection string must be for a Service Bus namespace, not limited tooa specific queue or topic.</span></span>
  <span data-ttu-id="ac95f-116">Ha nem adja meg `connection` üres, hello eseményindító azt feltételezi, hogy egy alapértelmezett Service Bus kapcsolati karakterlánc van megadva egy alkalmazás nevű beállításával `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="ac95f-116">If you leave `connection` empty, hello trigger assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="ac95f-117">A `accessRights`, elérhető értékek a következők `manage` és `listen`.</span><span class="sxs-lookup"><span data-stu-id="ac95f-117">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="ac95f-118">hello alapértelmezett érték a `manage`, ami azt jelenti, hogy hello `connection` hello rendelkezik **kezelése** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="ac95f-118">hello default is `manage`, which indicates that hello `connection` has hello **Manage** permission.</span></span> <span data-ttu-id="ac95f-119">Ha használja a kapcsolati karakterláncot, amely nem rendelkezik hello **kezelése** engedély, `accessRights` túl`listen`.</span><span class="sxs-lookup"><span data-stu-id="ac95f-119">If you use a connection string that does not have hello **Manage** permission, set `accessRights` too`listen`.</span></span> <span data-ttu-id="ac95f-120">Ellenkező esetben a futásidejű meghiúsulhat, miközben a rendszer toodo műveletet, amelyhez hello funkciók jogosultságaik kezelését.</span><span class="sxs-lookup"><span data-stu-id="ac95f-120">Otherwise, hello Functions runtime might fail trying toodo operations that require manage rights.</span></span>

## <a name="trigger-behavior"></a><span data-ttu-id="ac95f-121">Eseményindító viselkedése</span><span class="sxs-lookup"><span data-stu-id="ac95f-121">Trigger behavior</span></span>
* <span data-ttu-id="ac95f-122">**Single-threading** - alapértelmezés szerint hello funkciók futásidejű folyamatok több üzenetet párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="ac95f-122">**Single-threading** - By default, hello Functions runtime processes multiple messages concurrently.</span></span> <span data-ttu-id="ac95f-123">toodirect hello futásidejű tooprocess csak egyetlen üzenetsor vagy témakör üzenet egyszerre, állítsa be `serviceBus.maxConcurrentCalls` a too1 *host.json*.</span><span class="sxs-lookup"><span data-stu-id="ac95f-123">toodirect hello runtime tooprocess only a single queue or topic message at a time, set `serviceBus.maxConcurrentCalls` too1 in *host.json*.</span></span> 
  <span data-ttu-id="ac95f-124">További információ *host.json*, lásd: [mappaszerkezet](functions-reference.md#folder-structure) és [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="ac95f-124">For information about *host.json*, see [Folder Structure](functions-reference.md#folder-structure) and [host.json](https://github .com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>
* <span data-ttu-id="ac95f-125">**Elhalt üzenetek kezelésének** -Service Bus does saját elhalt üzenetek kezelésének, nem szabályozott, vagy az Azure Functions konfiguráció vagy a kód konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="ac95f-125">**Poison message handling** - Service Bus does its own poison message handling, which can't be controlled or configured in Azure Functions configuration or code.</span></span> 
* <span data-ttu-id="ac95f-126">**PeekLock viselkedés** -hello Functions futtatókörnyezete az üzenetet kap [ `PeekLock` mód](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) és hívások `Complete` üdvözlőüzenetére Ha hello függvény futtatása sikeresen befejeződött, vagy a hívások `Abandon` Ha hello parancs nem működik.</span><span class="sxs-lookup"><span data-stu-id="ac95f-126">**PeekLock behavior** - hello Functions runtime receives a message in [`PeekLock` mode](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) and calls `Complete` on hello message if hello function finishes successfully, or calls `Abandon` if hello function fails.</span></span> 
  <span data-ttu-id="ac95f-127">Ha hello függvény futásakor hello hosszabb `PeekLock` időkorlát, hello zárolása automatikusan megújítják.</span><span class="sxs-lookup"><span data-stu-id="ac95f-127">If hello function runs longer than hello `PeekLock` timeout, hello lock is automatically renewed.</span></span>

<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="ac95f-128">Eseményindító kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="ac95f-128">Trigger usage</span></span>
<span data-ttu-id="ac95f-129">Ez a szakasz bemutatja, hogyan toouse a Service Bus indul el, a függvény kódban.</span><span class="sxs-lookup"><span data-stu-id="ac95f-129">This section shows you how toouse your Service Bus trigger in your function code.</span></span> 

<span data-ttu-id="ac95f-130">C# és F #, a Service Bus eseményindító üdvözlőüzenetére lehet a következő bemeneti típusnak sem hello deszerializált tooany:</span><span class="sxs-lookup"><span data-stu-id="ac95f-130">In C# and F#, hello Service Bus trigger message can be deserialized tooany of hello following input types:</span></span>

* <span data-ttu-id="ac95f-131">`string`-hasznosak lehetnek a karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="ac95f-131">`string` - useful for string messages</span></span>
* <span data-ttu-id="ac95f-132">`byte[]`-hasznosak lehetnek a bináris adatok</span><span class="sxs-lookup"><span data-stu-id="ac95f-132">`byte[]` - useful for binary data</span></span>
* <span data-ttu-id="ac95f-133">Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) - hasznosak lehetnek a JSON-adatokból.</span><span class="sxs-lookup"><span data-stu-id="ac95f-133">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized data.</span></span>
  <span data-ttu-id="ac95f-134">Egy egyéni bemeneti típus, például a deklarálható `CustomType`, az Azure Functions megpróbál toodeserialize hello JSON-adatokat a megadott típus be.</span><span class="sxs-lookup"><span data-stu-id="ac95f-134">If you declare a custom input type, such as `CustomType`, Azure Functions tries toodeserialize hello JSON data into your specified type.</span></span>
* <span data-ttu-id="ac95f-135">`BrokeredMessage`-tesz lehetővé, hogy hello deszerializálni hello üzenet [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="ac95f-135">`BrokeredMessage` - gives you hello deserialized message with hello [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) method.</span></span>

<span data-ttu-id="ac95f-136">A node.js a Service Bus eseményindító üdvözlőüzenetére átadott hello függvény egy karakterlánc vagy a JSON-objektumból.</span><span class="sxs-lookup"><span data-stu-id="ac95f-136">In Node.js, hello Service Bus trigger message is passed into hello function as either a string or JSON object.</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="ac95f-137">Eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="ac95f-137">Trigger sample</span></span>
<span data-ttu-id="ac95f-138">Tegyük fel, a következő function.json hello:</span><span class="sxs-lookup"><span data-stu-id="ac95f-138">Suppose you have hello following function.json:</span></span>

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

<span data-ttu-id="ac95f-139">Lásd: hello nyelvspecifikus mintát, amely egy Service Bus üzenetsor-üzenetet feldolgozza.</span><span class="sxs-lookup"><span data-stu-id="ac95f-139">See hello language-specific sample that processes a Service Bus queue message.</span></span>

* [<span data-ttu-id="ac95f-140">C#</span><span class="sxs-lookup"><span data-stu-id="ac95f-140">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="ac95f-141">F#</span><span class="sxs-lookup"><span data-stu-id="ac95f-141">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="ac95f-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="ac95f-142">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="ac95f-143">A C# eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="ac95f-143">Trigger sample in C#</span></span> #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a><span data-ttu-id="ac95f-144">Az F # eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="ac95f-144">Trigger sample in F#</span></span> #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="ac95f-145">A node.js eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="ac95f-145">Trigger sample in Node.js</span></span>

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a><span data-ttu-id="ac95f-146">A Service Bus kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="ac95f-146">Service Bus output binding</span></span>
<span data-ttu-id="ac95f-147">a Service Bus várólista és ez a témakör kimeneti függvény hello használata hello hello a JSON-objektumok a következő `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="ac95f-147">hello Service Bus queue and topic output for a function use hello following JSON objects in hello `bindings` array of function.json:</span></span>

* <span data-ttu-id="ac95f-148">*várólista* kimenete:</span><span class="sxs-lookup"><span data-stu-id="ac95f-148">*queue* output:</span></span>

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of hello queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* <span data-ttu-id="ac95f-149">*a témakör* kimenete:</span><span class="sxs-lookup"><span data-stu-id="ac95f-149">*topic* output:</span></span>

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of hello topic>",
        "subscriptionName" : "<Name of hello subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for hello connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

<span data-ttu-id="ac95f-150">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="ac95f-150">Note hello following:</span></span>

* <span data-ttu-id="ac95f-151">A `connection`, [alkalmazásbeállítás létrehozása az függvény alkalmazásban](functions-how-to-use-azure-function-app-settings.md) tartalmazó hello kapcsolati karakterlánc tooyour Service Bus-névtér, majd meg kell adnia hello nevet hello Alkalmazásbeállítás hello `connection` szöveget a kimenetben tulajdonság kötelező.</span><span class="sxs-lookup"><span data-stu-id="ac95f-151">For `connection`, [create an app setting in your function app](functions-how-to-use-azure-function-app-settings.md) that contains hello connection string tooyour Service Bus namespace, then specify hello name of hello app setting in hello `connection` property in your output binding.</span></span> <span data-ttu-id="ac95f-152">Hello kapcsolati karakterlánc látható hello lépéseket követve szerezze be [hello felügyeleti hitelesítő adatok beszerzése](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span><span class="sxs-lookup"><span data-stu-id="ac95f-152">You obtain hello connection string by following hello steps shown at [Obtain hello management credentials](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).</span></span>
  <span data-ttu-id="ac95f-153">hello kapcsolati karakterláncnak kell lennie, a Service Bus névtér, korlátozás nélkül tooa adott üzenetsor vagy témakör.</span><span class="sxs-lookup"><span data-stu-id="ac95f-153">hello connection string must be for a Service Bus namespace, not limited tooa specific queue or topic.</span></span>
  <span data-ttu-id="ac95f-154">Ha nem adja meg `connection` üres, hello kimeneti kötés azt feltételezi, hogy egy alapértelmezett Service Bus kapcsolati karakterlánc van megadva egy alkalmazás nevű beállításával `AzureWebJobsServiceBus`.</span><span class="sxs-lookup"><span data-stu-id="ac95f-154">If you leave `connection` empty, hello output binding assumes that a default Service Bus connection string is specified in an app setting named `AzureWebJobsServiceBus`.</span></span>
* <span data-ttu-id="ac95f-155">A `accessRights`, elérhető értékek a következők `manage` és `listen`.</span><span class="sxs-lookup"><span data-stu-id="ac95f-155">For `accessRights`, available values are `manage` and `listen`.</span></span> <span data-ttu-id="ac95f-156">hello alapértelmezett érték a `manage`, ami azt jelenti, hogy hello `connection` hello rendelkezik **kezelése** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="ac95f-156">hello default is `manage`, which indicates that hello `connection` has hello **Manage** permission.</span></span> <span data-ttu-id="ac95f-157">Ha használja a kapcsolati karakterláncot, amely nem rendelkezik hello **kezelése** engedély, `accessRights` túl`listen`.</span><span class="sxs-lookup"><span data-stu-id="ac95f-157">If you use a connection string that does not have hello **Manage** permission, set `accessRights` too`listen`.</span></span> <span data-ttu-id="ac95f-158">Ellenkező esetben a futásidejű meghiúsulhat, miközben a rendszer toodo műveletet, amelyhez hello funkciók jogosultságaik kezelését.</span><span class="sxs-lookup"><span data-stu-id="ac95f-158">Otherwise, hello Functions runtime might fail trying toodo operations that require manage rights.</span></span>

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="ac95f-159">Kimeneti használata</span><span class="sxs-lookup"><span data-stu-id="ac95f-159">Output usage</span></span>
<span data-ttu-id="ac95f-160">A C# és F # az Azure Functions a következő típusok hello bármelyik hozhat létre egy Service Bus-üzenetsor:</span><span class="sxs-lookup"><span data-stu-id="ac95f-160">In C# and F#, Azure Functions can create a Service Bus queue message from any of hello following types:</span></span>

* <span data-ttu-id="ac95f-161">Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) -paraméterek definícióját a következőképpen néz `out T paramName` (C#).</span><span class="sxs-lookup"><span data-stu-id="ac95f-161">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - Parameter definition looks like `out T paramName` (C#).</span></span>
  <span data-ttu-id="ac95f-162">Funkciók hello objektum deserializes JSON üzenetbe.</span><span class="sxs-lookup"><span data-stu-id="ac95f-162">Functions deserializes hello object into a JSON message.</span></span> <span data-ttu-id="ac95f-163">Ha hello kimeneti értéket értéke null, ha hello függvény kilép, Funkciók üdvözlőüzenetére null objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ac95f-163">If hello output value is null when hello function exits, Functions creates hello message with a null object.</span></span>
* <span data-ttu-id="ac95f-164">`string`-Paraméterek definícióját a következőképpen néz `out string paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="ac95f-164">`string` - Parameter definition looks like `out string paraName` (C#).</span></span> <span data-ttu-id="ac95f-165">Ha hello paraméter értéke nem null értékű amikor hello függvény kilép, a funkciók létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="ac95f-165">If hello parameter value is non-null when hello function exits, Functions creates a message.</span></span>
* <span data-ttu-id="ac95f-166">`byte[]`-Paraméterek definícióját a következőképpen néz `out byte[] paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="ac95f-166">`byte[]` - Parameter definition looks like `out byte[] paraName` (C#).</span></span> <span data-ttu-id="ac95f-167">Ha hello paraméter értéke nem null értékű amikor hello függvény kilép, a funkciók létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="ac95f-167">If hello parameter value is non-null when hello function exits, Functions creates a message.</span></span>
* <span data-ttu-id="ac95f-168">`BrokeredMessage`Paraméterek definícióját a következőképpen néz `out BrokeredMessage paraName` (C#).</span><span class="sxs-lookup"><span data-stu-id="ac95f-168">`BrokeredMessage` Parameter definition looks like `out BrokeredMessage paraName` (C#).</span></span> <span data-ttu-id="ac95f-169">Ha hello paraméter értéke nem null értékű amikor hello függvény kilép, a funkciók létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="ac95f-169">If hello parameter value is non-null when hello function exits, Functions creates a message.</span></span>

<span data-ttu-id="ac95f-170">A több üzenet létrehozása a C# függvényben, használhat `ICollector<T>` vagy `IAsyncCollector<T>`.</span><span class="sxs-lookup"><span data-stu-id="ac95f-170">For creating multiple messages in a C# function, you can use `ICollector<T>` or `IAsyncCollector<T>`.</span></span> <span data-ttu-id="ac95f-171">Egy üzenet jön létre, ha meghívja a hello `Add` metódust.</span><span class="sxs-lookup"><span data-stu-id="ac95f-171">A message is created when you call hello `Add` method.</span></span>

<span data-ttu-id="ac95f-172">Node.js, rendelhet egy karakterlánc, egy bájttömböt vagy egy Javascript-objektum (deszerializálni a JSON-ba) túl`context.binding.<paramName>`.</span><span class="sxs-lookup"><span data-stu-id="ac95f-172">In Node.js, you can assign a string, a byte array, or a Javascript object (deserialized into JSON) too`context.binding.<paramName>`.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="ac95f-173">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="ac95f-173">Output sample</span></span>
<span data-ttu-id="ac95f-174">Tegyük fel, a következő function.json, amely meghatározza egy Service Bus várólista kimeneti hello:</span><span class="sxs-lookup"><span data-stu-id="ac95f-174">Suppose you have hello following function.json, that defines a Service Bus queue output:</span></span>

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

<span data-ttu-id="ac95f-175">Lásd: hello nyelvspecifikus minta által küldött üzenet toohello service bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="ac95f-175">See hello language-specific sample that sends a message toohello service bus queue.</span></span>

* [<span data-ttu-id="ac95f-176">C#</span><span class="sxs-lookup"><span data-stu-id="ac95f-176">C#</span></span>](#outcsharp)
* [<span data-ttu-id="ac95f-177">F#</span><span class="sxs-lookup"><span data-stu-id="ac95f-177">F#</span></span>](#outfsharp)
* [<span data-ttu-id="ac95f-178">Node.js</span><span class="sxs-lookup"><span data-stu-id="ac95f-178">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="ac95f-179">A C# kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="ac95f-179">Output sample in C#</span></span> #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

<span data-ttu-id="ac95f-180">Vagy toocreate több üzenetet:</span><span class="sxs-lookup"><span data-stu-id="ac95f-180">Or, toocreate multiple messages:</span></span>

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

### <a name="output-sample-in-f"></a><span data-ttu-id="ac95f-181">Az F # kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="ac95f-181">Output sample in F#</span></span> #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="ac95f-182">Kimeneti minta node.js</span><span class="sxs-lookup"><span data-stu-id="ac95f-182">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

<span data-ttu-id="ac95f-183">Vagy toocreate több üzenetet:</span><span class="sxs-lookup"><span data-stu-id="ac95f-183">Or, toocreate multiple messages:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ac95f-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac95f-184">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


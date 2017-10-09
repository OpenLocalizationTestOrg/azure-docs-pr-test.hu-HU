---
title: "aaaAzure funkciók várólista tárolási kötések |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Storage eseményindítók és kötések az Azure Functions."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: 4e6a837d-e64f-45a0-87b7-aa02688a75f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.openlocfilehash: 438b4f63e823149072c86fdefa7e15bfd2a2c4df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-queue-storage-bindings"></a><span data-ttu-id="8ad15-104">Az Azure Queue Storage funkciók kötések</span><span class="sxs-lookup"><span data-stu-id="8ad15-104">Azure Functions Queue Storage bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="8ad15-105">Ez a cikk ismerteti, hogyan tooconfigure és kód Azure Queue storage kötések Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8ad15-105">This article describes how tooconfigure and code Azure Queue storage bindings in Azure Functions.</span></span> <span data-ttu-id="8ad15-106">Az Azure Functions támogatja aktiválhatja, és kimeneti Azure várólisták kötései.</span><span class="sxs-lookup"><span data-stu-id="8ad15-106">Azure Functions supports trigger and output bindings for Azure queues.</span></span> <span data-ttu-id="8ad15-107">Elérhető összes kötését lásd [Azure Functions eseményindítók és kötések fogalmak](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="8ad15-107">For features that are available in all bindings, see [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a><span data-ttu-id="8ad15-108">Várólista tárolási eseményindító</span><span class="sxs-lookup"><span data-stu-id="8ad15-108">Queue storage trigger</span></span>
<span data-ttu-id="8ad15-109">hello Azure Queue storage eseményindító lehetővé teszi, hogy a várólista tárolási funkciókat biztosító új üzenetek toomonitor, és reagálni toothem.</span><span class="sxs-lookup"><span data-stu-id="8ad15-109">hello Azure Queue storage trigger enables you toomonitor a queue storage for new messages and react toothem.</span></span> 

<span data-ttu-id="8ad15-110">Adja meg egy várólista eseményindító hello segítségével **integráció** hello Functions portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="8ad15-110">Define a queue trigger using hello **Integrate** tab in hello Functions portal.</span></span> <span data-ttu-id="8ad15-111">hello portál létrehoz egyet a következő hello definíciójában hello **kötések** szakasza *function.json*:</span><span class="sxs-lookup"><span data-stu-id="8ad15-111">hello portal creates hello following definition in hello  **bindings** section of *function.json*:</span></span>

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "queueName": "<Name of queue toopoll>",
    "connection":"<Name of app setting - see below>"
}
```

* <span data-ttu-id="8ad15-112">Hello `connection` tulajdonságának tartalmaznia kell egy tárolási kapcsolati karakterlánc tartalmazó Alkalmazásbeállítás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="8ad15-112">hello `connection` property must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="8ad15-113">Hello Azure-portálon, a hello hello a szokásos szerkesztő **integráció** lapon konfigurálja az Alkalmazásbeállítás meg, amikor kiválaszt egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="8ad15-113">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

<span data-ttu-id="8ad15-114">További beállításokat lehet megadni a [host.json fájl](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) toofurther tárolási eseményindítók finomhangolásához.</span><span class="sxs-lookup"><span data-stu-id="8ad15-114">Additional settings can be provided in a [host.json file](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) toofurther fine-tune queue storage triggers.</span></span> <span data-ttu-id="8ad15-115">Módosíthatja például host.json hello várólista lekérdezési időközét.</span><span class="sxs-lookup"><span data-stu-id="8ad15-115">For example, you can change hello queue polling interval in host.json.</span></span>

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a><span data-ttu-id="8ad15-116">A várólista eseményindító használatával</span><span class="sxs-lookup"><span data-stu-id="8ad15-116">Using a queue trigger</span></span>
<span data-ttu-id="8ad15-117">A Node.js funkciók, lépjen a hello várólista használatával végzett `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="8ad15-117">In Node.js functions, access hello queue data using `context.bindings.<name>`.</span></span>


<span data-ttu-id="8ad15-118">A .NET-es függvényeket, lépjen a hello várólista hasznos, mint egy metódus paraméter használatával `CloudQueueMessage paramName`.</span><span class="sxs-lookup"><span data-stu-id="8ad15-118">In .NET functions, access hello queue payload using a method parameter such as `CloudQueueMessage paramName`.</span></span> <span data-ttu-id="8ad15-119">Itt `paramName` hello megadott hello érték [eseményindító konfigurációs](#trigger).</span><span class="sxs-lookup"><span data-stu-id="8ad15-119">Here, `paramName` is hello value you specified in hello [trigger configuration](#trigger).</span></span> <span data-ttu-id="8ad15-120">várólista üdvözlőüzenetére lehet a következő típusok hello deszerializált tooany:</span><span class="sxs-lookup"><span data-stu-id="8ad15-120">hello queue message can be deserialized tooany of hello following types:</span></span>

* <span data-ttu-id="8ad15-121">POCO objektum.</span><span class="sxs-lookup"><span data-stu-id="8ad15-121">POCO object.</span></span> <span data-ttu-id="8ad15-122">Ha hello várólista hasznos egy JSON-objektum használatát.</span><span class="sxs-lookup"><span data-stu-id="8ad15-122">Use if hello queue payload is a JSON object.</span></span> <span data-ttu-id="8ad15-123">hello Functions futtatókörnyezete deserializes hello hasznos hello POCO objektumba.</span><span class="sxs-lookup"><span data-stu-id="8ad15-123">hello Functions runtime deserializes hello payload into hello POCO object.</span></span> 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a><span data-ttu-id="8ad15-124">Várólista eseményindító metaadatok</span><span class="sxs-lookup"><span data-stu-id="8ad15-124">Queue trigger metadata</span></span>
<span data-ttu-id="8ad15-125">hello várólista eseményindító biztosít több metaadat-tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="8ad15-125">hello queue trigger provides several metadata properties.</span></span> <span data-ttu-id="8ad15-126">Ezeket a tulajdonságokat meg más kötésekben kötési kifejezés részeként vagy a kód paramétereiben használható.</span><span class="sxs-lookup"><span data-stu-id="8ad15-126">These properties can be used as part of binding expressions in other bindings or as parameters in your code.</span></span> <span data-ttu-id="8ad15-127">hello értékek rendelkezik hello azonos szemantikákkal, [ `CloudQueueMessage` ].</span><span class="sxs-lookup"><span data-stu-id="8ad15-127">hello values have hello same semantics as [`CloudQueueMessage`].</span></span>

* <span data-ttu-id="8ad15-128">**QueueTrigger** -várólista forgalma (Ha egy érvényes karakterláncot)</span><span class="sxs-lookup"><span data-stu-id="8ad15-128">**QueueTrigger** - queue payload (if a valid string)</span></span>
* <span data-ttu-id="8ad15-129">**DequeueCount** -típus `int`.</span><span class="sxs-lookup"><span data-stu-id="8ad15-129">**DequeueCount** - Type `int`.</span></span> <span data-ttu-id="8ad15-130">Ez az üzenet várólistából nincs kivéve. hányszor hello.</span><span class="sxs-lookup"><span data-stu-id="8ad15-130">hello number of times this message has been dequeued.</span></span>
* <span data-ttu-id="8ad15-131">**ExpirationTime** -típus `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="8ad15-131">**ExpirationTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="8ad15-132">hello ideje, hogy üdvözlőüzenetére lejár.</span><span class="sxs-lookup"><span data-stu-id="8ad15-132">hello time that hello message expires.</span></span>
* <span data-ttu-id="8ad15-133">**Azonosító** -típus `string`.</span><span class="sxs-lookup"><span data-stu-id="8ad15-133">**Id** - Type `string`.</span></span> <span data-ttu-id="8ad15-134">Várólista azonosító.</span><span class="sxs-lookup"><span data-stu-id="8ad15-134">Queue message ID.</span></span>
* <span data-ttu-id="8ad15-135">**InsertionTime** -típus `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="8ad15-135">**InsertionTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="8ad15-136">hello ideje, hogy üdvözlőüzenetére toohello várólista lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="8ad15-136">hello time that hello message was added toohello queue.</span></span>
* <span data-ttu-id="8ad15-137">**NextVisibleTime** -típus `DateTimeOffset?`.</span><span class="sxs-lookup"><span data-stu-id="8ad15-137">**NextVisibleTime** - Type `DateTimeOffset?`.</span></span> <span data-ttu-id="8ad15-138">hello ideje, hogy üdvözlőüzenetére mellett látható lesz.</span><span class="sxs-lookup"><span data-stu-id="8ad15-138">hello time that hello message will next be visible.</span></span>
* <span data-ttu-id="8ad15-139">**PopReceipt** -típus `string`.</span><span class="sxs-lookup"><span data-stu-id="8ad15-139">**PopReceipt** - Type `string`.</span></span> <span data-ttu-id="8ad15-140">hello üzenet pop fogadását.</span><span class="sxs-lookup"><span data-stu-id="8ad15-140">hello message's pop receipt.</span></span>

<span data-ttu-id="8ad15-141">Tekintse meg, hogyan toouse hello várólista metaadatok [eseményindító minta](#triggersample).</span><span class="sxs-lookup"><span data-stu-id="8ad15-141">See how toouse hello queue metadata in [Trigger sample](#triggersample).</span></span>

<a name="triggersample"></a>

## <a name="trigger-sample"></a><span data-ttu-id="8ad15-142">Eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="8ad15-142">Trigger sample</span></span>
<span data-ttu-id="8ad15-143">Tegyük fel, amely meghatározza egy várólista eseményindító function.json következő hello:</span><span class="sxs-lookup"><span data-stu-id="8ad15-143">Suppose you have hello following function.json that defines a queue trigger:</span></span>

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionString"
        }
    ]
}
```

<span data-ttu-id="8ad15-144">Lásd: hello nyelvspecifikus mintát, amely lekéri és várólista metaadatok naplózza.</span><span class="sxs-lookup"><span data-stu-id="8ad15-144">See hello language-specific sample that retrieves and logs queue metadata.</span></span>

* [<span data-ttu-id="8ad15-145">C#</span><span class="sxs-lookup"><span data-stu-id="8ad15-145">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="8ad15-146">Node.js</span><span class="sxs-lookup"><span data-stu-id="8ad15-146">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="8ad15-147">A C# eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="8ad15-147">Trigger sample in C#</span></span> #
```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger sample in F# ## 
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="8ad15-148">A node.js eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="8ad15-148">Trigger sample in Node.js</span></span>

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id=', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

### <a name="handling-poison-queue-messages"></a><span data-ttu-id="8ad15-149">Az elhalt üzenetek kezelése</span><span class="sxs-lookup"><span data-stu-id="8ad15-149">Handling poison queue messages</span></span>
<span data-ttu-id="8ad15-150">A várólista funkció meghibásodása esetén az Azure Functions újrapróbálkozik a függvény az egy adott várólistában üzenetnek hello első alkalommal próbálja toofive be.</span><span class="sxs-lookup"><span data-stu-id="8ad15-150">When a queue trigger function fails, Azure Functions retries that function up toofive times for a given queue message, including hello first try.</span></span> <span data-ttu-id="8ad15-151">Minden öt kísérlet sikertelen, ha hello functions futtatókörnyezete hozzáadja egy üzenet tooa a queue storage nevű  *&lt;originalqueuename >-poison*.</span><span class="sxs-lookup"><span data-stu-id="8ad15-151">If all five attempts fail, hello functions runtime adds a message tooa queue storage named *&lt;originalqueuename>-poison*.</span></span> <span data-ttu-id="8ad15-152">Írhat egy függvény tooprocess üzenetek hello poison várólistából naplózásukhoz vagy, hogy szükség van-e a kézi beavatkozást értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="8ad15-152">You can write a function tooprocess messages from hello poison queue by logging them or sending a  notification that manual attention is needed.</span></span> 

<span data-ttu-id="8ad15-153">az elhalt üzenetek toohandle manuálisan, ellenőrizze a hello `dequeueCount` hello várólista üzenet (lásd: [várólista eseményindító metaadatok](#meta)).</span><span class="sxs-lookup"><span data-stu-id="8ad15-153">toohandle poison messages manually, check hello `dequeueCount` of hello queue message (see [Queue trigger metadata](#meta)).</span></span>

<a name="output"></a>

## <a name="queue-storage-output-binding"></a><span data-ttu-id="8ad15-154">A Queue storage kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="8ad15-154">Queue storage output binding</span></span>
<span data-ttu-id="8ad15-155">a kimeneti hello Azure a queue storage kötés lehetővé teszi a toowrite üzenetek tooa várólista.</span><span class="sxs-lookup"><span data-stu-id="8ad15-155">hello Azure queue storage output binding enables you toowrite messages tooa queue.</span></span> 

<span data-ttu-id="8ad15-156">Adja meg a várólista kimeneti kötést hello **integráció** hello Functions portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="8ad15-156">Define a queue output binding using hello **Integrate** tab in hello Functions portal.</span></span> <span data-ttu-id="8ad15-157">hello portál létrehoz egyet a következő hello definíciójában hello **kötések** szakasza *function.json*:</span><span class="sxs-lookup"><span data-stu-id="8ad15-157">hello portal creates hello following definition in hello  **bindings** section of *function.json*:</span></span>

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<hello name used tooidentify hello trigger data in your code>",
   "queueName": "<Name of queue toowrite to>",
   "connection":"<Name of app setting - see below>"
}
```

* <span data-ttu-id="8ad15-158">Hello `connection` tulajdonságának tartalmaznia kell egy tárolási kapcsolati karakterlánc tartalmazó Alkalmazásbeállítás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="8ad15-158">hello `connection` property must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="8ad15-159">Hello Azure-portálon, a hello hello a szokásos szerkesztő **integráció** lapon konfigurálja az Alkalmazásbeállítás meg, amikor kiválaszt egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="8ad15-159">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a><span data-ttu-id="8ad15-160">Kimeneti várólista alapján kötése</span><span class="sxs-lookup"><span data-stu-id="8ad15-160">Using a queue output binding</span></span>
<span data-ttu-id="8ad15-161">A Node.js funkciók segítségével érhető el hello kimeneti várólista `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="8ad15-161">In Node.js functions, you access hello output queue using `context.bindings.<name>`.</span></span>

<span data-ttu-id="8ad15-162">A .NET-es függvényeket a következő típusok hello tooany készíthető.</span><span class="sxs-lookup"><span data-stu-id="8ad15-162">In .NET functions, you can output tooany of hello following types.</span></span> <span data-ttu-id="8ad15-163">A típusparaméter esetén `T`, `T` kell típusok egyike lehet hello támogatott kimeneti, például a `string` vagy egy POCO.</span><span class="sxs-lookup"><span data-stu-id="8ad15-163">When there is a type parameter `T`, `T` must be one of hello supported output types, such as `string` or a POCO.</span></span>

* <span data-ttu-id="8ad15-164">`out T`(JSON-ként szerializált)</span><span class="sxs-lookup"><span data-stu-id="8ad15-164">`out T` (serialized as JSON)</span></span>
* `out string`
* `out byte[]`
* <span data-ttu-id="8ad15-165">`out` [`CloudQueueMessage`]</span><span class="sxs-lookup"><span data-stu-id="8ad15-165">`out` [`CloudQueueMessage`]</span></span> 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

<span data-ttu-id="8ad15-166">Használhatja a hello metódus visszatérési típusa, hello kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="8ad15-166">You can also use hello method return type as hello output binding.</span></span>

<a name="outputsample"></a>

## <a name="queue-output-sample"></a><span data-ttu-id="8ad15-167">Várólista kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="8ad15-167">Queue output sample</span></span>
<span data-ttu-id="8ad15-168">hello következő *function.json* definiál egy HTTP-eseményindítóval üzenetsorokat kimeneti kötése:</span><span class="sxs-lookup"><span data-stu-id="8ad15-168">hello following *function.json* defines an HTTP trigger with a queue output binding:</span></span>

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionString",
    }
  ]
}
``` 

<span data-ttu-id="8ad15-169">Lásd: hello nyelvspecifikus mintát, amely egy üzenetsor-üzenetet a hello bejövő HTTP-tartalom.</span><span class="sxs-lookup"><span data-stu-id="8ad15-169">See hello language-specific sample that outputs a queue message with hello incoming HTTP payload.</span></span>

* [<span data-ttu-id="8ad15-170">C#</span><span class="sxs-lookup"><span data-stu-id="8ad15-170">C#</span></span>](#outcsharp)
* [<span data-ttu-id="8ad15-171">Node.js</span><span class="sxs-lookup"><span data-stu-id="8ad15-171">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a><span data-ttu-id="8ad15-172">Várólista kimeneti minta C#</span><span class="sxs-lookup"><span data-stu-id="8ad15-172">Queue output sample in C#</span></span> #

```cs
// C# example of HTTP trigger binding tooa custom POCO, with a queue output binding
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

<span data-ttu-id="8ad15-173">toosend több üzenetet, használjon egy `ICollector`:</span><span class="sxs-lookup"><span data-stu-id="8ad15-173">toosend multiple messages, use an `ICollector`:</span></span>

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a><span data-ttu-id="8ad15-174">Kimeneti várólista minta Node.js</span><span class="sxs-lookup"><span data-stu-id="8ad15-174">Queue output sample in Node.js</span></span>

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

<span data-ttu-id="8ad15-175">Vagy toosend több üzenetet</span><span class="sxs-lookup"><span data-stu-id="8ad15-175">Or, toosend multiple messages,</span></span>

```javascript
module.exports = function(context) {
    // Define a message array for hello myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="8ad15-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ad15-176">Next steps</span></span>

<span data-ttu-id="8ad15-177">Egy függvény által használt tárolási eseményindítók és kötések példáért lásd: [hozzon létre egy Azure-függvény csatlakoztatott tooan Azure szolgáltatás](functions-create-an-azure-connected-function.md).</span><span class="sxs-lookup"><span data-stu-id="8ad15-177">For an example of a function that uses queue storage triggers and bindings, see [Create an Azure Function connected tooan Azure service](functions-create-an-azure-connected-function.md).</span></span>

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

["CloudQueueMessage"]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage

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
# <a name="azure-functions-queue-storage-bindings"></a>Az Azure Queue Storage funkciók kötések
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogyan tooconfigure és kód Azure Queue storage kötések Azure Functions. Az Azure Functions támogatja aktiválhatja, és kimeneti Azure várólisták kötései. Elérhető összes kötését lásd [Azure Functions eseményindítók és kötések fogalmak](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a>Várólista tárolási eseményindító
hello Azure Queue storage eseményindító lehetővé teszi, hogy a várólista tárolási funkciókat biztosító új üzenetek toomonitor, és reagálni toothem. 

Adja meg egy várólista eseményindító hello segítségével **integráció** hello Functions portálra a lap. hello portál létrehoz egyet a következő hello definíciójában hello **kötések** szakasza *function.json*:

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "queueName": "<Name of queue toopoll>",
    "connection":"<Name of app setting - see below>"
}
```

* Hello `connection` tulajdonságának tartalmaznia kell egy tárolási kapcsolati karakterlánc tartalmazó Alkalmazásbeállítás hello nevét. Hello Azure-portálon, a hello hello a szokásos szerkesztő **integráció** lapon konfigurálja az Alkalmazásbeállítás meg, amikor kiválaszt egy tárfiókot.

További beállításokat lehet megadni a [host.json fájl](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) toofurther tárolási eseményindítók finomhangolásához. Módosíthatja például host.json hello várólista lekérdezési időközét.

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a>A várólista eseményindító használatával
A Node.js funkciók, lépjen a hello várólista használatával végzett `context.bindings.<name>`.


A .NET-es függvényeket, lépjen a hello várólista hasznos, mint egy metódus paraméter használatával `CloudQueueMessage paramName`. Itt `paramName` hello megadott hello érték [eseményindító konfigurációs](#trigger). várólista üdvözlőüzenetére lehet a következő típusok hello deszerializált tooany:

* POCO objektum. Ha hello várólista hasznos egy JSON-objektum használatát. hello Functions futtatókörnyezete deserializes hello hasznos hello POCO objektumba. 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a>Várólista eseményindító metaadatok
hello várólista eseményindító biztosít több metaadat-tulajdonságot. Ezeket a tulajdonságokat meg más kötésekben kötési kifejezés részeként vagy a kód paramétereiben használható. hello értékek rendelkezik hello azonos szemantikákkal, [ `CloudQueueMessage` ].

* **QueueTrigger** -várólista forgalma (Ha egy érvényes karakterláncot)
* **DequeueCount** -típus `int`. Ez az üzenet várólistából nincs kivéve. hányszor hello.
* **ExpirationTime** -típus `DateTimeOffset?`. hello ideje, hogy üdvözlőüzenetére lejár.
* **Azonosító** -típus `string`. Várólista azonosító.
* **InsertionTime** -típus `DateTimeOffset?`. hello ideje, hogy üdvözlőüzenetére toohello várólista lett hozzáadva.
* **NextVisibleTime** -típus `DateTimeOffset?`. hello ideje, hogy üdvözlőüzenetére mellett látható lesz.
* **PopReceipt** -típus `string`. hello üzenet pop fogadását.

Tekintse meg, hogyan toouse hello várólista metaadatok [eseményindító minta](#triggersample).

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Eseményindító minta
Tegyük fel, amely meghatározza egy várólista eseményindító function.json következő hello:

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

Lásd: hello nyelvspecifikus mintát, amely lekéri és várólista metaadatok naplózza.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>A C# eseményindító minta #
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

### <a name="trigger-sample-in-nodejs"></a>A node.js eseményindító minta

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

### <a name="handling-poison-queue-messages"></a>Az elhalt üzenetek kezelése
A várólista funkció meghibásodása esetén az Azure Functions újrapróbálkozik a függvény az egy adott várólistában üzenetnek hello első alkalommal próbálja toofive be. Minden öt kísérlet sikertelen, ha hello functions futtatókörnyezete hozzáadja egy üzenet tooa a queue storage nevű  *&lt;originalqueuename >-poison*. Írhat egy függvény tooprocess üzenetek hello poison várólistából naplózásukhoz vagy, hogy szükség van-e a kézi beavatkozást értesítést küld. 

az elhalt üzenetek toohandle manuálisan, ellenőrizze a hello `dequeueCount` hello várólista üzenet (lásd: [várólista eseményindító metaadatok](#meta)).

<a name="output"></a>

## <a name="queue-storage-output-binding"></a>A Queue storage kimeneti kötése
a kimeneti hello Azure a queue storage kötés lehetővé teszi a toowrite üzenetek tooa várólista. 

Adja meg a várólista kimeneti kötést hello **integráció** hello Functions portálra a lap. hello portál létrehoz egyet a következő hello definíciójában hello **kötések** szakasza *function.json*:

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<hello name used tooidentify hello trigger data in your code>",
   "queueName": "<Name of queue toowrite to>",
   "connection":"<Name of app setting - see below>"
}
```

* Hello `connection` tulajdonságának tartalmaznia kell egy tárolási kapcsolati karakterlánc tartalmazó Alkalmazásbeállítás hello nevét. Hello Azure-portálon, a hello hello a szokásos szerkesztő **integráció** lapon konfigurálja az Alkalmazásbeállítás meg, amikor kiválaszt egy tárfiókot.

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a>Kimeneti várólista alapján kötése
A Node.js funkciók segítségével érhető el hello kimeneti várólista `context.bindings.<name>`.

A .NET-es függvényeket a következő típusok hello tooany készíthető. A típusparaméter esetén `T`, `T` kell típusok egyike lehet hello támogatott kimeneti, például a `string` vagy egy POCO.

* `out T`(JSON-ként szerializált)
* `out string`
* `out byte[]`
* `out` [`CloudQueueMessage`] 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Használhatja a hello metódus visszatérési típusa, hello kimeneti kötése.

<a name="outputsample"></a>

## <a name="queue-output-sample"></a>Várólista kimeneti minta
hello következő *function.json* definiál egy HTTP-eseményindítóval üzenetsorokat kimeneti kötése:

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

Lásd: hello nyelvspecifikus mintát, amely egy üzenetsor-üzenetet a hello bejövő HTTP-tartalom.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a>Várólista kimeneti minta C# #

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

toosend több üzenetet, használjon egy `ICollector`:

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a>Kimeneti várólista minta Node.js

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Vagy toosend több üzenetet

```javascript
module.exports = function(context) {
    // Define a message array for hello myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a>Következő lépések

Egy függvény által használt tárolási eseményindítók és kötések példáért lásd: [hozzon létre egy Azure-függvény csatlakoztatott tooan Azure szolgáltatás](functions-create-an-azure-connected-function.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

["CloudQueueMessage"]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage

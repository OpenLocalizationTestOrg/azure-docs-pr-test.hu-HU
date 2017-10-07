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
# <a name="azure-functions-service-bus-bindings"></a>Az Azure Functions a Service Bus kötések
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk azt ismerteti, hogyan tooconfigure és az Azure Functions Azure Service Bus kötések munkahelyi. 

Az Azure Functions támogatja aktiválhatja és Service Bus-üzenetsorok és témakörök kimeneti.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a>A Service Bus eseményindító
Hello Service Bus eseményindító toorespond toomessages a Service Bus-üzenetsor vagy témakör használja. 

hello Service Bus-üzenetsor és a témakör eseményindítók határozzák meg a következő hello a JSON-objektumok hello `bindings` function.json tömbje:

* *várólista* eseményindító:

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

* *a témakör* eseményindító:

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

Vegye figyelembe a következőket hello:

* A `connection`, [alkalmazásbeállítás létrehozása az függvény alkalmazásban](functions-how-to-use-azure-function-app-settings.md) tartalmazó hello kapcsolati karakterlánc tooyour Service Bus-névtér, majd meg kell adnia hello nevet hello Alkalmazásbeállítás hello `connection` az eseményindító tulajdonságait. Hello kapcsolati karakterlánc látható hello lépéseket követve szerezze be [hello felügyeleti hitelesítő adatok beszerzése](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  hello kapcsolati karakterláncnak kell lennie, a Service Bus névtér, korlátozás nélkül tooa adott üzenetsor vagy témakör.
  Ha nem adja meg `connection` üres, hello eseményindító azt feltételezi, hogy egy alapértelmezett Service Bus kapcsolati karakterlánc van megadva egy alkalmazás nevű beállításával `AzureWebJobsServiceBus`.
* A `accessRights`, elérhető értékek a következők `manage` és `listen`. hello alapértelmezett érték a `manage`, ami azt jelenti, hogy hello `connection` hello rendelkezik **kezelése** engedéllyel. Ha használja a kapcsolati karakterláncot, amely nem rendelkezik hello **kezelése** engedély, `accessRights` túl`listen`. Ellenkező esetben a futásidejű meghiúsulhat, miközben a rendszer toodo műveletet, amelyhez hello funkciók jogosultságaik kezelését.

## <a name="trigger-behavior"></a>Eseményindító viselkedése
* **Single-threading** - alapértelmezés szerint hello funkciók futásidejű folyamatok több üzenetet párhuzamosan. toodirect hello futásidejű tooprocess csak egyetlen üzenetsor vagy témakör üzenet egyszerre, állítsa be `serviceBus.maxConcurrentCalls` a too1 *host.json*. 
  További információ *host.json*, lásd: [mappaszerkezet](functions-reference.md#folder-structure) és [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).
* **Elhalt üzenetek kezelésének** -Service Bus does saját elhalt üzenetek kezelésének, nem szabályozott, vagy az Azure Functions konfiguráció vagy a kód konfigurálva. 
* **PeekLock viselkedés** -hello Functions futtatókörnyezete az üzenetet kap [ `PeekLock` mód](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) és hívások `Complete` üdvözlőüzenetére Ha hello függvény futtatása sikeresen befejeződött, vagy a hívások `Abandon` Ha hello parancs nem működik. 
  Ha hello függvény futásakor hello hosszabb `PeekLock` időkorlát, hello zárolása automatikusan megújítják.

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Eseményindító kihasználtsága
Ez a szakasz bemutatja, hogyan toouse a Service Bus indul el, a függvény kódban. 

C# és F #, a Service Bus eseményindító üdvözlőüzenetére lehet a következő bemeneti típusnak sem hello deszerializált tooany:

* `string`-hasznosak lehetnek a karakterlánc-üzenetek
* `byte[]`-hasznosak lehetnek a bináris adatok
* Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) - hasznosak lehetnek a JSON-adatokból.
  Egy egyéni bemeneti típus, például a deklarálható `CustomType`, az Azure Functions megpróbál toodeserialize hello JSON-adatokat a megadott típus be.
* `BrokeredMessage`-tesz lehetővé, hogy hello deszerializálni hello üzenet [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metódust.

A node.js a Service Bus eseményindító üdvözlőüzenetére átadott hello függvény egy karakterlánc vagy a JSON-objektumból.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Eseményindító minta
Tegyük fel, a következő function.json hello:

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

Lásd: hello nyelvspecifikus mintát, amely egy Service Bus üzenetsor-üzenetet feldolgozza.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>A C# eseményindító minta #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Az F # eseményindító minta #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>A node.js eseményindító minta

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a>A Service Bus kimeneti kötése
a Service Bus várólista és ez a témakör kimeneti függvény hello használata hello hello a JSON-objektumok a következő `bindings` function.json tömbje:

* *várólista* kimenete:

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
* *a témakör* kimenete:

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

Vegye figyelembe a következőket hello:

* A `connection`, [alkalmazásbeállítás létrehozása az függvény alkalmazásban](functions-how-to-use-azure-function-app-settings.md) tartalmazó hello kapcsolati karakterlánc tooyour Service Bus-névtér, majd meg kell adnia hello nevet hello Alkalmazásbeállítás hello `connection` szöveget a kimenetben tulajdonság kötelező. Hello kapcsolati karakterlánc látható hello lépéseket követve szerezze be [hello felügyeleti hitelesítő adatok beszerzése](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  hello kapcsolati karakterláncnak kell lennie, a Service Bus névtér, korlátozás nélkül tooa adott üzenetsor vagy témakör.
  Ha nem adja meg `connection` üres, hello kimeneti kötés azt feltételezi, hogy egy alapértelmezett Service Bus kapcsolati karakterlánc van megadva egy alkalmazás nevű beállításával `AzureWebJobsServiceBus`.
* A `accessRights`, elérhető értékek a következők `manage` és `listen`. hello alapértelmezett érték a `manage`, ami azt jelenti, hogy hello `connection` hello rendelkezik **kezelése** engedéllyel. Ha használja a kapcsolati karakterláncot, amely nem rendelkezik hello **kezelése** engedély, `accessRights` túl`listen`. Ellenkező esetben a futásidejű meghiúsulhat, miközben a rendszer toodo műveletet, amelyhez hello funkciók jogosultságaik kezelését.

<a name="outputusage"></a>

## <a name="output-usage"></a>Kimeneti használata
A C# és F # az Azure Functions a következő típusok hello bármelyik hozhat létre egy Service Bus-üzenetsor:

* Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) -paraméterek definícióját a következőképpen néz `out T paramName` (C#).
  Funkciók hello objektum deserializes JSON üzenetbe. Ha hello kimeneti értéket értéke null, ha hello függvény kilép, Funkciók üdvözlőüzenetére null objektumot hoz létre.
* `string`-Paraméterek definícióját a következőképpen néz `out string paraName` (C#). Ha hello paraméter értéke nem null értékű amikor hello függvény kilép, a funkciók létrehoz egy üzenetet.
* `byte[]`-Paraméterek definícióját a következőképpen néz `out byte[] paraName` (C#). Ha hello paraméter értéke nem null értékű amikor hello függvény kilép, a funkciók létrehoz egy üzenetet.
* `BrokeredMessage`Paraméterek definícióját a következőképpen néz `out BrokeredMessage paraName` (C#). Ha hello paraméter értéke nem null értékű amikor hello függvény kilép, a funkciók létrehoz egy üzenetet.

A több üzenet létrehozása a C# függvényben, használhat `ICollector<T>` vagy `IAsyncCollector<T>`. Egy üzenet jön létre, ha meghívja a hello `Add` metódust.

Node.js, rendelhet egy karakterlánc, egy bájttömböt vagy egy Javascript-objektum (deszerializálni a JSON-ba) túl`context.binding.<paramName>`.

<a name="outputsample"></a>

## <a name="output-sample"></a>Minta kimenet
Tegyük fel, a következő function.json, amely meghatározza egy Service Bus várólista kimeneti hello:

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

Lásd: hello nyelvspecifikus minta által küldött üzenet toohello service bus-üzenetsorba.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>A C# kimeneti minta #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Vagy toocreate több üzenetet:

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

### <a name="output-sample-in-f"></a>Az F # kimeneti minta #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Kimeneti minta node.js

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Vagy toocreate több üzenetet:

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

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


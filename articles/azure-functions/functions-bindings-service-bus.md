---
title: "Az Azure Functions az Azure Service Bus kötései"
description: "Azure Service Bus-eseményindítók és kötések az Azure Functions használatának megismerése."
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
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
ms.author: tdykstra
ms.openlocfilehash: 2df003d47291570b31e1091f34994e4023000981
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/02/2018
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Az Azure Functions az Azure Service Bus kötései

Ez a cikk ismerteti az Azure Functions kötések Azure Service Bus használata. Az Azure Functions támogatja aktiválhatja és Service Bus-üzenetsorok és témakörök kimeneti.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Eseményindító

A Service Bus eseményindító használatával a Service Bus-üzenetsor vagy témakör üzenetek válaszolni. 

## <a name="trigger---example"></a>Eseményindító – példa

Tekintse meg a nyelvspecifikus példát:

* [C#](#trigger---c-example)
* [C# parancsfájl (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Eseményindító - C# – példa

Az alábbi példa mutatja egy [C# függvény](functions-dotnet-class-library.md) , amely egy Service Bus-üzenetsor naplózza.

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] 
    string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

### <a name="trigger---c-script-example"></a>Eseményindító - C# parancsfájl – példa

A következő példa bemutatja egy Service Bus eseményindító kötelező egy *function.json* fájlt és egy [C# parancsfájl függvény](functions-reference-csharp.md) , amely a kötés használja. A függvény egy Service Bus-üzenetsor naplózza.

Itt az kötés adatai a *function.json* fájlt:

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

A C# parancsfájl kód itt látható:

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

### <a name="trigger---f-example"></a>Eseményindító - F # – példa

A következő példa bemutatja egy Service Bus eseményindító kötelező egy *function.json* fájlt és egy [F # függvény](functions-reference-fsharp.md) , amely a kötés használja. A függvény egy Service Bus-üzenetsor naplózza. 

Itt az kötés adatai a *function.json* fájlt:

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

A F # parancsfájl kód itt látható:

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

### <a name="trigger---javascript-example"></a>Eseményindító - JavaScript – példa

A következő példa bemutatja egy Service Bus eseményindító kötelező egy *function.json* fájlt és egy [JavaScript függvény](functions-reference-node.md) , amely a kötés használja. A függvény egy Service Bus-üzenetsor naplózza. 

Itt az kötés adatai a *function.json* fájlt:

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

A JavaScript parancsfájl kód itt látható:

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

## <a name="trigger---attributes"></a>Eseményindító - attribútumok

A [C# osztálykönyvtárakhoz](functions-dotnet-class-library.md), ha egy Service Bus eseményindítót a következő attribútumokat használhatja:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusTriggerAttribute.cs)NuGet-csomagot a definiált [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus)

  Az attribútum konstruktora a várólista vagy a témakör és előfizetés neve vesz igénybe. A kapcsolat hozzáférési jogosultságokat is megadható. Ha nem ad meg hozzáférési jogosultságokat, az alapértelmezett érték `Manage`. A hozzáférési jogok beállítás kiválasztása esetén, tekintse meg a a [eseményindító - konfigurációs](#trigger---configuration) szakasz. Íme egy példa, amely megjeleníti a karakterlánc-paraméterrel használt attribútum:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

  Beállíthatja a `Connection` tulajdonság fiókot szeretne megadni a Service Bus használatához a következő példában látható módon:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

  Tekintse meg a teljes például [eseményindító - C# példa](#trigger---c-example).

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)NuGet-csomagot a definiált [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus)

  Adja meg a Service Bus-fiókot használni más megoldást kínál. A konstruktornak, amely egy Service Bus kapcsolati karakterláncot tartalmaz alkalmazásbeállítás neve vesz igénybe. Az attribútum a paraméter, módszer vagy osztály szintjén is alkalmazható. A következő példa bemutatja az osztály és módszer:

  ```csharp
  [ServiceBusAccount("ClassLevelServiceBusAppSetting")]
  public static class AzureFunctions
  {
      [ServiceBusAccount("MethodLevelServiceBusAppSetting")]
      [FunctionName("ServiceBusQueueTriggerCSharp")]
      public static void Run(
          [ServiceBusTrigger("myqueue", AccessRights.Manage)] 
          string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

A Service Bus használandó fiókot határozza meg a következő sorrendben:

* A `ServiceBusTrigger` attribútum `Connection` tulajdonság.
* A `ServiceBusAccount` attribútuma ugyanezt a paramétert, mint a `ServiceBusTrigger` attribútum.
* A `ServiceBusAccount` függvény attribútuma.
* A `ServiceBusAccount` osztály attribútuma.
* Az "AzureWebJobsServiceBus" alkalmazásbeállítás.

## <a name="trigger---configuration"></a>Eseményindító - konfiguráció

Az alábbi táblázat ismerteti a beállított kötés konfigurációs tulajdonságok a *function.json* fájl és a `ServiceBusTrigger` attribútum.

|Function.JSON tulajdonság | Attribútum tulajdonsága |Leírás|
|---------|---------|----------------------|
|**típusa** | n/a | "ServiceBusTrigger" értékre kell állítani. Ez a tulajdonság rendszer automatikusan beállítja az eseményindítót hoz létre az Azure portálon.|
|**iránya** | n/a | "A" értékre kell állítani. Ez a tulajdonság rendszer automatikusan beállítja az eseményindítót hoz létre az Azure portálon. |
|**név** | n/a | Az üzenetsor vagy témakör üzenet funkciókódot jelölő neve. "$Return" értékre való hivatkozáshoz függvény visszatérési értéke. | 
|**queueName**|**QueueName**|A várólista figyelése neve.  Csak akkor, ha a várólista nem a témakör a figyelés beállítása.
|**topicName**|**TopicName**|A témakör a figyelheti neve. Állítsa be, csak akkor, ha a témakör a várólista nem figyelni.|
|**subscriptionName**|**SubscriptionName**|Az előfizetés figyelése neve. Állítsa be, csak akkor, ha a témakör a várólista nem figyelni.|
|**kapcsolat**|**Kapcsolat**|A Service Bus kapcsolati karakterlánc az ehhez a kötéshez használandó tartalmazó alkalmazásbeállítás neve. Ha az alkalmazás neve "AzureWebJobs" kezdődik, megadhatja a név csak a maradékot. Ha például `connection` "MyServiceBus", hogy a Functions futtatókörnyezete keresi, hogy az alkalmazás neve "AzureWebJobsMyServiceBus." Ha nem adja meg `connection` üres, a Functions futtatókörnyezete használja az alapértelmezett Service Bus kapcsolati karakterlánc a "AzureWebJobsServiceBus" nevű Alkalmazásbeállítás.<br><br>Szerezzen be egy kapcsolati karakterláncot, kövesse a lépéseket látható [a felügyeleti hitelesítő adatok beszerzése](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). A kapcsolati karakterláncnak kell lennie, a Service Bus-névtér, nem kizárólagosan az adott üzenetsor vagy témakör. |
|**accessRights**|**Access (Hozzáférés)**|Hozzáférési jogosultsága a kapcsolódási karakterláncban. Lehetséges értékek a következők `manage` és `listen`. Az alapértelmezett érték `manage`, amely azt jelzi, hogy a `connection` rendelkezik a **kezelése** engedéllyel. Ha használja a kapcsolati karakterláncot, amely nem rendelkezik a **kezelése** engedély, `accessRights` "figyelésére". Ellenkező esetben a futásidejű meghiúsulhat igénylő műveletek végrehajtását megkísérlő funkciók jogosultságaik kezelését.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Eseményindító - használat

C# és C# a parancsfájlt, nyissa meg az üzenetsor vagy témakör üzenet metódusparaméter használatával `string paramName`. A C# parancsfájl `paramName` érték szerepel a `name` tulajdonsága *function.json*. A következő típusú helyett használhatja `string`:

* `byte[]`-Bináris adatoknál lehet hasznos.
* Egyéni típusa: Ha az üzenet tartalmaz JSON, az Azure Functions próbálja meg a JSON-adatok deszerializálása.
* `BrokeredMessage`-Lehetővé teszi a deszerializált üzenetet a [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metódust.

A JavaScript, hozzáférhetnek az üzenetsor vagy témakör üzenet használatával `context.bindings.<name>`. `<name>`az érték szerepel a `name` tulajdonsága *function.json*. A Service Bus message átad egy karakterlánc- vagy JSON-objektum a funkciót a.

## <a name="trigger---poison-messages"></a>Eseményindító - elhalt üzenetek

Elhalt üzenetek kezelésének nem lehet konfigurálni az Azure Functions vagy alatt áll. A Service Bus kezeli az elhalt üzenetek magát.

## <a name="trigger---peeklock-behavior"></a>Eseményindító - PeekLock viselkedése

A Functions futtatókörnyezete az üzenetet kap [PeekLock mód](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode). Meghívja `Complete` az üzenetet, ha a függvény futtatása sikeresen befejeződött, vagy a hívások `Abandon` Ha a parancs nem működik. Ha a függvény futásakor hosszabb, mint a `PeekLock` automatikusan megújítják időtúllépés, a zárolás.

## <a name="trigger---hostjson-properties"></a>Eseményindító - host.json tulajdonságai

A [host.json](functions-host-json.md#servicebus) fájl Service Bus eseményindító viselkedését vezérlő beállításokat tartalmaz.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-service-bus.md)]

## <a name="output"></a>Kimenet

Azure Service Bus kimeneti kötés segítségével üzenetsor vagy témakör üzeneteket küldeni.

## <a name="output---example"></a>Kimeneti – példa

Tekintse meg a nyelvspecifikus példát:

* [C#](#output---c-example)
* [C# parancsfájl (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Kimeneti - C# – példa

Az alábbi példa mutatja egy [C# függvény](functions-dotnet-class-library.md) , küld egy Service Bus-üzenetsor:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```

### <a name="output---c-script-example"></a>Kimeneti - C# parancsfájl – példa

A következő példa bemutatja egy Service Bus kimeneti kötelező egy *function.json* fájlt és egy [C# parancsfájl függvény](functions-reference-csharp.md) , amely a kötés használja. A függvény egy időzítő indítófeltételt használó 15 másodpercenként várólista üzenet küldése.

Itt az kötés adatai a *function.json* fájlt:

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

C# parancsfájlkód, amely létrehoz egy üzenetet a következő:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Az alábbiakban C# parancsfájl létrehozó kód mellől több üzenetet:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Kimeneti - F # – példa

A következő példa bemutatja egy Service Bus kimeneti kötelező egy *function.json* fájlt és egy [F # parancsfájl függvény](functions-reference-fsharp.md) , amely a kötés használja. A függvény egy időzítő indítófeltételt használó 15 másodpercenként várólista üzenet küldése.

Itt az kötés adatai a *function.json* fájlt:

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

Itt az F # parancsfájlkód, amely létrehoz egy üzenetet:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

### <a name="output---javascript-example"></a>Kimeneti - JavaScript – példa

A következő példa bemutatja egy Service Bus kimeneti kötelező egy *function.json* fájlt és egy [JavaScript függvény](functions-reference-node.md) , amely a kötés használja. A függvény egy időzítő indítófeltételt használó 15 másodpercenként várólista üzenet küldése.

Itt az kötés adatai a *function.json* fájlt:

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

Létrehoz egy üzenetet JavaScript parancsfájlkódot itt található:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

JavaScript parancsfájl létrehozó kód mellől több üzenet a következő:

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

## <a name="output---attributes"></a>Kimeneti - attribútumok

A [C# osztálykönyvtárakhoz](functions-dotnet-class-library.md), használja a [ServiceBusAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), amely van megadva a NuGet-csomag [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

Az attribútum konstruktora a várólista vagy a témakör és előfizetés neve vesz igénybe. A kapcsolat hozzáférési jogosultságokat is megadható. A hozzáférési jogok beállítás kiválasztása esetén, tekintse meg a a [kimeneti - konfigurációs](#output---configuration) szakasz. Íme egy példa bemutatja, a függvény visszatérési értéke a attribútuma:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, TraceWriter log)
{
    ...
}
```

Beállíthatja a `Connection` tulajdonság fiókot szeretne megadni a Service Bus használatához a következő példában látható módon:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, TraceWriter log)
{
    ...
}
```

Tekintse meg a teljes például [kimeneti - C# példa](#output---c-example).

Használhatja a `ServiceBusAccount` attribútum segítségével adhatja meg a Service Bus osztály, módszer vagy paraméter szinten használandó fiókot.  További információkért lásd: [eseményindító - attribútumok](#trigger---attributes).

## <a name="output---configuration"></a>Kimeneti - konfiguráció

Az alábbi táblázat ismerteti a beállított kötés konfigurációs tulajdonságok a *function.json* fájl és a `ServiceBus` attribútum.

|Function.JSON tulajdonság | Attribútum tulajdonsága |Leírás|
|---------|---------|----------------------|
|**típusa** | n/a | "A Szolgáltatásbusz" értékre kell állítani. Ez a tulajdonság rendszer automatikusan beállítja az eseményindítót hoz létre az Azure portálon.|
|**iránya** | n/a | "Ki" értékre kell állítani. Ez a tulajdonság rendszer automatikusan beállítja az eseményindítót hoz létre az Azure portálon. |
|**név** | n/a | A várólista vagy funkciókódot témakörében jelölő neve. "$Return" értékre való hivatkozáshoz függvény visszatérési értéke. | 
|**queueName**|**QueueName**|A várólista nevét.  Állítsa be, csak akkor, ha üzenetküldésre várólista, a témakör a nem.
|**topicName**|**TopicName**|A témakör a figyelheti neve. Csak akkor, ha üzenetküldésre témakör, a várólista nem beállítása.|
|**subscriptionName**|**SubscriptionName**|Az előfizetés figyelése neve. Csak akkor, ha üzenetküldésre témakör, a várólista nem beállítása.|
|**kapcsolat**|**Kapcsolat**|A Service Bus kapcsolati karakterlánc az ehhez a kötéshez használandó tartalmazó alkalmazásbeállítás neve. Ha az alkalmazás neve "AzureWebJobs" kezdődik, megadhatja a név csak a maradékot. Ha például `connection` "MyServiceBus", hogy a Functions futtatókörnyezete keresi, hogy az alkalmazás neve "AzureWebJobsMyServiceBus." Ha nem adja meg `connection` üres, a Functions futtatókörnyezete használja az alapértelmezett Service Bus kapcsolati karakterlánc a "AzureWebJobsServiceBus" nevű Alkalmazásbeállítás.<br><br>Szerezzen be egy kapcsolati karakterláncot, kövesse a lépéseket látható [a felügyeleti hitelesítő adatok beszerzése](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). A kapcsolati karakterláncnak kell lennie, a Service Bus-névtér, nem kizárólagosan az adott üzenetsor vagy témakör.|
|**accessRights**|**Access (Hozzáférés)** |Hozzáférési jogosultsága a kapcsolódási karakterláncban. Lehetséges értékek a következők: "kezelése" és "figyelő". Az alapértelmezett érték a "kezelése", ami azt jelenti, hogy rendelkezik-e a kapcsolat **kezelése** engedélyek. Ha használja a kapcsolati karakterláncot, amely nem rendelkezik **kezelése** engedélyek beállítása `accessRights` "figyelésére". Ellenkező esetben a futásidejű meghiúsulhat igénylő műveletek végrehajtását megkísérlő funkciók jogosultságaik kezelését.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Kimeneti - használat

C# és C# a parancsfájlt, nyissa meg az üzenetsor vagy témakör metódusparaméter használatával `out string paramName`. A C# parancsfájl `paramName` érték szerepel a `name` tulajdonsága *function.json*. A következő típusú paraméter használhatja:

* `out T paramName` - `T`bármely JSON-szerializálható típusú lehet. Ha a paraméter értéke nem null, ha kilép, a függvény, funkciók az üzenet null objektumot hoz létre.
* `out string`-Ha a paraméter értéke nem null, amikor a függvény kilép, a funkciók nem hoz létre egy üzenetet.
* `out byte[]`-Ha a paraméter értéke nem null, amikor a függvény kilép, a funkciók nem hoz létre egy üzenetet.
* `out BrokeredMessage`-Ha a paraméter értéke nem null, amikor a függvény kilép, a funkciók nem hoz létre egy üzenetet.

A több üzenet létrehozása a C# vagy C# parancsfájl függvény, használhat `ICollector<T>` vagy `IAsyncCollector<T>`. Egy üzenet jön létre, ha meghívja a `Add` metódust.

JavaScript, nyissa meg az üzenetsor vagy témakör segítségével `context.bindings.<name>`. `<name>`az érték szerepel a `name` tulajdonsága *function.json*. Rendelhet egy karakterlánc, egy bájttömböt vagy egy Javascript-objektum (deszerializálni a JSON-ba) `context.binding.<name>`.

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [További tudnivalók az Azure functions eseményindítók és kötések](functions-triggers-bindings.md)

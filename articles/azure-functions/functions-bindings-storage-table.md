---
title: "aaaAzure funkciók tárolási tábla kötések |} Microsoft Docs"
description: "Megértéséhez hogyan toouse Azure Storage kötések Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: 65b3437e-2571-4d3f-a996-61a74b50a1c2
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/28/2016
ms.author: chrande
ms.openlocfilehash: 90c2a73329139d4ab3504bc0e2c90370133158bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-storage-table-bindings"></a>Az Azure Functions tárolási tábla kötések
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogyan tooconfigure és az Azure Storage kód tábla az Azure Functions kötések. Azure Functions támogatja bemeneti és kimeneti Azure Storage-táblákat kötéseit.

hello tárolási tábla kötés támogatja-e a következő forgatókönyvek hello:

* **Egy C# vagy Node.js függvény egyetlen sor olvasása** - beállított `partitionKey` és `rowKey`. Hello `filter` és `take` tulajdonságok nem szerepel ebben a forgatókönyvben.
* **Olvassa el a C# függvényben több sort** -hello Functions futtatókörnyezete biztosít egy `IQueryable<T>` objektumhoz kötött toohello tábla. Típus `T` kell származnia `TableEntity` vagy megvalósítása `ITableEntity`. Hello `partitionKey`, `rowKey`, `filter`, és `take` tulajdonságok nem szerepel ebben a forgatókönyvben; használhatja a hello `IQueryable` objektum toodo szükséges szűrés. 
* **Egy csomópont függvény több sor olvasása** - beállított hello `filter` és `take` tulajdonságok. Nincs beállítva `partitionKey` vagy `rowKey`.
* **Egy vagy több sor írása C# függvények** -hello Functions futtatókörnyezete biztosít egy `ICollector<T>` vagy `IAsyncCollector<T>` kötött toohello tábla, ahol `T` hello séma meghatározza hello entitások tooadd szeretné. Általában, írja be a `T` származik `TableEntity` vagy megvalósítja `ITableEntity`, de nem kell. Hello `partitionKey`, `rowKey`, `filter`, és `take` tulajdonságok nem szerepel ebben a forgatókönyvben.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a>Tárolási tábla bemeneti kötése
hello Azure Storage bemeneti táblakötéssel lehetővé teszi a függvény egy tárolási tábla toouse. 

hello tárolási tábla bemeneti tooa függvény használja a következő hello a JSON-objektumok hello `bindings` function.json tömbje:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "in",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity tooread - see below>",
    "rowKey": "<RowKey of table entity tooread - see below>",
    "take": "<Maximum number of entities tooread in Node.js - optional>",
    "filter": "<OData filter expression for table input in Node.js - optional>",
    "connection": "<Name of app setting - see below>",
}
```

Vegye figyelembe a következőket hello: 

* Használjon `partitionKey` és `rowKey` együtt tooread egyetlen entitás. Ezek a tulajdonságok egyike sem kötelező. 
* `connection`egy tárolási kapcsolati karakterlánc tartalmazó Alkalmazásbeállítás hello nevét kell tartalmaznia. Hello Azure-portálon, a hello hello a szokásos szerkesztő **integráció** lapon konfigurálja az Alkalmazásbeállítás hoz létre, a tárolási fiók vagy választja ki egy meglévőt. Emellett [konfigurálása az alkalmazás manuális beállításával](functions-how-to-use-azure-function-app-settings.md#settings).  

<a name="inputusage"></a>

## <a name="input-usage"></a>Bemeneti kihasználtsága
A C# függvények, akkor kötni toohello bemeneti tábla entitás (vagy entitások) használatával egy elnevezett paraméter a függvényaláíráshoz, például `<T> <name>`.
Ha `T` adattípus hello megjeleníteni kívánt toodeserialize hello adatokat, és `paramName` hello megadott hello név [kötés bemeneti](#input). A Node.js funkciókat érheti el hello bemeneti tábla entitás (vagy entitások) használatával `context.bindings.<name>`.

Node.js vagy C# funkciók is deszerializálható hello bemeneti adatok. hello deszerializálni objektumok `RowKey` és `PartitionKey` tulajdonságok.

A C# funkciók is köthető a következő típusok hello tooany, és futásidejű megpróbál hello funkciók túl deszerializálni hello adatait, hogy a típus használatával:

* Magában foglaló típussal`ITableEntity`
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a>A minta bemeneti
Kellett volna lennie a következő function.json, használja a várólista eseményindító tooread egy-egy sorának hello rendelkezik. hello JSON megadja `PartitionKey`  
 `RowKey`. `"rowKey": "{queueTrigger}"`azt jelzi, hogy hello sor kulcs hello várólista üzenet karakterlánc származik.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Lásd: hello nyelvspecifikus minta a táblázat egyetlen entitás olvasó.

* [C#](#inputcsharp)
* [F#](#inputfsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>A C# bemeneti minta #
```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

<a name="inputfsharp"></a>

### <a name="input-sample-in-f"></a>Az F # bemeneti minta #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>A node.js bemeneti minta
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a>Tárolási tábla kimeneti kötése
a kimeneti hello Azure Storage tábla kötés lehetővé teszi a toowrite entitások tooa tárolási tábla a függvényben. 

hello tárolási táblázatos kimenete egy függvény hello hello a JSON-objektumok a következő célokra `bindings` function.json tömbje:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "out",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity toowrite - see below>",
    "rowKey": "<RowKey of table entity toowrite - see below>",
    "connection": "<Name of app setting - see below>",
}
```

Vegye figyelembe a következőket hello: 

* Használjon `partitionKey` és `rowKey` együtt toowrite egyetlen entitás. Ezek a tulajdonságok egyike sem kötelező. Azt is megadhatja, `PartitionKey` és `RowKey` létrehozásakor hello entitásobjektumok gyűjteményeit a függvény kódban.
* `connection`egy tárolási kapcsolati karakterlánc tartalmazó Alkalmazásbeállítás hello nevét kell tartalmaznia. Hello Azure-portálon, a hello hello a szokásos szerkesztő **integráció** lapon konfigurálja az Alkalmazásbeállítás hoz létre, a tárolási fiók vagy választja ki egy meglévőt. Emellett [konfigurálása az alkalmazás manuális beállításával](functions-how-to-use-azure-function-app-settings.md#settings). 

<a name="outputusage"></a>

## <a name="output-usage"></a>Kimeneti használata
A C# függvények, akkor eszközben toohello táblázatos kimenete nevű hello `out` a függvényaláíráshoz a paraméter, például `out <T> <name>`, ahol `T` adattípus hello megjeleníteni kívánt tooserialize hello adatokat, és `paramName` van hello nevét, hello megadott [kimeneti kötése](#output). A Node.js funkciókat érheti el hello tábla használatával kimeneti `context.bindings.<name>`.

Node.js vagy C# funkciók objektumokat is szerializálni. A C# funkciók is köthető a következő típusok toohello:

* Magában foglaló típussal`ITableEntity`
* `ICollector<T>`(toooutput több entitás. Lásd: [minta](#outcsharp).)
* `IAsyncCollector<T>`(aszinkron verzióját `ICollector<T>`)
* `CloudTable`(hello Azure Storage szolgáltatás SDK használatával. Lásd: [minta](#readmulti).)

<a name="outputsample"></a>

## <a name="output-sample"></a>Minta kimenet
hello következő *function.json* és *run.csx* példa azt mutatja meg hogyan toowrite több tábla entitás.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Lásd: hello nyelvspecifikus mintát, amely több tábla entitás hoz létre.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>A C# kimeneti minta #
```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```
<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Az F # kimeneti minta #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 too10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Kimeneti minta node.js
```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

<a name="readmulti"></a>

## <a name="sample-read-multiple-table-entities-in-c"></a>Minta: Olvassa el a C# több tábla entitás  #
hello következő *function.json* és C# Kódpélda beolvassa várólista üdvözlőüzenetére megadott partíciókulcsot az entitásokat.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

hello C#-kódban, hogy hello entitás típusa is származik ad hozzá egy hivatkozást toohello Azure Storage szolgáltatás SDK `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


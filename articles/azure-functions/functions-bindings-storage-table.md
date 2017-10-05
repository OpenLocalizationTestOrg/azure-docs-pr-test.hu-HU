---
title: "Az Azure tárolási táblában funkciók kötések |} Microsoft Docs"
description: "Azure Storage kötések az Azure Functions használatának megismerése."
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
ms.openlocfilehash: bb01be3ee044f60376e0c9c2de7b3dd34f3b7aca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-storage-table-bindings"></a><span data-ttu-id="3f07a-104">Az Azure Functions tárolási tábla kötések</span><span class="sxs-lookup"><span data-stu-id="3f07a-104">Azure Functions Storage table bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="3f07a-105">Ez a cikk azt ismerteti, konfigurálása és kód Azure Storage tábla kötések az Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="3f07a-105">This article explains how to configure and code Azure Storage table bindings in Azure Functions.</span></span> <span data-ttu-id="3f07a-106">Azure Functions támogatja bemeneti és kimeneti Azure Storage-táblákat kötéseit.</span><span class="sxs-lookup"><span data-stu-id="3f07a-106">Azure Functions supports input and output bindings for Azure Storage tables.</span></span>

<span data-ttu-id="3f07a-107">A tárolási táblakötéssel a következő szituációkat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="3f07a-107">The Storage table binding supports the following scenarios:</span></span>

* <span data-ttu-id="3f07a-108">**Egy C# vagy Node.js függvény egyetlen sor olvasása** - beállított `partitionKey` és `rowKey`.</span><span class="sxs-lookup"><span data-stu-id="3f07a-108">**Read a single row in a C# or Node.js function** - Set `partitionKey` and `rowKey`.</span></span> <span data-ttu-id="3f07a-109">A `filter` és `take` tulajdonságok nem szerepel ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="3f07a-109">The `filter` and `take` properties are not used in this scenario.</span></span>
* <span data-ttu-id="3f07a-110">**Olvassa el a C# függvényben több sort** – a Functions futtatókörnyezete biztosít egy `IQueryable<T>` objektum kötve a tábla.</span><span class="sxs-lookup"><span data-stu-id="3f07a-110">**Read multiple rows in a C# function** - The Functions runtime provides an `IQueryable<T>` object bound to the table.</span></span> <span data-ttu-id="3f07a-111">Típus `T` kell származnia `TableEntity` vagy megvalósítása `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="3f07a-111">Type `T` must derive from `TableEntity` or implement `ITableEntity`.</span></span> <span data-ttu-id="3f07a-112">A `partitionKey`, `rowKey`, `filter`, és `take` tulajdonságok nem szerepel ebben a forgatókönyvben; használhatja a `IQueryable` elvégzéséhez szükséges szűrés objektum.</span><span class="sxs-lookup"><span data-stu-id="3f07a-112">The `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario; you can use the `IQueryable` object to do any filtering required.</span></span> 
* <span data-ttu-id="3f07a-113">**Egy csomópont függvény több sor olvasása** – állítsa be a `filter` és `take` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="3f07a-113">**Read multiple rows in a Node function** - Set the `filter` and `take` properties.</span></span> <span data-ttu-id="3f07a-114">Nincs beállítva `partitionKey` vagy `rowKey`.</span><span class="sxs-lookup"><span data-stu-id="3f07a-114">Don't set `partitionKey` or `rowKey`.</span></span>
* <span data-ttu-id="3f07a-115">**Egy vagy több sor írása C# függvények** -a Functions futtatókörnyezete biztosít egy `ICollector<T>` vagy `IAsyncCollector<T>` kötve a tábla, ahol `T` határozza meg a hozzáadni kívánt entitásokat sémája.</span><span class="sxs-lookup"><span data-stu-id="3f07a-115">**Write one or more rows in a C# function** - The Functions runtime provides an `ICollector<T>` or `IAsyncCollector<T>` bound to the table, where `T` specifies the schema of the entities you want to add.</span></span> <span data-ttu-id="3f07a-116">Általában, írja be a `T` származik `TableEntity` vagy megvalósítja `ITableEntity`, de nem kell.</span><span class="sxs-lookup"><span data-stu-id="3f07a-116">Typically, type `T` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to.</span></span> <span data-ttu-id="3f07a-117">A `partitionKey`, `rowKey`, `filter`, és `take` tulajdonságok nem szerepel ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="3f07a-117">The `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a><span data-ttu-id="3f07a-118">Tárolási tábla bemeneti kötése</span><span class="sxs-lookup"><span data-stu-id="3f07a-118">Storage table input binding</span></span>
<span data-ttu-id="3f07a-119">Az Azure Storage bemeneti táblakötéssel lehetővé teszi a tárolási tábla használatát a függvényben.</span><span class="sxs-lookup"><span data-stu-id="3f07a-119">The Azure Storage table input binding enables you to use a storage table in your function.</span></span> 

<span data-ttu-id="3f07a-120">A tárolási tábla bemenete egy olyan függvényt használja a következő JSON-objektumok a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="3f07a-120">The Storage table input to a function uses the following JSON objects in the `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "in",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to read - see below>",
    "rowKey": "<RowKey of table entity to read - see below>",
    "take": "<Maximum number of entities to read in Node.js - optional>",
    "filter": "<OData filter expression for table input in Node.js - optional>",
    "connection": "<Name of app setting - see below>",
}
```

<span data-ttu-id="3f07a-121">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="3f07a-121">Note the following:</span></span> 

* <span data-ttu-id="3f07a-122">Használjon `partitionKey` és `rowKey` egyetlen entitás elolvasására együtt.</span><span class="sxs-lookup"><span data-stu-id="3f07a-122">Use `partitionKey` and `rowKey` together to read a single entity.</span></span> <span data-ttu-id="3f07a-123">Ezek a tulajdonságok egyike sem kötelező.</span><span class="sxs-lookup"><span data-stu-id="3f07a-123">These properties are optional.</span></span> 
* <span data-ttu-id="3f07a-124">`connection`egy alkalmazás-beállítás, amely tartalmazza a tárolási kapcsolati karakterlánc nevét kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="3f07a-124">`connection` must contain the name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="3f07a-125">Az Azure portálon, a szokásos szerkesztő a **integráció** lapon konfigurálja az Alkalmazásbeállítás hoz létre, a tárolási fiók vagy választja ki egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="3f07a-125">In the Azure portal, the standard editor in the **Integrate** tab configures this app setting for you when you create a Storage account or selects an existing one.</span></span> <span data-ttu-id="3f07a-126">Emellett [konfigurálása az alkalmazás manuális beállításával](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="3f07a-126">You can also [configure this app setting manually](functions-how-to-use-azure-function-app-settings.md#settings).</span></span>  

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="3f07a-127">Bemeneti kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="3f07a-127">Input usage</span></span>
<span data-ttu-id="3f07a-128">A C# függvények, akkor eszközben csatlakozzon a bemeneti tábla entitás (vagy entitások) egy elnevezett paraméter a függvényaláíráshoz a például `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="3f07a-128">In C# functions, you bind to the input table entity (or entities) by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="3f07a-129">Ha `T` , az adatokat, írja be, hogy szeretné-e deszerializálni az adatokat, és `paramName` a megadott név a [kötés bemeneti](#input).</span><span class="sxs-lookup"><span data-stu-id="3f07a-129">Where `T` is the data type that you want to deserialize the data into, and `paramName` is the name you specified in the [input binding](#input).</span></span> <span data-ttu-id="3f07a-130">A Node.js funkciókat érheti el a bemeneti tábla entitás (vagy entitások) használatával `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="3f07a-130">In Node.js functions, you access the input table entity (or entities) using `context.bindings.<name>`.</span></span>

<span data-ttu-id="3f07a-131">A bemeneti adatok Node.js vagy C# funkciók is deszerializálható.</span><span class="sxs-lookup"><span data-stu-id="3f07a-131">The input data can be deserialized in Node.js or C# functions.</span></span> <span data-ttu-id="3f07a-132">A deszerializált objektum rendelkezik `RowKey` és `PartitionKey` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="3f07a-132">The deserialized objects have `RowKey` and `PartitionKey` properties.</span></span>

<span data-ttu-id="3f07a-133">A C# funkciók is köthető a következő típusok, és a Functions futtatókörnyezete megkísérli deszerializálni a tábla adatait, hogy a típus használatával:</span><span class="sxs-lookup"><span data-stu-id="3f07a-133">In C# functions, you can also bind to any of the following types, and the Functions runtime will attempt to deserialize the table data using that type:</span></span>

* <span data-ttu-id="3f07a-134">Magában foglaló típussal`ITableEntity`</span><span class="sxs-lookup"><span data-stu-id="3f07a-134">Any type that implements `ITableEntity`</span></span>
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="3f07a-135">A minta bemeneti</span><span class="sxs-lookup"><span data-stu-id="3f07a-135">Input sample</span></span>
<span data-ttu-id="3f07a-136">Kellene, hogy rendelkezik-e a következő function.json, amely várólista eseményindítót használ egy-egy sorának olvasása.</span><span class="sxs-lookup"><span data-stu-id="3f07a-136">Supposed you have the following function.json, which uses a queue trigger to read a single table row.</span></span> <span data-ttu-id="3f07a-137">Megadja a JSON `PartitionKey`  
 `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="3f07a-137">The JSON specifies `PartitionKey` 
`RowKey`.</span></span> <span data-ttu-id="3f07a-138">`"rowKey": "{queueTrigger}"`azt jelzi, hogy a sorkulcs származik-e a várólista üzenet karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="3f07a-138">`"rowKey": "{queueTrigger}"` indicates that the row key comes from the queue message string.</span></span>

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

<span data-ttu-id="3f07a-139">Tekintse meg a nyelvspecifikus minta a táblázat egyetlen entitás olvasó.</span><span class="sxs-lookup"><span data-stu-id="3f07a-139">See the language-specific sample that reads a single table entity.</span></span>

* [<span data-ttu-id="3f07a-140">C#</span><span class="sxs-lookup"><span data-stu-id="3f07a-140">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="3f07a-141">F#</span><span class="sxs-lookup"><span data-stu-id="3f07a-141">F#</span></span>](#inputfsharp)
* [<span data-ttu-id="3f07a-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="3f07a-142">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="3f07a-143">A C# bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="3f07a-143">Input sample in C#</span></span> #
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

### <a name="input-sample-in-f"></a><span data-ttu-id="3f07a-144">Az F # bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="3f07a-144">Input sample in F#</span></span> #
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

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="3f07a-145">A node.js bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="3f07a-145">Input sample in Node.js</span></span>
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a><span data-ttu-id="3f07a-146">Tárolási tábla kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="3f07a-146">Storage table output binding</span></span>
<span data-ttu-id="3f07a-147">Az Azure Storage táblázatos kimenete kötés lehetővé teszi, hogy entitások írását tárolási tábla a függvényben.</span><span class="sxs-lookup"><span data-stu-id="3f07a-147">The Azure Storage table output binding enables you to write entities to a Storage table in your function.</span></span> 

<span data-ttu-id="3f07a-148">A kimeneti a függvényt használja a következő JSON-objektumok tárolási tábla a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="3f07a-148">The Storage table output for a function uses the following JSON objects in the `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "out",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to write - see below>",
    "rowKey": "<RowKey of table entity to write - see below>",
    "connection": "<Name of app setting - see below>",
}
```

<span data-ttu-id="3f07a-149">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="3f07a-149">Note the following:</span></span> 

* <span data-ttu-id="3f07a-150">Használjon `partitionKey` és `rowKey` együtt egyetlen entitás írni.</span><span class="sxs-lookup"><span data-stu-id="3f07a-150">Use `partitionKey` and `rowKey` together to write a single entity.</span></span> <span data-ttu-id="3f07a-151">Ezek a tulajdonságok egyike sem kötelező.</span><span class="sxs-lookup"><span data-stu-id="3f07a-151">These properties are optional.</span></span> <span data-ttu-id="3f07a-152">Azt is megadhatja, `PartitionKey` és `RowKey` létrehozásakor az entitásobjektumok gyűjteményeit a függvény kódban.</span><span class="sxs-lookup"><span data-stu-id="3f07a-152">You can also specify `PartitionKey` and `RowKey` when you create the entity objects in your function code.</span></span>
* <span data-ttu-id="3f07a-153">`connection`egy alkalmazás-beállítás, amely tartalmazza a tárolási kapcsolati karakterlánc nevét kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="3f07a-153">`connection` must contain the name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="3f07a-154">Az Azure portálon, a szokásos szerkesztő a **integráció** lapon konfigurálja az Alkalmazásbeállítás hoz létre, a tárolási fiók vagy választja ki egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="3f07a-154">In the Azure portal, the standard editor in the **Integrate** tab configures this app setting for you when you create a Storage account or selects an existing one.</span></span> <span data-ttu-id="3f07a-155">Emellett [konfigurálása az alkalmazás manuális beállításával](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="3f07a-155">You can also [configure this app setting manually](functions-how-to-use-azure-function-app-settings.md#settings).</span></span> 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="3f07a-156">Kimeneti használata</span><span class="sxs-lookup"><span data-stu-id="3f07a-156">Output usage</span></span>
<span data-ttu-id="3f07a-157">A C# függvények, akkor eszközben csatlakozzon a táblázatos kimenete az elnevezett `out` a függvényaláíráshoz a paraméter, például `out <T> <name>`, ahol `T` , az adatokat, írja be, hogy szeretné-e szerializálni az adatokat, és `paramName` a megadott név a [kimeneti kötése](#output).</span><span class="sxs-lookup"><span data-stu-id="3f07a-157">In C# functions, you bind to the table output by using the named `out` parameter in your function signature, like `out <T> <name>`, where `T` is the data type that you want to serialize the data into, and `paramName` is the name you specified in the [output binding](#output).</span></span> <span data-ttu-id="3f07a-158">Node.js-függvény, akkor a táblának az elérésére használja `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="3f07a-158">In Node.js functions, you access the table output using `context.bindings.<name>`.</span></span>

<span data-ttu-id="3f07a-159">Node.js vagy C# funkciók objektumokat is szerializálni.</span><span class="sxs-lookup"><span data-stu-id="3f07a-159">You can serialize objects in Node.js or C# functions.</span></span> <span data-ttu-id="3f07a-160">A C# funkciók is kell kötni a következő esetében:</span><span class="sxs-lookup"><span data-stu-id="3f07a-160">In C# functions, you can also bind to the following types:</span></span>

* <span data-ttu-id="3f07a-161">Magában foglaló típussal`ITableEntity`</span><span class="sxs-lookup"><span data-stu-id="3f07a-161">Any type that implements `ITableEntity`</span></span>
* <span data-ttu-id="3f07a-162">`ICollector<T>`(a kimeneti több entitás.</span><span class="sxs-lookup"><span data-stu-id="3f07a-162">`ICollector<T>` (to output multiple entities.</span></span> <span data-ttu-id="3f07a-163">Lásd: [minta](#outcsharp).)</span><span class="sxs-lookup"><span data-stu-id="3f07a-163">See [sample](#outcsharp).)</span></span>
* <span data-ttu-id="3f07a-164">`IAsyncCollector<T>`(aszinkron verzióját `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="3f07a-164">`IAsyncCollector<T>` (async version of `ICollector<T>`)</span></span>
* <span data-ttu-id="3f07a-165">`CloudTable`(az Azure Storage szolgáltatás SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="3f07a-165">`CloudTable` (using the Azure Storage SDK.</span></span> <span data-ttu-id="3f07a-166">Lásd: [minta](#readmulti).)</span><span class="sxs-lookup"><span data-stu-id="3f07a-166">See [sample](#readmulti).)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="3f07a-167">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="3f07a-167">Output sample</span></span>
<span data-ttu-id="3f07a-168">A következő *function.json* és *run.csx* példa bemutatja, hogyan több tábla entitás írni.</span><span class="sxs-lookup"><span data-stu-id="3f07a-168">The following *function.json* and *run.csx* example shows how to write multiple table entities.</span></span>

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

<span data-ttu-id="3f07a-169">Tekintse meg a nyelvspecifikus mintát, amely több tábla entitás hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3f07a-169">See the language-specific sample that creates multiple table entities.</span></span>

* [<span data-ttu-id="3f07a-170">C#</span><span class="sxs-lookup"><span data-stu-id="3f07a-170">C#</span></span>](#outcsharp)
* [<span data-ttu-id="3f07a-171">F#</span><span class="sxs-lookup"><span data-stu-id="3f07a-171">F#</span></span>](#outfsharp)
* [<span data-ttu-id="3f07a-172">Node.js</span><span class="sxs-lookup"><span data-stu-id="3f07a-172">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="3f07a-173">A C# kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="3f07a-173">Output sample in C#</span></span> #
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

### <a name="output-sample-in-f"></a><span data-ttu-id="3f07a-174">Az F # kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="3f07a-174">Output sample in F#</span></span> #
```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="3f07a-175">Kimeneti minta node.js</span><span class="sxs-lookup"><span data-stu-id="3f07a-175">Output sample in Node.js</span></span>
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

## <a name="sample-read-multiple-table-entities-in-c"></a><span data-ttu-id="3f07a-176">Minta: Olvassa el a C# több tábla entitás</span><span class="sxs-lookup"><span data-stu-id="3f07a-176">Sample: Read multiple table entities in C#</span></span>  #
<span data-ttu-id="3f07a-177">A következő *function.json* és C# Kódpélda beolvassa az üzenetsorban lévő üzenetet megadott partíciókulcsot az entitásokat.</span><span class="sxs-lookup"><span data-stu-id="3f07a-177">The following *function.json* and C# code example reads entities for a partition key that is specified in the queue message.</span></span>

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

<span data-ttu-id="3f07a-178">A C#-kódban hozzáad egy hivatkozást az Azure Storage szolgáltatás SDK, úgy, hogy az entitás típusa is származik `TableEntity`.</span><span class="sxs-lookup"><span data-stu-id="3f07a-178">The C# code adds a reference to the Azure Storage SDK so that the entity type can derive from `TableEntity`.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="3f07a-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3f07a-179">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


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
# <a name="azure-functions-storage-table-bindings"></a><span data-ttu-id="c7b7c-104">Az Azure Functions tárolási tábla kötések</span><span class="sxs-lookup"><span data-stu-id="c7b7c-104">Azure Functions Storage table bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="c7b7c-105">Ez a cikk ismerteti, hogyan tooconfigure és az Azure Storage kód tábla az Azure Functions kötések.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-105">This article explains how tooconfigure and code Azure Storage table bindings in Azure Functions.</span></span> <span data-ttu-id="c7b7c-106">Azure Functions támogatja bemeneti és kimeneti Azure Storage-táblákat kötéseit.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-106">Azure Functions supports input and output bindings for Azure Storage tables.</span></span>

<span data-ttu-id="c7b7c-107">hello tárolási tábla kötés támogatja-e a következő forgatókönyvek hello:</span><span class="sxs-lookup"><span data-stu-id="c7b7c-107">hello Storage table binding supports hello following scenarios:</span></span>

* <span data-ttu-id="c7b7c-108">**Egy C# vagy Node.js függvény egyetlen sor olvasása** - beállított `partitionKey` és `rowKey`.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-108">**Read a single row in a C# or Node.js function** - Set `partitionKey` and `rowKey`.</span></span> <span data-ttu-id="c7b7c-109">Hello `filter` és `take` tulajdonságok nem szerepel ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-109">hello `filter` and `take` properties are not used in this scenario.</span></span>
* <span data-ttu-id="c7b7c-110">**Olvassa el a C# függvényben több sort** -hello Functions futtatókörnyezete biztosít egy `IQueryable<T>` objektumhoz kötött toohello tábla.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-110">**Read multiple rows in a C# function** - hello Functions runtime provides an `IQueryable<T>` object bound toohello table.</span></span> <span data-ttu-id="c7b7c-111">Típus `T` kell származnia `TableEntity` vagy megvalósítása `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-111">Type `T` must derive from `TableEntity` or implement `ITableEntity`.</span></span> <span data-ttu-id="c7b7c-112">Hello `partitionKey`, `rowKey`, `filter`, és `take` tulajdonságok nem szerepel ebben a forgatókönyvben; használhatja a hello `IQueryable` objektum toodo szükséges szűrés.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-112">hello `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario; you can use hello `IQueryable` object toodo any filtering required.</span></span> 
* <span data-ttu-id="c7b7c-113">**Egy csomópont függvény több sor olvasása** - beállított hello `filter` és `take` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-113">**Read multiple rows in a Node function** - Set hello `filter` and `take` properties.</span></span> <span data-ttu-id="c7b7c-114">Nincs beállítva `partitionKey` vagy `rowKey`.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-114">Don't set `partitionKey` or `rowKey`.</span></span>
* <span data-ttu-id="c7b7c-115">**Egy vagy több sor írása C# függvények** -hello Functions futtatókörnyezete biztosít egy `ICollector<T>` vagy `IAsyncCollector<T>` kötött toohello tábla, ahol `T` hello séma meghatározza hello entitások tooadd szeretné.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-115">**Write one or more rows in a C# function** - hello Functions runtime provides an `ICollector<T>` or `IAsyncCollector<T>` bound toohello table, where `T` specifies hello schema of hello entities you want tooadd.</span></span> <span data-ttu-id="c7b7c-116">Általában, írja be a `T` származik `TableEntity` vagy megvalósítja `ITableEntity`, de nem kell.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-116">Typically, type `T` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to.</span></span> <span data-ttu-id="c7b7c-117">Hello `partitionKey`, `rowKey`, `filter`, és `take` tulajdonságok nem szerepel ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-117">hello `partitionKey`, `rowKey`, `filter`, and `take` properties are not used in this scenario.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a><span data-ttu-id="c7b7c-118">Tárolási tábla bemeneti kötése</span><span class="sxs-lookup"><span data-stu-id="c7b7c-118">Storage table input binding</span></span>
<span data-ttu-id="c7b7c-119">hello Azure Storage bemeneti táblakötéssel lehetővé teszi a függvény egy tárolási tábla toouse.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-119">hello Azure Storage table input binding enables you toouse a storage table in your function.</span></span> 

<span data-ttu-id="c7b7c-120">hello tárolási tábla bemeneti tooa függvény használja a következő hello a JSON-objektumok hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="c7b7c-120">hello Storage table input tooa function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

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

<span data-ttu-id="c7b7c-121">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="c7b7c-121">Note hello following:</span></span> 

* <span data-ttu-id="c7b7c-122">Használjon `partitionKey` és `rowKey` együtt tooread egyetlen entitás.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-122">Use `partitionKey` and `rowKey` together tooread a single entity.</span></span> <span data-ttu-id="c7b7c-123">Ezek a tulajdonságok egyike sem kötelező.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-123">These properties are optional.</span></span> 
* <span data-ttu-id="c7b7c-124">`connection`egy tárolási kapcsolati karakterlánc tartalmazó Alkalmazásbeállítás hello nevét kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-124">`connection` must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="c7b7c-125">Hello Azure-portálon, a hello hello a szokásos szerkesztő **integráció** lapon konfigurálja az Alkalmazásbeállítás hoz létre, a tárolási fiók vagy választja ki egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-125">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you create a Storage account or selects an existing one.</span></span> <span data-ttu-id="c7b7c-126">Emellett [konfigurálása az alkalmazás manuális beállításával](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="c7b7c-126">You can also [configure this app setting manually](functions-how-to-use-azure-function-app-settings.md#settings).</span></span>  

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="c7b7c-127">Bemeneti kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="c7b7c-127">Input usage</span></span>
<span data-ttu-id="c7b7c-128">A C# függvények, akkor kötni toohello bemeneti tábla entitás (vagy entitások) használatával egy elnevezett paraméter a függvényaláíráshoz, például `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-128">In C# functions, you bind toohello input table entity (or entities) by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="c7b7c-129">Ha `T` adattípus hello megjeleníteni kívánt toodeserialize hello adatokat, és `paramName` hello megadott hello név [kötés bemeneti](#input).</span><span class="sxs-lookup"><span data-stu-id="c7b7c-129">Where `T` is hello data type that you want toodeserialize hello data into, and `paramName` is hello name you specified in hello [input binding](#input).</span></span> <span data-ttu-id="c7b7c-130">A Node.js funkciókat érheti el hello bemeneti tábla entitás (vagy entitások) használatával `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-130">In Node.js functions, you access hello input table entity (or entities) using `context.bindings.<name>`.</span></span>

<span data-ttu-id="c7b7c-131">Node.js vagy C# funkciók is deszerializálható hello bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-131">hello input data can be deserialized in Node.js or C# functions.</span></span> <span data-ttu-id="c7b7c-132">hello deszerializálni objektumok `RowKey` és `PartitionKey` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-132">hello deserialized objects have `RowKey` and `PartitionKey` properties.</span></span>

<span data-ttu-id="c7b7c-133">A C# funkciók is köthető a következő típusok hello tooany, és futásidejű megpróbál hello funkciók túl deszerializálni hello adatait, hogy a típus használatával:</span><span class="sxs-lookup"><span data-stu-id="c7b7c-133">In C# functions, you can also bind tooany of hello following types, and hello Functions runtime will attempt too deserialize hello table data using that type:</span></span>

* <span data-ttu-id="c7b7c-134">Magában foglaló típussal`ITableEntity`</span><span class="sxs-lookup"><span data-stu-id="c7b7c-134">Any type that implements `ITableEntity`</span></span>
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="c7b7c-135">A minta bemeneti</span><span class="sxs-lookup"><span data-stu-id="c7b7c-135">Input sample</span></span>
<span data-ttu-id="c7b7c-136">Kellett volna lennie a következő function.json, használja a várólista eseményindító tooread egy-egy sorának hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-136">Supposed you have hello following function.json, which uses a queue trigger tooread a single table row.</span></span> <span data-ttu-id="c7b7c-137">hello JSON megadja `PartitionKey`  
 `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-137">hello JSON specifies `PartitionKey` 
`RowKey`.</span></span> <span data-ttu-id="c7b7c-138">`"rowKey": "{queueTrigger}"`azt jelzi, hogy hello sor kulcs hello várólista üzenet karakterlánc származik.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-138">`"rowKey": "{queueTrigger}"` indicates that hello row key comes from hello queue message string.</span></span>

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

<span data-ttu-id="c7b7c-139">Lásd: hello nyelvspecifikus minta a táblázat egyetlen entitás olvasó.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-139">See hello language-specific sample that reads a single table entity.</span></span>

* [<span data-ttu-id="c7b7c-140">C#</span><span class="sxs-lookup"><span data-stu-id="c7b7c-140">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="c7b7c-141">F#</span><span class="sxs-lookup"><span data-stu-id="c7b7c-141">F#</span></span>](#inputfsharp)
* [<span data-ttu-id="c7b7c-142">Node.js</span><span class="sxs-lookup"><span data-stu-id="c7b7c-142">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="c7b7c-143">A C# bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="c7b7c-143">Input sample in C#</span></span> #
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

### <a name="input-sample-in-f"></a><span data-ttu-id="c7b7c-144">Az F # bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="c7b7c-144">Input sample in F#</span></span> #
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

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="c7b7c-145">A node.js bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="c7b7c-145">Input sample in Node.js</span></span>
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a><span data-ttu-id="c7b7c-146">Tárolási tábla kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="c7b7c-146">Storage table output binding</span></span>
<span data-ttu-id="c7b7c-147">a kimeneti hello Azure Storage tábla kötés lehetővé teszi a toowrite entitások tooa tárolási tábla a függvényben.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-147">hello Azure Storage table output binding enables you toowrite entities tooa Storage table in your function.</span></span> 

<span data-ttu-id="c7b7c-148">hello tárolási táblázatos kimenete egy függvény hello hello a JSON-objektumok a következő célokra `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="c7b7c-148">hello Storage table output for a function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

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

<span data-ttu-id="c7b7c-149">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="c7b7c-149">Note hello following:</span></span> 

* <span data-ttu-id="c7b7c-150">Használjon `partitionKey` és `rowKey` együtt toowrite egyetlen entitás.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-150">Use `partitionKey` and `rowKey` together toowrite a single entity.</span></span> <span data-ttu-id="c7b7c-151">Ezek a tulajdonságok egyike sem kötelező.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-151">These properties are optional.</span></span> <span data-ttu-id="c7b7c-152">Azt is megadhatja, `PartitionKey` és `RowKey` létrehozásakor hello entitásobjektumok gyűjteményeit a függvény kódban.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-152">You can also specify `PartitionKey` and `RowKey` when you create hello entity objects in your function code.</span></span>
* <span data-ttu-id="c7b7c-153">`connection`egy tárolási kapcsolati karakterlánc tartalmazó Alkalmazásbeállítás hello nevét kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-153">`connection` must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="c7b7c-154">Hello Azure-portálon, a hello hello a szokásos szerkesztő **integráció** lapon konfigurálja az Alkalmazásbeállítás hoz létre, a tárolási fiók vagy választja ki egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-154">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you create a Storage account or selects an existing one.</span></span> <span data-ttu-id="c7b7c-155">Emellett [konfigurálása az alkalmazás manuális beállításával](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="c7b7c-155">You can also [configure this app setting manually](functions-how-to-use-azure-function-app-settings.md#settings).</span></span> 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="c7b7c-156">Kimeneti használata</span><span class="sxs-lookup"><span data-stu-id="c7b7c-156">Output usage</span></span>
<span data-ttu-id="c7b7c-157">A C# függvények, akkor eszközben toohello táblázatos kimenete nevű hello `out` a függvényaláíráshoz a paraméter, például `out <T> <name>`, ahol `T` adattípus hello megjeleníteni kívánt tooserialize hello adatokat, és `paramName` van hello nevét, hello megadott [kimeneti kötése](#output).</span><span class="sxs-lookup"><span data-stu-id="c7b7c-157">In C# functions, you bind toohello table output by using hello named `out` parameter in your function signature, like `out <T> <name>`, where `T` is hello data type that you want tooserialize hello data into, and `paramName` is hello name you specified in hello [output binding](#output).</span></span> <span data-ttu-id="c7b7c-158">A Node.js funkciókat érheti el hello tábla használatával kimeneti `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-158">In Node.js functions, you access hello table output using `context.bindings.<name>`.</span></span>

<span data-ttu-id="c7b7c-159">Node.js vagy C# funkciók objektumokat is szerializálni.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-159">You can serialize objects in Node.js or C# functions.</span></span> <span data-ttu-id="c7b7c-160">A C# funkciók is köthető a következő típusok toohello:</span><span class="sxs-lookup"><span data-stu-id="c7b7c-160">In C# functions, you can also bind toohello following types:</span></span>

* <span data-ttu-id="c7b7c-161">Magában foglaló típussal`ITableEntity`</span><span class="sxs-lookup"><span data-stu-id="c7b7c-161">Any type that implements `ITableEntity`</span></span>
* <span data-ttu-id="c7b7c-162">`ICollector<T>`(toooutput több entitás.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-162">`ICollector<T>` (toooutput multiple entities.</span></span> <span data-ttu-id="c7b7c-163">Lásd: [minta](#outcsharp).)</span><span class="sxs-lookup"><span data-stu-id="c7b7c-163">See [sample](#outcsharp).)</span></span>
* <span data-ttu-id="c7b7c-164">`IAsyncCollector<T>`(aszinkron verzióját `ICollector<T>`)</span><span class="sxs-lookup"><span data-stu-id="c7b7c-164">`IAsyncCollector<T>` (async version of `ICollector<T>`)</span></span>
* <span data-ttu-id="c7b7c-165">`CloudTable`(hello Azure Storage szolgáltatás SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-165">`CloudTable` (using hello Azure Storage SDK.</span></span> <span data-ttu-id="c7b7c-166">Lásd: [minta](#readmulti).)</span><span class="sxs-lookup"><span data-stu-id="c7b7c-166">See [sample](#readmulti).)</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="c7b7c-167">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="c7b7c-167">Output sample</span></span>
<span data-ttu-id="c7b7c-168">hello következő *function.json* és *run.csx* példa azt mutatja meg hogyan toowrite több tábla entitás.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-168">hello following *function.json* and *run.csx* example shows how toowrite multiple table entities.</span></span>

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

<span data-ttu-id="c7b7c-169">Lásd: hello nyelvspecifikus mintát, amely több tábla entitás hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-169">See hello language-specific sample that creates multiple table entities.</span></span>

* [<span data-ttu-id="c7b7c-170">C#</span><span class="sxs-lookup"><span data-stu-id="c7b7c-170">C#</span></span>](#outcsharp)
* [<span data-ttu-id="c7b7c-171">F#</span><span class="sxs-lookup"><span data-stu-id="c7b7c-171">F#</span></span>](#outfsharp)
* [<span data-ttu-id="c7b7c-172">Node.js</span><span class="sxs-lookup"><span data-stu-id="c7b7c-172">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="c7b7c-173">A C# kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="c7b7c-173">Output sample in C#</span></span> #
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

### <a name="output-sample-in-f"></a><span data-ttu-id="c7b7c-174">Az F # kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="c7b7c-174">Output sample in F#</span></span> #
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

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="c7b7c-175">Kimeneti minta node.js</span><span class="sxs-lookup"><span data-stu-id="c7b7c-175">Output sample in Node.js</span></span>
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

## <a name="sample-read-multiple-table-entities-in-c"></a><span data-ttu-id="c7b7c-176">Minta: Olvassa el a C# több tábla entitás</span><span class="sxs-lookup"><span data-stu-id="c7b7c-176">Sample: Read multiple table entities in C#</span></span>  #
<span data-ttu-id="c7b7c-177">hello következő *function.json* és C# Kódpélda beolvassa várólista üdvözlőüzenetére megadott partíciókulcsot az entitásokat.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-177">hello following *function.json* and C# code example reads entities for a partition key that is specified in hello queue message.</span></span>

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

<span data-ttu-id="c7b7c-178">hello C#-kódban, hogy hello entitás típusa is származik ad hozzá egy hivatkozást toohello Azure Storage szolgáltatás SDK `TableEntity`.</span><span class="sxs-lookup"><span data-stu-id="c7b7c-178">hello C# code adds a reference toohello Azure Storage SDK so that hello entity type can derive from `TableEntity`.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c7b7c-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c7b7c-179">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


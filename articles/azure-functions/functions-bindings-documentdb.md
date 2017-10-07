---
title: "aaaAzure funkciók Cosmos DB kötések |} Microsoft Docs"
description: "Megértéséhez hogyan toouse Azure Cosmos DB kötések Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: 3d8497f0-21f3-437d-ba24-5ece8c90ac85
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/18/2016
ms.author: glenga
ms.openlocfilehash: 76b89e8296db1dd28dff9528903b1f6a28f55232
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-cosmos-db-bindings"></a><span data-ttu-id="9f5f7-104">Az Azure Functions Cosmos DB kötések</span><span class="sxs-lookup"><span data-stu-id="9f5f7-104">Azure Functions Cosmos DB bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="9f5f7-105">Ez a cikk azt ismerteti, hogyan tooconfigure és kód az Azure Functions Azure Cosmos DB kötések.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-105">This article explains how tooconfigure and code Azure Cosmos DB bindings in Azure Functions.</span></span> <span data-ttu-id="9f5f7-106">Az Azure Functions támogatja bemeneti és kimeneti Cosmos DB kötései.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-106">Azure Functions supports input and output bindings for Cosmos DB.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="9f5f7-107">A Cosmos DB további információkért lásd: [bemutatása tooCosmos DB](../documentdb/documentdb-introduction.md) és [egy Cosmos DB Konzolalkalmazás létrehozása](../documentdb/documentdb-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9f5f7-107">For more information on Cosmos DB, see [Introduction tooCosmos DB](../documentdb/documentdb-introduction.md) and [Build a Cosmos DB console application](../documentdb/documentdb-get-started.md).</span></span>

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a><span data-ttu-id="9f5f7-108">A DocumentDB API bemeneti kötése</span><span class="sxs-lookup"><span data-stu-id="9f5f7-108">DocumentDB API input binding</span></span>
<span data-ttu-id="9f5f7-109">hello DocumentDB API bemeneti kötése lekérdezi egy Cosmos DB dokumentumot, és átadja toohello nevű hello függvény a bemeneti paraméter.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-109">hello DocumentDB API input binding retrieves a Cosmos DB document and passes it toohello named input parameter of hello function.</span></span> <span data-ttu-id="9f5f7-110">hello dokumentum azonosító is meg lehet határozni, amely meghívja hello függvényt hello eseményindító alapján.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-110">hello document ID can be determined based on hello trigger that invokes hello function.</span></span> 

<span data-ttu-id="9f5f7-111">hello DocumentDB API bemeneti kötése van a következő tulajdonságok hello *function.json*:</span><span class="sxs-lookup"><span data-stu-id="9f5f7-111">hello DocumentDB API input binding has hello following properties in *function.json*:</span></span>

- <span data-ttu-id="9f5f7-112">`name`: Hello dokumentum függvény kódban használt azonosító neve</span><span class="sxs-lookup"><span data-stu-id="9f5f7-112">`name` : Identifier name used in function code for hello document</span></span>
- <span data-ttu-id="9f5f7-113">`type`: túl állítható be "documentdb"</span><span class="sxs-lookup"><span data-stu-id="9f5f7-113">`type` : must be set too"documentdb"</span></span>
- <span data-ttu-id="9f5f7-114">`databaseName`: hello dokumentumot tartalmazó hello adatbázis</span><span class="sxs-lookup"><span data-stu-id="9f5f7-114">`databaseName` : hello database containing hello document</span></span>
- <span data-ttu-id="9f5f7-115">`collectionName`: hello dokumentum tartalmazó hello gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="9f5f7-115">`collectionName` : hello collection containing hello document</span></span>
- <span data-ttu-id="9f5f7-116">`id`: hello hello dokumentum tooretrieve azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-116">`id` : hello Id of hello document tooretrieve.</span></span> <span data-ttu-id="9f5f7-117">Ez a tulajdonság támogatja kötések paraméterek; Lásd: [kötési kifejezésekben toocustom bemeneti tulajdonságai más elemekhez köthetők](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) hello cikkben [Azure Functions eseményindítók és kötések fogalmak](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="9f5f7-117">This property supports bindings parameters; see [Bind toocustom input properties in a binding expression](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) in hello article [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>
- <span data-ttu-id="9f5f7-118">`sqlQuery`: Egy Cosmos adatbázis SQL-lekérdezésben használt több dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-118">`sqlQuery` : A Cosmos DB SQL query used for retrieving multiple documents.</span></span> <span data-ttu-id="9f5f7-119">hello lekérdezés futásidejű kötések támogatja.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-119">hello query supports runtime bindings.</span></span> <span data-ttu-id="9f5f7-120">Például:`SELECT * FROM c where c.departmentId = {departmentId}`</span><span class="sxs-lookup"><span data-stu-id="9f5f7-120">For example: `SELECT * FROM c where c.departmentId = {departmentId}`</span></span>
- <span data-ttu-id="9f5f7-121">`connection`: a Cosmos DB kapcsolati karakterláncot tartalmazó hello Alkalmazásbeállítás hello neve</span><span class="sxs-lookup"><span data-stu-id="9f5f7-121">`connection` : hello name of hello app setting containing your Cosmos DB connection string</span></span>
- <span data-ttu-id="9f5f7-122">`direction`: be kell állítani túl`"in"`.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-122">`direction`  : must be set too`"in"`.</span></span>

<span data-ttu-id="9f5f7-123">Tulajdonságok hello `id` és `sqlQuery` nem adható meg egyszerre.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-123">hello properties `id` and `sqlQuery` cannot both be specified.</span></span> <span data-ttu-id="9f5f7-124">Ha sem `id` sem `sqlQuery` be van állítva, hello teljes gyűjtemény lekérése.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-124">If neither `id` nor `sqlQuery` is set, hello entire collection is retrieved.</span></span>

## <a name="using-a-documentdb-api-input-binding"></a><span data-ttu-id="9f5f7-125">A DocumentDB API bemeneti kötés használata</span><span class="sxs-lookup"><span data-stu-id="9f5f7-125">Using a DocumentDB API input binding</span></span>

* <span data-ttu-id="9f5f7-126">A C# és F # függvényekben amikor hello függvény sikeresen, kilép toohello bemeneti dokumentum elnevezett bemeneti paraméterek keresztül végzett módosítások automatikusan megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-126">In C# and F# functions, when hello function exits successfully, any changes made toohello input document via named input parameters are automatically persisted.</span></span> 
* <span data-ttu-id="9f5f7-127">A JavaScript-funkcióként frissítések nem automatikusan történik függvény kilépés után.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-127">In JavaScript functions, updates are not made automatically upon function exit.</span></span> <span data-ttu-id="9f5f7-128">Ehelyett használjon `context.bindings.<documentName>In` és `context.bindings.<documentName>Out` toomake frissítések.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-128">Instead, use `context.bindings.<documentName>In` and `context.bindings.<documentName>Out` toomake updates.</span></span> <span data-ttu-id="9f5f7-129">Lásd: hello [JavaScript minta](#injavascript).</span><span class="sxs-lookup"><span data-stu-id="9f5f7-129">See hello [JavaScript sample](#injavascript).</span></span>

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a><span data-ttu-id="9f5f7-130">Egyetlen dokumentum bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-130">Input sample for single document</span></span>
<span data-ttu-id="9f5f7-131">Tegyük fel, hogy hello következő rendelkezik DocumentDB API bemeneti hello kötések `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="9f5f7-131">Suppose you have hello following DocumentDB API input binding in hello `bindings` array of function.json:</span></span>

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```

<span data-ttu-id="9f5f7-132">Lásd: hello nyelvspecifikus mintát, amely a bemeneti kötése tooupdate hello dokumentum szöveges értéket használja.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-132">See hello language-specific sample that uses this input binding tooupdate hello document's text value.</span></span>

* [<span data-ttu-id="9f5f7-133">C#</span><span class="sxs-lookup"><span data-stu-id="9f5f7-133">C#</span></span>](#incsharp)
* [<span data-ttu-id="9f5f7-134">F#</span><span class="sxs-lookup"><span data-stu-id="9f5f7-134">F#</span></span>](#infsharp)
* [<span data-ttu-id="9f5f7-135">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9f5f7-135">JavaScript</span></span>](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a><span data-ttu-id="9f5f7-136">A C# bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-136">Input sample in C#</span></span> #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a><span data-ttu-id="9f5f7-137">Az F # bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-137">Input sample in F#</span></span> #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

<span data-ttu-id="9f5f7-138">Ez a minta szükséges egy `project.json` fájl, amely meghatározza a hello `FSharp.Interop.Dynamic` és `Dynamitey` NuGet függőségek:</span><span class="sxs-lookup"><span data-stu-id="9f5f7-138">This sample requires a `project.json` file that specifies hello `FSharp.Interop.Dynamic` and `Dynamitey` NuGet dependencies:</span></span>

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

<span data-ttu-id="9f5f7-139">tooadd egy `project.json` fájl című [F # felügyeleti csomag](functions-reference-fsharp.md#package).</span><span class="sxs-lookup"><span data-stu-id="9f5f7-139">tooadd a `project.json` file, see [F# package management](functions-reference-fsharp.md#package).</span></span>

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a><span data-ttu-id="9f5f7-140">A JavaScript bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-140">Input sample in JavaScript</span></span>

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a><span data-ttu-id="9f5f7-141">Több dokumentumokkal bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-141">Input sample with multiple documents</span></span>

<span data-ttu-id="9f5f7-142">Tegyük fel, hogy kívánja tooretrieve SQL-lekérdezést, amelyet több dokumentumok egy várólista eseményindító toocustomize hello lekérdezési paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-142">Suppose that you wish tooretrieve multiple documents specified by a SQL query, using a queue trigger toocustomize hello query parameters.</span></span> 

<span data-ttu-id="9f5f7-143">Ebben a példában hello várólista eseményindító biztosít egy paraméter `departmentId`. A várólista üzenet `{ "departmentId" : "Finance" }` alakítanák vissza hello pénzügyi részleg összes rekordja.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-143">In this example, hello queue trigger provides a parameter `departmentId`.A queue message of `{ "departmentId" : "Finance" }` would return all records for hello finance department.</span></span> <span data-ttu-id="9f5f7-144">Használja a hello következő *function.json*:</span><span class="sxs-lookup"><span data-stu-id="9f5f7-144">Use hello following in *function.json*:</span></span>

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}"
    "connection": "CosmosDBConnection"
}
```

### <a name="input-sample-with-multiple-documents-in-c"></a><span data-ttu-id="9f5f7-145">A C# több dokumentumokkal bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-145">Input sample with multiple documents in C#</span></span>

```csharp
public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
{   
    foreach (var doc in documents)
    {
        // operate on each document
    }    
}

public class QueuePayload
{
    public string departmentId { get; set; }
}
```

### <a name="input-sample-with-multiple-documents-in-javascript"></a><span data-ttu-id="9f5f7-146">Több JavaScript-dokumentumok bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-146">Input sample with multiple documents in JavaScript</span></span>

```javascript
module.exports = function (context, input) {    
    var documents = context.bindings.documents;
    for (var i = 0; i < documents.length; i++) {
        var document = documents[i];
        // operate on each document
    }       
    context.done();
};
```

## <span data-ttu-id="9f5f7-147"><a id="docdboutput"></a>A DocumentDB API kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="9f5f7-147"><a id="docdboutput"></a>DocumentDB API output binding</span></span>
<span data-ttu-id="9f5f7-148">a DocumentDB API hello kimeneti kötés lehetővé teszi egy új dokumentumot tooan Azure Cosmos DB adatbázis írását.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-148">hello DocumentDB API output binding lets you write a new document tooan Azure Cosmos DB database.</span></span> <span data-ttu-id="9f5f7-149">A következő tulajdonságok hello rendelkezik *function.json*:</span><span class="sxs-lookup"><span data-stu-id="9f5f7-149">It has hello following properties in *function.json*:</span></span>

- <span data-ttu-id="9f5f7-150">`name`: Új dokumentum hello függvény kódban használt azonosítója</span><span class="sxs-lookup"><span data-stu-id="9f5f7-150">`name` : Identifier used in function code for hello new document</span></span>
- <span data-ttu-id="9f5f7-151">`type`: be kell állítani túl`"documentdb"`</span><span class="sxs-lookup"><span data-stu-id="9f5f7-151">`type` : must be set too`"documentdb"`</span></span>
- <span data-ttu-id="9f5f7-152">`databaseName`: hello gyűjtemény, ahol létrejön az új dokumentum hello tartalmazó hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-152">`databaseName` : hello database containing hello collection where hello new document will be created.</span></span>
- <span data-ttu-id="9f5f7-153">`collectionName`: gyűjtemény, ahol létrejön az új dokumentum hello hello.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-153">`collectionName` : hello collection where hello new document will be created.</span></span>
- <span data-ttu-id="9f5f7-154">`createIfNotExists`: Egy logikai érték tooindicate, hogy hello gyűjtemény jön létre, ha nem létezik.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-154">`createIfNotExists` : A boolean value tooindicate whether hello collection will be created if it does not exist.</span></span> <span data-ttu-id="9f5f7-155">hello alapértelmezett érték a *hamis*.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-155">hello default is *false*.</span></span> <span data-ttu-id="9f5f7-156">hello oka a következő új gyűjtemények fenntartott átviteli sebességet, amely jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-156">hello reason for this is new collections are created with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="9f5f7-157">További részletekért látogasson el a hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="9f5f7-157">For more details, please visit hello [pricing page](https://azure.microsoft.com/pricing/details/documentdb/).</span></span>
- <span data-ttu-id="9f5f7-158">`connection`: a Cosmos DB kapcsolati karakterláncot tartalmazó hello Alkalmazásbeállítás hello neve</span><span class="sxs-lookup"><span data-stu-id="9f5f7-158">`connection` : hello name of hello app setting containing your Cosmos DB connection string</span></span>
- <span data-ttu-id="9f5f7-159">`direction`: be kell állítani túl`"out"`</span><span class="sxs-lookup"><span data-stu-id="9f5f7-159">`direction` : must be set too`"out"`</span></span>

## <a name="using-a-documentdb-api-output-binding"></a><span data-ttu-id="9f5f7-160">A DocumentDB API-jával kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="9f5f7-160">Using a DocumentDB API output binding</span></span>
<span data-ttu-id="9f5f7-161">Ez a szakasz bemutatja, hogyan toouse a DocumentDB API kimeneti kötelező a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-161">This section shows you how toouse your DocumentDB API output binding in your function code.</span></span>

<span data-ttu-id="9f5f7-162">Amikor ír toohello kimeneti paraméter a függvény alapértelmezés szerint egy új dokumentumot jön létre az adatbázisban, az automatikusan előállított GUID Azonosítóhoz hello, a dokumentum azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-162">When you write toohello output parameter in your function, by default a new document is generated in your database, with an automatically generated GUID as hello document ID.</span></span> <span data-ttu-id="9f5f7-163">Megadhatja a kimeneti dokumentum hello Dokumentumazonosítója hello megadásával `id` hello tulajdonságait JSON kimeneti paraméterként.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-163">You can specify hello document ID of output document by specifying hello `id` JSON property in hello output parameter.</span></span> 

>[!Note]  
><span data-ttu-id="9f5f7-164">Amikor megad egy meglévő dokumentum hello azonosítója, a fiókbeállítása felülíródik hello új kimeneti dokumentum szerint.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-164">When you specify hello ID of an existing document, it gets overwritten by hello new output document.</span></span> 

<span data-ttu-id="9f5f7-165">toooutput több dokumentumokat is köthető túl`ICollector<T>` vagy `IAsyncCollector<T>` ahol `T` hello támogatott típusok egyike.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-165">toooutput multiple documents, you can also bind too`ICollector<T>` or `IAsyncCollector<T>` where `T` is one of hello supported types.</span></span>

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a><span data-ttu-id="9f5f7-166">A DocumentDB API kimeneti kötése minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-166">DocumentDB API output binding sample</span></span>
<span data-ttu-id="9f5f7-167">Tegyük fel, hogy hello következő rendelkezik DocumentDB API kimeneti hello kötések `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="9f5f7-167">Suppose you have hello following DocumentDB API output binding in hello `bindings` array of function.json:</span></span>

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
  "direction": "out"
}
```

<span data-ttu-id="9f5f7-168">És egy várólista fogadása JSON formátum a következő hello várólista bemeneti kötése van:</span><span class="sxs-lookup"><span data-stu-id="9f5f7-168">And you have a queue input binding for a queue that receives JSON in hello following format:</span></span>

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

<span data-ttu-id="9f5f7-169">És azt szeretné, hogy az egyes rekordok formátuma a következő hello toocreate Cosmos DB dokumentumainak:</span><span class="sxs-lookup"><span data-stu-id="9f5f7-169">And you want toocreate Cosmos DB documents in hello following format for each record:</span></span>

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

<span data-ttu-id="9f5f7-170">Tekintse meg a kimeneti kötése tooadd dokumentumok tooyour adatbázist használó hello nyelvspecifikus minta.</span><span class="sxs-lookup"><span data-stu-id="9f5f7-170">See hello language-specific sample that uses this output binding tooadd documents tooyour database.</span></span>

* [<span data-ttu-id="9f5f7-171">C#</span><span class="sxs-lookup"><span data-stu-id="9f5f7-171">C#</span></span>](#outcsharp)
* [<span data-ttu-id="9f5f7-172">F#</span><span class="sxs-lookup"><span data-stu-id="9f5f7-172">F#</span></span>](#outfsharp)
* [<span data-ttu-id="9f5f7-173">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9f5f7-173">JavaScript</span></span>](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="9f5f7-174">A C# kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-174">Output sample in C#</span></span> #

```cs
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
{
  log.Info($"C# Queue trigger function processed: {myQueueItem}");

  dynamic employee = JObject.Parse(myQueueItem);

  employeeDocument = new {
    id = employee.name + "-" + employee.employeeId,
    name = employee.name,
    employeeId = employee.employeeId,
    address = employee.address
  };
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a><span data-ttu-id="9f5f7-175">Az F # kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-175">Output sample in F#</span></span> #

```fsharp
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Employee = {
  id: string
  name: string
  employeeId: string
  address: string
}

let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
  log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
  let employee = JObject.Parse(myQueueItem)
  employeeDocument <-
    { id = sprintf "%s-%s" employee?name employee?employeeId
      name = employee?name
      employeeId = employee?employeeId
      address = employee?address }
```

<span data-ttu-id="9f5f7-176">Ez a minta szükséges egy `project.json` fájl, amely meghatározza a hello `FSharp.Interop.Dynamic` és `Dynamitey` NuGet függőségek:</span><span class="sxs-lookup"><span data-stu-id="9f5f7-176">This sample requires a `project.json` file that specifies hello `FSharp.Interop.Dynamic` and `Dynamitey` NuGet dependencies:</span></span>

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

<span data-ttu-id="9f5f7-177">tooadd egy `project.json` fájl című [F # felügyeleti csomag](functions-reference-fsharp.md#package).</span><span class="sxs-lookup"><span data-stu-id="9f5f7-177">tooadd a `project.json` file, see [F# package management](functions-reference-fsharp.md#package).</span></span>

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a><span data-ttu-id="9f5f7-178">A JavaScript kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="9f5f7-178">Output sample in JavaScript</span></span>

```javascript
module.exports = function (context) {

  context.bindings.employeeDocument = JSON.stringify({ 
    id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
    name: context.bindings.myQueueItem.name,
    employeeId: context.bindings.myQueueItem.employeeId,
    address: context.bindings.myQueueItem.address
  });

  context.done();
};
```

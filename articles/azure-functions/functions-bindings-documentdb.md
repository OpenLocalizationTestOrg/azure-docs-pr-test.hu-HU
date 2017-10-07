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
# <a name="azure-functions-cosmos-db-bindings"></a>Az Azure Functions Cosmos DB kötések
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk azt ismerteti, hogyan tooconfigure és kód az Azure Functions Azure Cosmos DB kötések. Az Azure Functions támogatja bemeneti és kimeneti Cosmos DB kötései.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

A Cosmos DB további információkért lásd: [bemutatása tooCosmos DB](../documentdb/documentdb-introduction.md) és [egy Cosmos DB Konzolalkalmazás létrehozása](../documentdb/documentdb-get-started.md).

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a>A DocumentDB API bemeneti kötése
hello DocumentDB API bemeneti kötése lekérdezi egy Cosmos DB dokumentumot, és átadja toohello nevű hello függvény a bemeneti paraméter. hello dokumentum azonosító is meg lehet határozni, amely meghívja hello függvényt hello eseményindító alapján. 

hello DocumentDB API bemeneti kötése van a következő tulajdonságok hello *function.json*:

- `name`: Hello dokumentum függvény kódban használt azonosító neve
- `type`: túl állítható be "documentdb"
- `databaseName`: hello dokumentumot tartalmazó hello adatbázis
- `collectionName`: hello dokumentum tartalmazó hello gyűjtemény
- `id`: hello hello dokumentum tooretrieve azonosítója. Ez a tulajdonság támogatja kötések paraméterek; Lásd: [kötési kifejezésekben toocustom bemeneti tulajdonságai más elemekhez köthetők](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression) hello cikkben [Azure Functions eseményindítók és kötések fogalmak](functions-triggers-bindings.md).
- `sqlQuery`: Egy Cosmos adatbázis SQL-lekérdezésben használt több dokumentumot. hello lekérdezés futásidejű kötések támogatja. Például:`SELECT * FROM c where c.departmentId = {departmentId}`
- `connection`: a Cosmos DB kapcsolati karakterláncot tartalmazó hello Alkalmazásbeállítás hello neve
- `direction`: be kell állítani túl`"in"`.

Tulajdonságok hello `id` és `sqlQuery` nem adható meg egyszerre. Ha sem `id` sem `sqlQuery` be van állítva, hello teljes gyűjtemény lekérése.

## <a name="using-a-documentdb-api-input-binding"></a>A DocumentDB API bemeneti kötés használata

* A C# és F # függvényekben amikor hello függvény sikeresen, kilép toohello bemeneti dokumentum elnevezett bemeneti paraméterek keresztül végzett módosítások automatikusan megmaradnak. 
* A JavaScript-funkcióként frissítések nem automatikusan történik függvény kilépés után. Ehelyett használjon `context.bindings.<documentName>In` és `context.bindings.<documentName>Out` toomake frissítések. Lásd: hello [JavaScript minta](#injavascript).

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a>Egyetlen dokumentum bemeneti minta
Tegyük fel, hogy hello következő rendelkezik DocumentDB API bemeneti hello kötések `bindings` function.json tömbje:

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

Lásd: hello nyelvspecifikus mintát, amely a bemeneti kötése tooupdate hello dokumentum szöveges értéket használja.

* [C#](#incsharp)
* [F#](#infsharp)
* [JavaScript](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a>A C# bemeneti minta #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a>Az F # bemeneti minta #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

Ez a minta szükséges egy `project.json` fájl, amely meghatározza a hello `FSharp.Interop.Dynamic` és `Dynamitey` NuGet függőségek:

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

tooadd egy `project.json` fájl című [F # felügyeleti csomag](functions-reference-fsharp.md#package).

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a>A JavaScript bemeneti minta

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a>Több dokumentumokkal bemeneti minta

Tegyük fel, hogy kívánja tooretrieve SQL-lekérdezést, amelyet több dokumentumok egy várólista eseményindító toocustomize hello lekérdezési paraméterek használatával. 

Ebben a példában hello várólista eseményindító biztosít egy paraméter `departmentId`. A várólista üzenet `{ "departmentId" : "Finance" }` alakítanák vissza hello pénzügyi részleg összes rekordja. Használja a hello következő *function.json*:

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

### <a name="input-sample-with-multiple-documents-in-c"></a>A C# több dokumentumokkal bemeneti minta

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

### <a name="input-sample-with-multiple-documents-in-javascript"></a>Több JavaScript-dokumentumok bemeneti minta

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

## <a id="docdboutput"></a>A DocumentDB API kimeneti kötése
a DocumentDB API hello kimeneti kötés lehetővé teszi egy új dokumentumot tooan Azure Cosmos DB adatbázis írását. A következő tulajdonságok hello rendelkezik *function.json*:

- `name`: Új dokumentum hello függvény kódban használt azonosítója
- `type`: be kell állítani túl`"documentdb"`
- `databaseName`: hello gyűjtemény, ahol létrejön az új dokumentum hello tartalmazó hello adatbázis.
- `collectionName`: gyűjtemény, ahol létrejön az új dokumentum hello hello.
- `createIfNotExists`: Egy logikai érték tooindicate, hogy hello gyűjtemény jön létre, ha nem létezik. hello alapértelmezett érték a *hamis*. hello oka a következő új gyűjtemények fenntartott átviteli sebességet, amely jönnek létre. További részletekért látogasson el a hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: a Cosmos DB kapcsolati karakterláncot tartalmazó hello Alkalmazásbeállítás hello neve
- `direction`: be kell állítani túl`"out"`

## <a name="using-a-documentdb-api-output-binding"></a>A DocumentDB API-jával kimeneti kötése
Ez a szakasz bemutatja, hogyan toouse a DocumentDB API kimeneti kötelező a funkciókódot.

Amikor ír toohello kimeneti paraméter a függvény alapértelmezés szerint egy új dokumentumot jön létre az adatbázisban, az automatikusan előállított GUID Azonosítóhoz hello, a dokumentum azonosítóját. Megadhatja a kimeneti dokumentum hello Dokumentumazonosítója hello megadásával `id` hello tulajdonságait JSON kimeneti paraméterként. 

>[!Note]  
>Amikor megad egy meglévő dokumentum hello azonosítója, a fiókbeállítása felülíródik hello új kimeneti dokumentum szerint. 

toooutput több dokumentumokat is köthető túl`ICollector<T>` vagy `IAsyncCollector<T>` ahol `T` hello támogatott típusok egyike.

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a>A DocumentDB API kimeneti kötése minta
Tegyük fel, hogy hello következő rendelkezik DocumentDB API kimeneti hello kötések `bindings` function.json tömbje:

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

És egy várólista fogadása JSON formátum a következő hello várólista bemeneti kötése van:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

És azt szeretné, hogy az egyes rekordok formátuma a következő hello toocreate Cosmos DB dokumentumainak:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Tekintse meg a kimeneti kötése tooadd dokumentumok tooyour adatbázist használó hello nyelvspecifikus minta.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [JavaScript](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>A C# kimeneti minta #

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

### <a name="output-sample-in-f"></a>Az F # kimeneti minta #

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

Ez a minta szükséges egy `project.json` fájl, amely meghatározza a hello `FSharp.Interop.Dynamic` és `Dynamitey` NuGet függőségek:

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

tooadd egy `project.json` fájl című [F # felügyeleti csomag](functions-reference-fsharp.md#package).

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a>A JavaScript kimeneti minta

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

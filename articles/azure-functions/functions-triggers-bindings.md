---
title: "az eseményindítók és kötések az Azure Functions aaaWork |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse eseményindítók és kötések az Azure Functions tooconnect a kód végrehajtása tooonline események és a felhő alapú szolgáltatások."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "azure-függvények, függvények, eseményfeldolgozás, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: cbc7460a-4d8a-423f-a63e-1cd33fef7252
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: donnam
ms.openlocfilehash: eb2ebfca172fcc8c0f479adbcfec99e90fc33615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Az Azure Functions eseményindítók és kötések fogalmak
Az Azure Functions lehetővé teszi az Azure-ban és egyéb szolgáltatások válasz tooevents toowrite kód keresztül *eseményindítók* és *kötések*. Ez a cikk eseményindítók elméleti áttekintését és kötések az összes támogatott programozási nyelveket. Szolgáltatások közös tooall kötések is itt részelemcímkék ismertetését.

## <a name="overview"></a>Áttekintés

Eseményindítók és kötések egy deklaratív módon toodefine hogyan függvényt hívják, és milyen adatokat is működik. A *eseményindító* határozza meg, hogyan függvényt hívják. A függvénynek pontosan egy eseményindító kell rendelkeznie. Eseményindítók olyan adatok, amely általában hello függvény kiváltó hello hasznos lehet társítva. 

Bemeneti és kimeneti *kötések* adjon meg egy deklaratív módon tooconnect toodata, az a kód. Hasonló tootriggers, adja meg a kapcsolati karakterláncokat és egyéb tulajdonságai a függvény konfigurációban. Kötések nem kötelező, és egy függvény több bemeneti és a kimeneti kötéseket. 

Az eseményindítók és kötések, írhat kódot, amely több általános, és amelyen nem kódba foglalni hello részletek hello szolgáltatás, amellyel kommunikál. Egyszerűen vált szolgáltatások bemeneti értékeket a funkciókódot érkező adatokat. tooanother adatszolgáltatás toooutput (például létrehozhat egy új sor Azure Table Storage-ban), használja a hello hello metódus visszatérési értéke. Vagy ha több értéket kell toooutput, akkor egy segítő objektuma. Eseményindítók és kötések rendelkezik egy **neve** tulajdonságot, amelynek segítségével a kód tooaccess hello kötés azonosítója.

Eseményindítók és kötések konfigurálható hello **integráció** hello Azure Functions portálon lapon. Hello színfalak hello felhasználói felület módosít egy fájlt, úgynevezett *function.json* hello függvény fájl. Ez a fájl szerkesztésével toohello módosítása **speciális szerkesztő**.

hello következő táblázatban hello eseményindítók és kötések, az Azure Functions által támogatott. 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a>Példa: a várólista eseményindító és tábla kimeneti kötése

Tegyük fel, hogy egy új sor tooAzure Table Storage toowrite szüksége, amikor az Azure Queue Storage egy új üzenet jelenik meg. Ebben a forgatókönyvben az Azure Queue valósítható eseményindító és egy tábla kimeneti kötése. 

Várólista eseményindító igényel a következő információk hello hello **integráció** lapon:

* hello hello tárolási fiók kapcsolati karakterlánc hello várólista tartalmazó hello alkalmazásbeállítás neve
* hello várólista neve
* a kód tooread hello tartalmában várólista üdvözlőüzenetére azonosítója, mint hello `order`.

toowrite tooAzure Table Storage egy kimeneti kötése a következő adatok hello használata:

* hello hello tárolási fiók kapcsolati karakterlánc hello tábla tartalmazó hello alkalmazásbeállítás neve
* hello tábla neve
* a kód toocreate hello azonosító kimeneti elemek vagy hello visszatérési érték a hello függvény.

Kapcsolati karakterláncok tooenforce hello bevált gyakorlat az, hogy inkább kötések Alkalmazásbeállítások *function.json* szolgáltatás titkos kulcsokat tartalmaz.

Ezt követően a toointegrate az Azure Storage a kódban megadott hello-azonosítók használata.

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write tooTable Storage
// hello method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

```javascript
// From an incoming queue message that is a JSON object, add fields and write tooTable Storage
// hello second parameter toocontext.done is used as hello value for hello new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

Íme hello *function.json* , amely megfelel a kód megelőző toohello. Vegye figyelembe, hogy hello azonos konfigurációval is használható, függetlenül hello függvény végrehajtása hello nyelvét.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```
tooview és Szerkesztés hello tartalmát *function.json* hello Azure-portálon, kattintson a hello **speciális szerkesztő** hello beállítást **integráció** lapon, a függvény.

További példákat és részleteinek integrálása az Azure Storage: [Azure Functions eseményindítók és kötések az Azure Storage](functions-bindings-storage.md).

### <a name="binding-direction"></a>Kötési iránya

Az összes eseményindítók és kötések vannak egy `direction` tulajdonság:

- Az eseményindítók hello irány mindig van kapcsolva`in`
- Bemeneti és kimeneti kötések használhatják `in` és`out`
- Néhány kötések támogatja a speciális paraméterirányt `inout`. Ha `inout`, csak hello **speciális szerkesztő** érhető el hello **integráció** lapon.

## <a name="using-hello-function-return-type-tooreturn-a-single-output"></a>Hello függvény visszatérési típusa tooreturn egyetlen kimeneti használatával

hello előző példa bemutatja, hogyan toouse hello függvény visszatérési értéke tooprovide kimeneti tooa kötést, amely speciális hello paraméter használatával `$return`. (Ez csak akkor támogatott a nyelveket, amelyeken a visszatérési érték, például a C#, JavaScript és F #.) Ha egy függvény több kimeneti kötése, `$return` hello kimeneti kötések csak az egyik. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

alább megjelenítése hello példák hogyan visszatérési típusok használhatók kimeneti kötések C#, JavaScript és F #.

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```javascript
// JavaScript: return a value in hello second parameter toocontext.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="binding-datatype-property"></a>Kötés dataType tulajdonsága

A .NET a bemeneti adatok a hello típusok toodefine hello adattípust használja. Használja például a `string` toobind toohello szöveg várólista eseményindító és egy bájt tömb tooread bináris formában.

A dinamikusan begépelt például JavaScript nyelven, használja a hello `dataType` hello kötés definícióban tulajdonság. Például tooread hello bináris formátumú HTTP-kérelem tartalma, hello típust használjon `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Más beállításokat a `dataType` vannak `stream` és `string`.

## <a name="resolving-app-settings"></a>Alkalmazásbeállítások feloldása
Ajánlott eljárásként titkos kulcsok és a kapcsolati karakterláncok használatával kell irányítani Alkalmazásbeállítások, nem pedig konfigurációs fájlok. Ez korlátozza a hozzáférést toothese titkos kulcsokat, és lehetővé teszi az biztonságos toostore *function.json* a egy nyilvános verziókövetési tárházzal.

Alkalmazásbeállítások is hasznosak, ha azt szeretné, hogy a hello környezete alapján toochange konfigurációs. Például egy tesztkörnyezetben, érdemes lehet toomonitor különböző várólista vagy a blob storage tárolót.

Alkalmazásbeállítások fakadó problémák megoldásával, amikor egy érték szimpla százalékjelek, például a `%MyAppSetting%`. Vegye figyelembe, hogy hello `connection` eseményindítók és kötések tulajdonsága egy különleges esetben, és automatikusan feloldja az értékeket, ha az alkalmazás beállításait. 

hello alábbi példa: egy sor eseményindító Alkalmazásbeállítás használó `%input-queue-name%` toodefine hello várólista tootrigger meg.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

## <a name="trigger-metadata-properties"></a>Eseményindító metaadat-tulajdonságainak

Sok eseményindítók hozzáadása a trigger (például egy olyan függvényt kiváltó hello üzenetsor) által biztosított toohello adatokat tartalmaz, adja meg a további metaadatokat értékét. Ezeket az értékeket a C# és F # vagy hello tulajdonságainak bemeneti paraméter használható `context.bindings` JavaScript objektumban. 

Például egy várólista eseményindító támogatja hello következő tulajdonságai:

* QueueTrigger - indítására üzenet tartalmát, ha egy érvényes karakterláncot
* DequeueCount
* ExpirationTime
* Azonosító
* InsertionTime
* NextVisibleTime
* PopReceipt

Hello megfelelő referencia-témakör ismerteti a metaadat-tulajdonságainak minden eseményindító részleteit. Dokumentáció is rendelkezésre áll, a hello **integráció** hello portál, a hello lapján **dokumentáció** hello kötés konfigurációs terület szakaszban olvashatók.  

Például, mivel a blob eseményindítók késedelmes rendelkeznek, egy várólista eseményindító toorun a funkció használata (lásd: [Blob Storage eseményindító](functions-bindings-storage-blob.md#storage-blob-trigger). várólista üdvözlőüzenetére hello blob fájlnév tootrigger tartalmazná a. Hello segítségével `queueTrigger` metaadat-tulajdonságnak adhat meg ezt a viselkedést összes konfigurációjáról, nem pedig a kódot.

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Egy metaadat-tulajdonságot is használható egy *kötési kifejezés* egy másik kötés, mint a következő szakaszban ismertetett hello.

## <a name="binding-expressions-and-patterns"></a>Kötelező kifejezések és minták

Egyik leghatékonyabb szolgáltatása hello eseményindítók és kötések *kötési kifejezésként*. A kötés belül mintát kifejezések, amelyek ezután felhasználhatók adhat meg más kötésekben vagy a kód. Eseményindító metaadatait is használható a kötési kifejezésként, mint az előző szakaszban hello hello mintában megjelenítése.

Tegyük fel például, azt szeretné, hogy az adott blob storage tárolót, hasonló toohello tooresize képek **kép méret** hello sablon **új függvény** lap. Nyissa meg túl**új függvény** -> nyelvi **C#** forgatókönyv -> **minták** -> **ImageResizer-c Sharp**. 

Íme hello *function.json* definíciója:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

Figyelje meg, hogy hello `filename` paraméter van megadva a egyaránt hello blob eseményindító definícióját, valamint hello blob kimeneti kötése. Ez a paraméter funkciókódot is használható.

```csharp
// C# example of binding too{filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->


### <a name="random-guids"></a>Véletlenszerű GUID azonosítók
Az Azure Functions kényelmi szintaxist tartalmaz a kötések hello keresztül a GUID-EK létrehozásának `{rand-guid}` kötési kifejezés. hello következő példában a toogenerate egy egyedi blob neve: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Aktuális idő

Hello kötési kifejezés használható `DateTime`, amely feloldja túl`DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-toocustom-input-properties-in-a-binding-expression"></a>Kötési kifejezésekben toocustom bemeneti tulajdonságai más elemekhez köthetők

Kötési kifejezésként is hivatkozhat hello eseményindító hasznos maga definiált tulajdonságai. Például érdemes lehet toodynamically bind tooa blob storage fájlt egy olyan webhook megadott fájlnév.

Például a következő hello *function.json* tulajdonságot használja `BlobName` a hello eseményindító hasznos:

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson",
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

tooaccomplish Ez a C# és F #, meg kell adnia egy POCO, amely meghatározza a deszerializálása hello mezők hello eseményindító tartalmaz.

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

A JavaScript JSON-deszerializálás automatikusan történik, és közvetlenül hello tulajdonságok használhatók.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

## <a name="configuring-binding-data-at-runtime"></a>Futásidőben kötés adatok konfigurálása

C# és egyéb .NET nyelven, akkor használható egy imperatív kötés mintát deklaratív kötések megakadályozását toohello *function.json*. Imperatív kötés akkor hasznos, ha a kötési paraméterekhez kell tervezési helyett futásidejű időpontban számított toobe. toolearn több, lásd: [imperatív kötéseken keresztül futásidőben kötés](functions-reference-csharp.md#imperative-bindings) hello C# fejlesztői útmutatóban.

## <a name="next-steps"></a>Következő lépések
Egy adott kötés további információkért tekintse meg a következő cikkek hello:

- [HTTP és webhookok](functions-bindings-http-webhook.md)
- [Időzítő](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Event Hub](functions-bindings-event-hubs.md)
- [Szolgáltatásbusz](functions-bindings-service-bus.md)
- [Cosmos DB](functions-bindings-documentdb.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Értesítési központ](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Külső fájl](functions-bindings-external-file.md)

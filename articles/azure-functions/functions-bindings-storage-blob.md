---
title: "aaaAzure funkciók Blob Storage kötések |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Storage eseményindítók és kötések az Azure Functions."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: aba8976c-6568-4ec7-86f5-410efd6b0fb9
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: cef44bd2154d0b97cca9220b6c5024a5b620c80d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-blob-storage-bindings"></a>Az Azure Functions Blob tároló kötések
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk azt ismerteti, hogyan tooconfigure és dolgozhat Azure Blob storage kötések Azure Functions a. Az Azure Functions támogatja eseményindító, adjon meg, és kimeneti Azure Blob Storage tárolóban kötéseit. Elérhető összes kötését lásd [Azure Functions eseményindítók és kötések fogalmak](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> A [blob csak a storage-fiók](../storage/common/storage-create-storage-account.md#blob-storage-accounts) nem támogatott. A BLOB storage eseményindítók és kötések általános célú tárfiók szükséges. 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a>A BLOB storage eseményindítók és kötések

Hello Azure Blob storage eseményindítót használ, a függvény kódot nevezik egy új vagy frissített blob észlelése esetén. hello blob tartalmát vannak megadva, a bemeneti toohello függvény.

Adja meg a blob storage eseményindító hello segítségével **integráció** hello Functions portálra a lap. hello portál létrehoz egyet a következő hello definíciójában hello **kötések** szakasza *function.json*:

```json
{
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container toomonitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

A BLOB bemeneti és kimeneti kötések meghatározása `blob` hello kötés típusa:

```json
{
  "name": "<hello name used tooidentify hello blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* Hello `path` tulajdonság által támogatott kötelező kifejezések és a szűrő paramétereit. Lásd: [minták neve](#pattern).
* Hello `connection` tulajdonságának tartalmaznia kell egy tárolási kapcsolati karakterlánc tartalmazó Alkalmazásbeállítás hello nevét. Hello Azure-portálon, a hello hello a szokásos szerkesztő **integráció** lapon konfigurálja az Alkalmazásbeállítás meg, amikor kiválaszt egy tárfiókot.

> [!NOTE]
> Egy blob eseményindító használatakor a fogyasztás terven lehet tooa 10 perces késleltetést mentése új blobok feldolgozása után egy függvény app inaktív állapotba került. Hello függvény alkalmazás futtatása után blobok feldolgozása azonnal megtörténik. a kezdeti tooavoid késleltetés, javasoljuk, hogy hello alábbi beállítások egyikét:
> - Használja az App Service-csomag a mindig engedélyezve van.
> - Használjon egy másik mechanizmust tootrigger hello blob feldolgozására, például egy üzenetsor-üzenetet, amely hello blob nevét tartalmazza. Egy vonatkozó példáért lásd: [várólista eseményindító blob bemeneti kötése](#input-sample).

<a name="pattern"></a>

### <a name="name-patterns"></a>Név minták
Megadhat egy blob mintát hello `path` tulajdonság, amely lehet egy szűrőt, vagy kötési kifejezés. Lásd: [kötelező kifejezések és minták](functions-triggers-bindings.md#binding-expressions-and-patterns).

Például a toofilter tooblobs "eredeti" hello karakterlánccal kezdődő definícióját a következő hello használja. Az elérési út keresése nevű blob *eredeti-Blob1.txt* a hello *bemeneti* tároló és a hello hello értékének `name` függvény a kódban a változó `Blob1`.

```json
"path": "input/original-{name}",
```

toobind toohello blob nevét és kiterjesztését külön-külön, használja a két mintát. Ezt az elérési utat is talál nevű blob *eredeti-Blob1.txt*, és hello hello értékének `blobname` és `blobextension` változók a funkciókódot *eredeti-Blob1* és *txt*.

```json
"path": "input/{blobname}.{blobextension}",
```

Korlátozhatja, hogy a BLOB típusú hello fájl hello fájlnévkiterjesztéshez rögzített érték használatával. Például tootrigger csak a .png fájlok, a következő mintát használja hello:

```json
"path": "samples/{name}.png",
```

Kapcsos zárójelek speciális karakterek a mintában. toospecify blob neve kapcsos zárójelek hello nevében, is escape hello kapcsos zárójeleket két kell használni. hello következő példát talál nevű blob *{20140101}-soundfile.mp3* a hello *képek* tároló és a hello `name` változó hello funkciókódot érték  *soundfile.mp3*. 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a>Eseményindító metaadatok

hello blob eseményindító biztosít több metaadat-tulajdonságot. Ezeket a tulajdonságokat meg más kötésekben kötések kifejezések részeként vagy a kód paramétereiben használható. Ezekkel az értékekkel rendelkezik hello azonos szemantikákkal, [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).

- **BlobTrigger**. Gépelje be: `string`. hello eseményindító blob elérési út
- **URI**. Gépelje be: `System.Uri`. elsődleges hely hello hello blob URI-Azonosítóját.
- **Tulajdonságok**. Gépelje be: `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`. hello blob tulajdonságai.
- **Metaadatok**. Gépelje be: `IDictionary<string,string>`. hello felhasználó által definiált metaadatok hello BLOB.

<a name="receipts"></a>

### <a name="blob-receipts"></a>A BLOB visszaigazolások
hello Azure Functions futásidejű biztosítja, hogy nincs blob funkció egynél többször meghívása megtörténik a hello azonos új vagy frissített blob. Ha egy adott blobverzió feldolgozása toodetermine, tart fenn *blob-visszaigazolások*.

Az Azure Functions tárolók blob-visszaigazolások nevű tároló *azure-webjobs-állomások* az Azure storage-fiók alkalmazás függvény hello (hello Alkalmazásbeállítás által meghatározott `AzureWebJobsStorage`). Egy blob fogadását a következő információk hello rendelkezik:

* hello indított függvény ("*&lt;függvény alkalmazás neve >*. Működik.  *&lt;függvény neve >*", például:"MyFunctionApp.Functions.CopyBlob")
* hello tároló neve
* hello blob típushoz ("BlockBlob" vagy "PageBlob")
* hello blob neve
* hello ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")

egy blob újrafeldolgozása tooforce hello blob fogadását, hogy a BLOB törlése hello *azure-webjobs-állomások* tároló manuálisan.

<a name="poison"></a>

### <a name="handling-poison-blobs"></a>Az elhalt blobok kezelése
Ha egy adott BLOB egy blob funkció nem sikerül, az Azure Functions újrapróbálkozik, amelyek összesen 5 alkalommal alapértelmezés szerint. 

Minden 5 próbálkozás sikertelen lesz, ha az Azure Functions hozzáadja egy tooa tárolási üzenetsor nevű *webjobs-blobtrigger-poison*. az elhalt blobok várólista üdvözlőüzenetére egy JSON-objektum, amely tartalmazza a következő tulajdonságai hello:

* FunctionId (hello formátumban  *&lt;függvény alkalmazás neve >*. Működik.  *&lt;függvény neve >*)
* BlobType ("BlockBlob" vagy "PageBlob")
* ContainerName
* Blobnév
* ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")

### <a name="blob-polling-for-large-containers"></a>Ciklikus lekérdezés a nagyméretű tárolók BLOB
Ha a figyelt hello blobtárolóban több mint 10 000 blobot tartalmaz, hello funkciók futásidejű vizsgálatok napló fájlok toowatch új vagy módosított blobot. Ez a folyamat nincs valós időben. Egy olyan függvényt előfordulhat, hogy nem beolvasása indulnak el, amíg több percig vagy már hello blob létrehozása után. Emellett [tárolási naplófájlokat hoz létre a lehető legjobb rendezését,"a](/rest/api/storageservices/About-Storage-Analytics-Logging) alapján. Nincs nem garantálja, hogy a rendszer rögzíti-e az összes esemény. Bizonyos körülmények között a naplók kimaradhatnak. Ha a gyorsabb és megbízhatóbb blob feldolgozási van szüksége, érdemes létrehozni egy [üzenetsor](../storage/queues/storage-dotnet-how-to-use-queues.md) hello blob létrehozásakor. Ezt követően egy [várólista eseményindító](functions-bindings-storage-queue.md) blob eseményindító tooprocess hello blob helyett.

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a>Egy blob eseményindítót használ, és a bemeneti kötése
.NET-es függvényeket, férnek hozzá, mint egy metódus paraméter használatával hello Blobadatok `Stream paramName`. Itt `paramName` hello megadott hello érték [eseményindító konfigurációs](#trigger). A Node.js funkciók hozzáférés hello bemeneti blob használatával végzett `context.bindings.<name>`.

A .NET az alábbi listában hello kötni az tooany hello típusú. Ha egy bemeneti kötése, néhány, a következő típusú szükséges egy `inout` irányban kötés *function.json*. Ebben az irányban nem támogatja hello szokásos szerkesztő, így speciális szerkesztő hello kell használni.

* `TextReader`
* `Stream`
* `ICloudBlob`(a "inout" kötés irányát igényel)
* `CloudBlockBlob`(a "inout" kötés irányát igényel)
* `CloudPageBlob`(a "inout" kötés irányát igényel)
* `CloudAppendBlob`(a "inout" kötés irányát igényel)

Szöveges BLOB várható, ha is köthető tooa .NET `string` típusa. Ez csak akkor javasolt, ha hello blob mérete kisebb, mint hello teljes blob tartalmát a memóriába betöltött. Általában akkor érdemes toouse egy `Stream` vagy `CloudBlockBlob` típusa.

## <a name="trigger-sample"></a>Eseményindító minta
Tegyük fel, amely meghatározza egy blob storage eseményindító function.json következő hello:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccount"
        }
    ]
}
```

Lásd: hello nyelvspecifikus mintát, amely minden egyes blob toohello figyelt tároló hozzáadott hello tartalmát naplózza.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a>A BLOB eseményindító példákban a C# #

```cs
// Blob trigger sample using a Stream binding
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

```cs
// Blob trigger binding tooa CloudBlockBlob
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

<a name="triggernodejs"></a>

### <a name="trigger-example-in-nodejs"></a>A node.js eseményindító – példa

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a>Egy blob használatával kimeneti kötése

A .NET-es függvényeket, akkor vagy használja a `out string` a függvényaláíráshoz vagy használja a következő lista hello hello típusú paraméter. A Node.js funkciókat érheti el hello kimeneti blob használatával `context.bindings.<name>`.

A .NET-es függvényeket a kimenetre küldheti a következő típusok hello tooany:

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a>Várólista eseményindító blob bemeneti és kimeneti minta
Tegyük fel, hogy a következő function.json hello rendelkezik, amely meghatározza egy [Queue Storage eseményindító](functions-bindings-storage-queue.md), a blob-tároló bemeneti és kimeneti blob-tároló. Értesítés hello használata hello `queueTrigger` metaadat-tulajdonságnak. a bemeneti és kimeneti hello blob `path` tulajdonságok:

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
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Lásd: hello nyelvspecifikus mintát, amely hello bemeneti blob toohello kimeneti blob másolása.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a>A BLOB kötése például a C# #

```cs
// Copy blob from input toooutput, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a>A node.js BLOB kötés – példa

```javascript
// Copy blob from input toooutput, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


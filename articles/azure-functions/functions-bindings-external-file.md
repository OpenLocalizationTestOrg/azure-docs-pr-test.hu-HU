---
title: "aaaAzure funkciók külső fájl kötések (előzetes verzió) |} Microsoft Docs"
description: "Külső fájl kötések az Azure Functions használatával"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 583d9c0b871dc68a79614749ba6ac6711fa820fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-external-file-bindings-preview"></a>Az Azure Functions külső fájl kötések (előzetes verzió)
Ez a cikk bemutatja, hogyan toomanipulate fájljainak különböző SaaS szolgáltatók (például a onedrive vállalati verzió, a Dropbox) a beépített kötések okhoz függvényen belül. Az Azure functions támogatja indítás, bemeneti és kimeneti külső fájl kötései.

A kötés tooSaaS szolgáltatók API-kapcsolatot hoz létre, vagy használja a meglévő API-kapcsolatok a függvény App erőforráscsoportból.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a>Támogatott fájl kapcsolatok

|összekötő|Eseményindító|Input (Bemenet)|Kimenet|
|:-----|:---:|:---:|:---:|
|[Mezőbe](https://www.box.com)|x|x|x
|[Dropbox-bA](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp)|x|x|x
|[Onedrive vállalati verzió](https://onedrive.live.com)|x|x|x
|[OneDrive Vállalati verzió](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google-meghajtó](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Külső fájl kapcsolatok is használható a [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)

## <a name="external-file-trigger-binding"></a>Külső fájl kötés indítás

hello Azure külső fájl eseményindító lehetővé teszi a távoli mappa megfigyelése, és futtassa a funkciókódot, amikor észlel.

hello külső fájl eseményindító használja a következő hello a JSON-objektumok hello `bindings` function.json tömbje

```json
{
  "type": "apiHubFileTrigger",
  "name": "<Name of input parameter in function signature>",
  "direction": "in",
  "path": "<folder toomonitor, and optionally a name pattern - see below>",
  "connection": "<name of external file connection - see above>"
}
```
<!---
See one of hello following subheadings for more information:

* [Name patterns](#pattern)
* [File receipts](#receipts)
* [Handling poison files](#poison)
--->

<a name="pattern"></a>

### <a name="name-patterns"></a>Név minták
Megadhat egy Fájlnévminta hello `path` tulajdonság. hello Szolgáltatottszoftver-szolgáltató hivatkozott hello mappa léteznie kell.
Példák:

```json
"path": "input/original-{name}",
```

Az elérési út megkereshető egy fájlt *eredeti-File1.txt* a hello *bemeneti* mappa, és hello hello értékének `name` funkciókódot változót lenne `File1.txt`.

Egy másik példa:

```json
"path": "input/{filename}.{fileextension}",
```

Ezt az elérési utat is tenné található nevű fájl *eredeti-File1.txt*, és hello hello értékének `filename` és `fileextension` funkciókódot változók lenne *eredeti-File1* és  *txt*.

Fájlok hello fájltípusa hello fájlnévkiterjesztéshez rögzített érték használatával korlátozhatja. Példa:

```json
"path": "samples/{name}.png",
```

Ebben az esetben csak a *.png* hello fájlok *minták* mappa hello funkció.

Kapcsos zárójelek speciális karakterek a mintában. hello neve, dupla hello kapcsos zárójelek kapcsos zárójelek rendelkező toospecify fájlneveket.
Példa:

```json
"path": "images/{{20140101}}-{name}",
```

Az elérési út megkereshető egy fájlt *{20140101}-soundfile.mp3* a hello *képek* mappát, és hello `name` hello funkciókódot a változó értéke lenne *soundfile.mp3*.

<a name="receipts"></a>

<!--- ### File receipts
hello Azure Functions runtime makes sure that no external file trigger function gets called more than once for hello same new or updated file.
It does so by maintaining *file receipts* toodetermine if a given file version has been processed.

File receipts are stored in a folder named *azure-webjobs-hosts* in hello Azure storage account for your function app
(specified by hello `AzureWebJobsStorage` app setting). A file receipt has hello following information:

* hello triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "functionsf74b96f7.Functions.CopyFile")
* hello folder name
* hello file type ("BlockFile" or "PageFile")
* hello file name
* hello ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")

tooforce reprocessing of a file, delete hello file receipt for that file from hello *azure-webjobs-hosts* folder manually.
--->
<a name="poison"></a>

### <a name="handling-poison-files"></a>Az elhalt fájlokat
Ha egy külső fájl funkció nem sikerül, az Azure Functions too5 alkalommal be a függvény alapértelmezés szerint (első próbálkozás hello is beleértve) egy adott fájlhoz újrapróbálkozik.
Összes 5 próbálkozás sikertelen lesz, ha funkciókat ad hozzá egy tooa tárolási üzenetsor nevű *webjobs-apihubtrigger-poison*. várólista üdvözlőüzenetére poison fájlok egy JSON-objektum, amely tartalmazza a következő tulajdonságai hello:

* FunctionId (hello formátumban  *&lt;függvény alkalmazás neve >*. Működik.  *&lt;függvény neve >*)
* Fájltípus
* Mappanév
* Fájlnév
* ETag (például egy fájl verziójának azonosítója: "0x8D1DC6E70A277EF")


<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Eseményindító kihasználtsága
A C# függvények, akkor eszközben toohello bemeneti fájl adatainak elnevezett paraméter a függvényaláíráshoz a például `<T> <name>`.
Ha `T` adattípus hello megjeleníteni kívánt toodeserialize hello adatokat, és `paramName` megadott hello név a [JSON indítás](#trigger). A Node.js funkciókat érheti el hello bemeneti fájl használatával végzett `context.bindings.<name>`.

hello fájl is deszerializálható a következő típusok hello bármelyikét:

* Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) - fájl JSON-szerializált adatoknál lehet hasznos.
  Ha egy egyéni bemeneti típus deklarálhatja (pl. `FooType`), az Azure Functions toodeserialize hello JSON-adatokat próbál be a megadott típus.
* Karakterlánc - szöveg fájladatok esetén hasznos.

A C# funkciók is köthető a következő típusok hello tooany, és hello Functions futtatókörnyezete megkísérli deszerializálni az adott típusú hello fájladatok:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a>Eseményindító minta
Tegyük fel, hogy rendelkezik a következő function.json hello, amely meghatározza, hogy egy külső fájl eseményindító:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

Lásd: hello nyelvspecifikus minta, amelyre bejelentkezik mindegyik fájl toohello figyelt mappa hozzáadott hello tartalmát.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a>A C# eseményindító-használat #

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger usage in F# ##
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-usage-in-nodejs"></a>Node.js eseményindító használata

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a>Külső fájl bemeneti kötése
hello Azure külső fájl bemeneti kötése lehetővé teszi egy fájl, a függvény egy külső mappából toouse.

hello külső fájl bemeneti tooa függvény használja a következő hello a JSON-objektumok hello `bindings` function.json tömbje:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

Vegye figyelembe a következőket hello:

* `path`hello mappa és hello nevét kell tartalmaznia. Ha van például egy [várólista eseményindító](functions-bindings-storage-queue.md) a függvényben használható `"path": "samples-workitems/{queueTrigger}"` toopoint tooa fájl hello `samples-workitems` hello eseményindító üzenetben megadott hello fájl nevével egyező nevű mappa.   

<a name="inputusage"></a>

## <a name="input-usage"></a>Bemeneti kihasználtsága
A C# függvények, akkor eszközben toohello bemeneti fájl adatainak elnevezett paraméter a függvényaláíráshoz a például `<T> <name>`.
Ha `T` adattípus hello megjeleníteni kívánt toodeserialize hello adatokat, és `paramName` megadott hello név a [kötés bemeneti](#input). A Node.js funkciókat érheti el hello bemeneti fájl használatával végzett `context.bindings.<name>`.

hello fájl is deszerializálható a következő típusok hello bármelyikét:

* Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) - fájl JSON-szerializált adatoknál lehet hasznos.
  Ha egy egyéni bemeneti típus deklarálhatja (pl. `InputType`), az Azure Functions toodeserialize hello JSON-adatokat próbál be a megadott típus.
* Karakterlánc - szöveg fájladatok esetén hasznos.

A C# funkciók is köthető a következő típusok hello tooany, és hello Functions futtatókörnyezete megkísérli deszerializálni az adott típusú hello fájladatok:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a>Külső fájl kimeneti kötése
a kimeneti hello Azure külső fájl kötés lehetővé teszi a toowrite fájlok tooan külső mappa a függvényben.

egy függvény hello hello a JSON-objektumok a következő célokra kimeneti hello külső fájl `bindings` function.json tömbje:

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

Vegye figyelembe a következőket hello:

* `path`hello mappa neve és hello fájl neve toowrite való tartalmaznia kell. Ha van például egy [várólista eseményindító](functions-bindings-storage-queue.md) a függvényben használható `"path": "samples-workitems/{queueTrigger}"` toopoint tooa fájl hello `samples-workitems` hello eseményindító üzenetben megadott hello fájl nevével egyező nevű mappa.   

<a name="outputusage"></a>

## <a name="output-usage"></a>Kimeneti használata
A C# függvények, akkor eszközben toohello kimeneti fájl nevű hello `out` a függvényaláíráshoz a paraméter, például `out <T> <name>`, ahol `T` adattípus hello megjeleníteni kívánt tooserialize hello adatokat, és `paramName` van hello nevét, a megadott a [kimeneti kötése](#output). A Node.js funkciókat érheti el hello kimeneti fájl használatával `context.bindings.<name>`.

Toohello kimeneti fájl a következő típusok hello használatával írhat be:

* Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) – a JSON-szerializálás hasznos.
  Ha egy egyéni kimeneti típust deklarálhatja (pl. `out OutputType paramName`), az Azure Functions tooserialize objektum megkísérli a JSON-ba. Ha hello kimeneti paraméter értéke null, ha hello függvény kilép, hello Functions futtatókörnyezete null objektumot, létrehoz egy fájlt.
* Karakterlánc - (`out string paramName`) szöveges fájl adatoknál lehet hasznos. hello Functions futtatókörnyezete létrehoz egy fájlt, csak ha a karakterlánc-paraméter null értékű hello függvény kilép.

C# függvény a következő típusok hello tooany is kimenetre:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a>Bemeneti + kimeneti minta
Tegyük fel, hogy a következő function.json hello rendelkezik, amely meghatározza egy [tároló várólista eseményindító](functions-bindings-storage-queue.md), a külső fájlban adja meg, és a külső fájlban kimeneti:

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
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Lásd: hello nyelvspecifikus mintát, amely átmásolja a terjesztendő hello bemeneti fájl toohello kimeneti fájlt.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a>A C# szintaxis #

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

<!--
<a name="infsharp"></a>
### Input usage in F# ##
```fsharp

```
-->

<a name="innodejs"></a>

### <a name="usage-in-nodejs"></a>A node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

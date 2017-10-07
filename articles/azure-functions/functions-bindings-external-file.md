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
# <a name="azure-functions-external-file-bindings-preview"></a><span data-ttu-id="08727-103">Az Azure Functions külső fájl kötések (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="08727-103">Azure Functions External File bindings (Preview)</span></span>
<span data-ttu-id="08727-104">Ez a cikk bemutatja, hogyan toomanipulate fájljainak különböző SaaS szolgáltatók (például a onedrive vállalati verzió, a Dropbox) a beépített kötések okhoz függvényen belül.</span><span class="sxs-lookup"><span data-stu-id="08727-104">This article shows how toomanipulate files from different SaaS providers (e.g. OneDrive, Dropbox) within your function utilizing built-in bindings.</span></span> <span data-ttu-id="08727-105">Az Azure functions támogatja indítás, bemeneti és kimeneti külső fájl kötései.</span><span class="sxs-lookup"><span data-stu-id="08727-105">Azure functions supports trigger, input, and output bindings for external file.</span></span>

<span data-ttu-id="08727-106">A kötés tooSaaS szolgáltatók API-kapcsolatot hoz létre, vagy használja a meglévő API-kapcsolatok a függvény App erőforráscsoportból.</span><span class="sxs-lookup"><span data-stu-id="08727-106">This binding creates API connections tooSaaS providers, or uses existing API connections from your Function App's resource group.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a><span data-ttu-id="08727-107">Támogatott fájl kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="08727-107">Supported File connections</span></span>

|<span data-ttu-id="08727-108">összekötő</span><span class="sxs-lookup"><span data-stu-id="08727-108">Connector</span></span>|<span data-ttu-id="08727-109">Eseményindító</span><span class="sxs-lookup"><span data-stu-id="08727-109">Trigger</span></span>|<span data-ttu-id="08727-110">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="08727-110">Input</span></span>|<span data-ttu-id="08727-111">Kimenet</span><span class="sxs-lookup"><span data-stu-id="08727-111">Output</span></span>|
|:-----|:---:|:---:|:---:|
|[<span data-ttu-id="08727-112">Mezőbe</span><span class="sxs-lookup"><span data-stu-id="08727-112">Box</span></span>](https://www.box.com)|<span data-ttu-id="08727-113">x</span><span class="sxs-lookup"><span data-stu-id="08727-113">x</span></span>|<span data-ttu-id="08727-114">x</span><span class="sxs-lookup"><span data-stu-id="08727-114">x</span></span>|<span data-ttu-id="08727-115">x</span><span class="sxs-lookup"><span data-stu-id="08727-115">x</span></span>
|[<span data-ttu-id="08727-116">Dropbox-bA</span><span class="sxs-lookup"><span data-stu-id="08727-116">Dropbox</span></span>](https://www.dropbox.com)|<span data-ttu-id="08727-117">x</span><span class="sxs-lookup"><span data-stu-id="08727-117">x</span></span>|<span data-ttu-id="08727-118">x</span><span class="sxs-lookup"><span data-stu-id="08727-118">x</span></span>|<span data-ttu-id="08727-119">x</span><span class="sxs-lookup"><span data-stu-id="08727-119">x</span></span>
|[<span data-ttu-id="08727-120">FTP</span><span class="sxs-lookup"><span data-stu-id="08727-120">FTP</span></span>](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp)|<span data-ttu-id="08727-121">x</span><span class="sxs-lookup"><span data-stu-id="08727-121">x</span></span>|<span data-ttu-id="08727-122">x</span><span class="sxs-lookup"><span data-stu-id="08727-122">x</span></span>|<span data-ttu-id="08727-123">x</span><span class="sxs-lookup"><span data-stu-id="08727-123">x</span></span>
|[<span data-ttu-id="08727-124">Onedrive vállalati verzió</span><span class="sxs-lookup"><span data-stu-id="08727-124">OneDrive</span></span>](https://onedrive.live.com)|<span data-ttu-id="08727-125">x</span><span class="sxs-lookup"><span data-stu-id="08727-125">x</span></span>|<span data-ttu-id="08727-126">x</span><span class="sxs-lookup"><span data-stu-id="08727-126">x</span></span>|<span data-ttu-id="08727-127">x</span><span class="sxs-lookup"><span data-stu-id="08727-127">x</span></span>
|[<span data-ttu-id="08727-128">OneDrive Vállalati verzió</span><span class="sxs-lookup"><span data-stu-id="08727-128">OneDrive for Business</span></span>](https://onedrive.live.com/about/business/)|<span data-ttu-id="08727-129">x</span><span class="sxs-lookup"><span data-stu-id="08727-129">x</span></span>|<span data-ttu-id="08727-130">x</span><span class="sxs-lookup"><span data-stu-id="08727-130">x</span></span>|<span data-ttu-id="08727-131">x</span><span class="sxs-lookup"><span data-stu-id="08727-131">x</span></span>
|[<span data-ttu-id="08727-132">SFTP</span><span class="sxs-lookup"><span data-stu-id="08727-132">SFTP</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|<span data-ttu-id="08727-133">x</span><span class="sxs-lookup"><span data-stu-id="08727-133">x</span></span>|<span data-ttu-id="08727-134">x</span><span class="sxs-lookup"><span data-stu-id="08727-134">x</span></span>|<span data-ttu-id="08727-135">x</span><span class="sxs-lookup"><span data-stu-id="08727-135">x</span></span>
|[<span data-ttu-id="08727-136">Google-meghajtó</span><span class="sxs-lookup"><span data-stu-id="08727-136">Google Drive</span></span>](https://www.google.com/drive/)||<span data-ttu-id="08727-137">x</span><span class="sxs-lookup"><span data-stu-id="08727-137">x</span></span>|<span data-ttu-id="08727-138">x</span><span class="sxs-lookup"><span data-stu-id="08727-138">x</span></span>|

> [!NOTE]
> <span data-ttu-id="08727-139">Külső fájl kapcsolatok is használható a [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="08727-139">External File connections can also be used in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>

## <a name="external-file-trigger-binding"></a><span data-ttu-id="08727-140">Külső fájl kötés indítás</span><span class="sxs-lookup"><span data-stu-id="08727-140">External File trigger binding</span></span>

<span data-ttu-id="08727-141">hello Azure külső fájl eseményindító lehetővé teszi a távoli mappa megfigyelése, és futtassa a funkciókódot, amikor észlel.</span><span class="sxs-lookup"><span data-stu-id="08727-141">hello Azure external file trigger lets you monitor a remote folder and run your function code when changes are detected.</span></span>

<span data-ttu-id="08727-142">hello külső fájl eseményindító használja a következő hello a JSON-objektumok hello `bindings` function.json tömbje</span><span class="sxs-lookup"><span data-stu-id="08727-142">hello external file trigger uses hello following JSON objects in hello `bindings` array of function.json</span></span>

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

### <a name="name-patterns"></a><span data-ttu-id="08727-143">Név minták</span><span class="sxs-lookup"><span data-stu-id="08727-143">Name patterns</span></span>
<span data-ttu-id="08727-144">Megadhat egy Fájlnévminta hello `path` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="08727-144">You can specify a file name pattern in hello `path` property.</span></span> <span data-ttu-id="08727-145">hello Szolgáltatottszoftver-szolgáltató hivatkozott hello mappa léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="08727-145">hello folder referenced must exist in hello SaaS provider.</span></span>
<span data-ttu-id="08727-146">Példák:</span><span class="sxs-lookup"><span data-stu-id="08727-146">Examples:</span></span>

```json
"path": "input/original-{name}",
```

<span data-ttu-id="08727-147">Az elérési út megkereshető egy fájlt *eredeti-File1.txt* a hello *bemeneti* mappa, és hello hello értékének `name` funkciókódot változót lenne `File1.txt`.</span><span class="sxs-lookup"><span data-stu-id="08727-147">This path would find a file named *original-File1.txt* in hello *input* folder, and hello value of hello `name` variable in function code would be `File1.txt`.</span></span>

<span data-ttu-id="08727-148">Egy másik példa:</span><span class="sxs-lookup"><span data-stu-id="08727-148">Another example:</span></span>

```json
"path": "input/{filename}.{fileextension}",
```

<span data-ttu-id="08727-149">Ezt az elérési utat is tenné található nevű fájl *eredeti-File1.txt*, és hello hello értékének `filename` és `fileextension` funkciókódot változók lenne *eredeti-File1* és  *txt*.</span><span class="sxs-lookup"><span data-stu-id="08727-149">This path would also find a file named *original-File1.txt*, and hello value of hello `filename` and `fileextension` variables in function code would be *original-File1* and *txt*.</span></span>

<span data-ttu-id="08727-150">Fájlok hello fájltípusa hello fájlnévkiterjesztéshez rögzített érték használatával korlátozhatja.</span><span class="sxs-lookup"><span data-stu-id="08727-150">You can restrict hello file type of files by using a fixed value for hello file extension.</span></span> <span data-ttu-id="08727-151">Példa:</span><span class="sxs-lookup"><span data-stu-id="08727-151">For example:</span></span>

```json
"path": "samples/{name}.png",
```

<span data-ttu-id="08727-152">Ebben az esetben csak a *.png* hello fájlok *minták* mappa hello funkció.</span><span class="sxs-lookup"><span data-stu-id="08727-152">In this case, only *.png* files in hello *samples* folder trigger hello function.</span></span>

<span data-ttu-id="08727-153">Kapcsos zárójelek speciális karakterek a mintában.</span><span class="sxs-lookup"><span data-stu-id="08727-153">Curly braces are special characters in name patterns.</span></span> <span data-ttu-id="08727-154">hello neve, dupla hello kapcsos zárójelek kapcsos zárójelek rendelkező toospecify fájlneveket.</span><span class="sxs-lookup"><span data-stu-id="08727-154">toospecify file names that have curly braces in hello name, double hello curly braces.</span></span>
<span data-ttu-id="08727-155">Példa:</span><span class="sxs-lookup"><span data-stu-id="08727-155">For example:</span></span>

```json
"path": "images/{{20140101}}-{name}",
```

<span data-ttu-id="08727-156">Az elérési út megkereshető egy fájlt *{20140101}-soundfile.mp3* a hello *képek* mappát, és hello `name` hello funkciókódot a változó értéke lenne *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="08727-156">This path would find a file named *{20140101}-soundfile.mp3* in hello *images* folder, and hello `name` variable value in hello function code would be *soundfile.mp3*.</span></span>

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

### <a name="handling-poison-files"></a><span data-ttu-id="08727-157">Az elhalt fájlokat</span><span class="sxs-lookup"><span data-stu-id="08727-157">Handling poison files</span></span>
<span data-ttu-id="08727-158">Ha egy külső fájl funkció nem sikerül, az Azure Functions too5 alkalommal be a függvény alapértelmezés szerint (első próbálkozás hello is beleértve) egy adott fájlhoz újrapróbálkozik.</span><span class="sxs-lookup"><span data-stu-id="08727-158">When an external file trigger function fails, Azure Functions retries that function up too5 times by default (including hello first try) for a given file.</span></span>
<span data-ttu-id="08727-159">Összes 5 próbálkozás sikertelen lesz, ha funkciókat ad hozzá egy tooa tárolási üzenetsor nevű *webjobs-apihubtrigger-poison*.</span><span class="sxs-lookup"><span data-stu-id="08727-159">If all 5 tries fail, Functions adds a message tooa Storage queue named *webjobs-apihubtrigger-poison*.</span></span> <span data-ttu-id="08727-160">várólista üdvözlőüzenetére poison fájlok egy JSON-objektum, amely tartalmazza a következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="08727-160">hello queue message for poison files is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="08727-161">FunctionId (hello formátumban  *&lt;függvény alkalmazás neve >*. Működik.  *&lt;függvény neve >*)</span><span class="sxs-lookup"><span data-stu-id="08727-161">FunctionId (in hello format *&lt;function app name>*.Functions.*&lt;function name>*)</span></span>
* <span data-ttu-id="08727-162">Fájltípus</span><span class="sxs-lookup"><span data-stu-id="08727-162">FileType</span></span>
* <span data-ttu-id="08727-163">Mappanév</span><span class="sxs-lookup"><span data-stu-id="08727-163">FolderName</span></span>
* <span data-ttu-id="08727-164">Fájlnév</span><span class="sxs-lookup"><span data-stu-id="08727-164">FileName</span></span>
* <span data-ttu-id="08727-165">ETag (például egy fájl verziójának azonosítója: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="08727-165">ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")</span></span>


<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="08727-166">Eseményindító kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="08727-166">Trigger usage</span></span>
<span data-ttu-id="08727-167">A C# függvények, akkor eszközben toohello bemeneti fájl adatainak elnevezett paraméter a függvényaláíráshoz a például `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="08727-167">In C# functions, you bind toohello input file data by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="08727-168">Ha `T` adattípus hello megjeleníteni kívánt toodeserialize hello adatokat, és `paramName` megadott hello név a [JSON indítás](#trigger).</span><span class="sxs-lookup"><span data-stu-id="08727-168">Where `T` is hello data type that you want toodeserialize hello data into, and `paramName` is hello name you specified in the [trigger JSON](#trigger).</span></span> <span data-ttu-id="08727-169">A Node.js funkciókat érheti el hello bemeneti fájl használatával végzett `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="08727-169">In Node.js functions, you access hello input file data using `context.bindings.<name>`.</span></span>

<span data-ttu-id="08727-170">hello fájl is deszerializálható a következő típusok hello bármelyikét:</span><span class="sxs-lookup"><span data-stu-id="08727-170">hello file can be deserialized into any of hello following types:</span></span>

* <span data-ttu-id="08727-171">Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) - fájl JSON-szerializált adatoknál lehet hasznos.</span><span class="sxs-lookup"><span data-stu-id="08727-171">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized file data.</span></span>
  <span data-ttu-id="08727-172">Ha egy egyéni bemeneti típus deklarálhatja (pl. `FooType`), az Azure Functions toodeserialize hello JSON-adatokat próbál be a megadott típus.</span><span class="sxs-lookup"><span data-stu-id="08727-172">If you declare a custom input type (e.g. `FooType`), Azure Functions attempts toodeserialize hello JSON data into your specified type.</span></span>
* <span data-ttu-id="08727-173">Karakterlánc - szöveg fájladatok esetén hasznos.</span><span class="sxs-lookup"><span data-stu-id="08727-173">String - useful for text file data.</span></span>

<span data-ttu-id="08727-174">A C# funkciók is köthető a következő típusok hello tooany, és hello Functions futtatókörnyezete megkísérli deszerializálni az adott típusú hello fájladatok:</span><span class="sxs-lookup"><span data-stu-id="08727-174">In C# functions, you can also bind tooany of hello following types, and hello Functions runtime attempts to deserialize hello file data using that type:</span></span>

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a><span data-ttu-id="08727-175">Eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="08727-175">Trigger sample</span></span>
<span data-ttu-id="08727-176">Tegyük fel, hogy rendelkezik a következő function.json hello, amely meghatározza, hogy egy külső fájl eseményindító:</span><span class="sxs-lookup"><span data-stu-id="08727-176">Suppose you have hello following function.json, that defines an external file trigger:</span></span>

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

<span data-ttu-id="08727-177">Lásd: hello nyelvspecifikus minta, amelyre bejelentkezik mindegyik fájl toohello figyelt mappa hozzáadott hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="08727-177">See hello language-specific sample that logs hello contents of each file that is added toohello monitored folder.</span></span>

* [<span data-ttu-id="08727-178">C#</span><span class="sxs-lookup"><span data-stu-id="08727-178">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="08727-179">Node.js</span><span class="sxs-lookup"><span data-stu-id="08727-179">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a><span data-ttu-id="08727-180">A C# eseményindító-használat</span><span class="sxs-lookup"><span data-stu-id="08727-180">Trigger usage in C#</span></span> #

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

### <a name="trigger-usage-in-nodejs"></a><span data-ttu-id="08727-181">Node.js eseményindító használata</span><span class="sxs-lookup"><span data-stu-id="08727-181">Trigger usage in Node.js</span></span>

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a><span data-ttu-id="08727-182">Külső fájl bemeneti kötése</span><span class="sxs-lookup"><span data-stu-id="08727-182">External File input binding</span></span>
<span data-ttu-id="08727-183">hello Azure külső fájl bemeneti kötése lehetővé teszi egy fájl, a függvény egy külső mappából toouse.</span><span class="sxs-lookup"><span data-stu-id="08727-183">hello Azure external file input binding enables you toouse a file from an external folder in your function.</span></span>

<span data-ttu-id="08727-184">hello külső fájl bemeneti tooa függvény használja a következő hello a JSON-objektumok hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="08727-184">hello external file input tooa function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

<span data-ttu-id="08727-185">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="08727-185">Note hello following:</span></span>

* <span data-ttu-id="08727-186">`path`hello mappa és hello nevét kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="08727-186">`path` must contain hello folder name and hello file name.</span></span> <span data-ttu-id="08727-187">Ha van például egy [várólista eseményindító](functions-bindings-storage-queue.md) a függvényben használható `"path": "samples-workitems/{queueTrigger}"` toopoint tooa fájl hello `samples-workitems` hello eseményindító üzenetben megadott hello fájl nevével egyező nevű mappa.</span><span class="sxs-lookup"><span data-stu-id="08727-187">For example, if you have a [queue trigger](functions-bindings-storage-queue.md) in your function, you can use `"path": "samples-workitems/{queueTrigger}"` toopoint tooa file in hello `samples-workitems` folder with a name that matches hello file name specified in hello trigger message.</span></span>   

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="08727-188">Bemeneti kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="08727-188">Input usage</span></span>
<span data-ttu-id="08727-189">A C# függvények, akkor eszközben toohello bemeneti fájl adatainak elnevezett paraméter a függvényaláíráshoz a például `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="08727-189">In C# functions, you bind toohello input file data by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="08727-190">Ha `T` adattípus hello megjeleníteni kívánt toodeserialize hello adatokat, és `paramName` megadott hello név a [kötés bemeneti](#input).</span><span class="sxs-lookup"><span data-stu-id="08727-190">Where `T` is hello data type that you want toodeserialize hello data into, and `paramName` is hello name you specified in the [input binding](#input).</span></span> <span data-ttu-id="08727-191">A Node.js funkciókat érheti el hello bemeneti fájl használatával végzett `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="08727-191">In Node.js functions, you access hello input file data using `context.bindings.<name>`.</span></span>

<span data-ttu-id="08727-192">hello fájl is deszerializálható a következő típusok hello bármelyikét:</span><span class="sxs-lookup"><span data-stu-id="08727-192">hello file can be deserialized into any of hello following types:</span></span>

* <span data-ttu-id="08727-193">Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) - fájl JSON-szerializált adatoknál lehet hasznos.</span><span class="sxs-lookup"><span data-stu-id="08727-193">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized file data.</span></span>
  <span data-ttu-id="08727-194">Ha egy egyéni bemeneti típus deklarálhatja (pl. `InputType`), az Azure Functions toodeserialize hello JSON-adatokat próbál be a megadott típus.</span><span class="sxs-lookup"><span data-stu-id="08727-194">If you declare a custom input type (e.g. `InputType`), Azure Functions attempts toodeserialize hello JSON data into your specified type.</span></span>
* <span data-ttu-id="08727-195">Karakterlánc - szöveg fájladatok esetén hasznos.</span><span class="sxs-lookup"><span data-stu-id="08727-195">String - useful for text file data.</span></span>

<span data-ttu-id="08727-196">A C# funkciók is köthető a következő típusok hello tooany, és hello Functions futtatókörnyezete megkísérli deszerializálni az adott típusú hello fájladatok:</span><span class="sxs-lookup"><span data-stu-id="08727-196">In C# functions, you can also bind tooany of hello following types, and hello Functions runtime attempts to deserialize hello file data using that type:</span></span>

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a><span data-ttu-id="08727-197">Külső fájl kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="08727-197">External File output binding</span></span>
<span data-ttu-id="08727-198">a kimeneti hello Azure külső fájl kötés lehetővé teszi a toowrite fájlok tooan külső mappa a függvényben.</span><span class="sxs-lookup"><span data-stu-id="08727-198">hello Azure external file output binding enables you toowrite files tooan external folder in your function.</span></span>

<span data-ttu-id="08727-199">egy függvény hello hello a JSON-objektumok a következő célokra kimeneti hello külső fájl `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="08727-199">hello external file output for a function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

<span data-ttu-id="08727-200">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="08727-200">Note hello following:</span></span>

* <span data-ttu-id="08727-201">`path`hello mappa neve és hello fájl neve toowrite való tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="08727-201">`path` must contain hello folder name and hello file name toowrite to.</span></span> <span data-ttu-id="08727-202">Ha van például egy [várólista eseményindító](functions-bindings-storage-queue.md) a függvényben használható `"path": "samples-workitems/{queueTrigger}"` toopoint tooa fájl hello `samples-workitems` hello eseményindító üzenetben megadott hello fájl nevével egyező nevű mappa.</span><span class="sxs-lookup"><span data-stu-id="08727-202">For example, if you have a [queue trigger](functions-bindings-storage-queue.md) in your function, you can use `"path": "samples-workitems/{queueTrigger}"` toopoint tooa file in hello `samples-workitems` folder with a name that matches hello file name specified in hello trigger message.</span></span>   

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="08727-203">Kimeneti használata</span><span class="sxs-lookup"><span data-stu-id="08727-203">Output usage</span></span>
<span data-ttu-id="08727-204">A C# függvények, akkor eszközben toohello kimeneti fájl nevű hello `out` a függvényaláíráshoz a paraméter, például `out <T> <name>`, ahol `T` adattípus hello megjeleníteni kívánt tooserialize hello adatokat, és `paramName` van hello nevét, a megadott a [kimeneti kötése](#output).</span><span class="sxs-lookup"><span data-stu-id="08727-204">In C# functions, you bind toohello output file by using hello named `out` parameter in your function signature, like `out <T> <name>`, where `T` is hello data type that you want tooserialize hello data into, and `paramName` is hello name you specified in the [output binding](#output).</span></span> <span data-ttu-id="08727-205">A Node.js funkciókat érheti el hello kimeneti fájl használatával `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="08727-205">In Node.js functions, you access hello output file using `context.bindings.<name>`.</span></span>

<span data-ttu-id="08727-206">Toohello kimeneti fájl a következő típusok hello használatával írhat be:</span><span class="sxs-lookup"><span data-stu-id="08727-206">You can write toohello output file using any of hello following types:</span></span>

* <span data-ttu-id="08727-207">Bármely [objektum](https://msdn.microsoft.com/library/system.object.aspx) – a JSON-szerializálás hasznos.</span><span class="sxs-lookup"><span data-stu-id="08727-207">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialization.</span></span>
  <span data-ttu-id="08727-208">Ha egy egyéni kimeneti típust deklarálhatja (pl. `out OutputType paramName`), az Azure Functions tooserialize objektum megkísérli a JSON-ba.</span><span class="sxs-lookup"><span data-stu-id="08727-208">If you declare a custom output type (e.g. `out OutputType paramName`), Azure Functions attempts tooserialize object into JSON.</span></span> <span data-ttu-id="08727-209">Ha hello kimeneti paraméter értéke null, ha hello függvény kilép, hello Functions futtatókörnyezete null objektumot, létrehoz egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="08727-209">If hello output parameter is null when hello function exits, hello Functions runtime creates a file as a null object.</span></span>
* <span data-ttu-id="08727-210">Karakterlánc - (`out string paramName`) szöveges fájl adatoknál lehet hasznos.</span><span class="sxs-lookup"><span data-stu-id="08727-210">String - (`out string paramName`) useful for text file data.</span></span> <span data-ttu-id="08727-211">hello Functions futtatókörnyezete létrehoz egy fájlt, csak ha a karakterlánc-paraméter null értékű hello függvény kilép.</span><span class="sxs-lookup"><span data-stu-id="08727-211">hello Functions runtime creates a file only if the string parameter is non-null when hello function exits.</span></span>

<span data-ttu-id="08727-212">C# függvény a következő típusok hello tooany is kimenetre:</span><span class="sxs-lookup"><span data-stu-id="08727-212">In C# functions you can also output tooany of hello following types:</span></span>

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a><span data-ttu-id="08727-213">Bemeneti + kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="08727-213">Input + Output sample</span></span>
<span data-ttu-id="08727-214">Tegyük fel, hogy a következő function.json hello rendelkezik, amely meghatározza egy [tároló várólista eseményindító](functions-bindings-storage-queue.md), a külső fájlban adja meg, és a külső fájlban kimeneti:</span><span class="sxs-lookup"><span data-stu-id="08727-214">Suppose you have hello following function.json, that defines a [Storage queue trigger](functions-bindings-storage-queue.md), an external file input, and an external file output:</span></span>

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

<span data-ttu-id="08727-215">Lásd: hello nyelvspecifikus mintát, amely átmásolja a terjesztendő hello bemeneti fájl toohello kimeneti fájlt.</span><span class="sxs-lookup"><span data-stu-id="08727-215">See hello language-specific sample that copies hello input file toohello output file.</span></span>

* [<span data-ttu-id="08727-216">C#</span><span class="sxs-lookup"><span data-stu-id="08727-216">C#</span></span>](#incsharp)
* [<span data-ttu-id="08727-217">Node.js</span><span class="sxs-lookup"><span data-stu-id="08727-217">Node.js</span></span>](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a><span data-ttu-id="08727-218">A C# szintaxis</span><span class="sxs-lookup"><span data-stu-id="08727-218">Usage in C#</span></span> #

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

### <a name="usage-in-nodejs"></a><span data-ttu-id="08727-219">A node.js</span><span class="sxs-lookup"><span data-stu-id="08727-219">Usage in Node.js</span></span>

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="08727-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="08727-220">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

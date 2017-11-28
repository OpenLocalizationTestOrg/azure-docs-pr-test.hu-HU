---
title: "az Azure Functions aaaJavaScript fejlesztői útmutatója |} Microsoft Docs"
description: "Ismerje meg, hogyan toodevelop működik-e a JavaScript használatával."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "azure-függvények, függvények, eseményfeldolgozás, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: functions
ms.devlang: nodejs
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: 6220b42f965b6ee2463341aaf270836623fdf7fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-javascript-developer-guide"></a><span data-ttu-id="4ae54-104">Az Azure Functions JavaScript fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="4ae54-104">Azure Functions JavaScript developer guide</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4ae54-105">C# parancsfájl</span><span class="sxs-lookup"><span data-stu-id="4ae54-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="4ae54-106">F # parancsfájl</span><span class="sxs-lookup"><span data-stu-id="4ae54-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="4ae54-107">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4ae54-107">JavaScript</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="4ae54-108">hello JavaScript tapasztalattal az Azure Functions segítségével könnyen tooexport egy függvénynek, amely szerint átadása egy `context` objektum hello futásidejű való kommunikációhoz és fogadása és adatküldés kötéseken keresztül.</span><span class="sxs-lookup"><span data-stu-id="4ae54-108">hello JavaScript experience for Azure Functions makes it easy tooexport a function, which is passed as a `context` object for communicating with hello runtime and for receiving and sending data via bindings.</span></span>

<span data-ttu-id="4ae54-109">Ez a cikk feltételezi, hogy Ön már elolvasta hello [Azure Functions fejlesztői segédanyagai](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="4ae54-109">This article assumes that you've already read hello [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="exporting-a-function"></a><span data-ttu-id="4ae54-110">Egy függvény exportálása</span><span class="sxs-lookup"><span data-stu-id="4ae54-110">Exporting a function</span></span>
<span data-ttu-id="4ae54-111">Minden JavaScript-funkcióként kell exportálnia az egyetlen `function` keresztül `module.exports` hello futási időben toofind hello függvény, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="4ae54-111">All JavaScript functions must export a single `function` via `module.exports` for hello runtime toofind hello function and run it.</span></span> <span data-ttu-id="4ae54-112">Ez a funkció mindig tartalmaznia kell egy `context` objektum.</span><span class="sxs-lookup"><span data-stu-id="4ae54-112">This function must always include a `context` object.</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by hello arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

<span data-ttu-id="4ae54-113">Kötések `direction === "in"` függvény argumentumaként, ami azt jelenti, amelyeket felhasználhat továbbítódnak [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) toodynamically kezelni új bemeneti adatokat (például használatával `arguments.length` tooiterate keresztül a bemeneti adatok).</span><span class="sxs-lookup"><span data-stu-id="4ae54-113">Bindings of `direction === "in"` are passed along as function arguments, which means that you can use [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) toodynamically handle new inputs (for example, by using `arguments.length` tooiterate over all your inputs).</span></span> <span data-ttu-id="4ae54-114">Ez a funkció esetén kényelmes csak egy eseményindító és nincs további bevitel, mivel nélkül hivatkozó kiszámítható hozzáfér az eseményindítóra vonatkozó információt a `context` objektum.</span><span class="sxs-lookup"><span data-stu-id="4ae54-114">This functionality is convenient when you have only a trigger and no additional inputs, because you can predictably access your trigger data without referencing your `context` object.</span></span>

<span data-ttu-id="4ae54-115">hello argumentumok mindig továbbítódnak toohello függvény hello sorrendben történik a *function.json*, még akkor is, ha nem adja meg azokat a kivitel utasításban.</span><span class="sxs-lookup"><span data-stu-id="4ae54-115">hello arguments are always passed along toohello function in hello order in which they occur in *function.json*, even if you don't specify them in your exports statement.</span></span> <span data-ttu-id="4ae54-116">Például, ha van `function(context, a, b)` módosítsa azt túl`function(context, a)`, továbbra is használhatja a hello értékének `b` túl hivatkozással függvény kódban`arguments[3]`.</span><span class="sxs-lookup"><span data-stu-id="4ae54-116">For example, if you have `function(context, a, b)` and change it too`function(context, a)`, you can still get hello value of `b` in function code by referring too`arguments[3]`.</span></span>

<span data-ttu-id="4ae54-117">Összes kötését, irány, attól függetlenül is továbbítódnak a hello `context` (lásd a következő parancsfájl hello) objektum.</span><span class="sxs-lookup"><span data-stu-id="4ae54-117">All bindings, regardless of direction, are also passed along on hello `context` object (see hello following script).</span></span> 

## <a name="context-object"></a><span data-ttu-id="4ae54-118">környezeti objektumot</span><span class="sxs-lookup"><span data-stu-id="4ae54-118">context object</span></span>
<span data-ttu-id="4ae54-119">hello futásidejű használ egy `context` objektum toopass adatok tooand a függvény és kommunikálhat a hello futásidejű toolet.</span><span class="sxs-lookup"><span data-stu-id="4ae54-119">hello runtime uses a `context` object toopass data tooand from your function and toolet you communicate with hello runtime.</span></span>

<span data-ttu-id="4ae54-120">hello környezeti objektumot mindig hello első tooa függvény és szerepeljen, mert a metódusok, mint `context.done` és `context.log`, szükséges toouse hello futásidejű ezek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="4ae54-120">hello context object is always hello first parameter tooa function and must be included because it has methods such as `context.done` and `context.log`, which are required toouse hello runtime correctly.</span></span> <span data-ttu-id="4ae54-121">Nevére hello objektum függetlenül szeretné (például `ctx` vagy `c`).</span><span class="sxs-lookup"><span data-stu-id="4ae54-121">You can name hello object whatever you would like (for example, `ctx` or `c`).</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a><span data-ttu-id="4ae54-122">Context.Bindings tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4ae54-122">context.bindings property</span></span>

```
context.bindings
```
<span data-ttu-id="4ae54-123">Egy elnevezett objektum beállítása/beolvasása, amely a bemeneti és kimeneti adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4ae54-123">Returns a named object that contains all your input and output data.</span></span> <span data-ttu-id="4ae54-124">Például a következő kötés-definíciójában hello a *function.json* megadható, hogy van-hozzáférési hello hello hello várólista tartalmát `context.bindings.myInput` objektum.</span><span class="sxs-lookup"><span data-stu-id="4ae54-124">For example, hello following binding definition in your *function.json* lets you access hello contents of hello queue from hello `context.bindings.myInput` object.</span></span> 

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
}
```

```javascript
// myInput contains hello input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

### <a name="contextdone-method"></a><span data-ttu-id="4ae54-125">Context.Done módszer</span><span class="sxs-lookup"><span data-stu-id="4ae54-125">context.done method</span></span>
```
context.done([err],[propertyBag])
```

<span data-ttu-id="4ae54-126">Arról tájékoztatja, hogy a kód befejezte hello futásidejű.</span><span class="sxs-lookup"><span data-stu-id="4ae54-126">Informs hello runtime that your code has finished.</span></span> <span data-ttu-id="4ae54-127">Meg kell hívnia `context.done`, vagy más hello futásidejű soha nem tudja, hogy a függvény befejeződött, és hello végrehajtása időtúllépést okoz.</span><span class="sxs-lookup"><span data-stu-id="4ae54-127">You must call `context.done`, or else hello runtime never knows that your function is complete, and hello execution will time out.</span></span> 

<span data-ttu-id="4ae54-128">Hello `context.done` módszer lehetővé teszi, hogy toopass biztonsági másolatot a felhasználó által definiált hiba toohello futásidejű és a felülírási hello hello tulajdonságainak tulajdonságok tulajdonságcsomag `context.bindings` objektum.</span><span class="sxs-lookup"><span data-stu-id="4ae54-128">hello `context.done` method allows you toopass back both a user-defined error toohello runtime and a property bag of properties that overwrite hello properties on hello `context.bindings` object.</span></span>

```javascript
// Even though we set myOutput toohave:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object toohello done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// hello done method will overwrite hello myOutput binding toobe: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a><span data-ttu-id="4ae54-129">Context.log módszer</span><span class="sxs-lookup"><span data-stu-id="4ae54-129">context.log method</span></span>  

```
context.log(message)
```
<span data-ttu-id="4ae54-130">Lehetővé teszi a toowrite toohello konzol streamnaplókba hello alapértelmezett nyomkövetési szinten.</span><span class="sxs-lookup"><span data-stu-id="4ae54-130">Allows you toowrite toohello streaming console logs at hello default trace level.</span></span> <span data-ttu-id="4ae54-131">A `context.log`, további naplózási módszerek elérhetőek, amelyek lehetővé teszik a toohello konzolbeli naplóit más nyomkövetési szinten írni:</span><span class="sxs-lookup"><span data-stu-id="4ae54-131">On `context.log`, additional logging methods are available that let you write toohello console log at other trace levels:</span></span>


| <span data-ttu-id="4ae54-132">Módszer</span><span class="sxs-lookup"><span data-stu-id="4ae54-132">Method</span></span>                 | <span data-ttu-id="4ae54-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="4ae54-133">Description</span></span>                                |
| ---------------------- | ------------------------------------------ |
| <span data-ttu-id="4ae54-134">**Hiba (_üzenet_)**</span><span class="sxs-lookup"><span data-stu-id="4ae54-134">**error(_message_)**</span></span>   | <span data-ttu-id="4ae54-135">Írja a naplózás vagy alacsonyabb tooerror szintjét.</span><span class="sxs-lookup"><span data-stu-id="4ae54-135">Writes tooerror level logging, or lower.</span></span>   |
| <span data-ttu-id="4ae54-136">**Figyelmeztetés (_üzenet_)**</span><span class="sxs-lookup"><span data-stu-id="4ae54-136">**warn(_message_)**</span></span>    | <span data-ttu-id="4ae54-137">Írja a naplózás vagy alacsonyabb toowarning szintjét.</span><span class="sxs-lookup"><span data-stu-id="4ae54-137">Writes toowarning level logging, or lower.</span></span> |
| <span data-ttu-id="4ae54-138">**Info (_üzenet_)**</span><span class="sxs-lookup"><span data-stu-id="4ae54-138">**info(_message_)**</span></span>    | <span data-ttu-id="4ae54-139">Írja a naplózás vagy alacsonyabb tooinfo szintjét.</span><span class="sxs-lookup"><span data-stu-id="4ae54-139">Writes tooinfo level logging, or lower.</span></span>    |
| <span data-ttu-id="4ae54-140">**részletes (_üzenet_)**</span><span class="sxs-lookup"><span data-stu-id="4ae54-140">**verbose(_message_)**</span></span> | <span data-ttu-id="4ae54-141">Webhelyszintű naplózás tooverbose ír.</span><span class="sxs-lookup"><span data-stu-id="4ae54-141">Writes tooverbose level logging.</span></span>           |

<span data-ttu-id="4ae54-142">hello alábbi példa ír toohello konzol hello figyelmeztetés nyomkövetési szint:</span><span class="sxs-lookup"><span data-stu-id="4ae54-142">hello following example writes toohello console at hello warning trace level:</span></span>

```javascript
context.log.warn("Something has happened."); 
```
<span data-ttu-id="4ae54-143">Állítsa be a nyomkövetési szintű küszöbértéke hello hello host.json fájl van bejelentkezve, vagy kapcsolja ki.</span><span class="sxs-lookup"><span data-stu-id="4ae54-143">You can set hello trace-level threshold for logging in hello host.json file, or turn it off.</span></span>  <span data-ttu-id="4ae54-144">Hogyan naplózza az toowrite toohello kapcsolatos további információkért lásd a hello következő szakaszt.</span><span class="sxs-lookup"><span data-stu-id="4ae54-144">For more information about how toowrite toohello logs, see hello next section.</span></span>

## <a name="binding-data-type"></a><span data-ttu-id="4ae54-145">Kötési adattípus</span><span class="sxs-lookup"><span data-stu-id="4ae54-145">Binding data type</span></span>

<span data-ttu-id="4ae54-146">toodefine hello adattípus egy bemeneti kötése használja hello `dataType` hello kötés definícióban tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="4ae54-146">toodefine hello data type for an input binding, use hello `dataType` property in hello binding definition.</span></span> <span data-ttu-id="4ae54-147">Például tooread hello bináris formátumú HTTP-kérelem tartalma, hello típust használjon `binary`:</span><span class="sxs-lookup"><span data-stu-id="4ae54-147">For example, tooread hello content of an HTTP request in binary format, use hello type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="4ae54-148">Más beállításokat a `dataType` vannak `stream` és `string`.</span><span class="sxs-lookup"><span data-stu-id="4ae54-148">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="writing-trace-output-toohello-console"></a><span data-ttu-id="4ae54-149">Írás a nyomkövetési kimeneti toohello konzol</span><span class="sxs-lookup"><span data-stu-id="4ae54-149">Writing trace output toohello console</span></span> 

<span data-ttu-id="4ae54-150">A funkciók hello használja `context.log` módszerek toowrite nyomkövetési kimeneti toohello konzolt.</span><span class="sxs-lookup"><span data-stu-id="4ae54-150">In Functions, you use hello `context.log` methods toowrite trace output toohello console.</span></span> <span data-ttu-id="4ae54-151">Ezen a ponton nem használható `console.log` toowrite toohello konzol.</span><span class="sxs-lookup"><span data-stu-id="4ae54-151">At this point, you cannot use `console.log` toowrite toohello console.</span></span>

<span data-ttu-id="4ae54-152">A hívás esetén `context.log()`, az üzenet írása hello alapértelmezett nyomkövetési szint, amely hello toohello a konzol _info_ nyomkövetési szint.</span><span class="sxs-lookup"><span data-stu-id="4ae54-152">When you call `context.log()`, your message is written toohello console at hello default trace level, which is hello _info_ trace level.</span></span> <span data-ttu-id="4ae54-153">hello következő kódot ír toohello konzol hello információ a nyomkövetési szint:</span><span class="sxs-lookup"><span data-stu-id="4ae54-153">hello following code writes toohello console at hello info trace level:</span></span>

```javascript
context.log({hello: 'world'});  
```

<span data-ttu-id="4ae54-154">hello előző kód egyenértékű toohello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="4ae54-154">hello preceding code is equivalent toohello following code:</span></span>

```javascript
context.log.info({hello: 'world'});  
```

<span data-ttu-id="4ae54-155">hello következő kódot ír toohello konzol hello hiba szinten:</span><span class="sxs-lookup"><span data-stu-id="4ae54-155">hello following code writes toohello console at hello error level:</span></span>

```javascript
context.log.error("An error has occurred.");  
```

<span data-ttu-id="4ae54-156">Mivel _hiba_ hello legmagasabb nyomkövetési szint a nyomkövetési adatok íródnak toohello kimeneti minden nyomkövetési szintű mindaddig, amíg naplózás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="4ae54-156">Because _error_ is hello highest trace level, this trace is written toohello output at all trace levels as long as logging is enabled.</span></span>  


<span data-ttu-id="4ae54-157">Minden `context.log` módszerek támogatják hello azonos paraméter formátuma hello Node.js által támogatott [util.format metódus](https://nodejs.org/api/util.html#util_util_format_format).</span><span class="sxs-lookup"><span data-stu-id="4ae54-157">All `context.log` methods support hello same parameter format that's supported by hello Node.js [util.format method](https://nodejs.org/api/util.html#util_util_format_format).</span></span> <span data-ttu-id="4ae54-158">Vegye figyelembe a következő kódot, amely toohello konzol írja hello alapértelmezett nyomkövetési szint használatával hello:</span><span class="sxs-lookup"><span data-stu-id="4ae54-158">Consider hello following code, which writes toohello console by using hello default trace level:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

<span data-ttu-id="4ae54-159">További lehetőségek írási hello azonos hello formátuma a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="4ae54-159">You can also write hello same code in hello following format:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-hello-trace-level-for-console-logging"></a><span data-ttu-id="4ae54-160">Hello nyomkövetési szint konzol naplózás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4ae54-160">Configure hello trace level for console logging</span></span>

<span data-ttu-id="4ae54-161">Funkciók lehetővé teszi a nyomkövetési küszöbértéket hello toohello konzol, így könnyen toocontrol hello módon nyomkövetések az a funkciók írt toohello konzol írásra meghatározása.</span><span class="sxs-lookup"><span data-stu-id="4ae54-161">Functions lets you define hello threshold trace level for writing toohello console, which makes it easy toocontrol hello way traces are written toohello console from your functions.</span></span> <span data-ttu-id="4ae54-162">tooset hello küszöbértéke toohello konzol használata hello írt minden nyomkövetési `tracing.consoleLevel` tulajdonság a hello host.json fájlban.</span><span class="sxs-lookup"><span data-stu-id="4ae54-162">tooset hello threshold for all traces written toohello console, use hello `tracing.consoleLevel` property in hello host.json file.</span></span> <span data-ttu-id="4ae54-163">Ez a beállítás az függvény alkalmazásban tooall funkciók vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="4ae54-163">This setting applies tooall functions in your function app.</span></span> <span data-ttu-id="4ae54-164">hello alábbi mintakód hello nyomkövetési küszöbérték tooenable részletes naplózás:</span><span class="sxs-lookup"><span data-stu-id="4ae54-164">hello following example sets hello trace threshold tooenable verbose logging:</span></span>

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

<span data-ttu-id="4ae54-165">Az értékek **consoleLevel** hello toohello nevei megegyeznek `context.log` módszerek.</span><span class="sxs-lookup"><span data-stu-id="4ae54-165">Values of **consoleLevel** correspond toohello names of hello `context.log` methods.</span></span> <span data-ttu-id="4ae54-166">toodisable minden nyomkövetési naplózás toohello konzol beállítása **consoleLevel** too_off_.</span><span class="sxs-lookup"><span data-stu-id="4ae54-166">toodisable all trace logging toohello console, set **consoleLevel** too_off_.</span></span> <span data-ttu-id="4ae54-167">Hello host.json fájllal kapcsolatos további információkért lásd: hello [host.json referencia-témakör](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="4ae54-167">For more information about hello host.json file, see hello [host.json reference topic](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>

## <a name="http-triggers-and-bindings"></a><span data-ttu-id="4ae54-168">HTTP-eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="4ae54-168">HTTP triggers and bindings</span></span>

<span data-ttu-id="4ae54-169">A HTTP és a webhook eseményindítók és a HTTP kimeneti kötések kérés és válasz objektumok toorepresent hello HTTP üzenetküldési használhatják.</span><span class="sxs-lookup"><span data-stu-id="4ae54-169">HTTP and webhook triggers and HTTP output bindings use request and response objects toorepresent hello HTTP messaging.</span></span>  

### <a name="request-object"></a><span data-ttu-id="4ae54-170">Kérelem objektum</span><span class="sxs-lookup"><span data-stu-id="4ae54-170">Request object</span></span>

<span data-ttu-id="4ae54-171">Hello `request` objektumnak hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="4ae54-171">hello `request` object has hello following properties:</span></span>

| <span data-ttu-id="4ae54-172">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4ae54-172">Property</span></span>      | <span data-ttu-id="4ae54-173">Leírás</span><span class="sxs-lookup"><span data-stu-id="4ae54-173">Description</span></span>                                                    |
| ------------- | -------------------------------------------------------------- |
| <span data-ttu-id="4ae54-174">_törzs_</span><span class="sxs-lookup"><span data-stu-id="4ae54-174">_body_</span></span>        | <span data-ttu-id="4ae54-175">Egy objektum, amely tartalmazza a hello hello kérelem törzse.</span><span class="sxs-lookup"><span data-stu-id="4ae54-175">An object that contains hello body of hello request.</span></span>               |
| <span data-ttu-id="4ae54-176">_fejlécek_</span><span class="sxs-lookup"><span data-stu-id="4ae54-176">_headers_</span></span>     | <span data-ttu-id="4ae54-177">Egy objektum, amely tartalmazza a hello kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="4ae54-177">An object that contains hello request headers.</span></span>                   |
| <span data-ttu-id="4ae54-178">_módszer_</span><span class="sxs-lookup"><span data-stu-id="4ae54-178">_method_</span></span>      | <span data-ttu-id="4ae54-179">hello hello kérelem HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="4ae54-179">hello HTTP method of hello request.</span></span>                                |
| <span data-ttu-id="4ae54-180">_originalUrl_</span><span class="sxs-lookup"><span data-stu-id="4ae54-180">_originalUrl_</span></span> | <span data-ttu-id="4ae54-181">hello hello kérelem URL-címe</span><span class="sxs-lookup"><span data-stu-id="4ae54-181">hello URL of hello request.</span></span>                                        |
| <span data-ttu-id="4ae54-182">_paraméterei_</span><span class="sxs-lookup"><span data-stu-id="4ae54-182">_params_</span></span>      | <span data-ttu-id="4ae54-183">Egy objektum, amely tartalmazza a hello hello kérés útválasztási paramétereivel.</span><span class="sxs-lookup"><span data-stu-id="4ae54-183">An object that contains hello routing parameters of hello request.</span></span> |
| <span data-ttu-id="4ae54-184">_lekérdezés_</span><span class="sxs-lookup"><span data-stu-id="4ae54-184">_query_</span></span>       | <span data-ttu-id="4ae54-185">Egy objektum, amely tartalmazza a hello lekérdezési paramétereket.</span><span class="sxs-lookup"><span data-stu-id="4ae54-185">An object that contains hello query parameters.</span></span>                  |
| <span data-ttu-id="4ae54-186">_rawBody_</span><span class="sxs-lookup"><span data-stu-id="4ae54-186">_rawBody_</span></span>     | <span data-ttu-id="4ae54-187">hello üzenet karakterláncként hello törzsét.</span><span class="sxs-lookup"><span data-stu-id="4ae54-187">hello body of hello message as a string.</span></span>                           |


### <a name="response-object"></a><span data-ttu-id="4ae54-188">Válasz objektum</span><span class="sxs-lookup"><span data-stu-id="4ae54-188">Response object</span></span>

<span data-ttu-id="4ae54-189">Hello `response` objektumnak hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="4ae54-189">hello `response` object has hello following properties:</span></span>

| <span data-ttu-id="4ae54-190">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4ae54-190">Property</span></span>  | <span data-ttu-id="4ae54-191">Leírás</span><span class="sxs-lookup"><span data-stu-id="4ae54-191">Description</span></span>                                               |
| --------- | --------------------------------------------------------- |
| <span data-ttu-id="4ae54-192">_törzs_</span><span class="sxs-lookup"><span data-stu-id="4ae54-192">_body_</span></span>    | <span data-ttu-id="4ae54-193">Hello válasz hello törzsét tartalmazó objektum.</span><span class="sxs-lookup"><span data-stu-id="4ae54-193">An object that contains hello body of hello response.</span></span>         |
| <span data-ttu-id="4ae54-194">_fejlécek_</span><span class="sxs-lookup"><span data-stu-id="4ae54-194">_headers_</span></span> | <span data-ttu-id="4ae54-195">Egy objektum, amely tartalmazza a hello response fejlécekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="4ae54-195">An object that contains hello response headers.</span></span>             |
| <span data-ttu-id="4ae54-196">_isRaw_</span><span class="sxs-lookup"><span data-stu-id="4ae54-196">_isRaw_</span></span>   | <span data-ttu-id="4ae54-197">Azt jelzi, hogy formázás ki van kapcsolva hello válasz.</span><span class="sxs-lookup"><span data-stu-id="4ae54-197">Indicates that formatting is skipped for hello response.</span></span>    |
| <span data-ttu-id="4ae54-198">_állapot_</span><span class="sxs-lookup"><span data-stu-id="4ae54-198">_status_</span></span>  | <span data-ttu-id="4ae54-199">hello HTTP-állapotkód: hello válasz.</span><span class="sxs-lookup"><span data-stu-id="4ae54-199">hello HTTP status code of hello response.</span></span>                     |

### <a name="accessing-hello-request-and-response"></a><span data-ttu-id="4ae54-200">Hello kérés és válasz elérése</span><span class="sxs-lookup"><span data-stu-id="4ae54-200">Accessing hello request and response</span></span> 

<span data-ttu-id="4ae54-201">Ha HTTP-eseményindítók használata esetén elérhető hello HTTP kérelem-válasz három módokon objektumok:</span><span class="sxs-lookup"><span data-stu-id="4ae54-201">When you work with HTTP triggers, you can access hello HTTP request and response objects in any of three ways:</span></span>

+ <span data-ttu-id="4ae54-202">A hello nevű bemeneti és kimeneti kötéseket.</span><span class="sxs-lookup"><span data-stu-id="4ae54-202">From hello named input and output bindings.</span></span> <span data-ttu-id="4ae54-203">Ily módon mint bármely más kötésnél hello HTTP-eseményindítóval és kötések munkahelyi hello azonos.</span><span class="sxs-lookup"><span data-stu-id="4ae54-203">In this way, hello HTTP trigger and bindings work hello same as any other binding.</span></span> <span data-ttu-id="4ae54-204">hello alábbi mintakód hello válasz objektum használatával elnevezett `response` kötés:</span><span class="sxs-lookup"><span data-stu-id="4ae54-204">hello following example sets hello response object by using a named `response` binding:</span></span> 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ <span data-ttu-id="4ae54-205">A `req` és `res` hello tulajdonságainak `context` objektum.</span><span class="sxs-lookup"><span data-stu-id="4ae54-205">From `req` and `res` properties on hello `context` object.</span></span> <span data-ttu-id="4ae54-206">Ily módon használhatja hello hagyományos mintát tooaccess HTTP adatait hello környezeti objektumot, ahelyett, hogy teljes toouse hello `context.bindings.name` mintát.</span><span class="sxs-lookup"><span data-stu-id="4ae54-206">In this way, you can use hello conventional pattern tooaccess HTTP data from hello context object, instead of having toouse hello full `context.bindings.name` pattern.</span></span> <span data-ttu-id="4ae54-207">a következő példa azt mutatja meg hogyan hello tooaccess hello `req` és `res` hello objektumok `context`:</span><span class="sxs-lookup"><span data-stu-id="4ae54-207">hello following example shows how tooaccess hello `req` and `res` objects on hello `context`:</span></span>

    ```javascript
    // You can access your http request off hello context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ <span data-ttu-id="4ae54-208">Meghívásával `context.done()`.</span><span class="sxs-lookup"><span data-stu-id="4ae54-208">By calling `context.done()`.</span></span> <span data-ttu-id="4ae54-209">HTTP-kötés olyan különleges toohello átadott hello választ ad vissza `context.done()` metódust.</span><span class="sxs-lookup"><span data-stu-id="4ae54-209">A special kind of HTTP binding returns hello response that is passed toohello `context.done()` method.</span></span> <span data-ttu-id="4ae54-210">a következő HTTP hello kimeneti kötés határozza meg a `$return` kimeneti paraméterként:</span><span class="sxs-lookup"><span data-stu-id="4ae54-210">hello following HTTP output binding defines a `$return` output parameter:</span></span>

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    <span data-ttu-id="4ae54-211">A kimeneti kötés vár, toosupply hello válasz hívásakor `done()`, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4ae54-211">This output binding expects you toosupply hello response when you call `done()`, as follows:</span></span>

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a><span data-ttu-id="4ae54-212">Csomópont verziója és a felügyeleti csomag</span><span class="sxs-lookup"><span data-stu-id="4ae54-212">Node version and Package Management</span></span>
<span data-ttu-id="4ae54-213">hello csomópont verziója jelenleg zárolva van `6.5.0`.</span><span class="sxs-lookup"><span data-stu-id="4ae54-213">hello node version is currently locked at `6.5.0`.</span></span> <span data-ttu-id="4ae54-214">Jelenleg vizsgálja a további verziók támogatása, és így konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="4ae54-214">We're investigating adding support for more versions and making it configurable.</span></span>

<span data-ttu-id="4ae54-215">a lépéseket követve hello lehetővé teszik, hogy a csomagok tartalmazzák a függvény alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="4ae54-215">hello following steps let you include packages in your function app:</span></span> 

1. <span data-ttu-id="4ae54-216">Nyissa meg túl`https://<function_app_name>.scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="4ae54-216">Go too`https://<function_app_name>.scm.azurewebsites.net`.</span></span>

2. <span data-ttu-id="4ae54-217">Kattintson a **konzol Debug** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="4ae54-217">Click **Debug Console** > **CMD**.</span></span>

3. <span data-ttu-id="4ae54-218">Nyissa meg túl`D:\home\site\wwwroot`, majd húzza a package.json fájl toohello **wwwroot** mappát a hello hello lap felső részén.</span><span class="sxs-lookup"><span data-stu-id="4ae54-218">Go too`D:\home\site\wwwroot`, and then drag your package.json file toohello **wwwroot** folder at hello top half of hello page.</span></span>  
    <span data-ttu-id="4ae54-219">Fájlok tooyour függvény app más módokon is feltöltheti.</span><span class="sxs-lookup"><span data-stu-id="4ae54-219">You can upload files tooyour function app in other ways also.</span></span> <span data-ttu-id="4ae54-220">További információkért lásd: [hogyan tooupdate működni az alkalmazás fájljai](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="4ae54-220">For more information, see [How tooupdate function app files](functions-reference.md#fileupdate).</span></span> 

4. <span data-ttu-id="4ae54-221">Hello package.json fájl feltöltése, után futtassa a hello `npm install` hello parancsot **Kudu távoli végrehajtás konzol**.</span><span class="sxs-lookup"><span data-stu-id="4ae54-221">After hello package.json file is uploaded, run hello `npm install` command in hello **Kudu remote execution console**.</span></span>  
    <span data-ttu-id="4ae54-222">Ez a művelet hello package.json fájlban jelzett hello csomagok letölti és hello függvény alkalmazás újraindul.</span><span class="sxs-lookup"><span data-stu-id="4ae54-222">This action downloads hello packages indicated in hello package.json file and restarts hello function app.</span></span>

<span data-ttu-id="4ae54-223">Hello szükséges csomagok telepítése után importálja a fájlokat a tooyour függvény meghívásával `require('packagename')`, a példában a következő hello:</span><span class="sxs-lookup"><span data-stu-id="4ae54-223">After hello packages you need are installed, you import them tooyour function by calling `require('packagename')`, as in hello following example:</span></span>

```javascript
// Import hello underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

<span data-ttu-id="4ae54-224">Meg kell határozni egy `package.json` fájlban a következő hello legfelső szintű függvény alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="4ae54-224">You should define a `package.json` file at hello root of your function app.</span></span> <span data-ttu-id="4ae54-225">Definiáló hello fájl lehetővé teszi, hogy az összes funkciójának hello app megosztás hello azonos gyorsítótárazott csomagok, ami hello lehető legjobb teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="4ae54-225">Defining hello file lets all functions in hello app share hello same cached packages, which gives hello best performance.</span></span> <span data-ttu-id="4ae54-226">Ha verziószáma, hogyan oldható meg hozzáadásával egy `package.json` hello fájlban egy adott funkció.</span><span class="sxs-lookup"><span data-stu-id="4ae54-226">If a version conflict arises, you can resolve it by adding a `package.json` file in hello folder of a specific function.</span></span>  

## <a name="environment-variables"></a><span data-ttu-id="4ae54-227">Környezeti változók</span><span class="sxs-lookup"><span data-stu-id="4ae54-227">Environment variables</span></span>
<span data-ttu-id="4ae54-228">tooget egy környezeti változó vagy egy alkalmazás beállításérték, használjon `process.env`, ahogy az alábbi kódpéldát hello:</span><span class="sxs-lookup"><span data-stu-id="4ae54-228">tooget an environment variable or an app setting value, use `process.env`, as shown in hello following code example:</span></span>

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));

    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```
## <a name="considerations-for-javascript-functions"></a><span data-ttu-id="4ae54-229">JavaScript-funkcióként szempontjai</span><span class="sxs-lookup"><span data-stu-id="4ae54-229">Considerations for JavaScript functions</span></span>

<span data-ttu-id="4ae54-230">JavaScript-funkcióként használatakor a következő két szakasz hello hello szempontok figyelembe.</span><span class="sxs-lookup"><span data-stu-id="4ae54-230">When you work with JavaScript functions, be aware of hello considerations in hello following two sections.</span></span>

### <a name="choose-single-core-app-service-plans"></a><span data-ttu-id="4ae54-231">Válasszon egy mag App Service-csomagokról</span><span class="sxs-lookup"><span data-stu-id="4ae54-231">Choose single-core App Service plans</span></span>

<span data-ttu-id="4ae54-232">Egy függvény alkalmazás által használt hello App Service-csomag létrehozásakor azt javasoljuk, hogy kiválassza a több mag terv helyett egy egymagos terv.</span><span class="sxs-lookup"><span data-stu-id="4ae54-232">When you create a function app that uses hello App Service plan, we recommend that you select a single-core plan rather than a plan with multiple cores.</span></span> <span data-ttu-id="4ae54-233">Napjainkban funkciók fut JavaScript-funkcióként hatékonyabban egymagos virtuális gépeken, és nagyobb virtuális gépek használata nem eredményez várt hello teljesítménynövekedést.</span><span class="sxs-lookup"><span data-stu-id="4ae54-233">Today, Functions runs JavaScript functions more efficiently on single-core VMs, and using larger VMs does not produce hello expected performance improvements.</span></span> <span data-ttu-id="4ae54-234">Szükség esetén manuálisan méretezheti ki további egymagos Virtuálisgép-példányok hozzáadásával, vagy engedélyezheti az automatikus skálázása.</span><span class="sxs-lookup"><span data-stu-id="4ae54-234">When necessary, you can manually scale out by adding more single-core VM instances, or you can enable auto-scale.</span></span> <span data-ttu-id="4ae54-235">További információkért lásd: [méretezése példányszám manuális vagy automatikus](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4ae54-235">For more information, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span></span>    

### <a name="typescript-and-coffeescript-support"></a><span data-ttu-id="4ae54-236">Géppel és CoffeeScript támogatása</span><span class="sxs-lookup"><span data-stu-id="4ae54-236">TypeScript and CoffeeScript support</span></span>
<span data-ttu-id="4ae54-237">Közvetlen támogatási még nem létezik automatikus fordítása géppel vagy CoffeeScript hello futásidejű keresztül, mert az ilyen támogatás kell a központi telepítéskor hello futásidejű kívül kezelt toobe.</span><span class="sxs-lookup"><span data-stu-id="4ae54-237">Because direct support does not yet exist for auto-compiling TypeScript or CoffeeScript via hello runtime, such support needs toobe handled outside hello runtime, at deployment time.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4ae54-238">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4ae54-238">Next steps</span></span>
<span data-ttu-id="4ae54-239">További információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="4ae54-239">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="4ae54-240">Azure Functions – ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="4ae54-240">Best practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="4ae54-241">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="4ae54-241">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="4ae54-242">Az Azure Functions C# fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="4ae54-242">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="4ae54-243">Az Azure Functions F # fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="4ae54-243">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="4ae54-244">Az Azure Functions eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="4ae54-244">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)


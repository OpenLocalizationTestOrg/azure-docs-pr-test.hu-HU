---
title: "Az Azure Functions JavaScript-fejlesztői útmutatója |} Microsoft Docs"
description: "Megtudhatja, hogyan fejleszthet függvények JavaScript használatával."
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
ms.openlocfilehash: 7ea81ed47f391fbce1432c2b11ac176ab6c04ae0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-javascript-developer-guide"></a><span data-ttu-id="db28a-104">Az Azure Functions JavaScript fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="db28a-104">Azure Functions JavaScript developer guide</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="db28a-105">C# parancsfájl</span><span class="sxs-lookup"><span data-stu-id="db28a-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="db28a-106">F # parancsfájl</span><span class="sxs-lookup"><span data-stu-id="db28a-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="db28a-107">JavaScript</span><span class="sxs-lookup"><span data-stu-id="db28a-107">JavaScript</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="db28a-108">Az Azure Functions JavaScript élményét megkönnyíti, hogy egy függvénynek, amely szerint átadott exportálása egy `context` objektum kommunikál a futtatókörnyezet és fogadása és adatküldés kötéseken keresztül.</span><span class="sxs-lookup"><span data-stu-id="db28a-108">The JavaScript experience for Azure Functions makes it easy to export a function, which is passed as a `context` object for communicating with the runtime and for receiving and sending data via bindings.</span></span>

<span data-ttu-id="db28a-109">Ez a cikk feltételezi, hogy Ön már elolvasta a [Azure Functions fejlesztői segédanyagai](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="db28a-109">This article assumes that you've already read the [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="exporting-a-function"></a><span data-ttu-id="db28a-110">Egy függvény exportálása</span><span class="sxs-lookup"><span data-stu-id="db28a-110">Exporting a function</span></span>
<span data-ttu-id="db28a-111">Minden JavaScript-funkcióként kell exportálnia az egyetlen `function` keresztül `module.exports` található a függvény, majd futtassa a futási időben.</span><span class="sxs-lookup"><span data-stu-id="db28a-111">All JavaScript functions must export a single `function` via `module.exports` for the runtime to find the function and run it.</span></span> <span data-ttu-id="db28a-112">Ez a funkció mindig tartalmaznia kell egy `context` objektum.</span><span class="sxs-lookup"><span data-stu-id="db28a-112">This function must always include a `context` object.</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

<span data-ttu-id="db28a-113">A kötések `direction === "in"` függvény argumentumaként, ami azt jelenti, amelyeket felhasználhat továbbítódnak [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) dinamikusan kezelje az új bemeneti adatokat (például használatával `arguments.length` az ismétlés a bemeneti adatok).</span><span class="sxs-lookup"><span data-stu-id="db28a-113">Bindings of `direction === "in"` are passed along as function arguments, which means that you can use [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) to dynamically handle new inputs (for example, by using `arguments.length` to iterate over all your inputs).</span></span> <span data-ttu-id="db28a-114">Ez a funkció esetén kényelmes csak egy eseményindító és nincs további bevitel, mivel nélkül hivatkozó kiszámítható hozzáfér az eseményindítóra vonatkozó információt a `context` objektum.</span><span class="sxs-lookup"><span data-stu-id="db28a-114">This functionality is convenient when you have only a trigger and no additional inputs, because you can predictably access your trigger data without referencing your `context` object.</span></span>

<span data-ttu-id="db28a-115">Az argumentumok mindig továbbítódnak a sorrendben történik a függvény *function.json*, még akkor is, ha nem adja meg azokat a kivitel utasításban.</span><span class="sxs-lookup"><span data-stu-id="db28a-115">The arguments are always passed along to the function in the order in which they occur in *function.json*, even if you don't specify them in your exports statement.</span></span> <span data-ttu-id="db28a-116">Például, ha rendelkezik `function(context, a, b)` és módosítsa úgy, hogy `function(context, a)`, továbbra is használhatja a értékének `b` szakaszra függvény kódban `arguments[3]`.</span><span class="sxs-lookup"><span data-stu-id="db28a-116">For example, if you have `function(context, a, b)` and change it to `function(context, a)`, you can still get the value of `b` in function code by referring to `arguments[3]`.</span></span>

<span data-ttu-id="db28a-117">Összes kötését, irány, attól függetlenül is továbbítódnak a `context` (lásd a következő parancsfájl) objektum.</span><span class="sxs-lookup"><span data-stu-id="db28a-117">All bindings, regardless of direction, are also passed along on the `context` object (see the following script).</span></span> 

## <a name="context-object"></a><span data-ttu-id="db28a-118">környezeti objektumot</span><span class="sxs-lookup"><span data-stu-id="db28a-118">context object</span></span>
<span data-ttu-id="db28a-119">A futtatókörnyezet használ egy `context` objektum és a függvény az adatok, és lehetővé teszik, hogy a futtatókörnyezet kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="db28a-119">The runtime uses a `context` object to pass data to and from your function and to let you communicate with the runtime.</span></span>

<span data-ttu-id="db28a-120">A környezeti objektumot mindig a függvény az első paraméter, és szerepelnie kell függvénykötésnek mert metódusok, mint `context.done` és `context.log`, amelyek ahhoz, hogy a futtatókörnyezet helyesen használni.</span><span class="sxs-lookup"><span data-stu-id="db28a-120">The context object is always the first parameter to a function and must be included because it has methods such as `context.done` and `context.log`, which are required to use the runtime correctly.</span></span> <span data-ttu-id="db28a-121">Nevezze el az lehet az objektum függetlenül szeretné (például `ctx` vagy `c`).</span><span class="sxs-lookup"><span data-stu-id="db28a-121">You can name the object whatever you would like (for example, `ctx` or `c`).</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a><span data-ttu-id="db28a-122">Context.Bindings tulajdonság</span><span class="sxs-lookup"><span data-stu-id="db28a-122">context.bindings property</span></span>

```
context.bindings
```
<span data-ttu-id="db28a-123">Egy elnevezett objektum beállítása/beolvasása, amely a bemeneti és kimeneti adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="db28a-123">Returns a named object that contains all your input and output data.</span></span> <span data-ttu-id="db28a-124">Például a következő kötés meghatározást a *function.json* megadható, hogy a várólista tartalmának eléréséhez a `context.bindings.myInput` objektum.</span><span class="sxs-lookup"><span data-stu-id="db28a-124">For example, the following binding definition in your *function.json* lets you access the contents of the queue from the `context.bindings.myInput` object.</span></span> 

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
}
```

```javascript
// myInput contains the input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

### <a name="contextdone-method"></a><span data-ttu-id="db28a-125">Context.Done módszer</span><span class="sxs-lookup"><span data-stu-id="db28a-125">context.done method</span></span>
```
context.done([err],[propertyBag])
```

<span data-ttu-id="db28a-126">Figyelmeztet, hogy a kód befejezte a futtatókörnyezet.</span><span class="sxs-lookup"><span data-stu-id="db28a-126">Informs the runtime that your code has finished.</span></span> <span data-ttu-id="db28a-127">Meg kell hívnia `context.done`, vagy ellenkező esetben a futásidejű soha nem tudja, hogy a függvény befejeződött, és végrehajtása időtúllépést okoz.</span><span class="sxs-lookup"><span data-stu-id="db28a-127">You must call `context.done`, or else the runtime never knows that your function is complete, and the execution will time out.</span></span> 

<span data-ttu-id="db28a-128">A `context.done` módszer lehetővé teszi a átadása vissza mindkét egy felhasználó által definiált hiba történt a futtatókörnyezet és tulajdonságai egy tulajdonságcsomagot, amelyek felülírják a tulajdonságok a `context.bindings` objektum.</span><span class="sxs-lookup"><span data-stu-id="db28a-128">The `context.done` method allows you to pass back both a user-defined error to the runtime and a property bag of properties that overwrite the properties on the `context.bindings` object.</span></span>

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a><span data-ttu-id="db28a-129">Context.log módszer</span><span class="sxs-lookup"><span data-stu-id="db28a-129">context.log method</span></span>  

```
context.log(message)
```
<span data-ttu-id="db28a-130">Lehetővé teszi a adatfolyam, a nyomkövetési szint konzolnaplófájlokban írni.</span><span class="sxs-lookup"><span data-stu-id="db28a-130">Allows you to write to the streaming console logs at the default trace level.</span></span> <span data-ttu-id="db28a-131">A `context.log`, további naplózási módszerek elérhetőek, amelyek lehetővé teszik a konzol naplófájlba más nyomkövetési szinten:</span><span class="sxs-lookup"><span data-stu-id="db28a-131">On `context.log`, additional logging methods are available that let you write to the console log at other trace levels:</span></span>


| <span data-ttu-id="db28a-132">Módszer</span><span class="sxs-lookup"><span data-stu-id="db28a-132">Method</span></span>                 | <span data-ttu-id="db28a-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="db28a-133">Description</span></span>                                |
| ---------------------- | ------------------------------------------ |
| <span data-ttu-id="db28a-134">**Hiba (_üzenet_)**</span><span class="sxs-lookup"><span data-stu-id="db28a-134">**error(_message_)**</span></span>   | <span data-ttu-id="db28a-135">Naplózás vagy alacsonyabb hibaszintet ír.</span><span class="sxs-lookup"><span data-stu-id="db28a-135">Writes to error level logging, or lower.</span></span>   |
| <span data-ttu-id="db28a-136">**Figyelmeztetés (_üzenet_)**</span><span class="sxs-lookup"><span data-stu-id="db28a-136">**warn(_message_)**</span></span>    | <span data-ttu-id="db28a-137">Figyelmeztetési szintű naplózás vagy alacsonyabb ír.</span><span class="sxs-lookup"><span data-stu-id="db28a-137">Writes to warning level logging, or lower.</span></span> |
| <span data-ttu-id="db28a-138">**Info (_üzenet_)**</span><span class="sxs-lookup"><span data-stu-id="db28a-138">**info(_message_)**</span></span>    | <span data-ttu-id="db28a-139">Információ szintet naplózás vagy alacsonyabb ír.</span><span class="sxs-lookup"><span data-stu-id="db28a-139">Writes to info level logging, or lower.</span></span>    |
| <span data-ttu-id="db28a-140">**részletes (_üzenet_)**</span><span class="sxs-lookup"><span data-stu-id="db28a-140">**verbose(_message_)**</span></span> | <span data-ttu-id="db28a-141">A részletes szint naplózás ír.</span><span class="sxs-lookup"><span data-stu-id="db28a-141">Writes to verbose level logging.</span></span>           |

<span data-ttu-id="db28a-142">A következő példa ír a figyelmeztetési nyomkövetési szint a konzolhoz:</span><span class="sxs-lookup"><span data-stu-id="db28a-142">The following example writes to the console at the warning trace level:</span></span>

```javascript
context.log.warn("Something has happened."); 
```
<span data-ttu-id="db28a-143">Állítsa be a nyomkövetési szintű küszöbértéke a host.json fájl van bejelentkezve, vagy kapcsolja ki.</span><span class="sxs-lookup"><span data-stu-id="db28a-143">You can set the trace-level threshold for logging in the host.json file, or turn it off.</span></span>  <span data-ttu-id="db28a-144">A naplókba írásával kapcsolatban további információkért tekintse meg a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="db28a-144">For more information about how to write to the logs, see the next section.</span></span>

## <a name="binding-data-type"></a><span data-ttu-id="db28a-145">Kötési adattípus</span><span class="sxs-lookup"><span data-stu-id="db28a-145">Binding data type</span></span>

<span data-ttu-id="db28a-146">Egy bemeneti kötése adattípusát megadásához használja a `dataType` tulajdonság kötése definíciójában.</span><span class="sxs-lookup"><span data-stu-id="db28a-146">To define the data type for an input binding, use the `dataType` property in the binding definition.</span></span> <span data-ttu-id="db28a-147">Olvassa el a tartalom HTTP-kérések bináris formátumú, például típust használjon `binary`:</span><span class="sxs-lookup"><span data-stu-id="db28a-147">For example, to read the content of an HTTP request in binary format, use the type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="db28a-148">Más beállításokat a `dataType` vannak `stream` és `string`.</span><span class="sxs-lookup"><span data-stu-id="db28a-148">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="writing-trace-output-to-the-console"></a><span data-ttu-id="db28a-149">A konzol nyomkövetés írása</span><span class="sxs-lookup"><span data-stu-id="db28a-149">Writing trace output to the console</span></span> 

<span data-ttu-id="db28a-150">A Funkciók, használja a `context.log` módszerek nyomkövetés írni a konzolt.</span><span class="sxs-lookup"><span data-stu-id="db28a-150">In Functions, you use the `context.log` methods to write trace output to the console.</span></span> <span data-ttu-id="db28a-151">Ezen a ponton nem használható `console.log` írni a konzolt.</span><span class="sxs-lookup"><span data-stu-id="db28a-151">At this point, you cannot use `console.log` to write to the console.</span></span>

<span data-ttu-id="db28a-152">A hívás esetén `context.log()`, az üzenetet a nyomkövetési szint, amely a konzol ír a _info_ nyomkövetési szint.</span><span class="sxs-lookup"><span data-stu-id="db28a-152">When you call `context.log()`, your message is written to the console at the default trace level, which is the _info_ trace level.</span></span> <span data-ttu-id="db28a-153">A következő kódot ír az információ a nyomkövetési szintet a konzolhoz:</span><span class="sxs-lookup"><span data-stu-id="db28a-153">The following code writes to the console at the info trace level:</span></span>

```javascript
context.log({hello: 'world'});  
```

<span data-ttu-id="db28a-154">Az előzőekben látható kód értéke megegyezik a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="db28a-154">The preceding code is equivalent to the following code:</span></span>

```javascript
context.log.info({hello: 'world'});  
```

<span data-ttu-id="db28a-155">A következő kódot ír a hibaszintet a konzolhoz:</span><span class="sxs-lookup"><span data-stu-id="db28a-155">The following code writes to the console at the error level:</span></span>

```javascript
context.log.error("An error has occurred.");  
```

<span data-ttu-id="db28a-156">Mivel _hiba_ a legmagasabb nyomkövetési szint a nyomkövetési ír a kimeneti minden nyomkövetési szintű mindaddig, amíg naplózás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="db28a-156">Because _error_ is the highest trace level, this trace is written to the output at all trace levels as long as logging is enabled.</span></span>  


<span data-ttu-id="db28a-157">Minden `context.log` módszerek támogatják a paraméter formátuma a Node.js által támogatott [util.format metódus](https://nodejs.org/api/util.html#util_util_format_format).</span><span class="sxs-lookup"><span data-stu-id="db28a-157">All `context.log` methods support the same parameter format that's supported by the Node.js [util.format method](https://nodejs.org/api/util.html#util_util_format_format).</span></span> <span data-ttu-id="db28a-158">Vegye figyelembe az alábbi kód, amely a konzol ír az alapértelmezett nyomkövetési szint használatával:</span><span class="sxs-lookup"><span data-stu-id="db28a-158">Consider the following code, which writes to the console by using the default trace level:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

<span data-ttu-id="db28a-159">Ugyanazt a kódot is kiírhatja a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="db28a-159">You can also write the same code in the following format:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a><span data-ttu-id="db28a-160">A nyomkövetési szint konzol naplózás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="db28a-160">Configure the trace level for console logging</span></span>

<span data-ttu-id="db28a-161">Funkciók lehetővé teszi a küszöbérték nyomkövetési szint írásra, a konzolt, amely megkönnyíti a módon nyomkövetési adatokat a konzoljára írt a funkciók a vezérlő meghatározása.</span><span class="sxs-lookup"><span data-stu-id="db28a-161">Functions lets you define the threshold trace level for writing to the console, which makes it easy to control the way traces are written to the console from your functions.</span></span> <span data-ttu-id="db28a-162">Összes adat konzolon küszöbértéke beállításához használja a `tracing.consoleLevel` tulajdonság a host.json fájlban.</span><span class="sxs-lookup"><span data-stu-id="db28a-162">To set the threshold for all traces written to the console, use the `tracing.consoleLevel` property in the host.json file.</span></span> <span data-ttu-id="db28a-163">A függvény alkalmazás összes funkciójának alkalmazza a beállítást.</span><span class="sxs-lookup"><span data-stu-id="db28a-163">This setting applies to all functions in your function app.</span></span> <span data-ttu-id="db28a-164">A következő példa a küszöbértéket nyomkövetési részletes naplózás engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="db28a-164">The following example sets the trace threshold to enable verbose logging:</span></span>

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

<span data-ttu-id="db28a-165">Az értékek **consoleLevel** nevének felel meg a `context.log` módszerek.</span><span class="sxs-lookup"><span data-stu-id="db28a-165">Values of **consoleLevel** correspond to the names of the `context.log` methods.</span></span> <span data-ttu-id="db28a-166">A konzol összes nyomkövetési naplózás letiltásához állítsa **consoleLevel** való _ki_.</span><span class="sxs-lookup"><span data-stu-id="db28a-166">To disable all trace logging to the console, set **consoleLevel** to _off_.</span></span> <span data-ttu-id="db28a-167">A host.json fájllal kapcsolatos további információkért lásd: a [host.json referencia-témakör](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="db28a-167">For more information about the host.json file, see the [host.json reference topic](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>

## <a name="http-triggers-and-bindings"></a><span data-ttu-id="db28a-168">HTTP-eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="db28a-168">HTTP triggers and bindings</span></span>

<span data-ttu-id="db28a-169">A HTTP és a webhook eseményindítók és a HTTP kimeneti kötések kérés- és -objektumok segítségével határoz meg a HTTP-üzenetküldést.</span><span class="sxs-lookup"><span data-stu-id="db28a-169">HTTP and webhook triggers and HTTP output bindings use request and response objects to represent the HTTP messaging.</span></span>  

### <a name="request-object"></a><span data-ttu-id="db28a-170">Kérelem objektum</span><span class="sxs-lookup"><span data-stu-id="db28a-170">Request object</span></span>

<span data-ttu-id="db28a-171">A `request` objektum tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="db28a-171">The `request` object has the following properties:</span></span>

| <span data-ttu-id="db28a-172">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="db28a-172">Property</span></span>      | <span data-ttu-id="db28a-173">Leírás</span><span class="sxs-lookup"><span data-stu-id="db28a-173">Description</span></span>                                                    |
| ------------- | -------------------------------------------------------------- |
| <span data-ttu-id="db28a-174">_törzs_</span><span class="sxs-lookup"><span data-stu-id="db28a-174">_body_</span></span>        | <span data-ttu-id="db28a-175">A kérelem törzsét tartalmazó objektum.</span><span class="sxs-lookup"><span data-stu-id="db28a-175">An object that contains the body of the request.</span></span>               |
| <span data-ttu-id="db28a-176">_fejlécek_</span><span class="sxs-lookup"><span data-stu-id="db28a-176">_headers_</span></span>     | <span data-ttu-id="db28a-177">A kérelem fejlécében tartalmazó objektum.</span><span class="sxs-lookup"><span data-stu-id="db28a-177">An object that contains the request headers.</span></span>                   |
| <span data-ttu-id="db28a-178">_módszer_</span><span class="sxs-lookup"><span data-stu-id="db28a-178">_method_</span></span>      | <span data-ttu-id="db28a-179">A kérelem HTTP-metódust.</span><span class="sxs-lookup"><span data-stu-id="db28a-179">The HTTP method of the request.</span></span>                                |
| <span data-ttu-id="db28a-180">_originalUrl_</span><span class="sxs-lookup"><span data-stu-id="db28a-180">_originalUrl_</span></span> | <span data-ttu-id="db28a-181">A kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="db28a-181">The URL of the request.</span></span>                                        |
| <span data-ttu-id="db28a-182">_paraméterei_</span><span class="sxs-lookup"><span data-stu-id="db28a-182">_params_</span></span>      | <span data-ttu-id="db28a-183">A kérés útválasztási paramétereit tartalmazó objektum.</span><span class="sxs-lookup"><span data-stu-id="db28a-183">An object that contains the routing parameters of the request.</span></span> |
| <span data-ttu-id="db28a-184">_lekérdezés_</span><span class="sxs-lookup"><span data-stu-id="db28a-184">_query_</span></span>       | <span data-ttu-id="db28a-185">Egy objektum, amely tartalmazza a lekérdezési paramétereket.</span><span class="sxs-lookup"><span data-stu-id="db28a-185">An object that contains the query parameters.</span></span>                  |
| <span data-ttu-id="db28a-186">_rawBody_</span><span class="sxs-lookup"><span data-stu-id="db28a-186">_rawBody_</span></span>     | <span data-ttu-id="db28a-187">A karakterlánc az üzenet törzsét.</span><span class="sxs-lookup"><span data-stu-id="db28a-187">The body of the message as a string.</span></span>                           |


### <a name="response-object"></a><span data-ttu-id="db28a-188">Válasz objektum</span><span class="sxs-lookup"><span data-stu-id="db28a-188">Response object</span></span>

<span data-ttu-id="db28a-189">A `response` objektum tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="db28a-189">The `response` object has the following properties:</span></span>

| <span data-ttu-id="db28a-190">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="db28a-190">Property</span></span>  | <span data-ttu-id="db28a-191">Leírás</span><span class="sxs-lookup"><span data-stu-id="db28a-191">Description</span></span>                                               |
| --------- | --------------------------------------------------------- |
| <span data-ttu-id="db28a-192">_törzs_</span><span class="sxs-lookup"><span data-stu-id="db28a-192">_body_</span></span>    | <span data-ttu-id="db28a-193">A választörzs tartalmazó objektum.</span><span class="sxs-lookup"><span data-stu-id="db28a-193">An object that contains the body of the response.</span></span>         |
| <span data-ttu-id="db28a-194">_fejlécek_</span><span class="sxs-lookup"><span data-stu-id="db28a-194">_headers_</span></span> | <span data-ttu-id="db28a-195">Egy objektum, amely tartalmazza a response fejlécekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="db28a-195">An object that contains the response headers.</span></span>             |
| <span data-ttu-id="db28a-196">_isRaw_</span><span class="sxs-lookup"><span data-stu-id="db28a-196">_isRaw_</span></span>   | <span data-ttu-id="db28a-197">Azt jelzi, hogy formázás ki van kapcsolva a választ.</span><span class="sxs-lookup"><span data-stu-id="db28a-197">Indicates that formatting is skipped for the response.</span></span>    |
| <span data-ttu-id="db28a-198">_állapot_</span><span class="sxs-lookup"><span data-stu-id="db28a-198">_status_</span></span>  | <span data-ttu-id="db28a-199">A HTTP-állapotkód: a válasz.</span><span class="sxs-lookup"><span data-stu-id="db28a-199">The HTTP status code of the response.</span></span>                     |

### <a name="accessing-the-request-and-response"></a><span data-ttu-id="db28a-200">A kérelem és válasz elérése</span><span class="sxs-lookup"><span data-stu-id="db28a-200">Accessing the request and response</span></span> 

<span data-ttu-id="db28a-201">HTTP-eseményindítók használata során is elérheti HTTP kérelem-válasz objektumok három módokon:</span><span class="sxs-lookup"><span data-stu-id="db28a-201">When you work with HTTP triggers, you can access the HTTP request and response objects in any of three ways:</span></span>

+ <span data-ttu-id="db28a-202">Az elnevezett bemeneti és kimeneti kötések.</span><span class="sxs-lookup"><span data-stu-id="db28a-202">From the named input and output bindings.</span></span> <span data-ttu-id="db28a-203">Ezzel a módszerrel a HTTP-eseményindítóval és kötések működik ugyanaz, mint bármely más kötés.</span><span class="sxs-lookup"><span data-stu-id="db28a-203">In this way, the HTTP trigger and bindings work the same as any other binding.</span></span> <span data-ttu-id="db28a-204">A következő példa egy elnevezett használatával állítja be a válasz objektum `response` kötés:</span><span class="sxs-lookup"><span data-stu-id="db28a-204">The following example sets the response object by using a named `response` binding:</span></span> 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ <span data-ttu-id="db28a-205">A `req` és `res` tulajdonságainak a `context` objektum.</span><span class="sxs-lookup"><span data-stu-id="db28a-205">From `req` and `res` properties on the `context` object.</span></span> <span data-ttu-id="db28a-206">Ily módon használhatja a hagyományos mintát fér hozzá a HTTP adatokhoz a context objektumot, a teljes használata helyett a `context.bindings.name` mintát.</span><span class="sxs-lookup"><span data-stu-id="db28a-206">In this way, you can use the conventional pattern to access HTTP data from the context object, instead of having to use the full `context.bindings.name` pattern.</span></span> <span data-ttu-id="db28a-207">A következő példa bemutatja, hogyan férhet hozzá a `req` és `res` az objektumokat a `context`:</span><span class="sxs-lookup"><span data-stu-id="db28a-207">The following example shows how to access the `req` and `res` objects on the `context`:</span></span>

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ <span data-ttu-id="db28a-208">Meghívásával `context.done()`.</span><span class="sxs-lookup"><span data-stu-id="db28a-208">By calling `context.done()`.</span></span> <span data-ttu-id="db28a-209">HTTP-kötés olyan különleges visszaadja a választ, átadott a `context.done()` metódust.</span><span class="sxs-lookup"><span data-stu-id="db28a-209">A special kind of HTTP binding returns the response that is passed to the `context.done()` method.</span></span> <span data-ttu-id="db28a-210">A következő HTTP kimeneti kötése definiál egy `$return` kimeneti paraméterként:</span><span class="sxs-lookup"><span data-stu-id="db28a-210">The following HTTP output binding defines a `$return` output parameter:</span></span>

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    <span data-ttu-id="db28a-211">A kimeneti kötés vár, amelyen meg kell adnia a válasz hívásakor `done()`, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="db28a-211">This output binding expects you to supply the response when you call `done()`, as follows:</span></span>

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a><span data-ttu-id="db28a-212">Csomópont verziója és a felügyeleti csomag</span><span class="sxs-lookup"><span data-stu-id="db28a-212">Node version and Package Management</span></span>
<span data-ttu-id="db28a-213">A csomópont verziója jelenleg zárolva van `6.5.0`.</span><span class="sxs-lookup"><span data-stu-id="db28a-213">The node version is currently locked at `6.5.0`.</span></span> <span data-ttu-id="db28a-214">Jelenleg vizsgálja a további verziók támogatása, és így konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="db28a-214">We're investigating adding support for more versions and making it configurable.</span></span>

<span data-ttu-id="db28a-215">Az alábbi lépéseket lehetővé teszik, hogy a csomagok tartalmazzák a függvény alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="db28a-215">The following steps let you include packages in your function app:</span></span> 

1. <span data-ttu-id="db28a-216">Nyissa meg a következőt: `https://<function_app_name>.scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="db28a-216">Go to `https://<function_app_name>.scm.azurewebsites.net`.</span></span>

2. <span data-ttu-id="db28a-217">Kattintson a **konzol Debug** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="db28a-217">Click **Debug Console** > **CMD**.</span></span>

3. <span data-ttu-id="db28a-218">Ugrás a `D:\home\site\wwwroot`, majd húzza a package.json fájlt a **wwwroot** mappát a a lap felső részén.</span><span class="sxs-lookup"><span data-stu-id="db28a-218">Go to `D:\home\site\wwwroot`, and then drag your package.json file to the **wwwroot** folder at the top half of the page.</span></span>  
    <span data-ttu-id="db28a-219">Feltöltheti fájlok az függvény alkalmazásban más módokon is.</span><span class="sxs-lookup"><span data-stu-id="db28a-219">You can upload files to your function app in other ways also.</span></span> <span data-ttu-id="db28a-220">További információkért lásd: [függvény alkalmazásfájlok frissítése](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="db28a-220">For more information, see [How to update function app files](functions-reference.md#fileupdate).</span></span> 

4. <span data-ttu-id="db28a-221">Miután a package.json fájl feltöltése, futtassa a `npm install` parancsot a **Kudu távoli végrehajtás konzol**.</span><span class="sxs-lookup"><span data-stu-id="db28a-221">After the package.json file is uploaded, run the `npm install` command in the **Kudu remote execution console**.</span></span>  
    <span data-ttu-id="db28a-222">Ez a művelet letölti a csomagot a package.json fájlban jelzett, és a függvény alkalmazás újraindul.</span><span class="sxs-lookup"><span data-stu-id="db28a-222">This action downloads the packages indicated in the package.json file and restarts the function app.</span></span>

<span data-ttu-id="db28a-223">Miután telepítette a szükséges csomagokat, importálja a fájlokat a függvény meghívásával `require('packagename')`, az alábbi példa szerint:</span><span class="sxs-lookup"><span data-stu-id="db28a-223">After the packages you need are installed, you import them to your function by calling `require('packagename')`, as in the following example:</span></span>

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

<span data-ttu-id="db28a-224">Meg kell határozni egy `package.json` fájl, a függvény alkalmazás gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="db28a-224">You should define a `package.json` file at the root of your function app.</span></span> <span data-ttu-id="db28a-225">Ennek révén a legjobb teljesítmény érdekében az azonos gyorsítótárazott csomagok megosztani az alkalmazásban funkciók meghatározása a fájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="db28a-225">Defining the file lets all functions in the app share the same cached packages, which gives the best performance.</span></span> <span data-ttu-id="db28a-226">Ha verziószáma, hogyan oldható meg hozzáadásával egy `package.json` fájl a mappában, egy adott funkció.</span><span class="sxs-lookup"><span data-stu-id="db28a-226">If a version conflict arises, you can resolve it by adding a `package.json` file in the folder of a specific function.</span></span>  

## <a name="environment-variables"></a><span data-ttu-id="db28a-227">Környezeti változók</span><span class="sxs-lookup"><span data-stu-id="db28a-227">Environment variables</span></span>
<span data-ttu-id="db28a-228">Egy környezeti változó vagy olyan alkalmazás, beállítás értékét, amelyet `process.env`, ahogy az az alábbi példakód:</span><span class="sxs-lookup"><span data-stu-id="db28a-228">To get an environment variable or an app setting value, use `process.env`, as shown in the following code example:</span></span>

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
## <a name="considerations-for-javascript-functions"></a><span data-ttu-id="db28a-229">JavaScript-funkcióként szempontjai</span><span class="sxs-lookup"><span data-stu-id="db28a-229">Considerations for JavaScript functions</span></span>

<span data-ttu-id="db28a-230">JavaScript-funkcióként használatakor figyelembe a következő két szakaszban ismertetett szempontok alapján.</span><span class="sxs-lookup"><span data-stu-id="db28a-230">When you work with JavaScript functions, be aware of the considerations in the following two sections.</span></span>

### <a name="choose-single-core-app-service-plans"></a><span data-ttu-id="db28a-231">Válasszon egy mag App Service-csomagokról</span><span class="sxs-lookup"><span data-stu-id="db28a-231">Choose single-core App Service plans</span></span>

<span data-ttu-id="db28a-232">Egy függvény alkalmazást, amely használja az App Service-csomag létrehozásakor azt javasoljuk, hogy kiválassza a több mag terv helyett egy egymagos terv.</span><span class="sxs-lookup"><span data-stu-id="db28a-232">When you create a function app that uses the App Service plan, we recommend that you select a single-core plan rather than a plan with multiple cores.</span></span> <span data-ttu-id="db28a-233">Napjainkban funkciók fut JavaScript-funkcióként hatékonyabban egymagos virtuális gépeken, és nagyobb virtuális gépek használata nem eredményez a várt teljesítménynövekedést.</span><span class="sxs-lookup"><span data-stu-id="db28a-233">Today, Functions runs JavaScript functions more efficiently on single-core VMs, and using larger VMs does not produce the expected performance improvements.</span></span> <span data-ttu-id="db28a-234">Szükség esetén manuálisan méretezheti ki további egymagos Virtuálisgép-példányok hozzáadásával, vagy engedélyezheti az automatikus skálázása.</span><span class="sxs-lookup"><span data-stu-id="db28a-234">When necessary, you can manually scale out by adding more single-core VM instances, or you can enable auto-scale.</span></span> <span data-ttu-id="db28a-235">További információkért lásd: [méretezése példányszám manuális vagy automatikus](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="db28a-235">For more information, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span></span>    

### <a name="typescript-and-coffeescript-support"></a><span data-ttu-id="db28a-236">Géppel és CoffeeScript támogatása</span><span class="sxs-lookup"><span data-stu-id="db28a-236">TypeScript and CoffeeScript support</span></span>
<span data-ttu-id="db28a-237">Közvetlen támogatási még nem létezik automatikus fordítása géppel vagy CoffeeScript a futtatókörnyezet keresztül, mert az ilyen támogatás kezelendő kívül a futtatókörnyezetet, a központi telepítéskor.</span><span class="sxs-lookup"><span data-stu-id="db28a-237">Because direct support does not yet exist for auto-compiling TypeScript or CoffeeScript via the runtime, such support needs to be handled outside the runtime, at deployment time.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="db28a-238">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="db28a-238">Next steps</span></span>
<span data-ttu-id="db28a-239">További információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="db28a-239">For more information, see the following resources:</span></span>

* [<span data-ttu-id="db28a-240">Azure Functions – ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="db28a-240">Best practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="db28a-241">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="db28a-241">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="db28a-242">Az Azure Functions C# fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="db28a-242">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="db28a-243">Az Azure Functions F # fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="db28a-243">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="db28a-244">Az Azure Functions eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="db28a-244">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)


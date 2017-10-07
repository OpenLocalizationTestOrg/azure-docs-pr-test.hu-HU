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
# <a name="azure-functions-javascript-developer-guide"></a>Az Azure Functions JavaScript fejlesztői útmutató
> [!div class="op_single_selector"]
> * [C# parancsfájl](functions-reference-csharp.md)
> * [F # parancsfájl](functions-reference-fsharp.md)
> * [JavaScript](functions-reference-node.md)
> 
> 

hello JavaScript tapasztalattal az Azure Functions segítségével könnyen tooexport egy függvénynek, amely szerint átadása egy `context` objektum hello futásidejű való kommunikációhoz és fogadása és adatküldés kötéseken keresztül.

Ez a cikk feltételezi, hogy Ön már elolvasta hello [Azure Functions fejlesztői segédanyagai](functions-reference.md).

## <a name="exporting-a-function"></a>Egy függvény exportálása
Minden JavaScript-funkcióként kell exportálnia az egyetlen `function` keresztül `module.exports` hello futási időben toofind hello függvény, és futtassa azt. Ez a funkció mindig tartalmaznia kell egy `context` objektum.

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

Kötések `direction === "in"` függvény argumentumaként, ami azt jelenti, amelyeket felhasználhat továbbítódnak [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) toodynamically kezelni új bemeneti adatokat (például használatával `arguments.length` tooiterate keresztül a bemeneti adatok). Ez a funkció esetén kényelmes csak egy eseményindító és nincs további bevitel, mivel nélkül hivatkozó kiszámítható hozzáfér az eseményindítóra vonatkozó információt a `context` objektum.

hello argumentumok mindig továbbítódnak toohello függvény hello sorrendben történik a *function.json*, még akkor is, ha nem adja meg azokat a kivitel utasításban. Például, ha van `function(context, a, b)` módosítsa azt túl`function(context, a)`, továbbra is használhatja a hello értékének `b` túl hivatkozással függvény kódban`arguments[3]`.

Összes kötését, irány, attól függetlenül is továbbítódnak a hello `context` (lásd a következő parancsfájl hello) objektum. 

## <a name="context-object"></a>környezeti objektumot
hello futásidejű használ egy `context` objektum toopass adatok tooand a függvény és kommunikálhat a hello futásidejű toolet.

hello környezeti objektumot mindig hello első tooa függvény és szerepeljen, mert a metódusok, mint `context.done` és `context.log`, szükséges toouse hello futásidejű ezek megfelelően. Nevére hello objektum függetlenül szeretné (például `ctx` vagy `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a>Context.Bindings tulajdonság

```
context.bindings
```
Egy elnevezett objektum beállítása/beolvasása, amely a bemeneti és kimeneti adatokat tartalmaz. Például a következő kötés-definíciójában hello a *function.json* megadható, hogy van-hozzáférési hello hello hello várólista tartalmát `context.bindings.myInput` objektum. 

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

### <a name="contextdone-method"></a>Context.Done módszer
```
context.done([err],[propertyBag])
```

Arról tájékoztatja, hogy a kód befejezte hello futásidejű. Meg kell hívnia `context.done`, vagy más hello futásidejű soha nem tudja, hogy a függvény befejeződött, és hello végrehajtása időtúllépést okoz. 

Hello `context.done` módszer lehetővé teszi, hogy toopass biztonsági másolatot a felhasználó által definiált hiba toohello futásidejű és a felülírási hello hello tulajdonságainak tulajdonságok tulajdonságcsomag `context.bindings` objektum.

```javascript
// Even though we set myOutput toohave:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object toohello done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// hello done method will overwrite hello myOutput binding toobe: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a>Context.log módszer  

```
context.log(message)
```
Lehetővé teszi a toowrite toohello konzol streamnaplókba hello alapértelmezett nyomkövetési szinten. A `context.log`, további naplózási módszerek elérhetőek, amelyek lehetővé teszik a toohello konzolbeli naplóit más nyomkövetési szinten írni:


| Módszer                 | Leírás                                |
| ---------------------- | ------------------------------------------ |
| **Hiba (_üzenet_)**   | Írja a naplózás vagy alacsonyabb tooerror szintjét.   |
| **Figyelmeztetés (_üzenet_)**    | Írja a naplózás vagy alacsonyabb toowarning szintjét. |
| **Info (_üzenet_)**    | Írja a naplózás vagy alacsonyabb tooinfo szintjét.    |
| **részletes (_üzenet_)** | Webhelyszintű naplózás tooverbose ír.           |

hello alábbi példa ír toohello konzol hello figyelmeztetés nyomkövetési szint:

```javascript
context.log.warn("Something has happened."); 
```
Állítsa be a nyomkövetési szintű küszöbértéke hello hello host.json fájl van bejelentkezve, vagy kapcsolja ki.  Hogyan naplózza az toowrite toohello kapcsolatos további információkért lásd a hello következő szakaszt.

## <a name="binding-data-type"></a>Kötési adattípus

toodefine hello adattípus egy bemeneti kötése használja hello `dataType` hello kötés definícióban tulajdonság. Például tooread hello bináris formátumú HTTP-kérelem tartalma, hello típust használjon `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Más beállításokat a `dataType` vannak `stream` és `string`.

## <a name="writing-trace-output-toohello-console"></a>Írás a nyomkövetési kimeneti toohello konzol 

A funkciók hello használja `context.log` módszerek toowrite nyomkövetési kimeneti toohello konzolt. Ezen a ponton nem használható `console.log` toowrite toohello konzol.

A hívás esetén `context.log()`, az üzenet írása hello alapértelmezett nyomkövetési szint, amely hello toohello a konzol _info_ nyomkövetési szint. hello következő kódot ír toohello konzol hello információ a nyomkövetési szint:

```javascript
context.log({hello: 'world'});  
```

hello előző kód egyenértékű toohello a következő kódot:

```javascript
context.log.info({hello: 'world'});  
```

hello következő kódot ír toohello konzol hello hiba szinten:

```javascript
context.log.error("An error has occurred.");  
```

Mivel _hiba_ hello legmagasabb nyomkövetési szint a nyomkövetési adatok íródnak toohello kimeneti minden nyomkövetési szintű mindaddig, amíg naplózás engedélyezve van.  


Minden `context.log` módszerek támogatják hello azonos paraméter formátuma hello Node.js által támogatott [util.format metódus](https://nodejs.org/api/util.html#util_util_format_format). Vegye figyelembe a következő kódot, amely toohello konzol írja hello alapértelmezett nyomkövetési szint használatával hello:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

További lehetőségek írási hello azonos hello formátuma a következő kódot:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-hello-trace-level-for-console-logging"></a>Hello nyomkövetési szint konzol naplózás konfigurálása

Funkciók lehetővé teszi a nyomkövetési küszöbértéket hello toohello konzol, így könnyen toocontrol hello módon nyomkövetések az a funkciók írt toohello konzol írásra meghatározása. tooset hello küszöbértéke toohello konzol használata hello írt minden nyomkövetési `tracing.consoleLevel` tulajdonság a hello host.json fájlban. Ez a beállítás az függvény alkalmazásban tooall funkciók vonatkozik. hello alábbi mintakód hello nyomkövetési küszöbérték tooenable részletes naplózás:

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

Az értékek **consoleLevel** hello toohello nevei megegyeznek `context.log` módszerek. toodisable minden nyomkövetési naplózás toohello konzol beállítása **consoleLevel** too_off_. Hello host.json fájllal kapcsolatos további információkért lásd: hello [host.json referencia-témakör](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

## <a name="http-triggers-and-bindings"></a>HTTP-eseményindítók és kötések

A HTTP és a webhook eseményindítók és a HTTP kimeneti kötések kérés és válasz objektumok toorepresent hello HTTP üzenetküldési használhatják.  

### <a name="request-object"></a>Kérelem objektum

Hello `request` objektumnak hello következő tulajdonságai:

| Tulajdonság      | Leírás                                                    |
| ------------- | -------------------------------------------------------------- |
| _törzs_        | Egy objektum, amely tartalmazza a hello hello kérelem törzse.               |
| _fejlécek_     | Egy objektum, amely tartalmazza a hello kérelemfejlécekben.                   |
| _módszer_      | hello hello kérelem HTTP-metódus.                                |
| _originalUrl_ | hello hello kérelem URL-címe                                        |
| _paraméterei_      | Egy objektum, amely tartalmazza a hello hello kérés útválasztási paramétereivel. |
| _lekérdezés_       | Egy objektum, amely tartalmazza a hello lekérdezési paramétereket.                  |
| _rawBody_     | hello üzenet karakterláncként hello törzsét.                           |


### <a name="response-object"></a>Válasz objektum

Hello `response` objektumnak hello következő tulajdonságai:

| Tulajdonság  | Leírás                                               |
| --------- | --------------------------------------------------------- |
| _törzs_    | Hello válasz hello törzsét tartalmazó objektum.         |
| _fejlécek_ | Egy objektum, amely tartalmazza a hello response fejlécekkel együtt.             |
| _isRaw_   | Azt jelzi, hogy formázás ki van kapcsolva hello válasz.    |
| _állapot_  | hello HTTP-állapotkód: hello válasz.                     |

### <a name="accessing-hello-request-and-response"></a>Hello kérés és válasz elérése 

Ha HTTP-eseményindítók használata esetén elérhető hello HTTP kérelem-válasz három módokon objektumok:

+ A hello nevű bemeneti és kimeneti kötéseket. Ily módon mint bármely más kötésnél hello HTTP-eseményindítóval és kötések munkahelyi hello azonos. hello alábbi mintakód hello válasz objektum használatával elnevezett `response` kötés: 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ A `req` és `res` hello tulajdonságainak `context` objektum. Ily módon használhatja hello hagyományos mintát tooaccess HTTP adatait hello környezeti objektumot, ahelyett, hogy teljes toouse hello `context.bindings.name` mintát. a következő példa azt mutatja meg hogyan hello tooaccess hello `req` és `res` hello objektumok `context`:

    ```javascript
    // You can access your http request off hello context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Meghívásával `context.done()`. HTTP-kötés olyan különleges toohello átadott hello választ ad vissza `context.done()` metódust. a következő HTTP hello kimeneti kötés határozza meg a `$return` kimeneti paraméterként:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    A kimeneti kötés vár, toosupply hello válasz hívásakor `done()`, az alábbiak szerint:

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a>Csomópont verziója és a felügyeleti csomag
hello csomópont verziója jelenleg zárolva van `6.5.0`. Jelenleg vizsgálja a további verziók támogatása, és így konfigurálható.

a lépéseket követve hello lehetővé teszik, hogy a csomagok tartalmazzák a függvény alkalmazásban: 

1. Nyissa meg túl`https://<function_app_name>.scm.azurewebsites.net`.

2. Kattintson a **konzol Debug** > **CMD**.

3. Nyissa meg túl`D:\home\site\wwwroot`, majd húzza a package.json fájl toohello **wwwroot** mappát a hello hello lap felső részén.  
    Fájlok tooyour függvény app más módokon is feltöltheti. További információkért lásd: [hogyan tooupdate működni az alkalmazás fájljai](functions-reference.md#fileupdate). 

4. Hello package.json fájl feltöltése, után futtassa a hello `npm install` hello parancsot **Kudu távoli végrehajtás konzol**.  
    Ez a művelet hello package.json fájlban jelzett hello csomagok letölti és hello függvény alkalmazás újraindul.

Hello szükséges csomagok telepítése után importálja a fájlokat a tooyour függvény meghívásával `require('packagename')`, a példában a következő hello:

```javascript
// Import hello underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

Meg kell határozni egy `package.json` fájlban a következő hello legfelső szintű függvény alkalmazása. Definiáló hello fájl lehetővé teszi, hogy az összes funkciójának hello app megosztás hello azonos gyorsítótárazott csomagok, ami hello lehető legjobb teljesítményt. Ha verziószáma, hogyan oldható meg hozzáadásával egy `package.json` hello fájlban egy adott funkció.  

## <a name="environment-variables"></a>Környezeti változók
tooget egy környezeti változó vagy egy alkalmazás beállításérték, használjon `process.env`, ahogy az alábbi kódpéldát hello:

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
## <a name="considerations-for-javascript-functions"></a>JavaScript-funkcióként szempontjai

JavaScript-funkcióként használatakor a következő két szakasz hello hello szempontok figyelembe.

### <a name="choose-single-core-app-service-plans"></a>Válasszon egy mag App Service-csomagokról

Egy függvény alkalmazás által használt hello App Service-csomag létrehozásakor azt javasoljuk, hogy kiválassza a több mag terv helyett egy egymagos terv. Napjainkban funkciók fut JavaScript-funkcióként hatékonyabban egymagos virtuális gépeken, és nagyobb virtuális gépek használata nem eredményez várt hello teljesítménynövekedést. Szükség esetén manuálisan méretezheti ki további egymagos Virtuálisgép-példányok hozzáadásával, vagy engedélyezheti az automatikus skálázása. További információkért lásd: [méretezése példányszám manuális vagy automatikus](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>Géppel és CoffeeScript támogatása
Közvetlen támogatási még nem létezik automatikus fordítása géppel vagy CoffeeScript hello futásidejű keresztül, mert az ilyen támogatás kell a központi telepítéskor hello futásidejű kívül kezelt toobe. 

## <a name="next-steps"></a>Következő lépések
További információkért tekintse meg a következő erőforrások hello:

* [Azure Functions – ajánlott eljárások](functions-best-practices.md)
* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md)
* [Az Azure Functions C# fejlesztői leírás](functions-reference-csharp.md)
* [Az Azure Functions F # fejlesztői leírás](functions-reference-fsharp.md)
* [Az Azure Functions eseményindítók és kötések](functions-triggers-bindings.md)


---
title: "aaaAzure funkciók HTTP és a webhook kötések |} Microsoft Docs"
description: "Megérteni, hogyan toouse HTTP és a webhook eseményindítók és kötések az Azure Functions."
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, esemény feldolgozása, webhookokkal, dinamikus számítás-, kiszolgáló nélküli architektúra, HTTP, API REST"
ms.assetid: 2b12200d-63d8-4ec1-9da8-39831d5a51b1
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/18/2016
ms.author: mahender
ms.openlocfilehash: c23b7a1443d492ed78c595e97d1d778a7ab12416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-http-and-webhook-bindings"></a>Az Azure Functions HTTP és a webhook kötések
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogyan tooconfigure és a HTTP használata eseményindítók és kötések az Azure Functions.
A fenti kiszolgáló nélküli-es számú Azure Functions toobuild API-k és válaszoljon rendszerhez készült toowebhooks is használhatja.

Az Azure Functions a következő kötések hello biztosítja:
- Egy [HTTP-eseményindítóval](#httptrigger) lehetővé teszi, hogy a HTTP-kérelem a függvényt. Ez lehet túl testreszabott toorespond[webhookok](#hooktrigger).
- Egy [HTTP kimeneti kötése](#output) toorespond toohello kérelem lehetővé teszi.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a>HTTP eseményindító
hello HTTP-eseményindítóval válasz tooan HTTP-kérelmek az függvény hajtható végre. Toorespond tooa adott URL-cím vagy a HTTP-metódus halmaza testreszabható. Egy HTTP-eseményindítóval konfigurált toorespond toowebhooks is lehet. 

Ha hello funkciók portál használata esetén is elkezdheti rögtön az előre elkészített sablon használatával. Válassza ki **új függvény** és választható "API és Webhookokkal" hello **forgatókönyv** legördülő menüből. Válasszon egyet a hello sablonok, majd kattintson a **létrehozása**.

Alapértelmezés szerint HTTP-eseményindítóval válaszol egy HTTP 200 OK állapotkódot és azt egy üres szövegtörzzsel toohello kérelmet. toomodify hello válaszban, konfigurálja az [HTTP kimeneti kötése](#output)

### <a name="configuring-an-http-trigger"></a>Egy HTTP-eseményindítóval konfigurálása
Határozza meg egy HTTP-eseményindítóval többek között a JSON objektum hasonló toohello hello a következő `bindings` function.json tömbje:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function",
    "methods": [ "get" ],
    "route": "values/{id}"
},
```
hello kötés támogatja-e hello következő tulajdonságai:

* **név** : szükséges – hello hello kérelemmel vagy a kérelem törzsében függvény a kódban használt változó neve. Lásd: [egy HTTP-eseményindítóval kódból használata](#httptriggerusage).
* **típus** : szükséges – túl lehet beállított "httpTrigger".
* **irány** : szükséges – túl beállított "a" kell lennie.
* _authLevel_ : Ez határozza meg, milyen kulcsok, ha van ilyen kell toobe rendelés tooinvoke hello funkció hello kérésre szerepel. Lásd: [kulcsoknál](#keys) alatt. hello érték hello a következők egyike lehet:
    * _Névtelen_: nem API-kulcsot meg kell adni.
    * _függvény_: egy funkcióspecifikus API-kulcsot meg kell adni. Ez az hello alapértelmezett érték, ha nincs megadva.
    * _felügyeleti_ : hello főkulcsára szükség.
* **módszerek** : Ez a tömb hello HTTP módszerek toowhich hello függvény válaszol. Ha nincs megadva, hello függvény tooall HTTP módszerek válaszol. Lásd: [hello HTTP-végpont testreszabása](#url).
* **útvonal** : hello útvonalsablonhoz toowhich szabályozása határozza meg a kérelem URL-címeket, a függvény válaszol. hello Ha nincs megadva, az alapérték `<functionname>`. Lásd: [hello HTTP-végpont testreszabása](#url).
* **webHookType** : Ez szerint állítja be hello HTTP-eseményindító tooact egy webhook reciever hello megadott szolgáltatóhoz. Hello _módszerek_ tulajdonság nem állítható be, ha ezt választja. Lásd: [válaszoló toowebhooks](#hooktrigger). hello érték hello a következők egyike lehet:
    * _genericJson_ : egy általános célú webhook végpont logika egy adott szolgáltató nélkül.
    * _github_ : hello függvény tooGitHub webhookok válaszol. Hello _authLevel_ tulajdonság nem állítható be, ha ezt választja.
    * _slackhez_ : hello függvény tooSlack webhookok válaszol. Hello _authLevel_ tulajdonság nem állítható be, ha ezt választja.

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a>Egy HTTP-eseményindítóval kódból használata
A C# és F # funkciók deklarálhatja az eseményindító bemeneti toobe hello típusú vagy `HttpRequestMessage` vagy olyan egyéni típusra. Ha úgy dönt, `HttpRequestMessage`, majd elérhetővé válik a teljes körű hozzáférési toohello request objektumon. Egy egyéni típust (például egy POCO) a funkciók az tooparse hello kérelemtörzset JSON toopopulate hello objektumtulajdonságokat megpróbál.

Node.js funkciók hello Functions futtatókörnyezete itt hello kérelemtörzset hello kérelem objektum helyett.

Lásd: [HTTP-eseményindító minták](#httptriggersample) például is érvényesek.


<a name="output"></a>
## <a name="http-response-output-binding"></a>HTTP-válasz kimeneti kötése
Hello HTTP kimeneti kötése toorespond toohello HTTP kérést küldő használja. A kötés HTTP-eseményindítóval igényel, és lehetővé teszi a hello eseményindító kérelemhez társított toocustomize hello válasz. Ha a HTTP kimeneti kötése van a nem Microsofttól származó, egy HTTP-eseményindítóval HTTP 200 OK visszatér egy üres szövegtörzzsel. 

### <a name="configuring-an-http-output-binding"></a>HTTP konfigurálása kimeneti kötése
hello HTTP kimeneti kötése definiált többek között a JSON objektum hasonló toohello hello a következő `bindings` function.json tömbje:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
hello kötés tartalmaz hello következő tulajdonságai:

* **név** : szükséges – hello hello válasz függvény a kódban használt változó neve. Lásd: [HTTP használata kimeneti kötése kódból](#outputusage).
* **típus** : szükséges – túl lehet beállított "http".
* **irány** : szükséges – állítsa be túl, "out" kell lennie.

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a>HTTP használata kimeneti kötése kódból
Hello kimeneti paraméter (például "res") toorespond toohello http vagy a webhook hívó is használhatja. Másik lehetőségként használhatja a szabványos `Request.CreateResponse()` (C#) vagy `context.res` (Node.JS) mintát tooreturn a válaszát. Hogyan toouse hello utóbbi metódus példákért lásd [HTTP-eseményindító minták](#httptriggersample) és [Webhook eseményindító minták](#hooktriggersample).


<a name="hooktrigger"></a>
## <a name="responding-toowebhooks"></a>Toowebhooks válaszol
Egy HTTP-eseményindítóval rendelkező hello _webHookType_ tulajdonság túl lesz konfigurált toorespond[webhookok](https://en.wikipedia.org/wiki/Webhook). Alapszintű hello konfigurálása hello "genericJson" beállítást használja. Ez a HTTP POST és hello korlátozza kérelmek tooonly `application/json` tartalomtípus.

hello eseményindító továbbá lehet meg a következőkhöz ideális tooa adott webhook szolgáltató (pl. [GitHub](https://developer.github.com/webhooks/) és [Slackhez](https://api.slack.com/outgoing-webhooks)). Ha egy szolgáltató van megadva, hello Functions futtatókörnyezete is kezeli hello szolgáltató ellenőrzési logika meg.  

### <a name="configuring-github-as-a-webhook-provider"></a>GitHub webhook szolgáltatóként konfigurálása
toorespond tooGitHub webhookokkal, először hozzon létre a függvény egy HTTP-eseményindítóval, és állítsa be a hello _webHookType_ tulajdonság túl "github". Másolja a [URL-cím](#url) és [API-kulcs](#keys) a GitHub-tárházban történő **adja hozzá a webhook** lap. Lásd a GitHub [Webhook létrehozása](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) több dokumentációját.

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a>A webhook szolgáltató Slackhez konfigurálása
hello Slack webhook létrehoz egy jogkivonatot helyett, és adja meg azt, konfigurálnia kell egy funkcióspecifikus kulcs hello jogkivonat a Slackhez, így Önnek. Lásd: [kulcsoknál](#keys).

<a name="url"></a>
## <a name="customizing-hello-http-endpoint"></a>Hello HTTP-végpont testreszabása
Alapértelmezés szerint HTTP-eseményindítóval, vagy WebHook, a függvény létrehozásakor hello függvény megcímezhető hello űrlap útvonal:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Ez az útvonal nem kötelező hello segítségével testre szabható `route` hello HTTP-eseményindító tulajdonsága bemeneti kötése. Tegyük fel, a következő hello *function.json* fájl határozza meg a `route` tulajdonság egy HTTP-eseményindító:

```json
    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }
```

Használja ezt a konfigurációt, hello függvény most megcímezhető az útvonal helyett hello eredeti útvonal a következő hello.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Ez lehetővé teszi, hogy hello függvény a két toosupport paraméterek az hello cím, "kategória" és "id". Akkor használhatja [webes API útvonal megkötés](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) a paraméterekkel. a következő C# funkciókódot hello mindkét paraméter használ.

```csharp
    public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }
```

Node.js függvény kód toouse hello itt található ilyen útvonal paraméterekkel.

```javascript
    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;

        if (!id) {
            context.res = {
                // status: 200, /* Defaults too200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults too200 */
                body: category + " item with id = " + id + " was requested."
            };
        }

        context.done();
    } 
```

Alapértelmezés szerint az összes függvény útvonal előtagként *api*. Is testre szabhatja, vagy távolítsa el a hello előtagja hello a `http.routePrefix` tulajdonságot a *host.json* fájlt. hello következő példában eltávolítjuk hello *api* útvonal előtagja hello hello előtagot üres karakterlánc használatával *host.json* fájlt.

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

Részletes információt tooupdate hello *host.json* fájl, a függvény, tekintse át, [hogyan tooupdate működni az alkalmazás fájljai](functions-reference.md#fileupdate). 

További tudnivalók egyéb konfigurálhatja a a *host.json* fájlba, lásd: [host.json hivatkozás](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).


<a name="keys"></a>
## <a name="working-with-keys"></a>Kulcsok használata
A nagyobb biztonság kulcsok HttpTriggers használhatják fel. Szabványos HttpTrigger használhat API-kulcs, ezek igénylő hello kulcs toobe hello kérelem megtalálható. Webhook használható kulcsok tooauthorize kérelmek az sokféleképpen, attól függően, hogy milyen hello szolgáltató támogatja.

Kulcsok részeként a függvény alkalmazást az Azure-ban tároljuk, és inaktív titkosított. tooview a kulcsokat, hozzon létre újakat, vagy állítsa a kulcsok toonew értékek, keresse meg a funkciók hello portálon tooone, és válassza ki a "Kezelése." 

A kulcsok két típusa van:
- **Kulcsok tárolására**: hello függvény alkalmazáson belüli összes funkciók által megosztott ezeket a kulcsokat. Ha egy API-kulcsot, ezek teszik lehetővé a hozzáférést tooany függvény hello függvény alkalmazáson belül.
- **Funkcióbillentyűk**: ezek a kulcsok alkalmazása csak toohello adott funkciókhoz használt vannak definiálva. Ha egy API-kulcsot, ezek csak engedélyezése hozzáférés toothat függvény.

Minden kulcs neve referenciaként, és nincs (a "alapértelmezett" nevű) alapértelmezett kulcs hello függvény és a gazdagép szintjén. Hello **főkulcs** egy alapértelmezett állomáskulcs neve "_master", amely minden függvény alkalmazás van definiálva, és nem vonható vissza. Rendszergazdai hozzáférés toohello futásidejű API-kat biztosít. Használatával `"authLevel": "admin"` hello a JSON kötés a kulcs toobe hello kérés jelenik meg a szükséges; bármely más kulcs engedélyezési hibát eredményez.

> [!NOTE]
> Esedékes toohello emelt szintű engedélyek hello főkulccsal, nem kell harmadik felek ossza meg ezt a kulcsot, vagy a natív ügyfélalkalmazások eloszthatják azt. Körültekintően járjon el hello rendszergazdai jogosultsági szint kiválasztásakor.
> 
> 

### <a name="api-key-authorization"></a>API-kulcs engedélyezési
Alapértelmezés szerint egy HttpTrigger hello HTTP-kérelmek API-kulcs szükséges. Ezért a HTTP-kérelmek általában néz ki:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

hello kulcs tartalmazhat egy lekérdezési karakterlánc-változóvá nevű `code`, a fentiek szerint, vagy a része egy `x-functions-key` HTTP-fejléc. hello kulcs hello értéke lehet bármely hello függvény definiált függvény kulcs, vagy bármely állomás kulcsát.

Választhat, tooallow kérelmek kulcsok nélkül, vagy adja meg, hogy hello főkulcs kell használható hello módosításával `authLevel` JSON kötés hello tulajdonságot (lásd: [HTTP-eseményindítóval](#httptrigger)).

### <a name="keys-and-webhooks"></a>Kulcsok és webhookokkal
Webhook engedélyezési hello webhook reciever összetevő kezeli, hello HttpTrigger és hello mechanizmus hello webhook típus alapján változik. Minden egyes mechanizmus nem, a kulcs azonban támaszkodnak. Alapértelmezés szerint "alapértelmezett" nevű hello függvény kulcs használható. Ha egy másik kulcsot akarja toouse, tooconfigure hello webhook szolgáltató toosend hello kulcsnév valamelyik a következő módokon hello hello kérelemmel lesz szüksége:

- **Lekérdezési karakterlánc**: hello szolgáltató hello kulcsnév továbbítja a hello `clientid` lekérdezési karakterlánc (pl. `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).
- **Kérelemfejléc**: hello szolgáltató hello kulcsnév továbbítja a hello `x-functions-clientid` fejléc.

> [!NOTE]
> Funkcióbillentyűk élveznek állomások kulcsait. Ha két kulcsok vannak meghatározva, az azonos név, hello hello függvény kulccsal.
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a>HTTP-eseményindító minták
Tegyük fel, hogy rendelkezik a következő HTTP-eseményindítóval a hello hello `bindings` function.json tömbje:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Lásd: hello nyelvspecifikus mintát, amely megkeresi a `name` hello lekérdezési karakterláncot vagy hello HTTP-kérelem törzse hello paraméter.

* [C#](#httptriggercsharp)
* [F#](#httptriggerfsharp)
* [Node.js](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a>A C# HTTP-eseményindító minta #
```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name tooquery string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on hello query string or in hello request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

Is köthető tooa POCO helyett `HttpRequestMessage`. Ez a hello szervezetnek hello kérelem JSON-ként értelmezni hidratált lehet. Hasonlóképpen egy kötés toohello HTTP-válasz kimenetének függvénynek adható át, és ez változatlanul adódik vissza hello adott válasz törzsének, 200 állapotkóddal.
```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
}
```

<a name="httptriggerfsharp"></a>
### <a name="http-trigger-sample-in-f"></a>Az F # HTTP-eseményindító minta #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on hello query string or in hello request body")
    } |> Async.StartAsTask
```

Kell egy `project.json` NuGet tooreference hello használó fájl `FSharp.Interop.Dynamic` és `Dynamitey` szerelvényeket, ehhez hasonló:

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

Ez fogja használni NuGet toofetch a függőségeket, és a parancsfájlban hivatkoznak rájuk.

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a>A Node.JS-ben HTTP-eseményindító minta
```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults too200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on hello query string or in hello request body"
        };
    }
    context.done();
};
```



<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a>Webhook minták
Tegyük fel, hogy rendelkezik a következő webhook eseményindítót hello hello `bindings` function.json tömbje:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

Lásd: hello nyelvspecifikus minta, amelyre bejelentkezik GitHub probléma megjegyzések.

* [C#](#hooktriggercsharp)
* [F#](#hooktriggerfsharp)
* [Node.js](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a>A C# Webhook minta #
```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

<a name="hooktriggerfsharp"></a>

### <a name="webhook-sample-in-f"></a>Az F # Webhook minta #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

<a name="hooktriggernodejs"></a>

### <a name="webhook-sample-in-nodejs"></a>A node.js Webhook minta
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


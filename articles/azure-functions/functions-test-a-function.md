---
title: az Azure Functions aaaTesting |} Microsoft Docs
description: "Az Azure functions tesztelése Postman, a cURL és a Node.js segítségével."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure funkciók, Funkciók, esemény feldolgozása, webhookokkal, dinamikus számítási, kiszolgáló nélküli architektúra tesztelése"
ms.assetid: c00f3082-30d2-46b3-96ea-34faf2f15f77
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/02/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a084f8dbc8089356c3c19d789dc9098f2bb63052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a>A kód az Azure Functions tesztelése kapcsolatos olyan stratégiák

Ebben a témakörben bemutatjuk hello megközelíti a különböző módokon tootest funkciókkal, beleértve a következő általános hello használata:

+ HTTP-alapú eszközök, például cURL Postman vagy webes eseményindítók még egy webes böngésző
+ Az Azure Tártallózó, tootest Azure Storage-alapú eseményindítók
+ Teszt lapon hello Azure Functions portálon
+ Időzítő-eseményindítóval aktivált függvény
+ Alkalmazás vagy a keretrendszer tesztelése

A fenti módszerek tesztelés funkcióval egy HTTP eseményindító, amely a bemeneti vagy keresztül fogadja a lekérdezési karakterlánc paraméter vagy hello kérés törzsében. Ez a funkció hello első szakaszában hoz létre.

## <a name="create-a-function-for-testing"></a>Tesztelési függvény létrehozása
Ez az oktatóanyag a legtöbb, a függvény létrehozása esetén érhető el HttpTrigger JavaScript-függvény sablon hello kis mértékben módosított verzióját használjuk. Ha a függvény létrehozásához segítségre van szüksége, tekintse meg a [oktatóanyag](functions-create-first-azure-function.md). Válassza ki a hello **HttpTrigger - JavaScript** sablon hello hello teszt függvény létrehozásakor [Azure-portálon].

hello alapértelmezett függvény sablon alapjában echók hátsó hello nevét hello kérelem törzse vagy a lekérdezési karakterlánc paramétert, a "hello world" függvény `name=<your name>`.  A Microsoft hello kód frissítése tooalso lehetővé teszik tooprovide hello nevét és egy címet JSON-tartalomként hello kérés törzsében. Majd hello függvény echók ezek hátsó toohello ügyfél, ha elérhető.   

Frissítés hello függvény a következő kódra, amely a tesztelési használjuk hello:

```javascript
module.exports = function (context, req) {
    context.log("HTTP trigger function processed a request. RequestUri=%s", req.originalUrl);
    context.log("Request Headers = " + JSON.stringify(req.headers));
    var res;

    if (req.query.name || (req.body && req.body.name)) {
        if (typeof req.query.name != "undefined") {
            context.log("Name was provided as a query string param...");
            res = ProcessNewUserInformation(context, req.query.name);
        }
        else {
            context.log("Processing user info from request body...");
            res = ProcessNewUserInformation(context, req.body.name, req.body.address);
        }
    }
    else {
        res = {
            status: 400,
            body: "Please pass a name on hello query string or in hello request body"
        };
    }
    context.done(null, res);
};
function ProcessNewUserInformation(context, name, address) {
    context.log("Processing user information...");
    context.log("name = " + name);
    var echoString = "Hello " + name;
    var res;

    if (typeof address != "undefined") {
        echoString += "\n" + "hello address you provided is " + address;
        context.log("address = " + address);
    }
    res = {
        // status: 200, /* Defaults too200 */
        body: echoString
    };
    return res;
}
```

## <a name="test-a-function-with-tools"></a>Függvény eszközök és tesztelése
Hello Azure-portálon, kívül vannak használható tootrigger a funkciók vizsgálatához különböző eszközöket. Ezek közé tartozik a HTTP-eszközök (Felhasználóifelület-alapú, mind parancs sor), Azure Storage access eszközök és még egy egyszerű webböngésző tesztelése.

### <a name="test-with-a-browser"></a>Egy böngésző tesztelése
hello webböngésző egy egyszerű módon tootrigger funkciók HTTP Protokollon keresztül. A GET-kérésekhez, amelyekhez nem szükséges a szervezet hasznos adatok között használható egy böngészőt, és használata csak lekérdezési karakterlánc-paraméterek.

korábban, meghatározott másolási hello tootest hello függvény **függvény URL-cím** hello portálról. A következő képernyő hello rendelkezik:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Hello hozzáfűzése `name` paraméter toohello lekérdezési karakterlánc. Használható hello tényleges név `<Enter a name here>` helyőrző.

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>&name=<Enter a name here>

Beillesztés hello URL-CÍMÉT a böngésző, és a válasz hasonló toohello következő kapja meg.

![Képernyőfelvétel, Chrome böngésző lapon teszt válasz](./media/functions-test-a-function/browser-test.png)

Ebben a példában az XML-karakterláncot adott vissza hello becsomagolja hello Chrome böngészőben. A más böngészőkkel csak a hello karakterlánc értéket jeleníti meg.

Hello portálon **naplók** ablakban kimeneti hasonló toohello következő hello függvény végrehajtása van bejelentkezve:

    2016-03-23T07:34:59  Welcome, you are now connected toolog-streaming service.
    2016-03-23T07:35:09.195 Function started (Id=61a8c5a9-5e44-4da0-909d-91d293f20445)
    2016-03-23T07:35:10.338 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==&name=Glenn from a browser
    2016-03-23T07:35:10.338 Request Headers = {"cache-control":"max-age=0","connection":"Keep-Alive","accept":"text/html","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T07:35:10.338 Name was provided as a query string param.
    2016-03-23T07:35:10.338 Processing User Information...
    2016-03-23T07:35:10.369 Function completed (Success, Id=61a8c5a9-5e44-4da0-909d-91d293f20445)

### <a name="test-with-postman"></a>A Postman tesztelése
ajánlott eszköz tootest hello többsége a függvények Postman, amely integrálható a hello Chrome böngészőben. tooinstall Postman, lásd: [beolvasása Postman](https://www.getpostman.com/). Postman segítségével szabályozhatja, HTTP-kérések számos további attribútumokat.

> [!TIP]
> Eszközzel hello HTTP tesztelési, hogy ismeri a Feladatkezelő. Az alábbiakban néhány alternatívák tooPostman:  
>
> * [Fiddler](http://www.telerik.com/fiddler)  
> * [Paw](https://luckymarmot.com/paw)  
>
>

a kérelem törzsében lévő Postman tootest hello függvényt:

1. Indítsa el Postman hello **alkalmazások** hello bal felső sarkában a böngészőablakba gomb.
2. Másolás a **függvény URL-cím**, majd illessze be a Postman. Ez magában foglalja a hello hozzáférési kód lekérdezési karakterláncot.
3. Hello HTTP-módszer váltása túl**POST**.
4. Kattintson a **törzs** > **nyers**, és adja hozzá a JSON kérelem törzse hasonló toohello következő:

    ```json
    {
        "name" : "Wes testing with Postman",
        "address" : "Seattle, WA 98101"
    }
    ```
5. Kattintson a **küldése**.

hello következő kép bemutatja, tesztelési hello egyszerű echo függvény példa az oktatóanyag.

![Képernyőfelvétel a Postman felhasználói felülete](./media/functions-test-a-function/postman-test.png)

Hello portálon **naplók** ablakban kimeneti hasonló toohello következő hello függvény végrehajtása van bejelentkezve:

    2016-03-23T08:04:51  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:04:57.107 Function started (Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)
    2016-03-23T08:04:57.763 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==
    2016-03-23T08:04:57.763 Request Headers = {"cache-control":"no-cache","connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:04:57.763 Processing user info from request body...
    2016-03-23T08:04:57.763 Processing User Information...
    2016-03-23T08:04:57.763 name = Wes testing with Postman
    2016-03-23T08:04:57.763 address = Seattle, W.A. 98101
    2016-03-23T08:04:57.795 Function completed (Success, Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)

### <a name="test-with-curl-from-hello-command-line"></a>A cURL hello parancssorból tesztelése
Gyakran szoftver tesztelést, esetén nem szükséges toolook minden további, mint a hello parancssori toohelp hibakeresési az alkalmazás. Ez nem eltér a functions tesztelése. Ne feledje, hogy hello cURL alapértelmezés szerint a Linux-alapú rendszerekhez. A Windows, először töltse le és telepítse a hello [cURL eszköz](https://curl.haxx.se/).

tootest hello függvény, hogy korábban meghatározott, másolása hello **függvény URL-cím** hello portálról. A következő képernyő hello rendelkezik:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Ez az hello URL indítására, a függvény. Tesztelje hello parancssori toomake hello cURL-parancs használatával egy GET (`-G` vagy `--get`) hello függvény kérelmet:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Ebben a példában a lekérdezési karakterlánc paraméterként, amelyek adatként átadhatók igényel (`-d`) hello a cURL parancs:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code> -d name=<Enter a name here>

Futtatási hello parancsot, és tekintse meg a következő kimeneti hello függvény hello parancssorban hello:

![Képernyőkép a parancssori kimenet](./media/functions-test-a-function/curl-test.png)

Hello portálon **naplók** ablakban kimeneti hasonló toohello következő hello függvény végrehajtása van bejelentkezve:

    2016-04-05T21:55:09  Welcome, you are now connected toolog-streaming service.
    2016-04-05T21:55:30.738 Function started (Id=ae6955da-29db-401a-b706-482fcd1b8f7a)
    2016-04-05T21:55:30.738 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/HttpTriggerNodeJS1?code=XXXXXXX&name=Azure Functions
    2016-04-05T21:55:30.738 Function completed (Success, Id=ae6955da-29db-401a-b706-482fcd1b8f7a)

### <a name="test-a-blob-trigger-by-using-storage-explorer"></a>Egy blob eseményindító tesztelése Tártallózó használatával
Egy blob funkció segítségével tesztelheti [Azure Tártallózó](http://storageexplorer.com/).

1. A hello [Azure-portálon] függvény alkalmazás, hozzon létre egy C#, F # vagy JavaScript blob eseményindító függvényt. Állítsa be a hello toomonitor toohello elérési útjának a blob-tároló. Példa:

        files
2. Kattintson a hello  **+**  tooselect gombra, vagy azt szeretné, hogy toouse hello storage-fiók létrehozása. Ezt követően kattintson a **Create** (Létrehozás) gombra.
3. A következő szöveg hello hozzon létre egy szövegfájlt, és mentse azt:

        A text file for blob trigger function testing.
4. Futtatás [Azure Tártallózó](http://storageexplorer.com/), és toohello blob tároló figyelt hello tárfiókban csatlakozzon.
5. Kattintson a **feltöltése** tooupload hello szövegfájl.

    ![Képernyőkép a Tártallózó alkalmazással](./media/functions-test-a-function/azure-storage-explorer-test.png)

hello alapértelmezett blob eseményindító funkciókódot hello blob hello naplók feldolgozása hello jelentések:

    2016-03-24T11:30:10  Welcome, you are now connected toolog-streaming service.
    2016-03-24T11:30:34.472 Function started (Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)
    2016-03-24T11:30:34.472 C# Blob trigger function processed: A text file for blob trigger function testing.
    2016-03-24T11:30:34.472 Function completed (Success, Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)

## <a name="test-a-function-within-functions"></a>Egy függvény funkciók tesztelése
hello Azure Functions portálon célja tesztelése a HTTP és a időzítő toolet indított funkciók. Funkciók tootrigger egyéb, a tesztelni kívánt funkciót is létrehozhat.

### <a name="test-with-hello-functions-portal-run-button"></a>Hello funkciók portál Futtatás gombra a teszt
hello portal biztosít egy **futtatása** toodo néhány használható gomb korlátozott tesztelése. Megadhat egy kérelemtörzset hello gomb használatával, de nem adja meg a lekérdezési karakterlánc paramétereket, illetve nem frissíthető a kérelemfejlécekben.

A JSON karakterlánc hasonló toohello következő hello való hozzáadásával korábban létrehozott hello HTTP funkció tesztelése **Request body** mező. Kattintson a hello **futtatása** gombra.

```json
{
    "name" : "Wes testing Run button",
    "address" : "USA"
}
```

Hello portálon **naplók** ablakban kimeneti hasonló toohello következő hello függvény végrehajtása van bejelentkezve:

    2016-03-23T08:03:12  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:03:17.357 Function started (Id=753a01b0-45a8-4125-a030-3ad543a89409)
    2016-03-23T08:03:18.697 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/wesmchttptriggernodejs1
    2016-03-23T08:03:18.697 Request Headers = {"connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:03:18.697 Processing user info from request body...
    2016-03-23T08:03:18.697 Processing User Information...
    2016-03-23T08:03:18.697 name = Wes testing Run button
    2016-03-23T08:03:18.697 address = USA
    2016-03-23T08:03:18.744 Function completed (Success, Id=753a01b0-45a8-4125-a030-3ad543a89409)


### <a name="test-with-a-timer-trigger"></a>Az időzítő eseményindító tesztelése
Egyes funkciók a korábban említett hello eszközökkel megfelelően nem tesztelhető. Tegyük fel, amely akkor fut, amikor egy üzenet megszakad a várólista eseményindító függvény [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md). Mindig írhat kódot toodrop egy üzenetet a várólistán, és példákat a konzol projektben áll a cikk későbbi részében. Van azonban egy másik módszert alkalmaz is használhat, amely közvetlenül tesztek funkciók.  

Egy időzítő indítófeltételt konfigurált üzenetsorokat használhatja kimeneti kötése. Hogy időzítő aktiváló kód majd írhat hello teszt üzenetek toohello várólista. Ez a szakasz egy példán keresztül bemutatja, hogyan.

További információt az Azure Functions kötések használatával, lásd: hello [Azure Functions fejlesztői segédanyagai](functions-reference.md).

#### <a name="create-a-queue-trigger-for-testing"></a>A létrehozott várólista tesztelése
toodemonstrate ezt a módszert használja, azt először létre kell hoznia egy várólista funkció, amely tootest szeretnénk nevű várólista `queue-newusers`. Ez a funkció be egy új felhasználó a Queue storage eldobott nevét és címét adatokat dolgozza fel.

> [!NOTE]
> Ha egy másik várólistához nevet használja, ellenőrizze, hogy a használt hello név megfelel toohello [elnevezési üzenetsorok és metaadatok](https://msdn.microsoft.com/library/dd179349.aspx) szabályok. Ellenkező esetben hibaüzenetet kap.
>
>

1. A hello [Azure-portálon] függvény alkalmazás, kattintson a **új függvény** > **QueueTrigger - C#**.
2. Adja meg a hello várólista neve toobe hello várólista függvény által figyelt:

        queue-newusers
3. Kattintson a hello  **+**  tooselect gombra, vagy azt szeretné, hogy toouse hello storage-fiók létrehozása. Ezt követően kattintson a **Create** (Létrehozás) gombra.
4. Hagyja megnyitva, a portál böngészőablakot, így hello naplóbejegyzések hello sablon alapértelmezett várólista funkciókódot a figyelheti.

#### <a name="create-a-timer-trigger-toodrop-a-message-in-hello-queue"></a>Egy időzítő eseményindító toodrop egy üzenet hello várólista létrehozása
1. Nyissa meg hello [Azure-portálon] egy új böngészőablakban, és keresse meg a tooyour függvény alkalmazást.
2. Kattintson a **új függvény** > **TimerTrigger - C#**. Adjon meg egy cron-kifejezés tooset milyen gyakran hello időzítő kód teszteli a várólista függvény. Ezt követően kattintson a **Create** (Létrehozás) gombra. Ha azt szeretné, hello teszt toorun 30 másodpercenként, hello következő használható [CRON-kifejezés](https://wikipedia.org/wiki/Cron#CRON_expression):

        */30 * * * * *
3. Kattintson a hello **integráció** lap az új időzítő eseményindító.
4. A **kimeneti**, kattintson a **+ új kimeneti**. Kattintson a **várólista** és **válasszon**.
5. Megjegyzés: hello nevét használja hello **várólista message objektumot**. Ezzel a hello időzítő funkciókódot.

        myQueue
6. Adja meg hello várólista neve, ahol hello üzenetet küld:

        queue-newusers
7. Kattintson a hello  **+**  tooselect hello hello várólista eseményindító a korábban használt tárfiók gombra. Ezután kattintson a **Save** (Mentés) gombra.
8. Kattintson a hello **Develop** az időzítő eseményindító fülre.
9. A következő kód hello C# időzítő függvény, hello mindaddig, amíg az azonos feldolgozási sor üzenetet objektumnév korábban bemutatott hello használt használhatja. Ezután kattintson a **Save** (Mentés) gombra.

    ```cs
    using System;

    public static void Run(TimerInfo myTimer, out String myQueue, TraceWriter log)
    {
        String newUser =
        "{\"name\":\"User testing from C# timer function\",\"address\":\"XYZ\"}";

        log.Verbose($"C# Timer trigger function executed at: {DateTime.Now}");   
        log.Verbose($"{newUser}");   

        myQueue = newUser;
    }
    ```

Ezen a ponton hello C# időzítő hajtja végre a 30 másodpercenként hello példa cron-kifejezés esetén. hello naplókat hello időzítő függvény a jelentés minden egyes végrehajtása:

    2016-03-24T10:27:02  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.004 Function started (Id=04061790-974f-4043-b851-48bd4ac424d1)
    2016-03-24T10:27:30.004 C# Timer trigger function executed at: 3/24/2016 10:27:30 AM
    2016-03-24T10:27:30.004 {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.004 Function completed (Success, Id=04061790-974f-4043-b851-48bd4ac424d1)

Hello böngészőablakban hello várólista függvény látható minden egyes feldolgozott üzenet:

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)

## <a name="test-a-function-with-code"></a>Egy függvény kóddal tesztelése
Szükség lehet egy külső alkalmazás- vagy keretrendszer tootest toocreate függvényeit.

### <a name="test-an-http-trigger-function-with-code-nodejs"></a>Egy HTTP-eseményindító függvény kóddal tesztelése: Node.js
A Node.js-alkalmazás tooexecute egy HTTP-kérelem tootest a funkció használata.
Győződjön meg arról, hogy tooset:

* Hello `host` hello kérelem beállítások tooyour függvény app fogadó.
* A függvény nevét a hello `path`.
* A hozzáférési kódját (`<your code>`) a hello `path`.

Példa:

```javascript
var http = require("http");

var nameQueryString = "name=Wes%20Query%20String%20Test%20From%20Node.js";

var nameBodyJSON = {
    name : "Wes testing with Node.JS code",
    address : "Dallas, T.X. 75201"
};

var bodyString = JSON.stringify(nameBodyJSON);

var options = {
  host: "functions841def78.azurewebsites.net",
  //path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9&" + nameQueryString,
  path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9",
  method: "POST",
  headers : {
      "Content-Type":"application/json",
      "Content-Length": Buffer.byteLength(bodyString)
    }    
};

callback = function(response) {
  var str = ""
  response.on("data", function (chunk) {
    str += chunk;
  });

  response.on("end", function () {
    console.log(str);
  });
}

var req = http.request(options, callback);
console.log("*** Sending name and address in body ***");
console.log(bodyString);
req.end(bodyString);
```


Kimenet:

    C:\Users\Wesley\testing\Node.js>node testHttpTriggerExample.js
    *** Sending name and address in body ***
    {"name" : "Wes testing with Node.JS code","address" : "Dallas, T.X. 75201"}
    Hello Wes testing with Node.JS code
    hello address you provided is Dallas, T.X. 75201

Hello portálon **naplók** ablakban kimeneti hasonló toohello következő hello függvény végrehajtása van bejelentkezve:

    2016-03-23T08:08:55  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:08:59.736 Function started (Id=607b891c-08a1-427f-910c-af64ae4f7f9c)
    2016-03-23T08:09:01.153 HTTP trigger function processed a request. RequestUri=http://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1/?code=XXXXXXXXXX==
    2016-03-23T08:09:01.153 Request Headers = {"connection":"Keep-Alive","host":"functionsExample.azurewebsites.net"}
    2016-03-23T08:09:01.153 Name not provided as query string param. Checking body...
    2016-03-23T08:09:01.153 Request Body Type = object
    2016-03-23T08:09:01.153 Request Body = [object Object]
    2016-03-23T08:09:01.153 Processing User Information...
    2016-03-23T08:09:01.215 Function completed (Success, Id=607b891c-08a1-427f-910c-af64ae4f7f9c)


### <a name="test-a-queue-trigger-function-with-code-c"></a>A várólista funkció kóddal tesztelése: C# #
Azt a korábban említett, hogy egy várólista eseményindító kód toodrop egy üzenetet a várólistában lévő segítségével tesztelheti. a következő példakód hello hello C#-kód jelenik meg a hello alapuló [Ismerkedés az Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) oktatóanyag. Kód más nyelveken érhető el az adott hivatkozáson.

tootest kell ezt a kódot egy konzol alkalmazásban:

* [A tárolási kapcsolati karakterlánc konfigurálása hello app.config fájlban](../storage/queues/storage-dotnet-how-to-use-queues.md).
* Adjon át egy `name` és `address` paraméterek toohello alkalmazásként. Például: `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`. (Ez a kód fogad el hello nevét és címét egy új felhasználó parancssori argumentumok futásidőben.)

C# példakódot:

```cs
static void Main(string[] args)
{
    string name = null;
    string address = null;
    string queueName = "queue-newusers";
    string JSON = null;

    if (args.Length > 0)
    {
        name = args[0];
    }
    if (args.Length > 1)
    {
        address = args[1];
    }

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["StorageConnectionString"]);

    // Create hello queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference tooa queue
    CloudQueue queue = queueClient.GetQueueReference(queueName);

    // Create hello queue if it doesn't already exist
    queue.CreateIfNotExists();

    // Create a message and add it toohello queue.
    if (name != null)
    {
        if (address != null)
            JSON = String.Format("{{\"name\":\"{0}\",\"address\":\"{1}\"}}", name, address);
        else
            JSON = String.Format("{{\"name\":\"{0}\"}}", name);
    }

    Console.WriteLine("Adding message too" + queueName + "...");
    Console.WriteLine(JSON);

    CloudQueueMessage message = new CloudQueueMessage(JSON);
    queue.AddMessage(message);
}
```

Hello böngészőablakban hello várólista függvény látható minden egyes feldolgozott üzenet:

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"Wes testing queues","address":"in a console app"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)


<!-- URLs. -->

[Azure-portálon]: https://portal.azure.com

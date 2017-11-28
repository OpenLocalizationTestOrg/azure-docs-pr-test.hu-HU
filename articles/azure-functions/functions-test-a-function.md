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
# <a name="strategies-for-testing-your-code-in-azure-functions"></a><span data-ttu-id="6ce15-104">A kód az Azure Functions tesztelése kapcsolatos olyan stratégiák</span><span class="sxs-lookup"><span data-stu-id="6ce15-104">Strategies for testing your code in Azure Functions</span></span>

<span data-ttu-id="6ce15-105">Ebben a témakörben bemutatjuk hello megközelíti a különböző módokon tootest funkciókkal, beleértve a következő általános hello használata:</span><span class="sxs-lookup"><span data-stu-id="6ce15-105">This topic demonstrates hello various ways tootest functions, including using hello following general approaches:</span></span>

+ <span data-ttu-id="6ce15-106">HTTP-alapú eszközök, például cURL Postman vagy webes eseményindítók még egy webes böngésző</span><span class="sxs-lookup"><span data-stu-id="6ce15-106">HTTP-based tools, such as cURL, Postman, and even a web browser for web-based triggers</span></span>
+ <span data-ttu-id="6ce15-107">Az Azure Tártallózó, tootest Azure Storage-alapú eseményindítók</span><span class="sxs-lookup"><span data-stu-id="6ce15-107">Azure Storage Explorer, tootest Azure Storage-based triggers</span></span>
+ <span data-ttu-id="6ce15-108">Teszt lapon hello Azure Functions portálon</span><span class="sxs-lookup"><span data-stu-id="6ce15-108">Test tab in hello Azure Functions portal</span></span>
+ <span data-ttu-id="6ce15-109">Időzítő-eseményindítóval aktivált függvény</span><span class="sxs-lookup"><span data-stu-id="6ce15-109">Timer-triggered function</span></span>
+ <span data-ttu-id="6ce15-110">Alkalmazás vagy a keretrendszer tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ce15-110">Testing application or framework</span></span>

<span data-ttu-id="6ce15-111">A fenti módszerek tesztelés funkcióval egy HTTP eseményindító, amely a bemeneti vagy keresztül fogadja a lekérdezési karakterlánc paraméter vagy hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="6ce15-111">All these testing methods use an HTTP trigger function that accepts input through either a query string parameter or hello request body.</span></span> <span data-ttu-id="6ce15-112">Ez a funkció hello első szakaszában hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6ce15-112">You create this function in hello first section.</span></span>

## <a name="create-a-function-for-testing"></a><span data-ttu-id="6ce15-113">Tesztelési függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ce15-113">Create a function for testing</span></span>
<span data-ttu-id="6ce15-114">Ez az oktatóanyag a legtöbb, a függvény létrehozása esetén érhető el HttpTrigger JavaScript-függvény sablon hello kis mértékben módosított verzióját használjuk.</span><span class="sxs-lookup"><span data-stu-id="6ce15-114">For most of this tutorial, we use a slightly modified version of hello HttpTrigger JavaScript function template that is available when you create a function.</span></span> <span data-ttu-id="6ce15-115">Ha a függvény létrehozásához segítségre van szüksége, tekintse meg a [oktatóanyag](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="6ce15-115">If you need help creating a function, review this [tutorial](functions-create-first-azure-function.md).</span></span> <span data-ttu-id="6ce15-116">Válassza ki a hello **HttpTrigger - JavaScript** sablon hello hello teszt függvény létrehozásakor [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="6ce15-116">Choose hello **HttpTrigger- JavaScript** template when creating hello test function in hello [Azure portal].</span></span>

<span data-ttu-id="6ce15-117">hello alapértelmezett függvény sablon alapjában echók hátsó hello nevét hello kérelem törzse vagy a lekérdezési karakterlánc paramétert, a "hello world" függvény `name=<your name>`.</span><span class="sxs-lookup"><span data-stu-id="6ce15-117">hello default function template is basically a "hello world" function that echoes back hello name from hello request body or query string parameter, `name=<your name>`.</span></span>  <span data-ttu-id="6ce15-118">A Microsoft hello kód frissítése tooalso lehetővé teszik tooprovide hello nevét és egy címet JSON-tartalomként hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="6ce15-118">We'll update hello code tooalso allow you tooprovide hello name and an address as JSON content in hello request body.</span></span> <span data-ttu-id="6ce15-119">Majd hello függvény echók ezek hátsó toohello ügyfél, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="6ce15-119">Then hello function echoes these back toohello client when available.</span></span>   

<span data-ttu-id="6ce15-120">Frissítés hello függvény a következő kódra, amely a tesztelési használjuk hello:</span><span class="sxs-lookup"><span data-stu-id="6ce15-120">Update hello function with hello following code, which we will use for testing:</span></span>

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

## <a name="test-a-function-with-tools"></a><span data-ttu-id="6ce15-121">Függvény eszközök és tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ce15-121">Test a function with tools</span></span>
<span data-ttu-id="6ce15-122">Hello Azure-portálon, kívül vannak használható tootrigger a funkciók vizsgálatához különböző eszközöket.</span><span class="sxs-lookup"><span data-stu-id="6ce15-122">Outside hello Azure portal, there are various tools that you can use tootrigger your functions for testing.</span></span> <span data-ttu-id="6ce15-123">Ezek közé tartozik a HTTP-eszközök (Felhasználóifelület-alapú, mind parancs sor), Azure Storage access eszközök és még egy egyszerű webböngésző tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6ce15-123">These include HTTP testing tools (both UI-based and command line), Azure Storage access tools, and even a simple web browser.</span></span>

### <a name="test-with-a-browser"></a><span data-ttu-id="6ce15-124">Egy böngésző tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ce15-124">Test with a browser</span></span>
<span data-ttu-id="6ce15-125">hello webböngésző egy egyszerű módon tootrigger funkciók HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="6ce15-125">hello web browser is a simple way tootrigger functions via HTTP.</span></span> <span data-ttu-id="6ce15-126">A GET-kérésekhez, amelyekhez nem szükséges a szervezet hasznos adatok között használható egy böngészőt, és használata csak lekérdezési karakterlánc-paraméterek.</span><span class="sxs-lookup"><span data-stu-id="6ce15-126">You can use a browser for GET requests that do not require a body payload, and that use only query string parameters.</span></span>

<span data-ttu-id="6ce15-127">korábban, meghatározott másolási hello tootest hello függvény **függvény URL-cím** hello portálról.</span><span class="sxs-lookup"><span data-stu-id="6ce15-127">tootest hello function we defined earlier, copy hello **Function Url** from hello portal.</span></span> <span data-ttu-id="6ce15-128">A következő képernyő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="6ce15-128">It has hello following form:</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="6ce15-129">Hello hozzáfűzése `name` paraméter toohello lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="6ce15-129">Append hello `name` parameter toohello query string.</span></span> <span data-ttu-id="6ce15-130">Használható hello tényleges név `<Enter a name here>` helyőrző.</span><span class="sxs-lookup"><span data-stu-id="6ce15-130">Use an actual name for hello `<Enter a name here>` placeholder.</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>&name=<Enter a name here>

<span data-ttu-id="6ce15-131">Beillesztés hello URL-CÍMÉT a böngésző, és a válasz hasonló toohello következő kapja meg.</span><span class="sxs-lookup"><span data-stu-id="6ce15-131">Paste hello URL into your browser, and you should get a response similar toohello following.</span></span>

![Képernyőfelvétel, Chrome böngésző lapon teszt válasz](./media/functions-test-a-function/browser-test.png)

<span data-ttu-id="6ce15-133">Ebben a példában az XML-karakterláncot adott vissza hello becsomagolja hello Chrome böngészőben.</span><span class="sxs-lookup"><span data-stu-id="6ce15-133">This example is hello Chrome browser, which wraps hello returned string in XML.</span></span> <span data-ttu-id="6ce15-134">A más böngészőkkel csak a hello karakterlánc értéket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6ce15-134">Other browsers display just hello string value.</span></span>

<span data-ttu-id="6ce15-135">Hello portálon **naplók** ablakban kimeneti hasonló toohello következő hello függvény végrehajtása van bejelentkezve:</span><span class="sxs-lookup"><span data-stu-id="6ce15-135">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T07:34:59  Welcome, you are now connected toolog-streaming service.
    2016-03-23T07:35:09.195 Function started (Id=61a8c5a9-5e44-4da0-909d-91d293f20445)
    2016-03-23T07:35:10.338 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==&name=Glenn from a browser
    2016-03-23T07:35:10.338 Request Headers = {"cache-control":"max-age=0","connection":"Keep-Alive","accept":"text/html","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T07:35:10.338 Name was provided as a query string param.
    2016-03-23T07:35:10.338 Processing User Information...
    2016-03-23T07:35:10.369 Function completed (Success, Id=61a8c5a9-5e44-4da0-909d-91d293f20445)

### <a name="test-with-postman"></a><span data-ttu-id="6ce15-136">A Postman tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ce15-136">Test with Postman</span></span>
<span data-ttu-id="6ce15-137">ajánlott eszköz tootest hello többsége a függvények Postman, amely integrálható a hello Chrome böngészőben.</span><span class="sxs-lookup"><span data-stu-id="6ce15-137">hello recommended tool tootest most of your functions is Postman, which integrates with hello Chrome browser.</span></span> <span data-ttu-id="6ce15-138">tooinstall Postman, lásd: [beolvasása Postman](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="6ce15-138">tooinstall Postman, see [Get Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="6ce15-139">Postman segítségével szabályozhatja, HTTP-kérések számos további attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="6ce15-139">Postman provides control over many more attributes of an HTTP request.</span></span>

> [!TIP]
> <span data-ttu-id="6ce15-140">Eszközzel hello HTTP tesztelési, hogy ismeri a Feladatkezelő.</span><span class="sxs-lookup"><span data-stu-id="6ce15-140">Use hello HTTP testing tool that you are most comfortable with.</span></span> <span data-ttu-id="6ce15-141">Az alábbiakban néhány alternatívák tooPostman:</span><span class="sxs-lookup"><span data-stu-id="6ce15-141">Here are some alternatives tooPostman:</span></span>  
>
> * [<span data-ttu-id="6ce15-142">Fiddler</span><span class="sxs-lookup"><span data-stu-id="6ce15-142">Fiddler</span></span>](http://www.telerik.com/fiddler)  
> * [<span data-ttu-id="6ce15-143">Paw</span><span class="sxs-lookup"><span data-stu-id="6ce15-143">Paw</span></span>](https://luckymarmot.com/paw)  
>
>

<span data-ttu-id="6ce15-144">a kérelem törzsében lévő Postman tootest hello függvényt:</span><span class="sxs-lookup"><span data-stu-id="6ce15-144">tootest hello function with a request body in Postman:</span></span>

1. <span data-ttu-id="6ce15-145">Indítsa el Postman hello **alkalmazások** hello bal felső sarkában a böngészőablakba gomb.</span><span class="sxs-lookup"><span data-stu-id="6ce15-145">Start Postman from hello **Apps** button in hello upper-left corner of a Chrome browser window.</span></span>
2. <span data-ttu-id="6ce15-146">Másolás a **függvény URL-cím**, majd illessze be a Postman.</span><span class="sxs-lookup"><span data-stu-id="6ce15-146">Copy your **Function Url**, and paste it into Postman.</span></span> <span data-ttu-id="6ce15-147">Ez magában foglalja a hello hozzáférési kód lekérdezési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="6ce15-147">It includes hello access code query string parameter.</span></span>
3. <span data-ttu-id="6ce15-148">Hello HTTP-módszer váltása túl**POST**.</span><span class="sxs-lookup"><span data-stu-id="6ce15-148">Change hello HTTP method too**POST**.</span></span>
4. <span data-ttu-id="6ce15-149">Kattintson a **törzs** > **nyers**, és adja hozzá a JSON kérelem törzse hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="6ce15-149">Click **Body** > **raw**, and add a JSON request body similar toohello following:</span></span>

    ```json
    {
        "name" : "Wes testing with Postman",
        "address" : "Seattle, WA 98101"
    }
    ```
5. <span data-ttu-id="6ce15-150">Kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="6ce15-150">Click **Send**.</span></span>

<span data-ttu-id="6ce15-151">hello következő kép bemutatja, tesztelési hello egyszerű echo függvény példa az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="6ce15-151">hello following image shows testing hello simple echo function example in this tutorial.</span></span>

![Képernyőfelvétel a Postman felhasználói felülete](./media/functions-test-a-function/postman-test.png)

<span data-ttu-id="6ce15-153">Hello portálon **naplók** ablakban kimeneti hasonló toohello következő hello függvény végrehajtása van bejelentkezve:</span><span class="sxs-lookup"><span data-stu-id="6ce15-153">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T08:04:51  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:04:57.107 Function started (Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)
    2016-03-23T08:04:57.763 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==
    2016-03-23T08:04:57.763 Request Headers = {"cache-control":"no-cache","connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:04:57.763 Processing user info from request body...
    2016-03-23T08:04:57.763 Processing User Information...
    2016-03-23T08:04:57.763 name = Wes testing with Postman
    2016-03-23T08:04:57.763 address = Seattle, W.A. 98101
    2016-03-23T08:04:57.795 Function completed (Success, Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)

### <a name="test-with-curl-from-hello-command-line"></a><span data-ttu-id="6ce15-154">A cURL hello parancssorból tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ce15-154">Test with cURL from hello command line</span></span>
<span data-ttu-id="6ce15-155">Gyakran szoftver tesztelést, esetén nem szükséges toolook minden további, mint a hello parancssori toohelp hibakeresési az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6ce15-155">Often when you're testing software, it's not necessary toolook any further than hello command line toohelp debug your application.</span></span> <span data-ttu-id="6ce15-156">Ez nem eltér a functions tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6ce15-156">This is no different with testing functions.</span></span> <span data-ttu-id="6ce15-157">Ne feledje, hogy hello cURL alapértelmezés szerint a Linux-alapú rendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="6ce15-157">Note that hello cURL is available by default on Linux-based systems.</span></span> <span data-ttu-id="6ce15-158">A Windows, először töltse le és telepítse a hello [cURL eszköz](https://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="6ce15-158">On Windows, you must first download and install hello [cURL tool](https://curl.haxx.se/).</span></span>

<span data-ttu-id="6ce15-159">tootest hello függvény, hogy korábban meghatározott, másolása hello **függvény URL-cím** hello portálról.</span><span class="sxs-lookup"><span data-stu-id="6ce15-159">tootest hello function that we defined earlier, copy hello **Function URL** from hello portal.</span></span> <span data-ttu-id="6ce15-160">A következő képernyő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="6ce15-160">It has hello following form:</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="6ce15-161">Ez az hello URL indítására, a függvény.</span><span class="sxs-lookup"><span data-stu-id="6ce15-161">This is hello URL for triggering your function.</span></span> <span data-ttu-id="6ce15-162">Tesztelje hello parancssori toomake hello cURL-parancs használatával egy GET (`-G` vagy `--get`) hello függvény kérelmet:</span><span class="sxs-lookup"><span data-stu-id="6ce15-162">Test this by using hello cURL command on hello command line toomake a GET (`-G` or `--get`) request against hello function:</span></span>

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="6ce15-163">Ebben a példában a lekérdezési karakterlánc paraméterként, amelyek adatként átadhatók igényel (`-d`) hello a cURL parancs:</span><span class="sxs-lookup"><span data-stu-id="6ce15-163">This particular example requires a query string parameter, which can be passed as Data (`-d`) in hello cURL command:</span></span>

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code> -d name=<Enter a name here>

<span data-ttu-id="6ce15-164">Futtatási hello parancsot, és tekintse meg a következő kimeneti hello függvény hello parancssorban hello:</span><span class="sxs-lookup"><span data-stu-id="6ce15-164">Run hello command, and you see hello following output of hello function on hello command line:</span></span>

![Képernyőkép a parancssori kimenet](./media/functions-test-a-function/curl-test.png)

<span data-ttu-id="6ce15-166">Hello portálon **naplók** ablakban kimeneti hasonló toohello következő hello függvény végrehajtása van bejelentkezve:</span><span class="sxs-lookup"><span data-stu-id="6ce15-166">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-04-05T21:55:09  Welcome, you are now connected toolog-streaming service.
    2016-04-05T21:55:30.738 Function started (Id=ae6955da-29db-401a-b706-482fcd1b8f7a)
    2016-04-05T21:55:30.738 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/HttpTriggerNodeJS1?code=XXXXXXX&name=Azure Functions
    2016-04-05T21:55:30.738 Function completed (Success, Id=ae6955da-29db-401a-b706-482fcd1b8f7a)

### <a name="test-a-blob-trigger-by-using-storage-explorer"></a><span data-ttu-id="6ce15-167">Egy blob eseményindító tesztelése Tártallózó használatával</span><span class="sxs-lookup"><span data-stu-id="6ce15-167">Test a blob trigger by using Storage Explorer</span></span>
<span data-ttu-id="6ce15-168">Egy blob funkció segítségével tesztelheti [Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="6ce15-168">You can test a blob trigger function by using [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

1. <span data-ttu-id="6ce15-169">A hello [Azure-portálon] függvény alkalmazás, hozzon létre egy C#, F # vagy JavaScript blob eseményindító függvényt.</span><span class="sxs-lookup"><span data-stu-id="6ce15-169">In hello [Azure portal] for your function app, create a C#, F# or JavaScript blob trigger function.</span></span> <span data-ttu-id="6ce15-170">Állítsa be a hello toomonitor toohello elérési útjának a blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="6ce15-170">Set hello path toomonitor toohello name of your blob container.</span></span> <span data-ttu-id="6ce15-171">Példa:</span><span class="sxs-lookup"><span data-stu-id="6ce15-171">For example:</span></span>

        files
2. <span data-ttu-id="6ce15-172">Kattintson a hello  **+**  tooselect gombra, vagy azt szeretné, hogy toouse hello storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6ce15-172">Click hello **+** button tooselect or create hello storage account you want toouse.</span></span> <span data-ttu-id="6ce15-173">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6ce15-173">Then click **Create**.</span></span>
3. <span data-ttu-id="6ce15-174">A következő szöveg hello hozzon létre egy szövegfájlt, és mentse azt:</span><span class="sxs-lookup"><span data-stu-id="6ce15-174">Create a text file with hello following text, and save it:</span></span>

        A text file for blob trigger function testing.
4. <span data-ttu-id="6ce15-175">Futtatás [Azure Tártallózó](http://storageexplorer.com/), és toohello blob tároló figyelt hello tárfiókban csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="6ce15-175">Run [Azure Storage Explorer](http://storageexplorer.com/), and connect toohello blob container in hello storage account being monitored.</span></span>
5. <span data-ttu-id="6ce15-176">Kattintson a **feltöltése** tooupload hello szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="6ce15-176">Click **Upload** tooupload hello text file.</span></span>

    ![Képernyőkép a Tártallózó alkalmazással](./media/functions-test-a-function/azure-storage-explorer-test.png)

<span data-ttu-id="6ce15-178">hello alapértelmezett blob eseményindító funkciókódot hello blob hello naplók feldolgozása hello jelentések:</span><span class="sxs-lookup"><span data-stu-id="6ce15-178">hello default blob trigger function code reports hello processing of hello blob in hello logs:</span></span>

    2016-03-24T11:30:10  Welcome, you are now connected toolog-streaming service.
    2016-03-24T11:30:34.472 Function started (Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)
    2016-03-24T11:30:34.472 C# Blob trigger function processed: A text file for blob trigger function testing.
    2016-03-24T11:30:34.472 Function completed (Success, Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)

## <a name="test-a-function-within-functions"></a><span data-ttu-id="6ce15-179">Egy függvény funkciók tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ce15-179">Test a function within functions</span></span>
<span data-ttu-id="6ce15-180">hello Azure Functions portálon célja tesztelése a HTTP és a időzítő toolet indított funkciók.</span><span class="sxs-lookup"><span data-stu-id="6ce15-180">hello Azure Functions portal is designed toolet you test HTTP and timer triggered functions.</span></span> <span data-ttu-id="6ce15-181">Funkciók tootrigger egyéb, a tesztelni kívánt funkciót is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="6ce15-181">You can also create functions tootrigger other functions that you are testing.</span></span>

### <a name="test-with-hello-functions-portal-run-button"></a><span data-ttu-id="6ce15-182">Hello funkciók portál Futtatás gombra a teszt</span><span class="sxs-lookup"><span data-stu-id="6ce15-182">Test with hello Functions portal Run button</span></span>
<span data-ttu-id="6ce15-183">hello portal biztosít egy **futtatása** toodo néhány használható gomb korlátozott tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6ce15-183">hello portal provides a **Run** button that you can use toodo some limited testing.</span></span> <span data-ttu-id="6ce15-184">Megadhat egy kérelemtörzset hello gomb használatával, de nem adja meg a lekérdezési karakterlánc paramétereket, illetve nem frissíthető a kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="6ce15-184">You can provide a request body by using hello button, but you can't provide query string parameters or update request headers.</span></span>

<span data-ttu-id="6ce15-185">A JSON karakterlánc hasonló toohello következő hello való hozzáadásával korábban létrehozott hello HTTP funkció tesztelése **Request body** mező.</span><span class="sxs-lookup"><span data-stu-id="6ce15-185">Test hello HTTP trigger function we created earlier by adding a JSON string similar toohello following in hello **Request body** field.</span></span> <span data-ttu-id="6ce15-186">Kattintson a hello **futtatása** gombra.</span><span class="sxs-lookup"><span data-stu-id="6ce15-186">Then click hello **Run** button.</span></span>

```json
{
    "name" : "Wes testing Run button",
    "address" : "USA"
}
```

<span data-ttu-id="6ce15-187">Hello portálon **naplók** ablakban kimeneti hasonló toohello következő hello függvény végrehajtása van bejelentkezve:</span><span class="sxs-lookup"><span data-stu-id="6ce15-187">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T08:03:12  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:03:17.357 Function started (Id=753a01b0-45a8-4125-a030-3ad543a89409)
    2016-03-23T08:03:18.697 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/wesmchttptriggernodejs1
    2016-03-23T08:03:18.697 Request Headers = {"connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:03:18.697 Processing user info from request body...
    2016-03-23T08:03:18.697 Processing User Information...
    2016-03-23T08:03:18.697 name = Wes testing Run button
    2016-03-23T08:03:18.697 address = USA
    2016-03-23T08:03:18.744 Function completed (Success, Id=753a01b0-45a8-4125-a030-3ad543a89409)


### <a name="test-with-a-timer-trigger"></a><span data-ttu-id="6ce15-188">Az időzítő eseményindító tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ce15-188">Test with a timer trigger</span></span>
<span data-ttu-id="6ce15-189">Egyes funkciók a korábban említett hello eszközökkel megfelelően nem tesztelhető.</span><span class="sxs-lookup"><span data-stu-id="6ce15-189">Some functions can't be adequately tested with hello tools mentioned previously.</span></span> <span data-ttu-id="6ce15-190">Tegyük fel, amely akkor fut, amikor egy üzenet megszakad a várólista eseményindító függvény [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="6ce15-190">For example, consider a queue trigger function that runs when a message is dropped into [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span> <span data-ttu-id="6ce15-191">Mindig írhat kódot toodrop egy üzenetet a várólistán, és példákat a konzol projektben áll a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="6ce15-191">You can always write code toodrop a message into your queue, and an example of this in a console project is provided later in this article.</span></span> <span data-ttu-id="6ce15-192">Van azonban egy másik módszert alkalmaz is használhat, amely közvetlenül tesztek funkciók.</span><span class="sxs-lookup"><span data-stu-id="6ce15-192">However, there is another approach you can use that tests functions directly.</span></span>  

<span data-ttu-id="6ce15-193">Egy időzítő indítófeltételt konfigurált üzenetsorokat használhatja kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="6ce15-193">You can use a timer trigger configured with a queue output binding.</span></span> <span data-ttu-id="6ce15-194">Hogy időzítő aktiváló kód majd írhat hello teszt üzenetek toohello várólista.</span><span class="sxs-lookup"><span data-stu-id="6ce15-194">That timer trigger code can then write hello test messages toohello queue.</span></span> <span data-ttu-id="6ce15-195">Ez a szakasz egy példán keresztül bemutatja, hogyan.</span><span class="sxs-lookup"><span data-stu-id="6ce15-195">This section walks through an example.</span></span>

<span data-ttu-id="6ce15-196">További információt az Azure Functions kötések használatával, lásd: hello [Azure Functions fejlesztői segédanyagai](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="6ce15-196">For more in-depth information on using bindings with Azure Functions, see hello [Azure Functions developer reference](functions-reference.md).</span></span>

#### <a name="create-a-queue-trigger-for-testing"></a><span data-ttu-id="6ce15-197">A létrehozott várólista tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ce15-197">Create a queue trigger for testing</span></span>
<span data-ttu-id="6ce15-198">toodemonstrate ezt a módszert használja, azt először létre kell hoznia egy várólista funkció, amely tootest szeretnénk nevű várólista `queue-newusers`.</span><span class="sxs-lookup"><span data-stu-id="6ce15-198">toodemonstrate this approach, we first create a queue trigger function that we want tootest for a queue named `queue-newusers`.</span></span> <span data-ttu-id="6ce15-199">Ez a funkció be egy új felhasználó a Queue storage eldobott nevét és címét adatokat dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="6ce15-199">This function processes name and address information dropped into Queue storage for a new user.</span></span>

> [!NOTE]
> <span data-ttu-id="6ce15-200">Ha egy másik várólistához nevet használja, ellenőrizze, hogy a használt hello név megfelel toohello [elnevezési üzenetsorok és metaadatok](https://msdn.microsoft.com/library/dd179349.aspx) szabályok.</span><span class="sxs-lookup"><span data-stu-id="6ce15-200">If you use a different queue name, make sure hello name you use conforms toohello [Naming Queues and MetaData](https://msdn.microsoft.com/library/dd179349.aspx) rules.</span></span> <span data-ttu-id="6ce15-201">Ellenkező esetben hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="6ce15-201">Otherwise, you get an error.</span></span>
>
>

1. <span data-ttu-id="6ce15-202">A hello [Azure-portálon] függvény alkalmazás, kattintson a **új függvény** > **QueueTrigger - C#**.</span><span class="sxs-lookup"><span data-stu-id="6ce15-202">In hello [Azure portal] for your function app, click **New Function** > **QueueTrigger - C#**.</span></span>
2. <span data-ttu-id="6ce15-203">Adja meg a hello várólista neve toobe hello várólista függvény által figyelt:</span><span class="sxs-lookup"><span data-stu-id="6ce15-203">Enter hello queue name toobe monitored by hello queue function:</span></span>

        queue-newusers
3. <span data-ttu-id="6ce15-204">Kattintson a hello  **+**  tooselect gombra, vagy azt szeretné, hogy toouse hello storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6ce15-204">Click hello **+** button tooselect or create hello storage account you want toouse.</span></span> <span data-ttu-id="6ce15-205">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6ce15-205">Then click **Create**.</span></span>
4. <span data-ttu-id="6ce15-206">Hagyja megnyitva, a portál böngészőablakot, így hello naplóbejegyzések hello sablon alapértelmezett várólista funkciókódot a figyelheti.</span><span class="sxs-lookup"><span data-stu-id="6ce15-206">Leave this portal browser window open, so you can monitor hello log entries for hello default queue function template code.</span></span>

#### <a name="create-a-timer-trigger-toodrop-a-message-in-hello-queue"></a><span data-ttu-id="6ce15-207">Egy időzítő eseményindító toodrop egy üzenet hello várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ce15-207">Create a timer trigger toodrop a message in hello queue</span></span>
1. <span data-ttu-id="6ce15-208">Nyissa meg hello [Azure-portálon] egy új böngészőablakban, és keresse meg a tooyour függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6ce15-208">Open hello [Azure portal] in a new browser window, and navigate tooyour function app.</span></span>
2. <span data-ttu-id="6ce15-209">Kattintson a **új függvény** > **TimerTrigger - C#**.</span><span class="sxs-lookup"><span data-stu-id="6ce15-209">Click **New Function** > **TimerTrigger - C#**.</span></span> <span data-ttu-id="6ce15-210">Adjon meg egy cron-kifejezés tooset milyen gyakran hello időzítő kód teszteli a várólista függvény.</span><span class="sxs-lookup"><span data-stu-id="6ce15-210">Enter a cron expression tooset how often hello timer code tests your queue function.</span></span> <span data-ttu-id="6ce15-211">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6ce15-211">Then click **Create**.</span></span> <span data-ttu-id="6ce15-212">Ha azt szeretné, hello teszt toorun 30 másodpercenként, hello következő használható [CRON-kifejezés](https://wikipedia.org/wiki/Cron#CRON_expression):</span><span class="sxs-lookup"><span data-stu-id="6ce15-212">If you want hello test toorun every 30 seconds, you can use hello following [CRON expression](https://wikipedia.org/wiki/Cron#CRON_expression):</span></span>

        */30 * * * * *
3. <span data-ttu-id="6ce15-213">Kattintson a hello **integráció** lap az új időzítő eseményindító.</span><span class="sxs-lookup"><span data-stu-id="6ce15-213">Click hello **Integrate** tab for your new timer trigger.</span></span>
4. <span data-ttu-id="6ce15-214">A **kimeneti**, kattintson a **+ új kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="6ce15-214">Under **Output**, click **+ New Output**.</span></span> <span data-ttu-id="6ce15-215">Kattintson a **várólista** és **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="6ce15-215">Then click **queue** and **Select**.</span></span>
5. <span data-ttu-id="6ce15-216">Megjegyzés: hello nevét használja hello **várólista message objektumot**.</span><span class="sxs-lookup"><span data-stu-id="6ce15-216">Note hello name you use for hello **queue message object**.</span></span> <span data-ttu-id="6ce15-217">Ezzel a hello időzítő funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="6ce15-217">You use this in hello timer function code.</span></span>

        myQueue
6. <span data-ttu-id="6ce15-218">Adja meg hello várólista neve, ahol hello üzenetet küld:</span><span class="sxs-lookup"><span data-stu-id="6ce15-218">Enter hello queue name where hello message is sent:</span></span>

        queue-newusers
7. <span data-ttu-id="6ce15-219">Kattintson a hello  **+**  tooselect hello hello várólista eseményindító a korábban használt tárfiók gombra.</span><span class="sxs-lookup"><span data-stu-id="6ce15-219">Click hello **+** button tooselect hello storage account you used previously with hello queue trigger.</span></span> <span data-ttu-id="6ce15-220">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="6ce15-220">Then click **Save**.</span></span>
8. <span data-ttu-id="6ce15-221">Kattintson a hello **Develop** az időzítő eseményindító fülre.</span><span class="sxs-lookup"><span data-stu-id="6ce15-221">Click hello **Develop** tab for your timer trigger.</span></span>
9. <span data-ttu-id="6ce15-222">A következő kód hello C# időzítő függvény, hello mindaddig, amíg az azonos feldolgozási sor üzenetet objektumnév korábban bemutatott hello használt használhatja.</span><span class="sxs-lookup"><span data-stu-id="6ce15-222">You can use hello following code for hello C# timer function, as long as you used hello same queue message object name shown earlier.</span></span> <span data-ttu-id="6ce15-223">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="6ce15-223">Then click **Save**.</span></span>

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

<span data-ttu-id="6ce15-224">Ezen a ponton hello C# időzítő hajtja végre a 30 másodpercenként hello példa cron-kifejezés esetén.</span><span class="sxs-lookup"><span data-stu-id="6ce15-224">At this point, hello C# timer function executes every 30 seconds if you used hello example cron expression.</span></span> <span data-ttu-id="6ce15-225">hello naplókat hello időzítő függvény a jelentés minden egyes végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="6ce15-225">hello logs for hello timer function report each execution:</span></span>

    2016-03-24T10:27:02  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.004 Function started (Id=04061790-974f-4043-b851-48bd4ac424d1)
    2016-03-24T10:27:30.004 C# Timer trigger function executed at: 3/24/2016 10:27:30 AM
    2016-03-24T10:27:30.004 {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.004 Function completed (Success, Id=04061790-974f-4043-b851-48bd4ac424d1)

<span data-ttu-id="6ce15-226">Hello böngészőablakban hello várólista függvény látható minden egyes feldolgozott üzenet:</span><span class="sxs-lookup"><span data-stu-id="6ce15-226">In hello browser window for hello queue function, you can see each message being processed:</span></span>

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)

## <a name="test-a-function-with-code"></a><span data-ttu-id="6ce15-227">Egy függvény kóddal tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ce15-227">Test a function with code</span></span>
<span data-ttu-id="6ce15-228">Szükség lehet egy külső alkalmazás- vagy keretrendszer tootest toocreate függvényeit.</span><span class="sxs-lookup"><span data-stu-id="6ce15-228">You may need toocreate an external application or framework tootest your functions.</span></span>

### <a name="test-an-http-trigger-function-with-code-nodejs"></a><span data-ttu-id="6ce15-229">Egy HTTP-eseményindító függvény kóddal tesztelése: Node.js</span><span class="sxs-lookup"><span data-stu-id="6ce15-229">Test an HTTP trigger function with code: Node.js</span></span>
<span data-ttu-id="6ce15-230">A Node.js-alkalmazás tooexecute egy HTTP-kérelem tootest a funkció használata.</span><span class="sxs-lookup"><span data-stu-id="6ce15-230">You can use a Node.js app tooexecute an HTTP request tootest your function.</span></span>
<span data-ttu-id="6ce15-231">Győződjön meg arról, hogy tooset:</span><span class="sxs-lookup"><span data-stu-id="6ce15-231">Make sure tooset:</span></span>

* <span data-ttu-id="6ce15-232">Hello `host` hello kérelem beállítások tooyour függvény app fogadó.</span><span class="sxs-lookup"><span data-stu-id="6ce15-232">hello `host` in hello request options tooyour function app host.</span></span>
* <span data-ttu-id="6ce15-233">A függvény nevét a hello `path`.</span><span class="sxs-lookup"><span data-stu-id="6ce15-233">Your function name in hello `path`.</span></span>
* <span data-ttu-id="6ce15-234">A hozzáférési kódját (`<your code>`) a hello `path`.</span><span class="sxs-lookup"><span data-stu-id="6ce15-234">Your access code (`<your code>`) in hello `path`.</span></span>

<span data-ttu-id="6ce15-235">Példa:</span><span class="sxs-lookup"><span data-stu-id="6ce15-235">Code example:</span></span>

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


<span data-ttu-id="6ce15-236">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="6ce15-236">Output:</span></span>

    C:\Users\Wesley\testing\Node.js>node testHttpTriggerExample.js
    *** Sending name and address in body ***
    {"name" : "Wes testing with Node.JS code","address" : "Dallas, T.X. 75201"}
    Hello Wes testing with Node.JS code
    hello address you provided is Dallas, T.X. 75201

<span data-ttu-id="6ce15-237">Hello portálon **naplók** ablakban kimeneti hasonló toohello következő hello függvény végrehajtása van bejelentkezve:</span><span class="sxs-lookup"><span data-stu-id="6ce15-237">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T08:08:55  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:08:59.736 Function started (Id=607b891c-08a1-427f-910c-af64ae4f7f9c)
    2016-03-23T08:09:01.153 HTTP trigger function processed a request. RequestUri=http://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1/?code=XXXXXXXXXX==
    2016-03-23T08:09:01.153 Request Headers = {"connection":"Keep-Alive","host":"functionsExample.azurewebsites.net"}
    2016-03-23T08:09:01.153 Name not provided as query string param. Checking body...
    2016-03-23T08:09:01.153 Request Body Type = object
    2016-03-23T08:09:01.153 Request Body = [object Object]
    2016-03-23T08:09:01.153 Processing User Information...
    2016-03-23T08:09:01.215 Function completed (Success, Id=607b891c-08a1-427f-910c-af64ae4f7f9c)


### <a name="test-a-queue-trigger-function-with-code-c"></a><span data-ttu-id="6ce15-238">A várólista funkció kóddal tesztelése: C#</span><span class="sxs-lookup"><span data-stu-id="6ce15-238">Test a queue trigger function with code: C#</span></span> #
<span data-ttu-id="6ce15-239">Azt a korábban említett, hogy egy várólista eseményindító kód toodrop egy üzenetet a várólistában lévő segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="6ce15-239">We mentioned earlier that you can test a queue trigger by using code toodrop a message in your queue.</span></span> <span data-ttu-id="6ce15-240">a következő példakód hello hello C#-kód jelenik meg a hello alapuló [Ismerkedés az Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="6ce15-240">hello following example code is based on hello C# code presented in hello [Getting started with Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) tutorial.</span></span> <span data-ttu-id="6ce15-241">Kód más nyelveken érhető el az adott hivatkozáson.</span><span class="sxs-lookup"><span data-stu-id="6ce15-241">Code for other languages is also available from that link.</span></span>

<span data-ttu-id="6ce15-242">tootest kell ezt a kódot egy konzol alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="6ce15-242">tootest this code in a console app, you must:</span></span>

* <span data-ttu-id="6ce15-243">[A tárolási kapcsolati karakterlánc konfigurálása hello app.config fájlban](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="6ce15-243">[Configure your storage connection string in hello app.config file](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>
* <span data-ttu-id="6ce15-244">Adjon át egy `name` és `address` paraméterek toohello alkalmazásként.</span><span class="sxs-lookup"><span data-stu-id="6ce15-244">Pass a `name` and `address` as parameters toohello app.</span></span> <span data-ttu-id="6ce15-245">Például: `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`.</span><span class="sxs-lookup"><span data-stu-id="6ce15-245">For example, `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`.</span></span> <span data-ttu-id="6ce15-246">(Ez a kód fogad el hello nevét és címét egy új felhasználó parancssori argumentumok futásidőben.)</span><span class="sxs-lookup"><span data-stu-id="6ce15-246">(This code accepts hello name and address for a new user as command-line arguments during runtime.)</span></span>

<span data-ttu-id="6ce15-247">C# példakódot:</span><span class="sxs-lookup"><span data-stu-id="6ce15-247">Example C# code:</span></span>

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

<span data-ttu-id="6ce15-248">Hello böngészőablakban hello várólista függvény látható minden egyes feldolgozott üzenet:</span><span class="sxs-lookup"><span data-stu-id="6ce15-248">In hello browser window for hello queue function, you can see each message being processed:</span></span>

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"Wes testing queues","address":"in a console app"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)


<!-- URLs. -->

[Azure-portálon]: https://portal.azure.com

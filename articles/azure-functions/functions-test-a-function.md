---
title: "Az Azure Functions tesztelése |} Microsoft Docs"
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
ms.openlocfilehash: aca03ba4137893157fcbe6650336782ab88cd234
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a><span data-ttu-id="8cd63-104">A kód az Azure Functions tesztelése kapcsolatos olyan stratégiák</span><span class="sxs-lookup"><span data-stu-id="8cd63-104">Strategies for testing your code in Azure Functions</span></span>

<span data-ttu-id="8cd63-105">Ebben a témakörben bemutatjuk a különböző módszereket funkciókkal, beleértve a következő általános módszerek használatával teszteléséhez:</span><span class="sxs-lookup"><span data-stu-id="8cd63-105">This topic demonstrates the various ways to test functions, including using the following general approaches:</span></span>

+ <span data-ttu-id="8cd63-106">HTTP-alapú eszközök, például cURL Postman vagy webes eseményindítók még egy webes böngésző</span><span class="sxs-lookup"><span data-stu-id="8cd63-106">HTTP-based tools, such as cURL, Postman, and even a web browser for web-based triggers</span></span>
+ <span data-ttu-id="8cd63-107">Az Azure Tártallózó, Azure Storage-alapú eseményindítók tesztelése</span><span class="sxs-lookup"><span data-stu-id="8cd63-107">Azure Storage Explorer, to test Azure Storage-based triggers</span></span>
+ <span data-ttu-id="8cd63-108">Az Azure Functions portálon teszt lap</span><span class="sxs-lookup"><span data-stu-id="8cd63-108">Test tab in the Azure Functions portal</span></span>
+ <span data-ttu-id="8cd63-109">Időzítő-eseményindítóval aktivált függvény</span><span class="sxs-lookup"><span data-stu-id="8cd63-109">Timer-triggered function</span></span>
+ <span data-ttu-id="8cd63-110">Alkalmazás vagy a keretrendszer tesztelése</span><span class="sxs-lookup"><span data-stu-id="8cd63-110">Testing application or framework</span></span>

<span data-ttu-id="8cd63-111">A tesztelési módszerek használják az egy HTTP funkció, amely a bemeneti egy lekérdezési karakterlánc paramétert vagy a kérelem törzsében keresztül fogadja.</span><span class="sxs-lookup"><span data-stu-id="8cd63-111">All these testing methods use an HTTP trigger function that accepts input through either a query string parameter or the request body.</span></span> <span data-ttu-id="8cd63-112">Ez a függvény az első szakaszban hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8cd63-112">You create this function in the first section.</span></span>

## <a name="create-a-function-for-testing"></a><span data-ttu-id="8cd63-113">Tesztelési függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="8cd63-113">Create a function for testing</span></span>
<span data-ttu-id="8cd63-114">Ez az oktatóanyag a legtöbb, a függvény létrehozása esetén érhető el HttpTrigger JavaScript-függvény sablon kis mértékben módosított verzióját használjuk.</span><span class="sxs-lookup"><span data-stu-id="8cd63-114">For most of this tutorial, we use a slightly modified version of the HttpTrigger JavaScript function template that is available when you create a function.</span></span> <span data-ttu-id="8cd63-115">Ha a függvény létrehozásához segítségre van szüksége, tekintse meg a [oktatóanyag](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="8cd63-115">If you need help creating a function, review this [tutorial](functions-create-first-azure-function.md).</span></span> <span data-ttu-id="8cd63-116">Válassza ki a **HttpTrigger - JavaScript** sablon teszt funkció létrehozásakor a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="8cd63-116">Choose the **HttpTrigger- JavaScript** template when creating the test function in the [Azure portal].</span></span>

<span data-ttu-id="8cd63-117">Az alapértelmezett függvény sablon alapjában echók vissza a nevét, a kérelem törzsében vagy lekérdezési karakterlánc-paraméter, a "hello world" függvény `name=<your name>`.</span><span class="sxs-lookup"><span data-stu-id="8cd63-117">The default function template is basically a "hello world" function that echoes back the name from the request body or query string parameter, `name=<your name>`.</span></span>  <span data-ttu-id="8cd63-118">Azt is lehetővé teszi, hogy adja meg a nevet és egy címet a kérelem törzsében szereplő JSON-tartalomként a kód frissítése.</span><span class="sxs-lookup"><span data-stu-id="8cd63-118">We'll update the code to also allow you to provide the name and an address as JSON content in the request body.</span></span> <span data-ttu-id="8cd63-119">Ezután a függvény echók ezek vissza az ügyfélnek, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="8cd63-119">Then the function echoes these back to the client when available.</span></span>   

<span data-ttu-id="8cd63-120">Frissítse a függvény a következő kódra, amely a tesztelési használjuk:</span><span class="sxs-lookup"><span data-stu-id="8cd63-120">Update the function with the following code, which we will use for testing:</span></span>

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
            body: "Please pass a name on the query string or in the request body"
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
        echoString += "\n" + "The address you provided is " + address;
        context.log("address = " + address);
    }
    res = {
        // status: 200, /* Defaults to 200 */
        body: echoString
    };
    return res;
}
```

## <a name="test-a-function-with-tools"></a><span data-ttu-id="8cd63-121">Függvény eszközök és tesztelése</span><span class="sxs-lookup"><span data-stu-id="8cd63-121">Test a function with tools</span></span>
<span data-ttu-id="8cd63-122">Az Azure-portálon kívül vannak különböző eszközök, amelyek használatával indul el, a funkciók vizsgálatához.</span><span class="sxs-lookup"><span data-stu-id="8cd63-122">Outside the Azure portal, there are various tools that you can use to trigger your functions for testing.</span></span> <span data-ttu-id="8cd63-123">Ezek közé tartozik a HTTP-eszközök (Felhasználóifelület-alapú, mind parancs sor), Azure Storage access eszközök és még egy egyszerű webböngésző tesztelése.</span><span class="sxs-lookup"><span data-stu-id="8cd63-123">These include HTTP testing tools (both UI-based and command line), Azure Storage access tools, and even a simple web browser.</span></span>

### <a name="test-with-a-browser"></a><span data-ttu-id="8cd63-124">Egy böngésző tesztelése</span><span class="sxs-lookup"><span data-stu-id="8cd63-124">Test with a browser</span></span>
<span data-ttu-id="8cd63-125">A webböngésző egyszerű módja a eseményindító funkciók HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="8cd63-125">The web browser is a simple way to trigger functions via HTTP.</span></span> <span data-ttu-id="8cd63-126">A GET-kérésekhez, amelyekhez nem szükséges a szervezet hasznos adatok között használható egy böngészőt, és használata csak lekérdezési karakterlánc-paraméterek.</span><span class="sxs-lookup"><span data-stu-id="8cd63-126">You can use a browser for GET requests that do not require a body payload, and that use only query string parameters.</span></span>

<span data-ttu-id="8cd63-127">Az ellenőrzéshez a korábban meghatározott függvény másolni a **függvény URL-cím** a portálról.</span><span class="sxs-lookup"><span data-stu-id="8cd63-127">To test the function we defined earlier, copy the **Function Url** from the portal.</span></span> <span data-ttu-id="8cd63-128">Rendelkezik a következő formában:</span><span class="sxs-lookup"><span data-stu-id="8cd63-128">It has the following form:</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="8cd63-129">Hozzáfűzése a `name` a lekérdezési karakterlánc paramétert.</span><span class="sxs-lookup"><span data-stu-id="8cd63-129">Append the `name` parameter to the query string.</span></span> <span data-ttu-id="8cd63-130">Egy tényleges nevét használja a `<Enter a name here>` helyőrző.</span><span class="sxs-lookup"><span data-stu-id="8cd63-130">Use an actual name for the `<Enter a name here>` placeholder.</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>&name=<Enter a name here>

<span data-ttu-id="8cd63-131">Illessze be az URL-CÍMÉT a böngészőbe, és szerezheti be a válasz a következőhöz hasonló.</span><span class="sxs-lookup"><span data-stu-id="8cd63-131">Paste the URL into your browser, and you should get a response similar to the following.</span></span>

![Képernyőfelvétel, Chrome böngésző lapon teszt válasz](./media/functions-test-a-function/browser-test.png)

<span data-ttu-id="8cd63-133">Ebben a példában a Chrome böngészőben becsomagolja a visszaadott karakterlánc az XML-Fájlban.</span><span class="sxs-lookup"><span data-stu-id="8cd63-133">This example is the Chrome browser, which wraps the returned string in XML.</span></span> <span data-ttu-id="8cd63-134">A más böngészőkkel csak a karakterlánc értéket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8cd63-134">Other browsers display just the string value.</span></span>

<span data-ttu-id="8cd63-135">A portál **naplók** ablakban kimenet az alábbihoz hasonló naplózott, a függvény végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8cd63-135">In the portal **Logs** window, output similar to the following is logged in executing the function:</span></span>

    2016-03-23T07:34:59  Welcome, you are now connected to log-streaming service.
    2016-03-23T07:35:09.195 Function started (Id=61a8c5a9-5e44-4da0-909d-91d293f20445)
    2016-03-23T07:35:10.338 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==&name=Glenn from a browser
    2016-03-23T07:35:10.338 Request Headers = {"cache-control":"max-age=0","connection":"Keep-Alive","accept":"text/html","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T07:35:10.338 Name was provided as a query string param.
    2016-03-23T07:35:10.338 Processing User Information...
    2016-03-23T07:35:10.369 Function completed (Success, Id=61a8c5a9-5e44-4da0-909d-91d293f20445)

### <a name="test-with-postman"></a><span data-ttu-id="8cd63-136">A Postman tesztelése</span><span class="sxs-lookup"><span data-stu-id="8cd63-136">Test with Postman</span></span>
<span data-ttu-id="8cd63-137">A funkciók a legtöbb teszteléséhez ajánlott eszköze Postman, amely integrálható a Chrome böngészőben.</span><span class="sxs-lookup"><span data-stu-id="8cd63-137">The recommended tool to test most of your functions is Postman, which integrates with the Chrome browser.</span></span> <span data-ttu-id="8cd63-138">Postman telepítéséhez tekintse át [beolvasása Postman](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="8cd63-138">To install Postman, see [Get Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="8cd63-139">Postman segítségével szabályozhatja, HTTP-kérések számos további attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="8cd63-139">Postman provides control over many more attributes of an HTTP request.</span></span>

> [!TIP]
> <span data-ttu-id="8cd63-140">A vizsgálati eszköz, amely ismeri a Feladatkezelő HTTP használata.</span><span class="sxs-lookup"><span data-stu-id="8cd63-140">Use the HTTP testing tool that you are most comfortable with.</span></span> <span data-ttu-id="8cd63-141">Az alábbiakban néhány más Postman:</span><span class="sxs-lookup"><span data-stu-id="8cd63-141">Here are some alternatives to Postman:</span></span>  
>
> * [<span data-ttu-id="8cd63-142">Fiddler</span><span class="sxs-lookup"><span data-stu-id="8cd63-142">Fiddler</span></span>](http://www.telerik.com/fiddler)  
> * [<span data-ttu-id="8cd63-143">Paw</span><span class="sxs-lookup"><span data-stu-id="8cd63-143">Paw</span></span>](https://luckymarmot.com/paw)  
>
>

<span data-ttu-id="8cd63-144">A függvény egy kérelemtörzset tesztelése a Postman:</span><span class="sxs-lookup"><span data-stu-id="8cd63-144">To test the function with a request body in Postman:</span></span>

1. <span data-ttu-id="8cd63-145">Indítsa el a Postman a **alkalmazások** gomb a böngészőablakba bal felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="8cd63-145">Start Postman from the **Apps** button in the upper-left corner of a Chrome browser window.</span></span>
2. <span data-ttu-id="8cd63-146">Másolás a **függvény URL-cím**, majd illessze be a Postman.</span><span class="sxs-lookup"><span data-stu-id="8cd63-146">Copy your **Function Url**, and paste it into Postman.</span></span> <span data-ttu-id="8cd63-147">Ez magában foglalja a hozzáférési kód lekérdezési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="8cd63-147">It includes the access code query string parameter.</span></span>
3. <span data-ttu-id="8cd63-148">Módosítsa a HTTP-metódus **POST**.</span><span class="sxs-lookup"><span data-stu-id="8cd63-148">Change the HTTP method to **POST**.</span></span>
4. <span data-ttu-id="8cd63-149">Kattintson a **törzs** > **nyers**, és adja hozzá a következőhöz hasonló JSON kérelemtörzset:</span><span class="sxs-lookup"><span data-stu-id="8cd63-149">Click **Body** > **raw**, and add a JSON request body similar to the following:</span></span>

    ```json
    {
        "name" : "Wes testing with Postman",
        "address" : "Seattle, WA 98101"
    }
    ```
5. <span data-ttu-id="8cd63-150">Kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="8cd63-150">Click **Send**.</span></span>

<span data-ttu-id="8cd63-151">A következő kép bemutatja, hogy az oktatóanyag az egyszerű echo függvény példa tesztelése.</span><span class="sxs-lookup"><span data-stu-id="8cd63-151">The following image shows testing the simple echo function example in this tutorial.</span></span>

![Képernyőfelvétel a Postman felhasználói felülete](./media/functions-test-a-function/postman-test.png)

<span data-ttu-id="8cd63-153">A portál **naplók** ablakban kimenet az alábbihoz hasonló naplózott, a függvény végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8cd63-153">In the portal **Logs** window, output similar to the following is logged in executing the function:</span></span>

    2016-03-23T08:04:51  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:04:57.107 Function started (Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)
    2016-03-23T08:04:57.763 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==
    2016-03-23T08:04:57.763 Request Headers = {"cache-control":"no-cache","connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:04:57.763 Processing user info from request body...
    2016-03-23T08:04:57.763 Processing User Information...
    2016-03-23T08:04:57.763 name = Wes testing with Postman
    2016-03-23T08:04:57.763 address = Seattle, W.A. 98101
    2016-03-23T08:04:57.795 Function completed (Success, Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)

### <a name="test-with-curl-from-the-command-line"></a><span data-ttu-id="8cd63-154">Tesztelje a cURL a parancssorból</span><span class="sxs-lookup"><span data-stu-id="8cd63-154">Test with cURL from the command line</span></span>
<span data-ttu-id="8cd63-155">Gyakran amikor tesztelést szoftver, nincs szükség minden további, mint a parancssor segítségével az alkalmazás hibakeresését kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="8cd63-155">Often when you're testing software, it's not necessary to look any further than the command line to help debug your application.</span></span> <span data-ttu-id="8cd63-156">Ez nem eltér a functions tesztelése.</span><span class="sxs-lookup"><span data-stu-id="8cd63-156">This is no different with testing functions.</span></span> <span data-ttu-id="8cd63-157">Ne feledje, hogy a cURL alapértelmezés szerint a Linux-alapú rendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="8cd63-157">Note that the cURL is available by default on Linux-based systems.</span></span> <span data-ttu-id="8cd63-158">A Windows, először le kell töltenie és telepítse a [cURL eszköz](https://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="8cd63-158">On Windows, you must first download and install the [cURL tool](https://curl.haxx.se/).</span></span>

<span data-ttu-id="8cd63-159">A függvény, amely korábban meghatározott teszteléséhez másolja a **függvény URL-cím** a portálról.</span><span class="sxs-lookup"><span data-stu-id="8cd63-159">To test the function that we defined earlier, copy the **Function URL** from the portal.</span></span> <span data-ttu-id="8cd63-160">Rendelkezik a következő formában:</span><span class="sxs-lookup"><span data-stu-id="8cd63-160">It has the following form:</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="8cd63-161">Ez a indítására, a függvény az URL-cím.</span><span class="sxs-lookup"><span data-stu-id="8cd63-161">This is the URL for triggering your function.</span></span> <span data-ttu-id="8cd63-162">Használjon a cURL-parancsot a parancssorból egy GET végrehajtásához (`-G` vagy `--get`) függvény kérelmet:</span><span class="sxs-lookup"><span data-stu-id="8cd63-162">Test this by using the cURL command on the command line to make a GET (`-G` or `--get`) request against the function:</span></span>

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="8cd63-163">Ebben a példában a lekérdezési karakterlánc paraméterként, amelyek adatként átadhatók igényel (`-d`) található a cURL-parancsot:</span><span class="sxs-lookup"><span data-stu-id="8cd63-163">This particular example requires a query string parameter, which can be passed as Data (`-d`) in the cURL command:</span></span>

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code> -d name=<Enter a name here>

<span data-ttu-id="8cd63-164">Futtassa a parancsot, és a függvény a következő kimenet jelenik meg a parancssorban:</span><span class="sxs-lookup"><span data-stu-id="8cd63-164">Run the command, and you see the following output of the function on the command line:</span></span>

![Képernyőkép a parancssori kimenet](./media/functions-test-a-function/curl-test.png)

<span data-ttu-id="8cd63-166">A portál **naplók** ablakban kimenet az alábbihoz hasonló naplózott, a függvény végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8cd63-166">In the portal **Logs** window, output similar to the following is logged in executing the function:</span></span>

    2016-04-05T21:55:09  Welcome, you are now connected to log-streaming service.
    2016-04-05T21:55:30.738 Function started (Id=ae6955da-29db-401a-b706-482fcd1b8f7a)
    2016-04-05T21:55:30.738 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/HttpTriggerNodeJS1?code=XXXXXXX&name=Azure Functions
    2016-04-05T21:55:30.738 Function completed (Success, Id=ae6955da-29db-401a-b706-482fcd1b8f7a)

### <a name="test-a-blob-trigger-by-using-storage-explorer"></a><span data-ttu-id="8cd63-167">Egy blob eseményindító tesztelése Tártallózó használatával</span><span class="sxs-lookup"><span data-stu-id="8cd63-167">Test a blob trigger by using Storage Explorer</span></span>
<span data-ttu-id="8cd63-168">Egy blob funkció segítségével tesztelheti [Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="8cd63-168">You can test a blob trigger function by using [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

1. <span data-ttu-id="8cd63-169">Az a [Azure-portálon] függvény alkalmazás, hozzon létre egy C#, F # vagy JavaScript blob eseményindító függvényt.</span><span class="sxs-lookup"><span data-stu-id="8cd63-169">In the [Azure portal] for your function app, create a C#, F# or JavaScript blob trigger function.</span></span> <span data-ttu-id="8cd63-170">A blob-tároló neve a figyelheti a elérési útjának beállítása.</span><span class="sxs-lookup"><span data-stu-id="8cd63-170">Set the path to monitor to the name of your blob container.</span></span> <span data-ttu-id="8cd63-171">Példa:</span><span class="sxs-lookup"><span data-stu-id="8cd63-171">For example:</span></span>

        files
2. <span data-ttu-id="8cd63-172">Kattintson a  **+**  gombra, válassza ki vagy hozzon létre a használni kívánt tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="8cd63-172">Click the **+** button to select or create the storage account you want to use.</span></span> <span data-ttu-id="8cd63-173">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8cd63-173">Then click **Create**.</span></span>
3. <span data-ttu-id="8cd63-174">A következő szöveggel hozzon létre egy szövegfájlt, és mentse azt:</span><span class="sxs-lookup"><span data-stu-id="8cd63-174">Create a text file with the following text, and save it:</span></span>

        A text file for blob trigger function testing.
4. <span data-ttu-id="8cd63-175">Futtatás [Azure Tártallózó](http://storageexplorer.com/), és kapcsolódjon a figyelt tárfiók a blob tároló.</span><span class="sxs-lookup"><span data-stu-id="8cd63-175">Run [Azure Storage Explorer](http://storageexplorer.com/), and connect to the blob container in the storage account being monitored.</span></span>
5. <span data-ttu-id="8cd63-176">Kattintson a **feltöltése** a szöveges fájl feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="8cd63-176">Click **Upload** to upload the text file.</span></span>

    ![Képernyőkép a Tártallózó alkalmazással](./media/functions-test-a-function/azure-storage-explorer-test.png)

<span data-ttu-id="8cd63-178">Az alapértelmezett blob eseményindító függvény kód a naplókban lévő blob feldolgozása jelentések:</span><span class="sxs-lookup"><span data-stu-id="8cd63-178">The default blob trigger function code reports the processing of the blob in the logs:</span></span>

    2016-03-24T11:30:10  Welcome, you are now connected to log-streaming service.
    2016-03-24T11:30:34.472 Function started (Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)
    2016-03-24T11:30:34.472 C# Blob trigger function processed: A text file for blob trigger function testing.
    2016-03-24T11:30:34.472 Function completed (Success, Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)

## <a name="test-a-function-within-functions"></a><span data-ttu-id="8cd63-179">Egy függvény funkciók tesztelése</span><span class="sxs-lookup"><span data-stu-id="8cd63-179">Test a function within functions</span></span>
<span data-ttu-id="8cd63-180">A portál az célja, hogy lehetővé teszik, hogy HTTP tesztelése az Azure Functions és indított időzítő funkciók.</span><span class="sxs-lookup"><span data-stu-id="8cd63-180">The Azure Functions portal is designed to let you test HTTP and timer triggered functions.</span></span> <span data-ttu-id="8cd63-181">Egyéb, a tesztelni kívánt funkciót kiváltásához funkciók is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="8cd63-181">You can also create functions to trigger other functions that you are testing.</span></span>

### <a name="test-with-the-functions-portal-run-button"></a><span data-ttu-id="8cd63-182">A funkciók portál Futtatás gombra a teszt</span><span class="sxs-lookup"><span data-stu-id="8cd63-182">Test with the Functions portal Run button</span></span>
<span data-ttu-id="8cd63-183">A portál biztosít egy **futtatása** gombra, hogy használhatják-bizonyos korlátozott tesztelése.</span><span class="sxs-lookup"><span data-stu-id="8cd63-183">The portal provides a **Run** button that you can use to do some limited testing.</span></span> <span data-ttu-id="8cd63-184">A gombra kattintva megadhatja egy kérelemtörzset, de nem adja meg a lekérdezési karakterlánc paramétereket, illetve nem frissíthető a kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="8cd63-184">You can provide a request body by using the button, but you can't provide query string parameters or update request headers.</span></span>

<span data-ttu-id="8cd63-185">A HTTP-eseményindító a következőhöz hasonló JSON karakterláncnak hozzáadásával korábban létrehozott függvény tesztelése a **Request body** mező.</span><span class="sxs-lookup"><span data-stu-id="8cd63-185">Test the HTTP trigger function we created earlier by adding a JSON string similar to the following in the **Request body** field.</span></span> <span data-ttu-id="8cd63-186">Kattintson a **futtatása** gombra.</span><span class="sxs-lookup"><span data-stu-id="8cd63-186">Then click the **Run** button.</span></span>

```json
{
    "name" : "Wes testing Run button",
    "address" : "USA"
}
```

<span data-ttu-id="8cd63-187">A portál **naplók** ablakban kimenet az alábbihoz hasonló naplózott, a függvény végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8cd63-187">In the portal **Logs** window, output similar to the following is logged in executing the function:</span></span>

    2016-03-23T08:03:12  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:03:17.357 Function started (Id=753a01b0-45a8-4125-a030-3ad543a89409)
    2016-03-23T08:03:18.697 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/wesmchttptriggernodejs1
    2016-03-23T08:03:18.697 Request Headers = {"connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:03:18.697 Processing user info from request body...
    2016-03-23T08:03:18.697 Processing User Information...
    2016-03-23T08:03:18.697 name = Wes testing Run button
    2016-03-23T08:03:18.697 address = USA
    2016-03-23T08:03:18.744 Function completed (Success, Id=753a01b0-45a8-4125-a030-3ad543a89409)


### <a name="test-with-a-timer-trigger"></a><span data-ttu-id="8cd63-188">Az időzítő eseményindító tesztelése</span><span class="sxs-lookup"><span data-stu-id="8cd63-188">Test with a timer trigger</span></span>
<span data-ttu-id="8cd63-189">Egyes funkciók nem megfelelően tesztelni a korábban említett eszközök.</span><span class="sxs-lookup"><span data-stu-id="8cd63-189">Some functions can't be adequately tested with the tools mentioned previously.</span></span> <span data-ttu-id="8cd63-190">Tegyük fel, amely akkor fut, amikor egy üzenet megszakad a várólista eseményindító függvény [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="8cd63-190">For example, consider a queue trigger function that runs when a message is dropped into [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span> <span data-ttu-id="8cd63-191">Mindig írhat kódot a dobja el az üzenetet a várólistán, és példákat a konzol projektben áll a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="8cd63-191">You can always write code to drop a message into your queue, and an example of this in a console project is provided later in this article.</span></span> <span data-ttu-id="8cd63-192">Van azonban egy másik módszert alkalmaz is használhat, amely közvetlenül tesztek funkciók.</span><span class="sxs-lookup"><span data-stu-id="8cd63-192">However, there is another approach you can use that tests functions directly.</span></span>  

<span data-ttu-id="8cd63-193">Egy időzítő indítófeltételt konfigurált üzenetsorokat használhatja kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="8cd63-193">You can use a timer trigger configured with a queue output binding.</span></span> <span data-ttu-id="8cd63-194">Hogy időzítő aktiváló kód majd írhat a tesztüzenetet a várakozási sorba.</span><span class="sxs-lookup"><span data-stu-id="8cd63-194">That timer trigger code can then write the test messages to the queue.</span></span> <span data-ttu-id="8cd63-195">Ez a szakasz egy példán keresztül bemutatja, hogyan.</span><span class="sxs-lookup"><span data-stu-id="8cd63-195">This section walks through an example.</span></span>

<span data-ttu-id="8cd63-196">További információt az Azure Functions kötések használatával, lásd: a [Azure Functions fejlesztői segédanyagai](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="8cd63-196">For more in-depth information on using bindings with Azure Functions, see the [Azure Functions developer reference](functions-reference.md).</span></span>

#### <a name="create-a-queue-trigger-for-testing"></a><span data-ttu-id="8cd63-197">A létrehozott várólista tesztelése</span><span class="sxs-lookup"><span data-stu-id="8cd63-197">Create a queue trigger for testing</span></span>
<span data-ttu-id="8cd63-198">Bemutatják, ezt a módszert használja, azt először létre kell hoznia egy várólista funkció, amely teszteléséhez nevű várólista szeretnénk `queue-newusers`.</span><span class="sxs-lookup"><span data-stu-id="8cd63-198">To demonstrate this approach, we first create a queue trigger function that we want to test for a queue named `queue-newusers`.</span></span> <span data-ttu-id="8cd63-199">Ez a funkció be egy új felhasználó a Queue storage eldobott nevét és címét adatokat dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="8cd63-199">This function processes name and address information dropped into Queue storage for a new user.</span></span>

> [!NOTE]
> <span data-ttu-id="8cd63-200">Ha egy másik várólistához nevet használja, győződjön meg arról, hogy a használt név megfelel-e a [elnevezési üzenetsorok és metaadatok](https://msdn.microsoft.com/library/dd179349.aspx) szabályok.</span><span class="sxs-lookup"><span data-stu-id="8cd63-200">If you use a different queue name, make sure the name you use conforms to the [Naming Queues and MetaData](https://msdn.microsoft.com/library/dd179349.aspx) rules.</span></span> <span data-ttu-id="8cd63-201">Ellenkező esetben hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="8cd63-201">Otherwise, you get an error.</span></span>
>
>

1. <span data-ttu-id="8cd63-202">Az a [Azure-portálon] függvény alkalmazás, kattintson a **új függvény** > **QueueTrigger - C#**.</span><span class="sxs-lookup"><span data-stu-id="8cd63-202">In the [Azure portal] for your function app, click **New Function** > **QueueTrigger - C#**.</span></span>
2. <span data-ttu-id="8cd63-203">Adja meg a várólista nevét a várólista függvény által figyelt:</span><span class="sxs-lookup"><span data-stu-id="8cd63-203">Enter the queue name to be monitored by the queue function:</span></span>

        queue-newusers
3. <span data-ttu-id="8cd63-204">Kattintson a  **+**  gombra, válassza ki vagy hozzon létre a használni kívánt tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="8cd63-204">Click the **+** button to select or create the storage account you want to use.</span></span> <span data-ttu-id="8cd63-205">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8cd63-205">Then click **Create**.</span></span>
4. <span data-ttu-id="8cd63-206">Hagyja megnyitva, a portál böngészőablakot, figyelheti a naplóbejegyzéseket, az alapértelmezett várólista függvény sablon kódot.</span><span class="sxs-lookup"><span data-stu-id="8cd63-206">Leave this portal browser window open, so you can monitor the log entries for the default queue function template code.</span></span>

#### <a name="create-a-timer-trigger-to-drop-a-message-in-the-queue"></a><span data-ttu-id="8cd63-207">Hozzon létre egy időzítő indítófeltételt üzenetet a várólistában lévő elvetése érdekében</span><span class="sxs-lookup"><span data-stu-id="8cd63-207">Create a timer trigger to drop a message in the queue</span></span>
1. <span data-ttu-id="8cd63-208">Nyissa meg a [Azure-portálon] egy új böngészőablakban, és keresse meg a függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8cd63-208">Open the [Azure portal] in a new browser window, and navigate to your function app.</span></span>
2. <span data-ttu-id="8cd63-209">Kattintson a **új függvény** > **TimerTrigger - C#**.</span><span class="sxs-lookup"><span data-stu-id="8cd63-209">Click **New Function** > **TimerTrigger - C#**.</span></span> <span data-ttu-id="8cd63-210">Adja meg a beállítása, hogy milyen gyakran az időzítő kód teszteli a várólista függvény cron-kifejezést.</span><span class="sxs-lookup"><span data-stu-id="8cd63-210">Enter a cron expression to set how often the timer code tests your queue function.</span></span> <span data-ttu-id="8cd63-211">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8cd63-211">Then click **Create**.</span></span> <span data-ttu-id="8cd63-212">Ha a vizsgálatot, 30 másodperces futtatásához, a következő használható [CRON-kifejezés](https://wikipedia.org/wiki/Cron#CRON_expression):</span><span class="sxs-lookup"><span data-stu-id="8cd63-212">If you want the test to run every 30 seconds, you can use the following [CRON expression](https://wikipedia.org/wiki/Cron#CRON_expression):</span></span>

        */30 * * * * *
3. <span data-ttu-id="8cd63-213">Kattintson a **integráció** lap az új időzítő eseményindító.</span><span class="sxs-lookup"><span data-stu-id="8cd63-213">Click the **Integrate** tab for your new timer trigger.</span></span>
4. <span data-ttu-id="8cd63-214">A **kimeneti**, kattintson a **+ új kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="8cd63-214">Under **Output**, click **+ New Output**.</span></span> <span data-ttu-id="8cd63-215">Kattintson a **várólista** és **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="8cd63-215">Then click **queue** and **Select**.</span></span>
5. <span data-ttu-id="8cd63-216">Megjegyzés: a nevet használja a **várólista message objektumot**.</span><span class="sxs-lookup"><span data-stu-id="8cd63-216">Note the name you use for the **queue message object**.</span></span> <span data-ttu-id="8cd63-217">Ezzel a időzítő függvény kódban.</span><span class="sxs-lookup"><span data-stu-id="8cd63-217">You use this in the timer function code.</span></span>

        myQueue
6. <span data-ttu-id="8cd63-218">Adja meg a várólista nevét, ahol a hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="8cd63-218">Enter the queue name where the message is sent:</span></span>

        queue-newusers
7. <span data-ttu-id="8cd63-219">Kattintson a  **+**  gombra kattintva válassza ki a korábban használt a várólista eseményindító tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="8cd63-219">Click the **+** button to select the storage account you used previously with the queue trigger.</span></span> <span data-ttu-id="8cd63-220">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8cd63-220">Then click **Save**.</span></span>
8. <span data-ttu-id="8cd63-221">Kattintson a **Develop** az időzítő eseményindító fülre.</span><span class="sxs-lookup"><span data-stu-id="8cd63-221">Click the **Develop** tab for your timer trigger.</span></span>
9. <span data-ttu-id="8cd63-222">A következő kódot használhatja a C# időzítő függvény, mindaddig, amíg a várólista üzenet objektum néven korábban bemutatott használta.</span><span class="sxs-lookup"><span data-stu-id="8cd63-222">You can use the following code for the C# timer function, as long as you used the same queue message object name shown earlier.</span></span> <span data-ttu-id="8cd63-223">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8cd63-223">Then click **Save**.</span></span>

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

<span data-ttu-id="8cd63-224">Ezen a ponton a C# időzítő végrehajtja 30 másodpercenként, ha követte a példa cron-kifejezés.</span><span class="sxs-lookup"><span data-stu-id="8cd63-224">At this point, the C# timer function executes every 30 seconds if you used the example cron expression.</span></span> <span data-ttu-id="8cd63-225">A naplókat az időzítő jelentést minden egyes végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8cd63-225">The logs for the timer function report each execution:</span></span>

    2016-03-24T10:27:02  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.004 Function started (Id=04061790-974f-4043-b851-48bd4ac424d1)
    2016-03-24T10:27:30.004 C# Timer trigger function executed at: 3/24/2016 10:27:30 AM
    2016-03-24T10:27:30.004 {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.004 Function completed (Success, Id=04061790-974f-4043-b851-48bd4ac424d1)

<span data-ttu-id="8cd63-226">A várólista függvény böngészőablakban minden egyes üzenet feldolgozás alatt látható:</span><span class="sxs-lookup"><span data-stu-id="8cd63-226">In the browser window for the queue function, you can see each message being processed:</span></span>

    2016-03-24T10:27:06  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)

## <a name="test-a-function-with-code"></a><span data-ttu-id="8cd63-227">Egy függvény kóddal tesztelése</span><span class="sxs-lookup"><span data-stu-id="8cd63-227">Test a function with code</span></span>
<span data-ttu-id="8cd63-228">Szükség lehet egy külső alkalmazás vagy keretrendszer, a funkciók ellenőrzéséhez hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="8cd63-228">You may need to create an external application or framework to test your functions.</span></span>

### <a name="test-an-http-trigger-function-with-code-nodejs"></a><span data-ttu-id="8cd63-229">Egy HTTP-eseményindító függvény kóddal tesztelése: Node.js</span><span class="sxs-lookup"><span data-stu-id="8cd63-229">Test an HTTP trigger function with code: Node.js</span></span>
<span data-ttu-id="8cd63-230">A Node.js-alkalmazás segítségével hajtható végre egy HTTP-kérést a függvény tesztelése.</span><span class="sxs-lookup"><span data-stu-id="8cd63-230">You can use a Node.js app to execute an HTTP request to test your function.</span></span>
<span data-ttu-id="8cd63-231">Feltétlenül állítson be:</span><span class="sxs-lookup"><span data-stu-id="8cd63-231">Make sure to set:</span></span>

* <span data-ttu-id="8cd63-232">A `host` a kérelem-beállításokban, a függvény app gazdagépre.</span><span class="sxs-lookup"><span data-stu-id="8cd63-232">The `host` in the request options to your function app host.</span></span>
* <span data-ttu-id="8cd63-233">A függvény nevét a `path`.</span><span class="sxs-lookup"><span data-stu-id="8cd63-233">Your function name in the `path`.</span></span>
* <span data-ttu-id="8cd63-234">A hozzáférési kódját (`<your code>`) található a `path`.</span><span class="sxs-lookup"><span data-stu-id="8cd63-234">Your access code (`<your code>`) in the `path`.</span></span>

<span data-ttu-id="8cd63-235">Példa:</span><span class="sxs-lookup"><span data-stu-id="8cd63-235">Code example:</span></span>

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


<span data-ttu-id="8cd63-236">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="8cd63-236">Output:</span></span>

    C:\Users\Wesley\testing\Node.js>node testHttpTriggerExample.js
    *** Sending name and address in body ***
    {"name" : "Wes testing with Node.JS code","address" : "Dallas, T.X. 75201"}
    Hello Wes testing with Node.JS code
    The address you provided is Dallas, T.X. 75201

<span data-ttu-id="8cd63-237">A portál **naplók** ablakban kimenet az alábbihoz hasonló naplózott, a függvény végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8cd63-237">In the portal **Logs** window, output similar to the following is logged in executing the function:</span></span>

    2016-03-23T08:08:55  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:08:59.736 Function started (Id=607b891c-08a1-427f-910c-af64ae4f7f9c)
    2016-03-23T08:09:01.153 HTTP trigger function processed a request. RequestUri=http://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1/?code=XXXXXXXXXX==
    2016-03-23T08:09:01.153 Request Headers = {"connection":"Keep-Alive","host":"functionsExample.azurewebsites.net"}
    2016-03-23T08:09:01.153 Name not provided as query string param. Checking body...
    2016-03-23T08:09:01.153 Request Body Type = object
    2016-03-23T08:09:01.153 Request Body = [object Object]
    2016-03-23T08:09:01.153 Processing User Information...
    2016-03-23T08:09:01.215 Function completed (Success, Id=607b891c-08a1-427f-910c-af64ae4f7f9c)


### <a name="test-a-queue-trigger-function-with-code-c"></a><span data-ttu-id="8cd63-238">A várólista funkció kóddal tesztelése: C#</span><span class="sxs-lookup"><span data-stu-id="8cd63-238">Test a queue trigger function with code: C#</span></span> #
<span data-ttu-id="8cd63-239">Azt a korábban említett, hogy várólista eseményindító eldobásához egy üzenetet a várólistában lévő kód segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8cd63-239">We mentioned earlier that you can test a queue trigger by using code to drop a message in your queue.</span></span> <span data-ttu-id="8cd63-240">Az alábbi példakód C#-kódban szerepel a alapul a [Ismerkedés az Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="8cd63-240">The following example code is based on the C# code presented in the [Getting started with Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) tutorial.</span></span> <span data-ttu-id="8cd63-241">Kód más nyelveken érhető el az adott hivatkozáson.</span><span class="sxs-lookup"><span data-stu-id="8cd63-241">Code for other languages is also available from that link.</span></span>

<span data-ttu-id="8cd63-242">Ez a kód egy konzolalkalmazás teszteléséhez a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="8cd63-242">To test this code in a console app, you must:</span></span>

* <span data-ttu-id="8cd63-243">[A tárolási kapcsolati karakterlánc konfigurálása az app.config fájlban](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="8cd63-243">[Configure your storage connection string in the app.config file](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>
* <span data-ttu-id="8cd63-244">Adjon át egy `name` és `address` az alkalmazás paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="8cd63-244">Pass a `name` and `address` as parameters to the app.</span></span> <span data-ttu-id="8cd63-245">Például: `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`.</span><span class="sxs-lookup"><span data-stu-id="8cd63-245">For example, `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`.</span></span> <span data-ttu-id="8cd63-246">(Ez a kód fogad el nevét és címét egy új felhasználó parancssori argumentumok futásidőben.)</span><span class="sxs-lookup"><span data-stu-id="8cd63-246">(This code accepts the name and address for a new user as command-line arguments during runtime.)</span></span>

<span data-ttu-id="8cd63-247">C# példakódot:</span><span class="sxs-lookup"><span data-stu-id="8cd63-247">Example C# code:</span></span>

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

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference(queueName);

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    if (name != null)
    {
        if (address != null)
            JSON = String.Format("{{\"name\":\"{0}\",\"address\":\"{1}\"}}", name, address);
        else
            JSON = String.Format("{{\"name\":\"{0}\"}}", name);
    }

    Console.WriteLine("Adding message to " + queueName + "...");
    Console.WriteLine(JSON);

    CloudQueueMessage message = new CloudQueueMessage(JSON);
    queue.AddMessage(message);
}
```

<span data-ttu-id="8cd63-248">A várólista függvény böngészőablakban minden egyes üzenet feldolgozás alatt látható:</span><span class="sxs-lookup"><span data-stu-id="8cd63-248">In the browser window for the queue function, you can see each message being processed:</span></span>

    2016-03-24T10:27:06  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"Wes testing queues","address":"in a console app"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)


<!-- URLs. -->

[Azure-portálon]: https://portal.azure.com

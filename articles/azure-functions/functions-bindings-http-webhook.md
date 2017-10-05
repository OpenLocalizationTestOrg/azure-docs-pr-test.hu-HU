---
title: "Az Azure Functions HTTP és a webhook kötések |} Microsoft Docs"
description: "A HTTP és a webhook eseményindítók és kötések az Azure Functions használatának megismerése."
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
ms.openlocfilehash: 71c0d22c4b1824078982b9d1cc76645f947ae603
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-functions-http-and-webhook-bindings"></a><span data-ttu-id="5b3ca-104">Az Azure Functions HTTP és a webhook kötések</span><span class="sxs-lookup"><span data-stu-id="5b3ca-104">Azure Functions HTTP and webhook bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="5b3ca-105">Ez a cikk azt ismerteti, hogyan konfigurálását és a HTTP-eseményindítók és kötések az Azure Functions használatát.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-105">This article explains how to configure and work with HTTP triggers and bindings in Azure Functions.</span></span>
<span data-ttu-id="5b3ca-106">Ezek segítségével az Azure Functions kiszolgáló nélküli API-k létrehozása, és webhookokkal reagálhat rájuk.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-106">With these, you can use Azure Functions to build serverless APIs and respond to webhooks.</span></span>

<span data-ttu-id="5b3ca-107">Az Azure Functions a következő kötéseket biztosít:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-107">Azure Functions provides the following bindings:</span></span>
- <span data-ttu-id="5b3ca-108">Egy [HTTP-eseményindítóval](#httptrigger) lehetővé teszi, hogy a HTTP-kérelem a függvényt.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-108">An [HTTP trigger](#httptrigger) lets you invoke a function with an HTTP request.</span></span> <span data-ttu-id="5b3ca-109">A testre szabható válaszolni [webhookok](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-109">This can be customized to respond to [webhooks](#hooktrigger).</span></span>
- <span data-ttu-id="5b3ca-110">Egy [HTTP kimeneti kötése](#output) lehetővé teszi a válaszol a kérelemre.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-110">An [HTTP output binding](#output) allows you to respond to the request.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a><span data-ttu-id="5b3ca-111">HTTP eseményindító</span><span class="sxs-lookup"><span data-stu-id="5b3ca-111">HTTP trigger</span></span>
<span data-ttu-id="5b3ca-112">A HTTP-eseményindítóval végrehajtja a függvény egy HTTP-kérelemre adott válasz.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-112">The HTTP trigger will execute your function in response to an HTTP request.</span></span> <span data-ttu-id="5b3ca-113">Testre szabhatja, hogy egy adott URL-cím vagy HTTP-metódus válaszolni.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-113">You can customize it to respond to a particular URL or set of HTTP methods.</span></span> <span data-ttu-id="5b3ca-114">Egy HTTP-eseményindítóval is beállítható úgy, hogy webhookok válaszolni.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-114">An HTTP trigger can also be configured to respond to webhooks.</span></span> 

<span data-ttu-id="5b3ca-115">A funkciók portál használata esetén is elkezdheti rögtön az előre elkészített sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-115">If using the Functions portal, you can also get started right away using a pre-made template.</span></span> <span data-ttu-id="5b3ca-116">Válassza ki **új függvény** és válassza az "API és Webhookokkal" lehetőséget a **forgatókönyv** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-116">Select **New function** and choose "API & Webhooks" from the **Scenario** dropdown.</span></span> <span data-ttu-id="5b3ca-117">Válassza ki a sablonok egyikét, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-117">Select one of the templates and click **Create**.</span></span>

<span data-ttu-id="5b3ca-118">Alapértelmezés szerint a kérelem olyan HTTP 200 OK állapotkódot és azt egy üres szövegtörzzsel válaszol egy HTTP-eseményindítóval.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-118">By default, an HTTP trigger will respond to the request with an HTTP 200 OK status code and an empty body.</span></span> <span data-ttu-id="5b3ca-119">A válasz módosításához konfigurálása egy [HTTP kimeneti kötése](#output)</span><span class="sxs-lookup"><span data-stu-id="5b3ca-119">To modify the response, configure an [HTTP output binding](#output)</span></span>

### <a name="configuring-an-http-trigger"></a><span data-ttu-id="5b3ca-120">Egy HTTP-eseményindítóval konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5b3ca-120">Configuring an HTTP trigger</span></span>
<span data-ttu-id="5b3ca-121">Egy HTTP-eseményindítóval induló JSON-objektumnak az alábbihoz hasonló, beleértve a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-121">An HTTP trigger is defined by including a JSON object similar to the following in the `bindings` array of function.json:</span></span>

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
<span data-ttu-id="5b3ca-122">A kötés támogatja-e a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-122">The binding supports the following properties:</span></span>

* <span data-ttu-id="5b3ca-123">**név** : szükséges – a kérelem vagy kérelemtörzset függvény a kódban használt változó neve.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-123">**name** : Required - the variable name used in function code for the request or request body.</span></span> <span data-ttu-id="5b3ca-124">Lásd: [egy HTTP-eseményindítóval kódból használata](#httptriggerusage).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-124">See [Working with an HTTP trigger from code](#httptriggerusage).</span></span>
* <span data-ttu-id="5b3ca-125">**típus** : szükséges – "httpTrigger" értékűre kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-125">**type** : Required - must be set to "httpTrigger".</span></span>
* <span data-ttu-id="5b3ca-126">**irány** : szükséges – "a" értékűre kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-126">**direction** : Required - must be set to "in".</span></span>
* <span data-ttu-id="5b3ca-127">_authLevel_ : Ez határozza meg, mi kulcsok, ha van ilyen kell jelen lennie ahhoz, hogy a függvény meghívása a kérésre.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-127">_authLevel_ : This determines what keys, if any, need to be present on the request in order to invoke the function.</span></span> <span data-ttu-id="5b3ca-128">Lásd: [kulcsoknál](#keys) alatt.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-128">See [Working with keys](#keys) below.</span></span> <span data-ttu-id="5b3ca-129">Az érték a következők egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-129">The value can be one of the following:</span></span>
    * <span data-ttu-id="5b3ca-130">_Névtelen_: nem API-kulcsot meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-130">_anonymous_: No API key is required.</span></span>
    * <span data-ttu-id="5b3ca-131">_függvény_: egy funkcióspecifikus API-kulcsot meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-131">_function_: A function-specific API key is required.</span></span> <span data-ttu-id="5b3ca-132">Ez az az alapértelmezett érték, ha nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-132">This is the default value if none is provided.</span></span>
    * <span data-ttu-id="5b3ca-133">_felügyeleti_ : A fő kulcsot meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-133">_admin_ : The master key is required.</span></span>
* <span data-ttu-id="5b3ca-134">**módszerek** : Ez a, amelyre a függvény válaszol a HTTP-metódus tömbjét.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-134">**methods** : This is an array of the HTTP methods to which the function will respond.</span></span> <span data-ttu-id="5b3ca-135">Ha nincs megadva, a függvény az összes HTTP-metódus fog válaszolni.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-135">If not specified, the function will respond to all HTTP methods.</span></span> <span data-ttu-id="5b3ca-136">Lásd: [testreszabása a HTTP-végpont](#url).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-136">See [Customizing the HTTP endpoint](#url).</span></span>
* <span data-ttu-id="5b3ca-137">**útvonal** : Ez határozza meg, hogy az útvonal-sablon, a funkció hamarosan beszámol szabályozása, amelyhez a kérés URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-137">**route** : This defines the route template, controlling to which request URLs your function will respond.</span></span> <span data-ttu-id="5b3ca-138">Az alapértelmezett érték, ha nincs megadva `<functionname>`.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-138">The default value if none is provided is `<functionname>`.</span></span> <span data-ttu-id="5b3ca-139">Lásd: [testreszabása a HTTP-végpont](#url).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-139">See [Customizing the HTTP endpoint](#url).</span></span>
* <span data-ttu-id="5b3ca-140">**webHookType** : Ez konfigurálja, hogy a HTTP-eseményindítóval egy webhook reciever a megadott szolgáltató nevében járhasson el.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-140">**webHookType** : This configures the HTTP trigger to act as a webhook reciever for the specified provider.</span></span> <span data-ttu-id="5b3ca-141">A _módszerek_ tulajdonság nem állítható be, ha ezt választja.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-141">The _methods_ property should not be set if this is chosen.</span></span> <span data-ttu-id="5b3ca-142">Lásd: [válaszol a webhookok](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-142">See [Responding to webhooks](#hooktrigger).</span></span> <span data-ttu-id="5b3ca-143">Az érték a következők egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-143">The value can be one of the following:</span></span>
    * <span data-ttu-id="5b3ca-144">_genericJson_ : egy általános célú webhook végpont logika egy adott szolgáltató nélkül.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-144">_genericJson_ : A general purpose webhook endpoint without logic for a specific provider.</span></span>
    * <span data-ttu-id="5b3ca-145">_github_ : A függvény GitHub webhook fog válaszolni.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-145">_github_ : The function will respond to GitHub webhooks.</span></span> <span data-ttu-id="5b3ca-146">A _authLevel_ tulajdonság nem állítható be, ha ezt választja.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-146">The _authLevel_ property should not be set if this is chosen.</span></span>
    * <span data-ttu-id="5b3ca-147">_slackhez_ : A függvény Slack webhookok válaszol.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-147">_slack_ : The function will respond to Slack webhooks.</span></span> <span data-ttu-id="5b3ca-148">A _authLevel_ tulajdonság nem állítható be, ha ezt választja.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-148">The _authLevel_ property should not be set if this is chosen.</span></span>

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a><span data-ttu-id="5b3ca-149">Egy HTTP-eseményindítóval kódból használata</span><span class="sxs-lookup"><span data-stu-id="5b3ca-149">Working with an HTTP trigger from code</span></span>
<span data-ttu-id="5b3ca-150">C# és F # függvények, adjon meg lehet az eseményindító típusú deklarálhatnak `HttpRequestMessage` vagy olyan egyéni típusra.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-150">For C# and F# functions, you can declare the type of your trigger input to be either `HttpRequestMessage` or a custom type.</span></span> <span data-ttu-id="5b3ca-151">Ha úgy dönt, `HttpRequestMessage`, akkor a request objektumon teljes hozzáférést kap.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-151">If you choose `HttpRequestMessage`, then you will get full access to the request object.</span></span> <span data-ttu-id="5b3ca-152">Egy egyéni típust (például egy POCO), a funkciók megpróbál a kérés törzsében a objektum tulajdonságok JSON-ként értelmezni.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-152">For a custom type (such as a POCO), Functions will attempt to parse the request body as JSON to populate the object properties.</span></span>

<span data-ttu-id="5b3ca-153">A Functions futtatókörnyezete Node.js funkciók, itt a kérés törzsében helyett a request objektumon.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-153">For Node.js functions, the Functions runtime provides the request body instead of the request object.</span></span>

<span data-ttu-id="5b3ca-154">Lásd: [HTTP-eseményindító minták](#httptriggersample) például is érvényesek.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-154">See [HTTP trigger samples](#httptriggersample) for example usages.</span></span>


<a name="output"></a>
## <a name="http-response-output-binding"></a><span data-ttu-id="5b3ca-155">HTTP-válasz kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="5b3ca-155">HTTP response output binding</span></span>
<span data-ttu-id="5b3ca-156">Használja a HTTP kimeneti kötése a HTTP-kérést küldő válaszolni.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-156">Use the HTTP output binding to respond to the HTTP request sender.</span></span> <span data-ttu-id="5b3ca-157">A kötés egy HTTP-eseményindítóval igényel, és lehetővé teszi a válasz az eseményindító kérelemhez társított testreszabását.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-157">This binding requires an HTTP trigger and allows you to customize the response associated with the trigger's request.</span></span> <span data-ttu-id="5b3ca-158">Ha a HTTP kimeneti kötése van a nem Microsofttól származó, egy HTTP-eseményindítóval HTTP 200 OK visszatér egy üres szövegtörzzsel.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-158">If an HTTP output binding is not provided, an HTTP trigger will return HTTP 200 OK with an empty body.</span></span> 

### <a name="configuring-an-http-output-binding"></a><span data-ttu-id="5b3ca-159">HTTP konfigurálása kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="5b3ca-159">Configuring an HTTP output binding</span></span>
<span data-ttu-id="5b3ca-160">A HTTP kimeneti kötés definiált JSON-objektumnak az alábbihoz hasonló, beleértve a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-160">The HTTP output binding is defined by including a JSON object similar to the following in the `bindings` array of function.json:</span></span>

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
<span data-ttu-id="5b3ca-161">A kötés tartalmaz a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-161">The binding contains the following properties:</span></span>

* <span data-ttu-id="5b3ca-162">**név** : szükséges – a változó nevét, a válasz függvény kódban használt.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-162">**name** : Required - the variable name used in function code for the response.</span></span> <span data-ttu-id="5b3ca-163">Lásd: [HTTP használata kimeneti kötése kódból](#outputusage).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-163">See [Working with an HTTP output binding from code](#outputusage).</span></span>
* <span data-ttu-id="5b3ca-164">**típus** : szükséges – kell lennie a "http" értékre.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-164">**type** : Required - must be set to "http".</span></span>
* <span data-ttu-id="5b3ca-165">**irány** : szükséges – "ki" értékre kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-165">**direction** : Required - must be set to "out".</span></span>

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a><span data-ttu-id="5b3ca-166">HTTP használata kimeneti kötése kódból</span><span class="sxs-lookup"><span data-stu-id="5b3ca-166">Working with an HTTP output binding from code</span></span>
<span data-ttu-id="5b3ca-167">A kimeneti paraméter (például "res") segítségével válaszol a http- vagy webhook hívók számára.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-167">You can use the output parameter (e.g., "res") to respond to the http or webhook caller.</span></span> <span data-ttu-id="5b3ca-168">Másik lehetőségként használhatja a szabványos `Request.CreateResponse()` (C#) vagy `context.res` (Node.JS) mintát térjen vissza a válaszban.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-168">Alternatively, you can use the standard `Request.CreateResponse()` (C#) or `context.res` (Node.JS) pattern to return your response.</span></span> <span data-ttu-id="5b3ca-169">Az utóbbi metódus használatával, tekintse meg a [HTTP-eseményindító minták](#httptriggersample) és [Webhook eseményindító minták](#hooktriggersample).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-169">For examples on how to use the latter method, see [HTTP trigger samples](#httptriggersample) and [Webhook trigger samples](#hooktriggersample).</span></span>


<a name="hooktrigger"></a>
## <a name="responding-to-webhooks"></a><span data-ttu-id="5b3ca-170">Válaszol a webhookok</span><span class="sxs-lookup"><span data-stu-id="5b3ca-170">Responding to webhooks</span></span>
<span data-ttu-id="5b3ca-171">Egy HTTP eseményindító a _webHookType_ tulajdonság teszi válaszolni [webhookok](https://en.wikipedia.org/wiki/Webhook).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-171">An HTTP trigger with the _webHookType_ property will be configured to respond to [webhooks](https://en.wikipedia.org/wiki/Webhook).</span></span> <span data-ttu-id="5b3ca-172">Az alapszintű konfigurációs "genericJson" beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-172">The basic configuration uses the "genericJson" setting.</span></span> <span data-ttu-id="5b3ca-173">Ha csak a HTTP-n keresztül, POST és a kérelmek ez korlátozza a `application/json` tartalomtípus.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-173">This restricts requests to only those using HTTP POST and with the `application/json` content type.</span></span>

<span data-ttu-id="5b3ca-174">Az eseményindító továbbá testreszabható egy adott webhook szolgáltató (pl. [GitHub](https://developer.github.com/webhooks/) és [Slackhez](https://api.slack.com/outgoing-webhooks)).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-174">The trigger can additionally be tailored to a specific webhook provider (e.g., [GitHub](https://developer.github.com/webhooks/) and [Slack](https://api.slack.com/outgoing-webhooks)).</span></span> <span data-ttu-id="5b3ca-175">Ha egy szolgáltató van megadva, a Functions futtatókörnyezete is kezeli a szolgáltató ellenőrzési logika meg.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-175">If a provider is specified, the Functions runtime can take care of the provider's validation logic for you.</span></span>  

### <a name="configuring-github-as-a-webhook-provider"></a><span data-ttu-id="5b3ca-176">GitHub webhook szolgáltatóként konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5b3ca-176">Configuring GitHub as a webhook provider</span></span>
<span data-ttu-id="5b3ca-177">GitHub webhook válaszolni, először hozzon létre a függvény egy HTTP-eseményindítóval, és állítsa be a _webHookType_ "github" tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-177">To respond to GitHub webhooks, first create your function with an HTTP Trigger, and set the _webHookType_ property to "github".</span></span> <span data-ttu-id="5b3ca-178">Másolja a [URL-cím](#url) és [API-kulcs](#keys) a GitHub-tárházban történő **adja hozzá a webhook** lap.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-178">Then copy its [URL](#url) and [API key](#keys) into your GitHub repository's **Add webhook** page.</span></span> <span data-ttu-id="5b3ca-179">Lásd a GitHub [Webhook létrehozása](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) több dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-179">See GitHub's [Creating Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) documentation for more.</span></span>

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a><span data-ttu-id="5b3ca-180">A webhook szolgáltató Slackhez konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5b3ca-180">Configuring Slack as a webhook provider</span></span>
<span data-ttu-id="5b3ca-181">A Slack webhook létrehoz egy jogkivonatot helyett, és adja meg azt, konfigurálnia kell egy funkcióspecifikus kulcsot a jogkivonatot a Slackhez, így Önnek.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-181">The Slack webhook generates a token for you instead of letting you specify it, so you must configure a function-specific key with the token from Slack.</span></span> <span data-ttu-id="5b3ca-182">Lásd: [kulcsoknál](#keys).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-182">See [Working with keys](#keys).</span></span>

<a name="url"></a>
## <a name="customizing-the-http-endpoint"></a><span data-ttu-id="5b3ca-183">A HTTP-végpont testreszabása</span><span class="sxs-lookup"><span data-stu-id="5b3ca-183">Customizing the HTTP endpoint</span></span>
<span data-ttu-id="5b3ca-184">Alapértelmezés szerint amikor egy HTTP-eseményindítóval, vagy a WebHook, a függvény létrehozása a függvény megcímezhető útvonal a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-184">By default when you create a function for an HTTP trigger, or WebHook, the function is addressable with a route of the form:</span></span>

    http://<yourapp>.azurewebsites.net/api/<funcname> 

<span data-ttu-id="5b3ca-185">Ez az útvonal nem kötelező testreszabható `route` a HTTP-eseményindítóval tulajdonsága bemeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-185">You can customize this route using the optional `route` property on the HTTP trigger's input binding.</span></span> <span data-ttu-id="5b3ca-186">Tegyük fel, a következő *function.json* fájl határozza meg a `route` tulajdonság egy HTTP-eseményindító:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-186">As an example, the following *function.json* file defines a `route` property for an HTTP trigger:</span></span>

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

<span data-ttu-id="5b3ca-187">Ezt a konfigurációt használja, a függvény már megcímezhető helyett az eredeti útvonal a következő útvonal.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-187">Using this configuration, the function is now addressable with the following route instead of the original route.</span></span>

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

<span data-ttu-id="5b3ca-188">Ez lehetővé teszi, hogy a funkció támogatja a két paramétert a cím, a "kategória" és "id".</span><span class="sxs-lookup"><span data-stu-id="5b3ca-188">This allows the function code to support two parameters in the address, "category" and "id".</span></span> <span data-ttu-id="5b3ca-189">Akkor használhatja [webes API útvonal megkötés](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) a paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-189">You can use any [Web API Route Constraint](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) with your parameters.</span></span> <span data-ttu-id="5b3ca-190">A következő C# funkciókódot mindkét paraméter használ.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-190">The following C# function code makes use of both parameters.</span></span>

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

<span data-ttu-id="5b3ca-191">Ez az azonos útvonal-paraméterek használata a Node.js-függvény kód.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-191">Here is Node.js function code to use the same route parameters.</span></span>

```javascript
    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;

        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }

        context.done();
    } 
```

<span data-ttu-id="5b3ca-192">Alapértelmezés szerint az összes függvény útvonal előtagként *api*.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-192">By default, all function routes are prefixed with *api*.</span></span> <span data-ttu-id="5b3ca-193">Is testre szabhatja, vagy távolítsa el a előtag használatával a `http.routePrefix` tulajdonságot a *host.json* fájlt.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-193">You can also customize or remove the prefix using the `http.routePrefix` property in your *host.json* file.</span></span> <span data-ttu-id="5b3ca-194">A következő példában eltávolítjuk a *api* útvonal előtagja üres karakterlánc használja az-előtagot a *host.json* fájlt.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-194">The following example removes the *api* route prefix by using an empty string for the prefix in the *host.json* file.</span></span>

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

<span data-ttu-id="5b3ca-195">Részletes információkat, hogyan lehet frissíteni a *host.json* fájl, a függvény, tekintse meg, [függvény alkalmazásfájlok frissítése](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-195">For detailed information on how to update the *host.json* file for your function, See, [How to update function app files](functions-reference.md#fileupdate).</span></span> 

<span data-ttu-id="5b3ca-196">További tudnivalók egyéb konfigurálhatja a a *host.json* fájlba, lásd: [host.json hivatkozás](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-196">For information on other properties you can configure in your *host.json* file, see [host.json reference](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>


<a name="keys"></a>
## <a name="working-with-keys"></a><span data-ttu-id="5b3ca-197">Kulcsok használata</span><span class="sxs-lookup"><span data-stu-id="5b3ca-197">Working with keys</span></span>
<span data-ttu-id="5b3ca-198">A nagyobb biztonság kulcsok HttpTriggers használhatják fel.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-198">HttpTriggers can leverage keys for added security.</span></span> <span data-ttu-id="5b3ca-199">Egy szabványos HttpTrigger ennek segítségével API-kulcs, mint a kulcs szerepel a kérelem igénylő.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-199">A standard HttpTrigger can use these as an API key, requiring the key to be present on the request.</span></span> <span data-ttu-id="5b3ca-200">Webhook kulcsok segítségével többféle módon, attól függően, hogy a szolgáltató támogatja a kérelmek engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-200">Webhooks can use keys to authorize requests in a variety of ways, depending on what the provider supports.</span></span>

<span data-ttu-id="5b3ca-201">Kulcsok részeként a függvény alkalmazást az Azure-ban tároljuk, és inaktív titkosított.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-201">Keys are stored as part of your function app in Azure and are encrypted at rest.</span></span> <span data-ttu-id="5b3ca-202">A kulcsok megtekintéséhez újakat hozhat létre, vagy kulcsok állni új értékek váltson a függvényt a portálon, és válassza ki a "Kezelése."</span><span class="sxs-lookup"><span data-stu-id="5b3ca-202">To view your keys, create new ones, or roll keys to new values, navigate to one of your functions within the portal and select "Manage."</span></span> 

<span data-ttu-id="5b3ca-203">A kulcsok két típusa van:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-203">There are two types of keys:</span></span>
- <span data-ttu-id="5b3ca-204">**Kulcsok tárolására**: a függvény alkalmazáson belüli összes funkciók által megosztott ezeket a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-204">**Host keys**: These keys are shared by all functions within the function app.</span></span> <span data-ttu-id="5b3ca-205">Ha egy API-kulcsot, ezek a függvény alkalmazásban függvényeket elérését teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-205">When used as an API key, these allow access to any function within the function app.</span></span>
- <span data-ttu-id="5b3ca-206">**Funkcióbillentyűk**: ezek a kulcsok csak alkalmazása az adott funkciókhoz, amely alatt vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-206">**Function keys**: These keys apply only to the specific functions under which they are defined.</span></span> <span data-ttu-id="5b3ca-207">Ha egy API-kulcsot, ezek csak a hozzáférést a függvény.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-207">When used as an API key, these only allow access to that function.</span></span>

<span data-ttu-id="5b3ca-208">Minden kulcs neve referenciaként, és nincs (a "alapértelmezett" nevű) alapértelmezett kulcs függvény és a gazdagép szintjén.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-208">Each key is named for reference, and there is a default key (named "default") at the function and host level.</span></span> <span data-ttu-id="5b3ca-209">A **főkulcs** egy alapértelmezett állomáskulcs neve "_master", amely minden függvény alkalmazás van definiálva, és nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-209">The **master key** is a default host key named "_master" that is defined for each function app and cannot be revoked.</span></span> <span data-ttu-id="5b3ca-210">A futtatókörnyezet API-k rendszergazdai hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-210">It provides administrative access to the runtime APIs.</span></span> <span data-ttu-id="5b3ca-211">Használatával `"authLevel": "admin"` a kötés a JSON ezt a kulcsot a kérés jelenik meg a szükséges; bármely más kulcs engedélyezési hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-211">Using `"authLevel": "admin"` in the binding JSON will require this key to be presented on the request; any other key will result in a authorization failure.</span></span>

> [!NOTE]
> <span data-ttu-id="5b3ca-212">A főkulccsal adott emelt szintű engedélyek, mert nem kell ezt a kulcsot megosztása a harmadik felek vagy natív ügyfélalkalmazásokban eloszthatják azt.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-212">Due to the elevated permissions granted by the master key, you should not share this key with third parties or distribute it in native client applications.</span></span> <span data-ttu-id="5b3ca-213">Körültekintően járjon el a rendszergazdai jogosultsági szint kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-213">Exercise caution when choosing the admin authorization level.</span></span>
> 
> 

### <a name="api-key-authorization"></a><span data-ttu-id="5b3ca-214">API-kulcs engedélyezési</span><span class="sxs-lookup"><span data-stu-id="5b3ca-214">API key authorization</span></span>
<span data-ttu-id="5b3ca-215">Alapértelmezés szerint egy HttpTrigger a HTTP-kérelmek API-kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-215">By default, an HttpTrigger requires an API key in the HTTP request.</span></span> <span data-ttu-id="5b3ca-216">Ezért a HTTP-kérelmek általában néz ki:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-216">So your HTTP request normally looks like this:</span></span>

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

<span data-ttu-id="5b3ca-217">A kulcs tartalmazhat egy lekérdezési karakterlánc-változóvá nevű `code`, a fentiek szerint, vagy a része egy `x-functions-key` HTTP-fejléc.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-217">The key can be included in a query string variable named `code`, as above, or it can be included in an `x-functions-key` HTTP header.</span></span> <span data-ttu-id="5b3ca-218">A kulcsnak az értéke lehet bármely billentyűt, a függvény definiálva, vagy bármely állomás kulcsát.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-218">The value of the key can be any function key defined for the function, or any host key.</span></span>

<span data-ttu-id="5b3ca-219">Kérelmek kulcsok nélkül, vagy adja meg, hogy kell-e a fő oszlopkulcs használni módosításával választhatja a `authLevel` tulajdonság JSON kötésében (lásd: [HTTP-eseményindítóval](#httptrigger)).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-219">You can choose to allow requests without keys or specify that the master key must be used by changing the `authLevel` property in the binding JSON (see [HTTP trigger](#httptrigger)).</span></span>

### <a name="keys-and-webhooks"></a><span data-ttu-id="5b3ca-220">Kulcsok és webhookokkal</span><span class="sxs-lookup"><span data-stu-id="5b3ca-220">Keys and webhooks</span></span>
<span data-ttu-id="5b3ca-221">Webhook engedélyezési kezeli a webhook reciever összetevőt, a HttpTrigger része, és a mechanizmus függ a webhook típusa.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-221">Webhook authorization is handled by the webhook reciever component, part of the HttpTrigger, and the mechanism varies based on the webhook type.</span></span> <span data-ttu-id="5b3ca-222">Minden egyes mechanizmus nem, a kulcs azonban támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-222">Each mechanism does, however rely on a key.</span></span> <span data-ttu-id="5b3ca-223">Alapértelmezés szerint az "alapértelmezett" nevű funkció kulcs használható.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-223">By default, the function key named "default" will be used.</span></span> <span data-ttu-id="5b3ca-224">Ha szeretne egy másik kulcsot használ, szüksége lesz a webhook szolgáltatót, amelyet a kulcs neve a kérés küldése a következő módokon konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-224">If you wish to use a different key, you will need to configure the webhook provider to send the key name with the request in one of the following ways:</span></span>

- <span data-ttu-id="5b3ca-225">**Lekérdezési karakterlánc**: A szolgáltató továbbítja a kulcs nevét a `clientid` lekérdezési karakterlánc (pl. `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span><span class="sxs-lookup"><span data-stu-id="5b3ca-225">**Query string**: The provider passes the key name in the `clientid` query string parameter (e.g., `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span></span>
- <span data-ttu-id="5b3ca-226">**Kérelemfejléc**: A szolgáltató továbbítja a kulcs nevét a `x-functions-clientid` fejléc.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-226">**Request header**: The provider passes the key name in the `x-functions-clientid` header.</span></span>

> [!NOTE]
> <span data-ttu-id="5b3ca-227">Funkcióbillentyűk élveznek állomások kulcsait.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-227">Function keys take precedence over host keys.</span></span> <span data-ttu-id="5b3ca-228">Ha két kulcsok vannak meghatározva, ugyanazzal a névvel, a függvény kulcs használható.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-228">If two keys are defined with the same name, the function key will be used.</span></span>
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a><span data-ttu-id="5b3ca-229">HTTP-eseményindító minták</span><span class="sxs-lookup"><span data-stu-id="5b3ca-229">HTTP trigger samples</span></span>
<span data-ttu-id="5b3ca-230">Tegyük fel, hogy rendelkezik a következő HTTP-eseményindítóval a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-230">Suppose you have the following HTTP trigger in the `bindings` array of function.json:</span></span>

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

<span data-ttu-id="5b3ca-231">A nyelvspecifikus mintát keres, tekintse meg a `name` paraméter, a lekérdezési karakterláncot vagy a HTTP-kérelem törzsét.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-231">See the language-specific sample that looks for a `name` parameter either in the query string or the body of the HTTP request.</span></span>

* [<span data-ttu-id="5b3ca-232">C#</span><span class="sxs-lookup"><span data-stu-id="5b3ca-232">C#</span></span>](#httptriggercsharp)
* [<span data-ttu-id="5b3ca-233">F#</span><span class="sxs-lookup"><span data-stu-id="5b3ca-233">F#</span></span>](#httptriggerfsharp)
* [<span data-ttu-id="5b3ca-234">Node.js</span><span class="sxs-lookup"><span data-stu-id="5b3ca-234">Node.js</span></span>](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a><span data-ttu-id="5b3ca-235">A C# HTTP-eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="5b3ca-235">HTTP trigger sample in C#</span></span> #
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

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

<span data-ttu-id="5b3ca-236">Ahelyett, hogy egy POCO is köthető `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-236">You can also bind to a POCO instead of `HttpRequestMessage`.</span></span> <span data-ttu-id="5b3ca-237">Ez a kérelem JSON-ként értelmezni a szervezetnek hidratált lesz.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-237">This will be hydrated from the body of the request, parsed as JSON.</span></span> <span data-ttu-id="5b3ca-238">Hasonlóképpen egy típust a kötés HTTP-válasz kimenetének adhatók át, és ez változatlanul adódik vissza az adott válasz törzsének 200 állapotkóddal.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-238">Similarly, a type can be passed to the HTTP response output binding, and this will be returned as the response body, with a 200 status code.</span></span>
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
### <a name="http-trigger-sample-in-f"></a><span data-ttu-id="5b3ca-239">Az F # HTTP-eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="5b3ca-239">HTTP trigger sample in F#</span></span> #
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
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

<span data-ttu-id="5b3ca-240">Kell egy `project.json` fájlt, amely a NuGet való hivatkozáshoz használja a `FSharp.Interop.Dynamic` és `Dynamitey` szerelvényeket, ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-240">You need a `project.json` file that uses NuGet to reference the `FSharp.Interop.Dynamic` and `Dynamitey` assemblies, like this:</span></span>

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

<span data-ttu-id="5b3ca-241">Ez fogja használni a NuGet beolvasása a függőségek, és a parancsfájlban hivatkoznak rájuk.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-241">This will use NuGet to fetch your dependencies and will reference them in your script.</span></span>

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a><span data-ttu-id="5b3ca-242">A Node.JS-ben HTTP-eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="5b3ca-242">HTTP trigger sample in Node.JS</span></span>
```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```



<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a><span data-ttu-id="5b3ca-243">Webhook minták</span><span class="sxs-lookup"><span data-stu-id="5b3ca-243">Webhook samples</span></span>
<span data-ttu-id="5b3ca-244">Tegyük fel, hogy rendelkezik a következő webhook eseményindító a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="5b3ca-244">Suppose you have the following webhook trigger in the `bindings` array of function.json:</span></span>

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

<span data-ttu-id="5b3ca-245">Tekintse meg a nyelvspecifikus mintát, amelyre bejelentkezik GitHub probléma megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="5b3ca-245">See the language-specific sample that logs GitHub issue comments.</span></span>

* [<span data-ttu-id="5b3ca-246">C#</span><span class="sxs-lookup"><span data-stu-id="5b3ca-246">C#</span></span>](#hooktriggercsharp)
* [<span data-ttu-id="5b3ca-247">F#</span><span class="sxs-lookup"><span data-stu-id="5b3ca-247">F#</span></span>](#hooktriggerfsharp)
* [<span data-ttu-id="5b3ca-248">Node.js</span><span class="sxs-lookup"><span data-stu-id="5b3ca-248">Node.js</span></span>](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a><span data-ttu-id="5b3ca-249">A C# Webhook minta</span><span class="sxs-lookup"><span data-stu-id="5b3ca-249">Webhook sample in C#</span></span> #
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

### <a name="webhook-sample-in-f"></a><span data-ttu-id="5b3ca-250">Az F # Webhook minta</span><span class="sxs-lookup"><span data-stu-id="5b3ca-250">Webhook sample in F#</span></span> #
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

### <a name="webhook-sample-in-nodejs"></a><span data-ttu-id="5b3ca-251">A node.js Webhook minta</span><span class="sxs-lookup"><span data-stu-id="5b3ca-251">Webhook sample in Node.JS</span></span>
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a><span data-ttu-id="5b3ca-252">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5b3ca-252">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


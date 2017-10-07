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
# <a name="azure-functions-http-and-webhook-bindings"></a><span data-ttu-id="8ca72-104">Az Azure Functions HTTP és a webhook kötések</span><span class="sxs-lookup"><span data-stu-id="8ca72-104">Azure Functions HTTP and webhook bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="8ca72-105">Ez a cikk ismerteti, hogyan tooconfigure és a HTTP használata eseményindítók és kötések az Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8ca72-105">This article explains how tooconfigure and work with HTTP triggers and bindings in Azure Functions.</span></span>
<span data-ttu-id="8ca72-106">A fenti kiszolgáló nélküli-es számú Azure Functions toobuild API-k és válaszoljon rendszerhez készült toowebhooks is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8ca72-106">With these, you can use Azure Functions toobuild serverless APIs and respond toowebhooks.</span></span>

<span data-ttu-id="8ca72-107">Az Azure Functions a következő kötések hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="8ca72-107">Azure Functions provides hello following bindings:</span></span>
- <span data-ttu-id="8ca72-108">Egy [HTTP-eseményindítóval](#httptrigger) lehetővé teszi, hogy a HTTP-kérelem a függvényt.</span><span class="sxs-lookup"><span data-stu-id="8ca72-108">An [HTTP trigger](#httptrigger) lets you invoke a function with an HTTP request.</span></span> <span data-ttu-id="8ca72-109">Ez lehet túl testreszabott toorespond[webhookok](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="8ca72-109">This can be customized toorespond too[webhooks](#hooktrigger).</span></span>
- <span data-ttu-id="8ca72-110">Egy [HTTP kimeneti kötése](#output) toorespond toohello kérelem lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="8ca72-110">An [HTTP output binding](#output) allows you toorespond toohello request.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a><span data-ttu-id="8ca72-111">HTTP eseményindító</span><span class="sxs-lookup"><span data-stu-id="8ca72-111">HTTP trigger</span></span>
<span data-ttu-id="8ca72-112">hello HTTP-eseményindítóval válasz tooan HTTP-kérelmek az függvény hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="8ca72-112">hello HTTP trigger will execute your function in response tooan HTTP request.</span></span> <span data-ttu-id="8ca72-113">Toorespond tooa adott URL-cím vagy a HTTP-metódus halmaza testreszabható.</span><span class="sxs-lookup"><span data-stu-id="8ca72-113">You can customize it toorespond tooa particular URL or set of HTTP methods.</span></span> <span data-ttu-id="8ca72-114">Egy HTTP-eseményindítóval konfigurált toorespond toowebhooks is lehet.</span><span class="sxs-lookup"><span data-stu-id="8ca72-114">An HTTP trigger can also be configured toorespond toowebhooks.</span></span> 

<span data-ttu-id="8ca72-115">Ha hello funkciók portál használata esetén is elkezdheti rögtön az előre elkészített sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="8ca72-115">If using hello Functions portal, you can also get started right away using a pre-made template.</span></span> <span data-ttu-id="8ca72-116">Válassza ki **új függvény** és választható "API és Webhookokkal" hello **forgatókönyv** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="8ca72-116">Select **New function** and choose "API & Webhooks" from hello **Scenario** dropdown.</span></span> <span data-ttu-id="8ca72-117">Válasszon egyet a hello sablonok, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8ca72-117">Select one of hello templates and click **Create**.</span></span>

<span data-ttu-id="8ca72-118">Alapértelmezés szerint HTTP-eseményindítóval válaszol egy HTTP 200 OK állapotkódot és azt egy üres szövegtörzzsel toohello kérelmet.</span><span class="sxs-lookup"><span data-stu-id="8ca72-118">By default, an HTTP trigger will respond toohello request with an HTTP 200 OK status code and an empty body.</span></span> <span data-ttu-id="8ca72-119">toomodify hello válaszban, konfigurálja az [HTTP kimeneti kötése](#output)</span><span class="sxs-lookup"><span data-stu-id="8ca72-119">toomodify hello response, configure an [HTTP output binding](#output)</span></span>

### <a name="configuring-an-http-trigger"></a><span data-ttu-id="8ca72-120">Egy HTTP-eseményindítóval konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8ca72-120">Configuring an HTTP trigger</span></span>
<span data-ttu-id="8ca72-121">Határozza meg egy HTTP-eseményindítóval többek között a JSON objektum hasonló toohello hello a következő `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="8ca72-121">An HTTP trigger is defined by including a JSON object similar toohello following in hello `bindings` array of function.json:</span></span>

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
<span data-ttu-id="8ca72-122">hello kötés támogatja-e hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="8ca72-122">hello binding supports hello following properties:</span></span>

* <span data-ttu-id="8ca72-123">**név** : szükséges – hello hello kérelemmel vagy a kérelem törzsében függvény a kódban használt változó neve.</span><span class="sxs-lookup"><span data-stu-id="8ca72-123">**name** : Required - hello variable name used in function code for hello request or request body.</span></span> <span data-ttu-id="8ca72-124">Lásd: [egy HTTP-eseményindítóval kódból használata](#httptriggerusage).</span><span class="sxs-lookup"><span data-stu-id="8ca72-124">See [Working with an HTTP trigger from code](#httptriggerusage).</span></span>
* <span data-ttu-id="8ca72-125">**típus** : szükséges – túl lehet beállított "httpTrigger".</span><span class="sxs-lookup"><span data-stu-id="8ca72-125">**type** : Required - must be set too"httpTrigger".</span></span>
* <span data-ttu-id="8ca72-126">**irány** : szükséges – túl beállított "a" kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8ca72-126">**direction** : Required - must be set too"in".</span></span>
* <span data-ttu-id="8ca72-127">_authLevel_ : Ez határozza meg, milyen kulcsok, ha van ilyen kell toobe rendelés tooinvoke hello funkció hello kérésre szerepel.</span><span class="sxs-lookup"><span data-stu-id="8ca72-127">_authLevel_ : This determines what keys, if any, need toobe present on hello request in order tooinvoke hello function.</span></span> <span data-ttu-id="8ca72-128">Lásd: [kulcsoknál](#keys) alatt.</span><span class="sxs-lookup"><span data-stu-id="8ca72-128">See [Working with keys](#keys) below.</span></span> <span data-ttu-id="8ca72-129">hello érték hello a következők egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="8ca72-129">hello value can be one of hello following:</span></span>
    * <span data-ttu-id="8ca72-130">_Névtelen_: nem API-kulcsot meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="8ca72-130">_anonymous_: No API key is required.</span></span>
    * <span data-ttu-id="8ca72-131">_függvény_: egy funkcióspecifikus API-kulcsot meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="8ca72-131">_function_: A function-specific API key is required.</span></span> <span data-ttu-id="8ca72-132">Ez az hello alapértelmezett érték, ha nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="8ca72-132">This is hello default value if none is provided.</span></span>
    * <span data-ttu-id="8ca72-133">_felügyeleti_ : hello főkulcsára szükség.</span><span class="sxs-lookup"><span data-stu-id="8ca72-133">_admin_ : hello master key is required.</span></span>
* <span data-ttu-id="8ca72-134">**módszerek** : Ez a tömb hello HTTP módszerek toowhich hello függvény válaszol.</span><span class="sxs-lookup"><span data-stu-id="8ca72-134">**methods** : This is an array of hello HTTP methods toowhich hello function will respond.</span></span> <span data-ttu-id="8ca72-135">Ha nincs megadva, hello függvény tooall HTTP módszerek válaszol.</span><span class="sxs-lookup"><span data-stu-id="8ca72-135">If not specified, hello function will respond tooall HTTP methods.</span></span> <span data-ttu-id="8ca72-136">Lásd: [hello HTTP-végpont testreszabása](#url).</span><span class="sxs-lookup"><span data-stu-id="8ca72-136">See [Customizing hello HTTP endpoint](#url).</span></span>
* <span data-ttu-id="8ca72-137">**útvonal** : hello útvonalsablonhoz toowhich szabályozása határozza meg a kérelem URL-címeket, a függvény válaszol.</span><span class="sxs-lookup"><span data-stu-id="8ca72-137">**route** : This defines hello route template, controlling toowhich request URLs your function will respond.</span></span> <span data-ttu-id="8ca72-138">hello Ha nincs megadva, az alapérték `<functionname>`.</span><span class="sxs-lookup"><span data-stu-id="8ca72-138">hello default value if none is provided is `<functionname>`.</span></span> <span data-ttu-id="8ca72-139">Lásd: [hello HTTP-végpont testreszabása](#url).</span><span class="sxs-lookup"><span data-stu-id="8ca72-139">See [Customizing hello HTTP endpoint](#url).</span></span>
* <span data-ttu-id="8ca72-140">**webHookType** : Ez szerint állítja be hello HTTP-eseményindító tooact egy webhook reciever hello megadott szolgáltatóhoz.</span><span class="sxs-lookup"><span data-stu-id="8ca72-140">**webHookType** : This configures hello HTTP trigger tooact as a webhook reciever for hello specified provider.</span></span> <span data-ttu-id="8ca72-141">Hello _módszerek_ tulajdonság nem állítható be, ha ezt választja.</span><span class="sxs-lookup"><span data-stu-id="8ca72-141">hello _methods_ property should not be set if this is chosen.</span></span> <span data-ttu-id="8ca72-142">Lásd: [válaszoló toowebhooks](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="8ca72-142">See [Responding toowebhooks](#hooktrigger).</span></span> <span data-ttu-id="8ca72-143">hello érték hello a következők egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="8ca72-143">hello value can be one of hello following:</span></span>
    * <span data-ttu-id="8ca72-144">_genericJson_ : egy általános célú webhook végpont logika egy adott szolgáltató nélkül.</span><span class="sxs-lookup"><span data-stu-id="8ca72-144">_genericJson_ : A general purpose webhook endpoint without logic for a specific provider.</span></span>
    * <span data-ttu-id="8ca72-145">_github_ : hello függvény tooGitHub webhookok válaszol.</span><span class="sxs-lookup"><span data-stu-id="8ca72-145">_github_ : hello function will respond tooGitHub webhooks.</span></span> <span data-ttu-id="8ca72-146">Hello _authLevel_ tulajdonság nem állítható be, ha ezt választja.</span><span class="sxs-lookup"><span data-stu-id="8ca72-146">hello _authLevel_ property should not be set if this is chosen.</span></span>
    * <span data-ttu-id="8ca72-147">_slackhez_ : hello függvény tooSlack webhookok válaszol.</span><span class="sxs-lookup"><span data-stu-id="8ca72-147">_slack_ : hello function will respond tooSlack webhooks.</span></span> <span data-ttu-id="8ca72-148">Hello _authLevel_ tulajdonság nem állítható be, ha ezt választja.</span><span class="sxs-lookup"><span data-stu-id="8ca72-148">hello _authLevel_ property should not be set if this is chosen.</span></span>

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a><span data-ttu-id="8ca72-149">Egy HTTP-eseményindítóval kódból használata</span><span class="sxs-lookup"><span data-stu-id="8ca72-149">Working with an HTTP trigger from code</span></span>
<span data-ttu-id="8ca72-150">A C# és F # funkciók deklarálhatja az eseményindító bemeneti toobe hello típusú vagy `HttpRequestMessage` vagy olyan egyéni típusra.</span><span class="sxs-lookup"><span data-stu-id="8ca72-150">For C# and F# functions, you can declare hello type of your trigger input toobe either `HttpRequestMessage` or a custom type.</span></span> <span data-ttu-id="8ca72-151">Ha úgy dönt, `HttpRequestMessage`, majd elérhetővé válik a teljes körű hozzáférési toohello request objektumon.</span><span class="sxs-lookup"><span data-stu-id="8ca72-151">If you choose `HttpRequestMessage`, then you will get full access toohello request object.</span></span> <span data-ttu-id="8ca72-152">Egy egyéni típust (például egy POCO) a funkciók az tooparse hello kérelemtörzset JSON toopopulate hello objektumtulajdonságokat megpróbál.</span><span class="sxs-lookup"><span data-stu-id="8ca72-152">For a custom type (such as a POCO), Functions will attempt tooparse hello request body as JSON toopopulate hello object properties.</span></span>

<span data-ttu-id="8ca72-153">Node.js funkciók hello Functions futtatókörnyezete itt hello kérelemtörzset hello kérelem objektum helyett.</span><span class="sxs-lookup"><span data-stu-id="8ca72-153">For Node.js functions, hello Functions runtime provides hello request body instead of hello request object.</span></span>

<span data-ttu-id="8ca72-154">Lásd: [HTTP-eseményindító minták](#httptriggersample) például is érvényesek.</span><span class="sxs-lookup"><span data-stu-id="8ca72-154">See [HTTP trigger samples](#httptriggersample) for example usages.</span></span>


<a name="output"></a>
## <a name="http-response-output-binding"></a><span data-ttu-id="8ca72-155">HTTP-válasz kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="8ca72-155">HTTP response output binding</span></span>
<span data-ttu-id="8ca72-156">Hello HTTP kimeneti kötése toorespond toohello HTTP kérést küldő használja.</span><span class="sxs-lookup"><span data-stu-id="8ca72-156">Use hello HTTP output binding toorespond toohello HTTP request sender.</span></span> <span data-ttu-id="8ca72-157">A kötés HTTP-eseményindítóval igényel, és lehetővé teszi a hello eseményindító kérelemhez társított toocustomize hello válasz.</span><span class="sxs-lookup"><span data-stu-id="8ca72-157">This binding requires an HTTP trigger and allows you toocustomize hello response associated with hello trigger's request.</span></span> <span data-ttu-id="8ca72-158">Ha a HTTP kimeneti kötése van a nem Microsofttól származó, egy HTTP-eseményindítóval HTTP 200 OK visszatér egy üres szövegtörzzsel.</span><span class="sxs-lookup"><span data-stu-id="8ca72-158">If an HTTP output binding is not provided, an HTTP trigger will return HTTP 200 OK with an empty body.</span></span> 

### <a name="configuring-an-http-output-binding"></a><span data-ttu-id="8ca72-159">HTTP konfigurálása kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="8ca72-159">Configuring an HTTP output binding</span></span>
<span data-ttu-id="8ca72-160">hello HTTP kimeneti kötése definiált többek között a JSON objektum hasonló toohello hello a következő `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="8ca72-160">hello HTTP output binding is defined by including a JSON object similar toohello following in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
<span data-ttu-id="8ca72-161">hello kötés tartalmaz hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="8ca72-161">hello binding contains hello following properties:</span></span>

* <span data-ttu-id="8ca72-162">**név** : szükséges – hello hello válasz függvény a kódban használt változó neve.</span><span class="sxs-lookup"><span data-stu-id="8ca72-162">**name** : Required - hello variable name used in function code for hello response.</span></span> <span data-ttu-id="8ca72-163">Lásd: [HTTP használata kimeneti kötése kódból](#outputusage).</span><span class="sxs-lookup"><span data-stu-id="8ca72-163">See [Working with an HTTP output binding from code](#outputusage).</span></span>
* <span data-ttu-id="8ca72-164">**típus** : szükséges – túl lehet beállított "http".</span><span class="sxs-lookup"><span data-stu-id="8ca72-164">**type** : Required - must be set too"http".</span></span>
* <span data-ttu-id="8ca72-165">**irány** : szükséges – állítsa be túl, "out" kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8ca72-165">**direction** : Required - must be set too"out".</span></span>

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a><span data-ttu-id="8ca72-166">HTTP használata kimeneti kötése kódból</span><span class="sxs-lookup"><span data-stu-id="8ca72-166">Working with an HTTP output binding from code</span></span>
<span data-ttu-id="8ca72-167">Hello kimeneti paraméter (például "res") toorespond toohello http vagy a webhook hívó is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8ca72-167">You can use hello output parameter (e.g., "res") toorespond toohello http or webhook caller.</span></span> <span data-ttu-id="8ca72-168">Másik lehetőségként használhatja a szabványos `Request.CreateResponse()` (C#) vagy `context.res` (Node.JS) mintát tooreturn a válaszát.</span><span class="sxs-lookup"><span data-stu-id="8ca72-168">Alternatively, you can use the standard `Request.CreateResponse()` (C#) or `context.res` (Node.JS) pattern tooreturn your response.</span></span> <span data-ttu-id="8ca72-169">Hogyan toouse hello utóbbi metódus példákért lásd [HTTP-eseményindító minták](#httptriggersample) és [Webhook eseményindító minták](#hooktriggersample).</span><span class="sxs-lookup"><span data-stu-id="8ca72-169">For examples on how toouse hello latter method, see [HTTP trigger samples](#httptriggersample) and [Webhook trigger samples](#hooktriggersample).</span></span>


<a name="hooktrigger"></a>
## <a name="responding-toowebhooks"></a><span data-ttu-id="8ca72-170">Toowebhooks válaszol</span><span class="sxs-lookup"><span data-stu-id="8ca72-170">Responding toowebhooks</span></span>
<span data-ttu-id="8ca72-171">Egy HTTP-eseményindítóval rendelkező hello _webHookType_ tulajdonság túl lesz konfigurált toorespond[webhookok](https://en.wikipedia.org/wiki/Webhook).</span><span class="sxs-lookup"><span data-stu-id="8ca72-171">An HTTP trigger with hello _webHookType_ property will be configured toorespond too[webhooks](https://en.wikipedia.org/wiki/Webhook).</span></span> <span data-ttu-id="8ca72-172">Alapszintű hello konfigurálása hello "genericJson" beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="8ca72-172">hello basic configuration uses hello "genericJson" setting.</span></span> <span data-ttu-id="8ca72-173">Ez a HTTP POST és hello korlátozza kérelmek tooonly `application/json` tartalomtípus.</span><span class="sxs-lookup"><span data-stu-id="8ca72-173">This restricts requests tooonly those using HTTP POST and with hello `application/json` content type.</span></span>

<span data-ttu-id="8ca72-174">hello eseményindító továbbá lehet meg a következőkhöz ideális tooa adott webhook szolgáltató (pl. [GitHub](https://developer.github.com/webhooks/) és [Slackhez](https://api.slack.com/outgoing-webhooks)).</span><span class="sxs-lookup"><span data-stu-id="8ca72-174">hello trigger can additionally be tailored tooa specific webhook provider (e.g., [GitHub](https://developer.github.com/webhooks/) and [Slack](https://api.slack.com/outgoing-webhooks)).</span></span> <span data-ttu-id="8ca72-175">Ha egy szolgáltató van megadva, hello Functions futtatókörnyezete is kezeli hello szolgáltató ellenőrzési logika meg.</span><span class="sxs-lookup"><span data-stu-id="8ca72-175">If a provider is specified, hello Functions runtime can take care of hello provider's validation logic for you.</span></span>  

### <a name="configuring-github-as-a-webhook-provider"></a><span data-ttu-id="8ca72-176">GitHub webhook szolgáltatóként konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8ca72-176">Configuring GitHub as a webhook provider</span></span>
<span data-ttu-id="8ca72-177">toorespond tooGitHub webhookokkal, először hozzon létre a függvény egy HTTP-eseményindítóval, és állítsa be a hello _webHookType_ tulajdonság túl "github".</span><span class="sxs-lookup"><span data-stu-id="8ca72-177">toorespond tooGitHub webhooks, first create your function with an HTTP Trigger, and set hello _webHookType_ property too"github".</span></span> <span data-ttu-id="8ca72-178">Másolja a [URL-cím](#url) és [API-kulcs](#keys) a GitHub-tárházban történő **adja hozzá a webhook** lap.</span><span class="sxs-lookup"><span data-stu-id="8ca72-178">Then copy its [URL](#url) and [API key](#keys) into your GitHub repository's **Add webhook** page.</span></span> <span data-ttu-id="8ca72-179">Lásd a GitHub [Webhook létrehozása](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) több dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="8ca72-179">See GitHub's [Creating Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) documentation for more.</span></span>

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a><span data-ttu-id="8ca72-180">A webhook szolgáltató Slackhez konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8ca72-180">Configuring Slack as a webhook provider</span></span>
<span data-ttu-id="8ca72-181">hello Slack webhook létrehoz egy jogkivonatot helyett, és adja meg azt, konfigurálnia kell egy funkcióspecifikus kulcs hello jogkivonat a Slackhez, így Önnek.</span><span class="sxs-lookup"><span data-stu-id="8ca72-181">hello Slack webhook generates a token for you instead of letting you specify it, so you must configure a function-specific key with hello token from Slack.</span></span> <span data-ttu-id="8ca72-182">Lásd: [kulcsoknál](#keys).</span><span class="sxs-lookup"><span data-stu-id="8ca72-182">See [Working with keys](#keys).</span></span>

<a name="url"></a>
## <a name="customizing-hello-http-endpoint"></a><span data-ttu-id="8ca72-183">Hello HTTP-végpont testreszabása</span><span class="sxs-lookup"><span data-stu-id="8ca72-183">Customizing hello HTTP endpoint</span></span>
<span data-ttu-id="8ca72-184">Alapértelmezés szerint HTTP-eseményindítóval, vagy WebHook, a függvény létrehozásakor hello függvény megcímezhető hello űrlap útvonal:</span><span class="sxs-lookup"><span data-stu-id="8ca72-184">By default when you create a function for an HTTP trigger, or WebHook, hello function is addressable with a route of hello form:</span></span>

    http://<yourapp>.azurewebsites.net/api/<funcname> 

<span data-ttu-id="8ca72-185">Ez az útvonal nem kötelező hello segítségével testre szabható `route` hello HTTP-eseményindító tulajdonsága bemeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="8ca72-185">You can customize this route using hello optional `route` property on hello HTTP trigger's input binding.</span></span> <span data-ttu-id="8ca72-186">Tegyük fel, a következő hello *function.json* fájl határozza meg a `route` tulajdonság egy HTTP-eseményindító:</span><span class="sxs-lookup"><span data-stu-id="8ca72-186">As an example, hello following *function.json* file defines a `route` property for an HTTP trigger:</span></span>

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

<span data-ttu-id="8ca72-187">Használja ezt a konfigurációt, hello függvény most megcímezhető az útvonal helyett hello eredeti útvonal a következő hello.</span><span class="sxs-lookup"><span data-stu-id="8ca72-187">Using this configuration, hello function is now addressable with hello following route instead of hello original route.</span></span>

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

<span data-ttu-id="8ca72-188">Ez lehetővé teszi, hogy hello függvény a két toosupport paraméterek az hello cím, "kategória" és "id".</span><span class="sxs-lookup"><span data-stu-id="8ca72-188">This allows hello function code toosupport two parameters in hello address, "category" and "id".</span></span> <span data-ttu-id="8ca72-189">Akkor használhatja [webes API útvonal megkötés](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) a paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="8ca72-189">You can use any [Web API Route Constraint](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) with your parameters.</span></span> <span data-ttu-id="8ca72-190">a következő C# funkciókódot hello mindkét paraméter használ.</span><span class="sxs-lookup"><span data-stu-id="8ca72-190">hello following C# function code makes use of both parameters.</span></span>

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

<span data-ttu-id="8ca72-191">Node.js függvény kód toouse hello itt található ilyen útvonal paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="8ca72-191">Here is Node.js function code toouse hello same route parameters.</span></span>

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

<span data-ttu-id="8ca72-192">Alapértelmezés szerint az összes függvény útvonal előtagként *api*.</span><span class="sxs-lookup"><span data-stu-id="8ca72-192">By default, all function routes are prefixed with *api*.</span></span> <span data-ttu-id="8ca72-193">Is testre szabhatja, vagy távolítsa el a hello előtagja hello a `http.routePrefix` tulajdonságot a *host.json* fájlt.</span><span class="sxs-lookup"><span data-stu-id="8ca72-193">You can also customize or remove hello prefix using hello `http.routePrefix` property in your *host.json* file.</span></span> <span data-ttu-id="8ca72-194">hello következő példában eltávolítjuk hello *api* útvonal előtagja hello hello előtagot üres karakterlánc használatával *host.json* fájlt.</span><span class="sxs-lookup"><span data-stu-id="8ca72-194">hello following example removes hello *api* route prefix by using an empty string for hello prefix in hello *host.json* file.</span></span>

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

<span data-ttu-id="8ca72-195">Részletes információt tooupdate hello *host.json* fájl, a függvény, tekintse át, [hogyan tooupdate működni az alkalmazás fájljai](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="8ca72-195">For detailed information on how tooupdate hello *host.json* file for your function, See, [How tooupdate function app files](functions-reference.md#fileupdate).</span></span> 

<span data-ttu-id="8ca72-196">További tudnivalók egyéb konfigurálhatja a a *host.json* fájlba, lásd: [host.json hivatkozás](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="8ca72-196">For information on other properties you can configure in your *host.json* file, see [host.json reference](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>


<a name="keys"></a>
## <a name="working-with-keys"></a><span data-ttu-id="8ca72-197">Kulcsok használata</span><span class="sxs-lookup"><span data-stu-id="8ca72-197">Working with keys</span></span>
<span data-ttu-id="8ca72-198">A nagyobb biztonság kulcsok HttpTriggers használhatják fel.</span><span class="sxs-lookup"><span data-stu-id="8ca72-198">HttpTriggers can leverage keys for added security.</span></span> <span data-ttu-id="8ca72-199">Szabványos HttpTrigger használhat API-kulcs, ezek igénylő hello kulcs toobe hello kérelem megtalálható.</span><span class="sxs-lookup"><span data-stu-id="8ca72-199">A standard HttpTrigger can use these as an API key, requiring hello key toobe present on hello request.</span></span> <span data-ttu-id="8ca72-200">Webhook használható kulcsok tooauthorize kérelmek az sokféleképpen, attól függően, hogy milyen hello szolgáltató támogatja.</span><span class="sxs-lookup"><span data-stu-id="8ca72-200">Webhooks can use keys tooauthorize requests in a variety of ways, depending on what hello provider supports.</span></span>

<span data-ttu-id="8ca72-201">Kulcsok részeként a függvény alkalmazást az Azure-ban tároljuk, és inaktív titkosított.</span><span class="sxs-lookup"><span data-stu-id="8ca72-201">Keys are stored as part of your function app in Azure and are encrypted at rest.</span></span> <span data-ttu-id="8ca72-202">tooview a kulcsokat, hozzon létre újakat, vagy állítsa a kulcsok toonew értékek, keresse meg a funkciók hello portálon tooone, és válassza ki a "Kezelése."</span><span class="sxs-lookup"><span data-stu-id="8ca72-202">tooview your keys, create new ones, or roll keys toonew values, navigate tooone of your functions within hello portal and select "Manage."</span></span> 

<span data-ttu-id="8ca72-203">A kulcsok két típusa van:</span><span class="sxs-lookup"><span data-stu-id="8ca72-203">There are two types of keys:</span></span>
- <span data-ttu-id="8ca72-204">**Kulcsok tárolására**: hello függvény alkalmazáson belüli összes funkciók által megosztott ezeket a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="8ca72-204">**Host keys**: These keys are shared by all functions within hello function app.</span></span> <span data-ttu-id="8ca72-205">Ha egy API-kulcsot, ezek teszik lehetővé a hozzáférést tooany függvény hello függvény alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="8ca72-205">When used as an API key, these allow access tooany function within hello function app.</span></span>
- <span data-ttu-id="8ca72-206">**Funkcióbillentyűk**: ezek a kulcsok alkalmazása csak toohello adott funkciókhoz használt vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="8ca72-206">**Function keys**: These keys apply only toohello specific functions under which they are defined.</span></span> <span data-ttu-id="8ca72-207">Ha egy API-kulcsot, ezek csak engedélyezése hozzáférés toothat függvény.</span><span class="sxs-lookup"><span data-stu-id="8ca72-207">When used as an API key, these only allow access toothat function.</span></span>

<span data-ttu-id="8ca72-208">Minden kulcs neve referenciaként, és nincs (a "alapértelmezett" nevű) alapértelmezett kulcs hello függvény és a gazdagép szintjén.</span><span class="sxs-lookup"><span data-stu-id="8ca72-208">Each key is named for reference, and there is a default key (named "default") at hello function and host level.</span></span> <span data-ttu-id="8ca72-209">Hello **főkulcs** egy alapértelmezett állomáskulcs neve "_master", amely minden függvény alkalmazás van definiálva, és nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="8ca72-209">hello **master key** is a default host key named "_master" that is defined for each function app and cannot be revoked.</span></span> <span data-ttu-id="8ca72-210">Rendszergazdai hozzáférés toohello futásidejű API-kat biztosít.</span><span class="sxs-lookup"><span data-stu-id="8ca72-210">It provides administrative access toohello runtime APIs.</span></span> <span data-ttu-id="8ca72-211">Használatával `"authLevel": "admin"` hello a JSON kötés a kulcs toobe hello kérés jelenik meg a szükséges; bármely más kulcs engedélyezési hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="8ca72-211">Using `"authLevel": "admin"` in hello binding JSON will require this key toobe presented on hello request; any other key will result in a authorization failure.</span></span>

> [!NOTE]
> <span data-ttu-id="8ca72-212">Esedékes toohello emelt szintű engedélyek hello főkulccsal, nem kell harmadik felek ossza meg ezt a kulcsot, vagy a natív ügyfélalkalmazások eloszthatják azt.</span><span class="sxs-lookup"><span data-stu-id="8ca72-212">Due toohello elevated permissions granted by hello master key, you should not share this key with third parties or distribute it in native client applications.</span></span> <span data-ttu-id="8ca72-213">Körültekintően járjon el hello rendszergazdai jogosultsági szint kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="8ca72-213">Exercise caution when choosing hello admin authorization level.</span></span>
> 
> 

### <a name="api-key-authorization"></a><span data-ttu-id="8ca72-214">API-kulcs engedélyezési</span><span class="sxs-lookup"><span data-stu-id="8ca72-214">API key authorization</span></span>
<span data-ttu-id="8ca72-215">Alapértelmezés szerint egy HttpTrigger hello HTTP-kérelmek API-kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="8ca72-215">By default, an HttpTrigger requires an API key in hello HTTP request.</span></span> <span data-ttu-id="8ca72-216">Ezért a HTTP-kérelmek általában néz ki:</span><span class="sxs-lookup"><span data-stu-id="8ca72-216">So your HTTP request normally looks like this:</span></span>

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

<span data-ttu-id="8ca72-217">hello kulcs tartalmazhat egy lekérdezési karakterlánc-változóvá nevű `code`, a fentiek szerint, vagy a része egy `x-functions-key` HTTP-fejléc.</span><span class="sxs-lookup"><span data-stu-id="8ca72-217">hello key can be included in a query string variable named `code`, as above, or it can be included in an `x-functions-key` HTTP header.</span></span> <span data-ttu-id="8ca72-218">hello kulcs hello értéke lehet bármely hello függvény definiált függvény kulcs, vagy bármely állomás kulcsát.</span><span class="sxs-lookup"><span data-stu-id="8ca72-218">hello value of hello key can be any function key defined for hello function, or any host key.</span></span>

<span data-ttu-id="8ca72-219">Választhat, tooallow kérelmek kulcsok nélkül, vagy adja meg, hogy hello főkulcs kell használható hello módosításával `authLevel` JSON kötés hello tulajdonságot (lásd: [HTTP-eseményindítóval](#httptrigger)).</span><span class="sxs-lookup"><span data-stu-id="8ca72-219">You can choose tooallow requests without keys or specify that hello master key must be used by changing hello `authLevel` property in hello binding JSON (see [HTTP trigger](#httptrigger)).</span></span>

### <a name="keys-and-webhooks"></a><span data-ttu-id="8ca72-220">Kulcsok és webhookokkal</span><span class="sxs-lookup"><span data-stu-id="8ca72-220">Keys and webhooks</span></span>
<span data-ttu-id="8ca72-221">Webhook engedélyezési hello webhook reciever összetevő kezeli, hello HttpTrigger és hello mechanizmus hello webhook típus alapján változik.</span><span class="sxs-lookup"><span data-stu-id="8ca72-221">Webhook authorization is handled by hello webhook reciever component, part of hello HttpTrigger, and hello mechanism varies based on hello webhook type.</span></span> <span data-ttu-id="8ca72-222">Minden egyes mechanizmus nem, a kulcs azonban támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="8ca72-222">Each mechanism does, however rely on a key.</span></span> <span data-ttu-id="8ca72-223">Alapértelmezés szerint "alapértelmezett" nevű hello függvény kulcs használható.</span><span class="sxs-lookup"><span data-stu-id="8ca72-223">By default, hello function key named "default" will be used.</span></span> <span data-ttu-id="8ca72-224">Ha egy másik kulcsot akarja toouse, tooconfigure hello webhook szolgáltató toosend hello kulcsnév valamelyik a következő módokon hello hello kérelemmel lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="8ca72-224">If you wish toouse a different key, you will need tooconfigure hello webhook provider toosend hello key name with hello request in one of hello following ways:</span></span>

- <span data-ttu-id="8ca72-225">**Lekérdezési karakterlánc**: hello szolgáltató hello kulcsnév továbbítja a hello `clientid` lekérdezési karakterlánc (pl. `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span><span class="sxs-lookup"><span data-stu-id="8ca72-225">**Query string**: hello provider passes hello key name in hello `clientid` query string parameter (e.g., `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span></span>
- <span data-ttu-id="8ca72-226">**Kérelemfejléc**: hello szolgáltató hello kulcsnév továbbítja a hello `x-functions-clientid` fejléc.</span><span class="sxs-lookup"><span data-stu-id="8ca72-226">**Request header**: hello provider passes hello key name in hello `x-functions-clientid` header.</span></span>

> [!NOTE]
> <span data-ttu-id="8ca72-227">Funkcióbillentyűk élveznek állomások kulcsait.</span><span class="sxs-lookup"><span data-stu-id="8ca72-227">Function keys take precedence over host keys.</span></span> <span data-ttu-id="8ca72-228">Ha két kulcsok vannak meghatározva, az azonos név, hello hello függvény kulccsal.</span><span class="sxs-lookup"><span data-stu-id="8ca72-228">If two keys are defined with hello same name, hello function key will be used.</span></span>
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a><span data-ttu-id="8ca72-229">HTTP-eseményindító minták</span><span class="sxs-lookup"><span data-stu-id="8ca72-229">HTTP trigger samples</span></span>
<span data-ttu-id="8ca72-230">Tegyük fel, hogy rendelkezik a következő HTTP-eseményindítóval a hello hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="8ca72-230">Suppose you have hello following HTTP trigger in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

<span data-ttu-id="8ca72-231">Lásd: hello nyelvspecifikus mintát, amely megkeresi a `name` hello lekérdezési karakterláncot vagy hello HTTP-kérelem törzse hello paraméter.</span><span class="sxs-lookup"><span data-stu-id="8ca72-231">See hello language-specific sample that looks for a `name` parameter either in hello query string or hello body of hello HTTP request.</span></span>

* [<span data-ttu-id="8ca72-232">C#</span><span class="sxs-lookup"><span data-stu-id="8ca72-232">C#</span></span>](#httptriggercsharp)
* [<span data-ttu-id="8ca72-233">F#</span><span class="sxs-lookup"><span data-stu-id="8ca72-233">F#</span></span>](#httptriggerfsharp)
* [<span data-ttu-id="8ca72-234">Node.js</span><span class="sxs-lookup"><span data-stu-id="8ca72-234">Node.js</span></span>](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a><span data-ttu-id="8ca72-235">A C# HTTP-eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="8ca72-235">HTTP trigger sample in C#</span></span> #
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

<span data-ttu-id="8ca72-236">Is köthető tooa POCO helyett `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="8ca72-236">You can also bind tooa POCO instead of `HttpRequestMessage`.</span></span> <span data-ttu-id="8ca72-237">Ez a hello szervezetnek hello kérelem JSON-ként értelmezni hidratált lehet.</span><span class="sxs-lookup"><span data-stu-id="8ca72-237">This will be hydrated from hello body of hello request, parsed as JSON.</span></span> <span data-ttu-id="8ca72-238">Hasonlóképpen egy kötés toohello HTTP-válasz kimenetének függvénynek adható át, és ez változatlanul adódik vissza hello adott válasz törzsének, 200 állapotkóddal.</span><span class="sxs-lookup"><span data-stu-id="8ca72-238">Similarly, a type can be passed toohello HTTP response output binding, and this will be returned as hello response body, with a 200 status code.</span></span>
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
### <a name="http-trigger-sample-in-f"></a><span data-ttu-id="8ca72-239">Az F # HTTP-eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="8ca72-239">HTTP trigger sample in F#</span></span> #
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

<span data-ttu-id="8ca72-240">Kell egy `project.json` NuGet tooreference hello használó fájl `FSharp.Interop.Dynamic` és `Dynamitey` szerelvényeket, ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="8ca72-240">You need a `project.json` file that uses NuGet tooreference hello `FSharp.Interop.Dynamic` and `Dynamitey` assemblies, like this:</span></span>

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

<span data-ttu-id="8ca72-241">Ez fogja használni NuGet toofetch a függőségeket, és a parancsfájlban hivatkoznak rájuk.</span><span class="sxs-lookup"><span data-stu-id="8ca72-241">This will use NuGet toofetch your dependencies and will reference them in your script.</span></span>

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a><span data-ttu-id="8ca72-242">A Node.JS-ben HTTP-eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="8ca72-242">HTTP trigger sample in Node.JS</span></span>
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
## <a name="webhook-samples"></a><span data-ttu-id="8ca72-243">Webhook minták</span><span class="sxs-lookup"><span data-stu-id="8ca72-243">Webhook samples</span></span>
<span data-ttu-id="8ca72-244">Tegyük fel, hogy rendelkezik a következő webhook eseményindítót hello hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="8ca72-244">Suppose you have hello following webhook trigger in hello `bindings` array of function.json:</span></span>

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

<span data-ttu-id="8ca72-245">Lásd: hello nyelvspecifikus minta, amelyre bejelentkezik GitHub probléma megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="8ca72-245">See hello language-specific sample that logs GitHub issue comments.</span></span>

* [<span data-ttu-id="8ca72-246">C#</span><span class="sxs-lookup"><span data-stu-id="8ca72-246">C#</span></span>](#hooktriggercsharp)
* [<span data-ttu-id="8ca72-247">F#</span><span class="sxs-lookup"><span data-stu-id="8ca72-247">F#</span></span>](#hooktriggerfsharp)
* [<span data-ttu-id="8ca72-248">Node.js</span><span class="sxs-lookup"><span data-stu-id="8ca72-248">Node.js</span></span>](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a><span data-ttu-id="8ca72-249">A C# Webhook minta</span><span class="sxs-lookup"><span data-stu-id="8ca72-249">Webhook sample in C#</span></span> #
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

### <a name="webhook-sample-in-f"></a><span data-ttu-id="8ca72-250">Az F # Webhook minta</span><span class="sxs-lookup"><span data-stu-id="8ca72-250">Webhook sample in F#</span></span> #
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

### <a name="webhook-sample-in-nodejs"></a><span data-ttu-id="8ca72-251">A node.js Webhook minta</span><span class="sxs-lookup"><span data-stu-id="8ca72-251">Webhook sample in Node.JS</span></span>
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a><span data-ttu-id="8ca72-252">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ca72-252">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


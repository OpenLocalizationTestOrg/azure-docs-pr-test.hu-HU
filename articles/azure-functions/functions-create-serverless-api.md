---
title: "az Azure Functions használatával egy kiszolgáló nélküli API aaaCreate |} Microsoft Docs"
description: "Hogyan toocreate egy kiszolgáló nélküli API-t az Azure Functions használatával"
services: functions
author: mattchenderson
manager: erikre
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: mahender
ms.custom: mvc
ms.openlocfilehash: 877e3b229d5477fc5fec594ccd284fb55d7f3c07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-serverless-api-using-azure-functions"></a><span data-ttu-id="de2f5-103">Az Azure Functions használatával egy kiszolgáló nélküli API létrehozása</span><span class="sxs-lookup"><span data-stu-id="de2f5-103">Create a serverless API using Azure Functions</span></span>

<span data-ttu-id="de2f5-104">Ebből az oktatóanyagból megtudhatja, hogyan Azure Functions lehetővé teszi a toobuild kiválóan méretezhető API-k.</span><span class="sxs-lookup"><span data-stu-id="de2f5-104">In this tutorial you will learn how Azure Functions allows you toobuild highly-scalable APIs.</span></span> <span data-ttu-id="de2f5-105">Az Azure Functions rendelkezik beépített HTTP eseményindítók és kötések, amelyek révén könnyen tooauthor gyűjteménye a különböző nyelveken, beleértve a Node.JS, a C#, és több végpont.</span><span class="sxs-lookup"><span data-stu-id="de2f5-105">Azure Functions comes with a collection of built-in HTTP triggers and bindings which make it easy tooauthor an endpoint in a variety of languages, including Node.JS, C#, and more.</span></span> <span data-ttu-id="de2f5-106">Ebben az oktatóanyagban testre fogja szabni egy HTTP eseményindító toohandle konkrét műveletek az API kialakításában.</span><span class="sxs-lookup"><span data-stu-id="de2f5-106">In this tutorial, you will customize an HTTP trigger toohandle specific actions in your API design.</span></span> <span data-ttu-id="de2f5-107">Is előkészítheti a növekvő az API integrálása az Azure Functions proxyk és utánzatait API-k beállítása.</span><span class="sxs-lookup"><span data-stu-id="de2f5-107">You will also prepare for growing your API by integrating it with Azure Functions Proxies and setting up mock APIs.</span></span> <span data-ttu-id="de2f5-108">Mindez valósítható meg hello funkciók kiszolgáló nélküli számítási környezet felett, erőforrások méretezésével kapcsolatos tooworry nincs – a API logika csak összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="de2f5-108">All of this is accomplished on top of hello Functions serverless compute environment, so you don't have tooworry about scaling resources - you can just focus on your API logic.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de2f5-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="de2f5-109">Prerequisites</span></span> 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

<span data-ttu-id="de2f5-110">Ez az oktatóanyag hello részeinek hello eredményül kapott függvény fog történni.</span><span class="sxs-lookup"><span data-stu-id="de2f5-110">hello resulting function will be used for hello rest of this tutorial.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="de2f5-111">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="de2f5-111">Sign in tooAzure</span></span>

<span data-ttu-id="de2f5-112">Nyissa meg hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="de2f5-112">Open hello Azure portal.</span></span> <span data-ttu-id="de2f5-113">toodo, jelentkezzen be a túl[https://portal.azure.com](https://portal.azure.com) fel fiókjával.</span><span class="sxs-lookup"><span data-stu-id="de2f5-113">toodo this, sign in too[https://portal.azure.com](https://portal.azure.com) with your Azure account.</span></span>

## <a name="customize-your-http-function"></a><span data-ttu-id="de2f5-114">A HTTP-funkció testreszabása</span><span class="sxs-lookup"><span data-stu-id="de2f5-114">Customize your HTTP function</span></span>

<span data-ttu-id="de2f5-115">Alapértelmezés szerint a HTTP-eseményindítóval aktivált függvény konfigurált tooaccept HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="de2f5-115">By default, your HTTP-triggered function is configured tooaccept any HTTP method.</span></span> <span data-ttu-id="de2f5-116">Emellett van alapértelmezett URL-cím hello `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`.</span><span class="sxs-lookup"><span data-stu-id="de2f5-116">There is also a default URL of hello form `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`.</span></span> <span data-ttu-id="de2f5-117">Ha hello gyors üzembe helyezés, majd követi `<funcname>` valószínűleg valahogy "HttpTriggerJS1".</span><span class="sxs-lookup"><span data-stu-id="de2f5-117">If you followed hello quickstart, then `<funcname>` probably looks something like "HttpTriggerJS1".</span></span> <span data-ttu-id="de2f5-118">Ebben a szakaszban hello függvény toorespond csak tooGET kéréseket a meghatározott módosítja `/api/hello` útvonal helyett.</span><span class="sxs-lookup"><span data-stu-id="de2f5-118">In this section, you will modify hello function toorespond only tooGET requests against `/api/hello` route instead.</span></span> 

<span data-ttu-id="de2f5-119">Keresse meg a hello Azure-portálon tooyour függvényt.</span><span class="sxs-lookup"><span data-stu-id="de2f5-119">Navigate tooyour function in hello Azure portal.</span></span> <span data-ttu-id="de2f5-120">Válassza ki **integráció** a bal oldali navigációs hello.</span><span class="sxs-lookup"><span data-stu-id="de2f5-120">Select **Integrate** in hello left navigation.</span></span>

![Egy HTTP-funkció testreszabása](./media/functions-create-serverless-api/customizing-http.png)

<span data-ttu-id="de2f5-122">Használja a HTTP-eseményindító beállítások hello táblázatban megadottak szerint.</span><span class="sxs-lookup"><span data-stu-id="de2f5-122">Use the HTTP trigger settings as specified in hello table.</span></span>

| <span data-ttu-id="de2f5-123">Mező</span><span class="sxs-lookup"><span data-stu-id="de2f5-123">Field</span></span> | <span data-ttu-id="de2f5-124">Mintaérték</span><span class="sxs-lookup"><span data-stu-id="de2f5-124">Sample value</span></span> | <span data-ttu-id="de2f5-125">Leírás</span><span class="sxs-lookup"><span data-stu-id="de2f5-125">Description</span></span> |
|---|---|---|
| <span data-ttu-id="de2f5-126">Engedélyezett HTTP-metódusok</span><span class="sxs-lookup"><span data-stu-id="de2f5-126">Allowed HTTP methods</span></span> | <span data-ttu-id="de2f5-127">A kijelölt metódusok</span><span class="sxs-lookup"><span data-stu-id="de2f5-127">Selected methods</span></span> | <span data-ttu-id="de2f5-128">Meghatározza, hogy mely HTTP-metódus lehet használt tooinvoke ezt a függvényt</span><span class="sxs-lookup"><span data-stu-id="de2f5-128">Determines what HTTP methods may be used tooinvoke this function</span></span> |
| <span data-ttu-id="de2f5-129">A kijelölt HTTP-metódusok</span><span class="sxs-lookup"><span data-stu-id="de2f5-129">Selected HTTP methods</span></span> | <span data-ttu-id="de2f5-130">GET</span><span class="sxs-lookup"><span data-stu-id="de2f5-130">GET</span></span> | <span data-ttu-id="de2f5-131">Lehetővé teszi, hogy csak kijelölt HTTP módszerek toobe tooinvoke e funkció használata</span><span class="sxs-lookup"><span data-stu-id="de2f5-131">Allows only selected HTTP methods toobe used tooinvoke this function</span></span> |
| <span data-ttu-id="de2f5-132">Útvonal-sablon</span><span class="sxs-lookup"><span data-stu-id="de2f5-132">Route template</span></span> | <span data-ttu-id="de2f5-133">hello</span><span class="sxs-lookup"><span data-stu-id="de2f5-133">/hello</span></span> | <span data-ttu-id="de2f5-134">Meghatározza, hogy milyen útvonal használt tooinvoke ezt a függvényt</span><span class="sxs-lookup"><span data-stu-id="de2f5-134">Determines what route is used tooinvoke this function</span></span> |

<span data-ttu-id="de2f5-135">Vegye figyelembe, hogy Ön nem foglal hello `/api` kiinduló hello útvonalsablonhoz, elérési út előtagot, mivel ez egy globális beállítás kezeli.</span><span class="sxs-lookup"><span data-stu-id="de2f5-135">Note that you did not include hello `/api` base path prefix in hello route template, as this is handled by a global setting.</span></span>

<span data-ttu-id="de2f5-136">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="de2f5-136">Click **Save**.</span></span>

<span data-ttu-id="de2f5-137">A HTTP-funkciók testreszabása többet is megtudhat [Azure Functions HTTP és a webhook kötések](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).</span><span class="sxs-lookup"><span data-stu-id="de2f5-137">You can learn more about customizing HTTP functions in [Azure Functions HTTP and webhook bindings](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).</span></span>

### <a name="test-your-api"></a><span data-ttu-id="de2f5-138">Az API-teszt</span><span class="sxs-lookup"><span data-stu-id="de2f5-138">Test your API</span></span>

<span data-ttu-id="de2f5-139">A következő tesztelheti a függvény toosee hello új API felület használata.</span><span class="sxs-lookup"><span data-stu-id="de2f5-139">Next, test your function toosee it working with hello new API surface.</span></span>

<span data-ttu-id="de2f5-140">Keresse meg a visszafelé toohello fejlesztési lap bal oldali navigációs hello hello függvény nevére kattintva.</span><span class="sxs-lookup"><span data-stu-id="de2f5-140">Navigate back toohello development page by clicking on hello function's name in hello left navigation.</span></span>

<span data-ttu-id="de2f5-141">Kattintson a **függvény URL-cím beszerzése** és hello URL-Címének másolása.</span><span class="sxs-lookup"><span data-stu-id="de2f5-141">Click **Get function URL** and copy hello URL.</span></span> <span data-ttu-id="de2f5-142">Láthatja, hogy az általa használt hello `/api/hello` címre.</span><span class="sxs-lookup"><span data-stu-id="de2f5-142">You should see that it uses hello `/api/hello` route now.</span></span>

<span data-ttu-id="de2f5-143">Hello URL-cím másolása egy új böngészőlapon vagy a kívánt REST-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="de2f5-143">Copy hello URL into a new browser tab or your preferred REST client.</span></span> <span data-ttu-id="de2f5-144">Böngészők alapértelmezés szerint fogja használni a GET.</span><span class="sxs-lookup"><span data-stu-id="de2f5-144">Browsers will use GET by default.</span></span>

<span data-ttu-id="de2f5-145">Hello funkció futtatását, és győződjön meg arról, hogy működik.</span><span class="sxs-lookup"><span data-stu-id="de2f5-145">Run hello function and confirm that it is working.</span></span> <span data-ttu-id="de2f5-146">Szükség lehet tooprovide hello "név" paraméternek meg egy lekérdezési karakterlánc toosatisfy hello gyors üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="de2f5-146">You may need tooprovide hello "name" parameter as a query string toosatisfy hello quickstart code.</span></span>

<span data-ttu-id="de2f5-147">A HTTP-metódus egy másik tooconfirm hello függvény végrehajtása nem hello-végpont meghívása is próbálkozhat.</span><span class="sxs-lookup"><span data-stu-id="de2f5-147">You can also try calling hello endpoint with another HTTP method tooconfirm that hello function is not executed.</span></span> <span data-ttu-id="de2f5-148">Ehhez szüksége lesz egy, a többi ügyfél, például cURL, a Postman vagy a Fiddler toouse.</span><span class="sxs-lookup"><span data-stu-id="de2f5-148">For this, you will need toouse a REST client, such as cURL, Postman, or Fiddler.</span></span>

## <a name="proxies-overview"></a><span data-ttu-id="de2f5-149">Proxyk áttekintése</span><span class="sxs-lookup"><span data-stu-id="de2f5-149">Proxies overview</span></span>

<span data-ttu-id="de2f5-150">Hello a következő szakaszban az API-proxyn keresztül fog surface.</span><span class="sxs-lookup"><span data-stu-id="de2f5-150">In hello next section, you will surface your API through a proxy.</span></span> <span data-ttu-id="de2f5-151">Az Azure Functions proxyk, amely lehetővé teszi tooforward kérelmek tooother erőforrások előzetes verziójú funkciók.</span><span class="sxs-lookup"><span data-stu-id="de2f5-151">Azure Functions Proxies is a preview feature that allows you tooforward requests tooother resources.</span></span> <span data-ttu-id="de2f5-152">Adhat meg egy HTTP-végpont például csak a HTTP-eseményindítóval, de ahelyett kód tooexecute adott végpontra metódus meghívásakor, akkor implementálásához URL-cím tooa távoli.</span><span class="sxs-lookup"><span data-stu-id="de2f5-152">You define an HTTP endpoint just like with HTTP trigger, but instead of writing code tooexecute when that endpoint is called, you provide a URL tooa remote implementation.</span></span> <span data-ttu-id="de2f5-153">Ez lehetővé teszi több API forrásokból történő egyetlen API felülete, amelyek segítségével az ügyfelek tooconsume toocompose.</span><span class="sxs-lookup"><span data-stu-id="de2f5-153">This allows you toocompose multiple API sources into a single API surface which is easy for clients tooconsume.</span></span> <span data-ttu-id="de2f5-154">Ez különösen fontos, ha toobuild mikroszolgáltatások létrehozására, az API-t.</span><span class="sxs-lookup"><span data-stu-id="de2f5-154">This is particularly useful if you wish toobuild your API as microservices.</span></span>

<span data-ttu-id="de2f5-155">Egy is proxypont tooany HTTP erőforrás, például:</span><span class="sxs-lookup"><span data-stu-id="de2f5-155">A proxy can point tooany HTTP resource, such as:</span></span>
- <span data-ttu-id="de2f5-156">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="de2f5-156">Azure Functions</span></span> 
- <span data-ttu-id="de2f5-157">Az API-alkalmazások [az Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)</span><span class="sxs-lookup"><span data-stu-id="de2f5-157">API apps in [Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)</span></span>
- <span data-ttu-id="de2f5-158">A docker-tároló [Linux App Service](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)</span><span class="sxs-lookup"><span data-stu-id="de2f5-158">Docker containers in [App Service on Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)</span></span>
- <span data-ttu-id="de2f5-159">Más üzemeltetett API-k</span><span class="sxs-lookup"><span data-stu-id="de2f5-159">Any other hosted API</span></span>

<span data-ttu-id="de2f5-160">toolearn proxykat, kapcsolatos további információkért lásd: [használata az Azure Functions proxyk (előzetes verzió)].</span><span class="sxs-lookup"><span data-stu-id="de2f5-160">toolearn more about proxies, see [Working with Azure Functions Proxies (preview)].</span></span>

## <a name="create-your-first-proxy"></a><span data-ttu-id="de2f5-161">Az első proxy létrehozása</span><span class="sxs-lookup"><span data-stu-id="de2f5-161">Create your first proxy</span></span>

<span data-ttu-id="de2f5-162">Ez a szakasz hoz létre egy új proxy mely szolgál, egy előtér-tooyour általános API.</span><span class="sxs-lookup"><span data-stu-id="de2f5-162">In this section, you will create a new proxy which serves as a frontend tooyour overall API.</span></span> 

### <a name="setting-up-hello-frontend-environment"></a><span data-ttu-id="de2f5-163">Hello előtér környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="de2f5-163">Setting up hello frontend environment</span></span>

<span data-ttu-id="de2f5-164">Ismételje meg a hello túl[függvény-alkalmazás létrehozása](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) toocreate egy új funkció alkalmazást a amely hoz létre a proxy.</span><span class="sxs-lookup"><span data-stu-id="de2f5-164">Repeat hello steps too[Create a function app](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) toocreate a new function app in which you will create your proxy.</span></span> <span data-ttu-id="de2f5-165">Az új alkalmazás erre a célra hello előtér az API-hoz, és korábban Szerkesztés hello függvény alkalmazás erre a célra egy háttér.</span><span class="sxs-lookup"><span data-stu-id="de2f5-165">This new app will serve as hello frontend for our API, and hello function app you were previously editing will serve as a backend.</span></span>

<span data-ttu-id="de2f5-166">Keresse meg a tooyour új előtér függvény app hello portálon.</span><span class="sxs-lookup"><span data-stu-id="de2f5-166">Navigate tooyour new frontend function app in hello portal.</span></span>

<span data-ttu-id="de2f5-167">Válassza ki **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="de2f5-167">Select **Settings**.</span></span> <span data-ttu-id="de2f5-168">Majd átváltása **engedélyezése Azure Functions proxyk (előzetes verzió)** túl "a".</span><span class="sxs-lookup"><span data-stu-id="de2f5-168">Then toggle **Enable Azure Functions Proxies (preview)** too"On".</span></span>

<span data-ttu-id="de2f5-169">Válassza ki **Platform beállítások** válassza **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="de2f5-169">Select **Platform Settings** and choose **Application Settings**.</span></span>

<span data-ttu-id="de2f5-170">Görgessen lefelé, túl**Alkalmazásbeállítások** , és hozzon létre egy új beállítás "HELLO_HOST" kulcs.</span><span class="sxs-lookup"><span data-stu-id="de2f5-170">Scroll down too**App settings** and create a new setting with key "HELLO_HOST".</span></span> <span data-ttu-id="de2f5-171">Állítsa be a háttérkiszolgáló függvény alkalmazáshoz, a érték toohello gazdagépe pl. `<YourApp>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="de2f5-171">Set its value toohello host of your backend function app, such as `<YourApp>.azurewebsites.net`.</span></span> <span data-ttu-id="de2f5-172">Ez már korábban átmásolt a HTTP-függvény tesztelése során hello URL-cím része.</span><span class="sxs-lookup"><span data-stu-id="de2f5-172">This is part of hello URL that you copied earlier when testing your HTTP function.</span></span> <span data-ttu-id="de2f5-173">Ezt a beállítást később hello konfigurációban fog hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="de2f5-173">You'll reference this setting in hello configuration later.</span></span>

> [!NOTE] 
> <span data-ttu-id="de2f5-174">Alkalmazásbeállítások ajánlottak hello gazdagép konfigurációs tooprevent hello proxy kódolt környezet függőségei.</span><span class="sxs-lookup"><span data-stu-id="de2f5-174">App settings are recommended for hello host configuration tooprevent a hard-coded environment dependency for hello proxy.</span></span> <span data-ttu-id="de2f5-175">Alkalmazásbeállítások használata azt jelenti, hogy hello proxykonfigurációt áthelyezheti különböző környezetek között, hello környezetfüggő app beállítások lesznek alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="de2f5-175">Using app settings means that you can move hello proxy configuration between environments, and hello environment-specific app settings will be applied.</span></span>

<span data-ttu-id="de2f5-176">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="de2f5-176">Click **Save**.</span></span>

### <a name="creating-a-proxy-on-hello-frontend"></a><span data-ttu-id="de2f5-177">A proxy hello előtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="de2f5-177">Creating a proxy on hello frontend</span></span>

<span data-ttu-id="de2f5-178">Lépjen vissza tooyour előtér függvény app hello portálon.</span><span class="sxs-lookup"><span data-stu-id="de2f5-178">Navigate back tooyour frontend function app in hello portal.</span></span>

<span data-ttu-id="de2f5-179">Hello bal oldali navigációs, kattintson a plusz jelre "+" következő hello túl "Proxyk (előzetes verzió)".</span><span class="sxs-lookup"><span data-stu-id="de2f5-179">In hello left-hand navigation, click hello plus sign '+' next too"Proxies (preview)".</span></span>

![A proxy létrehozása](./media/functions-create-serverless-api/creating-proxy.png)

<span data-ttu-id="de2f5-181">Proxybeállításainak használata hello táblázatban megadottak szerint.</span><span class="sxs-lookup"><span data-stu-id="de2f5-181">Use proxy settings as specified in hello table.</span></span>

| <span data-ttu-id="de2f5-182">Mező</span><span class="sxs-lookup"><span data-stu-id="de2f5-182">Field</span></span> | <span data-ttu-id="de2f5-183">Mintaérték</span><span class="sxs-lookup"><span data-stu-id="de2f5-183">Sample value</span></span> | <span data-ttu-id="de2f5-184">Leírás</span><span class="sxs-lookup"><span data-stu-id="de2f5-184">Description</span></span> |
|---|---|---|
| <span data-ttu-id="de2f5-185">Név</span><span class="sxs-lookup"><span data-stu-id="de2f5-185">Name</span></span> | <span data-ttu-id="de2f5-186">HelloProxy</span><span class="sxs-lookup"><span data-stu-id="de2f5-186">HelloProxy</span></span> | <span data-ttu-id="de2f5-187">Rövid név csak olyan felügyeleti</span><span class="sxs-lookup"><span data-stu-id="de2f5-187">A friendly name used only for management</span></span> |
| <span data-ttu-id="de2f5-188">Útvonal-sablon</span><span class="sxs-lookup"><span data-stu-id="de2f5-188">Route template</span></span> | <span data-ttu-id="de2f5-189">/ api/hello</span><span class="sxs-lookup"><span data-stu-id="de2f5-189">/api/hello</span></span> | <span data-ttu-id="de2f5-190">Meghatározza, hogy milyen útvonal használt tooinvoke a proxy</span><span class="sxs-lookup"><span data-stu-id="de2f5-190">Determines what route is used tooinvoke this proxy</span></span> |
| <span data-ttu-id="de2f5-191">Háttérkiszolgáló URL-címe</span><span class="sxs-lookup"><span data-stu-id="de2f5-191">Backend URL</span></span> | <span data-ttu-id="de2f5-192">https://%HELLO_HOST%/API/hello</span><span class="sxs-lookup"><span data-stu-id="de2f5-192">https://%HELLO_HOST%/api/hello</span></span> | <span data-ttu-id="de2f5-193">Megadja a hello végpont toowhich hello kérelem küldése a proxyn keresztül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="de2f5-193">Specifies hello endpoint toowhich hello request should be proxied</span></span> |

<span data-ttu-id="de2f5-194">Ne feledje, hogy a proxyk nem nyújtanak hello `/api` alap elérési útja előtag és a hello útvonalsablonhoz szerepelnie kell.</span><span class="sxs-lookup"><span data-stu-id="de2f5-194">Note that Proxies does not provide hello `/api` base path prefix, and this must be included in hello route template.</span></span>

<span data-ttu-id="de2f5-195">Hello `%HELLO_HOST%` szintaxis használatával a korábban létrehozott hello Alkalmazásbeállítás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="de2f5-195">hello `%HELLO_HOST%` syntax will reference hello app setting you created earlier.</span></span> <span data-ttu-id="de2f5-196">hello megoldás URL-címet fog mutatni tooyour eredeti funkciókat.</span><span class="sxs-lookup"><span data-stu-id="de2f5-196">hello resolved URL will point tooyour original function.</span></span>

<span data-ttu-id="de2f5-197">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="de2f5-197">Click **Create**.</span></span>

<span data-ttu-id="de2f5-198">Az új proxy kipróbálhatja hello Proxy URL-cím másolásával és a tesztelés hello böngészőben vagy a kedvenc HTTP-ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="de2f5-198">You can try out your new proxy by copying hello Proxy URL and testing it in hello browser or with your favorite HTTP client.</span></span>

## <a name="create-a-mock-api"></a><span data-ttu-id="de2f5-199">Egy utánzatait API létrehozása</span><span class="sxs-lookup"><span data-stu-id="de2f5-199">Create a mock API</span></span>

<span data-ttu-id="de2f5-200">A proxy toocreate utánzatait API a következő megoldást fogja használni.</span><span class="sxs-lookup"><span data-stu-id="de2f5-200">Next, you will use a proxy toocreate a mock API for your solution.</span></span> <span data-ttu-id="de2f5-201">Ez lehetővé teszi az ügyfél fejlesztési tooprogress, anélkül, hogy teljesen végre hello háttér.</span><span class="sxs-lookup"><span data-stu-id="de2f5-201">This allows client development tooprogress, without needing hello backend fully implemented.</span></span> <span data-ttu-id="de2f5-202">Később a fejlesztési hozzon létre egy új függvény alkalmazást, amely támogatja a logikai és a proxy tooit átirányítási sikerült.</span><span class="sxs-lookup"><span data-stu-id="de2f5-202">Later in development, you could create a new function app which supports this logic and redirect your proxy tooit.</span></span>

<span data-ttu-id="de2f5-203">toocreate ez mock API, létre fogunk hozni egy új proxy ezúttal hello [App Service-szerkesztő](https://github.com/projectkudu/kudu/wiki/App-Service-Editor).</span><span class="sxs-lookup"><span data-stu-id="de2f5-203">toocreate this mock API, we will create a new proxy, this time using hello [App Service Editor](https://github.com/projectkudu/kudu/wiki/App-Service-Editor).</span></span> <span data-ttu-id="de2f5-204">tooget indult el, keresse meg a tooyour függvény app hello portálon.</span><span class="sxs-lookup"><span data-stu-id="de2f5-204">tooget started, navigate tooyour function app in hello portal.</span></span> <span data-ttu-id="de2f5-205">Válassza ki **Platform funkciói** és található **App Service-szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="de2f5-205">Select **Platform features** and find **App Service Editor**.</span></span> <span data-ttu-id="de2f5-206">A hivatkozásra kattintva az megnyílik az App Service-szerkesztő hello új lapon.</span><span class="sxs-lookup"><span data-stu-id="de2f5-206">Clicking this will open hello App Service Editor in a new tab.</span></span>

<span data-ttu-id="de2f5-207">Válassza ki `proxies.json` a bal oldali navigációs hello.</span><span class="sxs-lookup"><span data-stu-id="de2f5-207">Select `proxies.json` in hello left navigation.</span></span> <span data-ttu-id="de2f5-208">Ez az hello fájlt, amely az összes a proxyk hello konfigurációs tárolja.</span><span class="sxs-lookup"><span data-stu-id="de2f5-208">This is hello file which stores hello configuration for all of your proxies.</span></span> <span data-ttu-id="de2f5-209">Hello használatakor [működik a központi telepítési módszerekkel](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), hello fájl verziókezelő rendszer megmaradjanak.</span><span class="sxs-lookup"><span data-stu-id="de2f5-209">If you use one of hello [Functions deployment methods](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), this is hello file you will maintain in source control.</span></span> <span data-ttu-id="de2f5-210">További információ a fájl toolearn lásd: [proxyk speciális konfigurációs](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).</span><span class="sxs-lookup"><span data-stu-id="de2f5-210">toolearn more about this file, see [Proxies advanced configuration](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).</span></span>

<span data-ttu-id="de2f5-211">Ha végrehajtotta eddig, a proxies.json hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="de2f5-211">If you've followed along so far, your proxies.json should look like hello following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        }
    }
}
```

<span data-ttu-id="de2f5-212">Ezután a utánzatait API fogja hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="de2f5-212">Next you'll add your mock API.</span></span> <span data-ttu-id="de2f5-213">Cserélje le a proxies.json fájl hello következő:</span><span class="sxs-lookup"><span data-stu-id="de2f5-213">Replace your proxies.json file with hello following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        },
        "GetUserByName" : {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/users/{username}"
            },
            "responseOverrides": {
                "response.statusCode": "200",
                "response.headers.Content-Type" : "application/json",
                "response.body": {
                    "name": "{username}",
                    "description": "Awesome developer and master of serverless APIs",
                    "skills": [
                        "Serverless",
                        "APIs",
                        "Azure",
                        "Cloud"
                    ]
                }
            }
        }
    }
}
```

<span data-ttu-id="de2f5-214">Ezzel hozzáad egy új proxy, "GetUserByName" hello backendUri tulajdonság nélkül.</span><span class="sxs-lookup"><span data-stu-id="de2f5-214">This adds a new proxy, "GetUserByName", without hello backendUri property.</span></span> <span data-ttu-id="de2f5-215">Helyett egy másik erőforrás, módosítja egy válasz felülbírálás használatával proxyk hello alapértelmezett válaszát.</span><span class="sxs-lookup"><span data-stu-id="de2f5-215">Instead of calling another resource, it modifies hello default response from Proxies using a response override.</span></span> <span data-ttu-id="de2f5-216">Kérelem és válasz felülbírálások használhatók a URL-t együtt.</span><span class="sxs-lookup"><span data-stu-id="de2f5-216">Request and response overrides can also be used in conjunction with a backend URL.</span></span> <span data-ttu-id="de2f5-217">Ez akkor különösen hasznos, ha a proxy használatát az ügynökökön tooa örökölt rendszer, amikor szükség lehet toomodify fejlécek, lekérdezési paraméterek, toolearn stb. További információk a kérelem-válasz felülbírálások, lásd a [kérelmeit és válaszait a proxyk módosítása](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).</span><span class="sxs-lookup"><span data-stu-id="de2f5-217">This is particularly useful when proxying tooa legacy system, where you might need toomodify headers, query parameters, etc. toolearn more about request and response overrides, see [Modifying requests and responses in Proxies](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).</span></span>

<span data-ttu-id="de2f5-218">Tesztelje az utánzatait API-t hívó hello `/api/users/{username}` végpont egy böngésző vagy a kedvenc REST-ügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="de2f5-218">Test your mock API by calling hello `/api/users/{username}` endpoint using a browser or your favorite REST client.</span></span> <span data-ttu-id="de2f5-219">Lehet, hogy tooreplace _{username}_ egy karakterláncértéket, amely egy felhasználónévvel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="de2f5-219">Be sure tooreplace _{username}_ with a string value representing a username.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de2f5-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de2f5-220">Next steps</span></span>

<span data-ttu-id="de2f5-221">Ebben az oktatóanyagban, megtudta, hogyan toobuild és testreszabása az Azure Functions egy API-t.</span><span class="sxs-lookup"><span data-stu-id="de2f5-221">In this tutorial, you learned how toobuild and customize an API on Azure Functions.</span></span> <span data-ttu-id="de2f5-222">Is megtanulta, hogyan toobring több API-k, beleértve a mocks, együtt, egy egyesített API felületen.</span><span class="sxs-lookup"><span data-stu-id="de2f5-222">You also learned how toobring multiple APIs, including mocks, together as a unified API surface.</span></span> <span data-ttu-id="de2f5-223">Használhat ezen módszerek toobuild kimenő bármely összetettségi API-k, a kiszolgáló nélküli hello futtatott összes számítási modellt az Azure Functions által biztosított.</span><span class="sxs-lookup"><span data-stu-id="de2f5-223">You can use these techniques toobuild out APIs of any complexity, all while running on hello serverless compute model provided by Azure Functions.</span></span>

<span data-ttu-id="de2f5-224">hello következő hivatkozásokat hasznos lehet, az API-további fejleszt:</span><span class="sxs-lookup"><span data-stu-id="de2f5-224">hello following references may be helpful as you develop your API further:</span></span>

- [<span data-ttu-id="de2f5-225">Az Azure Functions HTTP és a webhook kötések</span><span class="sxs-lookup"><span data-stu-id="de2f5-225">Azure Functions HTTP and webhook bindings</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- <span data-ttu-id="de2f5-226">[használata az Azure Functions proxyk (előzetes verzió)]</span><span class="sxs-lookup"><span data-stu-id="de2f5-226">[Working with Azure Functions Proxies (preview)]</span></span>
- [<span data-ttu-id="de2f5-227">Az Azure Functions API-k (előzetes verzió) dokumentálása</span><span class="sxs-lookup"><span data-stu-id="de2f5-227">Documenting an Azure Functions API (preview)</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[használata az Azure Functions proxyk (előzetes verzió)]: https://docs.microsoft.com/azure/azure-functions/functions-proxies

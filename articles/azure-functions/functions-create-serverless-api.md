---
title: "Az Azure Functions használatával egy kiszolgáló nélküli API létrehozása |} Microsoft Docs"
description: "Az Azure Functions használatával egy kiszolgáló nélküli API létrehozása"
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
ms.openlocfilehash: 28056a385b058f7daeca2253ccb304116b49eba0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-serverless-api-using-azure-functions"></a><span data-ttu-id="9d6e0-103">Az Azure Functions használatával egy kiszolgáló nélküli API létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-103">Create a serverless API using Azure Functions</span></span>

<span data-ttu-id="9d6e0-104">Ebből az oktatóanyagból megtudhatja, hogyan Azure Functions lehetővé teszi a magas szinten méretezhető API-k létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-104">In this tutorial you will learn how Azure Functions allows you to build highly-scalable APIs.</span></span> <span data-ttu-id="9d6e0-105">Az Azure Functions számos beépített HTTP eseményindítók és kötések, amely megkönnyíti a különböző nyelveken, beleértve a Node.JS, a C#, és több végpont szerzői gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-105">Azure Functions comes with a collection of built-in HTTP triggers and bindings which make it easy to author an endpoint in a variety of languages, including Node.JS, C#, and more.</span></span> <span data-ttu-id="9d6e0-106">Ebben az oktatóanyagban testre fogja szabni a konkrét műveletek az API kialakításában kezelni egy HTTP-eseményindítóval.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-106">In this tutorial, you will customize an HTTP trigger to handle specific actions in your API design.</span></span> <span data-ttu-id="9d6e0-107">Is előkészítheti a növekvő az API integrálása az Azure Functions proxyk és utánzatait API-k beállítása.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-107">You will also prepare for growing your API by integrating it with Azure Functions Proxies and setting up mock APIs.</span></span> <span data-ttu-id="9d6e0-108">Mindez érhető el, a funkciók kiszolgáló nélküli számítási környezet felett, nem kell foglalkoznia az erőforrások skálázás – a API logika csak összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-108">All of this is accomplished on top of the Functions serverless compute environment, so you don't have to worry about scaling resources - you can just focus on your API logic.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d6e0-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9d6e0-109">Prerequisites</span></span> 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

<span data-ttu-id="9d6e0-110">Az eredményül kapott funkció ebben az oktatóanyagban a többi fog történni.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-110">The resulting function will be used for the rest of this tutorial.</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="9d6e0-111">Bejelentkezés az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="9d6e0-111">Sign in to Azure</span></span>

<span data-ttu-id="9d6e0-112">Nyissa meg az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-112">Open the Azure portal.</span></span> <span data-ttu-id="9d6e0-113">Ehhez jelentkezzen be [https://portal.azure.com](https://portal.azure.com) fel fiókjával.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-113">To do this, sign in to [https://portal.azure.com](https://portal.azure.com) with your Azure account.</span></span>

## <a name="customize-your-http-function"></a><span data-ttu-id="9d6e0-114">A HTTP-funkció testreszabása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-114">Customize your HTTP function</span></span>

<span data-ttu-id="9d6e0-115">Alapértelmezés szerint a HTTP-eseményindítóval aktivált függvény HTTP-metódus elfogadására van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-115">By default, your HTTP-triggered function is configured to accept any HTTP method.</span></span> <span data-ttu-id="9d6e0-116">Emellett van alapértelmezett URL-cím a `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-116">There is also a default URL of the form `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`.</span></span> <span data-ttu-id="9d6e0-117">Ha a gyors üzembe helyezés, majd követi `<funcname>` valószínűleg valahogy "HttpTriggerJS1".</span><span class="sxs-lookup"><span data-stu-id="9d6e0-117">If you followed the quickstart, then `<funcname>` probably looks something like "HttpTriggerJS1".</span></span> <span data-ttu-id="9d6e0-118">Ebben a szakaszban módosítani fogja a függvény csak a GET kérelmek elleni megválaszolásához `/api/hello` útvonal helyett.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-118">In this section, you will modify the function to respond only to GET requests against `/api/hello` route instead.</span></span> 

<span data-ttu-id="9d6e0-119">Nyissa meg a függvény az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-119">Navigate to your function in the Azure portal.</span></span> <span data-ttu-id="9d6e0-120">Válassza ki **integráció** a bal oldali navigációs.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-120">Select **Integrate** in the left navigation.</span></span>

![Egy HTTP-funkció testreszabása](./media/functions-create-serverless-api/customizing-http.png)

<span data-ttu-id="9d6e0-122">A HTTP-eseményindító beállításokat használják a táblázatban megadott.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-122">Use the HTTP trigger settings as specified in the table.</span></span>

| <span data-ttu-id="9d6e0-123">Mező</span><span class="sxs-lookup"><span data-stu-id="9d6e0-123">Field</span></span> | <span data-ttu-id="9d6e0-124">Mintaérték</span><span class="sxs-lookup"><span data-stu-id="9d6e0-124">Sample value</span></span> | <span data-ttu-id="9d6e0-125">Leírás</span><span class="sxs-lookup"><span data-stu-id="9d6e0-125">Description</span></span> |
|---|---|---|
| <span data-ttu-id="9d6e0-126">Engedélyezett HTTP-metódusok</span><span class="sxs-lookup"><span data-stu-id="9d6e0-126">Allowed HTTP methods</span></span> | <span data-ttu-id="9d6e0-127">A kijelölt metódusok</span><span class="sxs-lookup"><span data-stu-id="9d6e0-127">Selected methods</span></span> | <span data-ttu-id="9d6e0-128">Meghatározza, hogy mely HTTP-metódus is használható a függvény meghívása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-128">Determines what HTTP methods may be used to invoke this function</span></span> |
| <span data-ttu-id="9d6e0-129">A kijelölt HTTP-metódusok</span><span class="sxs-lookup"><span data-stu-id="9d6e0-129">Selected HTTP methods</span></span> | <span data-ttu-id="9d6e0-130">GET</span><span class="sxs-lookup"><span data-stu-id="9d6e0-130">GET</span></span> | <span data-ttu-id="9d6e0-131">Lehetővé teszi, hogy csak a kijelölt HTTP metódusok használandó, a függvény meghívása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-131">Allows only selected HTTP methods to be used to invoke this function</span></span> |
| <span data-ttu-id="9d6e0-132">Útvonal-sablon</span><span class="sxs-lookup"><span data-stu-id="9d6e0-132">Route template</span></span> | <span data-ttu-id="9d6e0-133">hello</span><span class="sxs-lookup"><span data-stu-id="9d6e0-133">/hello</span></span> | <span data-ttu-id="9d6e0-134">Meghatározza, hogy milyen útvonalat használ a függvény meghívása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-134">Determines what route is used to invoke this function</span></span> |

<span data-ttu-id="9d6e0-135">Vegye figyelembe, hogy Ön nem foglal a `/api` kiinduló az útvonalsablonhoz elérési előtagot, mivel ez egy globális beállítás kezeli.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-135">Note that you did not include the `/api` base path prefix in the route template, as this is handled by a global setting.</span></span>

<span data-ttu-id="9d6e0-136">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-136">Click **Save**.</span></span>

<span data-ttu-id="9d6e0-137">A HTTP-funkciók testreszabása többet is megtudhat [Azure Functions HTTP és a webhook kötések](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).</span><span class="sxs-lookup"><span data-stu-id="9d6e0-137">You can learn more about customizing HTTP functions in [Azure Functions HTTP and webhook bindings](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).</span></span>

### <a name="test-your-api"></a><span data-ttu-id="9d6e0-138">Az API-teszt</span><span class="sxs-lookup"><span data-stu-id="9d6e0-138">Test your API</span></span>

<span data-ttu-id="9d6e0-139">Ezt követően tesztelje meg szeretné tekinteni, az új API felület használata a függvénynek.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-139">Next, test your function to see it working with the new API surface.</span></span>

<span data-ttu-id="9d6e0-140">Lépjen vissza a fejlesztési lapon kattintson a bal oldali navigációs a függvény nevét.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-140">Navigate back to the development page by clicking on the function's name in the left navigation.</span></span>

<span data-ttu-id="9d6e0-141">Kattintson a **függvény URL-cím beszerzése** és másolja az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-141">Click **Get function URL** and copy the URL.</span></span> <span data-ttu-id="9d6e0-142">Láthatja, hogy használja a `/api/hello` címre.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-142">You should see that it uses the `/api/hello` route now.</span></span>

<span data-ttu-id="9d6e0-143">Másolja az URL-címet egy új böngészőlapon vagy a kívánt REST-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-143">Copy the URL into a new browser tab or your preferred REST client.</span></span> <span data-ttu-id="9d6e0-144">Böngészők alapértelmezés szerint fogja használni a GET.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-144">Browsers will use GET by default.</span></span>

<span data-ttu-id="9d6e0-145">A funkció futtatásához, és győződjön meg arról, hogy működik.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-145">Run the function and confirm that it is working.</span></span> <span data-ttu-id="9d6e0-146">Adja meg a "név" paraméternek a gyors üzembe helyezés kód kielégítéséhez lekérdezési karakterláncként szeretne.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-146">You may need to provide the "name" parameter as a query string to satisfy the quickstart code.</span></span>

<span data-ttu-id="9d6e0-147">A végpont meghívása a megerősítéséhez, hogy a függvény nem hajtotta végre egy másik HTTP-metódussal is próbálkozhat.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-147">You can also try calling the endpoint with another HTTP method to confirm that the function is not executed.</span></span> <span data-ttu-id="9d6e0-148">Ehhez szüksége lesz egy REST-ügyfél, például a cURL, a Postman vagy a Fiddler használata.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-148">For this, you will need to use a REST client, such as cURL, Postman, or Fiddler.</span></span>

## <a name="proxies-overview"></a><span data-ttu-id="9d6e0-149">Proxyk áttekintése</span><span class="sxs-lookup"><span data-stu-id="9d6e0-149">Proxies overview</span></span>

<span data-ttu-id="9d6e0-150">A következő szakaszban az API-proxyn keresztül fog surface.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-150">In the next section, you will surface your API through a proxy.</span></span> <span data-ttu-id="9d6e0-151">Az Azure Functions proxyk, amely lehetővé teszi a kérelmek továbbítása más erőforrások előzetes verziójú funkciók.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-151">Azure Functions Proxies is a preview feature that allows you to forward requests to other resources.</span></span> <span data-ttu-id="9d6e0-152">Ugyanúgy, mint a HTTP-eseményindítóval, de kódot mikor hajthat végre ahelyett, hogy a végpont neve HTTP végpont meghatározása, megadja a távoli megvalósításának URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-152">You define an HTTP endpoint just like with HTTP trigger, but instead of writing code to execute when that endpoint is called, you provide a URL to a remote implementation.</span></span> <span data-ttu-id="9d6e0-153">Ez lehetővé teszi, hogy több API-forrás írja be az ügyfelek használhatnak könnyen egyetlen API felülete.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-153">This allows you to compose multiple API sources into a single API surface which is easy for clients to consume.</span></span> <span data-ttu-id="9d6e0-154">Ez különösen fontos, ha az API-t, mikroszolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-154">This is particularly useful if you wish to build your API as microservices.</span></span>

<span data-ttu-id="9d6e0-155">A proxy, mint a HTTP-erőforrás mutathat:</span><span class="sxs-lookup"><span data-stu-id="9d6e0-155">A proxy can point to any HTTP resource, such as:</span></span>
- <span data-ttu-id="9d6e0-156">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="9d6e0-156">Azure Functions</span></span> 
- <span data-ttu-id="9d6e0-157">Az API-alkalmazások [az Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)</span><span class="sxs-lookup"><span data-stu-id="9d6e0-157">API apps in [Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)</span></span>
- <span data-ttu-id="9d6e0-158">A docker-tároló [Linux App Service](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)</span><span class="sxs-lookup"><span data-stu-id="9d6e0-158">Docker containers in [App Service on Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)</span></span>
- <span data-ttu-id="9d6e0-159">Más üzemeltetett API-k</span><span class="sxs-lookup"><span data-stu-id="9d6e0-159">Any other hosted API</span></span>

<span data-ttu-id="9d6e0-160">Proxyk kapcsolatos további információkért lásd: [használata az Azure Functions proxyk (előzetes verzió)].</span><span class="sxs-lookup"><span data-stu-id="9d6e0-160">To learn more about proxies, see [Working with Azure Functions Proxies (preview)].</span></span>

## <a name="create-your-first-proxy"></a><span data-ttu-id="9d6e0-161">Az első proxy létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-161">Create your first proxy</span></span>

<span data-ttu-id="9d6e0-162">Ebben a szakaszban egy új proxy, amely az általános API-nak időtúllépést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-162">In this section, you will create a new proxy which serves as a frontend to your overall API.</span></span> 

### <a name="setting-up-the-frontend-environment"></a><span data-ttu-id="9d6e0-163">Az előtér-környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-163">Setting up the frontend environment</span></span>

<span data-ttu-id="9d6e0-164">Ismételje meg a [függvény-alkalmazás létrehozása](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) függvény a proxy hoz létre új alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-164">Repeat the steps to [Create a function app](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) to create a new function app in which you will create your proxy.</span></span> <span data-ttu-id="9d6e0-165">Az új alkalmazás erre a célra a frontend az API-hoz, és a korábban Szerkesztés függvény alkalmazás erre a célra egy háttér.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-165">This new app will serve as the frontend for our API, and the function app you were previously editing will serve as a backend.</span></span>

<span data-ttu-id="9d6e0-166">Nyissa meg az új előtér függvény alkalmazás a portálon.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-166">Navigate to your new frontend function app in the portal.</span></span>

<span data-ttu-id="9d6e0-167">Válassza ki **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-167">Select **Settings**.</span></span> <span data-ttu-id="9d6e0-168">Majd átváltása **engedélyezése Azure Functions proxyk (előzetes verzió)** "A" számára.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-168">Then toggle **Enable Azure Functions Proxies (preview)** to "On".</span></span>

<span data-ttu-id="9d6e0-169">Válassza ki **Platform beállítások** válassza **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-169">Select **Platform Settings** and choose **Application Settings**.</span></span>

<span data-ttu-id="9d6e0-170">Görgessen le a **Alkalmazásbeállítások** , és hozzon létre egy új beállítás "HELLO_HOST" kulcs.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-170">Scroll down to **App settings** and create a new setting with key "HELLO_HOST".</span></span> <span data-ttu-id="9d6e0-171">Állítsa be az értékét, mint a háttéralkalmazás függvény alkalmazáshoz, a gazdagép `<YourApp>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-171">Set its value to the host of your backend function app, such as `<YourApp>.azurewebsites.net`.</span></span> <span data-ttu-id="9d6e0-172">Ez a korábban kimásolt a HTTP-függvény tesztelése során URL-cím része.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-172">This is part of the URL that you copied earlier when testing your HTTP function.</span></span> <span data-ttu-id="9d6e0-173">Ez a beállítás később a konfigurációban kell hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-173">You'll reference this setting in the configuration later.</span></span>

> [!NOTE] 
> <span data-ttu-id="9d6e0-174">Alkalmazásbeállítások a gazdagép konfigurációja a proxy-sablonja környezet függőség megelőzése érdekében ajánlott.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-174">App settings are recommended for the host configuration to prevent a hard-coded environment dependency for the proxy.</span></span> <span data-ttu-id="9d6e0-175">Alkalmazásbeállítások használata azt jelenti, hogy a proxy konfigurációs áthelyezheti különböző környezetek között, a környezet-specifikus alkalmazás beállítások lesznek alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-175">Using app settings means that you can move the proxy configuration between environments, and the environment-specific app settings will be applied.</span></span>

<span data-ttu-id="9d6e0-176">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-176">Click **Save**.</span></span>

### <a name="creating-a-proxy-on-the-frontend"></a><span data-ttu-id="9d6e0-177">Az előtér egy proxy létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-177">Creating a proxy on the frontend</span></span>

<span data-ttu-id="9d6e0-178">Lépjen vissza az előtér-függvény alkalmazás a portálon.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-178">Navigate back to your frontend function app in the portal.</span></span>

<span data-ttu-id="9d6e0-179">A bal oldali navigációs, kattintson a plusz jelre "+" "Proxyk (előzetes verzió)" mellett.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-179">In the left-hand navigation, click the plus sign '+' next to "Proxies (preview)".</span></span>

![A proxy létrehozása](./media/functions-create-serverless-api/creating-proxy.png)

<span data-ttu-id="9d6e0-181">A táblázatban megadott proxybeállításainak használata.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-181">Use proxy settings as specified in the table.</span></span>

| <span data-ttu-id="9d6e0-182">Mező</span><span class="sxs-lookup"><span data-stu-id="9d6e0-182">Field</span></span> | <span data-ttu-id="9d6e0-183">Mintaérték</span><span class="sxs-lookup"><span data-stu-id="9d6e0-183">Sample value</span></span> | <span data-ttu-id="9d6e0-184">Leírás</span><span class="sxs-lookup"><span data-stu-id="9d6e0-184">Description</span></span> |
|---|---|---|
| <span data-ttu-id="9d6e0-185">Név</span><span class="sxs-lookup"><span data-stu-id="9d6e0-185">Name</span></span> | <span data-ttu-id="9d6e0-186">HelloProxy</span><span class="sxs-lookup"><span data-stu-id="9d6e0-186">HelloProxy</span></span> | <span data-ttu-id="9d6e0-187">Rövid név csak olyan felügyeleti</span><span class="sxs-lookup"><span data-stu-id="9d6e0-187">A friendly name used only for management</span></span> |
| <span data-ttu-id="9d6e0-188">Útvonal-sablon</span><span class="sxs-lookup"><span data-stu-id="9d6e0-188">Route template</span></span> | <span data-ttu-id="9d6e0-189">/ api/hello</span><span class="sxs-lookup"><span data-stu-id="9d6e0-189">/api/hello</span></span> | <span data-ttu-id="9d6e0-190">Meghatározza, hogy milyen útvonalat használ a proxy meghívása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-190">Determines what route is used to invoke this proxy</span></span> |
| <span data-ttu-id="9d6e0-191">Háttérkiszolgáló URL-címe</span><span class="sxs-lookup"><span data-stu-id="9d6e0-191">Backend URL</span></span> | <span data-ttu-id="9d6e0-192">https://%HELLO_HOST%/API/hello</span><span class="sxs-lookup"><span data-stu-id="9d6e0-192">https://%HELLO_HOST%/api/hello</span></span> | <span data-ttu-id="9d6e0-193">Megadja a végpontot, amelyhez a kérés küldése a proxyn keresztül kell lennie</span><span class="sxs-lookup"><span data-stu-id="9d6e0-193">Specifies the endpoint to which the request should be proxied</span></span> |

<span data-ttu-id="9d6e0-194">Vegye figyelembe, hogy proxyk nem biztosít a `/api` alap elérési útja előtag, és ez az útvonalsablonhoz szerepelnie kell.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-194">Note that Proxies does not provide the `/api` base path prefix, and this must be included in the route template.</span></span>

<span data-ttu-id="9d6e0-195">A `%HELLO_HOST%` szintaxis használatával a korábban létrehozott Alkalmazásbeállítás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-195">The `%HELLO_HOST%` syntax will reference the app setting you created earlier.</span></span> <span data-ttu-id="9d6e0-196">A feloldott URL-címet az eredeti funkciókat mutasson.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-196">The resolved URL will point to your original function.</span></span>

<span data-ttu-id="9d6e0-197">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-197">Click **Create**.</span></span>

<span data-ttu-id="9d6e0-198">Az új proxy kipróbálhatja a proxykiszolgáló URL-cím másolásával és a vizsgálja, hogy a böngészőben, vagy a kedvenc HTTP-ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-198">You can try out your new proxy by copying the Proxy URL and testing it in the browser or with your favorite HTTP client.</span></span>

## <a name="create-a-mock-api"></a><span data-ttu-id="9d6e0-199">Egy utánzatait API létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-199">Create a mock API</span></span>

<span data-ttu-id="9d6e0-200">A következő szüksége lesz egy proxy utánzatait API a megoldás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-200">Next, you will use a proxy to create a mock API for your solution.</span></span> <span data-ttu-id="9d6e0-201">Ez lehetővé teszi, hogy folyamatban van, az ügyféloldali fejlesztés anélkül, hogy a háttér teljes végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-201">This allows client development to progress, without needing the backend fully implemented.</span></span> <span data-ttu-id="9d6e0-202">Később a fejlesztési hozzon létre egy új függvény alkalmazást, amely támogatja a logikai és a proxy átirányítása sikerült.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-202">Later in development, you could create a new function app which supports this logic and redirect your proxy to it.</span></span>

<span data-ttu-id="9d6e0-203">Az utánzatait API létrehozásához létrehozunk egy új proxyt a használatával a [App Service-szerkesztő](https://github.com/projectkudu/kudu/wiki/App-Service-Editor).</span><span class="sxs-lookup"><span data-stu-id="9d6e0-203">To create this mock API, we will create a new proxy, this time using the [App Service Editor](https://github.com/projectkudu/kudu/wiki/App-Service-Editor).</span></span> <span data-ttu-id="9d6e0-204">Először keresse meg a függvény app a portálon.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-204">To get started, navigate to your function app in the portal.</span></span> <span data-ttu-id="9d6e0-205">Válassza ki **Platform funkciói** és található **App Service-szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-205">Select **Platform features** and find **App Service Editor**.</span></span> <span data-ttu-id="9d6e0-206">A hivatkozásra kattintva az megnyílik az App Service-szerkesztő új ablakban.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-206">Clicking this will open the App Service Editor in a new tab.</span></span>

<span data-ttu-id="9d6e0-207">Válassza ki `proxies.json` a bal oldali navigációs.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-207">Select `proxies.json` in the left navigation.</span></span> <span data-ttu-id="9d6e0-208">Ez az a fájlt, amely az összes a proxyk konfigurációs tárolja.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-208">This is the file which stores the configuration for all of your proxies.</span></span> <span data-ttu-id="9d6e0-209">Ha valamelyikét használja a [működik a központi telepítési módszerekkel](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), a fájl megmaradjanak a verziókövetési rendszerrel.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-209">If you use one of the [Functions deployment methods](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), this is the file you will maintain in source control.</span></span> <span data-ttu-id="9d6e0-210">Ez a fájl kapcsolatos további információkért lásd: [proxyk speciális konfigurációs](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).</span><span class="sxs-lookup"><span data-stu-id="9d6e0-210">To learn more about this file, see [Proxies advanced configuration](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).</span></span>

<span data-ttu-id="9d6e0-211">Ha végrehajtotta eddig, a proxies.json kell a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="9d6e0-211">If you've followed along so far, your proxies.json should look like the following:</span></span>

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

<span data-ttu-id="9d6e0-212">Ezután a utánzatait API fogja hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-212">Next you'll add your mock API.</span></span> <span data-ttu-id="9d6e0-213">Cserélje le a proxies.json fájl a következő:</span><span class="sxs-lookup"><span data-stu-id="9d6e0-213">Replace your proxies.json file with the following:</span></span>

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

<span data-ttu-id="9d6e0-214">Ezzel hozzáad egy új proxy, "GetUserByName", a backendUri tulajdonság nélkül.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-214">This adds a new proxy, "GetUserByName", without the backendUri property.</span></span> <span data-ttu-id="9d6e0-215">Helyett egy másik erőforrás, módosítja egy válasz felülbírálás használatával proxyk alapértelmezett válaszát.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-215">Instead of calling another resource, it modifies the default response from Proxies using a response override.</span></span> <span data-ttu-id="9d6e0-216">Kérelem és válasz felülbírálások használhatók a URL-t együtt.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-216">Request and response overrides can also be used in conjunction with a backend URL.</span></span> <span data-ttu-id="9d6e0-217">Ez akkor különösen hasznos, ha a proxy használatát az ügynökökön egy örökölt rendszerre, ahol előfordulhat, hogy módosítania kell a fejlécek, lekérdezési paraméterek stb. Kérelem és válasz felülbírálások kapcsolatos további információkért lásd: [kérelmeit és válaszait a proxyk módosítása](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).</span><span class="sxs-lookup"><span data-stu-id="9d6e0-217">This is particularly useful when proxying to a legacy system, where you might need to modify headers, query parameters, etc. To learn more about request and response overrides, see [Modifying requests and responses in Proxies](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).</span></span>

<span data-ttu-id="9d6e0-218">Tesztelje a utánzatait API hívása a `/api/users/{username}` végpont egy böngésző vagy a kedvenc REST-ügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-218">Test your mock API by calling the `/api/users/{username}` endpoint using a browser or your favorite REST client.</span></span> <span data-ttu-id="9d6e0-219">Ügyeljen arra, hogy a csere _{username}_ egy karakterláncértéket, amely egy felhasználónévvel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-219">Be sure to replace _{username}_ with a string value representing a username.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d6e0-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d6e0-220">Next steps</span></span>

<span data-ttu-id="9d6e0-221">Ebben az oktatóanyagban megtanulta, hogyan állíthatja össze és testreszabása az Azure Functions egy API-t.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-221">In this tutorial, you learned how to build and customize an API on Azure Functions.</span></span> <span data-ttu-id="9d6e0-222">Is megtanulta, hogyan kell az több API-k, mocks, beleértve a együtt, egyetlen egyesített API felület.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-222">You also learned how to bring multiple APIs, including mocks, together as a unified API surface.</span></span> <span data-ttu-id="9d6e0-223">Ezek a technológiák használatával ki bármely összetettségi API-k létrehozása minden, a kiszolgáló nélküli számítási modellt a futtatása közben az Azure Functions által biztosított.</span><span class="sxs-lookup"><span data-stu-id="9d6e0-223">You can use these techniques to build out APIs of any complexity, all while running on the serverless compute model provided by Azure Functions.</span></span>

<span data-ttu-id="9d6e0-224">Az API-további fejleszt, hasznos lehet a következő hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="9d6e0-224">The following references may be helpful as you develop your API further:</span></span>

- [<span data-ttu-id="9d6e0-225">Az Azure Functions HTTP és a webhook kötések</span><span class="sxs-lookup"><span data-stu-id="9d6e0-225">Azure Functions HTTP and webhook bindings</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- <span data-ttu-id="9d6e0-226">[használata az Azure Functions proxyk (előzetes verzió)]</span><span class="sxs-lookup"><span data-stu-id="9d6e0-226">[Working with Azure Functions Proxies (preview)]</span></span>
- [<span data-ttu-id="9d6e0-227">Az Azure Functions API-k (előzetes verzió) dokumentálása</span><span class="sxs-lookup"><span data-stu-id="9d6e0-227">Documenting an Azure Functions API (preview)</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[használata az Azure Functions proxyk (előzetes verzió)]: https://docs.microsoft.com/azure/azure-functions/functions-proxies

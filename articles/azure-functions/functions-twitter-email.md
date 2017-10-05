---
title: "Hozzon létre egy függvényt, amely az Azure Logic Apps |} Microsoft Docs"
description: "Hozzon létre egy függvényt, amely az Azure Logic Apps és az Azure kognitív szolgáltatások tweetet véleményeket kategorizálását, és értesítést küld, ha véleményeket gyenge."
services: functions, logic-apps, cognitive-services
keywords: "munkafolyamat, felhőalapú alkalmazások, felhőszolgáltatások, üzleti folyamatok, rendszerintegráció, vállalati alkalmazásintegráció, EAI"
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 4a5dc668e21c5328b308c8f5852aaa922232374d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a><span data-ttu-id="65311-104">Hozzon létre egy függvényt, amely az Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="65311-104">Create a function that integrates with Azure Logic Apps</span></span>

<span data-ttu-id="65311-105">Az Azure Functions integrálja az Azure Logic Apps a Logic Apps-tervezőben.</span><span class="sxs-lookup"><span data-stu-id="65311-105">Azure Functions integrates with Azure Logic Apps in the Logic Apps Designer.</span></span> <span data-ttu-id="65311-106">Ez az integráció lehetővé teszi a álló üzenettípusok összehangolását funkciók a számítási teljesítményt az egyéb Azure és a harmadik féltől származó szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="65311-106">This integration lets you use the computing power of Functions in orchestrations with other Azure and third-party services.</span></span> 

<span data-ttu-id="65311-107">Ez az oktatóanyag bemutatja, hogyan használható funkciók Logic Apps és kognitív Azure-szolgáltatások elemzésére Twitter-bejegyzéseket a céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="65311-107">This tutorial shows you how to use Functions with Logic Apps and Azure Cognitive Services to analyze sentiment from Twitter posts.</span></span> <span data-ttu-id="65311-108">Az indított HTTP függvény kategorizálja Twitter-üzeneteket, mint a zöld, sárga vagy piros a céggel kapcsolatos véleményeket pontszám alapján.</span><span class="sxs-lookup"><span data-stu-id="65311-108">An HTTP triggered function categorizes tweets as green, yellow, or red based on the sentiment score.</span></span> <span data-ttu-id="65311-109">Az e-mail elküldésekor történik a gyenge véleményeket észlelése esetén.</span><span class="sxs-lookup"><span data-stu-id="65311-109">An email is sent when poor sentiment is detected.</span></span> 

![kép két lépést a Logic App Designer alkalmazás](media/functions-twitter-email/designer1.png)

<span data-ttu-id="65311-111">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="65311-111">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="65311-112">Kognitív Services-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="65311-112">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="65311-113">Hozzon létre egy függvényt, Kategorizáló tweetet céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="65311-113">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="65311-114">Twitter csatlakozó logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="65311-114">Create a logic app that connects to Twitter.</span></span>
> * <span data-ttu-id="65311-115">Véleményeket észlelési hozzáadása a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="65311-115">Add sentiment detection to the logic app.</span></span> 
> * <span data-ttu-id="65311-116">Csatlakoztassa a logikai alkalmazást a függvény.</span><span class="sxs-lookup"><span data-stu-id="65311-116">Connect the logic app to the function.</span></span>
> * <span data-ttu-id="65311-117">Küldjön egy e-mailt a választ, a függvény alapján.</span><span class="sxs-lookup"><span data-stu-id="65311-117">Send an email based on the response from the function.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65311-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="65311-118">Prerequisites</span></span>

+ <span data-ttu-id="65311-119">Az aktív [Twitter](https://twitter.com/) fiók.</span><span class="sxs-lookup"><span data-stu-id="65311-119">An active [Twitter](https://twitter.com/) account.</span></span> 
+ <span data-ttu-id="65311-120">Egy [Outlook.com-os](https://outlook.com/) fiókot (a értesítések küldése).</span><span class="sxs-lookup"><span data-stu-id="65311-120">An [Outlook.com](https://outlook.com/) account (for sending notifications).</span></span>
+ <span data-ttu-id="65311-121">A témakör kiindulópontjául [Az első függvény létrehozása az Azure Portalon](functions-create-first-azure-function.md) című cikkben létrehozott erőforrások szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="65311-121">This topic uses as its starting point the resources created in [Create your first function from the Azure portal](functions-create-first-azure-function.md).</span></span>  
<span data-ttu-id="65311-122">Ha még nem tette meg, végezze el most ezeket a lépéseket a függvény-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="65311-122">If you haven't already done so, complete these steps now to create your function app.</span></span>

## <a name="create-a-cognitive-services-account"></a><span data-ttu-id="65311-123">Kognitív Services-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="65311-123">Create a Cognitive Services account</span></span>

<span data-ttu-id="65311-124">A figyelt Twitter-üzeneteket a céggel kapcsolatos véleményeket egy kognitív Services-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="65311-124">A Cognitive Services account is required to detect the sentiment of tweets being monitored.</span></span>

1. <span data-ttu-id="65311-125">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="65311-125">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="65311-126">Kattintson az Azure Portal bal felső sarkában található **Új** gombra.</span><span class="sxs-lookup"><span data-stu-id="65311-126">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

3. <span data-ttu-id="65311-127">Kattintson a **adatok + analitika** > **kognitív szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="65311-127">Click **Data + Analytics** > **Cognitive  Services**.</span></span> <span data-ttu-id="65311-128">Ezután használja a beállítások a táblázatban megadott, fogadja el a feltételeket, és ellenőrizze **rögzítés az irányítópulton**.</span><span class="sxs-lookup"><span data-stu-id="65311-128">Then, use the settings as specified in the table, accept the terms, and check **Pin to dashboard**.</span></span>

    ![Hozzon létre kognitív fiók panelen](media/functions-twitter-email/cog_svcs_account.png)

    | <span data-ttu-id="65311-130">Beállítás</span><span class="sxs-lookup"><span data-stu-id="65311-130">Setting</span></span>      |  <span data-ttu-id="65311-131">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="65311-131">Suggested value</span></span>   | <span data-ttu-id="65311-132">Leírás</span><span class="sxs-lookup"><span data-stu-id="65311-132">Description</span></span>                                        |
    | --- | --- | --- |
    | <span data-ttu-id="65311-133">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="65311-133">**Name**</span></span> | <span data-ttu-id="65311-134">MyCognitiveServicesAccnt</span><span class="sxs-lookup"><span data-stu-id="65311-134">MyCognitiveServicesAccnt</span></span> | <span data-ttu-id="65311-135">Válasszon egy egyedi fióknevet.</span><span class="sxs-lookup"><span data-stu-id="65311-135">Choose a unique account name.</span></span> |
    | <span data-ttu-id="65311-136">**API-típus**</span><span class="sxs-lookup"><span data-stu-id="65311-136">**API type**</span></span> | <span data-ttu-id="65311-137">Szövegelemzések API</span><span class="sxs-lookup"><span data-stu-id="65311-137">Text Analytics API</span></span> | <span data-ttu-id="65311-138">Szöveg elemzésére használt API.</span><span class="sxs-lookup"><span data-stu-id="65311-138">API used to analyze text.</span></span>  |
    | <span data-ttu-id="65311-139">**Hely**</span><span class="sxs-lookup"><span data-stu-id="65311-139">**Location**</span></span> | <span data-ttu-id="65311-140">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="65311-140">West US</span></span> | <span data-ttu-id="65311-141">Jelenleg csak **USA nyugati régiója** szövegelemzések érhető el.</span><span class="sxs-lookup"><span data-stu-id="65311-141">Currently, only **West US** is available for text analytics.</span></span> |
    | <span data-ttu-id="65311-142">**Tarifacsomag**</span><span class="sxs-lookup"><span data-stu-id="65311-142">**Pricing tier**</span></span> | <span data-ttu-id="65311-143">F0</span><span class="sxs-lookup"><span data-stu-id="65311-143">F0</span></span> | <span data-ttu-id="65311-144">Első lépésként legalacsonyabb.</span><span class="sxs-lookup"><span data-stu-id="65311-144">Start with the lowest tier.</span></span> <span data-ttu-id="65311-145">Ha elfogy a hívásokat, méretezhető, magasabb szintű használható.</span><span class="sxs-lookup"><span data-stu-id="65311-145">If you run out of calls, scale to a higher tier.</span></span>|
    | <span data-ttu-id="65311-146">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="65311-146">**Resource group**</span></span> | <span data-ttu-id="65311-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="65311-147">myResourceGroup</span></span> | <span data-ttu-id="65311-148">Ebben az oktatóanyagban minden szolgáltatáshoz használja ugyanazt az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="65311-148">Use the same resource group for all services in this tutorial.</span></span>|

4. <span data-ttu-id="65311-149">Kattintson a **létrehozása** a fiók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="65311-149">Click **Create** to create your account.</span></span> <span data-ttu-id="65311-150">A fiók létrehozása után kattintson az új kognitív szolgáltatások fiók rögzítve az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="65311-150">After the account is created, click your new Cognitive Services account pinned to the dashboard.</span></span> 

5. <span data-ttu-id="65311-151">Kattintson a fiók **kulcsok**, majd másolja az értékének **kulcs 1** , és mentse.</span><span class="sxs-lookup"><span data-stu-id="65311-151">In the account, click **Keys**, and then copy the value of **Key 1** and save it.</span></span> <span data-ttu-id="65311-152">Ez a kulcs segítségével csatlakozzon a logikai alkalmazást a kognitív Services-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="65311-152">You use this key to connect the logic app to your Cognitive Services account.</span></span> 
 
    ![Kulcsok](media/functions-twitter-email/keys.png)

## <a name="create-the-function"></a><span data-ttu-id="65311-154">A függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="65311-154">Create the function</span></span>

<span data-ttu-id="65311-155">A Functions kiváló módja annak, kiszervezheti a logic apps munkafolyamat feldolgozási feladatokat.</span><span class="sxs-lookup"><span data-stu-id="65311-155">Functions provides a great way to offload processing tasks in a logic apps workflow.</span></span> <span data-ttu-id="65311-156">Ez az oktatóanyag feldolgozása tweetet véleményeket pontszámok kognitív szolgáltatásokból és kategória érték visszaadása egy indított HTTP függvény segítségével.</span><span class="sxs-lookup"><span data-stu-id="65311-156">This tutorial uses an HTTP triggered function to process tweet sentiment scores from Cognitive Services and return a category value.</span></span>  

1. <span data-ttu-id="65311-157">Bontsa ki a függvény app, kattintson a  **+**  gombra **funkciók**, kattintson a **HTTPTrigger** sablont.</span><span class="sxs-lookup"><span data-stu-id="65311-157">Expand your function app, click the **+** button next to **Functions**, click the **HTTPTrigger** template.</span></span> <span data-ttu-id="65311-158">Típus `CategorizeSentiment` a függvény **neve** kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="65311-158">Type `CategorizeSentiment` for the function **Name** and click **Create**.</span></span>

    ![Függvény alkalmazások panelről, Funkciók +](media/functions-twitter-email/add_fun.png)

2. <span data-ttu-id="65311-160">Cserélje le a run.csx fájl tartalmát a következő kódra, majd kattintson az **mentése**:</span><span class="sxs-lookup"><span data-stu-id="65311-160">Replace the contents of the run.csx file with the following code, then click **Save**:</span></span>

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // The sentiment category defaults to 'GREEN'. 
        string category = "GREEN";
    
        // Get the sentiment score from the request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("The sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set the category based on the sentiment score.
        if (score < .3)
        {
            category = "RED";
        }
        else if (score < .6)
        {
            category = "YELLOW";
        }
        return req.CreateResponse(HttpStatusCode.OK, category);
    }
    ```
    <span data-ttu-id="65311-161">Ez a függvény kód alapján a kérésben a céggel kapcsolatos véleményeket pontszám szín kategória adja vissza.</span><span class="sxs-lookup"><span data-stu-id="65311-161">This function code returns a color category based on the sentiment score received in the request.</span></span> 

3. <span data-ttu-id="65311-162">A függvény teszteléséhez kattintson **tesztelése** bontsa ki a teszt lap jobb szélén. Írjon be egy értéket a `0.2` a a **Request body**, és kattintson a **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="65311-162">To test the function, click **Test** at the far right to expand the Test tab. Type a value of `0.2` for the **Request body**, and then click **Run**.</span></span> <span data-ttu-id="65311-163">Érték **piros** a válasz törzsében ad vissza.</span><span class="sxs-lookup"><span data-stu-id="65311-163">A value of **RED** is returned in the body of the response.</span></span> 

    ![A függvény tesztelése az Azure-portálon](./media/functions-twitter-email/test.png)

<span data-ttu-id="65311-165">Most is a céggel kapcsolatos véleményeket pontszámok Kategorizáló működnek.</span><span class="sxs-lookup"><span data-stu-id="65311-165">Now you have a function that categorizes sentiment scores.</span></span> <span data-ttu-id="65311-166">Ezután hozzon létre egy logikai alkalmazás, amely a függvény a Twitter és kognitív szolgáltatások fiókokhoz.</span><span class="sxs-lookup"><span data-stu-id="65311-166">Next, you create a logic app that integrates your function with your Twitter and Cognitive Services accounts.</span></span> 

## <a name="create-a-logic-app"></a><span data-ttu-id="65311-167">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="65311-167">Create a logic app</span></span>   

1. <span data-ttu-id="65311-168">Az Azure portálon kattintson a **új** gomb az Azure portál bal felső sarkában található.</span><span class="sxs-lookup"><span data-stu-id="65311-168">In the Azure portal, click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

2. <span data-ttu-id="65311-169">Kattintson a **vállalati integrációs** > **logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="65311-169">Click **Enterprise Integration** > **Logic App**.</span></span> <span data-ttu-id="65311-170">Ezután használja a beállítások a tábla, ellenőrizze a **rögzítés az irányítópulton**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="65311-170">Then, use the settings as specified in the table, check **Pin to dashboard**, and click **Create**.</span></span>
 
4. <span data-ttu-id="65311-171">Írja be a **neve** például `TweetSentiment`, a táblázatban megadott beállítások használatát, fogadja el a feltételeket, és ellenőrizze **rögzítés az irányítópulton**.</span><span class="sxs-lookup"><span data-stu-id="65311-171">Then, type a **Name** like `TweetSentiment`,  use the settings as specified in the table, accept the terms, and check **Pin to dashboard**.</span></span>

    ![Logikai alkalmazás létrehozása az Azure-portálon](./media/functions-twitter-email/new_logicApp.png)

    | <span data-ttu-id="65311-173">Beállítás</span><span class="sxs-lookup"><span data-stu-id="65311-173">Setting</span></span>      |  <span data-ttu-id="65311-174">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="65311-174">Suggested value</span></span>   | <span data-ttu-id="65311-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="65311-175">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="65311-176">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="65311-176">**Name**</span></span> | <span data-ttu-id="65311-177">TweetSentiment</span><span class="sxs-lookup"><span data-stu-id="65311-177">TweetSentiment</span></span> | <span data-ttu-id="65311-178">Válasszon egy megfelelő az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="65311-178">Choose an appropriate name for your app.</span></span> |
    | <span data-ttu-id="65311-179">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="65311-179">**Resource group**</span></span> | <span data-ttu-id="65311-180">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="65311-180">myResourceGroup</span></span> | <span data-ttu-id="65311-181">Szöveg elemzésére használt API.</span><span class="sxs-lookup"><span data-stu-id="65311-181">API used to analyze text.</span></span>  |
    | <span data-ttu-id="65311-182">**Hely**</span><span class="sxs-lookup"><span data-stu-id="65311-182">**Location**</span></span> | <span data-ttu-id="65311-183">USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="65311-183">East US</span></span> | <span data-ttu-id="65311-184">Válasszon az Önhöz legközelebb eső helyet.</span><span class="sxs-lookup"><span data-stu-id="65311-184">Choose a location close to you.</span></span> |
    | <span data-ttu-id="65311-185">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="65311-185">**Resource group**</span></span> | <span data-ttu-id="65311-186">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="65311-186">myResourceGroup</span></span> | <span data-ttu-id="65311-187">Válassza a meglévő erőforráscsoportot, előtt.</span><span class="sxs-lookup"><span data-stu-id="65311-187">Choose the same existing resource group as before.</span></span>|

4. <span data-ttu-id="65311-188">Kattintson a **létrehozása** a logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="65311-188">Click **Create** to create your logic app.</span></span> <span data-ttu-id="65311-189">Az alkalmazás létrehozása után kattintson az új logikai alkalmazás rögzítve az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="65311-189">After the app is created, click your new logic app pinned to the dashboard.</span></span> <span data-ttu-id="65311-190">Ezután a Logic Apps tervezőben, görgessen lefelé, és kattintson a **üres logikai alkalmazás** sablont.</span><span class="sxs-lookup"><span data-stu-id="65311-190">Then in the Logic Apps Designer, scroll down and click the **Blank Logic App** template.</span></span> 

    ![Üres Logic Apps-sablon](media/functions-twitter-email/blank.png)

<span data-ttu-id="65311-192">A Logic Apps Designer segítségével mostantól szolgáltatások és eseményindítók hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="65311-192">You can now use the Logic Apps Designer to add services and triggers to your app.</span></span>

## <a name="connect-to-twitter"></a><span data-ttu-id="65311-193">Kapcsolódás a Twitteren</span><span class="sxs-lookup"><span data-stu-id="65311-193">Connect to Twitter</span></span>

<span data-ttu-id="65311-194">Először hozzon létre kapcsolatot a Twitter-fiók.</span><span class="sxs-lookup"><span data-stu-id="65311-194">First, create a connection to your Twitter account.</span></span> <span data-ttu-id="65311-195">A logikai alkalmazás lekérdezi az Twitter-üzeneteket, amelyek indul el, az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="65311-195">The logic app polls for tweets, which trigger the app to run.</span></span>

1. <span data-ttu-id="65311-196">A tervezőben, kattintson a **Twitter** szolgáltatásra, és kattintson a **amikor egy új tweetet visszaküldi** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="65311-196">In the designer, click the **Twitter** service, and click the **When a new tweet is posted** trigger.</span></span> <span data-ttu-id="65311-197">Jelentkezzen be a Twitter-fiók, és engedélyezik a Logic Apps segítségével használhatja a fiókot.</span><span class="sxs-lookup"><span data-stu-id="65311-197">Sign in to your Twitter account and authorize Logic Apps to use your account.</span></span>

2. <span data-ttu-id="65311-198">A táblázatban megadott Twitter eseményindító beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="65311-198">Use the Twitter trigger settings as specified in the table.</span></span> 

    ![Twitter-összekötő beállításai](media/functions-twitter-email/azure_tweet.png)

    | <span data-ttu-id="65311-200">Beállítás</span><span class="sxs-lookup"><span data-stu-id="65311-200">Setting</span></span>      |  <span data-ttu-id="65311-201">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="65311-201">Suggested value</span></span>   | <span data-ttu-id="65311-202">Leírás</span><span class="sxs-lookup"><span data-stu-id="65311-202">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="65311-203">**Keresett szöveg**</span><span class="sxs-lookup"><span data-stu-id="65311-203">**Search text**</span></span> | <span data-ttu-id="65311-204">#Azure</span><span class="sxs-lookup"><span data-stu-id="65311-204">#Azure</span></span> | <span data-ttu-id="65311-205">Használja a hashtaggel történő, amely elegendő népszerű, az új Twitter-üzeneteket a választott időszakban létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="65311-205">Use a hashtag that is popular enough for to generate new tweets in the chosen interval.</span></span> <span data-ttu-id="65311-206">Ingyenes szint és a hashtaggel történő használata esetén túl népszerű gyorsan használhatja fel a tranzakciók a kognitív Services-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="65311-206">When using the Free tier and your hashtag is too popular, you can quickly use up the transactions in your Cognitive Services account.</span></span> |
    | <span data-ttu-id="65311-207">**Gyakoriság**</span><span class="sxs-lookup"><span data-stu-id="65311-207">**Frequency**</span></span> | <span data-ttu-id="65311-208">Perc</span><span class="sxs-lookup"><span data-stu-id="65311-208">Minute</span></span> | <span data-ttu-id="65311-209">A használt Twitter a lekérdezés gyakoriságát egység.</span><span class="sxs-lookup"><span data-stu-id="65311-209">The frequency unit used for polling Twitter.</span></span>  |
    | <span data-ttu-id="65311-210">**Időköz**</span><span class="sxs-lookup"><span data-stu-id="65311-210">**Interval**</span></span> | <span data-ttu-id="65311-211">15</span><span class="sxs-lookup"><span data-stu-id="65311-211">15</span></span> | <span data-ttu-id="65311-212">Twitter-kérelmek gyakorisága egységekben között eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="65311-212">The time elapsed between Twitter requests, in frequency units.</span></span> |

3.  <span data-ttu-id="65311-213">Kattintson a **mentése** csatlakozni a Twitter-fiók.</span><span class="sxs-lookup"><span data-stu-id="65311-213">Click **Save** to connect to your Twitter account.</span></span> 

<span data-ttu-id="65311-214">Az alkalmazás most Twitter kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="65311-214">Now your app is connected to Twitter.</span></span> <span data-ttu-id="65311-215">Ezután a céggel kapcsolatos véleményeket az összegyűjtött Twitter-üzenetek észlelése szövegelemzések csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="65311-215">Next, you connect to text analytics to detect the sentiment of collected tweets.</span></span>

## <a name="add-sentiment-detection"></a><span data-ttu-id="65311-216">Adja hozzá a céggel kapcsolatos véleményeket észlelése</span><span class="sxs-lookup"><span data-stu-id="65311-216">Add sentiment detection</span></span>

1. <span data-ttu-id="65311-217">Kattintson a **új lépés**, majd **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="65311-217">Click **New Step**, and then **Add an action**.</span></span>

    ![Új lépéssel, majd a művelet hozzáadása](media/functions-twitter-email/new_step.png)

2. <span data-ttu-id="65311-219">A **művelet kiválasztását**, kattintson a **Szövegelemzések**, majd kattintson a **észleli a céggel kapcsolatos véleményeket** művelet.</span><span class="sxs-lookup"><span data-stu-id="65311-219">In **Choose an action**, click **Text Analytics**, and then click the **Detect sentiment** action.</span></span>

    ![Véleményeket észlelése](media/functions-twitter-email/detect_sent.png)

3. <span data-ttu-id="65311-221">Írja be a kapcsolat neve, mint `MyCognitiveServicesConnection`, illessze be a kulcsot, a mentett a kognitív szolgáltatások fiókra, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="65311-221">Type a connection name such as `MyCognitiveServicesConnection`, paste the key for your Cognitive Services account that you saved, and click **Create**.</span></span>  

4. <span data-ttu-id="65311-222">Kattintson a **szöveg elemzésére** > **Tweetet szöveg**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="65311-222">Click **Text to analyze** > **Tweet text**, and then click **Save**.</span></span>  

    ![Véleményeket észlelése](media/functions-twitter-email/ds_tta.png)

<span data-ttu-id="65311-224">Most, hogy a céggel kapcsolatos véleményeket észlelési van konfigurálva, a függvény, amely a céggel kapcsolatos véleményeket pontszám kimeneti akkor is hozzáadhat egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="65311-224">Now that sentiment detection is configured, you can add a connection to your function that consumes the sentiment score output.</span></span>

## <a name="connect-sentiment-output-to-your-function"></a><span data-ttu-id="65311-225">A függvény véleményeket kimeneti kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="65311-225">Connect sentiment output to your function</span></span>

1. <span data-ttu-id="65311-226">A Logic Apps tervezőben kattintson **új lépés** > **művelet hozzáadása**, és kattintson a **Azure Functions**.</span><span class="sxs-lookup"><span data-stu-id="65311-226">In the Logic Apps Designer, click **New step** > **Add an action**, and then click **Azure Functions**.</span></span> 

2. <span data-ttu-id="65311-227">Kattintson a **jelöljön ki egy Azure függvényt**, jelölje be a **CategorizeSentiment** korábban létrehozott függvény.</span><span class="sxs-lookup"><span data-stu-id="65311-227">Click **Choose an Azure function**, select the **CategorizeSentiment** function you created earlier.</span></span>  

    ![Válasszon egy Azure függvény megjelenítő Azure függvény mezőben](media/functions-twitter-email/choose_fun.png)

3. <span data-ttu-id="65311-229">A **kérelem törzse**, kattintson a **pontszám** , majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="65311-229">In **Request Body**, click **Score** and then **Save**.</span></span>

    ![Pontszám](media/functions-twitter-email/trigger_score.png)

<span data-ttu-id="65311-231">Most a függvény lesz kiváltva, ha a céggel kapcsolatos véleményeket pontszám küldi a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="65311-231">Now, your function is triggered when a sentiment score is sent from the logic app.</span></span> <span data-ttu-id="65311-232">Itt a színkódolás kategóriát a függvény által visszaadott a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="65311-232">A color-coded category is returned to the logic app by the function.</span></span> <span data-ttu-id="65311-233">Ezután adja hozzá az e-mailben értesítést küldi, ha a céggel kapcsolatos véleményeket érték **piros** küld vissza a függvény.</span><span class="sxs-lookup"><span data-stu-id="65311-233">Next, you add an email notification that is sent when a sentiment value of **RED** is returned from the function.</span></span> 

## <a name="add-email-notifications"></a><span data-ttu-id="65311-234">E-mail értesítések hozzáadása</span><span class="sxs-lookup"><span data-stu-id="65311-234">Add email notifications</span></span>

<span data-ttu-id="65311-235">A munkafolyamat utolsó része indul el egy e-mailt, ha a céggel kapcsolatos véleményeket program pontozza a mennyiségeket _piros_.</span><span class="sxs-lookup"><span data-stu-id="65311-235">The last part of the workflow is to trigger an email when the sentiment is scored as _RED_.</span></span> <span data-ttu-id="65311-236">Ez a témakör egy Outlook.com-összekötőt használja.</span><span class="sxs-lookup"><span data-stu-id="65311-236">This topic uses an Outlook.com connector.</span></span> <span data-ttu-id="65311-237">A Gmailes vagy Office 365 Outlook-összekötő használatához hasonló lépésekkel végezheti el.</span><span class="sxs-lookup"><span data-stu-id="65311-237">You can perform similar steps to use a Gmail or Office 365 Outlook connector.</span></span>   

1. <span data-ttu-id="65311-238">A Logic Apps tervezőben kattintson **új lépés** > **feltétel hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="65311-238">In the Logic Apps Designer, click **New step** > **Add a condition**.</span></span> 

2. <span data-ttu-id="65311-239">Kattintson a **adjon meg értéket**, majd kattintson a **törzs**.</span><span class="sxs-lookup"><span data-stu-id="65311-239">Click **Choose a value**, then click **Body**.</span></span> <span data-ttu-id="65311-240">Válassza ki **egyenlő**, kattintson a **adjon meg értéket** és írja be `RED`, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="65311-240">Select **is equal to**, click **Choose a value** and type `RED`, and click **Save**.</span></span> 

    ![Egy feltétel hozzáadása a logikai alkalmazást.](media/functions-twitter-email/condition.png)

3. <span data-ttu-id="65311-242">A **Ha igen, nincs**, kattintson **művelet hozzáadása**, keresse meg `outlook.com`, kattintson a **egy e-mailek küldése**, és jelentkezzen be az Outlook.com-os fiókjába.</span><span class="sxs-lookup"><span data-stu-id="65311-242">In **IF YES, DO NOTHING**, click **Add an action**, search for `outlook.com`, click **Send an email**, and sign in to your Outlook.com account.</span></span>
    
    ![A feltételhez művelet kiválasztását.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > <span data-ttu-id="65311-244">Ha egy Outlook.com-fiók nem rendelkezik, választhat egy másik összekötő, például a Gmailes vagy Office 365 Outlook</span><span class="sxs-lookup"><span data-stu-id="65311-244">If you don't have an Outlook.com account, you can choose another connector, such as Gmail or Office 365 Outlook</span></span>

4. <span data-ttu-id="65311-245">Az a **egy e-mailek küldése** művelet, a megadott, az e-mail beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="65311-245">In the **Send an email** action, use the email settings as specified in the table.</span></span> 

    ![Konfigurálhatja az e-mailt az e-mailek művelet küldéskor.](media/functions-twitter-email/sendemail.png)

    | <span data-ttu-id="65311-247">Beállítás</span><span class="sxs-lookup"><span data-stu-id="65311-247">Setting</span></span>      |  <span data-ttu-id="65311-248">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="65311-248">Suggested value</span></span>   | <span data-ttu-id="65311-249">Leírás</span><span class="sxs-lookup"><span data-stu-id="65311-249">Description</span></span>  |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="65311-250">**A**</span><span class="sxs-lookup"><span data-stu-id="65311-250">**To**</span></span> | <span data-ttu-id="65311-251">Az e-mail címet</span><span class="sxs-lookup"><span data-stu-id="65311-251">Type your email address</span></span> | <span data-ttu-id="65311-252">Az e-mail címet, amely a értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="65311-252">The email address that receives the notification.</span></span> |
    | <span data-ttu-id="65311-253">**Tulajdonos**</span><span class="sxs-lookup"><span data-stu-id="65311-253">**Subject**</span></span> | <span data-ttu-id="65311-254">Negatív tweetet céggel kapcsolatos véleményeket észlelt</span><span class="sxs-lookup"><span data-stu-id="65311-254">Negative tweet sentiment detected</span></span>  | <span data-ttu-id="65311-255">Az e-mail értesítés tárgyát.</span><span class="sxs-lookup"><span data-stu-id="65311-255">The subject line of the email notification.</span></span>  |
    | <span data-ttu-id="65311-256">**Törzs**</span><span class="sxs-lookup"><span data-stu-id="65311-256">**Body**</span></span> | <span data-ttu-id="65311-257">Tweetet szöveg, amelyet helyre</span><span class="sxs-lookup"><span data-stu-id="65311-257">Tweet text, Location</span></span> | <span data-ttu-id="65311-258">Kattintson a **Tweetet szöveg** és **hely** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="65311-258">Click the **Tweet text** and **Location** parameters.</span></span> |

5.  <span data-ttu-id="65311-259">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="65311-259">Click **Save**.</span></span>

<span data-ttu-id="65311-260">Most, hogy a munkafolyamat befejeződött, a logikai alkalmazás engedélyezése, és tekintse meg a munkahelyi függvény.</span><span class="sxs-lookup"><span data-stu-id="65311-260">Now that the workflow is complete, you can enable the logic app and see the function at work.</span></span>

## <a name="test-the-workflow"></a><span data-ttu-id="65311-261">A munkafolyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="65311-261">Test the workflow</span></span>

1. <span data-ttu-id="65311-262">A Logic App tervezőben kattintson **futtatása** az alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="65311-262">In the Logic App Designer, click **Run** to start the app.</span></span>

2. <span data-ttu-id="65311-263">A bal oldali oszlopban kattintson **áttekintése** a logikai alkalmazás állapotának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="65311-263">In the left column, click **Overview** to see the status of the logic app.</span></span> 
 
    ![Logic app végrehajtási állapota](media/functions-twitter-email/over1.png)

3. <span data-ttu-id="65311-265">(Választható) Kattintson a fut, a végrehajtás részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="65311-265">(Optional) Click one of the runs to see details of the execution.</span></span>

4. <span data-ttu-id="65311-266">Nyissa meg a függvény, a naplók megtekintéséhez, és győződjön meg arról, hogy a céggel kapcsolatos véleményeket értéket is fogad és dolgoz.</span><span class="sxs-lookup"><span data-stu-id="65311-266">Go to your function, view the logs, and verify that sentiment values were received and processed.</span></span>
 
    ![Függvény naplók megtekintése](media/functions-twitter-email/sent.png)

5. <span data-ttu-id="65311-268">A potenciálisan negatív véleményeket észlelésekor e-mailt kapni.</span><span class="sxs-lookup"><span data-stu-id="65311-268">When a potentially negative sentiment is detected, you receive an email.</span></span> <span data-ttu-id="65311-269">Ha még nem kapott e-mailt, módosíthatja a funkciókódot piros vissza minden alkalommal, amikor:</span><span class="sxs-lookup"><span data-stu-id="65311-269">If you haven't received an email, you can change the function code to return RED every time:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    <span data-ttu-id="65311-270">Miután ellenőrizte, hogy e-mail értesítést, állítsa vissza az eredeti kódot:</span><span class="sxs-lookup"><span data-stu-id="65311-270">After you have verified email notifications, change back to the original code:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > <span data-ttu-id="65311-271">Ez az oktatóanyag befejezése után tiltsa le a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="65311-271">After you have completed this tutorial, you should disable the logic app.</span></span> <span data-ttu-id="65311-272">Ha letiltja az alkalmazást, elkerülheti a megterhelni a végrehajtások használt és a tranzakciók a kognitív Services-fiók mentése folyamatban.</span><span class="sxs-lookup"><span data-stu-id="65311-272">By disabling the app, you avoid being charged for executions and using up the transactions in your Cognitive Services account.</span></span>

<span data-ttu-id="65311-273">Most láthatta, milyen egyszerűen funkciók integrálja a Logic Apps munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="65311-273">Now you have seen how easy it is to integrate Functions into a Logic Apps workflow.</span></span>

## <a name="disable-the-logic-app"></a><span data-ttu-id="65311-274">A logikai alkalmazás letiltása</span><span class="sxs-lookup"><span data-stu-id="65311-274">Disable the logic app</span></span>

<span data-ttu-id="65311-275">A logikai alkalmazás letiltásához kattintson **áttekintése** majd **letiltása** a képernyő tetején.</span><span class="sxs-lookup"><span data-stu-id="65311-275">To disable the logic app, click **Overview** and then click **Disable** at the top of the screen.</span></span> <span data-ttu-id="65311-276">Ezzel leállítja a logikai alkalmazás fut, és ezzel járó költségek nélkül az alkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="65311-276">This stops the logic app from running and incurring charges without deleting the app.</span></span> 

![Függvény naplók](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a><span data-ttu-id="65311-278">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65311-278">Next steps</span></span>

<span data-ttu-id="65311-279">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="65311-279">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="65311-280">Kognitív Services-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="65311-280">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="65311-281">Hozzon létre egy függvényt, Kategorizáló tweetet céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="65311-281">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="65311-282">Twitter csatlakozó logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="65311-282">Create a logic app that connects to Twitter.</span></span>
> * <span data-ttu-id="65311-283">Véleményeket észlelési hozzáadása a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="65311-283">Add sentiment detection to the logic app.</span></span> 
> * <span data-ttu-id="65311-284">Csatlakoztassa a logikai alkalmazást a függvény.</span><span class="sxs-lookup"><span data-stu-id="65311-284">Connect the logic app to the function.</span></span>
> * <span data-ttu-id="65311-285">Küldjön egy e-mailt a választ, a függvény alapján.</span><span class="sxs-lookup"><span data-stu-id="65311-285">Send an email based on the response from the function.</span></span>

<span data-ttu-id="65311-286">Előzetes hogyan hozzon létre egy kiszolgáló nélküli API-t a függvény a következő oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="65311-286">Advance to the next tutorial to learn how to create a serverless API for your function.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="65311-287">Kiszolgáló nélküli API létrehozása az Azure Functions használatával</span><span class="sxs-lookup"><span data-stu-id="65311-287">Create a serverless API using Azure Functions</span></span>](functions-create-serverless-api.md)

<span data-ttu-id="65311-288">A Logic Apps kapcsolatos további információkért lásd: [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="65311-288">To learn more about Logic Apps, see [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).</span></span>


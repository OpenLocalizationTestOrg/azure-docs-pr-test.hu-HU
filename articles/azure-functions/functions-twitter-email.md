---
title: "egy függvény, amely az Azure Logic Apps aaaCreate |} Microsoft Docs"
description: "Hozzon létre egy függvényt, amely az Azure Logic Apps és az Azure kognitív szolgáltatások toocategorize tweetet véleményeket, és értesítést küld, ha véleményeket gyenge."
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
ms.openlocfilehash: e176bd946af9a3684b3ad0e4b1bed1c3ee344019
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a><span data-ttu-id="935a1-104">Hozzon létre egy függvényt, amely az Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="935a1-104">Create a function that integrates with Azure Logic Apps</span></span>

<span data-ttu-id="935a1-105">Az Azure Functions integrálja az Azure Logic Apps a Logic Apps Designer hello.</span><span class="sxs-lookup"><span data-stu-id="935a1-105">Azure Functions integrates with Azure Logic Apps in hello Logic Apps Designer.</span></span> <span data-ttu-id="935a1-106">Ez az integráció lehetővé teszi az informatika álló üzenettípusok összehangolását más Azure és a külső függvények power hello használatát.</span><span class="sxs-lookup"><span data-stu-id="935a1-106">This integration lets you use hello computing power of Functions in orchestrations with other Azure and third-party services.</span></span> 

<span data-ttu-id="935a1-107">Az oktatóanyag bemutatja, hogyan toouse működik, a Twitter-bejegyzéseket a Logic Apps és az Azure kognitív szolgáltatások tooanalyze véleményeket.</span><span class="sxs-lookup"><span data-stu-id="935a1-107">This tutorial shows you how toouse Functions with Logic Apps and Azure Cognitive Services tooanalyze sentiment from Twitter posts.</span></span> <span data-ttu-id="935a1-108">Az indított HTTP függvény kategorizálja Twitter-üzeneteket, mint a zöld, sárga vagy piros hello véleményeket pontszám alapján.</span><span class="sxs-lookup"><span data-stu-id="935a1-108">An HTTP triggered function categorizes tweets as green, yellow, or red based on hello sentiment score.</span></span> <span data-ttu-id="935a1-109">Az e-mail elküldésekor történik a gyenge véleményeket észlelése esetén.</span><span class="sxs-lookup"><span data-stu-id="935a1-109">An email is sent when poor sentiment is detected.</span></span> 

![kép két lépést a Logic App Designer alkalmazás](media/functions-twitter-email/designer1.png)

<span data-ttu-id="935a1-111">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="935a1-111">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="935a1-112">Kognitív Services-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="935a1-112">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="935a1-113">Hozzon létre egy függvényt, Kategorizáló tweetet céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="935a1-113">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="935a1-114">TooTwitter csatlakozó logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="935a1-114">Create a logic app that connects tooTwitter.</span></span>
> * <span data-ttu-id="935a1-115">Adja hozzá a céggel kapcsolatos véleményeket észlelési toohello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="935a1-115">Add sentiment detection toohello logic app.</span></span> 
> * <span data-ttu-id="935a1-116">Csatlakozás hello logic app toohello függvény.</span><span class="sxs-lookup"><span data-stu-id="935a1-116">Connect hello logic app toohello function.</span></span>
> * <span data-ttu-id="935a1-117">Küldjön egy e-mailt hello válaszát hello függvény alapján.</span><span class="sxs-lookup"><span data-stu-id="935a1-117">Send an email based on hello response from hello function.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="935a1-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="935a1-118">Prerequisites</span></span>

+ <span data-ttu-id="935a1-119">Az aktív [Twitter](https://twitter.com/) fiók.</span><span class="sxs-lookup"><span data-stu-id="935a1-119">An active [Twitter](https://twitter.com/) account.</span></span> 
+ <span data-ttu-id="935a1-120">Egy [Outlook.com-os](https://outlook.com/) fiókot (a értesítések küldése).</span><span class="sxs-lookup"><span data-stu-id="935a1-120">An [Outlook.com](https://outlook.com/) account (for sending notifications).</span></span>
+ <span data-ttu-id="935a1-121">Ez a témakör használja, mint a kiindulási pont hello erőforrások létrehozott [hello Azure-portálon az első függvényét létrehozása](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="935a1-121">This topic uses as its starting point hello resources created in [Create your first function from hello Azure portal](functions-create-first-azure-function.md).</span></span>  
<span data-ttu-id="935a1-122">Ha még nem tette meg, végezze el ezeket a lépéseket most toocreate a függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="935a1-122">If you haven't already done so, complete these steps now toocreate your function app.</span></span>

## <a name="create-a-cognitive-services-account"></a><span data-ttu-id="935a1-123">Kognitív Services-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="935a1-123">Create a Cognitive Services account</span></span>

<span data-ttu-id="935a1-124">Egy kognitív Services-fiók szükséges toodetect hello véleményeket Twitter-üzeneteket figyeli a rendszer.</span><span class="sxs-lookup"><span data-stu-id="935a1-124">A Cognitive Services account is required toodetect hello sentiment of tweets being monitored.</span></span>

1. <span data-ttu-id="935a1-125">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="935a1-125">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="935a1-126">Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.</span><span class="sxs-lookup"><span data-stu-id="935a1-126">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

3. <span data-ttu-id="935a1-127">Kattintson a **adatok + analitika** > **kognitív szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="935a1-127">Click **Data + Analytics** > **Cognitive  Services**.</span></span> <span data-ttu-id="935a1-128">Majd, mint hello-beállítások használata hello táblázatban megadott hello fogadnia, és ellenőrizze **PIN-kód toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="935a1-128">Then, use hello settings as specified in hello table, accept hello terms, and check **Pin toodashboard**.</span></span>

    ![Hozzon létre kognitív fiók panelen](media/functions-twitter-email/cog_svcs_account.png)

    | <span data-ttu-id="935a1-130">Beállítás</span><span class="sxs-lookup"><span data-stu-id="935a1-130">Setting</span></span>      |  <span data-ttu-id="935a1-131">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="935a1-131">Suggested value</span></span>   | <span data-ttu-id="935a1-132">Leírás</span><span class="sxs-lookup"><span data-stu-id="935a1-132">Description</span></span>                                        |
    | --- | --- | --- |
    | <span data-ttu-id="935a1-133">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="935a1-133">**Name**</span></span> | <span data-ttu-id="935a1-134">MyCognitiveServicesAccnt</span><span class="sxs-lookup"><span data-stu-id="935a1-134">MyCognitiveServicesAccnt</span></span> | <span data-ttu-id="935a1-135">Válasszon egy egyedi fióknevet.</span><span class="sxs-lookup"><span data-stu-id="935a1-135">Choose a unique account name.</span></span> |
    | <span data-ttu-id="935a1-136">**API-típus**</span><span class="sxs-lookup"><span data-stu-id="935a1-136">**API type**</span></span> | <span data-ttu-id="935a1-137">Szövegelemzések API</span><span class="sxs-lookup"><span data-stu-id="935a1-137">Text Analytics API</span></span> | <span data-ttu-id="935a1-138">API használt tooanalyze szöveg.</span><span class="sxs-lookup"><span data-stu-id="935a1-138">API used tooanalyze text.</span></span>  |
    | <span data-ttu-id="935a1-139">**Hely**</span><span class="sxs-lookup"><span data-stu-id="935a1-139">**Location**</span></span> | <span data-ttu-id="935a1-140">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="935a1-140">West US</span></span> | <span data-ttu-id="935a1-141">Jelenleg csak **USA nyugati régiója** szövegelemzések érhető el.</span><span class="sxs-lookup"><span data-stu-id="935a1-141">Currently, only **West US** is available for text analytics.</span></span> |
    | <span data-ttu-id="935a1-142">**Tarifacsomag**</span><span class="sxs-lookup"><span data-stu-id="935a1-142">**Pricing tier**</span></span> | <span data-ttu-id="935a1-143">F0</span><span class="sxs-lookup"><span data-stu-id="935a1-143">F0</span></span> | <span data-ttu-id="935a1-144">Első lépésként hello legalacsonyabb szint.</span><span class="sxs-lookup"><span data-stu-id="935a1-144">Start with hello lowest tier.</span></span> <span data-ttu-id="935a1-145">Ha elfogy a hívásokat, méretezhető tooa magasabb szintű használható.</span><span class="sxs-lookup"><span data-stu-id="935a1-145">If you run out of calls, scale tooa higher tier.</span></span>|
    | <span data-ttu-id="935a1-146">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="935a1-146">**Resource group**</span></span> | <span data-ttu-id="935a1-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="935a1-147">myResourceGroup</span></span> | <span data-ttu-id="935a1-148">Használja az azonos hello erőforráscsoport ebben az oktatóanyagban minden szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="935a1-148">Use hello same resource group for all services in this tutorial.</span></span>|

4. <span data-ttu-id="935a1-149">Kattintson a **létrehozása** toocreate fiókját.</span><span class="sxs-lookup"><span data-stu-id="935a1-149">Click **Create** toocreate your account.</span></span> <span data-ttu-id="935a1-150">Hello-fiók létrehozása után kattintson az új kognitív szolgáltatások fiók rögzített toohello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="935a1-150">After hello account is created, click your new Cognitive Services account pinned toohello dashboard.</span></span> 

5. <span data-ttu-id="935a1-151">Hello fiókot, kattintson **kulcsok**, majd másolja az hello értékének **kulcs 1** és mentse azt.</span><span class="sxs-lookup"><span data-stu-id="935a1-151">In hello account, click **Keys**, and then copy hello value of **Key 1** and save it.</span></span> <span data-ttu-id="935a1-152">A kulcs tooconnect hello logic app tooyour kognitív Services-fiók használ.</span><span class="sxs-lookup"><span data-stu-id="935a1-152">You use this key tooconnect hello logic app tooyour Cognitive Services account.</span></span> 
 
    ![Kulcsok](media/functions-twitter-email/keys.png)

## <a name="create-hello-function"></a><span data-ttu-id="935a1-154">Hello függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="935a1-154">Create hello function</span></span>

<span data-ttu-id="935a1-155">A Functions egy kiváló módja toooffload a logic apps munkafolyamat feladatokat.</span><span class="sxs-lookup"><span data-stu-id="935a1-155">Functions provides a great way toooffload processing tasks in a logic apps workflow.</span></span> <span data-ttu-id="935a1-156">Ez az oktatóanyag egy indított HTTP függvény tooprocess tweetet véleményeket pontszámok kognitív szolgáltatások és a visszatérési kategória értéket használja.</span><span class="sxs-lookup"><span data-stu-id="935a1-156">This tutorial uses an HTTP triggered function tooprocess tweet sentiment scores from Cognitive Services and return a category value.</span></span>  

1. <span data-ttu-id="935a1-157">Bontsa ki a függvény app, kattintson a hello  **+**  gomb melletti túl**funkciók**, kattintson a hello **HTTPTrigger** sablont.</span><span class="sxs-lookup"><span data-stu-id="935a1-157">Expand your function app, click hello **+** button next too**Functions**, click hello **HTTPTrigger** template.</span></span> <span data-ttu-id="935a1-158">Típus `CategorizeSentiment` hello függvény **neve** kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="935a1-158">Type `CategorizeSentiment` for hello function **Name** and click **Create**.</span></span>

    ![Függvény alkalmazások panelről, Funkciók +](media/functions-twitter-email/add_fun.png)

2. <span data-ttu-id="935a1-160">Cserélje le a következő kód hello hello hello run.csx fájl tartalmát, majd kattintson az **mentése**:</span><span class="sxs-lookup"><span data-stu-id="935a1-160">Replace hello contents of hello run.csx file with hello following code, then click **Save**:</span></span>

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // hello sentiment category defaults too'GREEN'. 
        string category = "GREEN";
    
        // Get hello sentiment score from hello request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("hello sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set hello category based on hello sentiment score.
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
    <span data-ttu-id="935a1-161">Ez a függvény kód hello véleményeket pontszám hello kérés alapján szín kategória adja vissza.</span><span class="sxs-lookup"><span data-stu-id="935a1-161">This function code returns a color category based on hello sentiment score received in hello request.</span></span> 

3. <span data-ttu-id="935a1-162">tootest hello függvény, kattintson a **teszt** hello jobb szélén tooexpand hello teszt lapját. Írjon be egy értéket a `0.2` a hello **Request body**, és kattintson a **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="935a1-162">tootest hello function, click **Test** at hello far right tooexpand hello Test tab. Type a value of `0.2` for hello **Request body**, and then click **Run**.</span></span> <span data-ttu-id="935a1-163">Érték **piros** hello törzsét hello választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="935a1-163">A value of **RED** is returned in hello body of hello response.</span></span> 

    ![Teszt hello függvényt hello Azure-portálon](./media/functions-twitter-email/test.png)

<span data-ttu-id="935a1-165">Most is a céggel kapcsolatos véleményeket pontszámok Kategorizáló működnek.</span><span class="sxs-lookup"><span data-stu-id="935a1-165">Now you have a function that categorizes sentiment scores.</span></span> <span data-ttu-id="935a1-166">Ezután hozzon létre egy logikai alkalmazás, amely a függvény a Twitter és kognitív szolgáltatások fiókokhoz.</span><span class="sxs-lookup"><span data-stu-id="935a1-166">Next, you create a logic app that integrates your function with your Twitter and Cognitive Services accounts.</span></span> 

## <a name="create-a-logic-app"></a><span data-ttu-id="935a1-167">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="935a1-167">Create a logic app</span></span>   

1. <span data-ttu-id="935a1-168">A hello Azure-portálon, kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.</span><span class="sxs-lookup"><span data-stu-id="935a1-168">In hello Azure portal, click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="935a1-169">Kattintson a **vállalati integrációs** > **logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="935a1-169">Click **Enterprise Integration** > **Logic App**.</span></span> <span data-ttu-id="935a1-170">Majd, mint hello-beállítások használata hello táblázatban megadott ellenőrizze **PIN-kód toodashboard**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="935a1-170">Then, use hello settings as specified in hello table, check **Pin toodashboard**, and click **Create**.</span></span>
 
4. <span data-ttu-id="935a1-171">Írja be a **neve** például `TweetSentiment`, hello beállításokkal hello táblázatban megadottak szerint, hello fogadnia, és ellenőrizze **PIN-kód toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="935a1-171">Then, type a **Name** like `TweetSentiment`,  use hello settings as specified in hello table, accept hello terms, and check **Pin toodashboard**.</span></span>

    ![Az Azure-portálon hello logikai alkalmazás létrehozása](./media/functions-twitter-email/new_logicApp.png)

    | <span data-ttu-id="935a1-173">Beállítás</span><span class="sxs-lookup"><span data-stu-id="935a1-173">Setting</span></span>      |  <span data-ttu-id="935a1-174">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="935a1-174">Suggested value</span></span>   | <span data-ttu-id="935a1-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="935a1-175">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="935a1-176">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="935a1-176">**Name**</span></span> | <span data-ttu-id="935a1-177">TweetSentiment</span><span class="sxs-lookup"><span data-stu-id="935a1-177">TweetSentiment</span></span> | <span data-ttu-id="935a1-178">Válasszon egy megfelelő az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="935a1-178">Choose an appropriate name for your app.</span></span> |
    | <span data-ttu-id="935a1-179">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="935a1-179">**Resource group**</span></span> | <span data-ttu-id="935a1-180">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="935a1-180">myResourceGroup</span></span> | <span data-ttu-id="935a1-181">API használt tooanalyze szöveg.</span><span class="sxs-lookup"><span data-stu-id="935a1-181">API used tooanalyze text.</span></span>  |
    | <span data-ttu-id="935a1-182">**Hely**</span><span class="sxs-lookup"><span data-stu-id="935a1-182">**Location**</span></span> | <span data-ttu-id="935a1-183">USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="935a1-183">East US</span></span> | <span data-ttu-id="935a1-184">Válasszon egy helyet Bezárás tooyou.</span><span class="sxs-lookup"><span data-stu-id="935a1-184">Choose a location close tooyou.</span></span> |
    | <span data-ttu-id="935a1-185">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="935a1-185">**Resource group**</span></span> | <span data-ttu-id="935a1-186">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="935a1-186">myResourceGroup</span></span> | <span data-ttu-id="935a1-187">Válassza ki azt korábban hello azonos meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="935a1-187">Choose hello same existing resource group as before.</span></span>|

4. <span data-ttu-id="935a1-188">Kattintson a **létrehozása** toocreate a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="935a1-188">Click **Create** toocreate your logic app.</span></span> <span data-ttu-id="935a1-189">Hello alkalmazás létrehozása után kattintson az új logikai alkalmazás rögzített toohello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="935a1-189">After hello app is created, click your new logic app pinned toohello dashboard.</span></span> <span data-ttu-id="935a1-190">Majd hello Logic Apps Designer, görgessen lefelé, és kattintson a hello **üres logikai alkalmazás** sablont.</span><span class="sxs-lookup"><span data-stu-id="935a1-190">Then in hello Logic Apps Designer, scroll down and click hello **Blank Logic App** template.</span></span> 

    ![Üres Logic Apps-sablon](media/functions-twitter-email/blank.png)

<span data-ttu-id="935a1-192">Hello Logic Apps Designer tooadd szolgáltatások és eseményindítók tooyour alkalmazás most már használhatja.</span><span class="sxs-lookup"><span data-stu-id="935a1-192">You can now use hello Logic Apps Designer tooadd services and triggers tooyour app.</span></span>

## <a name="connect-tootwitter"></a><span data-ttu-id="935a1-193">Csatlakozás tooTwitter</span><span class="sxs-lookup"><span data-stu-id="935a1-193">Connect tooTwitter</span></span>

<span data-ttu-id="935a1-194">Először hozzon létre egy kapcsolat tooyour Twitter-fiók.</span><span class="sxs-lookup"><span data-stu-id="935a1-194">First, create a connection tooyour Twitter account.</span></span> <span data-ttu-id="935a1-195">hello logikai alkalmazás lekérdezi az Twitter-üzeneteket, amelyek hello app toorun indítható el.</span><span class="sxs-lookup"><span data-stu-id="935a1-195">hello logic app polls for tweets, which trigger hello app toorun.</span></span>

1. <span data-ttu-id="935a1-196">Hello tervezőben kattintson hello **Twitter** szolgáltatásra, és kattintson a hello **amikor egy új tweetet visszaküldi** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="935a1-196">In hello designer, click hello **Twitter** service, and click hello **When a new tweet is posted** trigger.</span></span> <span data-ttu-id="935a1-197">Jelentkezzen be tooyour Twitter-fiók, és engedélyezheti a Logic Apps toouse fiókját.</span><span class="sxs-lookup"><span data-stu-id="935a1-197">Sign in tooyour Twitter account and authorize Logic Apps toouse your account.</span></span>

2. <span data-ttu-id="935a1-198">Beállításokkal hello Twitter eseményindító hello táblázatban megadottak szerint.</span><span class="sxs-lookup"><span data-stu-id="935a1-198">Use hello Twitter trigger settings as specified in hello table.</span></span> 

    ![Twitter-összekötő beállításai](media/functions-twitter-email/azure_tweet.png)

    | <span data-ttu-id="935a1-200">Beállítás</span><span class="sxs-lookup"><span data-stu-id="935a1-200">Setting</span></span>      |  <span data-ttu-id="935a1-201">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="935a1-201">Suggested value</span></span>   | <span data-ttu-id="935a1-202">Leírás</span><span class="sxs-lookup"><span data-stu-id="935a1-202">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="935a1-203">**Keresett szöveg**</span><span class="sxs-lookup"><span data-stu-id="935a1-203">**Search text**</span></span> | <span data-ttu-id="935a1-204">#Azure</span><span class="sxs-lookup"><span data-stu-id="935a1-204">#Azure</span></span> | <span data-ttu-id="935a1-205">Használja a hashtaggel történő, amely elegendő népszerű toogenerate új Twitter-üzeneteket hello választott időszakban.</span><span class="sxs-lookup"><span data-stu-id="935a1-205">Use a hashtag that is popular enough for toogenerate new tweets in hello chosen interval.</span></span> <span data-ttu-id="935a1-206">Hello ingyenes szint és a hashtaggel történő használata esetén túl népszerű gyorsan használhatja fel hello tranzakciók a kognitív Services-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="935a1-206">When using hello Free tier and your hashtag is too popular, you can quickly use up hello transactions in your Cognitive Services account.</span></span> |
    | <span data-ttu-id="935a1-207">**Gyakoriság**</span><span class="sxs-lookup"><span data-stu-id="935a1-207">**Frequency**</span></span> | <span data-ttu-id="935a1-208">Perc</span><span class="sxs-lookup"><span data-stu-id="935a1-208">Minute</span></span> | <span data-ttu-id="935a1-209">a lekérdezés Twitter használt hello gyakoriság egység.</span><span class="sxs-lookup"><span data-stu-id="935a1-209">hello frequency unit used for polling Twitter.</span></span>  |
    | <span data-ttu-id="935a1-210">**Időköz**</span><span class="sxs-lookup"><span data-stu-id="935a1-210">**Interval**</span></span> | <span data-ttu-id="935a1-211">15</span><span class="sxs-lookup"><span data-stu-id="935a1-211">15</span></span> | <span data-ttu-id="935a1-212">Twitter-kérelmek gyakorisága egységekben között eltelt hello idő.</span><span class="sxs-lookup"><span data-stu-id="935a1-212">hello time elapsed between Twitter requests, in frequency units.</span></span> |

3.  <span data-ttu-id="935a1-213">Kattintson a **mentése** tooconnect tooyour Twitter-fiók.</span><span class="sxs-lookup"><span data-stu-id="935a1-213">Click **Save** tooconnect tooyour Twitter account.</span></span> 

<span data-ttu-id="935a1-214">Az alkalmazás most már csatlakoztatott tooTwitter áll.</span><span class="sxs-lookup"><span data-stu-id="935a1-214">Now your app is connected tooTwitter.</span></span> <span data-ttu-id="935a1-215">A következő csatlakozás tootext analytics toodetect hello véleményeket az összegyűjtött Twitter-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="935a1-215">Next, you connect tootext analytics toodetect hello sentiment of collected tweets.</span></span>

## <a name="add-sentiment-detection"></a><span data-ttu-id="935a1-216">Adja hozzá a céggel kapcsolatos véleményeket észlelése</span><span class="sxs-lookup"><span data-stu-id="935a1-216">Add sentiment detection</span></span>

1. <span data-ttu-id="935a1-217">Kattintson a **új lépés**, majd **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="935a1-217">Click **New Step**, and then **Add an action**.</span></span>

    ![Új lépéssel, majd a művelet hozzáadása](media/functions-twitter-email/new_step.png)

2. <span data-ttu-id="935a1-219">A **művelet kiválasztását**, kattintson a **Szövegelemzések**, és kattintson a hello **észleli a céggel kapcsolatos véleményeket** művelet.</span><span class="sxs-lookup"><span data-stu-id="935a1-219">In **Choose an action**, click **Text Analytics**, and then click hello **Detect sentiment** action.</span></span>

    ![Véleményeket észlelése](media/functions-twitter-email/detect_sent.png)

3. <span data-ttu-id="935a1-221">Írja be a kapcsolat neve, mint `MyCognitiveServicesConnection`, illessze be a hello kulcs, a mentett a kognitív szolgáltatások fiókra, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="935a1-221">Type a connection name such as `MyCognitiveServicesConnection`, paste hello key for your Cognitive Services account that you saved, and click **Create**.</span></span>  

4. <span data-ttu-id="935a1-222">Kattintson a **szöveg tooanalyze** > **Tweetet szöveg**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="935a1-222">Click **Text tooanalyze** > **Tweet text**, and then click **Save**.</span></span>  

    ![Véleményeket észlelése](media/functions-twitter-email/ds_tta.png)

<span data-ttu-id="935a1-224">Most, hogy a céggel kapcsolatos véleményeket észlelési van konfigurálva, a kapcsolat tooyour függvény, amely akkor hello véleményeket pontszám kimeneti is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="935a1-224">Now that sentiment detection is configured, you can add a connection tooyour function that consumes hello sentiment score output.</span></span>

## <a name="connect-sentiment-output-tooyour-function"></a><span data-ttu-id="935a1-225">Csatlakozás a céggel kapcsolatos véleményeket kimeneti tooyour függvény</span><span class="sxs-lookup"><span data-stu-id="935a1-225">Connect sentiment output tooyour function</span></span>

1. <span data-ttu-id="935a1-226">A Logic Apps Designer hello, kattintson **új lépés** > **művelet hozzáadása**, és kattintson a **Azure Functions**.</span><span class="sxs-lookup"><span data-stu-id="935a1-226">In hello Logic Apps Designer, click **New step** > **Add an action**, and then click **Azure Functions**.</span></span> 

2. <span data-ttu-id="935a1-227">Kattintson a **jelöljön ki egy Azure függvényt**, jelölje be hello **CategorizeSentiment** korábban létrehozott függvény.</span><span class="sxs-lookup"><span data-stu-id="935a1-227">Click **Choose an Azure function**, select hello **CategorizeSentiment** function you created earlier.</span></span>  

    ![Válasszon egy Azure függvény megjelenítő Azure függvény mezőben](media/functions-twitter-email/choose_fun.png)

3. <span data-ttu-id="935a1-229">A **kérelem törzse**, kattintson a **pontszám** , majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="935a1-229">In **Request Body**, click **Score** and then **Save**.</span></span>

    ![Pontszám](media/functions-twitter-email/trigger_score.png)

<span data-ttu-id="935a1-231">Most a függvény lesz kiváltva, ha a céggel kapcsolatos véleményeket pontszám küldi hello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="935a1-231">Now, your function is triggered when a sentiment score is sent from hello logic app.</span></span> <span data-ttu-id="935a1-232">Itt a színkódolás kategória hello függvény által visszaadott toohello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="935a1-232">A color-coded category is returned toohello logic app by hello function.</span></span> <span data-ttu-id="935a1-233">Ezután adja hozzá az e-mailben értesítést küldi, ha a céggel kapcsolatos véleményeket érték **piros** hello függvény küld vissza.</span><span class="sxs-lookup"><span data-stu-id="935a1-233">Next, you add an email notification that is sent when a sentiment value of **RED** is returned from hello function.</span></span> 

## <a name="add-email-notifications"></a><span data-ttu-id="935a1-234">E-mail értesítések hozzáadása</span><span class="sxs-lookup"><span data-stu-id="935a1-234">Add email notifications</span></span>

<span data-ttu-id="935a1-235">hello hello munkafolyamat utolsó része esetén tootrigger egy e-mailt hello véleményeket pontozza a mennyiségeket _piros_.</span><span class="sxs-lookup"><span data-stu-id="935a1-235">hello last part of hello workflow is tootrigger an email when hello sentiment is scored as _RED_.</span></span> <span data-ttu-id="935a1-236">Ez a témakör egy Outlook.com-összekötőt használja.</span><span class="sxs-lookup"><span data-stu-id="935a1-236">This topic uses an Outlook.com connector.</span></span> <span data-ttu-id="935a1-237">Hasonló lépéseket toouse Gmail vagy Office 365 Outlook összekötő végezheti el.</span><span class="sxs-lookup"><span data-stu-id="935a1-237">You can perform similar steps toouse a Gmail or Office 365 Outlook connector.</span></span>   

1. <span data-ttu-id="935a1-238">A Logic Apps Designer hello, kattintson **új lépés** > **feltétel hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="935a1-238">In hello Logic Apps Designer, click **New step** > **Add a condition**.</span></span> 

2. <span data-ttu-id="935a1-239">Kattintson a **adjon meg értéket**, majd kattintson a **törzs**.</span><span class="sxs-lookup"><span data-stu-id="935a1-239">Click **Choose a value**, then click **Body**.</span></span> <span data-ttu-id="935a1-240">Válassza ki **egyenlő**, kattintson a **adjon meg értéket** és írja be `RED`, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="935a1-240">Select **is equal to**, click **Choose a value** and type `RED`, and click **Save**.</span></span> 

    ![Adjon hozzá egy feltétel toohello logikai alkalmazást.](media/functions-twitter-email/condition.png)

3. <span data-ttu-id="935a1-242">A **Ha igen, nincs**, kattintson a **művelet hozzáadása**, keresse meg `outlook.com`, kattintson **egy e-mailek küldése**, és jelentkezzen be tooyour Outlook.com-fiók.</span><span class="sxs-lookup"><span data-stu-id="935a1-242">In **IF YES, DO NOTHING**, click **Add an action**, search for `outlook.com`, click **Send an email**, and sign in tooyour Outlook.com account.</span></span>
    
    ![Hello feltétel művelet kiválasztását.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > <span data-ttu-id="935a1-244">Ha egy Outlook.com-fiók nem rendelkezik, választhat egy másik összekötő, például a Gmailes vagy Office 365 Outlook</span><span class="sxs-lookup"><span data-stu-id="935a1-244">If you don't have an Outlook.com account, you can choose another connector, such as Gmail or Office 365 Outlook</span></span>

4. <span data-ttu-id="935a1-245">A hello **egy e-mailek küldése** művelet, mint hello e-mail beállításokat meg hello táblában.</span><span class="sxs-lookup"><span data-stu-id="935a1-245">In hello **Send an email** action, use hello email settings as specified in hello table.</span></span> 

    ![Konfigurálhatja a hello küldési e-mail művelet hello e-maileket.](media/functions-twitter-email/sendemail.png)

    | <span data-ttu-id="935a1-247">Beállítás</span><span class="sxs-lookup"><span data-stu-id="935a1-247">Setting</span></span>      |  <span data-ttu-id="935a1-248">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="935a1-248">Suggested value</span></span>   | <span data-ttu-id="935a1-249">Leírás</span><span class="sxs-lookup"><span data-stu-id="935a1-249">Description</span></span>  |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="935a1-250">**A**</span><span class="sxs-lookup"><span data-stu-id="935a1-250">**To**</span></span> | <span data-ttu-id="935a1-251">Az e-mail címet</span><span class="sxs-lookup"><span data-stu-id="935a1-251">Type your email address</span></span> | <span data-ttu-id="935a1-252">hello e-mail címet, amely hello értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="935a1-252">hello email address that receives hello notification.</span></span> |
    | <span data-ttu-id="935a1-253">**Tulajdonos**</span><span class="sxs-lookup"><span data-stu-id="935a1-253">**Subject**</span></span> | <span data-ttu-id="935a1-254">Negatív tweetet céggel kapcsolatos véleményeket észlelt</span><span class="sxs-lookup"><span data-stu-id="935a1-254">Negative tweet sentiment detected</span></span>  | <span data-ttu-id="935a1-255">hello hello e-mail értesítés tárgyát.</span><span class="sxs-lookup"><span data-stu-id="935a1-255">hello subject line of hello email notification.</span></span>  |
    | <span data-ttu-id="935a1-256">**Törzs**</span><span class="sxs-lookup"><span data-stu-id="935a1-256">**Body**</span></span> | <span data-ttu-id="935a1-257">Tweetet szöveg, amelyet helyre</span><span class="sxs-lookup"><span data-stu-id="935a1-257">Tweet text, Location</span></span> | <span data-ttu-id="935a1-258">Kattintson a hello **Tweetet szöveg** és **hely** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="935a1-258">Click hello **Tweet text** and **Location** parameters.</span></span> |

5.  <span data-ttu-id="935a1-259">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="935a1-259">Click **Save**.</span></span>

<span data-ttu-id="935a1-260">Most, hogy hello munkafolyamat befejeződött, hello logikai alkalmazás engedélyezése, és tekintse meg a munkahelyi hello függvény.</span><span class="sxs-lookup"><span data-stu-id="935a1-260">Now that hello workflow is complete, you can enable hello logic app and see hello function at work.</span></span>

## <a name="test-hello-workflow"></a><span data-ttu-id="935a1-261">Teszt hello munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="935a1-261">Test hello workflow</span></span>

1. <span data-ttu-id="935a1-262">A Logic App Designer hello, kattintson **futtatása** toostart hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="935a1-262">In hello Logic App Designer, click **Run** toostart hello app.</span></span>

2. <span data-ttu-id="935a1-263">A hello bal oldali oszlopban kattintson **áttekintése** hello logikai alkalmazás toosee hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="935a1-263">In hello left column, click **Overview** toosee hello status of hello logic app.</span></span> 
 
    ![Logic app végrehajtási állapota](media/functions-twitter-email/over1.png)

3. <span data-ttu-id="935a1-265">(Választható) Kattintson az egyik hello futtatása toosee hello végrehajtási részleteit.</span><span class="sxs-lookup"><span data-stu-id="935a1-265">(Optional) Click one of hello runs toosee details of hello execution.</span></span>

4. <span data-ttu-id="935a1-266">Nyissa meg tooyour függvény, hello naplók megtekintése, és győződjön meg arról, hogy a céggel kapcsolatos véleményeket értéket is fogad és dolgoz.</span><span class="sxs-lookup"><span data-stu-id="935a1-266">Go tooyour function, view hello logs, and verify that sentiment values were received and processed.</span></span>
 
    ![Függvény naplók megtekintése](media/functions-twitter-email/sent.png)

5. <span data-ttu-id="935a1-268">A potenciálisan negatív véleményeket észlelésekor e-mailt kapni.</span><span class="sxs-lookup"><span data-stu-id="935a1-268">When a potentially negative sentiment is detected, you receive an email.</span></span> <span data-ttu-id="935a1-269">Ha még nem kapott e-mailt, minden egyes hello függvény kód tooreturn piros lehet módosítani:</span><span class="sxs-lookup"><span data-stu-id="935a1-269">If you haven't received an email, you can change hello function code tooreturn RED every time:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    <span data-ttu-id="935a1-270">Miután ellenőrizte, hogy e-mail értesítést, módosítsa a hátsó toohello eredeti kódot:</span><span class="sxs-lookup"><span data-stu-id="935a1-270">After you have verified email notifications, change back toohello original code:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > <span data-ttu-id="935a1-271">Ez az oktatóanyag befejezése után le kell tiltania hello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="935a1-271">After you have completed this tutorial, you should disable hello logic app.</span></span> <span data-ttu-id="935a1-272">Hello alkalmazás letiltása, elkerülheti a által használt és a végrehajtások megterhelni hello tranzakciók a kognitív Services-fiók mentése folyamatban.</span><span class="sxs-lookup"><span data-stu-id="935a1-272">By disabling hello app, you avoid being charged for executions and using up hello transactions in your Cognitive Services account.</span></span>

<span data-ttu-id="935a1-273">Most láthatta, milyen egyszerűen a Logic Apps munkafolyamat toointegrate funkcióit.</span><span class="sxs-lookup"><span data-stu-id="935a1-273">Now you have seen how easy it is toointegrate Functions into a Logic Apps workflow.</span></span>

## <a name="disable-hello-logic-app"></a><span data-ttu-id="935a1-274">Hello logikai alkalmazás letiltása</span><span class="sxs-lookup"><span data-stu-id="935a1-274">Disable hello logic app</span></span>

<span data-ttu-id="935a1-275">toodisable hello logic app, kattintson a **áttekintése** , majd **letiltása** üdvözlő képernyőt hello tetején.</span><span class="sxs-lookup"><span data-stu-id="935a1-275">toodisable hello logic app, click **Overview** and then click **Disable** at hello top of hello screen.</span></span> <span data-ttu-id="935a1-276">Ezzel leállítja a hello logikai alkalmazás fut, és ezzel járó költségek hello alkalmazás törlése nélkül.</span><span class="sxs-lookup"><span data-stu-id="935a1-276">This stops hello logic app from running and incurring charges without deleting hello app.</span></span> 

![Függvény naplók](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a><span data-ttu-id="935a1-278">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="935a1-278">Next steps</span></span>

<span data-ttu-id="935a1-279">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="935a1-279">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="935a1-280">Kognitív Services-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="935a1-280">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="935a1-281">Hozzon létre egy függvényt, Kategorizáló tweetet céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="935a1-281">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="935a1-282">TooTwitter csatlakozó logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="935a1-282">Create a logic app that connects tooTwitter.</span></span>
> * <span data-ttu-id="935a1-283">Adja hozzá a céggel kapcsolatos véleményeket észlelési toohello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="935a1-283">Add sentiment detection toohello logic app.</span></span> 
> * <span data-ttu-id="935a1-284">Csatlakozás hello logic app toohello függvény.</span><span class="sxs-lookup"><span data-stu-id="935a1-284">Connect hello logic app toohello function.</span></span>
> * <span data-ttu-id="935a1-285">Küldjön egy e-mailt hello válaszát hello függvény alapján.</span><span class="sxs-lookup"><span data-stu-id="935a1-285">Send an email based on hello response from hello function.</span></span>

<span data-ttu-id="935a1-286">Hogyan előzetes toohello következő útmutató toolearn toocreate egy kiszolgáló nélküli API-t, a függvény.</span><span class="sxs-lookup"><span data-stu-id="935a1-286">Advance toohello next tutorial toolearn how toocreate a serverless API for your function.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="935a1-287">Kiszolgáló nélküli API létrehozása az Azure Functions használatával</span><span class="sxs-lookup"><span data-stu-id="935a1-287">Create a serverless API using Azure Functions</span></span>](functions-create-serverless-api.md)

<span data-ttu-id="935a1-288">További információk a Logic Apps toolearn lásd [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="935a1-288">toolearn more about Logic Apps, see [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).</span></span>


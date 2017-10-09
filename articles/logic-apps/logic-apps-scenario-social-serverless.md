---
title: "aaaScenario - hozzon létre egy felhasználói irányítópult Azure kiszolgáló nélküli |} Microsoft Docs"
description: "Példa bemutatja, hogyan hozhat létre irányítópult toomanage ügyfél visszajelzést, közösségi adatok és további Azure Logic Apps és az Azure Functions."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
ms.openlocfilehash: db175e895e37aa795a9c34bf4d65566bf68f8c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a><span data-ttu-id="c14c4-103">Az Azure Logic Apps és az Azure Functions egy valós idejű felhasználói irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="c14c4-103">Create a real-time customer insights dashboard with Azure Logic Apps and Azure Functions</span></span>

<span data-ttu-id="c14c4-104">Azure-kiszolgáló nélküli eszközök alkalmazások biztosítása a hatékony képességekkel tooquickly build és a gazdagép hello felhőben anélkül, hogy az infrastruktúrával kapcsolatos toothink.</span><span class="sxs-lookup"><span data-stu-id="c14c4-104">Azure Serverless tools provide powerful capabilities tooquickly build and host applications in hello cloud, without having toothink about infrastructure.</span></span>  <span data-ttu-id="c14c4-105">Ebben az esetben a rendszer létrehoz egy irányítópult tootrigger az ügyfelek visszajelzései, visszajelzés machine Learning segítségével elemezheti, és közzététele insights egy forrás, például a Power bi-ban vagy az Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="c14c4-105">In this scenario, we will create a dashboard tootrigger on customer feedback, analyze feedback with machine learning, and publish insights a source like Power BI or Azure Data Lake.</span></span>

## <a name="overview-of-hello-scenario-and-tools-used"></a><span data-ttu-id="c14c4-106">Hello forgatókönyv és a használt eszközök áttekintése</span><span class="sxs-lookup"><span data-stu-id="c14c4-106">Overview of hello scenario and tools used</span></span>

<span data-ttu-id="c14c4-107">A rendezés tooimplement ebben a megoldásban, azt fogja használni a legfontosabb összetevők hello két kiszolgáló nélküli alkalmazások az Azure-ban: [Azure Functions](https://azure.microsoft.com/services/functions/) és [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span><span class="sxs-lookup"><span data-stu-id="c14c4-107">In order tooimplement this solution, we will leverage hello two key components of serverless apps in Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span></span>

<span data-ttu-id="c14c4-108">A Logic Apps egy kiszolgáló nélküli munkafolyamat-motor hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="c14c4-108">Logic Apps is a serverless workflow engine in hello cloud.</span></span>  <span data-ttu-id="c14c4-109">Vezénylési biztosít a kiszolgáló nélküli összetevői között, és tooover 100 szolgáltatások és API-kat is csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="c14c4-109">It provides orchestration across serverless components, and also connects tooover 100 services and APIs.</span></span>  <span data-ttu-id="c14c4-110">A jelen esetben az ügyfelek visszajelzései létrehozunk egy logic app tootrigger.</span><span class="sxs-lookup"><span data-stu-id="c14c4-110">For this scenario, we will create a logic app tootrigger on feedback from customers.</span></span>  <span data-ttu-id="c14c4-111">Hello összekötők, amelyek segítik a reakciójú toocustomer visszajelzés többek között Outlook.com-os, az Office 365, a felmérést Monkey, a Twitter és a HTTP-kérelem [egy webes űrlap](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).</span><span class="sxs-lookup"><span data-stu-id="c14c4-111">Some of hello connectors that can assist in reacting toocustomer feedback include Outlook.com, Office 365, Survey Monkey, Twitter, and an HTTP Request [from a web form](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).</span></span>  <span data-ttu-id="c14c4-112">Az alábbi hello munkafolyamat azt figyeli egy hashtaggel történő a Twitteren.</span><span class="sxs-lookup"><span data-stu-id="c14c4-112">For hello workflow below, we will monitor a hashtag on Twitter.</span></span>

<span data-ttu-id="c14c4-113">Funkciók adja meg a kiszolgáló nélküli számítási hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="c14c4-113">Functions provide serverless compute in hello cloud.</span></span>  <span data-ttu-id="c14c4-114">Ebben a forgatókönyvben használjuk az Azure Functions tooflag Twitter-üzeneteket az ügyfelektől, előre definiált kulcsszavakat sorozata alapján.</span><span class="sxs-lookup"><span data-stu-id="c14c4-114">In this scenario, we will use Azure Functions tooflag tweets from customers based on a series of pre-defined key words.</span></span>

<span data-ttu-id="c14c4-115">hello teljes megoldás lehet [a Visual Studio build](logic-apps-deploy-from-vs.md) és [erőforrás sablon részeként](logic-apps-create-deploy-template.md).</span><span class="sxs-lookup"><span data-stu-id="c14c4-115">hello entire solution can be [build in Visual Studio](logic-apps-deploy-from-vs.md) and [deployed as part of a resource template](logic-apps-create-deploy-template.md).</span></span>  <span data-ttu-id="c14c4-116">Szerepel továbbá hello forgatókönyv bemutató videó [a Channel 9](http://aka.ms/logicappsdemo).</span><span class="sxs-lookup"><span data-stu-id="c14c4-116">There is also video walkthrough of hello scenario [on Channel 9](http://aka.ms/logicappsdemo).</span></span>

## <a name="build-hello-logic-app-tootrigger-on-customer-data"></a><span data-ttu-id="c14c4-117">Hello logic app tootrigger ügyféladatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="c14c4-117">Build hello logic app tootrigger on customer data</span></span>

<span data-ttu-id="c14c4-118">Miután [logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md) a Visual Studio vagy hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="c14c4-118">After [creating a logic app](logic-apps-create-a-logic-app.md) in Visual Studio or hello Azure portal:</span></span>

1. <span data-ttu-id="c14c4-119">Adja hozzá az eseményindító **az új Twitter-üzeneteket** a Twitteren</span><span class="sxs-lookup"><span data-stu-id="c14c4-119">Add a trigger for **On New Tweets** from Twitter</span></span>
2. <span data-ttu-id="c14c4-120">Hello eseményindító toolisten tootweets konfigurálása kulcsszó vagy hashtaggel történő.</span><span class="sxs-lookup"><span data-stu-id="c14c4-120">Configure hello trigger toolisten tootweets on a keyword or hashtag.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c14c4-121">hello eseményindító hello ismétlődési tulajdonság határozza meg, hogy milyen gyakran hello logikai alkalmazás ellenőrzi a lekérdezési alapú eseményindítók új elemek</span><span class="sxs-lookup"><span data-stu-id="c14c4-121">hello recurrence property on hello trigger will determine how frequently hello logic app checks for new items on polling-based triggers</span></span>

   ![Twitter eseményindító – példa][1]

<span data-ttu-id="c14c4-123">Ez az alkalmazás most már az összes új Twitter-üzeneteket fog érvényesítést.</span><span class="sxs-lookup"><span data-stu-id="c14c4-123">This app will now fire on all new tweets.</span></span>  <span data-ttu-id="c14c4-124">Azt majd tweetet adatok igénybe vehet, és részletesebb kifejezett hello véleményeket.</span><span class="sxs-lookup"><span data-stu-id="c14c4-124">We can then take that tweet data and understand more of hello sentiment expressed.</span></span>  <span data-ttu-id="c14c4-125">A hello használjuk [Azure kognitív szolgáltatás](https://azure.microsoft.com/services/cognitive-services/) szöveg toodetect céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="c14c4-125">For this we use hello [Azure Cognitive Service](https://azure.microsoft.com/services/cognitive-services/) toodetect sentiment of text.</span></span>

1. <span data-ttu-id="c14c4-126">Kattintson a **új lépés**</span><span class="sxs-lookup"><span data-stu-id="c14c4-126">Click **New Step**</span></span>
1. <span data-ttu-id="c14c4-127">Válassza ki, vagy keresse meg a hello **Szövegelemzések** összekötő</span><span class="sxs-lookup"><span data-stu-id="c14c4-127">Select or search for hello **Text Analytics** connector</span></span>
1. <span data-ttu-id="c14c4-128">Jelölje be hello **észlelése céggel kapcsolatos véleményeket** művelet</span><span class="sxs-lookup"><span data-stu-id="c14c4-128">Select hello **Detect Sentiment** operation</span></span>
1. <span data-ttu-id="c14c4-129">Ha a rendszer kéri, adjon meg egy érvényes kognitív szolgáltatások kulcs hello Szövegelemzések szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c14c4-129">If prompted, provide a valid Cognitive Services key for hello Text Analytics service</span></span>
1. <span data-ttu-id="c14c4-130">Adja hozzá a hello **Tweetet szöveg** , szöveges tooanalyze hello.</span><span class="sxs-lookup"><span data-stu-id="c14c4-130">Add hello **Tweet Text** as hello text tooanalyze.</span></span>

<span data-ttu-id="c14c4-131">Most, hogy hello tweetet adatokat és az elemzések a hello tweetet, más összekötőket vonatkozó lehet:</span><span class="sxs-lookup"><span data-stu-id="c14c4-131">Now that we have hello tweet data, and insights on hello tweet, a number of other connectors may be relevant:</span></span>
* <span data-ttu-id="c14c4-132">Power BI - sorok tooStreaming adatkészlet hozzáadása: valós idejű egy Power BI-irányítópult nézet Twitter-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="c14c4-132">Power BI - Add Rows tooStreaming Dataset: View tweets real time on a Power BI dashboard.</span></span>
* <span data-ttu-id="c14c4-133">Azure Data Lake - fájl hozzáfűzése: felhasználói adatok tooan Azure Data Lake dataset tooinclude hozzáadása az analytics-feladatok.</span><span class="sxs-lookup"><span data-stu-id="c14c4-133">Azure Data Lake - Append file: Add customer data tooan Azure Data Lake dataset tooinclude in analytics jobs.</span></span>
* <span data-ttu-id="c14c4-134">SQL - sorok felvételének: adatok tárolása a későbbi beolvasásához adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c14c4-134">SQL - Add rows: Store data in a database for later retrieval.</span></span>
* <span data-ttu-id="c14c4-135">Slack - üzenetet küldeni: egy közzététele a slack-csatornát negatív visszajelzés műveletek igénylő riasztást.</span><span class="sxs-lookup"><span data-stu-id="c14c4-135">Slack - Send message: Alert a slack channel on negative feedback that requires actions.</span></span>

<span data-ttu-id="c14c4-136">Egy Azure-függvény használt toodo hello adatok felett számítási további egyéni is lehet.</span><span class="sxs-lookup"><span data-stu-id="c14c4-136">An Azure Function can also be used toodo more custom compute on top of hello data.</span></span>

## <a name="enriching-hello-data-with-an-azure-function"></a><span data-ttu-id="c14c4-137">Egy Azure-függvény hello adatok bővítése</span><span class="sxs-lookup"><span data-stu-id="c14c4-137">Enriching hello data with an Azure Function</span></span>

<span data-ttu-id="c14c4-138">Azt is hozzon létre egy függvényt, igazolnia kell toohave egy függvény alkalmazást az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="c14c4-138">Before we can create a function, we need toohave a function app in our Azure subscription.</span></span>  <span data-ttu-id="c14c4-139">Egy Azure-függvény létrehozásának hello portálon is [itt található](../azure-functions/functions-create-first-azure-function-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c14c4-139">Details on creating an Azure Function in hello portal can [be found here](../azure-functions/functions-create-first-azure-function-azure-portal.md)</span></span>

<span data-ttu-id="c14c4-140">Egy függvény toobe hívása közvetlenül egy logikai alkalmazást, az azt kell toohave HTTP indítható el, kötés.</span><span class="sxs-lookup"><span data-stu-id="c14c4-140">For a function toobe called directly from a logic app, it needs toohave an HTTP trigger binding.</span></span>  <span data-ttu-id="c14c4-141">Azt javasoljuk, hello **HttpTrigger** sablont.</span><span class="sxs-lookup"><span data-stu-id="c14c4-141">We recommend using hello **HttpTrigger** template.</span></span>

<span data-ttu-id="c14c4-142">Ebben a forgatókönyvben a hello kérelem törzse hello Azure-függvény lenne hello tweetet szöveg.</span><span class="sxs-lookup"><span data-stu-id="c14c4-142">In this scenario, hello request body of hello Azure Function would be hello tweet text.</span></span>  <span data-ttu-id="c14c4-143">Hello függvény kódban egyszerűen meg logika Ha hello tweetet szöveg tartalmazza-e egy kulcsszót vagy kifejezést.</span><span class="sxs-lookup"><span data-stu-id="c14c4-143">In hello function code, simply define logic on if hello tweet text contains a key word or phrase.</span></span>  <span data-ttu-id="c14c4-144">hello függvény önmagára sikerült megmarad, mint egyszerűek vagy összetettek, hello a forgatókönyvhöz szükséges.</span><span class="sxs-lookup"><span data-stu-id="c14c4-144">hello function itself could be kept as simple or complex as needed for hello scenario.</span></span>

<span data-ttu-id="c14c4-145">Hello függvény hello végén egyszerűen vissza válasz toohello logikai alkalmazás adatokkal.</span><span class="sxs-lookup"><span data-stu-id="c14c4-145">At hello end of hello function, simply return a response toohello logic app with some data.</span></span>  <span data-ttu-id="c14c4-146">Ennek oka lehet egy egyszerű logikai értéket (pl. `containsKeyword`), vagy egy összetett objektumot.</span><span class="sxs-lookup"><span data-stu-id="c14c4-146">This could be a simple boolean value (e.g. `containsKeyword`), or a complex object.</span></span>

![Konfigurált Azure-függvény lépés][2]

> [!TIP]
> <span data-ttu-id="c14c4-148">Egy logikai alkalmazás függvényt az összetett választ elérésekor művelettel hello elemzése JSON.</span><span class="sxs-lookup"><span data-stu-id="c14c4-148">When accessing a complex response from a function in a logic app, use hello Parse JSON action.</span></span>

<span data-ttu-id="c14c4-149">Miután hello függvény mentettük, a fentiekben létrehozott hello logika alkalmazásba vehető fel.</span><span class="sxs-lookup"><span data-stu-id="c14c4-149">Once hello function is saved, it can be added into hello logic app created above.</span></span>  <span data-ttu-id="c14c4-150">A hello logic app:</span><span class="sxs-lookup"><span data-stu-id="c14c4-150">In hello logic app:</span></span>

1. <span data-ttu-id="c14c4-151">Kattintson a tooadd egy **új lépés**</span><span class="sxs-lookup"><span data-stu-id="c14c4-151">Click tooadd a **New Step**</span></span>
1. <span data-ttu-id="c14c4-152">Jelölje be hello **Azure Functions** összekötő</span><span class="sxs-lookup"><span data-stu-id="c14c4-152">Select hello **Azure Functions** connector</span></span>
1. <span data-ttu-id="c14c4-153">Válasszon egy meglévő függvény toochoose, és keresse meg a létrehozott toohello függvény</span><span class="sxs-lookup"><span data-stu-id="c14c4-153">Select toochoose an existing function, and browse toohello function created</span></span>
1. <span data-ttu-id="c14c4-154">Hello küldése **Tweetet szöveg** a hello **kérelem törzsét.**</span><span class="sxs-lookup"><span data-stu-id="c14c4-154">Send in hello **Tweet Text** for hello **Request Body**</span></span>

## <a name="running-and-monitoring-hello-solution"></a><span data-ttu-id="c14c4-155">Futtatnia vagy felügyelnie hello megoldás</span><span class="sxs-lookup"><span data-stu-id="c14c4-155">Running and monitoring hello solution</span></span>

<span data-ttu-id="c14c4-156">Jelentéskészítő kiszolgáló nélküli álló üzenettípusok összehangolását a Logic Apps előnyeit hello egyik hello hatékony hibakeresési és figyelési képességek.</span><span class="sxs-lookup"><span data-stu-id="c14c4-156">One of hello benefits of authoring serverless orchestrations in Logic Apps is hello rich debug and monitoring capabilities.</span></span>  <span data-ttu-id="c14c4-157">A Futtatás (jelenlegi vagy történelmi) tekintheti meg a Visual Studio hello Azure-portálon vagy REST API hello és SDK-k segítségével.</span><span class="sxs-lookup"><span data-stu-id="c14c4-157">Any run (current or historic) can be viewed from within Visual Studio, hello Azure portal, or via hello REST API and SDKs.</span></span>

<span data-ttu-id="c14c4-158">Hello legegyszerűbb módja tootest logikai alkalmazás egyike hello használja **futtatása** hello Designer gombra.</span><span class="sxs-lookup"><span data-stu-id="c14c4-158">One of hello easiest ways tootest a logic app is using hello **Run** button in hello designer.</span></span>  <span data-ttu-id="c14c4-159">Kattintson a **futtatása** továbbra is toopoll hello eseményindító minden 5 másodperc, amíg a rendszer észlelt egy eseményt, és futtassa hello előrehaladtával egy élő betekintést.</span><span class="sxs-lookup"><span data-stu-id="c14c4-159">Clicking **Run** will continue toopoll hello trigger every 5 seconds until an event is detected, and give a live view as hello run progresses.</span></span>

<span data-ttu-id="c14c4-160">Előző futtatási alábbi előzményeinek hello áttekintése panelen hello Azure-portálon, vagy a Visual Studio Cloud Explorer hello segítségével tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c14c4-160">Previous run histories can be viewed on hello Overview blade in hello Azure portal, or using hello Visual Studio Cloud Explorer.</span></span>

## <a name="creating-a-deployment-template-for-automated-deployments"></a><span data-ttu-id="c14c4-161">Az automatikus központi telepítés központi telepítési sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="c14c4-161">Creating a deployment template for automated deployments</span></span>

<span data-ttu-id="c14c4-162">Ha olyan megoldást fejlesztett ki, rögzített, és egy Azure-telepítés sablon tooany hello world az Azure-régiót keresztül telepíthetők.</span><span class="sxs-lookup"><span data-stu-id="c14c4-162">Once a solution has been developed, it can be captured and deployed via an Azure deployment template tooany Azure region in hello world.</span></span>  <span data-ttu-id="c14c4-163">Ez akkor hasznos, mindkét módosítása paraméter különböző a munkafolyamat-verziót, hanem is használható az ebben a megoldásban egy build és kiadás folyamat.</span><span class="sxs-lookup"><span data-stu-id="c14c4-163">This is useful for both modifying parameters for different versions of this workflow, but also for integrating this solution in a build and release pipeline.</span></span>  <span data-ttu-id="c14c4-164">A központi telepítési sablonok létrehozásának részletes ismertetése [ebben a cikkben](logic-apps-create-deploy-template.md).</span><span class="sxs-lookup"><span data-stu-id="c14c4-164">Details on creating a deployment template can be found [in this article](logic-apps-create-deploy-template.md).</span></span>

<span data-ttu-id="c14c4-165">Az Azure Functions is építhető hello központi telepítési sablont -, így hello teljes megoldás az összes függősége kezelhető egyetlen sablont.</span><span class="sxs-lookup"><span data-stu-id="c14c4-165">Azure Functions can also be incorporated in hello deployment template - so hello entire solution with all dependencies can be managed as a single template.</span></span>  <span data-ttu-id="c14c4-166">Példa függvény a központi telepítési sablont hello található [Azure gyors üzembe helyezés sablon tárház](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).</span><span class="sxs-lookup"><span data-stu-id="c14c4-166">An example of a function deployment template can be found in hello [Azure quickstart template repository](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c14c4-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c14c4-167">Next steps</span></span>

* [<span data-ttu-id="c14c4-168">Tekintse meg az egyéb példák és forgatókönyvek az Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="c14c4-168">See other examples and scenarios for Azure Logic Apps</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="c14c4-169">Tekintse meg a video-útmutatót a megoldás-végpontok létrehozásáról</span><span class="sxs-lookup"><span data-stu-id="c14c4-169">Watch a video walkthrough on creating this solution end-to-end</span></span>](http://aka.ms/logicappsdemo)
* [<span data-ttu-id="c14c4-170">Megtudhatja, hogyan toohandle és catch kivételek logikai alkalmazás belül</span><span class="sxs-lookup"><span data-stu-id="c14c4-170">Learn how toohandle and catch exceptions within a logic app</span></span>](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png
---
title: "Forgatókönyv – a felhasználói irányítópult létrehozása az Azure kiszolgáló nélküli |} Microsoft Docs"
description: "Hogyan kezelheti az ügyfelek visszajelzései alapján, közösségi adatok és további Azure Logic Apps és az Azure Functions egy irányítópultot hozhat létre egy példát."
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
ms.openlocfilehash: 0b6e118cb13ab8185d8eeb42bec6147155967967
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a><span data-ttu-id="c7273-103">Az Azure Logic Apps és az Azure Functions egy valós idejű felhasználói irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7273-103">Create a real-time customer insights dashboard with Azure Logic Apps and Azure Functions</span></span>

<span data-ttu-id="c7273-104">Azure-kiszolgáló nélküli eszközök biztosítanak a hatékony funkciókat gyorsan készíthet, és nem kell gondolniuk infrastruktúra alkalmazásaikat a felhőben tárolni.</span><span class="sxs-lookup"><span data-stu-id="c7273-104">Azure Serverless tools provide powerful capabilities to quickly build and host applications in the cloud, without having to think about infrastructure.</span></span>  <span data-ttu-id="c7273-105">Ebben a forgatókönyvben létrehozunk indul el, az ügyfelek visszajelzései, visszajelzés machine Learning segítségével elemezheti és elemzések közzététele irányítópulton Power bi-ban vagy az Azure Data Lake ki forrást.</span><span class="sxs-lookup"><span data-stu-id="c7273-105">In this scenario, we will create a dashboard to trigger on customer feedback, analyze feedback with machine learning, and publish insights a source like Power BI or Azure Data Lake.</span></span>

## <a name="overview-of-the-scenario-and-tools-used"></a><span data-ttu-id="c7273-106">A forgatókönyv és a használt eszközök áttekintése</span><span class="sxs-lookup"><span data-stu-id="c7273-106">Overview of the scenario and tools used</span></span>

<span data-ttu-id="c7273-107">Ez a megoldás megvalósításához azt fogja használni, a két legfontosabb összetevők, a kiszolgáló nélküli alkalmazások az Azure-ban: [Azure Functions](https://azure.microsoft.com/services/functions/) és [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span><span class="sxs-lookup"><span data-stu-id="c7273-107">In order to implement this solution, we will leverage the two key components of serverless apps in Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span></span>

<span data-ttu-id="c7273-108">A Logic Apps egy kiszolgáló nélküli munkafolyamat-motor a felhőben.</span><span class="sxs-lookup"><span data-stu-id="c7273-108">Logic Apps is a serverless workflow engine in the cloud.</span></span>  <span data-ttu-id="c7273-109">Vezénylési biztosít a kiszolgáló nélküli összetevői között, és több mint 100 szolgáltatások és API-kat is csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="c7273-109">It provides orchestration across serverless components, and also connects to over 100 services and APIs.</span></span>  <span data-ttu-id="c7273-110">A jelen esetben létre fogunk hozni egy logikai alkalmazást az ügyfelek visszajelzései indításához.</span><span class="sxs-lookup"><span data-stu-id="c7273-110">For this scenario, we will create a logic app to trigger on feedback from customers.</span></span>  <span data-ttu-id="c7273-111">Az összekötők, amelyek segítik az ügyfelek visszajelzései alapján reagál a többek között Outlook.com-os, az Office 365, a felmérést Monkey, a Twitter és a HTTP-kérelem [egy webes űrlap](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).</span><span class="sxs-lookup"><span data-stu-id="c7273-111">Some of the connectors that can assist in reacting to customer feedback include Outlook.com, Office 365, Survey Monkey, Twitter, and an HTTP Request [from a web form](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).</span></span>  <span data-ttu-id="c7273-112">Az alábbi munkafolyamat azt figyeli egy hashtaggel történő a Twitteren.</span><span class="sxs-lookup"><span data-stu-id="c7273-112">For the workflow below, we will monitor a hashtag on Twitter.</span></span>

<span data-ttu-id="c7273-113">Funkciók adja meg a kiszolgáló nélküli számítási a felhőben.</span><span class="sxs-lookup"><span data-stu-id="c7273-113">Functions provide serverless compute in the cloud.</span></span>  <span data-ttu-id="c7273-114">Ebben a forgatókönyvben az Azure Functions segítségével jelzőt Twitter-üzeneteket az ügyfelektől, előre definiált kulcsszavakat sorozata alapján.</span><span class="sxs-lookup"><span data-stu-id="c7273-114">In this scenario, we will use Azure Functions to flag tweets from customers based on a series of pre-defined key words.</span></span>

<span data-ttu-id="c7273-115">A teljes megoldás lehet [a Visual Studio build](logic-apps-deploy-from-vs.md) és [erőforrás sablon részeként](logic-apps-create-deploy-template.md).</span><span class="sxs-lookup"><span data-stu-id="c7273-115">The entire solution can be [build in Visual Studio](logic-apps-deploy-from-vs.md) and [deployed as part of a resource template](logic-apps-create-deploy-template.md).</span></span>  <span data-ttu-id="c7273-116">Szerepel továbbá forgatókönyv bemutató videó [a Channel 9](http://aka.ms/logicappsdemo).</span><span class="sxs-lookup"><span data-stu-id="c7273-116">There is also video walkthrough of the scenario [on Channel 9](http://aka.ms/logicappsdemo).</span></span>

## <a name="build-the-logic-app-to-trigger-on-customer-data"></a><span data-ttu-id="c7273-117">Az ügyféladatok elindítani a logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7273-117">Build the logic app to trigger on customer data</span></span>

<span data-ttu-id="c7273-118">Miután [logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md) a Visual Studio vagy az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="c7273-118">After [creating a logic app](logic-apps-create-a-logic-app.md) in Visual Studio or the Azure portal:</span></span>

1. <span data-ttu-id="c7273-119">Adja hozzá az eseményindító **az új Twitter-üzeneteket** a Twitteren</span><span class="sxs-lookup"><span data-stu-id="c7273-119">Add a trigger for **On New Tweets** from Twitter</span></span>
2. <span data-ttu-id="c7273-120">Az eseményindító Twitter-üzeneteket kulcsszó vagy hashtaggel történő figyelésére konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="c7273-120">Configure the trigger to listen to tweets on a keyword or hashtag.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c7273-121">Az eseményindító ismétlődési tulajdonsága határozza meg, hogy milyen gyakran a logikai alkalmazás ellenőrzi a lekérdezési alapú eseményindítók új elemek</span><span class="sxs-lookup"><span data-stu-id="c7273-121">The recurrence property on the trigger will determine how frequently the logic app checks for new items on polling-based triggers</span></span>

   ![Twitter eseményindító – példa][1]

<span data-ttu-id="c7273-123">Ez az alkalmazás most már az összes új Twitter-üzeneteket fog érvényesítést.</span><span class="sxs-lookup"><span data-stu-id="c7273-123">This app will now fire on all new tweets.</span></span>  <span data-ttu-id="c7273-124">Azt majd tweetet adatok igénybe vehet, és részletesebb a céggel kapcsolatos véleményeket kifejezve.</span><span class="sxs-lookup"><span data-stu-id="c7273-124">We can then take that tweet data and understand more of the sentiment expressed.</span></span>  <span data-ttu-id="c7273-125">A használjuk a [Azure kognitív szolgáltatás](https://azure.microsoft.com/services/cognitive-services/) szöveg véleményeket észleléséhez.</span><span class="sxs-lookup"><span data-stu-id="c7273-125">For this we use the [Azure Cognitive Service](https://azure.microsoft.com/services/cognitive-services/) to detect sentiment of text.</span></span>

1. <span data-ttu-id="c7273-126">Kattintson a **új lépés**</span><span class="sxs-lookup"><span data-stu-id="c7273-126">Click **New Step**</span></span>
1. <span data-ttu-id="c7273-127">Válassza ki, vagy keresse meg a **Szövegelemzések** összekötő</span><span class="sxs-lookup"><span data-stu-id="c7273-127">Select or search for the **Text Analytics** connector</span></span>
1. <span data-ttu-id="c7273-128">Válassza ki a **észlelése céggel kapcsolatos véleményeket** művelet</span><span class="sxs-lookup"><span data-stu-id="c7273-128">Select the **Detect Sentiment** operation</span></span>
1. <span data-ttu-id="c7273-129">Ha a rendszer kéri, adja meg egy érvényes kognitív szolgáltatások kulcsát a Szövegelemzések szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c7273-129">If prompted, provide a valid Cognitive Services key for the Text Analytics service</span></span>
1. <span data-ttu-id="c7273-130">Adja hozzá a **Tweetet szöveg** , mint a szöveg elemzésére.</span><span class="sxs-lookup"><span data-stu-id="c7273-130">Add the **Tweet Text** as the text to analyze.</span></span>

<span data-ttu-id="c7273-131">Most, hogy az tweetet adatok, és az elemzések a a tweetet, más összekötőket vonatkozó lehet:</span><span class="sxs-lookup"><span data-stu-id="c7273-131">Now that we have the tweet data, and insights on the tweet, a number of other connectors may be relevant:</span></span>
* <span data-ttu-id="c7273-132">Power BI - sorok hozzáadása a folyamatos átviteli adatkészletet: valós idejű egy Power BI-irányítópult nézet Twitter-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="c7273-132">Power BI - Add Rows to Streaming Dataset: View tweets real time on a Power BI dashboard.</span></span>
* <span data-ttu-id="c7273-133">Azure Data Lake - fájl hozzáfűzése: felhasználói adatok hozzáadása egy Azure Data Lake analytics-feladatok szerepeljenek a DataSet adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="c7273-133">Azure Data Lake - Append file: Add customer data to an Azure Data Lake dataset to include in analytics jobs.</span></span>
* <span data-ttu-id="c7273-134">SQL - sorok felvételének: adatok tárolása a későbbi beolvasásához adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c7273-134">SQL - Add rows: Store data in a database for later retrieval.</span></span>
* <span data-ttu-id="c7273-135">Slack - üzenetet küldeni: egy közzététele a slack-csatornát negatív visszajelzés műveletek igénylő riasztást.</span><span class="sxs-lookup"><span data-stu-id="c7273-135">Slack - Send message: Alert a slack channel on negative feedback that requires actions.</span></span>

<span data-ttu-id="c7273-136">Egy Azure-függvény is használható felett az adatokat több egyéni számítás elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="c7273-136">An Azure Function can also be used to do more custom compute on top of the data.</span></span>

## <a name="enriching-the-data-with-an-azure-function"></a><span data-ttu-id="c7273-137">Az adatokat egy Azure-függvényt a bővítése</span><span class="sxs-lookup"><span data-stu-id="c7273-137">Enriching the data with an Azure Function</span></span>

<span data-ttu-id="c7273-138">Azt is hozzon létre egy függvényt, igazolnia kell egy függvény alkalmazást az Azure-előfizetésben rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c7273-138">Before we can create a function, we need to have a function app in our Azure subscription.</span></span>  <span data-ttu-id="c7273-139">Egy Azure-függvény létrehozása a portálon részleteket is [itt található](../azure-functions/functions-create-first-azure-function-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c7273-139">Details on creating an Azure Function in the portal can [be found here](../azure-functions/functions-create-first-azure-function-azure-portal.md)</span></span>

<span data-ttu-id="c7273-140">Egy függvény hívása közvetlenül a logikai alkalmazás, HTTP rendelkeznie kell elindítani a kötés.</span><span class="sxs-lookup"><span data-stu-id="c7273-140">For a function to be called directly from a logic app, it needs to have an HTTP trigger binding.</span></span>  <span data-ttu-id="c7273-141">Azt javasoljuk, a **HttpTrigger** sablont.</span><span class="sxs-lookup"><span data-stu-id="c7273-141">We recommend using the **HttpTrigger** template.</span></span>

<span data-ttu-id="c7273-142">Ebben a forgatókönyvben a kérés törzsében az Azure-függvény lenne a tweetet szöveget.</span><span class="sxs-lookup"><span data-stu-id="c7273-142">In this scenario, the request body of the Azure Function would be the tweet text.</span></span>  <span data-ttu-id="c7273-143">A függvény kódban egyszerűen határozza meg programot, ha a tweetet szöveg tartalmazza-e egy kulcsszót vagy kifejezést.</span><span class="sxs-lookup"><span data-stu-id="c7273-143">In the function code, simply define logic on if the tweet text contains a key word or phrase.</span></span>  <span data-ttu-id="c7273-144">A függvény önmagára sikerült megmarad, ennek egyszerűek vagy összetettek, a forgatókönyv konfigurálásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="c7273-144">The function itself could be kept as simple or complex as needed for the scenario.</span></span>

<span data-ttu-id="c7273-145">A függvény végén egyszerűen válaszol a logikai alkalmazás adatokkal.</span><span class="sxs-lookup"><span data-stu-id="c7273-145">At the end of the function, simply return a response to the logic app with some data.</span></span>  <span data-ttu-id="c7273-146">Ennek oka lehet egy egyszerű logikai értéket (pl. `containsKeyword`), vagy egy összetett objektumot.</span><span class="sxs-lookup"><span data-stu-id="c7273-146">This could be a simple boolean value (e.g. `containsKeyword`), or a complex object.</span></span>

![Konfigurált Azure-függvény lépés][2]

> [!TIP]
> <span data-ttu-id="c7273-148">Egy logikai alkalmazás függvényt az összetett választ elérésekor művelettel a JSON elemezni.</span><span class="sxs-lookup"><span data-stu-id="c7273-148">When accessing a complex response from a function in a logic app, use the Parse JSON action.</span></span>

<span data-ttu-id="c7273-149">A funkció megmarad, ha azokat az előbb létrehozott logikai alkalmazás vehető fel.</span><span class="sxs-lookup"><span data-stu-id="c7273-149">Once the function is saved, it can be added into the logic app created above.</span></span>  <span data-ttu-id="c7273-150">A logikai alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="c7273-150">In the logic app:</span></span>

1. <span data-ttu-id="c7273-151">Hozzáadásához kattintson ide a **új lépés**</span><span class="sxs-lookup"><span data-stu-id="c7273-151">Click to add a **New Step**</span></span>
1. <span data-ttu-id="c7273-152">Válassza ki a **Azure Functions** összekötő</span><span class="sxs-lookup"><span data-stu-id="c7273-152">Select the **Azure Functions** connector</span></span>
1. <span data-ttu-id="c7273-153">Válasszon egy meglévő függvényben, és keresse meg a létrehozott függvény kiválasztása</span><span class="sxs-lookup"><span data-stu-id="c7273-153">Select to choose an existing function, and browse to the function created</span></span>
1. <span data-ttu-id="c7273-154">Küldjön a **Tweetet szöveg** a a **kérelem törzse**</span><span class="sxs-lookup"><span data-stu-id="c7273-154">Send in the **Tweet Text** for the **Request Body**</span></span>

## <a name="running-and-monitoring-the-solution"></a><span data-ttu-id="c7273-155">Futtatnia vagy felügyelnie a megoldás</span><span class="sxs-lookup"><span data-stu-id="c7273-155">Running and monitoring the solution</span></span>

<span data-ttu-id="c7273-156">A jelentéskészítő kiszolgáló nélküli álló üzenettípusok összehangolását a Logic Apps előnyeit egyik a hatékony hibakeresési és a figyelési lehetőségek körét.</span><span class="sxs-lookup"><span data-stu-id="c7273-156">One of the benefits of authoring serverless orchestrations in Logic Apps is the rich debug and monitoring capabilities.</span></span>  <span data-ttu-id="c7273-157">A Futtatás (jelenlegi vagy történelmi) tekintheti meg a Visual studióban, az Azure-portálon, vagy a REST API-t és az SDK-k segítségével.</span><span class="sxs-lookup"><span data-stu-id="c7273-157">Any run (current or historic) can be viewed from within Visual Studio, the Azure portal, or via the REST API and SDKs.</span></span>

<span data-ttu-id="c7273-158">Egyik legegyszerűbb módja logikai alkalmazás teszteléséhez használja a **futtatása** gomb a tervezőben.</span><span class="sxs-lookup"><span data-stu-id="c7273-158">One of the easiest ways to test a logic app is using the **Run** button in the designer.</span></span>  <span data-ttu-id="c7273-159">Kattintson a **futtatása** továbbra is kérdezze le az eseményindító minden 5 másodperc, amíg a rendszer észlelt egy eseményt, és a futási előrehaladtával egy élő betekintést.</span><span class="sxs-lookup"><span data-stu-id="c7273-159">Clicking **Run** will continue to poll the trigger every 5 seconds until an event is detected, and give a live view as the run progresses.</span></span>

<span data-ttu-id="c7273-160">Előző futtatási alábbi előzményeinek az Áttekintés panel az Azure-portálon, vagy a Visual Studio Cloud Explorer használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c7273-160">Previous run histories can be viewed on the Overview blade in the Azure portal, or using the Visual Studio Cloud Explorer.</span></span>

## <a name="creating-a-deployment-template-for-automated-deployments"></a><span data-ttu-id="c7273-161">Az automatikus központi telepítés központi telepítési sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7273-161">Creating a deployment template for automated deployments</span></span>

<span data-ttu-id="c7273-162">Ha olyan megoldást fejlesztett ki, rögzített, és egy Azure központi telepítési sablont a világ bármely Azure régióban keresztül telepíthetők.</span><span class="sxs-lookup"><span data-stu-id="c7273-162">Once a solution has been developed, it can be captured and deployed via an Azure deployment template to any Azure region in the world.</span></span>  <span data-ttu-id="c7273-163">Ez akkor hasznos, mindkét módosítása paraméter különböző a munkafolyamat-verziót, hanem is használható az ebben a megoldásban egy build és kiadás folyamat.</span><span class="sxs-lookup"><span data-stu-id="c7273-163">This is useful for both modifying parameters for different versions of this workflow, but also for integrating this solution in a build and release pipeline.</span></span>  <span data-ttu-id="c7273-164">A központi telepítési sablonok létrehozásának részletes ismertetése [ebben a cikkben](logic-apps-create-deploy-template.md).</span><span class="sxs-lookup"><span data-stu-id="c7273-164">Details on creating a deployment template can be found [in this article](logic-apps-create-deploy-template.md).</span></span>

<span data-ttu-id="c7273-165">Az Azure Functions is be lehessen építeni a központi telepítési sablont -, a teljes megoldás az összes függősége kezelhető egyetlen sablont.</span><span class="sxs-lookup"><span data-stu-id="c7273-165">Azure Functions can also be incorporated in the deployment template - so the entire solution with all dependencies can be managed as a single template.</span></span>  <span data-ttu-id="c7273-166">Példa függvény a központi telepítési sablont itt található: a [Azure gyors üzembe helyezés sablon tárház](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).</span><span class="sxs-lookup"><span data-stu-id="c7273-166">An example of a function deployment template can be found in the [Azure quickstart template repository](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7273-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c7273-167">Next steps</span></span>

* [<span data-ttu-id="c7273-168">Tekintse meg az egyéb példák és forgatókönyvek az Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="c7273-168">See other examples and scenarios for Azure Logic Apps</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="c7273-169">Tekintse meg a video-útmutatót a megoldás-végpontok létrehozásáról</span><span class="sxs-lookup"><span data-stu-id="c7273-169">Watch a video walkthrough on creating this solution end-to-end</span></span>](http://aka.ms/logicappsdemo)
* [<span data-ttu-id="c7273-170">Megtudhatja, hogyan dolgozza fel a logikai alkalmazások kivételeket és kezelésére</span><span class="sxs-lookup"><span data-stu-id="c7273-170">Learn how to handle and catch exceptions within a logic app</span></span>](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png
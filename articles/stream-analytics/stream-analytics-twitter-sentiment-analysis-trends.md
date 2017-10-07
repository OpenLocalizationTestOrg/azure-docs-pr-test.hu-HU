---
title: "aaaReal-Twitter véleményeket elemzés az Azure Stream Analytics |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Stream Analytics Twitter-véleményeket valós idejű elemzés céljából. Részletes útmutatás az esemény generációs toodata az élő irányítópulton."
keywords: "valós időben twitterről trendelemzés, véleményeket elemzés, közösségi elemzés, trend elemzés példa"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42068691-074b-4c3b-a527-acafa484fda2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: jeffstok
ms.openlocfilehash: 157790caa7ea6f5570dd9c9d3bd9694d437eb4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a><span data-ttu-id="34e43-105">Azure Stream Analytics elemzés, valós idejű Twitter véleményeket</span><span class="sxs-lookup"><span data-stu-id="34e43-105">Real-time Twitter sentiment analysis in Azure Stream Analytics</span></span>

<span data-ttu-id="34e43-106">Megtudhatja, hogyan toobuild véleményeket elemzés megoldást a közösségi média elemzés, valós idejű hozásával Twitter az Azure Event Hubs események.</span><span class="sxs-lookup"><span data-stu-id="34e43-106">Learn how toobuild a sentiment analysis solution for social media analytics by bringing real-time Twitter events into Azure Event Hubs.</span></span> <span data-ttu-id="34e43-107">Ezután az Azure Stream Analytics lekérdezési tooanalyze hello adatok és a tárolókban későbbi használatra eredmények hello vagy írhat irányítópult használata és [Power BI](https://powerbi.com/) valós idejű elemzések tooprovide.</span><span class="sxs-lookup"><span data-stu-id="34e43-107">You can then write an Azure Stream Analytics query tooanalyze hello data and either store hello results for later use or use a dashboard and [Power BI](https://powerbi.com/) tooprovide insights in real time.</span></span>

<span data-ttu-id="34e43-108">Közösségi elemzőeszközök segítségével a szervezetek trendekkel kapcsolatos témakörök ismertetése.</span><span class="sxs-lookup"><span data-stu-id="34e43-108">Social media analytics tools help organizations understand trending topics.</span></span> <span data-ttu-id="34e43-109">Trendekkel témakörök tulajdonosok és a közösségi nagyszámú bejegyzéseket rendelkező szokások szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="34e43-109">Trending topics are subjects and attitudes that have a high volume of posts in social media.</span></span> <span data-ttu-id="34e43-110">Véleményeket elemzés, amely más néven *véleményével adatbányászati*, használja a közösségi média analytics eszközök toodetermine szokások felé egy termék, érdemes és így tovább.</span><span class="sxs-lookup"><span data-stu-id="34e43-110">Sentiment analysis, which is also called *opinion mining*, uses social media analytics tools toodetermine attitudes toward a product, idea, and so on.</span></span> 

<span data-ttu-id="34e43-111">Twitter-trend valós idejű elemzési kiváló példája egy elemzőeszköz, mert hello hashtaggel történő előfizetés modell lehetővé teszi a toolisten toospecific kulcsszavak (hashtageket) és fejlesztéséhez hírcsatorna hello véleményeket elemzése.</span><span class="sxs-lookup"><span data-stu-id="34e43-111">Real-time Twitter trend analysis is a great example of an analytics tool, because hello hashtag subscription model enables you toolisten toospecific keywords (hashtags) and develop sentiment analysis of hello feed.</span></span>

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a><span data-ttu-id="34e43-112">Forgatókönyv: Közösségi véleményeket elemzés, valós idejű</span><span class="sxs-lookup"><span data-stu-id="34e43-112">Scenario: Social media sentiment analysis in real time</span></span>

<span data-ttu-id="34e43-113">A vállalat, amely rendelkezik egy hírek media webhelyen olyan iránt érdeklődik előnyös használhatóságát a versenytársak való kiemeli a webhely tartalmát, amely azonnal megfelelő tooits olvasók által.</span><span class="sxs-lookup"><span data-stu-id="34e43-113">A company that has a news media website is interested in gaining an advantage over its competitors by featuring site content that is immediately relevant tooits readers.</span></span> <span data-ttu-id="34e43-114">hello vállalati közösségi elemzés valós idejű véleményeket Twitter-adatok elemzése végrehajtásával vonatkozó tooreaders témakörök használja.</span><span class="sxs-lookup"><span data-stu-id="34e43-114">hello company uses social media analysis on topics that are relevant tooreaders by doing real-time sentiment analysis of Twitter data.</span></span>

<span data-ttu-id="34e43-115">tooidentify trendekkel kapcsolatos témakörök valós időben Twitterről, a vállalat igényeinek valós idejű elemzési hello tweetet kötet és a céggel kapcsolatos véleményeket kapcsolatos főbb témakörök hello.</span><span class="sxs-lookup"><span data-stu-id="34e43-115">tooidentify trending topics in real time on Twitter, hello company needs real-time analytics about hello tweet volume and sentiment for key topics.</span></span> <span data-ttu-id="34e43-116">Más szóval hello kell olyan céggel kapcsolatos véleményeket analysis elemzés, amely a közösségi média hírcsatorna alapul.</span><span class="sxs-lookup"><span data-stu-id="34e43-116">In other words, hello need is a sentiment analysis analytics engine that's based on this social media feed.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34e43-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="34e43-117">Prerequisites</span></span>
<span data-ttu-id="34e43-118">Ebben az oktatóanyagban egy ügyfélalkalmazást, amely tooTwitter kapcsolódik, és megkeresi a Twitter-üzeneteket, amelyek bizonyos hashtageket (amely állíthatja be) használja.</span><span class="sxs-lookup"><span data-stu-id="34e43-118">In this tutorial, you use a client application that connects tooTwitter and looks for tweets that have certain hashtags (which you can set).</span></span> <span data-ttu-id="34e43-119">Ahhoz, toorun alkalmazás hello és elemezheti Azure Streaming Analytics segítségével Twitter-üzeneteket, rendelkeznie kell hello következő hello:</span><span class="sxs-lookup"><span data-stu-id="34e43-119">In order toorun hello application and analyze hello tweets using Azure Streaming Analytics, you must have hello following:</span></span>

* <span data-ttu-id="34e43-120">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="34e43-120">An Azure subscription</span></span>
* <span data-ttu-id="34e43-121">Twitter-fiók</span><span class="sxs-lookup"><span data-stu-id="34e43-121">A Twitter account</span></span> 
* <span data-ttu-id="34e43-122">Egy Twitter-alkalmazás, és hello [OAuth jogkivonat](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) az adott alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="34e43-122">A Twitter application, and hello [OAuth access token](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) for that application.</span></span> <span data-ttu-id="34e43-123">Magas szintű című cikkben talál útmutatást nyújtunk toocreate később egy Twitter-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="34e43-123">We provide high-level instructions for how toocreate a Twitter application later.</span></span>
* <span data-ttu-id="34e43-124">hello TwitterWPFClient alkalmazás, amelyben a hello Twitter hírcsatorna.</span><span class="sxs-lookup"><span data-stu-id="34e43-124">hello TwitterWPFClient application, which reads hello Twitter feed.</span></span> <span data-ttu-id="34e43-125">tooget az alkalmazás, a letöltési hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) a Githubról fájlt, és ezután bontsa ki a hello csomagot egy mappába a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="34e43-125">tooget this application, download hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) file from GitHub and then unzip hello package into a folder on your computer.</span></span> <span data-ttu-id="34e43-126">Ha azt szeretné, hogy toosee hello forráskódot, és hibakereső hello alkalmazást futtat, kaphat forráskódjának hello [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator).</span><span class="sxs-lookup"><span data-stu-id="34e43-126">If you want toosee hello source code and run hello application in a debugger, you can get hello source code from [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator).</span></span> 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a><span data-ttu-id="34e43-127">Létrehoz egy eseményközpontot Streaming Analytics bevitelhez</span><span class="sxs-lookup"><span data-stu-id="34e43-127">Create an event hub for Streaming Analytics input</span></span>

<span data-ttu-id="34e43-128">hello mintaalkalmazás eseményeket hoz létre, és tooan Azure event hubs leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="34e43-128">hello sample application generates events and pushes them tooan Azure event hub.</span></span> <span data-ttu-id="34e43-129">Az Azure event hubs esemény adatfeldolgozást a Stream Analytics hello előnyben részesített módja.</span><span class="sxs-lookup"><span data-stu-id="34e43-129">Azure event hubs are hello preferred method of event ingestion for Stream Analytics.</span></span> <span data-ttu-id="34e43-130">További információkért lásd: hello [Azure Event Hubs dokumentáció](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="34e43-130">For more information, see hello [Azure Event Hubs documentation](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>


### <a name="create-an-event-hub-namespace-and-event-hub"></a><span data-ttu-id="34e43-131">Event hub névtér és eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="34e43-131">Create an event hub namespace and event hub</span></span>
<span data-ttu-id="34e43-132">Ebben az eljárásban először létre kell hoznia egy event hub névtér, és ezután hozzáadhat egy event hub toothat névtér.</span><span class="sxs-lookup"><span data-stu-id="34e43-132">In this procedure, you first create an event hub namespace, and then you add an event hub toothat namespace.</span></span> <span data-ttu-id="34e43-133">Event hub névterek toologically csoport kapcsolódó esemény bus példányok.</span><span class="sxs-lookup"><span data-stu-id="34e43-133">Event hub namespaces are used toologically group related event bus instances.</span></span> 

1. <span data-ttu-id="34e43-134">Jelentkezzen be Azure-portálon toohello, és kattintson **új** > **az eszközök internetes hálózatát** > **Eseményközpont**.</span><span class="sxs-lookup"><span data-stu-id="34e43-134">Log  in toohello Azure portal and click **New** > **Internet of Things** > **Event Hub**.</span></span> 

2. <span data-ttu-id="34e43-135">A hello **névtér létrehozása** panelen adja meg például a névtér nevét `<yourname>-socialtwitter-eh-ns`.</span><span class="sxs-lookup"><span data-stu-id="34e43-135">In hello **Create namespace** blade, enter a namespace name such as `<yourname>-socialtwitter-eh-ns`.</span></span> <span data-ttu-id="34e43-136">Bármilyen név hello névtér esetében is használhatja, de hello nevének kell lennie egy URL-cím érvényes, és Azure között egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="34e43-136">You can use any name for hello namespace, but hello name must be valid for a URL and it must be unique across Azure.</span></span> 
    
3. <span data-ttu-id="34e43-137">Válasszon egy előfizetést, és hozzon létre vagy válasszon egy erőforráscsoportot, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="34e43-137">Select a subscription and create or choose a resource group, then click **Create**.</span></span> 

    ![Az event hub névtér létrehozása](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. <span data-ttu-id="34e43-139">Hello névtér telepítése befejeződött, található hello event hub névtér elemre az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="34e43-139">When hello namespace has finished deploying, find hello event hub namespace in your list of Azure resources.</span></span> 

5. <span data-ttu-id="34e43-140">Kattintson az új névtér hello, és hello névtér paneljén kattintson  **+ &nbsp;Eseményközpont**.</span><span class="sxs-lookup"><span data-stu-id="34e43-140">Click hello new namespace, and in hello namespace blade, click **+&nbsp;Event Hub**.</span></span> 

    ![<span data-ttu-id="34e43-141">hello Eseményközpont hozzáadása gombra egy új eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="34e43-141">hello Add Event Hub button for creating a new event hub</span></span> ](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. <span data-ttu-id="34e43-142">Név hello új eseményközpont `socialtwitter-eh`.</span><span class="sxs-lookup"><span data-stu-id="34e43-142">Name hello new event hub `socialtwitter-eh`.</span></span> <span data-ttu-id="34e43-143">Használhat egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="34e43-143">You can use a different name.</span></span> <span data-ttu-id="34e43-144">Ha így tesz, jegyezze fel, mert később szüksége hello nevét.</span><span class="sxs-lookup"><span data-stu-id="34e43-144">If you do, make a note of it, because you need hello name later.</span></span> <span data-ttu-id="34e43-145">Hello event hub tooset más beállításokat nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="34e43-145">You don't need tooset any other options for hello event hub.</span></span>

    ![Egy új eseményközpont létrehozása panel](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. <span data-ttu-id="34e43-147">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="34e43-147">Click **Create**.</span></span>


### <a name="grant-access-toohello-event-hub"></a><span data-ttu-id="34e43-148">Adjon hozzáférést toohello event hubs</span><span class="sxs-lookup"><span data-stu-id="34e43-148">Grant access toohello event hub</span></span>

<span data-ttu-id="34e43-149">A folyamat küldhet adatokat tooan eseményközpont, hello eseményközpont olyan házirendet, amely lehetővé teszi, hogy a megfelelő hozzáférési kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="34e43-149">Before a process can send data tooan event hub, hello event hub must have a policy that allows appropriate access.</span></span> <span data-ttu-id="34e43-150">hello hozzáférési házirendet hoz létre egy kapcsolati karakterláncot, amely tartalmazza az engedélyezési adatok.</span><span class="sxs-lookup"><span data-stu-id="34e43-150">hello access policy produces a connection string that includes authorization information.</span></span>

1.  <span data-ttu-id="34e43-151">Hello esemény névtér paneljén kattintson **Event Hubs** , és kattintson az új eseményközpont hello nevére.</span><span class="sxs-lookup"><span data-stu-id="34e43-151">In hello event namespace blade, click **Event Hubs** and then click hello name of your new event hub.</span></span>

2.  <span data-ttu-id="34e43-152">Hello event hub paneljén kattintson **megosztott elérési házirendek** majd  **+ &nbsp;Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="34e43-152">In hello event hub blade, click **Shared access policies** and then click **+&nbsp;Add**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="34e43-153">Ellenőrizze, hogy hello eseményközpont, nem hello event hub névtér dolgozik.</span><span class="sxs-lookup"><span data-stu-id="34e43-153">Make sure you're working with hello event hub, not hello event hub namespace.</span></span>

3.  <span data-ttu-id="34e43-154">Nevű házirend hozzáadása `socialtwitter-access` és **jogcím**, jelölje be **kezelése**.</span><span class="sxs-lookup"><span data-stu-id="34e43-154">Add a policy named `socialtwitter-access` and for **Claim**, select **Manage**.</span></span>

    ![Az új esemény központi hozzáférési házirend létrehozása panel](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  <span data-ttu-id="34e43-156">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="34e43-156">Click **Create**.</span></span>

5.  <span data-ttu-id="34e43-157">Hello házirend központi telepítése után kattintson rá a megosztott elérési házirendek hello listája.</span><span class="sxs-lookup"><span data-stu-id="34e43-157">After hello policy has been deployed, click it in hello list of shared access policies.</span></span>

6.  <span data-ttu-id="34e43-158">Keresés hello jelölőnégyzetet **KAPCSOLATI karakterlánc elsődleges kulcs** hello Másolás gombra következő toohello kapcsolati karakterláncot, majd.</span><span class="sxs-lookup"><span data-stu-id="34e43-158">Find hello box labeled **CONNECTION STRING-PRIMARY KEY** and click hello copy button next toohello connection string.</span></span> 
    
    ![Hello házirend hello elsődleges kapcsolódási karakterlánc kulcs másolása](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  <span data-ttu-id="34e43-160">Hello kapcsolati karakterlánc illessze be a szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="34e43-160">Paste hello connection string into a text editor.</span></span> <span data-ttu-id="34e43-161">Szükség van a kapcsolati karakterlánc hello a következő szakaszban néhány kisebb módosításokat tooit elvégzése után.</span><span class="sxs-lookup"><span data-stu-id="34e43-161">You need this connection string for hello next section, after you make some small edits tooit.</span></span>

    <span data-ttu-id="34e43-162">hello kapcsolati karakterlánc így néz ki:</span><span class="sxs-lookup"><span data-stu-id="34e43-162">hello connection string looks like this:</span></span>

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    <span data-ttu-id="34e43-163">Figyelje meg, hogy hello kapcsolati karakterláncot tartalmaz-e több kulcs-érték párok, pontosvesszővel elválasztva: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, és `EntityPath`.</span><span class="sxs-lookup"><span data-stu-id="34e43-163">Notice that hello connection string contains multiple key-value pairs, separated with semicolons: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, and `EntityPath`.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="34e43-164">Biztonsági okokból hello kapcsolati karakterlánc hello példában részei el lettek távolítva.</span><span class="sxs-lookup"><span data-stu-id="34e43-164">For security, parts of hello connection string in hello example have been removed.</span></span>

8.  <span data-ttu-id="34e43-165">Hello szövegszerkesztőben, távolítsa el a hello `EntityPath` pár hello kapcsolati karakterlánc (ne feledje azt megelőző tooremove hello pontosvessző).</span><span class="sxs-lookup"><span data-stu-id="34e43-165">In hello text editor, remove hello `EntityPath` pair from hello connection string (don't forget tooremove hello semicolon that precedes it).</span></span> <span data-ttu-id="34e43-166">Amikor elkészült, akkor a kapcsolati karakterlánc hello néz ki:</span><span class="sxs-lookup"><span data-stu-id="34e43-166">When you're done, hello connection string looks like this:</span></span>

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-hello-twitter-client-application"></a><span data-ttu-id="34e43-167">Konfigurálni és elindítani a hello Twitter-ügyfélalkalmazás</span><span class="sxs-lookup"><span data-stu-id="34e43-167">Configure and start hello Twitter client application</span></span>
<span data-ttu-id="34e43-168">hello ügyfélalkalmazás tweetet események lekérése Twitter-ről.</span><span class="sxs-lookup"><span data-stu-id="34e43-168">hello client application gets tweet events directly from Twitter.</span></span> <span data-ttu-id="34e43-169">A rendezés toodo tette, engedély toocall hello Streamelési API-k Twitter kell.</span><span class="sxs-lookup"><span data-stu-id="34e43-169">In order toodo so, it needs permission toocall hello Twitter Streaming APIs.</span></span> <span data-ttu-id="34e43-170">tooconfigure, hogy engedélyt, létrehoz egy alkalmazást a Twitter, amely egyedi hitelesítő adataikat (például az OAuth jogkivonat) hoz létre.</span><span class="sxs-lookup"><span data-stu-id="34e43-170">tooconfigure that permission, you create an application in Twitter, which generates unique credentials (such as an OAuth token).</span></span> <span data-ttu-id="34e43-171">Konfigurálhatja hello ügyfél alkalmazás toouse ezeket a hitelesítő adatokat, amikor az API-hívásokban teszi.</span><span class="sxs-lookup"><span data-stu-id="34e43-171">You can then configure hello client application toouse these credentials when it makes API calls.</span></span> 

### <a name="create-a-twitter-application"></a><span data-ttu-id="34e43-172">Twitter-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="34e43-172">Create a Twitter application</span></span>
<span data-ttu-id="34e43-173">Ha még nem rendelkezik egy Twitter-alkalmazás, amely ehhez az oktatóanyaghoz használható, akkor hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="34e43-173">If you do not already have a Twitter application that you can use for this tutorial, you can create one.</span></span> <span data-ttu-id="34e43-174">Már rendelkeznie kell egy Twitter-fiók.</span><span class="sxs-lookup"><span data-stu-id="34e43-174">You must already have a Twitter account.</span></span>

> [!NOTE]
> <span data-ttu-id="34e43-175">az alkalmazás létrehozása és hello kulcsokat, a titkos kulcsok és a token első Twitter hello pontos folyamat változhatnak.</span><span class="sxs-lookup"><span data-stu-id="34e43-175">hello exact process in Twitter for creating an application and getting hello keys, secrets, and token might change.</span></span> <span data-ttu-id="34e43-176">Ha ezek az utasítások nem egyeznek meg, tekintse meg hello Twitter hely, tekintse meg a toohello Twitter fejlesztői dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="34e43-176">If these instructions don't match what you see on hello Twitter site, refer toohello Twitter developer documentation.</span></span>

1. <span data-ttu-id="34e43-177">Nyissa meg toohello [Twitter alkalmazás kezelése lap](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="34e43-177">Go toohello [Twitter application management page](https://apps.twitter.com/).</span></span> 

2. <span data-ttu-id="34e43-178">Hozzon létre egy új alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="34e43-178">Create a new application.</span></span> 

    * <span data-ttu-id="34e43-179">Hello webhely URL-címe adja meg egy érvényes URL-címet.</span><span class="sxs-lookup"><span data-stu-id="34e43-179">For hello website URL, specify a valid URL.</span></span> <span data-ttu-id="34e43-180">Nincs toobe élő webhelyet.</span><span class="sxs-lookup"><span data-stu-id="34e43-180">It does not have toobe a live site.</span></span> <span data-ttu-id="34e43-181">(Nem lehet csak megadni `localhost`.)</span><span class="sxs-lookup"><span data-stu-id="34e43-181">(You can't specify just `localhost`.)</span></span>
    * <span data-ttu-id="34e43-182">Hello visszahívási mezőt hagyja üresen.</span><span class="sxs-lookup"><span data-stu-id="34e43-182">Leave hello callback field blank.</span></span> <span data-ttu-id="34e43-183">Ebben az oktatóanyagban használt hello ügyfélalkalmazás visszahívások nem igényel.</span><span class="sxs-lookup"><span data-stu-id="34e43-183">hello client application you use for this tutorial doesn't require callbacks.</span></span>

    ![Az alkalmazás létrehozása a Twitteren](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. <span data-ttu-id="34e43-185">Igény szerint megváltoztathatja csak tooread hello alkalmazás engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="34e43-185">Optionally, change hello application's permissions tooread-only.</span></span>

4. <span data-ttu-id="34e43-186">Hello alkalmazás létrehozásakor Ugrás toohello **kulcsok és a hozzáférési jogkivonatok** lap.</span><span class="sxs-lookup"><span data-stu-id="34e43-186">When hello application is created, go toohello **Keys and Access Tokens** page.</span></span>

5. <span data-ttu-id="34e43-187">Kattintson a hello gomb toogenerate olyan hozzáférési jogkivonatot, és a hozzáférési jogkivonat titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="34e43-187">Click hello button toogenerate an access token and access token secret.</span></span>

<span data-ttu-id="34e43-188">Tartani lesz szüksége, ezt az információt, mivel a következő eljárásban hello szüksége lesz rá.</span><span class="sxs-lookup"><span data-stu-id="34e43-188">Keep this information handy, because you will need it in hello next procedure.</span></span>

>[!NOTE]
><span data-ttu-id="34e43-189">hello kulcsok és titkos hello Twitter-alkalmazás biztosítanak hozzáférést tooyour Twitter-fiók.</span><span class="sxs-lookup"><span data-stu-id="34e43-189">hello keys and secrets for hello Twitter application provide access tooyour Twitter account.</span></span> <span data-ttu-id="34e43-190">Ez az információ tekinti érzékeny, hello ugyanaz, mint a Twitter-jelszó.</span><span class="sxs-lookup"><span data-stu-id="34e43-190">Treat this information as sensitive, hello same as you do your Twitter password.</span></span> <span data-ttu-id="34e43-191">Például nincs beágyazás ezt az információt az alkalmazás tooothers rendelt.</span><span class="sxs-lookup"><span data-stu-id="34e43-191">For example, don't embed this information in an application that you give tooothers.</span></span> 


### <a name="configure-hello-client-application"></a><span data-ttu-id="34e43-192">Hello ügyfélalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="34e43-192">Configure hello client application</span></span>
<span data-ttu-id="34e43-193">Létrehoztunk Önnek egy ügyfélalkalmazást, amely összeköti a tooTwitter használatával végzett [Twitter a Streamelési API-k](https://dev.twitter.com/streaming/overview) toocollect tweetet események kapcsolatos témakörök egy adott készletét.</span><span class="sxs-lookup"><span data-stu-id="34e43-193">We've created a client application that connects tooTwitter data using [Twitter's Streaming APIs](https://dev.twitter.com/streaming/overview) toocollect tweet events about a specific set of topics.</span></span> <span data-ttu-id="34e43-194">hello alkalmazás használ hello [Sentiment140](http://help.sentiment140.com/) nyílt forráskódú eszköz, amely rendeli hozzá a következő véleményeket érték tooeach tweetet hello:</span><span class="sxs-lookup"><span data-stu-id="34e43-194">hello application uses hello [Sentiment140](http://help.sentiment140.com/) open source tool, which assigns hello following sentiment value tooeach tweet:</span></span>

* <span data-ttu-id="34e43-195">0 = negatív.</span><span class="sxs-lookup"><span data-stu-id="34e43-195">0 = negative</span></span>
* <span data-ttu-id="34e43-196">2 = neutral</span><span class="sxs-lookup"><span data-stu-id="34e43-196">2 = neutral</span></span>
* <span data-ttu-id="34e43-197">4 = pozitív</span><span class="sxs-lookup"><span data-stu-id="34e43-197">4 = positive</span></span>

<span data-ttu-id="34e43-198">Hello tweetet események véleményeket érték van rendelve, miután azok elküldte azokat korábban létrehozott eseményközpont toohello.</span><span class="sxs-lookup"><span data-stu-id="34e43-198">After hello tweet events have been assigned a sentiment value, they are pushed toohello event hub that you created earlier.</span></span>

<span data-ttu-id="34e43-199">Mielőtt hello alkalmazás fut, egyes adatok, például a hello Twitter kulcsok és a hello event hub kapcsolati karakterlánc van szükség.</span><span class="sxs-lookup"><span data-stu-id="34e43-199">Before hello application runs, it requires certain information from you, like hello Twitter keys and hello event hub connection string.</span></span> <span data-ttu-id="34e43-200">Megadhatja, hogy hello konfigurációs adatait az alábbi módszerek:</span><span class="sxs-lookup"><span data-stu-id="34e43-200">You can provide hello configuration information in these ways:</span></span>

* <span data-ttu-id="34e43-201">Hello alkalmazás futtatásához, és aztán hello alkalmazás felhasználói felületén tooenter hello kulcsok, titkok és kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="34e43-201">Run hello application, and then use hello application's UI tooenter hello keys, secrets, and connection string.</span></span> <span data-ttu-id="34e43-202">Ha így tesz, hello konfigurációs adatok az aktuális munkamenetre, de nem menti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="34e43-202">If you do this, hello configuration information is used for your current session, but it isn't saved.</span></span>
* <span data-ttu-id="34e43-203">Hello alkalmazás .config fájl és a set hello értékeket szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="34e43-203">Edit hello application's .config file and set hello values there.</span></span> <span data-ttu-id="34e43-204">Ez a módszer továbbra is fennáll, hello konfigurációs adatokat, de azt is jelenti, hogy a bizalmas adatokat a számítógép egyszerű szövegként vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="34e43-204">This approach persists hello configuration information, but it also means that this potentially sensitive information is stored in plain text on your computer.</span></span>

<span data-ttu-id="34e43-205">hello következő eljárás dokumentumok mindkét megközelítés.</span><span class="sxs-lookup"><span data-stu-id="34e43-205">hello following procedure documents both approaches.</span></span> 

1. <span data-ttu-id="34e43-206">Győződjön meg arról, hogy a letöltött és unzipped hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) alkalmazás, a hello előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="34e43-206">Make sure you've downloaded and unzipped hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) application, as listed in hello prerequisites.</span></span>

2. <span data-ttu-id="34e43-207">tooset hello értékek futási időben (és csak az aktuális munkamenet hello), futtassa a hello `TwitterWPFClient.exe` alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="34e43-207">tooset hello values at run time (and only for hello current session), run hello `TwitterWPFClient.exe` application.</span></span> <span data-ttu-id="34e43-208">Amikor a hello alkalmazás kéri, adja meg a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="34e43-208">When hello application prompts you, enter hello following values:</span></span>

    * <span data-ttu-id="34e43-209">hello Twitter kulcsa (API-kulcsot).</span><span class="sxs-lookup"><span data-stu-id="34e43-209">hello Twitter Consumer Key (API Key).</span></span>
    * <span data-ttu-id="34e43-210">hello Twitter felhasználói titok (API titok).</span><span class="sxs-lookup"><span data-stu-id="34e43-210">hello Twitter Consumer Secret (API Secret).</span></span>
    * <span data-ttu-id="34e43-211">hello Twitter hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="34e43-211">hello Twitter Access Token.</span></span>
    * <span data-ttu-id="34e43-212">hello Twitter Access Token titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="34e43-212">hello Twitter Access Token Secret.</span></span>
    * <span data-ttu-id="34e43-213">hello kapcsolati karakterlánc információ korábban mentett.</span><span class="sxs-lookup"><span data-stu-id="34e43-213">hello connection string information that you saved earlier.</span></span> <span data-ttu-id="34e43-214">Győződjön meg arról, hogy eltávolította a hello hello kapcsolati karakterláncot használó `EntityPath` a kulcs-érték pár.</span><span class="sxs-lookup"><span data-stu-id="34e43-214">Make sure that you use hello connection string that you removed hello `EntityPath` key-value pair from.</span></span>
    * <span data-ttu-id="34e43-215">hello Twitter kulcsszavak, amelyet a toodetermine céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="34e43-215">hello Twitter keywords that you want toodetermine sentiment for.</span></span>

   ![TwitterWpfClient alkalmazás fut, melyen rejtett beállítások](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. <span data-ttu-id="34e43-217">tooset hello értékek folyamatosan fennáll, használja a szerkesztő tooopen hello TwitterWpfClient.exe.config szövegfájlba.</span><span class="sxs-lookup"><span data-stu-id="34e43-217">tooset hello values persistently, use a text editor tooopen hello TwitterWpfClient.exe.config file.</span></span> <span data-ttu-id="34e43-218">Ezt a hello `<appSettings>` elem, ehhez:</span><span class="sxs-lookup"><span data-stu-id="34e43-218">Then in hello `<appSettings>` element, do this:</span></span>

    * <span data-ttu-id="34e43-219">Állítsa be `oauth_consumer_key` toohello Twitter kulcsa (API-kulcsot).</span><span class="sxs-lookup"><span data-stu-id="34e43-219">Set `oauth_consumer_key` toohello Twitter Consumer Key (API Key).</span></span> 
    * <span data-ttu-id="34e43-220">Állítsa be `oauth_consumer_secret` toohello Twitter felhasználói titok (API titok).</span><span class="sxs-lookup"><span data-stu-id="34e43-220">Set `oauth_consumer_secret` toohello Twitter Consumer Secret (API Secret).</span></span>
    * <span data-ttu-id="34e43-221">Állítsa be `oauth_token` toohello Twitter hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="34e43-221">Set `oauth_token` toohello Twitter Access Token.</span></span>
    * <span data-ttu-id="34e43-222">Állítsa be `oauth_token_secret` toohello Twitter Access Token titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="34e43-222">Set `oauth_token_secret` toohello Twitter Access Token Secret.</span></span>

    <span data-ttu-id="34e43-223">Később a hello `<appSettings>` elem, ezeket a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="34e43-223">Later in hello `<appSettings>` element, make these changes:</span></span>

    * <span data-ttu-id="34e43-224">Állítsa be `EventHubName` toohello eseményközpont neveként (Ez azt jelenti, hogy toohello értéke hello entitás elérési út).</span><span class="sxs-lookup"><span data-stu-id="34e43-224">Set `EventHubName` toohello event hub name (that is, toohello value of hello entity path).</span></span>
    * <span data-ttu-id="34e43-225">Állítsa be `EventHubNameConnectionString` toohello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="34e43-225">Set `EventHubNameConnectionString` toohello connection string.</span></span> <span data-ttu-id="34e43-226">Győződjön meg arról, hogy eltávolította a hello hello kapcsolati karakterláncot használó `EntityPath` a kulcs-érték pár.</span><span class="sxs-lookup"><span data-stu-id="34e43-226">Make sure that you use hello connection string that you removed hello `EntityPath` key-value pair from.</span></span>

    <span data-ttu-id="34e43-227">Hello `<appSettings>` szakasz a következő példa hello tűnik.</span><span class="sxs-lookup"><span data-stu-id="34e43-227">hello `<appSettings>` section looks like hello following example.</span></span> <span data-ttu-id="34e43-228">(Az érthetőség és a biztonsági, azt becsomagolt a sorokat és eltávolítani néhány karaktert.)</span><span class="sxs-lookup"><span data-stu-id="34e43-228">(For clarity and security, we wrapped some lines and removed some characters.)</span></span>

    ![TwitterWpfClient alkalmazás konfigurációs fájlt egy szövegszerkesztőben, hello Twitter kulcsok és titkos kulcsok és hello események központ kapcsolati karakterlánc adatait megjelenítő](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. <span data-ttu-id="34e43-230">Ha már hello alkalmazás indította el, TwitterWpfClient.exe most kell futtatni.</span><span class="sxs-lookup"><span data-stu-id="34e43-230">If you didn't already start hello application, run TwitterWpfClient.exe now.</span></span> 

5. <span data-ttu-id="34e43-231">Kattintson a hello zöld start gomb toocollect közösségi céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="34e43-231">Click hello green start button toocollect social sentiment.</span></span> <span data-ttu-id="34e43-232">A hello Tweetet eseményeket **CreatedAt**, **témakör**, és **SentimentScore** értékek tooyour eseményközpont küldi el.</span><span class="sxs-lookup"><span data-stu-id="34e43-232">You see Tweet events with hello **CreatedAt**, **Topic**, and **SentimentScore** values being sent tooyour event hub.</span></span>

    ![TwitterWpfClient alkalmazás fut, a Twitter-üzenetek listáját ábrázoló](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    ><span data-ttu-id="34e43-234">Ha hibába ütközik, és a Twitter-üzeneteket hello hello ablakának alsó részén megjelennek az adatfolyam nem jelenik meg, gondosan ellenőrizze a hello kulcsok és titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="34e43-234">If you see errors, and you don't see a stream of tweets displayed in hello lower part of hello window, double-check hello keys and secrets.</span></span> <span data-ttu-id="34e43-235">Jelölniük hello kapcsolati karakterlánc (Győződjön meg arról, hogy nem tartalmazza a hello `EntityPath` kulcs-érték.)</span><span class="sxs-lookup"><span data-stu-id="34e43-235">Also check hello connection string (make sure that it does not include hello `EntityPath` key and value.)</span></span>


## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="34e43-236">Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="34e43-236">Create a Stream Analytics job</span></span>

<span data-ttu-id="34e43-237">Most, hogy a rendszer valós időben Twitterről a adatfolyam-tweetet események, beállíthat egy Stream Analytics-feladat tooanalyze ezeket az eseményeket valós időben.</span><span class="sxs-lookup"><span data-stu-id="34e43-237">Now that tweet events are streaming in real time from Twitter, you can set up a Stream Analytics job tooanalyze these events in real time.</span></span>

1. <span data-ttu-id="34e43-238">Hello Azure-portálon, kattintson **új** > **az eszközök internetes hálózatát** > **Stream Analytics-feladat**.</span><span class="sxs-lookup"><span data-stu-id="34e43-238">In hello Azure portal, click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>

2. <span data-ttu-id="34e43-239">Név hello feladat `socialtwitter-sa-job` , és adja meg egy előfizetési, erőforráscsoportot és helyet.</span><span class="sxs-lookup"><span data-stu-id="34e43-239">Name hello job `socialtwitter-sa-job` and specify a subscription, resource group, and location.</span></span>

    <span data-ttu-id="34e43-240">Egy jó ötlet tooplace hello feladat és hello eseményközpont a hello ugyanabban a régióban a legjobb teljesítmény és, hogy nem kell fizetnie tootransfer adatok régiók között.</span><span class="sxs-lookup"><span data-stu-id="34e43-240">It's a good idea tooplace hello job and hello event hub in hello same region for best performance and so that you don't pay tootransfer data between regions.</span></span>

    ![Egy új Stream Analytics-feladat létrehozása](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. <span data-ttu-id="34e43-242">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="34e43-242">Click **Create**.</span></span>

    <span data-ttu-id="34e43-243">hello feladat jön létre, és hello portál megjeleníti a feladat részleteit.</span><span class="sxs-lookup"><span data-stu-id="34e43-243">hello job is created and hello portal displays job details.</span></span>


## <a name="specify-hello-job-input"></a><span data-ttu-id="34e43-244">Adjon meg hello feladat bemeneti</span><span class="sxs-lookup"><span data-stu-id="34e43-244">Specify hello job input</span></span>

1. <span data-ttu-id="34e43-245">A Stream Analytics-feladat, a alatt **feladat topológia** hello középső hello feladat panelen, kattintson a **bemenetek**.</span><span class="sxs-lookup"><span data-stu-id="34e43-245">In your Stream Analytics job, under **Job Topology** in hello middle of hello job blade, click **Inputs**.</span></span> 

2. <span data-ttu-id="34e43-246">A hello **bemenetek** panelen kattintson  **+ &nbsp;Hozzáadás** hello panel ezekkel az értékekkel, majd töltse ki:</span><span class="sxs-lookup"><span data-stu-id="34e43-246">In hello **Inputs** blade, click **+&nbsp;Add** and then fill out hello blade with these values:</span></span>

    * <span data-ttu-id="34e43-247">**A bemeneti alias**: hello név használata `TwitterStream`.</span><span class="sxs-lookup"><span data-stu-id="34e43-247">**Input alias**: Use hello name `TwitterStream`.</span></span> <span data-ttu-id="34e43-248">Ha más nevet használjon, jegyezze fel a, mert később szüksége.</span><span class="sxs-lookup"><span data-stu-id="34e43-248">If you use a different name, make a note of it because you need it later.</span></span>
    * <span data-ttu-id="34e43-249">**Adatforrás típusa**: válasszon **adatfolyam**.</span><span class="sxs-lookup"><span data-stu-id="34e43-249">**Source type**: Select **Data stream**.</span></span>
    * <span data-ttu-id="34e43-250">**Forrás**: válasszon **eseményközpont**.</span><span class="sxs-lookup"><span data-stu-id="34e43-250">**Source**: Select **Event hub**.</span></span>
    * <span data-ttu-id="34e43-251">**Beállítás importálása**: válasszon **eseményközpont használható a jelenlegi előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="34e43-251">**Import option**: Select **Use event hub from current subscription**.</span></span> 
    * <span data-ttu-id="34e43-252">**Service bus-névtér**: Jelölje ki a korábban létrehozott hello esemény hub névteret (`<yourname>-socialtwitter-eh-ns`).</span><span class="sxs-lookup"><span data-stu-id="34e43-252">**Service bus namespace**: Select hello event hub namespace that you created earlier (`<yourname>-socialtwitter-eh-ns`).</span></span>
    * <span data-ttu-id="34e43-253">**Az Event hubs**: korábban létrehozott eseményközpont válassza hello (`socialtwitter-eh`).</span><span class="sxs-lookup"><span data-stu-id="34e43-253">**Event hub**: Select hello event hub that you created earlier (`socialtwitter-eh`).</span></span>
    * <span data-ttu-id="34e43-254">**Eseményközpont házirend neve**: válassza ki a korábban létrehozott hello hozzáférési házirend (`socialtwitter-access`).</span><span class="sxs-lookup"><span data-stu-id="34e43-254">**Event hub policy name**: Select hello access policy that you created earlier (`socialtwitter-access`).</span></span>

    ![Új bemeneti Streaming Analytics-feladat létrehozása](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. <span data-ttu-id="34e43-256">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="34e43-256">Click **Create**.</span></span>


## <a name="specify-hello-job-query"></a><span data-ttu-id="34e43-257">Hello feladat-lekérdezés megadása</span><span class="sxs-lookup"><span data-stu-id="34e43-257">Specify hello job query</span></span>

<span data-ttu-id="34e43-258">A Stream Analytics egy egyszerű, deklaratív lekérdezési modellel, átalakítások leíró támogatja.</span><span class="sxs-lookup"><span data-stu-id="34e43-258">Stream Analytics supports a simple, declarative query model that describes transformations.</span></span> <span data-ttu-id="34e43-259">toolearn hello nyelvi kapcsolatos további információkért lásd: hello [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span><span class="sxs-lookup"><span data-stu-id="34e43-259">toolearn more about hello language, see hello [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span></span>  <span data-ttu-id="34e43-260">Ez az oktatóanyag segítséget nyújt a szerzői és több lekérdezések tesztelése Twitter-adatok.</span><span class="sxs-lookup"><span data-stu-id="34e43-260">This tutorial helps you author and test several queries over Twitter data.</span></span>

<span data-ttu-id="34e43-261">megjegyzések között témakörök toocompare hello száma, használhatja a [Átfedésmentes ablak](https://msdn.microsoft.com/library/azure/dn835055.aspx) megjegyzések által témakör öt másodpercenként tooget hello száma.</span><span class="sxs-lookup"><span data-stu-id="34e43-261">toocompare hello number of mentions among topics, you can use a [Tumbling window](https://msdn.microsoft.com/library/azure/dn835055.aspx) tooget hello count of mentions by topic every five seconds.</span></span>

1. <span data-ttu-id="34e43-262">Bezárás hello **bemenetek** panelt, ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="34e43-262">Close hello **Inputs** blade if you haven't already.</span></span>

2. <span data-ttu-id="34e43-263">Hello feladat panelen, kattintson a hello **lekérdezés** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="34e43-263">In hello job blade, click hello **Query** box.</span></span> <span data-ttu-id="34e43-264">Azure hello bemenetek sorolja fel, és amelyet hello feladat van konfigurálva, és lehetővé teszi, hogy hozzon létre egy lekérdezést, amely lehetővé teszi hello bemeneti adatfolyam átalakítására, toohello kimeneti keresztül küldött.</span><span class="sxs-lookup"><span data-stu-id="34e43-264">Azure lists hello inputs and outputs that are configured for hello job, and lets you create a query that lets you transform hello input stream as it is sent toohello output.</span></span>

3. <span data-ttu-id="34e43-265">Győződjön meg arról, hogy hello TwitterWpfClient alkalmazás fut-e.</span><span class="sxs-lookup"><span data-stu-id="34e43-265">Make sure that hello TwitterWpfClient application is running.</span></span> 

3. <span data-ttu-id="34e43-266">A hello **lekérdezés** panelen kattintson hello pontokból következő toohello `TwitterStream` adja meg, és válassza ki **az adatokat a bemeneti**.</span><span class="sxs-lookup"><span data-stu-id="34e43-266">In hello **Query** blade, click hello dots next toohello `TwitterStream` input and then select **Sample data from input**.</span></span>

    ![Menü Beállítások toouse mintaadatainak hello Streaming Analytics-feladat bejegyzést, mintaadatokkal"bemeneti" kijelölve](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    <span data-ttu-id="34e43-268">Ekkor megnyílik egy panel, amely lehetővé teszi, hogy mennyi minta adatok tooget, mennyi ideig tooread hello bemeneti adatfolyam definiált adja meg.</span><span class="sxs-lookup"><span data-stu-id="34e43-268">This opens a blade that lets you specify how much sample data tooget, defined in terms of how long tooread hello input stream.</span></span>

4. <span data-ttu-id="34e43-269">Állítsa be **perc** too3 majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="34e43-269">Set **Minutes** too3 and then click **OK**.</span></span> 
    
    ![Mintavételi hello bemeneti adatfolyam, a "3 perc" kiválasztott beállítások.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    <span data-ttu-id="34e43-271">Azure 3 percet értékelésével hello bemeneti adatfolyamból minták, és értesítést küld, amikor készen áll a mintaadatok hello.</span><span class="sxs-lookup"><span data-stu-id="34e43-271">Azure samples 3 minutes' worth of data from hello input stream and notifies you when hello sample data is ready.</span></span> <span data-ttu-id="34e43-272">(Ez időt vesz igénybe egy rövid ideig.)</span><span class="sxs-lookup"><span data-stu-id="34e43-272">(This takes a short while.)</span></span> 

    <span data-ttu-id="34e43-273">hello mintaadatok ideiglenesen tárolja, és áll rendelkezésre, míg a hello lekérdezés ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="34e43-273">hello sample data is stored temporarily and is available while you have hello query window open.</span></span> <span data-ttu-id="34e43-274">Ha bezárja a hello lekérdezési ablakba, hello minta adatait a rendszer törli, és el kell toocreate mintaadatok új készletét.</span><span class="sxs-lookup"><span data-stu-id="34e43-274">If you close hello query window, hello sample data is discarded, and you have toocreate a new set of sample data.</span></span> 

5. <span data-ttu-id="34e43-275">Hello lekérdezés hello kód szerkesztő toohello alábbi módosítása:</span><span class="sxs-lookup"><span data-stu-id="34e43-275">Change hello query in hello code editor toohello following:</span></span>

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    <span data-ttu-id="34e43-276">Ha nem a `TwitterStream` hello alias hello bevitellel, mint helyettesítse be az aliast, a `TwitterStream` hello lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="34e43-276">If didn't use `TwitterStream` as hello alias for hello input, substitute your alias for `TwitterStream` in hello query.</span></span>  

    <span data-ttu-id="34e43-277">Ez a lekérdezés használ hello **TIMESTAMP BY** kulcsszó toospecify hello hasznos toobe hello historikus számítási használt Timestamp típusú mező.</span><span class="sxs-lookup"><span data-stu-id="34e43-277">This query uses hello **TIMESTAMP BY** keyword toospecify a timestamp field in hello payload toobe used in hello temporal computation.</span></span> <span data-ttu-id="34e43-278">Ha ez a mező nincs megadva, a hello leképezési művelet hello eseményközpont érkező minden esemény hello idő alapján történik.</span><span class="sxs-lookup"><span data-stu-id="34e43-278">If this field isn't specified, hello windowing operation is performed by using hello time that each event arrived at hello event hub.</span></span> <span data-ttu-id="34e43-279">További információ: hello "érkezésének ideje vs alkalmazási idő" szakasza [Stream Analytics lekérdezési hivatkozás](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span><span class="sxs-lookup"><span data-stu-id="34e43-279">Learn more in hello "Arrival Time vs Application Time" section of [Stream Analytics Query Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span></span>

    <span data-ttu-id="34e43-280">Ez a lekérdezés is segítségével éri el az egyes ablak hello end időbélyeg hello **System.Timestamp** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="34e43-280">This query also accesses a timestamp for hello end of each window by using hello **System.Timestamp** property.</span></span>

5. <span data-ttu-id="34e43-281">Kattintson a **teszt**.</span><span class="sxs-lookup"><span data-stu-id="34e43-281">Click **Test**.</span></span> <span data-ttu-id="34e43-282">hello lekérdezés, amely akkor mintát hello adatok alapján futtatja.</span><span class="sxs-lookup"><span data-stu-id="34e43-282">hello query runs against hello data that you sampled.</span></span>
    
6. <span data-ttu-id="34e43-283">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="34e43-283">Click **Save**.</span></span> <span data-ttu-id="34e43-284">Ezáltal hello lekérdezés hello Streaming Analytics-feladat részeként.</span><span class="sxs-lookup"><span data-stu-id="34e43-284">This saves hello query as part of hello Streaming Analytics job.</span></span> <span data-ttu-id="34e43-285">(Hello mintaadatok nem menti azt.)</span><span class="sxs-lookup"><span data-stu-id="34e43-285">(It doesn't save hello sample data.)</span></span>


## <a name="experiment-using-different-fields-from-hello-stream"></a><span data-ttu-id="34e43-286">Különböző mezőkkel hello adatfolyamból kísérlet</span><span class="sxs-lookup"><span data-stu-id="34e43-286">Experiment using different fields from hello stream</span></span> 

<span data-ttu-id="34e43-287">hello következő táblázatban hello JSON streamadatok részét képező hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="34e43-287">hello following table lists hello fields that are part of hello JSON streaming data.</span></span> <span data-ttu-id="34e43-288">A Lekérdezésszerkesztő hello szabad tooexperiment látja.</span><span class="sxs-lookup"><span data-stu-id="34e43-288">Feel free tooexperiment in hello query editor.</span></span>

|<span data-ttu-id="34e43-289">JSON-tulajdonság</span><span class="sxs-lookup"><span data-stu-id="34e43-289">JSON property</span></span> | <span data-ttu-id="34e43-290">Meghatározás</span><span class="sxs-lookup"><span data-stu-id="34e43-290">Definition</span></span>|
|--- | ---|
|<span data-ttu-id="34e43-291">createdAt</span><span class="sxs-lookup"><span data-stu-id="34e43-291">CreatedAt</span></span> | <span data-ttu-id="34e43-292">hello ideje, hogy hello tweetet lett létrehozva</span><span class="sxs-lookup"><span data-stu-id="34e43-292">hello time that hello tweet was created</span></span>|
|<span data-ttu-id="34e43-293">Témakör</span><span class="sxs-lookup"><span data-stu-id="34e43-293">Topic</span></span> | <span data-ttu-id="34e43-294">a megadott kulcsszó hello témakör, amely megfelel a hello</span><span class="sxs-lookup"><span data-stu-id="34e43-294">hello topic that matches hello specified keyword</span></span>|
|<span data-ttu-id="34e43-295">SentimentScore</span><span class="sxs-lookup"><span data-stu-id="34e43-295">SentimentScore</span></span> | <span data-ttu-id="34e43-296">hello véleményeket pontszám a Sentiment140</span><span class="sxs-lookup"><span data-stu-id="34e43-296">hello sentiment score from Sentiment140</span></span>|
|<span data-ttu-id="34e43-297">Szerző</span><span class="sxs-lookup"><span data-stu-id="34e43-297">Author</span></span> | <span data-ttu-id="34e43-298">hello Twitter leíró hello tweetet küldött</span><span class="sxs-lookup"><span data-stu-id="34e43-298">hello Twitter handle that sent hello tweet</span></span>|
|<span data-ttu-id="34e43-299">Szöveg</span><span class="sxs-lookup"><span data-stu-id="34e43-299">Text</span></span> | <span data-ttu-id="34e43-300">hello tweetet hello teljes törzse</span><span class="sxs-lookup"><span data-stu-id="34e43-300">hello full body of hello tweet</span></span>|


## <a name="create-an-output-sink"></a><span data-ttu-id="34e43-301">Kimeneti fogadóként létrehozása</span><span class="sxs-lookup"><span data-stu-id="34e43-301">Create an output sink</span></span>

<span data-ttu-id="34e43-302">Most már definiált az eseménystream egy event hub bemeneti tooingest események és a lekérdezés tooperform átalakítás hello adatfolyam keresztül.</span><span class="sxs-lookup"><span data-stu-id="34e43-302">You have now defined an event stream, an event hub input tooingest events, and a query tooperform a transformation over hello stream.</span></span> <span data-ttu-id="34e43-303">hello utolsó lépése toodefine kimeneti fogadóként hello feladat.</span><span class="sxs-lookup"><span data-stu-id="34e43-303">hello last step is toodefine an output sink for hello job.</span></span>  

<span data-ttu-id="34e43-304">Ebben az oktatóanyagban összesítve hello tweetet események hello feladat lekérdezés tooAzure Blob-tároló az írása.</span><span class="sxs-lookup"><span data-stu-id="34e43-304">In this tutorial, you write hello aggregated tweet events from hello job query tooAzure Blob storage.</span></span>  <span data-ttu-id="34e43-305">Az eredmények tooAzure SQL adatbázis Azure Table storage, az Event Hubs is beküldéssel vagy Power BI, attól függően, hogy az alkalmazás igényel.</span><span class="sxs-lookup"><span data-stu-id="34e43-305">You can also push your results tooAzure SQL Database, Azure Table storage, Event Hubs, or Power BI, depending on your application needs.</span></span>

## <a name="specify-hello-job-output"></a><span data-ttu-id="34e43-306">Adja meg a feladat kimenetére hello</span><span class="sxs-lookup"><span data-stu-id="34e43-306">Specify hello job output</span></span>

1. <span data-ttu-id="34e43-307">A hello **feladat topológia** területen kattintson a hello **kimeneti** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="34e43-307">In hello **Job Topology** section, click hello **Output** box.</span></span> 

2. <span data-ttu-id="34e43-308">A hello **kimenetek** panelen kattintson a  **+ &nbsp;Hozzáadás** hello panel ezekkel az értékekkel, majd töltse ki:</span><span class="sxs-lookup"><span data-stu-id="34e43-308">In hello **Outputs** blade, click **+&nbsp;Add** and then fill out hello blade with these values:</span></span>

    * <span data-ttu-id="34e43-309">**A kimeneti alias**: hello név használata `TwitterStream-Output`.</span><span class="sxs-lookup"><span data-stu-id="34e43-309">**Output alias**: Use hello name `TwitterStream-Output`.</span></span> 
    * <span data-ttu-id="34e43-310">**Gyűjtése**: válasszon **Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="34e43-310">**Sink**: Select **Blob storage**.</span></span>
    * <span data-ttu-id="34e43-311">**Importálási lehetőségeit**: válasszon **használja a jelenlegi előfizetés blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="34e43-311">**Import options**: Select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="34e43-312">**A tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="34e43-312">**Storage account**.</span></span> <span data-ttu-id="34e43-313">Válassza ki **hozzon létre egy új tárfiókot.**</span><span class="sxs-lookup"><span data-stu-id="34e43-313">Select **Create a new storage account.**</span></span>
    * <span data-ttu-id="34e43-314">**A tárfiók** (második mezőben).</span><span class="sxs-lookup"><span data-stu-id="34e43-314">**Storage account** (second box).</span></span> <span data-ttu-id="34e43-315">Adja meg `YOURNAMEsa`, ahol `YOURNAME` a neve, vagy egy másik egyedi karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="34e43-315">Enter `YOURNAMEsa`, where `YOURNAME` is your name or another unique string.</span></span> <span data-ttu-id="34e43-316">hello neve csak kisbetűket és számokat használhatja, és az Azure között egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="34e43-316">hello name can use only lowercase letters and numbers, and it must be unique across Azure.</span></span> 
    * <span data-ttu-id="34e43-317">**Tároló**.</span><span class="sxs-lookup"><span data-stu-id="34e43-317">**Container**.</span></span> <span data-ttu-id="34e43-318">Adja meg `socialtwitter`.</span><span class="sxs-lookup"><span data-stu-id="34e43-318">Enter `socialtwitter`.</span></span>
    <span data-ttu-id="34e43-319">hello tárfiók neve vagy a tároló neve használt együtt tooprovide egy URI-JÁNAK hello blob-tároló, ez például a következők:</span><span class="sxs-lookup"><span data-stu-id="34e43-319">hello storage account name and container name are used together tooprovide a URI for hello blob storage, like this:</span></span> 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    ![Panel "Új kimeneti" Stream Analytics-feladat](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. <span data-ttu-id="34e43-321">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="34e43-321">Click **Create**.</span></span> 

    <span data-ttu-id="34e43-322">Azure hello tárfiókot hoz létre, és automatikusan létrehoz egy kulcsot.</span><span class="sxs-lookup"><span data-stu-id="34e43-322">Azure creates hello storage account and generates a key automatically.</span></span> 

5. <span data-ttu-id="34e43-323">Bezárás hello **kimenetek** panelen.</span><span class="sxs-lookup"><span data-stu-id="34e43-323">Close hello **Outputs** blade.</span></span> 


## <a name="start-hello-job"></a><span data-ttu-id="34e43-324">Hello feladat indítása</span><span class="sxs-lookup"><span data-stu-id="34e43-324">Start hello job</span></span>

<span data-ttu-id="34e43-325">Egy feladat bemeneti, a lekérdezés és a kimeneti meg van adva.</span><span class="sxs-lookup"><span data-stu-id="34e43-325">A job input, query, and output are specified.</span></span> <span data-ttu-id="34e43-326">Készen áll a toostart hello Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="34e43-326">You are ready toostart hello Stream Analytics job.</span></span>

1. <span data-ttu-id="34e43-327">Győződjön meg arról, hogy hello TwitterWpfClient alkalmazás fut-e.</span><span class="sxs-lookup"><span data-stu-id="34e43-327">Make sure that hello TwitterWpfClient application is running.</span></span> 

2. <span data-ttu-id="34e43-328">Hello feladat panelen kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="34e43-328">In hello job blade, click **Start**.</span></span>

    ![Hello Stream Analytics-feladat indítása](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. <span data-ttu-id="34e43-330">A hello **indítási feladat** panelen a **feladatkiemenetét kezdési időpont**, jelölje be **most** , majd **Start**.</span><span class="sxs-lookup"><span data-stu-id="34e43-330">In hello **Start job** blade, for **Job output start time**, select **Now** and then click **Start**.</span></span> 

    !["Indítsa el a feladat" panel hello Stream Analytics-feladat](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    <span data-ttu-id="34e43-332">Azure értesíti, amikor hello feladat elindult, és hello feladat panelen hello állapot jelenik meg **futtató**.</span><span class="sxs-lookup"><span data-stu-id="34e43-332">Azure notifies you when hello job has started, and in hello job blade, hello status is displayed as **Running**.</span></span>

    ![Futó feladat](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a><span data-ttu-id="34e43-334">Nézet kimeneti véleményeket elemzés</span><span class="sxs-lookup"><span data-stu-id="34e43-334">View output for sentiment analysis</span></span>

<span data-ttu-id="34e43-335">A feladat elindult, és hello valós időben Twitterről adatfolyam feldolgozása után megtekintheti a hello kimeneti véleményeket elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="34e43-335">After your job has started running and is processing hello real-time Twitter stream, you can view hello output for sentiment analysis.</span></span>

<span data-ttu-id="34e43-336">Egy eszköz, például használhatja [Azure Tártallózó](https://http://storageexplorer.com/) vagy [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) tooview a feladat kimeneti valós időben.</span><span class="sxs-lookup"><span data-stu-id="34e43-336">You can use a tool like [Azure Storage Explorer](https://http://storageexplorer.com/) or [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) tooview your job output in real time.</span></span> <span data-ttu-id="34e43-337">Itt használható [Power BI](https://powerbi.com/) , például az alkalmazás tooinclude személyre szabott irányítópultot tooextend hello egyet a következő képernyőkép hello látható:</span><span class="sxs-lookup"><span data-stu-id="34e43-337">From here, you can use [Power BI](https://powerbi.com/) tooextend your application tooinclude a customized dashboard like hello one shown in hello following screenshot:</span></span>

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-tooidentify-trending-topics"></a><span data-ttu-id="34e43-339">Hozzon létre egy másik lekérdezést tooidentify trendekkel kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="34e43-339">Create another query tooidentify trending topics</span></span>

<span data-ttu-id="34e43-340">Azon alapul, használhatja a toounderstand Twitter sentiment egy másik lekérdezés egy [késleltetett ablak](https://msdn.microsoft.com/library/azure/dn835051.aspx).</span><span class="sxs-lookup"><span data-stu-id="34e43-340">Another query you can use toounderstand Twitter sentiment is based on a [Sliding Window](https://msdn.microsoft.com/library/azure/dn835051.aspx).</span></span> <span data-ttu-id="34e43-341">tooidentify trendekkel kapcsolatos témakörök, célszerű figyelni témakörök, amelyek a megadott időn belül megjegyzések vonatkozó küszöbérték cross.</span><span class="sxs-lookup"><span data-stu-id="34e43-341">tooidentify trending topics, you look for topics that cross a threshold value for mentions in a specified amount of time.</span></span>

<span data-ttu-id="34e43-342">Ez az oktatóanyag céljából hello akkor ellenőrizze témakörök, amelyek nem szerepelnek több mint 20 alkalommal hello utolsó 5 másodperc.</span><span class="sxs-lookup"><span data-stu-id="34e43-342">For hello purposes of this tutorial, you check for topics that are mentioned more than 20 times in hello last 5 seconds.</span></span>

1. <span data-ttu-id="34e43-343">Hello feladat panelen kattintson **leállítása** toostop hello feladat.</span><span class="sxs-lookup"><span data-stu-id="34e43-343">In hello job blade, click **Stop** toostop hello job.</span></span> 

2. <span data-ttu-id="34e43-344">A hello **feladat topológia** területen kattintson a hello **lekérdezés** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="34e43-344">In hello **Job Topology** section, click hello **Query** box.</span></span> 

3. <span data-ttu-id="34e43-345">Hello lekérdezés toohello következő módosítása:</span><span class="sxs-lookup"><span data-stu-id="34e43-345">Change hello query toohello following:</span></span>

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. <span data-ttu-id="34e43-346">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="34e43-346">Click **Save**.</span></span>

5. <span data-ttu-id="34e43-347">Győződjön meg arról, hogy hello TwitterWpfClient alkalmazás fut-e.</span><span class="sxs-lookup"><span data-stu-id="34e43-347">Make sure that hello TwitterWpfClient application is running.</span></span> 

6. <span data-ttu-id="34e43-348">Kattintson a **Start** toorestart hello feladat hello új lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="34e43-348">Click **Start** toorestart hello job using hello new query.</span></span>


## <a name="get-support"></a><span data-ttu-id="34e43-349">Támogatás kérése</span><span class="sxs-lookup"><span data-stu-id="34e43-349">Get support</span></span>
<span data-ttu-id="34e43-350">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="34e43-350">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="34e43-351">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34e43-351">Next steps</span></span>
* [<span data-ttu-id="34e43-352">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="34e43-352">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="34e43-353">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="34e43-353">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="34e43-354">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="34e43-354">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="34e43-355">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="34e43-355">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="34e43-356">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="34e43-356">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

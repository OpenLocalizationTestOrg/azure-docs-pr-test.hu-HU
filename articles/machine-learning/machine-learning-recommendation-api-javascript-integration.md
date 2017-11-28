---
title: "Machine Learning javaslatok: JavaScript integrációs |} Microsoft Docs"
description: "Az Azure Machine Learning javaslatok - integráció JavaScript - dokumentációja"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: bbbb5bb6-489d-4a62-a2ae-f36237e9e2e1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 8f27962d097bffc2a03de80244ae41d6573a4bf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a><span data-ttu-id="ad076-103">Azure Machine Learning Recommendations – JavaScript-integráció</span><span class="sxs-lookup"><span data-stu-id="ad076-103">Azure Machine Learning Recommendations - JavaScript Integration</span></span>
> [!NOTE]
> <span data-ttu-id="ad076-104">El kell kezdenie a javaslatok API kognitív szolgáltatás helyett jelen verziójában.</span><span class="sxs-lookup"><span data-stu-id="ad076-104">You should start using the Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="ad076-105">A javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és az új szolgáltatások nincs fejlesztik ki.</span><span class="sxs-lookup"><span data-stu-id="ad076-105">The Recommendations Cognitive Service will be replacing this service, and all the new features will be developed there.</span></span> <span data-ttu-id="ad076-106">Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.</span><span class="sxs-lookup"><span data-stu-id="ad076-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="ad076-107">További információ [áttelepítése az új kognitív szolgáltatáshoz](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="ad076-107">Learn more about [Migrating to the new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="ad076-108">Ez a dokumentum jelzik, hogyan integrálható a JavaScript használó webhelyet.</span><span class="sxs-lookup"><span data-stu-id="ad076-108">This document depict how to integrate your site using JavaScript.</span></span> <span data-ttu-id="ad076-109">A JavaScript lehetővé teszi a adatgyűjtést események küldésére, valamint a javaslatok felhasználását, miután egy javaslat modell létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ad076-109">The JavaScript enables you to send Data Acquisition events and to consume recommendations once you build a recommendation model.</span></span> <span data-ttu-id="ad076-110">JS keresztül végzett összes műveletet is megteheti a kiszolgálóoldali.</span><span class="sxs-lookup"><span data-stu-id="ad076-110">All operations done via JS can be also done from server side.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="ad076-111">1. Általános – áttekintés</span><span class="sxs-lookup"><span data-stu-id="ad076-111">1. General Overview</span></span>
<span data-ttu-id="ad076-112">A 2 fázisok a webhely integrálása az Azure ML javaslatok állnak:</span><span class="sxs-lookup"><span data-stu-id="ad076-112">Integrating your site with Azure ML Recommendations consist on 2 Phases:</span></span>

1. <span data-ttu-id="ad076-113">Események küldése az Azure ML javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="ad076-113">Send Events into Azure ML Recommendations.</span></span> <span data-ttu-id="ad076-114">Ez lehetővé teszi a javaslat modellek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ad076-114">This will enable to build a recommendation model.</span></span>
2. <span data-ttu-id="ad076-115">A javaslatok felhasználását.</span><span class="sxs-lookup"><span data-stu-id="ad076-115">Consume the recommendations.</span></span> <span data-ttu-id="ad076-116">Miután összeállította a modell használatba vehetné a javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="ad076-116">After the model is built you can consume the recommendations.</span></span> <span data-ttu-id="ad076-117">(Ez a dokumentum nem ismerteti a modell létrehozásához olvassa el a gyors üzembe helyezési útmutatót kapcsolatos további információkért).</span><span class="sxs-lookup"><span data-stu-id="ad076-117">(This document does not explain how to build a model, read the quick start guide to get more information on how).</span></span>

<span data-ttu-id="ad076-118"><ins>Első</ins></span><span class="sxs-lookup"><span data-stu-id="ad076-118"><ins>Phase I</ins></span></span>

<span data-ttu-id="ad076-119">Az első fázisban behelyezi a html-lapok egy kis JavaScript-függvénytárat, amely lehetővé teszi az események küldése az Azure ML javaslatok kiszolgálók (az adatok piaci) keresztül előforduló a html-weblap lapján:</span><span class="sxs-lookup"><span data-stu-id="ad076-119">In the first phase you insert into your html pages a small JavaScript library that enables the page to send events as they occur on the html page into Azure ML Recommendations servers (via Data Market):</span></span>

![Drawing1][1]

<span data-ttu-id="ad076-121"><ins>Fázis</ins></span><span class="sxs-lookup"><span data-stu-id="ad076-121"><ins>Phase II</ins></span></span>

<span data-ttu-id="ad076-122">A második fázisában, ha szeretne a javaslatok megjelenítése a lapon, válassza ki a következő lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="ad076-122">In the second phase when you want to show the recommendations on the page you select one of the following options:</span></span>

<span data-ttu-id="ad076-123">1. a kiszolgáló (az oldal megjelenítési fázisa) meghívja az Azure ML javaslatok kiszolgáló (az adatok piaci) keresztül javaslatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ad076-123">1.Your server (on the phase of page rendering) calls Azure ML Recommendations Server (via Data Market) to get recommendations.</span></span> <span data-ttu-id="ad076-124">Az eredmények tartalmazzák a cikkek azonosítójának listáját.</span><span class="sxs-lookup"><span data-stu-id="ad076-124">The results include a list of items id.</span></span> <span data-ttu-id="ad076-125">A cikkek metaadatai (pl. képek, leírás) az eredmények kiegészítése, és a létrehozott lapra kapjon a böngésző kell a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="ad076-125">Your server needs to enrich the results with the items Meta data (e.g. images, description) and send the created page to the browser.</span></span>

![Drawing2][2]

<span data-ttu-id="ad076-127">2 a másik lehetőség egy ajánlott elemek egyszerű listájának első lépése az kis JavaScript-fájl használatára.</span><span class="sxs-lookup"><span data-stu-id="ad076-127">2.The other option is to use the small JavaScript file from phase one to get a simple list of recommended items.</span></span> <span data-ttu-id="ad076-128">Itt kapott adatok Karcsúbb a az első lehetőség.</span><span class="sxs-lookup"><span data-stu-id="ad076-128">The data received here is leaner than the one in the first option.</span></span>

![Drawing3][3]

## <a name="2-prerequisites"></a><span data-ttu-id="ad076-130">2. Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ad076-130">2. Prerequisites</span></span>
1. <span data-ttu-id="ad076-131">Hozzon létre egy új modell API-val.</span><span class="sxs-lookup"><span data-stu-id="ad076-131">Create a new model using the APIs.</span></span> <span data-ttu-id="ad076-132">A gyors üzembe helyezési útmutatójában megtudhatja, hogyan teheti meg.</span><span class="sxs-lookup"><span data-stu-id="ad076-132">See the Quick start guide on how to do it.</span></span>
2. <span data-ttu-id="ad076-133">Kódolja a &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; a Base64 kódolású.</span><span class="sxs-lookup"><span data-stu-id="ad076-133">Encode your &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; with base64.</span></span> <span data-ttu-id="ad076-134">(Ez lesz használható az egyszerű hitelesítés esetén a JS kódot annak az API-k engedélyezése).</span><span class="sxs-lookup"><span data-stu-id="ad076-134">(This will be used for the basic authentication to enable the JS code to call the APIs).</span></span>

## <a name="3-send-data-acquisition-events-using-javascript"></a><span data-ttu-id="ad076-135">3. A JavaScript használatával adatgyűjtést események küldése</span><span class="sxs-lookup"><span data-stu-id="ad076-135">3. Send Data Acquisition events using JavaScript</span></span>
<span data-ttu-id="ad076-136">Az alábbi lépéseket a küldő események elősegítése:</span><span class="sxs-lookup"><span data-stu-id="ad076-136">The following steps facilitate sending events:</span></span>

1. <span data-ttu-id="ad076-137">Vegye fel a kód JQuery könyvtár.</span><span class="sxs-lookup"><span data-stu-id="ad076-137">Include JQuery library in your code.</span></span> <span data-ttu-id="ad076-138">Letöltheti a nugetből a következő URL-címben.</span><span class="sxs-lookup"><span data-stu-id="ad076-138">You can download it from nuget in the following URL.</span></span>
   
     <span data-ttu-id="ad076-139">http://www.nuget.org/Packages/jQuery/1.8.2</span><span class="sxs-lookup"><span data-stu-id="ad076-139">http://www.nuget.org/packages/jQuery/1.8.2</span></span>
2. <span data-ttu-id="ad076-140">A következő URL-címet a javaslatok Java parancsfájl kódtárat tartalmaz: http://aka.ms/RecoJSLib1</span><span class="sxs-lookup"><span data-stu-id="ad076-140">Include the Recommendations Java Script library from the following URL: http://aka.ms/RecoJSLib1</span></span>
3. <span data-ttu-id="ad076-141">A megfelelő paraméterekkel rendelkező Azure ML javaslatok kódtár inicializálása.</span><span class="sxs-lookup"><span data-stu-id="ad076-141">Initialize Azure ML Recommendations library with the appropriate parameters.</span></span>
   
     <span data-ttu-id="ad076-142"><script>AzureMLRecommendationsStart ("<base64encoding of username:key>", "< model_id >"); </script> 
4. Elküldeni a megfelelő eseményt.</span><span class="sxs-lookup"><span data-stu-id="ad076-142"><script> AzureMLRecommendationsStart("<base64encoding of username:key>", "<model_id>"); </script>
4. Send the appropriate event.</span></span> <span data-ttu-id="ad076-143">Tekintse meg a részletes című szakaszt az összes típusú eseményeket (például kattintson esemény) <script> Ha (typeof AzureMLRecommendationsEvent == "nincs definiálva") {</span><span class="sxs-lookup"><span data-stu-id="ad076-143">See detailed section below on all type of events (example of click event)  <script> if (typeof AzureMLRecommendationsEvent=="undefined") {</span></span>         
                     <span data-ttu-id="ad076-144">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script></span><span class="sxs-lookup"><span data-stu-id="ad076-144">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" }); </script></span></span>

### <a name="31----limitations-and-browser-support"></a><span data-ttu-id="ad076-145">3.1.</span><span class="sxs-lookup"><span data-stu-id="ad076-145">3.1.</span></span>    <span data-ttu-id="ad076-146">A korlátozásokkal, valamint a támogatott böngésző</span><span class="sxs-lookup"><span data-stu-id="ad076-146">Limitations and Browser Support</span></span>
<span data-ttu-id="ad076-147">Ez a hivatkozás megvalósítása és kap, mert a.</span><span class="sxs-lookup"><span data-stu-id="ad076-147">This is a reference implementation and it is given as is.</span></span> <span data-ttu-id="ad076-148">Az összes ismertebb böngésző támogatniuk kell.</span><span class="sxs-lookup"><span data-stu-id="ad076-148">It should support all major browsers.</span></span>

### <a name="32----type-of-events"></a><span data-ttu-id="ad076-149">3.2.</span><span class="sxs-lookup"><span data-stu-id="ad076-149">3.2.</span></span>    <span data-ttu-id="ad076-150">Események</span><span class="sxs-lookup"><span data-stu-id="ad076-150">Type of Events</span></span>
<span data-ttu-id="ad076-151">5 különböző típusú eseményt, amely támogatja a tár:, kattintson a javaslat, kattintson a Hozzáadás gombra üzemi kosárhoz, távolítsa el az üzemi bevásárlókocsiból és beszerzési.</span><span class="sxs-lookup"><span data-stu-id="ad076-151">There are 5 types of event that the library supports: Click, Recommendation Click, Add to Shop Cart, Remove from Shop Cart and Purchase.</span></span> <span data-ttu-id="ad076-152">Nincs olyan további esemény miatt lehet beállítani a felhasználói környezet bejelentkezési neve.</span><span class="sxs-lookup"><span data-stu-id="ad076-152">There is an additional event that is used to set the user context called Login.</span></span>

#### <a name="321-click-event"></a><span data-ttu-id="ad076-153">3.2.1.</span><span class="sxs-lookup"><span data-stu-id="ad076-153">3.2.1.</span></span> <span data-ttu-id="ad076-154">Kattintson az esemény</span><span class="sxs-lookup"><span data-stu-id="ad076-154">Click Event</span></span>
<span data-ttu-id="ad076-155">Ez az esemény minden alkalommal elem kattintott kell használni.</span><span class="sxs-lookup"><span data-stu-id="ad076-155">This event should be used any time a user clicked on an item.</span></span> <span data-ttu-id="ad076-156">Általában akkor, ha a felhasználó kattint egy elemet egy új lap megnyitása a cikk adatokkal; Ezen a lapon az esemény megtörténjen.</span><span class="sxs-lookup"><span data-stu-id="ad076-156">Usually when user clicks on an item a new page is opened with the item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="ad076-157">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ad076-157">Parameters:</span></span>

* <span data-ttu-id="ad076-158">"kattintson" (karakterlánc, kötelező) - esemény</span><span class="sxs-lookup"><span data-stu-id="ad076-158">event (string, mandatory) - “click”</span></span>
* <span data-ttu-id="ad076-159">elem (karakterlánc, kötelező) – az elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="ad076-159">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="ad076-160">itemName (karakterlánc, nem kötelező) – az elem neve</span><span class="sxs-lookup"><span data-stu-id="ad076-160">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="ad076-161">itemDescription (karakterlánc, nem kötelező) – a cikk leírása</span><span class="sxs-lookup"><span data-stu-id="ad076-161">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="ad076-162">itemCategory (karakterlánc, nem kötelező) – a cikk a kategória</span><span class="sxs-lookup"><span data-stu-id="ad076-162">itemCategory (string, optional) - the category of the item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

<span data-ttu-id="ad076-163">Vagy nem kötelező adatokkal:</span><span class="sxs-lookup"><span data-stu-id="ad076-163">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


#### <a name="322-recommendation-click-event"></a><span data-ttu-id="ad076-164">3.2.2.</span><span class="sxs-lookup"><span data-stu-id="ad076-164">3.2.2.</span></span> <span data-ttu-id="ad076-165">A javaslat kattintson esemény</span><span class="sxs-lookup"><span data-stu-id="ad076-165">Recommendation Click Event</span></span>
<span data-ttu-id="ad076-166">Ez az esemény minden alkalommal Azure ML javaslatok az ajánlott elemet fogadott elem kattintott kell használni.</span><span class="sxs-lookup"><span data-stu-id="ad076-166">This event should be used any time a user clicked on an item that was received from Azure ML Recommendations as a recommended item.</span></span> <span data-ttu-id="ad076-167">Általában akkor, ha a felhasználó kattint egy elemet egy új lap megnyitása a cikk adatokkal; Ezen a lapon az esemény megtörténjen.</span><span class="sxs-lookup"><span data-stu-id="ad076-167">Usually when user clicks on an item a new page is opened with the item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="ad076-168">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ad076-168">Parameters:</span></span>

* <span data-ttu-id="ad076-169">"recommendationclick" (karakterlánc, kötelező) - esemény</span><span class="sxs-lookup"><span data-stu-id="ad076-169">event (string, mandatory) - “recommendationclick”</span></span>
* <span data-ttu-id="ad076-170">elem (karakterlánc, kötelező) – az elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="ad076-170">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="ad076-171">itemName (karakterlánc, nem kötelező) – az elem neve</span><span class="sxs-lookup"><span data-stu-id="ad076-171">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="ad076-172">itemDescription (karakterlánc, nem kötelező) – a cikk leírása</span><span class="sxs-lookup"><span data-stu-id="ad076-172">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="ad076-173">itemCategory (karakterlánc, nem kötelező) – a cikk a kategória</span><span class="sxs-lookup"><span data-stu-id="ad076-173">itemCategory (string, optional) - the category of the item</span></span>
* <span data-ttu-id="ad076-174">magok (karakterlánc-tömbben, nem kötelező) – a mag, ami a javaslat lekérdezés jön létre.</span><span class="sxs-lookup"><span data-stu-id="ad076-174">seeds (string array, optional) - the seeds that generated the recommendation query.</span></span>
* <span data-ttu-id="ad076-175">(karakterlánc-tömbben, nem kötelező) - recoList kattintott elemet létrehozó javaslat kérelem eredményét.</span><span class="sxs-lookup"><span data-stu-id="ad076-175">recoList (string array, optional) - the result of the recommendation request that generated the item that was clicked.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

<span data-ttu-id="ad076-176">Vagy nem kötelező adatokkal:</span><span class="sxs-lookup"><span data-stu-id="ad076-176">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]                 });
        </script>


#### <a name="323-add-shopping-cart-event"></a><span data-ttu-id="ad076-177">3.2.3.</span><span class="sxs-lookup"><span data-stu-id="ad076-177">3.2.3.</span></span> <span data-ttu-id="ad076-178">Vásárlási bevásárlókocsiból esemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ad076-178">Add Shopping Cart Event</span></span>
<span data-ttu-id="ad076-179">Ezt az eseményt kell használni. Ha a felhasználó vegyen fel egy elemet a kosár.</span><span class="sxs-lookup"><span data-stu-id="ad076-179">This event should be used when the user add an item to the shopping cart.</span></span>
<span data-ttu-id="ad076-180">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ad076-180">Parameters:</span></span>

* <span data-ttu-id="ad076-181">"addshopcart" (karakterlánc, kötelező) - esemény</span><span class="sxs-lookup"><span data-stu-id="ad076-181">event (string, mandatory) - “addshopcart”</span></span>
* <span data-ttu-id="ad076-182">elem (karakterlánc, kötelező) – az elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="ad076-182">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="ad076-183">itemName (karakterlánc, nem kötelező) – az elem neve</span><span class="sxs-lookup"><span data-stu-id="ad076-183">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="ad076-184">itemDescription (karakterlánc, nem kötelező) – a cikk leírása</span><span class="sxs-lookup"><span data-stu-id="ad076-184">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="ad076-185">itemCategory (karakterlánc, nem kötelező) – a cikk a kategória</span><span class="sxs-lookup"><span data-stu-id="ad076-185">itemCategory (string, optional) - the category of the item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a><span data-ttu-id="ad076-186">3.2.4.</span><span class="sxs-lookup"><span data-stu-id="ad076-186">3.2.4.</span></span> <span data-ttu-id="ad076-187">Eltávolítása a bevásárlókocsiból esemény vásárlás</span><span class="sxs-lookup"><span data-stu-id="ad076-187">Remove Shopping Cart Event</span></span>
<span data-ttu-id="ad076-188">Ez az esemény akkor kell használni, amikor a felhasználó eltávolítja a kosár elemet.</span><span class="sxs-lookup"><span data-stu-id="ad076-188">This event should be used when the user removes an item to the shopping cart.</span></span>

<span data-ttu-id="ad076-189">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ad076-189">Parameters:</span></span>

* <span data-ttu-id="ad076-190">"removeshopcart" (karakterlánc, kötelező) - esemény</span><span class="sxs-lookup"><span data-stu-id="ad076-190">event (string, mandatory) - “removeshopcart”</span></span>
* <span data-ttu-id="ad076-191">elem (karakterlánc, kötelező) – az elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="ad076-191">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="ad076-192">itemName (karakterlánc, nem kötelező) – az elem neve</span><span class="sxs-lookup"><span data-stu-id="ad076-192">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="ad076-193">itemDescription (karakterlánc, nem kötelező) – a cikk leírása</span><span class="sxs-lookup"><span data-stu-id="ad076-193">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="ad076-194">itemCategory (karakterlánc, nem kötelező) – a cikk a kategória</span><span class="sxs-lookup"><span data-stu-id="ad076-194">itemCategory (string, optional) - the category of the item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a><span data-ttu-id="ad076-195">3.2.5.</span><span class="sxs-lookup"><span data-stu-id="ad076-195">3.2.5.</span></span> <span data-ttu-id="ad076-196">A vásárlás esemény</span><span class="sxs-lookup"><span data-stu-id="ad076-196">Purchase Event</span></span>
<span data-ttu-id="ad076-197">Ez az esemény akkor kell használni, amikor a felhasználó a kosár vásárolt.</span><span class="sxs-lookup"><span data-stu-id="ad076-197">This event should be used when the user purchased his shopping cart.</span></span>

<span data-ttu-id="ad076-198">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ad076-198">Parameters:</span></span>

* <span data-ttu-id="ad076-199">"beszerzési" (karakterlánc) - esemény</span><span class="sxs-lookup"><span data-stu-id="ad076-199">event (string) - “purchase”</span></span>
* <span data-ttu-id="ad076-200">([] beszerzett) cikkek - bejegyzést birtokló minden elem vásárolt tömb.</span><span class="sxs-lookup"><span data-stu-id="ad076-200">items ( Purchased[] ) - Array holding an entry for each item purchased.</span></span><br><br>
  <span data-ttu-id="ad076-201">Megvásárolt formátuma:</span><span class="sxs-lookup"><span data-stu-id="ad076-201">Purchased format:</span></span>
  * <span data-ttu-id="ad076-202">elem (karakterlánc) – az elem egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ad076-202">item (string) - Unique identifier of the item.</span></span>
  * <span data-ttu-id="ad076-203">megadott számú (egész szám vagy karakterlánc) - beszerzett cikkek.</span><span class="sxs-lookup"><span data-stu-id="ad076-203">count (int or string) - number of items that were purchased.</span></span>
  * <span data-ttu-id="ad076-204">ár (float vagy karakterlánc) - nem kötelező mező – a cikk árát.</span><span class="sxs-lookup"><span data-stu-id="ad076-204">price (float or string) - optional field - the price of the item.</span></span>

<span data-ttu-id="ad076-205">Az alábbi példában látható 3 beszerzési elemek (33, 34, 35), kettő fel mezők (elem, count, ár) és egy (elem 34) ár nélkül.</span><span class="sxs-lookup"><span data-stu-id="ad076-205">The example below shows purchase of 3 items (33, 34, 35), two with all fields populated (item, count, price) and one (item 34) without a price.</span></span>

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a><span data-ttu-id="ad076-206">3.2.6.</span><span class="sxs-lookup"><span data-stu-id="ad076-206">3.2.6.</span></span> <span data-ttu-id="ad076-207">Felhasználói bejelentkezési esemény</span><span class="sxs-lookup"><span data-stu-id="ad076-207">User Login Event</span></span>
<span data-ttu-id="ad076-208">Az Azure ML javaslatok esemény könyvtár hoz létre, és a cookie-t használja, amely innen származik: ugyanazzal a böngészővel események megismerése érdekében.</span><span class="sxs-lookup"><span data-stu-id="ad076-208">Azure ML Recommendations Event library creates and use a cookie in order to identify events that came from the same browser.</span></span> <span data-ttu-id="ad076-209">A modell eredmények Azure ML javaslatok javítása érdekében lehetővé teszi a felhasználó egyedi azonosítóját, amelyek felülírják a cookie-k használatának beállításához.</span><span class="sxs-lookup"><span data-stu-id="ad076-209">In order to improve the model results Azure ML Recommendations enables to set a user unique identification that will override the cookie usage.</span></span>

<span data-ttu-id="ad076-210">Ez az esemény után a felhasználói bejelentkezési és a hely számára használható.</span><span class="sxs-lookup"><span data-stu-id="ad076-210">This event should be used after the user login to your site.</span></span>

<span data-ttu-id="ad076-211">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ad076-211">Parameters:</span></span>

* <span data-ttu-id="ad076-212">"userlogin" (karakterlánc) - esemény</span><span class="sxs-lookup"><span data-stu-id="ad076-212">event (string) - “userlogin”</span></span>
* <span data-ttu-id="ad076-213">felhasználó (karakterlánc) – a felhasználó egyedi azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ad076-213">user (string) - unique identification of the user.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a><span data-ttu-id="ad076-214">4. Javaslatok JavaScripttel felhasználása</span><span class="sxs-lookup"><span data-stu-id="ad076-214">4. Consume Recommendations via JavaScript</span></span>
<span data-ttu-id="ad076-215">A kódot, amely a javaslat akkor váltja ki, néhány JavaScript esemény által az ügyfél weblap.</span><span class="sxs-lookup"><span data-stu-id="ad076-215">The code that consumes the recommendation is triggered by some JavaScript event by the client’s webpage.</span></span> <span data-ttu-id="ad076-216">A javaslat válasz tartalmazza, az ajánlott elemek van, a nevek és azok minősítése.</span><span class="sxs-lookup"><span data-stu-id="ad076-216">The recommendation response includes the recommended items Ids, their names and their ratings.</span></span> <span data-ttu-id="ad076-217">Legjobb, ha használja ezt a beállítást csak a lista megjeleníti a javasolt elemeket - összetettebb kezelése (például a cikk metaadatok hozzáadása) el kell végezni a kiszolgáló oldalán integrálását.</span><span class="sxs-lookup"><span data-stu-id="ad076-217">It’s best to use this option only for a list display of the recommended items - more complex handling (such as adding the item’s metadata) should be done on the server side integration.</span></span>

### <a name="41-consume-recommendations"></a><span data-ttu-id="ad076-218">4.1 felhasználásához javaslatok</span><span class="sxs-lookup"><span data-stu-id="ad076-218">4.1 Consume Recommendations</span></span>
<span data-ttu-id="ad076-219">Felhasználását ajánlásokat kell tartalmaznia a JavaScript szükséges kódtárak és AzureMLRecommendationsStart hívása a lap a.</span><span class="sxs-lookup"><span data-stu-id="ad076-219">To consume recommendations you need to include the required JavaScript libraries in your page and to call AzureMLRecommendationsStart.</span></span> <span data-ttu-id="ad076-220">Lásd a 2. szakasz.</span><span class="sxs-lookup"><span data-stu-id="ad076-220">See section 2.</span></span>

<span data-ttu-id="ad076-221">Meg kell hívnia a hívott metódus egy vagy több elem javaslatok felhasználásához: AzureMLRecommendationsGetI2IRecommendation.</span><span class="sxs-lookup"><span data-stu-id="ad076-221">To consume recommendations for one or more items you need to call a method called: AzureMLRecommendationsGetI2IRecommendation.</span></span>

<span data-ttu-id="ad076-222">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ad076-222">Parameters:</span></span>

* <span data-ttu-id="ad076-223">elemek (karakterláncokból álló tömb) - javaslatok segítségével egy vagy több elemét.</span><span class="sxs-lookup"><span data-stu-id="ad076-223">items (array of strings) - One or more items to get recommendations for.</span></span> <span data-ttu-id="ad076-224">Ha egy Fbt build felhasznált, beállíthat Itt csak egy elemet.</span><span class="sxs-lookup"><span data-stu-id="ad076-224">If you consume an Fbt build then you can set here only one item.</span></span>
* <span data-ttu-id="ad076-225">numberOfResults (int) - szükséges eredmények száma.</span><span class="sxs-lookup"><span data-stu-id="ad076-225">numberOfResults (int) - number of required results.</span></span>
* <span data-ttu-id="ad076-226">includeMetadata (logikai érték, nem kötelező) – Ha értéke "true" azt jelzi, hogy a metaadatok mezőt ki kell tölteni az eredményben.</span><span class="sxs-lookup"><span data-stu-id="ad076-226">includeMetadata (boolean, optional) - if set to ‘true’ indicates that the metadata field must be populated in the result.</span></span>
* <span data-ttu-id="ad076-227">Függvény - egy függvény kezelésére a javaslatok feldolgozása adott vissza.</span><span class="sxs-lookup"><span data-stu-id="ad076-227">Processing function - a function that will handle the recommendations returned.</span></span> <span data-ttu-id="ad076-228">Az adatok egy tömbjét adja vissza a rendszer:</span><span class="sxs-lookup"><span data-stu-id="ad076-228">The data is returned as an array of:</span></span>
  * <span data-ttu-id="ad076-229">Konfigurációelem - elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="ad076-229">Item - item unique id</span></span>
  * <span data-ttu-id="ad076-230">név - elem neve (ha létezik a katalógus)</span><span class="sxs-lookup"><span data-stu-id="ad076-230">name - item name (if exist in catalog)</span></span>
  * <span data-ttu-id="ad076-231">minősítés - javaslat minősítése</span><span class="sxs-lookup"><span data-stu-id="ad076-231">rating - recommendation rating</span></span>
  * <span data-ttu-id="ad076-232">metaadat - karakterlánc, amely a metaadatokat az elem jelöli</span><span class="sxs-lookup"><span data-stu-id="ad076-232">metadata - a string that represents the metadata of the item</span></span>

<span data-ttu-id="ad076-233">Példa: A következő kódot kér 8 javaslatok elem "64f6eb0d-947a-4c18-a16c-888da9e228ba" (és nem ad meg a includeMetadata - implicit módon felirat látható, hogy nincsenek metaadatok szükség), azt a puffer majd összefűzésére az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="ad076-233">Example: The following code requests 8 recommendations for item "64f6eb0d-947a-4c18-a16c-888da9e228ba" (and by not specifying includeMetadata - it implicitly says that no metadata is required), it then concatenate the results into a buffer.</span></span>

        <script>
             var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                 var buff = "";
                 for (var ii = 0; ii < reco.length; ii++) {
                       buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                 }
                 alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png

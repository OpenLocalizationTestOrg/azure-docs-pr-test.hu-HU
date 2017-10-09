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
redirect_document_id: True
ms.openlocfilehash: 4c5f0eee4aa04ce823321d52985374c52850f0d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a><span data-ttu-id="001e1-103">Azure Machine Learning Recommendations – JavaScript-integráció</span><span class="sxs-lookup"><span data-stu-id="001e1-103">Azure Machine Learning Recommendations - JavaScript Integration</span></span>
> [!NOTE]
> <span data-ttu-id="001e1-104">El kell kezdenie hello javaslatok API kognitív szolgáltatás helyett jelen verziójában.</span><span class="sxs-lookup"><span data-stu-id="001e1-104">You should start using hello Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="001e1-105">hello javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és minden hello új szolgáltatások nincs fejlesztik ki.</span><span class="sxs-lookup"><span data-stu-id="001e1-105">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="001e1-106">Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.</span><span class="sxs-lookup"><span data-stu-id="001e1-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="001e1-107">További információ [áttelepítése toohello új kognitív szolgáltatás](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="001e1-107">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="001e1-108">Ez a dokumentum jelzik a hogyan toointegrate a webhely JavaScript használatával.</span><span class="sxs-lookup"><span data-stu-id="001e1-108">This document depict how toointegrate your site using JavaScript.</span></span> <span data-ttu-id="001e1-109">hello JavaScript lehetővé teszi toosend adatgyűjtést események és tooconsume javaslatokat a javaslat modell létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="001e1-109">hello JavaScript enables you toosend Data Acquisition events and tooconsume recommendations once you build a recommendation model.</span></span> <span data-ttu-id="001e1-110">JS keresztül végzett összes műveletet is megteheti a kiszolgálóoldali.</span><span class="sxs-lookup"><span data-stu-id="001e1-110">All operations done via JS can be also done from server side.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="001e1-111">1. Általános – áttekintés</span><span class="sxs-lookup"><span data-stu-id="001e1-111">1. General Overview</span></span>
<span data-ttu-id="001e1-112">A 2 fázisok a webhely integrálása az Azure ML javaslatok állnak:</span><span class="sxs-lookup"><span data-stu-id="001e1-112">Integrating your site with Azure ML Recommendations consist on 2 Phases:</span></span>

1. <span data-ttu-id="001e1-113">Események küldése az Azure ML javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="001e1-113">Send Events into Azure ML Recommendations.</span></span> <span data-ttu-id="001e1-114">Ezzel a lépéssel engedélyezi a javaslat modell toobuild.</span><span class="sxs-lookup"><span data-stu-id="001e1-114">This will enable toobuild a recommendation model.</span></span>
2. <span data-ttu-id="001e1-115">Hello javaslatok felhasználását.</span><span class="sxs-lookup"><span data-stu-id="001e1-115">Consume hello recommendations.</span></span> <span data-ttu-id="001e1-116">Miután hello modelljei épülnek hello javaslatok felhasználhat.</span><span class="sxs-lookup"><span data-stu-id="001e1-116">After hello model is built you can consume hello recommendations.</span></span> <span data-ttu-id="001e1-117">(Ez a dokumentum nem azt ismertetik, hogyan toobuild modell, olvassa el hello gyors üzembe helyezési útmutató tooget olvashat).</span><span class="sxs-lookup"><span data-stu-id="001e1-117">(This document does not explain how toobuild a model, read hello quick start guide tooget more information on how).</span></span>

<span data-ttu-id="001e1-118"><ins>Első</ins></span><span class="sxs-lookup"><span data-stu-id="001e1-118"><ins>Phase I</ins></span></span>

<span data-ttu-id="001e1-119">Hello az első fázis behelyezi a html-lapok egy kis JavaScript-függvénytárat, amely lehetővé teszi azok bekövetkezésekor hello html-lapot az Azure ML javaslatok kiszolgálók (az adatok piaci) keresztül történő hello toosend események:</span><span class="sxs-lookup"><span data-stu-id="001e1-119">In hello first phase you insert into your html pages a small JavaScript library that enables hello page toosend events as they occur on hello html page into Azure ML Recommendations servers (via Data Market):</span></span>

![Drawing1][1]

<span data-ttu-id="001e1-121"><ins>Fázis</ins></span><span class="sxs-lookup"><span data-stu-id="001e1-121"><ins>Phase II</ins></span></span>

<span data-ttu-id="001e1-122">Hello második fázisában, ha azt szeretné, hogy tooshow hello javaslatok hello lapon, válassza ki az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="001e1-122">In hello second phase when you want tooshow hello recommendations on hello page you select one of hello following options:</span></span>

<span data-ttu-id="001e1-123">1. a kiszolgáló (az oldal megjelenítési hello fázisában) meghívja az Azure ML javaslatok kiszolgáló (az adatok piaci) keresztül tooget javaslatok.</span><span class="sxs-lookup"><span data-stu-id="001e1-123">1.Your server (on hello phase of page rendering) calls Azure ML Recommendations Server (via Data Market) tooget recommendations.</span></span> <span data-ttu-id="001e1-124">hello eredmények elemek azonosító listáját tartalmazza. A kiszolgáló tooenrich hello eredmények hello cikkek metaadatai (pl. képek, leírás) kell, és létre lap toohello böngésző hello küldése.</span><span class="sxs-lookup"><span data-stu-id="001e1-124">hello results include a list of items id. Your server needs tooenrich hello results with hello items Meta data (e.g. images, description) and send hello created page toohello browser.</span></span>

![Drawing2][2]

<span data-ttu-id="001e1-126">2 hello másik lehetőség egy toouse hello kis JavaScript-fájlt a fázis egy tooget ajánlott elemek egyszerű listája.</span><span class="sxs-lookup"><span data-stu-id="001e1-126">2.hello other option is toouse hello small JavaScript file from phase one tooget a simple list of recommended items.</span></span> <span data-ttu-id="001e1-127">Itt fogadott hello adatok Karcsúbb mint hello hello első beállításban.</span><span class="sxs-lookup"><span data-stu-id="001e1-127">hello data received here is leaner than hello one in hello first option.</span></span>

![Drawing3][3]

## <a name="2-prerequisites"></a><span data-ttu-id="001e1-129">2. Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="001e1-129">2. Prerequisites</span></span>
1. <span data-ttu-id="001e1-130">Hozzon létre egy új modell hello API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="001e1-130">Create a new model using hello APIs.</span></span> <span data-ttu-id="001e1-131">Hello – első lépések útmutató meg, hogyan toodo azt.</span><span class="sxs-lookup"><span data-stu-id="001e1-131">See hello Quick start guide on how toodo it.</span></span>
2. <span data-ttu-id="001e1-132">Kódolja a &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; a Base64 kódolású.</span><span class="sxs-lookup"><span data-stu-id="001e1-132">Encode your &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; with base64.</span></span> <span data-ttu-id="001e1-133">(Ez használandó hello az egyszerű hitelesítés tooenable hello JS kód toocall hello API-k).</span><span class="sxs-lookup"><span data-stu-id="001e1-133">(This will be used for hello basic authentication tooenable hello JS code toocall hello APIs).</span></span>

## <a name="3-send-data-acquisition-events-using-javascript"></a><span data-ttu-id="001e1-134">3. A JavaScript használatával adatgyűjtést események küldése</span><span class="sxs-lookup"><span data-stu-id="001e1-134">3. Send Data Acquisition events using JavaScript</span></span>
<span data-ttu-id="001e1-135">a lépéseket követve hello küldő események elősegítése:</span><span class="sxs-lookup"><span data-stu-id="001e1-135">hello following steps facilitate sending events:</span></span>

1. <span data-ttu-id="001e1-136">Vegye fel a kód JQuery könyvtár.</span><span class="sxs-lookup"><span data-stu-id="001e1-136">Include JQuery library in your code.</span></span> <span data-ttu-id="001e1-137">Azt az URL-cím a következő hello nuget-ből tölthetik le.</span><span class="sxs-lookup"><span data-stu-id="001e1-137">You can download it from nuget in hello following URL.</span></span>
   
     <span data-ttu-id="001e1-138">http://www.nuget.org/Packages/jQuery/1.8.2</span><span class="sxs-lookup"><span data-stu-id="001e1-138">http://www.nuget.org/packages/jQuery/1.8.2</span></span>
2. <span data-ttu-id="001e1-139">Hello javaslatok Java parancsfájl kódtárat hello a következő URL-címet tartalmaz: http://aka.ms/RecoJSLib1</span><span class="sxs-lookup"><span data-stu-id="001e1-139">Include hello Recommendations Java Script library from hello following URL: http://aka.ms/RecoJSLib1</span></span>
3. <span data-ttu-id="001e1-140">Hello megfelelő paraméterekkel rendelkező Azure ML javaslatok kódtár inicializálása.</span><span class="sxs-lookup"><span data-stu-id="001e1-140">Initialize Azure ML Recommendations library with hello appropriate parameters.</span></span>
   
     <span data-ttu-id="001e1-141"><script>AzureMLRecommendationsStart ("<base64encoding of username:key>", "< model_id >"); </script> 
4. Küldési hello megfelelő esemény.</span><span class="sxs-lookup"><span data-stu-id="001e1-141"><script> AzureMLRecommendationsStart("<base64encoding of username:key>", "<model_id>"); </script>
4. Send hello appropriate event.</span></span> <span data-ttu-id="001e1-142">Tekintse meg a részletes című szakaszt az összes típusú eseményeket (például kattintson esemény) <script> Ha (typeof AzureMLRecommendationsEvent == "nincs definiálva") {</span><span class="sxs-lookup"><span data-stu-id="001e1-142">See detailed section below on all type of events (example of click event)  <script> if (typeof AzureMLRecommendationsEvent=="undefined") {</span></span>         
                     <span data-ttu-id="001e1-143">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script></span><span class="sxs-lookup"><span data-stu-id="001e1-143">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" }); </script></span></span>

### <a name="31----limitations-and-browser-support"></a><span data-ttu-id="001e1-144">3.1.</span><span class="sxs-lookup"><span data-stu-id="001e1-144">3.1.</span></span>    <span data-ttu-id="001e1-145">A korlátozásokkal, valamint a támogatott böngésző</span><span class="sxs-lookup"><span data-stu-id="001e1-145">Limitations and Browser Support</span></span>
<span data-ttu-id="001e1-146">Ez a hivatkozás megvalósítása és kap, mert a.</span><span class="sxs-lookup"><span data-stu-id="001e1-146">This is a reference implementation and it is given as is.</span></span> <span data-ttu-id="001e1-147">Az összes ismertebb böngésző támogatniuk kell.</span><span class="sxs-lookup"><span data-stu-id="001e1-147">It should support all major browsers.</span></span>

### <a name="32----type-of-events"></a><span data-ttu-id="001e1-148">3.2.</span><span class="sxs-lookup"><span data-stu-id="001e1-148">3.2.</span></span>    <span data-ttu-id="001e1-149">Események</span><span class="sxs-lookup"><span data-stu-id="001e1-149">Type of Events</span></span>
<span data-ttu-id="001e1-150">Esemény 5 típusú hello könyvtár támogatja: kattintson a gombra, kattintson a javaslat, vegye fel a bevásárlókocsiból, tooShop eltávolítása üzemi bevásárlókocsiból és beszerzési.</span><span class="sxs-lookup"><span data-stu-id="001e1-150">There are 5 types of event that hello library supports: Click, Recommendation Click, Add tooShop Cart, Remove from Shop Cart and Purchase.</span></span> <span data-ttu-id="001e1-151">Nincs olyan további esemény miatt használt tooset hello felhasználói környezet bejelentkezési neve.</span><span class="sxs-lookup"><span data-stu-id="001e1-151">There is an additional event that is used tooset hello user context called Login.</span></span>

#### <a name="321-click-event"></a><span data-ttu-id="001e1-152">3.2.1.</span><span class="sxs-lookup"><span data-stu-id="001e1-152">3.2.1.</span></span> <span data-ttu-id="001e1-153">Kattintson az esemény</span><span class="sxs-lookup"><span data-stu-id="001e1-153">Click Event</span></span>
<span data-ttu-id="001e1-154">Ez az esemény minden alkalommal elem kattintott kell használni.</span><span class="sxs-lookup"><span data-stu-id="001e1-154">This event should be used any time a user clicked on an item.</span></span> <span data-ttu-id="001e1-155">Általában akkor, ha a felhasználó kattint egy elemet egy új lap megnyitása hello elem adatokkal; Ezen a lapon az esemény megtörténjen.</span><span class="sxs-lookup"><span data-stu-id="001e1-155">Usually when user clicks on an item a new page is opened with hello item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="001e1-156">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="001e1-156">Parameters:</span></span>

* <span data-ttu-id="001e1-157">"kattintson" (karakterlánc, kötelező) - esemény</span><span class="sxs-lookup"><span data-stu-id="001e1-157">event (string, mandatory) - “click”</span></span>
* <span data-ttu-id="001e1-158">elem (karakterlánc, kötelező) – hello elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="001e1-158">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="001e1-159">itemName (karakterlánc, nem kötelező) – hello hello elem neve</span><span class="sxs-lookup"><span data-stu-id="001e1-159">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="001e1-160">itemDescription (karakterlánc, nem kötelező) – hello elem hello leírása</span><span class="sxs-lookup"><span data-stu-id="001e1-160">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="001e1-161">itemCategory (karakterlánc, nem kötelező) – hello kategória hello elem</span><span class="sxs-lookup"><span data-stu-id="001e1-161">itemCategory (string, optional) - hello category of hello item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

<span data-ttu-id="001e1-162">Vagy nem kötelező adatokkal:</span><span class="sxs-lookup"><span data-stu-id="001e1-162">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


#### <a name="322-recommendation-click-event"></a><span data-ttu-id="001e1-163">3.2.2.</span><span class="sxs-lookup"><span data-stu-id="001e1-163">3.2.2.</span></span> <span data-ttu-id="001e1-164">A javaslat kattintson esemény</span><span class="sxs-lookup"><span data-stu-id="001e1-164">Recommendation Click Event</span></span>
<span data-ttu-id="001e1-165">Ez az esemény minden alkalommal Azure ML javaslatok az ajánlott elemet fogadott elem kattintott kell használni.</span><span class="sxs-lookup"><span data-stu-id="001e1-165">This event should be used any time a user clicked on an item that was received from Azure ML Recommendations as a recommended item.</span></span> <span data-ttu-id="001e1-166">Általában akkor, ha a felhasználó kattint egy elemet egy új lap megnyitása hello elem adatokkal; Ezen a lapon az esemény megtörténjen.</span><span class="sxs-lookup"><span data-stu-id="001e1-166">Usually when user clicks on an item a new page is opened with hello item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="001e1-167">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="001e1-167">Parameters:</span></span>

* <span data-ttu-id="001e1-168">"recommendationclick" (karakterlánc, kötelező) - esemény</span><span class="sxs-lookup"><span data-stu-id="001e1-168">event (string, mandatory) - “recommendationclick”</span></span>
* <span data-ttu-id="001e1-169">elem (karakterlánc, kötelező) – hello elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="001e1-169">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="001e1-170">itemName (karakterlánc, nem kötelező) – hello hello elem neve</span><span class="sxs-lookup"><span data-stu-id="001e1-170">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="001e1-171">itemDescription (karakterlánc, nem kötelező) – hello elem hello leírása</span><span class="sxs-lookup"><span data-stu-id="001e1-171">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="001e1-172">itemCategory (karakterlánc, nem kötelező) – hello kategória hello elem</span><span class="sxs-lookup"><span data-stu-id="001e1-172">itemCategory (string, optional) - hello category of hello item</span></span>
* <span data-ttu-id="001e1-173">(karakterlánc-tömbben, nem kötelező) - magok hello mag, ami hello javaslat lekérdezés jön létre.</span><span class="sxs-lookup"><span data-stu-id="001e1-173">seeds (string array, optional) - hello seeds that generated hello recommendation query.</span></span>
* <span data-ttu-id="001e1-174">(karakterlánc-tömbben, nem kötelező) - recoList hello kattintott hello elemet létrehozó hello javaslat kérelem eredményét.</span><span class="sxs-lookup"><span data-stu-id="001e1-174">recoList (string array, optional) - hello result of hello recommendation request that generated hello item that was clicked.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

<span data-ttu-id="001e1-175">Vagy nem kötelező adatokkal:</span><span class="sxs-lookup"><span data-stu-id="001e1-175">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]                 });
        </script>


#### <a name="323-add-shopping-cart-event"></a><span data-ttu-id="001e1-176">3.2.3.</span><span class="sxs-lookup"><span data-stu-id="001e1-176">3.2.3.</span></span> <span data-ttu-id="001e1-177">Vásárlási bevásárlókocsiból esemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="001e1-177">Add Shopping Cart Event</span></span>
<span data-ttu-id="001e1-178">Ezt az eseményt kell használni. Ha a hello felhasználó hozzáadása egy elem toohello bevásárlókocsiból vásárlás.</span><span class="sxs-lookup"><span data-stu-id="001e1-178">This event should be used when hello user add an item toohello shopping cart.</span></span>
<span data-ttu-id="001e1-179">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="001e1-179">Parameters:</span></span>

* <span data-ttu-id="001e1-180">"addshopcart" (karakterlánc, kötelező) - esemény</span><span class="sxs-lookup"><span data-stu-id="001e1-180">event (string, mandatory) - “addshopcart”</span></span>
* <span data-ttu-id="001e1-181">elem (karakterlánc, kötelező) – hello elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="001e1-181">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="001e1-182">itemName (karakterlánc, nem kötelező) – hello hello elem neve</span><span class="sxs-lookup"><span data-stu-id="001e1-182">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="001e1-183">itemDescription (karakterlánc, nem kötelező) – hello elem hello leírása</span><span class="sxs-lookup"><span data-stu-id="001e1-183">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="001e1-184">itemCategory (karakterlánc, nem kötelező) – hello kategória hello elem</span><span class="sxs-lookup"><span data-stu-id="001e1-184">itemCategory (string, optional) - hello category of hello item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a><span data-ttu-id="001e1-185">3.2.4.</span><span class="sxs-lookup"><span data-stu-id="001e1-185">3.2.4.</span></span> <span data-ttu-id="001e1-186">Eltávolítása a bevásárlókocsiból esemény vásárlás</span><span class="sxs-lookup"><span data-stu-id="001e1-186">Remove Shopping Cart Event</span></span>
<span data-ttu-id="001e1-187">Ez az esemény akkor kell használni, amikor hello felhasználó eltávolítja az elemet toohello kosár.</span><span class="sxs-lookup"><span data-stu-id="001e1-187">This event should be used when hello user removes an item toohello shopping cart.</span></span>

<span data-ttu-id="001e1-188">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="001e1-188">Parameters:</span></span>

* <span data-ttu-id="001e1-189">"removeshopcart" (karakterlánc, kötelező) - esemény</span><span class="sxs-lookup"><span data-stu-id="001e1-189">event (string, mandatory) - “removeshopcart”</span></span>
* <span data-ttu-id="001e1-190">elem (karakterlánc, kötelező) – hello elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="001e1-190">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="001e1-191">itemName (karakterlánc, nem kötelező) – hello hello elem neve</span><span class="sxs-lookup"><span data-stu-id="001e1-191">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="001e1-192">itemDescription (karakterlánc, nem kötelező) – hello elem hello leírása</span><span class="sxs-lookup"><span data-stu-id="001e1-192">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="001e1-193">itemCategory (karakterlánc, nem kötelező) – hello kategória hello elem</span><span class="sxs-lookup"><span data-stu-id="001e1-193">itemCategory (string, optional) - hello category of hello item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a><span data-ttu-id="001e1-194">3.2.5.</span><span class="sxs-lookup"><span data-stu-id="001e1-194">3.2.5.</span></span> <span data-ttu-id="001e1-195">A vásárlás esemény</span><span class="sxs-lookup"><span data-stu-id="001e1-195">Purchase Event</span></span>
<span data-ttu-id="001e1-196">Ez az esemény akkor kell használni, amikor hello felhasználói vásárolt annak kosár.</span><span class="sxs-lookup"><span data-stu-id="001e1-196">This event should be used when hello user purchased his shopping cart.</span></span>

<span data-ttu-id="001e1-197">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="001e1-197">Parameters:</span></span>

* <span data-ttu-id="001e1-198">"beszerzési" (karakterlánc) - esemény</span><span class="sxs-lookup"><span data-stu-id="001e1-198">event (string) - “purchase”</span></span>
* <span data-ttu-id="001e1-199">([] beszerzett) cikkek - bejegyzést birtokló minden elem vásárolt tömb.</span><span class="sxs-lookup"><span data-stu-id="001e1-199">items ( Purchased[] ) - Array holding an entry for each item purchased.</span></span><br><br>
  <span data-ttu-id="001e1-200">Megvásárolt formátuma:</span><span class="sxs-lookup"><span data-stu-id="001e1-200">Purchased format:</span></span>
  * <span data-ttu-id="001e1-201">elem (karakterlánc) – hello elem egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="001e1-201">item (string) - Unique identifier of hello item.</span></span>
  * <span data-ttu-id="001e1-202">megadott számú (egész szám vagy karakterlánc) - beszerzett cikkek.</span><span class="sxs-lookup"><span data-stu-id="001e1-202">count (int or string) - number of items that were purchased.</span></span>
  * <span data-ttu-id="001e1-203">Árlista (float vagy karakterlánc) – nem kötelező mező – hello ára hello elemet.</span><span class="sxs-lookup"><span data-stu-id="001e1-203">price (float or string) - optional field - hello price of hello item.</span></span>

<span data-ttu-id="001e1-204">hello az alábbi példában beszerzési 3 (33, 34, 35) elemek kettő fel mezők (elem, count, ár) és egy (elem 34) ár nélkül.</span><span class="sxs-lookup"><span data-stu-id="001e1-204">hello example below shows purchase of 3 items (33, 34, 35), two with all fields populated (item, count, price) and one (item 34) without a price.</span></span>

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a><span data-ttu-id="001e1-205">3.2.6.</span><span class="sxs-lookup"><span data-stu-id="001e1-205">3.2.6.</span></span> <span data-ttu-id="001e1-206">Felhasználói bejelentkezési esemény</span><span class="sxs-lookup"><span data-stu-id="001e1-206">User Login Event</span></span>
<span data-ttu-id="001e1-207">Az Azure ML javaslatok esemény könyvtár hoz létre és használja a cookie-k rendelés tooidentify esemény, amely innen származik: hello ugyanazt a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="001e1-207">Azure ML Recommendations Event library creates and use a cookie in order tooidentify events that came from hello same browser.</span></span> <span data-ttu-id="001e1-208">Eredmények Azure ML javaslatok rendelés tooimprove hello modell lehetővé teszi a felhasználó egyedi azonosítóját, amelyek felülírják hello cookie-k használatának tooset.</span><span class="sxs-lookup"><span data-stu-id="001e1-208">In order tooimprove hello model results Azure ML Recommendations enables tooset a user unique identification that will override hello cookie usage.</span></span>

<span data-ttu-id="001e1-209">Ez az esemény után hello felhasználói bejelentkezési tooyour helyet kell használni.</span><span class="sxs-lookup"><span data-stu-id="001e1-209">This event should be used after hello user login tooyour site.</span></span>

<span data-ttu-id="001e1-210">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="001e1-210">Parameters:</span></span>

* <span data-ttu-id="001e1-211">"userlogin" (karakterlánc) - esemény</span><span class="sxs-lookup"><span data-stu-id="001e1-211">event (string) - “userlogin”</span></span>
* <span data-ttu-id="001e1-212">felhasználó (karakterlánc) – hello felhasználó egyedi azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="001e1-212">user (string) - unique identification of hello user.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a><span data-ttu-id="001e1-213">4. Javaslatok JavaScripttel felhasználása</span><span class="sxs-lookup"><span data-stu-id="001e1-213">4. Consume Recommendations via JavaScript</span></span>
<span data-ttu-id="001e1-214">hello kódot, amely akkor hello javaslat váltja ki, néhány JavaScript esemény hello ügyfél weblap.</span><span class="sxs-lookup"><span data-stu-id="001e1-214">hello code that consumes hello recommendation is triggered by some JavaScript event by hello client’s webpage.</span></span> <span data-ttu-id="001e1-215">hello javaslat válasz ajánlott elemek van, a nevek és azok minősítései hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="001e1-215">hello recommendation response includes hello recommended items Ids, their names and their ratings.</span></span> <span data-ttu-id="001e1-216">Akkor ajánlott toouse ezt a beállítást ajánlott elemek - összetettebb (mint például a hello elem metaadatok hozzáadása) kezelése hello csak a lista megjeleníti a hello server ügyféloldali integrációs kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="001e1-216">It’s best toouse this option only for a list display of hello recommended items - more complex handling (such as adding hello item’s metadata) should be done on hello server side integration.</span></span>

### <a name="41-consume-recommendations"></a><span data-ttu-id="001e1-217">4.1 felhasználásához javaslatok</span><span class="sxs-lookup"><span data-stu-id="001e1-217">4.1 Consume Recommendations</span></span>
<span data-ttu-id="001e1-218">tooconsume javaslatok tooinclude hello van szüksége a lap és toocall AzureMLRecommendationsStart JavaScript-tárak szükséges.</span><span class="sxs-lookup"><span data-stu-id="001e1-218">tooconsume recommendations you need tooinclude hello required JavaScript libraries in your page and toocall AzureMLRecommendationsStart.</span></span> <span data-ttu-id="001e1-219">Lásd a 2. szakasz.</span><span class="sxs-lookup"><span data-stu-id="001e1-219">See section 2.</span></span>

<span data-ttu-id="001e1-220">egy vagy több elem tooconsume javaslatok van szüksége egy metódus hívása toocall: AzureMLRecommendationsGetI2IRecommendation.</span><span class="sxs-lookup"><span data-stu-id="001e1-220">tooconsume recommendations for one or more items you need toocall a method called: AzureMLRecommendationsGetI2IRecommendation.</span></span>

<span data-ttu-id="001e1-221">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="001e1-221">Parameters:</span></span>

* <span data-ttu-id="001e1-222">(karakterlánc tömbje) - elemek egy vagy több elem tooget javaslatok.</span><span class="sxs-lookup"><span data-stu-id="001e1-222">items (array of strings) - One or more items tooget recommendations for.</span></span> <span data-ttu-id="001e1-223">Ha egy Fbt build felhasznált, beállíthat Itt csak egy elemet.</span><span class="sxs-lookup"><span data-stu-id="001e1-223">If you consume an Fbt build then you can set here only one item.</span></span>
* <span data-ttu-id="001e1-224">numberOfResults (int) - szükséges eredmények száma.</span><span class="sxs-lookup"><span data-stu-id="001e1-224">numberOfResults (int) - number of required results.</span></span>
* <span data-ttu-id="001e1-225">includeMetadata (logikai érték, nem kötelező) – Ha too'true beállítása "hello metaadatok mező ki kell tölteni a hello eredmény jelzi.</span><span class="sxs-lookup"><span data-stu-id="001e1-225">includeMetadata (boolean, optional) - if set too‘true’ indicates that hello metadata field must be populated in hello result.</span></span>
* <span data-ttu-id="001e1-226">Függvény - egy függvény kezelésére hello javaslatok feldolgozása adott vissza.</span><span class="sxs-lookup"><span data-stu-id="001e1-226">Processing function - a function that will handle hello recommendations returned.</span></span> <span data-ttu-id="001e1-227">hello adatok tömbjét adja vissza a rendszer:</span><span class="sxs-lookup"><span data-stu-id="001e1-227">hello data is returned as an array of:</span></span>
  * <span data-ttu-id="001e1-228">Konfigurációelem - elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="001e1-228">Item - item unique id</span></span>
  * <span data-ttu-id="001e1-229">név - elem neve (ha létezik a katalógus)</span><span class="sxs-lookup"><span data-stu-id="001e1-229">name - item name (if exist in catalog)</span></span>
  * <span data-ttu-id="001e1-230">minősítés - javaslat minősítése</span><span class="sxs-lookup"><span data-stu-id="001e1-230">rating - recommendation rating</span></span>
  * <span data-ttu-id="001e1-231">metaadat - hello metaadatok hello elem jelölő karakterlánccá</span><span class="sxs-lookup"><span data-stu-id="001e1-231">metadata - a string that represents hello metadata of hello item</span></span>

<span data-ttu-id="001e1-232">Példa: a következő kód hello kérelmek 8 javaslatok elem "64f6eb0d-947a-4c18-a16c-888da9e228ba" (és nem ad meg a includeMetadata - implicit módon felirat látható, hogy nincsenek metaadatok szükség), azt a puffer majd összefűzésére hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="001e1-232">Example: hello following code requests 8 recommendations for item "64f6eb0d-947a-4c18-a16c-888da9e228ba" (and by not specifying includeMetadata - it implicitly says that no metadata is required), it then concatenate hello results into a buffer.</span></span>

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

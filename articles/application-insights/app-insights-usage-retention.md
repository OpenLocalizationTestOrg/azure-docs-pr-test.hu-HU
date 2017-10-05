---
title: "Megőrzési felhasználóelemzés webes alkalmazásokhoz az Azure Application insights szolgáltatással |} Microsoft docs"
description: "Hány felhasználó az alkalmazáshoz való visszatérésre?"
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: 7f7ca19ab171278bbd82f68e3822bc650b25373d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="b3384-103">Megőrzési felhasználóelemzés webes alkalmazásokhoz az Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b3384-103">User retention analysis for web applications with Application Insights</span></span>

<span data-ttu-id="b3384-104">A megőrzési szolgáltatásából [Azure Application Insights](app-insights-overview.md) hány felhasználó térjen vissza az alkalmazást, és milyen gyakran bizonyos feladatok végrehajtása, vagy célok elérése elemez nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="b3384-104">The retention feature in [Azure Application Insights](app-insights-overview.md) helps you analyze how many users return to your app, and how often they perform particular tasks or achieve goals.</span></span> <span data-ttu-id="b3384-105">Ha futtatja a game hely, például sikerült felhasználók számától játék elvesztése után térjen vissza a hely számára került ki nyertesen után térjen vissza a számok összehasonlítása.</span><span class="sxs-lookup"><span data-stu-id="b3384-105">For example, if you run a game site, you could compare the numbers of users who return to the site after losing a game with the number who return after winning.</span></span> <span data-ttu-id="b3384-106">A Tudásbázis segítségével javítható a felhasználói élmény és az üzleti stratégia is.</span><span class="sxs-lookup"><span data-stu-id="b3384-106">This knowledge can help you improve both your user experience and your business strategy.</span></span>

## <a name="get-started"></a><span data-ttu-id="b3384-107">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="b3384-107">Get started</span></span>

<span data-ttu-id="b3384-108">Ha még nem látja az Application Insights portáljáról, megőrzés eszközének adatok [megtudhatja, hogyan lásson a használati eszközök](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b3384-108">If you don't yet see data in the retention tool in the Application Insights portal, [learn how to get started with the usage tools](app-insights-usage-overview.md).</span></span>

## <a name="the-retention-tool"></a><span data-ttu-id="b3384-109">A megőrzési eszköz</span><span class="sxs-lookup"><span data-stu-id="b3384-109">The Retention tool</span></span>

![Megtartási eszköz](./media/app-insights-usage-retention/retention.png)

1. <span data-ttu-id="b3384-111">Az eszköztár lehetővé teszi a felhasználóknak új megőrzési jelentéseket készíthet, nyissa meg a meglévő adatmegőrzési jelentések, aktuális megőrzési jelentés mentése vagy menteni, mert visszaállítani a mentett jelentés végrehajtott módosításokat, a jelentés adatainak frissítése, e-mailben vagy közvetlen hivatkozást jelentés megosztása és a dokumentációja lap.</span><span class="sxs-lookup"><span data-stu-id="b3384-111">The toolbar allows users to create new retention reports, open existing retention reports, save current retention report or save as, revert changes made to saved reports, refresh data on the report, share report via email or direct link, and access the documentation page.</span></span> 
2. <span data-ttu-id="b3384-112">Alapértelmezés szerint a megőrzési minden felhasználó, aki nem végzett majd visszatért, és nem végzett más egy meghatározott időtartam során jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b3384-112">By default, retention shows all users who did anything then came back and did anything else over a period.</span></span> <span data-ttu-id="b3384-113">Kiválaszthatja, hogy az adott felhasználói tevékenységek szűkítése események különböző kombinációja.</span><span class="sxs-lookup"><span data-stu-id="b3384-113">You can select different combination of events to narrow the focus on specific user activities.</span></span>
3. <span data-ttu-id="b3384-114">Egy vagy több szűrők hozzáadása tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="b3384-114">Add one or more filters on properties.</span></span> <span data-ttu-id="b3384-115">Például összpontosíthat felhasználók egy adott országban vagy régióban.</span><span class="sxs-lookup"><span data-stu-id="b3384-115">For example, you can focus on users in a particular country or region.</span></span> <span data-ttu-id="b3384-116">Kattintson a **frissítés** a szűrők beállítását követően.</span><span class="sxs-lookup"><span data-stu-id="b3384-116">Click **Update** after setting the filters.</span></span> 
4. <span data-ttu-id="b3384-117">A teljes megőrzési diagram felhasználói megőrzési összegzését jeleníti meg a kijelölt időszakot között.</span><span class="sxs-lookup"><span data-stu-id="b3384-117">The overall retention chart shows a summary of user retention across the selected time period.</span></span> 
5. <span data-ttu-id="b3384-118">A rács őrzi meg a Lekérdezésszerkesztőben #2 szerint felhasználók számát mutatja.</span><span class="sxs-lookup"><span data-stu-id="b3384-118">The grid shows the number of users retained according to the query builder in #2.</span></span> <span data-ttu-id="b3384-119">Minden sor egy kohorszok időszak megjelenített idő bármely esemény elvégző felhasználók jelöli.</span><span class="sxs-lookup"><span data-stu-id="b3384-119">Each row represents a cohort of users who performed any event in the time period shown.</span></span> <span data-ttu-id="b3384-120">A sorban szereplő mindegyik cellához jeleníti meg, hány adott kohorszok visszaadott legalább egyszer újabb időn belül.</span><span class="sxs-lookup"><span data-stu-id="b3384-120">Each cell in the row shows how many of that cohort returned at least once in a later period.</span></span> <span data-ttu-id="b3384-121">Egyes felhasználók több időszakban adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="b3384-121">Some users may return in more than one period.</span></span> 
6. <span data-ttu-id="b3384-122">Az elemzések kártyák felső öt kezdeményező eseményeket, és lehetőséget nyújt a felhasználóknak egy szeretné jobban megismerni a megőrzési jelentés felső öt visszaadott események megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b3384-122">The insights cards show top five initiating events, and top five returned events to give users a better understanding of their retention report.</span></span> 

![Megőrzési egérrel való rámutatáskor használt](./media/app-insights-usage-retention/hover.png)

<span data-ttu-id="b3384-124">A megőrzési eszköz elmagyarázza, mit jelent a cella analytics gombra és eszköztippek eléréséhez a cellák felhasználók is mutasson.</span><span class="sxs-lookup"><span data-stu-id="b3384-124">Users can hover over cells on the retention tool to access the analytics button and tool tips explaining what the cell means.</span></span> <span data-ttu-id="b3384-125">Az elemzés gombra a felhasználók számára az előre megadott lekérdezéssel elemzőeszköz felhasználók generálása a cella vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b3384-125">The Analytics button takes users to the Analytics tool with a pre-populated query to generate users from the cell.</span></span> 

## <a name="use-business-events-to-track-retention"></a><span data-ttu-id="b3384-126">Az üzleti eseményeket segítségével nyomon követheti a megőrzési</span><span class="sxs-lookup"><span data-stu-id="b3384-126">Use business events to track retention</span></span>

<span data-ttu-id="b3384-127">Ahhoz, hogy a leghasznosabb megőrzési elemzés, mérjük az eseményeket, amelyek megfelelnek a jelentős üzleti tevékenységét.</span><span class="sxs-lookup"><span data-stu-id="b3384-127">To get the most useful retention analysis, measure events that represent significant business activities.</span></span> 

<span data-ttu-id="b3384-128">Például számos felhasználó előfordulhat, hogy a weblapot az alkalmazás megjeleníti a játék nélkül.</span><span class="sxs-lookup"><span data-stu-id="b3384-128">For example, many users might open a page in your app without playing the game that it displays.</span></span> <span data-ttu-id="b3384-129">Nyomon követése a lapmegtekintések ezért adjon volna pontos becslést hányan térjen vissza a játék korábban élvező azt követően.</span><span class="sxs-lookup"><span data-stu-id="b3384-129">Tracking just the page views would therefore provide an inaccurate estimate of how many people return to play the game after enjoying it previously.</span></span> <span data-ttu-id="b3384-130">Ahhoz, hogy az adatszolgáltató játékosok világossá, az alkalmazás küldjön egy egyéni eseményt, amikor egy felhasználó ténylegesen játszik.</span><span class="sxs-lookup"><span data-stu-id="b3384-130">To get a clear picture of returning players, your app should send a custom event when a user actually plays.</span></span>  

<span data-ttu-id="b3384-131">Tanácsos egyéni események, amelyek megfelelnek a fő üzleti műveletek code, és használhatja az adatmegőrzési elemzések végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="b3384-131">It's good practice to code custom events that represent key business actions, and use these for your retention analysis.</span></span> <span data-ttu-id="b3384-132">A game eredményének rögzítéséhez kell írnia egy egyéni eseményt küldeni az Application Insights kódsort.</span><span class="sxs-lookup"><span data-stu-id="b3384-132">To capture the game outcome, you need to write a line of code to send a custom event to Application Insights.</span></span> <span data-ttu-id="b3384-133">Ön írás a weblap kód vagy a Node.JS, néz ki:</span><span class="sxs-lookup"><span data-stu-id="b3384-133">If you write it in the web page code or in Node.JS, it looks like this:</span></span>

```JavaScript
    appinsights.trackEvent("won game");
```

<span data-ttu-id="b3384-134">Vagy az ASP.NET kiszolgálóoldali kódban:</span><span class="sxs-lookup"><span data-stu-id="b3384-134">Or in ASP.NET server code:</span></span>

```C#
   telemetry.TrackEvent("won game");
```

<span data-ttu-id="b3384-135">[További tudnivalók az egyéni események írása](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="b3384-135">[Learn more about writing custom events](app-insights-api-custom-events-metrics.md#trackevent).</span></span>


## <a name="next-steps"></a><span data-ttu-id="b3384-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3384-136">Next steps</span></span>
- <span data-ttu-id="b3384-137">Ahhoz, hogy a használati tapasztalatok, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="b3384-137">To enable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="b3384-138">Ha egyéni események vagy Lapmegtekintések már küld, megismerkedhet a használati eszközök további, a szolgáltatás használatát a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="b3384-138">If you already send custom events or page views, explore the Usage tools to learn how users use your service.</span></span>
    - [<span data-ttu-id="b3384-139">Felhasználók, munkamenetek, események</span><span class="sxs-lookup"><span data-stu-id="b3384-139">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="b3384-140">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="b3384-140">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="b3384-141">Felhasználói folyamatok</span><span class="sxs-lookup"><span data-stu-id="b3384-141">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="b3384-142">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="b3384-142">Workbooks</span></span>](app-insights-usage-workbooks.md)
    - [<span data-ttu-id="b3384-143">Felhasználói környezet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b3384-143">Add user context</span></span>](app-insights-usage-send-user-context.md)



---
title: "aaaUser megőrzési elemzés webes alkalmazásokhoz az Azure Application insights szolgáltatással |} Microsoft docs"
description: "Hány felhasználó vissza tooyour alkalmazást?"
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
ms.openlocfilehash: 8bcee5f1611afbd69016ec3eef27832c304762a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="20d19-103">Megőrzési felhasználóelemzés webes alkalmazásokhoz az Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="20d19-103">User retention analysis for web applications with Application Insights</span></span>

<span data-ttu-id="20d19-104">hello adatmegőrzési funkciója [Azure Application Insights](app-insights-overview.md) hány felhasználó elemez segítségével vissza tooyour alkalmazást, és milyen gyakran bizonyos feladatok végrehajtása, vagy célok elérése.</span><span class="sxs-lookup"><span data-stu-id="20d19-104">hello retention feature in [Azure Application Insights](app-insights-overview.md) helps you analyze how many users return tooyour app, and how often they perform particular tasks or achieve goals.</span></span> <span data-ttu-id="20d19-105">Ha futtatja a game hely, például sikerült hello számok felhasználók számára hello számú játék elvesztése után térjen vissza a toohello hely került ki nyertesen után vissza összehasonlítása.</span><span class="sxs-lookup"><span data-stu-id="20d19-105">For example, if you run a game site, you could compare hello numbers of users who return toohello site after losing a game with hello number who return after winning.</span></span> <span data-ttu-id="20d19-106">A Tudásbázis segítségével javítható a felhasználói élmény és az üzleti stratégia is.</span><span class="sxs-lookup"><span data-stu-id="20d19-106">This knowledge can help you improve both your user experience and your business strategy.</span></span>

## <a name="get-started"></a><span data-ttu-id="20d19-107">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="20d19-107">Get started</span></span>

<span data-ttu-id="20d19-108">Ha még nem látható adatok hello megőrzési eszközben hello Application Insights portál [megtudhatja, hogyan tooget lépések hello használati eszközök](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="20d19-108">If you don't yet see data in hello retention tool in hello Application Insights portal, [learn how tooget started with hello usage tools](app-insights-usage-overview.md).</span></span>

## <a name="hello-retention-tool"></a><span data-ttu-id="20d19-109">hello megőrzési eszköz</span><span class="sxs-lookup"><span data-stu-id="20d19-109">hello Retention tool</span></span>

![Megtartási eszköz](./media/app-insights-usage-retention/retention.png)

1. <span data-ttu-id="20d19-111">hello eszköztár lehetővé teszi a felhasználók toocreate új megőrzési jelentések, nyissa meg a meglévő adatmegőrzési jelentések, aktuális megőrzési jelentés mentése vagy a Mentés másként, állítsa vissza a végzett módosítások toosaved jelentések, hello jelentés, megosztás e-mailek vagy a közvetlen hivatkozást és a hozzáférés hello keresztül az adatok frissítése dokumentáció oldalán.</span><span class="sxs-lookup"><span data-stu-id="20d19-111">hello toolbar allows users toocreate new retention reports, open existing retention reports, save current retention report or save as, revert changes made toosaved reports, refresh data on hello report, share report via email or direct link, and access hello documentation page.</span></span> 
2. <span data-ttu-id="20d19-112">Alapértelmezés szerint a megőrzési minden felhasználó, aki nem végzett majd visszatért, és nem végzett más egy meghatározott időtartam során jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="20d19-112">By default, retention shows all users who did anything then came back and did anything else over a period.</span></span> <span data-ttu-id="20d19-113">Események toonarrow hello helyezi a hangsúlyt felhasználói tevékenységek különböző kombinációját kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="20d19-113">You can select different combination of events toonarrow hello focus on specific user activities.</span></span>
3. <span data-ttu-id="20d19-114">Egy vagy több szűrők hozzáadása tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="20d19-114">Add one or more filters on properties.</span></span> <span data-ttu-id="20d19-115">Például összpontosíthat felhasználók egy adott országban vagy régióban.</span><span class="sxs-lookup"><span data-stu-id="20d19-115">For example, you can focus on users in a particular country or region.</span></span> <span data-ttu-id="20d19-116">Kattintson a **frissítés** hello szűrők beállítását követően.</span><span class="sxs-lookup"><span data-stu-id="20d19-116">Click **Update** after setting hello filters.</span></span> 
4. <span data-ttu-id="20d19-117">hello általános megőrzési diagram összegzését jeleníti meg felhasználói megőrzési kijelölt időszakban hello között.</span><span class="sxs-lookup"><span data-stu-id="20d19-117">hello overall retention chart shows a summary of user retention across hello selected time period.</span></span> 
5. <span data-ttu-id="20d19-118">hello rács látható felhasználók száma hello őrzi meg a #2 függően toohello Lekérdezésszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="20d19-118">hello grid shows hello number of users retained according toohello query builder in #2.</span></span> <span data-ttu-id="20d19-119">Minden sor egy kohorszok hello időben időszak látható azon események elvégző felhasználók jelöli.</span><span class="sxs-lookup"><span data-stu-id="20d19-119">Each row represents a cohort of users who performed any event in hello time period shown.</span></span> <span data-ttu-id="20d19-120">Hello sorban szereplő mindegyik cellához jeleníti meg, hány adott kohorszok visszaadott legalább egyszer újabb időn belül.</span><span class="sxs-lookup"><span data-stu-id="20d19-120">Each cell in hello row shows how many of that cohort returned at least once in a later period.</span></span> <span data-ttu-id="20d19-121">Egyes felhasználók több időszakban adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="20d19-121">Some users may return in more than one period.</span></span> 
6. <span data-ttu-id="20d19-122">hello insights kártyák felső öt kezdeményező események megjelenítése, és legjobb öt visszaadott események toogive felhasználók kra a megőrzési jelentés.</span><span class="sxs-lookup"><span data-stu-id="20d19-122">hello insights cards show top five initiating events, and top five returned events toogive users a better understanding of their retention report.</span></span> 

![Megőrzési egérrel való rámutatáskor használt](./media/app-insights-usage-retention/hover.png)

<span data-ttu-id="20d19-124">Felhasználók is rámutat cellák hello megőrzési eszköz tooaccess hello analytics gombra, és azt jelenti, amely ismerteti, hogy milyen hello cella eszköztipp.</span><span class="sxs-lookup"><span data-stu-id="20d19-124">Users can hover over cells on hello retention tool tooaccess hello analytics button and tool tips explaining what hello cell means.</span></span> <span data-ttu-id="20d19-125">hello Analytics gomb felhasználók toohello Alkalmazáselemző eszköz, amely egy előre megadott lekérdezés toogenerate felhasználóival hello cella vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="20d19-125">hello Analytics button takes users toohello Analytics tool with a pre-populated query toogenerate users from hello cell.</span></span> 

## <a name="use-business-events-tootrack-retention"></a><span data-ttu-id="20d19-126">Üzleti események tootrack megőrzési használata</span><span class="sxs-lookup"><span data-stu-id="20d19-126">Use business events tootrack retention</span></span>

<span data-ttu-id="20d19-127">tooget hello leghasznosabb megőrzési elemzést követően mértéket események, amelyek megfelelnek a jelentős üzleti tevékenységét.</span><span class="sxs-lookup"><span data-stu-id="20d19-127">tooget hello most useful retention analysis, measure events that represent significant business activities.</span></span> 

<span data-ttu-id="20d19-128">Például számos felhasználó előfordulhat, hogy a weblapot az alkalmazás nélkül játék hello, amely azt jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="20d19-128">For example, many users might open a page in your app without playing hello game that it displays.</span></span> <span data-ttu-id="20d19-129">Csak hello Lapmegtekintések követési ezért adjon volna pontos becslést hányan tooplay hello játék után vissza élvező azt korábban.</span><span class="sxs-lookup"><span data-stu-id="20d19-129">Tracking just hello page views would therefore provide an inaccurate estimate of how many people return tooplay hello game after enjoying it previously.</span></span> <span data-ttu-id="20d19-130">az alkalmazás tooget játékosok adatszolgáltató világossá, küldjön egy egyéni eseményt, amikor egy felhasználó ténylegesen játszik.</span><span class="sxs-lookup"><span data-stu-id="20d19-130">tooget a clear picture of returning players, your app should send a custom event when a user actually plays.</span></span>  

<span data-ttu-id="20d19-131">Akkor célszerű toocode egyéni események, amelyek üzleti műveletek jelöl, és használhatja az adatmegőrzési elemzések végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="20d19-131">It's good practice toocode custom events that represent key business actions, and use these for your retention analysis.</span></span> <span data-ttu-id="20d19-132">toocapture hello játék eredménytől függően toowrite kód toosend egy egyéni esemény tooApplication Insights egy üzletági kell.</span><span class="sxs-lookup"><span data-stu-id="20d19-132">toocapture hello game outcome, you need toowrite a line of code toosend a custom event tooApplication Insights.</span></span> <span data-ttu-id="20d19-133">Ön írás hello weblap kód vagy a Node.JS, néz ki:</span><span class="sxs-lookup"><span data-stu-id="20d19-133">If you write it in hello web page code or in Node.JS, it looks like this:</span></span>

```JavaScript
    appinsights.trackEvent("won game");
```

<span data-ttu-id="20d19-134">Vagy az ASP.NET kiszolgálóoldali kódban:</span><span class="sxs-lookup"><span data-stu-id="20d19-134">Or in ASP.NET server code:</span></span>

```C#
   telemetry.TrackEvent("won game");
```

<span data-ttu-id="20d19-135">[További tudnivalók az egyéni események írása](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="20d19-135">[Learn more about writing custom events](app-insights-api-custom-events-metrics.md#trackevent).</span></span>


## <a name="next-steps"></a><span data-ttu-id="20d19-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="20d19-136">Next steps</span></span>
- <span data-ttu-id="20d19-137">tooenable használati észlel, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="20d19-137">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="20d19-138">Ha már küld egyéni események vagy Lapmegtekintések, így megismerkedhet hello használati eszközök toolearn hogyan felhasználók használhatja a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="20d19-138">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    - [<span data-ttu-id="20d19-139">Felhasználók, munkamenetek, események</span><span class="sxs-lookup"><span data-stu-id="20d19-139">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="20d19-140">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="20d19-140">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="20d19-141">Felhasználói folyamatok</span><span class="sxs-lookup"><span data-stu-id="20d19-141">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="20d19-142">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="20d19-142">Workbooks</span></span>](app-insights-usage-workbooks.md)
    - [<span data-ttu-id="20d19-143">Felhasználói környezet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20d19-143">Add user context</span></span>](app-insights-usage-send-user-context.md)



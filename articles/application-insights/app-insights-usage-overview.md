---
title: "Az Azure Application Insights-webalkalmazások számára történő használatelemzést |} Microsoft docs"
description: "Ismerje meg, a felhasználók és a webalkalmazással dolgukat."
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
ms.openlocfilehash: 63b74399790b718e14a5b6e09bc009a336caf928
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="usage-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="de95f-103">Az Application Insights-webalkalmazások számára történő használatának elemzése</span><span class="sxs-lookup"><span data-stu-id="de95f-103">Usage analysis for web applications with Application Insights</span></span>

<span data-ttu-id="de95f-104">A webes alkalmazás mely funkciók érhetők a legnépszerűbb?</span><span class="sxs-lookup"><span data-stu-id="de95f-104">Which features of your web app are most popular?</span></span> <span data-ttu-id="de95f-105">Hajtsa végre a felhasználók a kitűzött célokat a alkalmazással?</span><span class="sxs-lookup"><span data-stu-id="de95f-105">Do your users achieve their goals with your app?</span></span> <span data-ttu-id="de95f-106">Tegye azokat dobja el adott pontokon, és tegye ezeket térjen vissza később?</span><span class="sxs-lookup"><span data-stu-id="de95f-106">Do they drop out at particular points, and do they return later?</span></span>  <span data-ttu-id="de95f-107">[Az Azure Application Insights](app-insights-overview.md) használatának a webalkalmazás hatékony betekintést nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="de95f-107">[Azure Application Insights](app-insights-overview.md) helps you gain powerful insights into how people use your web app.</span></span> <span data-ttu-id="de95f-108">Minden alkalommal, amikor az alkalmazást frissíti, felmérheti, milyen jól működik a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="de95f-108">Every time you update your app, you can assess how well it works for users.</span></span> <span data-ttu-id="de95f-109">A Tudásbázis következő fejlesztési ciklusokkal kapcsolatos döntések adatvezérelt végezheti el.</span><span class="sxs-lookup"><span data-stu-id="de95f-109">With this knowledge, you can make data driven decisions about your next development cycles.</span></span>

## <a name="send-telemetry-from-your-app"></a><span data-ttu-id="de95f-110">Telemetriai adatokat küldhet az alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="de95f-110">Send telemetry from your app</span></span>

<span data-ttu-id="de95f-111">A legjobb élményt egységekre Application Insights telepítésével, az alkalmazás server kódját, mind a weblapokon.</span><span class="sxs-lookup"><span data-stu-id="de95f-111">The best experience is obtained by installing Application Insights both in your app server code, and in your web pages.</span></span> <span data-ttu-id="de95f-112">Az alkalmazás ügyfél- és összetevők vissza az Azure-portál a elemzés a telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="de95f-112">The client and server components of your app send telemetry back to the Azure portal for analysis.</span></span>

1. <span data-ttu-id="de95f-113">**Kiszolgálóoldali kódban:** telepítse a megfelelő modulját a [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), vagy [más](app-insights-platforms.md) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="de95f-113">**Server code:** Install the appropriate module for your [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), or [other](app-insights-platforms.md) app.</span></span>

    * <span data-ttu-id="de95f-114">*Nem szeretné telepíteni a kiszolgálóoldali kódban? Csak [hozzon létre egy Azure Application Insights-erőforrást](app-insights-create-new-resource.md).*</span><span class="sxs-lookup"><span data-stu-id="de95f-114">*Don't want to install server code? Just [create an Azure Application Insights resource](app-insights-create-new-resource.md).*</span></span>

2. <span data-ttu-id="de95f-115">**Weblap kód:** nyissa meg a [Azure-portálon](https://portal.azure.com), és nyissa meg az Application Insights-erőforrást az alkalmazáshoz, majd nyissa meg a **első lépések > figyelése és diagnosztizálása ügyféloldali**.</span><span class="sxs-lookup"><span data-stu-id="de95f-115">**Web page code:** Open the [Azure portal](https://portal.azure.com), open the Application Insights resource for your app, and then open **Getting Started > Monitor and Diagnose Client-Side**.</span></span> 

    ![Másolja a parancsfájlt a fő weblap head.](./media/app-insights-usage-overview/02-monitor-web-page.png)


3. <span data-ttu-id="de95f-117">**Telemetriai adatokat:** futtassa a projekt hibakeresési módban néhány percig, és keresse meg az Application Insights – áttekintés paneljén eredményez.</span><span class="sxs-lookup"><span data-stu-id="de95f-117">**Get telemetry:** Run your project in debug mode for a few minutes, and then look for results in the Overview blade in Application Insights.</span></span>

    <span data-ttu-id="de95f-118">Tegye közzé az alkalmazást, az az alkalmazás teljesítményének figyelése, és kideríti, hogy a felhasználók tevékenységeit az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="de95f-118">Publish your app to monitor your app's performance and find out what your users are doing with your app.</span></span>

## <a name="include-user-and-session-id-in-your-telemetry"></a><span data-ttu-id="de95f-119">Tartalmazza a felhasználó és a munkamenet-Azonosítót a telemetria</span><span class="sxs-lookup"><span data-stu-id="de95f-119">Include user and session ID in your telemetry</span></span>
<span data-ttu-id="de95f-120">Felhasználók nyomon követése adott idő alatt, az Application Insights előírja, hogy azonosítani tudja azokat.</span><span class="sxs-lookup"><span data-stu-id="de95f-120">To track users over time, Application Insights requires a way to identify them.</span></span> <span data-ttu-id="de95f-121">Az események eszköz a csak használati eszköz, amely nem igényel felhasználói azonosító vagy munkamenet-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="de95f-121">The Events tool is the only Usage tool that does not require a user ID or a session ID.</span></span>

<span data-ttu-id="de95f-122">Értesítésküldés ezek azonosítók [Itt](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span><span class="sxs-lookup"><span data-stu-id="de95f-122">Start sending these IDs [here](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span></span>

## <a name="explore-usage-demographics-and-statistics"></a><span data-ttu-id="de95f-123">Megismerkedhet a használati demográfiai adatok és statisztikák</span><span class="sxs-lookup"><span data-stu-id="de95f-123">Explore usage demographics and statistics</span></span>
<span data-ttu-id="de95f-124">Ismerje meg az alkalmazás használatakor a személyek, azok még legtöbb érdekli, ahol a felhasználók találhatók oldalak, milyen böngészők és operációs rendszerek használnak.</span><span class="sxs-lookup"><span data-stu-id="de95f-124">Find out when people use your app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> 

<span data-ttu-id="de95f-125">A felhasználók és a munkamenetek jelentések szűrje az adatokat lapok vagy egyéni események, és szegmentálja őket például hely, a környezet és a lap tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="de95f-125">The Users and Sessions reports filter your data by pages or custom events, and segment them by properties such as location, environment, and page.</span></span> <span data-ttu-id="de95f-126">A saját szűrőt is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="de95f-126">You can also add your own filters.</span></span>

![Felhasználók](./media/app-insights-usage-overview/users.png)  

<span data-ttu-id="de95f-128">A jobb oldali insights mutasson, hogy érdekes szabályszerűségeket készletében lévő adatok.</span><span class="sxs-lookup"><span data-stu-id="de95f-128">Insights on the right point out interesting patterns in the set of data.</span></span>  

* <span data-ttu-id="de95f-129">A **felhasználók** jelentés megjeleníti a kiválasztott időtartamon belül a lapok hozzáférő egyedi felhasználók számát.</span><span class="sxs-lookup"><span data-stu-id="de95f-129">The **Users** report counts the numbers of unique users that access your pages within your chosen time periods.</span></span> <span data-ttu-id="de95f-130">(A felhasználók a cookie-k használatával számítanak.</span><span class="sxs-lookup"><span data-stu-id="de95f-130">(Users are counted by using cookies.</span></span> <span data-ttu-id="de95f-131">Ha valaki hozzáfér a webhelyet a különböző böngészők vagy ügyfélszámítógépre, vagy törli a cookie-k, majd azok megszámlálandó egynél többször.)</span><span class="sxs-lookup"><span data-stu-id="de95f-131">If someone accesses your site with different browsers or client machines, or clears their cookies, then they will be counted more than once.)</span></span>
* <span data-ttu-id="de95f-132">A **munkamenetek** jelentés megszámlálása felhasználói munkamenetek, amelyek hozzáférhetnek a webhelyhez.</span><span class="sxs-lookup"><span data-stu-id="de95f-132">The **Sessions** report counts the number of user sessions that access your site.</span></span> <span data-ttu-id="de95f-133">A munkamenet a felhasználó megszakította a több mint fél óra inaktivitási tevékenység belül.</span><span class="sxs-lookup"><span data-stu-id="de95f-133">A session is a period of activity by a user, terminated by a period of inactivity of more than half an hour.</span></span>

[<span data-ttu-id="de95f-134">További információ a felhasználók, a munkamenetek és az események eszközök</span><span class="sxs-lookup"><span data-stu-id="de95f-134">More about the Users, Sessions, and Events tools</span></span>](app-insights-usage-segmentation.md)  

## <a name="page-views"></a><span data-ttu-id="de95f-135">Lapmegtekintések</span><span class="sxs-lookup"><span data-stu-id="de95f-135">Page views</span></span>

<span data-ttu-id="de95f-136">A használati paneljén kattintson a lapmegtekintések csempén a legnépszerűbb lap részletes információkat:</span><span class="sxs-lookup"><span data-stu-id="de95f-136">From the Usage blade, click through the Page Views tile to get a breakdown of your most popular pages:</span></span>

![Áttekintés paneljén kattintson a lap nézetek diagram](./media/app-insights-usage-overview/05-games.png)

<span data-ttu-id="de95f-138">A fenti példa egy játékok webhelyről.</span><span class="sxs-lookup"><span data-stu-id="de95f-138">The example above is from a games web site.</span></span> <span data-ttu-id="de95f-139">A diagramok azonnal láthatja:</span><span class="sxs-lookup"><span data-stu-id="de95f-139">From the charts, we can instantly see:</span></span>

* <span data-ttu-id="de95f-140">Használati az elmúlt héten nem javult.</span><span class="sxs-lookup"><span data-stu-id="de95f-140">Usage hasn't improved in the past week.</span></span> <span data-ttu-id="de95f-141">Lehet, hogy meg kell fontolnunk a optimalizálás érdekében?</span><span class="sxs-lookup"><span data-stu-id="de95f-141">Maybe we should think about search engine optimization?</span></span>
* <span data-ttu-id="de95f-142">Tenisz a legnépszerűbb játékok lapon.</span><span class="sxs-lookup"><span data-stu-id="de95f-142">Tennis is the most popular game page.</span></span> <span data-ttu-id="de95f-143">Ez a lap további fejlesztései lehetővé összpontosítanak.</span><span class="sxs-lookup"><span data-stu-id="de95f-143">Let's focus on further improvements to this page.</span></span>
* <span data-ttu-id="de95f-144">Átlagosan felhasználók által felkeresett a tenisz lap készül háromszor hetente.</span><span class="sxs-lookup"><span data-stu-id="de95f-144">On average, users visit the Tennis page about three times per week.</span></span> <span data-ttu-id="de95f-145">(Nincsenek felhasználók mint három-szer több munkamenetek.)</span><span class="sxs-lookup"><span data-stu-id="de95f-145">(There are about three times more sessions than users.)</span></span>
* <span data-ttu-id="de95f-146">A legtöbb felhasználó keresse fel a webhelyet, az USA működő héten, és a munkaidő alatt.</span><span class="sxs-lookup"><span data-stu-id="de95f-146">Most users visit the site during the U.S. working week, and in working hours.</span></span> <span data-ttu-id="de95f-147">Lehet, hogy a "gyors elrejtése" gombra kell nyújtunk a weblapon.</span><span class="sxs-lookup"><span data-stu-id="de95f-147">Perhaps we should provide a "quick hide" button on the web page.</span></span>
* <span data-ttu-id="de95f-148">A [jegyzetek](app-insights-annotations.md) a diagram megjelenítése, ha telepítve vannak-e az új verziókat a webhely.</span><span class="sxs-lookup"><span data-stu-id="de95f-148">The [annotations](app-insights-annotations.md) on the chart show when new versions of the website were deployed.</span></span> <span data-ttu-id="de95f-149">A legutóbbi központi telepítések egyike észrevehető hatást használata.</span><span class="sxs-lookup"><span data-stu-id="de95f-149">None of the recent deployments had a noticeable effect on usage.</span></span>

<span data-ttu-id="de95f-150">Mi történik, ha vizsgálni kívánt részletesen, például egy egyéni tulajdonság, a lap nézet telemetriai adatokat küldi el a hely által a felosztás webhelyekhez forgalom?</span><span class="sxs-lookup"><span data-stu-id="de95f-150">What if you want to investigate the traffic to your site in more detail, like splitting by a custom property your site sends in its page view telemetry?</span></span>

1. <span data-ttu-id="de95f-151">Nyissa meg a **események** eszköz az Application Insights-erőforrás menüjében.</span><span class="sxs-lookup"><span data-stu-id="de95f-151">Open the **Events** tool in the Application Insights resource menu.</span></span> <span data-ttu-id="de95f-152">Ez az eszköz lehetővé teszi, hogy hány Lapmegtekintések és egyéni események az alkalmazásból, a különböző szűrési, cohorting és Szegmentálás beállítások alapján küldött elemezheti.</span><span class="sxs-lookup"><span data-stu-id="de95f-152">This tool lets you analyze how many page views and custom events were sent from your app, based on a variety of filtering, cohorting, and segmentation options.</span></span>
2. <span data-ttu-id="de95f-153">A "Ki használt" legördülő listában jelölje ki "Any lap nézet".</span><span class="sxs-lookup"><span data-stu-id="de95f-153">In the "Who used" dropdown, select "Any Page View".</span></span>
3. <span data-ttu-id="de95f-154">A "Által megosztott" legördülő listában válassza ki ossza fel a lap nézet telemetriai egy tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="de95f-154">In the "Split by" dropdown, select a property by which to split your page view telemetry.</span></span>

## <a name="retention---how-many-users-come-back"></a><span data-ttu-id="de95f-155">Megőrzési - hány felhasználó térjen vissza?</span><span class="sxs-lookup"><span data-stu-id="de95f-155">Retention - how many users come back?</span></span>

<span data-ttu-id="de95f-156">Megőrzési segítségével megismerheti, hogy milyen gyakran térjen vissza a felhasználók az alkalmazással, a felhasználók által végrehajtott bizonyos üzleti művelet során egy bizonyos idő gyűjtőben cohorts alapján.</span><span class="sxs-lookup"><span data-stu-id="de95f-156">Retention helps you understand how often your users return to use their app, based on cohorts of users that performed some business action during a certain time bucket.</span></span> 

- <span data-ttu-id="de95f-157">Milyen funkciók miatt a felhasználók számára, térjen vissza több, mint a többire ismertetése</span><span class="sxs-lookup"><span data-stu-id="de95f-157">Understand what specific features cause users to come back more than others</span></span> 
- <span data-ttu-id="de95f-158">Űrlap feltételezéseket valós felhasználói adatok alapján</span><span class="sxs-lookup"><span data-stu-id="de95f-158">Form hypotheses based on real user data</span></span> 
- <span data-ttu-id="de95f-159">Annak megállapítása, hogy megőrzési a termékkel kapcsolatos probléma</span><span class="sxs-lookup"><span data-stu-id="de95f-159">Determine whether retention is a problem in your product</span></span> 

![Megőrzés](./media/app-insights-usage-overview/retention.png) 

<span data-ttu-id="de95f-161">A megőrzési vezérlők felül lehetővé teszik meg adott események és időtartomány megőrzési kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="de95f-161">The retention controls on top allow you to define specific events and time range to calculate retention.</span></span> <span data-ttu-id="de95f-162">A diagram a középső által az időtartomány lett megadva az összes megőrzési százalékos vizuális ábrázolását biztosítja.</span><span class="sxs-lookup"><span data-stu-id="de95f-162">The graph in the middle gives a visual representation of the overall retention percentage by the time range specified.</span></span> <span data-ttu-id="de95f-163">A grafikon alján egy adott időszakra vonatkozó egyéni megőrzési jelöli.</span><span class="sxs-lookup"><span data-stu-id="de95f-163">The graph on the bottom represents individual retention in a given time period.</span></span> <span data-ttu-id="de95f-164">Részletesség ilyen szintje lehetővé teszi, hogy a felhasználók tevékenységeit, és milyen hatással lehetnek a részletesebb lépésköz adatszolgáltató felhasználók megértését.</span><span class="sxs-lookup"><span data-stu-id="de95f-164">This level of detail allows you to understand what your users are doing and what might affect returning users on a more detailed granularity.</span></span>  

[<span data-ttu-id="de95f-165">További információ a megőrzési eszköz</span><span class="sxs-lookup"><span data-stu-id="de95f-165">More about the Retention tool</span></span>](app-insights-usage-retention.md)

## <a name="custom-business-events"></a><span data-ttu-id="de95f-166">Egyéni üzleti események</span><span class="sxs-lookup"><span data-stu-id="de95f-166">Custom business events</span></span>

<span data-ttu-id="de95f-167">Ahhoz, hogy pontosan ismeri a felhasználók mit a webalkalmazással, akkor célszerű beszúrása sornyi kód egyéni események naplózása.</span><span class="sxs-lookup"><span data-stu-id="de95f-167">To get a clear understanding of what users do with your web app, it's useful to insert lines of code to log custom events.</span></span> <span data-ttu-id="de95f-168">Ezeket az eseményeket is nyomon semmit részletes felhasználói műveletek, például adott gombokkal, például vásárol, vagy egy játékot győzelmével több jelentős üzleti eseményeket.</span><span class="sxs-lookup"><span data-stu-id="de95f-168">These events can track anything from detailed user actions such as clicking specific buttons, to more significant business events such as making a purchase or winning a game.</span></span> 

<span data-ttu-id="de95f-169">Bár bizonyos esetekben a lapmegtekintések hasznos események jelenthet, nem igaz általában.</span><span class="sxs-lookup"><span data-stu-id="de95f-169">Although in some cases, page views can represent useful events, it isn't true in general.</span></span> <span data-ttu-id="de95f-170">A felhasználó megnyithatja a termék oldalát a termék vásárlás nélkül.</span><span class="sxs-lookup"><span data-stu-id="de95f-170">A user can open a product page without buying the product.</span></span> 

<span data-ttu-id="de95f-171">Az adott üzleti eseményeket a webhelyen keresztül is diagram a felhasználók folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="de95f-171">With specific business events, you can chart your users' progress through your site.</span></span> <span data-ttu-id="de95f-172">Megtudhatja, a beállítások a különböző lehetőségek közül, és beállíthatja, amennyiben azok dobja el vagy megszüntetésekor nehézségekbe.</span><span class="sxs-lookup"><span data-stu-id="de95f-172">You can find out their preferences for different options, and where they drop out or have difficulties.</span></span> <span data-ttu-id="de95f-173">Ennek az információnak a tehet a fejlesztési várakozó fájlok számát a prioritásokkal kapcsolatos kérdésekre vonatkozó döntések.</span><span class="sxs-lookup"><span data-stu-id="de95f-173">With this knowledge, you can make informed decisions about the priorities in your development backlog.</span></span>

<span data-ttu-id="de95f-174">A weblap bejelentkezhet események:</span><span class="sxs-lookup"><span data-stu-id="de95f-174">Events can be logged in the web page:</span></span>

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

<span data-ttu-id="de95f-175">Vagy a web app server oldalán:</span><span class="sxs-lookup"><span data-stu-id="de95f-175">Or in the server side of the web app:</span></span>

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

<span data-ttu-id="de95f-176">Ezeket az eseményeket, hogy szűrje, vagy ossza fel az eseményeket, amikor a portál vizsgálhatja csatolhat tulajdonságértékek.</span><span class="sxs-lookup"><span data-stu-id="de95f-176">You can attach property values to these events, so that you can filter or split the events when you inspect them in the portal.</span></span> <span data-ttu-id="de95f-177">Emellett minden esemény, például a névtelen felhasználói Azonosítóját, amely lehetővé teszi egy adott felhasználó-tevékenységek sorrendjének követéséhez tulajdonságok szabványos készletét van csatolva.</span><span class="sxs-lookup"><span data-stu-id="de95f-177">In addition, a standard set of properties is attached to each event, such as anonymous user ID, which allows you to trace the sequence of activities of an individual user.</span></span>

<span data-ttu-id="de95f-178">További információ [egyéni események](app-insights-api-custom-events-metrics.md#trackevent) és [tulajdonságok](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="de95f-178">Learn more about [custom events](app-insights-api-custom-events-metrics.md#trackevent) and [properties](app-insights-api-custom-events-metrics.md#properties).</span></span>

### <a name="slice-and-dice-events"></a><span data-ttu-id="de95f-179">Szeletelésére és feldarabolására használnak események</span><span class="sxs-lookup"><span data-stu-id="de95f-179">Slice and dice events</span></span>

<span data-ttu-id="de95f-180">A felhasználók, a munkamenetek és az események eszközök akkor is részletekbe menően felhasználó, az esemény neve és a Tulajdonságok egyéni események.</span><span class="sxs-lookup"><span data-stu-id="de95f-180">In the Users, Sessions, and Events tools, you can slice and dice custom events by user, event name, and properties.</span></span>
<span data-ttu-id="de95f-181">![Felhasználók](./media/app-insights-usage-overview/users.png)</span><span class="sxs-lookup"><span data-stu-id="de95f-181">![Users](./media/app-insights-usage-overview/users.png)</span></span>  
  
## <a name="design-the-telemetry-with-the-app"></a><span data-ttu-id="de95f-182">A telemetriai adatok és az alkalmazás tervezése</span><span class="sxs-lookup"><span data-stu-id="de95f-182">Design the telemetry with the app</span></span>

<span data-ttu-id="de95f-183">Az alkalmazás egyes szolgáltatások tervez, vegye figyelembe hogyan kívánja a sikeresség felméréséhez a felhasználóival.</span><span class="sxs-lookup"><span data-stu-id="de95f-183">When you are designing each feature of your app, consider how you are going to measure its success with your users.</span></span> <span data-ttu-id="de95f-184">Döntse el, milyen rögzítendő az üzleti eseményeket, és a követési ezeket az eseményeket az alkalmazásba hívásokat a kezdetektől kódot.</span><span class="sxs-lookup"><span data-stu-id="de95f-184">Decide what business events you need to record, and code the tracking calls for those events into your app from the start.</span></span>

## <a name="a--b-testing"></a><span data-ttu-id="de95f-185">A |} B tesztelés</span><span class="sxs-lookup"><span data-stu-id="de95f-185">A | B Testing</span></span>
<span data-ttu-id="de95f-186">Ha nem tudja, melyik variant egy szolgáltatás több sikeres lesz, a kiadási helyezni őket, így minden elérhető különböző felhasználók.</span><span class="sxs-lookup"><span data-stu-id="de95f-186">If you don't know which variant of a feature will be more successful, release both of them, making each accessible to different users.</span></span> <span data-ttu-id="de95f-187">Az egyes alkalmazása sikerének mérésében, és anélkül helyezhet át egy egységes verziót.</span><span class="sxs-lookup"><span data-stu-id="de95f-187">Measure the success of each, and then move to a unified version.</span></span>

<span data-ttu-id="de95f-188">Ezzel a módszerrel a csatol különböző tulajdonságértékek minden verzió az alkalmazás által küldött összes telemetriai.</span><span class="sxs-lookup"><span data-stu-id="de95f-188">For this technique, you attach distinct property values to all the telemetry that is sent by each version of your app.</span></span> <span data-ttu-id="de95f-189">Azt az aktív TelemetryContext a tulajdonságok megadásával teheti meg.</span><span class="sxs-lookup"><span data-stu-id="de95f-189">You can do that by defining properties in the active TelemetryContext.</span></span> <span data-ttu-id="de95f-190">Minden telemetriai üzenet, amely elküldi az alkalmazás - nem csak az egyéni üzenetek, de a szabványos telemetriai ezek alapértelmezett tulajdonságokkal is bővül.</span><span class="sxs-lookup"><span data-stu-id="de95f-190">These default properties are added to every telemetry message that the application sends - not just your custom messages, but the standard telemetry as well.</span></span>

<span data-ttu-id="de95f-191">Az Application Insights portálon szűréséhez, és ossza fel a tulajdonságértékek esetén, hogy különböző hasonlítsa össze az adatokat.</span><span class="sxs-lookup"><span data-stu-id="de95f-191">In the Application Insights portal, filter and split your data on the property values, so as to compare the different versions.</span></span>

<span data-ttu-id="de95f-192">Ehhez a [állítson be egy telemetriai inicializáló](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span><span class="sxs-lookup"><span data-stu-id="de95f-192">To do this, [set up a telemetry initializer](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span></span>

```C#


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

<span data-ttu-id="de95f-193">A webes alkalmazás inicializáló Global.asax.cs például:</span><span class="sxs-lookup"><span data-stu-id="de95f-193">In the web app initializer such as Global.asax.cs:</span></span>

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

<span data-ttu-id="de95f-194">Minden új TelemetryClients automatikusan hozzáadja a megadott tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="de95f-194">All new TelemetryClients automatically add the property value you specify.</span></span> <span data-ttu-id="de95f-195">Egyéni telemetriai események felülbírálhatja az alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="de95f-195">Individual telemetry events can override the default values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de95f-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de95f-196">Next steps</span></span>
   - [<span data-ttu-id="de95f-197">Felhasználók, munkamenetek, események</span><span class="sxs-lookup"><span data-stu-id="de95f-197">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
   - [<span data-ttu-id="de95f-198">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="de95f-198">Funnels</span></span>](usage-funnels.md)
   - [<span data-ttu-id="de95f-199">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="de95f-199">Retention</span></span>](app-insights-usage-retention.md)
   - [<span data-ttu-id="de95f-200">Felhasználói folyamatok</span><span class="sxs-lookup"><span data-stu-id="de95f-200">User Flows</span></span>](app-insights-usage-flows.md)
   - [<span data-ttu-id="de95f-201">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="de95f-201">Workbooks</span></span>](app-insights-usage-workbooks.md)
   - [<span data-ttu-id="de95f-202">Felhasználói környezet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="de95f-202">Add user context</span></span>](app-insights-usage-send-user-context.md)

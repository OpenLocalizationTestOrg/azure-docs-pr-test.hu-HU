---
title: "aaaUsage elemzés webes alkalmazásokhoz az Azure Application insights szolgáltatással |} Microsoft docs"
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
ms.openlocfilehash: f7f9173cf411fa0d2dfb3b5ba99134a02bbc0e89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="usage-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="59231-103">Az Application Insights-webalkalmazások számára történő használatának elemzése</span><span class="sxs-lookup"><span data-stu-id="59231-103">Usage analysis for web applications with Application Insights</span></span>

<span data-ttu-id="59231-104">A webes alkalmazás mely funkciók érhetők a legnépszerűbb?</span><span class="sxs-lookup"><span data-stu-id="59231-104">Which features of your web app are most popular?</span></span> <span data-ttu-id="59231-105">Hajtsa végre a felhasználók a kitűzött célokat a alkalmazással?</span><span class="sxs-lookup"><span data-stu-id="59231-105">Do your users achieve their goals with your app?</span></span> <span data-ttu-id="59231-106">Tegye azokat dobja el adott pontokon, és tegye ezeket térjen vissza később?</span><span class="sxs-lookup"><span data-stu-id="59231-106">Do they drop out at particular points, and do they return later?</span></span>  <span data-ttu-id="59231-107">[Az Azure Application Insights](app-insights-overview.md) használatának a webalkalmazás hatékony betekintést nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="59231-107">[Azure Application Insights](app-insights-overview.md) helps you gain powerful insights into how people use your web app.</span></span> <span data-ttu-id="59231-108">Minden alkalommal, amikor az alkalmazást frissíti, felmérheti, milyen jól működik a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="59231-108">Every time you update your app, you can assess how well it works for users.</span></span> <span data-ttu-id="59231-109">A Tudásbázis következő fejlesztési ciklusokkal kapcsolatos döntések adatvezérelt végezheti el.</span><span class="sxs-lookup"><span data-stu-id="59231-109">With this knowledge, you can make data driven decisions about your next development cycles.</span></span>

## <a name="send-telemetry-from-your-app"></a><span data-ttu-id="59231-110">Telemetriai adatokat küldhet az alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="59231-110">Send telemetry from your app</span></span>

<span data-ttu-id="59231-111">az Application Insights telepítésével, az alkalmazás server kódját, mind a weblapok hello legjobb élmény egységekre.</span><span class="sxs-lookup"><span data-stu-id="59231-111">hello best experience is obtained by installing Application Insights both in your app server code, and in your web pages.</span></span> <span data-ttu-id="59231-112">az alkalmazás ügyfél- és összetevők hello küldése telemetriai hátsó toohello elemzése az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="59231-112">hello client and server components of your app send telemetry back toohello Azure portal for analysis.</span></span>

1. <span data-ttu-id="59231-113">**Kiszolgálóoldali kódban:** telepítés hello megfelelő modul a a [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), vagy [más](app-insights-platforms.md) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="59231-113">**Server code:** Install hello appropriate module for your [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), or [other](app-insights-platforms.md) app.</span></span>

    * <span data-ttu-id="59231-114">*Nem szeretnék tooinstall server kódot? Csak [hozzon létre egy Azure Application Insights-erőforrást](app-insights-create-new-resource.md).*</span><span class="sxs-lookup"><span data-stu-id="59231-114">*Don't want tooinstall server code? Just [create an Azure Application Insights resource](app-insights-create-new-resource.md).*</span></span>

2. <span data-ttu-id="59231-115">**Weblap kód:** nyitott hello [Azure-portálon](https://portal.azure.com), nyissa meg a hello Application Insights-erőforrást az alkalmazáshoz, és nyisson **első lépések > figyelése és diagnosztizálása ügyféloldali**.</span><span class="sxs-lookup"><span data-stu-id="59231-115">**Web page code:** Open hello [Azure portal](https://portal.azure.com), open hello Application Insights resource for your app, and then open **Getting Started > Monitor and Diagnose Client-Side**.</span></span> 

    ![Másolja a fő weblap hello head hello parancsfájl.](./media/app-insights-usage-overview/02-monitor-web-page.png)


3. <span data-ttu-id="59231-117">**Telemetriai adatokat:** futtassa a projekt hibakeresési módban néhány percig, és keresse a hello áttekintése panel az Application Insights eredményez.</span><span class="sxs-lookup"><span data-stu-id="59231-117">**Get telemetry:** Run your project in debug mode for a few minutes, and then look for results in hello Overview blade in Application Insights.</span></span>

    <span data-ttu-id="59231-118">Az alkalmazás toomonitor közzététele az alkalmazás teljesítményét, és kideríti, hogy a felhasználók tevékenységeit az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="59231-118">Publish your app toomonitor your app's performance and find out what your users are doing with your app.</span></span>

## <a name="include-user-and-session-id-in-your-telemetry"></a><span data-ttu-id="59231-119">Tartalmazza a felhasználó és a munkamenet-Azonosítót a telemetria</span><span class="sxs-lookup"><span data-stu-id="59231-119">Include user and session ID in your telemetry</span></span>
<span data-ttu-id="59231-120">tootrack felhasználók adott idő alatt, az Application Insights szükséges egy módon tooidentify őket.</span><span class="sxs-lookup"><span data-stu-id="59231-120">tootrack users over time, Application Insights requires a way tooidentify them.</span></span> <span data-ttu-id="59231-121">az eszköz hello események hello csak használati eszköz, amely nem igényel felhasználói azonosító vagy munkamenet-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="59231-121">hello Events tool is hello only Usage tool that does not require a user ID or a session ID.</span></span>

<span data-ttu-id="59231-122">Értesítésküldés ezek azonosítók [Itt](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span><span class="sxs-lookup"><span data-stu-id="59231-122">Start sending these IDs [here](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span></span>

## <a name="explore-usage-demographics-and-statistics"></a><span data-ttu-id="59231-123">Megismerkedhet a használati demográfiai adatok és statisztikák</span><span class="sxs-lookup"><span data-stu-id="59231-123">Explore usage demographics and statistics</span></span>
<span data-ttu-id="59231-124">Ismerje meg az alkalmazás használatakor a személyek, azok még legtöbb érdekli, ahol a felhasználók találhatók oldalak, milyen böngészők és operációs rendszerek használnak.</span><span class="sxs-lookup"><span data-stu-id="59231-124">Find out when people use your app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> 

<span data-ttu-id="59231-125">Felhasználók és a munkamenetek jelentések hello szűrje az adatokat lapok vagy egyéni események és szegmentálja őket például hely, a környezet és a lap tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="59231-125">hello Users and Sessions reports filter your data by pages or custom events, and segment them by properties such as location, environment, and page.</span></span> <span data-ttu-id="59231-126">A saját szűrőt is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="59231-126">You can also add your own filters.</span></span>

![Felhasználók](./media/app-insights-usage-overview/users.png)  

<span data-ttu-id="59231-128">A jobb oldali hello insights pont hogy érdekes szabályszerűségeket hello készletében lévő adatok.</span><span class="sxs-lookup"><span data-stu-id="59231-128">Insights on hello right point out interesting patterns in hello set of data.</span></span>  

* <span data-ttu-id="59231-129">Hello **felhasználók** jelentés hello számokat a megadott időtartamon belül a lapok hozzáférő egyedi felhasználók száma.</span><span class="sxs-lookup"><span data-stu-id="59231-129">hello **Users** report counts hello numbers of unique users that access your pages within your chosen time periods.</span></span> <span data-ttu-id="59231-130">(A felhasználók a cookie-k használatával számítanak.</span><span class="sxs-lookup"><span data-stu-id="59231-130">(Users are counted by using cookies.</span></span> <span data-ttu-id="59231-131">Ha valaki hozzáfér a webhelyet a különböző böngészők vagy ügyfélszámítógépre, vagy törli a cookie-k, majd azok megszámlálandó egynél többször.)</span><span class="sxs-lookup"><span data-stu-id="59231-131">If someone accesses your site with different browsers or client machines, or clears their cookies, then they will be counted more than once.)</span></span>
* <span data-ttu-id="59231-132">Hello **munkamenetek** jelentés megjeleníti a felhasználói munkamenetek, amelyek hozzáférhetnek a webhelyhez hello számát.</span><span class="sxs-lookup"><span data-stu-id="59231-132">hello **Sessions** report counts hello number of user sessions that access your site.</span></span> <span data-ttu-id="59231-133">A munkamenet a felhasználó megszakította a több mint fél óra inaktivitási tevékenység belül.</span><span class="sxs-lookup"><span data-stu-id="59231-133">A session is a period of activity by a user, terminated by a period of inactivity of more than half an hour.</span></span>

[<span data-ttu-id="59231-134">További információ az hello felhasználók munkamenetek és események eszközök</span><span class="sxs-lookup"><span data-stu-id="59231-134">More about hello Users, Sessions, and Events tools</span></span>](app-insights-usage-segmentation.md)  

## <a name="page-views"></a><span data-ttu-id="59231-135">Lapmegtekintések</span><span class="sxs-lookup"><span data-stu-id="59231-135">Page views</span></span>

<span data-ttu-id="59231-136">Hello használati panelen kattintson a hello Lapmegtekintések csempe tooget a legnépszerűbb lap részletes információkat:</span><span class="sxs-lookup"><span data-stu-id="59231-136">From hello Usage blade, click through hello Page Views tile tooget a breakdown of your most popular pages:</span></span>

![Hello áttekintése panelen kattintson a lap nézetek diagram hello](./media/app-insights-usage-overview/05-games.png)

<span data-ttu-id="59231-138">hello fenti példája egy játékok webhelyről.</span><span class="sxs-lookup"><span data-stu-id="59231-138">hello example above is from a games web site.</span></span> <span data-ttu-id="59231-139">A hello diagramokat azonnal láthatja:</span><span class="sxs-lookup"><span data-stu-id="59231-139">From hello charts, we can instantly see:</span></span>

* <span data-ttu-id="59231-140">Használati még tovább hello a múlt hét.</span><span class="sxs-lookup"><span data-stu-id="59231-140">Usage hasn't improved in hello past week.</span></span> <span data-ttu-id="59231-141">Lehet, hogy meg kell fontolnunk a optimalizálás érdekében?</span><span class="sxs-lookup"><span data-stu-id="59231-141">Maybe we should think about search engine optimization?</span></span>
* <span data-ttu-id="59231-142">Tenisz hello legnépszerűbb játék lap.</span><span class="sxs-lookup"><span data-stu-id="59231-142">Tennis is hello most popular game page.</span></span> <span data-ttu-id="59231-143">További fejlesztések toothis lap most összpontosítanak.</span><span class="sxs-lookup"><span data-stu-id="59231-143">Let's focus on further improvements toothis page.</span></span>
* <span data-ttu-id="59231-144">Átlagosan felhasználó által meglátogatott hello tenisz lap készül háromszor hetente.</span><span class="sxs-lookup"><span data-stu-id="59231-144">On average, users visit hello Tennis page about three times per week.</span></span> <span data-ttu-id="59231-145">(Nincsenek felhasználók mint három-szer több munkamenetek.)</span><span class="sxs-lookup"><span data-stu-id="59231-145">(There are about three times more sessions than users.)</span></span>
* <span data-ttu-id="59231-146">A legtöbb felhasználó webhelyen hello hello USA heti során, és a munkaidő alatt.</span><span class="sxs-lookup"><span data-stu-id="59231-146">Most users visit hello site during hello U.S. working week, and in working hours.</span></span> <span data-ttu-id="59231-147">Lehet, hogy a "gyors elrejtése" gombra kell nyújtunk hello weblapon.</span><span class="sxs-lookup"><span data-stu-id="59231-147">Perhaps we should provide a "quick hide" button on hello web page.</span></span>
* <span data-ttu-id="59231-148">Hello [jegyzetek](app-insights-annotations.md) hello diagram megjelenítése, ha telepítve vannak-e új verziók hello webhely.</span><span class="sxs-lookup"><span data-stu-id="59231-148">hello [annotations](app-insights-annotations.md) on hello chart show when new versions of hello website were deployed.</span></span> <span data-ttu-id="59231-149">Nincs hello legutóbbi központi telepítések észrevehető hatást használata.</span><span class="sxs-lookup"><span data-stu-id="59231-149">None of hello recent deployments had a noticeable effect on usage.</span></span>

<span data-ttu-id="59231-150">Mi történik, ha azt szeretné, tooinvestigate hello forgalom tooyour webhely részletesen, például a felosztás küldi el a hely a lap nézet telemetriai egyéni tulajdonság szerint?</span><span class="sxs-lookup"><span data-stu-id="59231-150">What if you want tooinvestigate hello traffic tooyour site in more detail, like splitting by a custom property your site sends in its page view telemetry?</span></span>

1. <span data-ttu-id="59231-151">Nyissa meg hello **események** eszköz hello Application Insights-erőforrás menüben.</span><span class="sxs-lookup"><span data-stu-id="59231-151">Open hello **Events** tool in hello Application Insights resource menu.</span></span> <span data-ttu-id="59231-152">Ez az eszköz lehetővé teszi, hogy hány Lapmegtekintések és egyéni események az alkalmazásból, a különböző szűrési, cohorting és Szegmentálás beállítások alapján küldött elemezheti.</span><span class="sxs-lookup"><span data-stu-id="59231-152">This tool lets you analyze how many page views and custom events were sent from your app, based on a variety of filtering, cohorting, and segmentation options.</span></span>
2. <span data-ttu-id="59231-153">A legördülő lista "Aki használja a" hello válassza ki az "Any lap nézet".</span><span class="sxs-lookup"><span data-stu-id="59231-153">In hello "Who used" dropdown, select "Any Page View".</span></span>
3. <span data-ttu-id="59231-154">A hello "Által megosztott" legördülő menüben válasszon ki egy tulajdonságot, mely toosplit által a lap telemetriai adatainak megtekintése.</span><span class="sxs-lookup"><span data-stu-id="59231-154">In hello "Split by" dropdown, select a property by which toosplit your page view telemetry.</span></span>

## <a name="retention---how-many-users-come-back"></a><span data-ttu-id="59231-155">Megőrzési - hány felhasználó térjen vissza?</span><span class="sxs-lookup"><span data-stu-id="59231-155">Retention - how many users come back?</span></span>

<span data-ttu-id="59231-156">Megőrzési segít megérteni, hogy milyen gyakran a felhasználók vissza toouse az alkalmazás felhasználók által végrehajtott bizonyos üzleti művelet során egy bizonyos idő gyűjtőben cohorts alapján.</span><span class="sxs-lookup"><span data-stu-id="59231-156">Retention helps you understand how often your users return toouse their app, based on cohorts of users that performed some business action during a certain time bucket.</span></span> 

- <span data-ttu-id="59231-157">Milyen funkciók miatt a felhasználók több, mint a többire toocome vissza ismertetése</span><span class="sxs-lookup"><span data-stu-id="59231-157">Understand what specific features cause users toocome back more than others</span></span> 
- <span data-ttu-id="59231-158">Űrlap feltételezéseket valós felhasználói adatok alapján</span><span class="sxs-lookup"><span data-stu-id="59231-158">Form hypotheses based on real user data</span></span> 
- <span data-ttu-id="59231-159">Annak megállapítása, hogy megőrzési a termékkel kapcsolatos probléma</span><span class="sxs-lookup"><span data-stu-id="59231-159">Determine whether retention is a problem in your product</span></span> 

![Megőrzés](./media/app-insights-usage-overview/retention.png) 

<span data-ttu-id="59231-161">hello megőrzési vezérlők felül lehetővé teszik, hogy toodefine adott események és idő tartomány toocalculate megőrzési.</span><span class="sxs-lookup"><span data-stu-id="59231-161">hello retention controls on top allow you toodefine specific events and time range toocalculate retention.</span></span> <span data-ttu-id="59231-162">hello hello középső graph ad hello vizuális ábrázolását általános megőrzési százalékos aránya hello időtartomány lett megadva.</span><span class="sxs-lookup"><span data-stu-id="59231-162">hello graph in hello middle gives a visual representation of hello overall retention percentage by hello time range specified.</span></span> <span data-ttu-id="59231-163">hello alsó hello grafikonon egy adott időszakra vonatkozó egyéni megőrzési jelöli.</span><span class="sxs-lookup"><span data-stu-id="59231-163">hello graph on hello bottom represents individual retention in a given time period.</span></span> <span data-ttu-id="59231-164">Részletesség ilyen szintje lehetővé teszi a felhasználók milyen végez toounderstand és milyen hatással lehetnek a részletesebb lépésköz adatszolgáltató felhasználója.</span><span class="sxs-lookup"><span data-stu-id="59231-164">This level of detail allows you toounderstand what your users are doing and what might affect returning users on a more detailed granularity.</span></span>  

[<span data-ttu-id="59231-165">További információk hello megőrzési eszköz</span><span class="sxs-lookup"><span data-stu-id="59231-165">More about hello Retention tool</span></span>](app-insights-usage-retention.md)

## <a name="custom-business-events"></a><span data-ttu-id="59231-166">Egyéni üzleti események</span><span class="sxs-lookup"><span data-stu-id="59231-166">Custom business events</span></span>

<span data-ttu-id="59231-167">tooget pontosan ismeri a felhasználók milyen elvégezni a segítségével a webalkalmazás, hasznos tooinsert sornyi kód toolog egyéni események.</span><span class="sxs-lookup"><span data-stu-id="59231-167">tooget a clear understanding of what users do with your web app, it's useful tooinsert lines of code toolog custom events.</span></span> <span data-ttu-id="59231-168">Ezeket az eseményeket is nyomon semmit részletes felhasználói műveletek, például az adott gombokkal, például vásárol, vagy egy játékot győzelmével toomore jelentős üzleti események.</span><span class="sxs-lookup"><span data-stu-id="59231-168">These events can track anything from detailed user actions such as clicking specific buttons, toomore significant business events such as making a purchase or winning a game.</span></span> 

<span data-ttu-id="59231-169">Bár bizonyos esetekben a lapmegtekintések hasznos események jelenthet, nem igaz általában.</span><span class="sxs-lookup"><span data-stu-id="59231-169">Although in some cases, page views can represent useful events, it isn't true in general.</span></span> <span data-ttu-id="59231-170">A felhasználó megnyithatja a termék oldalát hello termék vásárlás nélkül.</span><span class="sxs-lookup"><span data-stu-id="59231-170">A user can open a product page without buying hello product.</span></span> 

<span data-ttu-id="59231-171">Az adott üzleti eseményeket a webhelyen keresztül is diagram a felhasználók folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="59231-171">With specific business events, you can chart your users' progress through your site.</span></span> <span data-ttu-id="59231-172">Megtudhatja, a beállítások a különböző lehetőségek közül, és beállíthatja, amennyiben azok dobja el vagy megszüntetésekor nehézségekbe.</span><span class="sxs-lookup"><span data-stu-id="59231-172">You can find out their preferences for different options, and where they drop out or have difficulties.</span></span> <span data-ttu-id="59231-173">Ennek az információnak a tehet a fejlesztési várakozó hello prioritások megalapozott döntéseket.</span><span class="sxs-lookup"><span data-stu-id="59231-173">With this knowledge, you can make informed decisions about hello priorities in your development backlog.</span></span>

<span data-ttu-id="59231-174">Események hello weblap bejelentkezhet:</span><span class="sxs-lookup"><span data-stu-id="59231-174">Events can be logged in hello web page:</span></span>

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

<span data-ttu-id="59231-175">Vagy a kiszolgálóoldali hello hello webalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="59231-175">Or in hello server side of hello web app:</span></span>

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

<span data-ttu-id="59231-176">Tulajdonság értékek toothese események csatolhat, hogy szűrje, vagy feloszthatja az eseményeket hello, amikor hello portálon vizsgálata.</span><span class="sxs-lookup"><span data-stu-id="59231-176">You can attach property values toothese events, so that you can filter or split hello events when you inspect them in hello portal.</span></span> <span data-ttu-id="59231-177">Ezenkívül a Tulajdonságok szabványos készletét csatolt tooeach esemény, például a névtelen felhasználói Azonosítóját, amely lehetővé teszi egy adott felhasználó tevékenységét tootrace hello sorrendjét is.</span><span class="sxs-lookup"><span data-stu-id="59231-177">In addition, a standard set of properties is attached tooeach event, such as anonymous user ID, which allows you tootrace hello sequence of activities of an individual user.</span></span>

<span data-ttu-id="59231-178">További információ [egyéni események](app-insights-api-custom-events-metrics.md#trackevent) és [tulajdonságok](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="59231-178">Learn more about [custom events](app-insights-api-custom-events-metrics.md#trackevent) and [properties](app-insights-api-custom-events-metrics.md#properties).</span></span>

### <a name="slice-and-dice-events"></a><span data-ttu-id="59231-179">Szeletelésére és feldarabolására használnak események</span><span class="sxs-lookup"><span data-stu-id="59231-179">Slice and dice events</span></span>

<span data-ttu-id="59231-180">A hello felhasználók munkamenetek és események eszközök akkor is részletekbe menően felhasználó, az esemény neve és a Tulajdonságok egyéni események.</span><span class="sxs-lookup"><span data-stu-id="59231-180">In hello Users, Sessions, and Events tools, you can slice and dice custom events by user, event name, and properties.</span></span>
<span data-ttu-id="59231-181">![Felhasználók](./media/app-insights-usage-overview/users.png)</span><span class="sxs-lookup"><span data-stu-id="59231-181">![Users](./media/app-insights-usage-overview/users.png)</span></span>  
  
## <a name="design-hello-telemetry-with-hello-app"></a><span data-ttu-id="59231-182">Tervezési hello telemetriai hello alkalmazással</span><span class="sxs-lookup"><span data-stu-id="59231-182">Design hello telemetry with hello app</span></span>

<span data-ttu-id="59231-183">Az alkalmazás egyes szolgáltatások tervez, vegye figyelembe hogyan fog toomeasure annak sikerességét a felhasználóival.</span><span class="sxs-lookup"><span data-stu-id="59231-183">When you are designing each feature of your app, consider how you are going toomeasure its success with your users.</span></span> <span data-ttu-id="59231-184">Döntse el, üzleti események toorecord kell, és nyomon követés hívások eseményekre az alkalmazásba hello hello code elindításához.</span><span class="sxs-lookup"><span data-stu-id="59231-184">Decide what business events you need toorecord, and code hello tracking calls for those events into your app from hello start.</span></span>

## <a name="a--b-testing"></a><span data-ttu-id="59231-185">A |} B tesztelés</span><span class="sxs-lookup"><span data-stu-id="59231-185">A | B Testing</span></span>
<span data-ttu-id="59231-186">Ha nem tudja, melyik variant egy szolgáltatás több sikeres lesz, a kiadási helyezni őket, így minden elérhető toodifferent felhasználók.</span><span class="sxs-lookup"><span data-stu-id="59231-186">If you don't know which variant of a feature will be more successful, release both of them, making each accessible toodifferent users.</span></span> <span data-ttu-id="59231-187">Az egyes hello sikeresség felméréséhez, és helyezze a tooa egyesített verziója.</span><span class="sxs-lookup"><span data-stu-id="59231-187">Measure hello success of each, and then move tooa unified version.</span></span>

<span data-ttu-id="59231-188">Ez a módszer különböző tulajdonság értékek tooall hello telemetriai az alkalmazás minden verziója által küldött csatlakoztatnia.</span><span class="sxs-lookup"><span data-stu-id="59231-188">For this technique, you attach distinct property values tooall hello telemetry that is sent by each version of your app.</span></span> <span data-ttu-id="59231-189">Megtehetné tulajdonságainak meghatározása a hello aktív TelemetryContext.</span><span class="sxs-lookup"><span data-stu-id="59231-189">You can do that by defining properties in hello active TelemetryContext.</span></span> <span data-ttu-id="59231-190">Ezek alapértelmezett tulajdonságokkal is bővül tooevery telemetriai, amely az alkalmazás hello küld e-nem csak az egyéni üzenetek, de hello szabványos telemetriai adatokat is.</span><span class="sxs-lookup"><span data-stu-id="59231-190">These default properties are added tooevery telemetry message that hello application sends - not just your custom messages, but hello standard telemetry as well.</span></span>

<span data-ttu-id="59231-191">Hello Application Insights portál szűrése, és így toocompare hello különböző verziói osztani hello tulajdonságértékek esetén, az adatokat.</span><span class="sxs-lookup"><span data-stu-id="59231-191">In hello Application Insights portal, filter and split your data on hello property values, so as toocompare hello different versions.</span></span>

<span data-ttu-id="59231-192">toodo, [állítson be egy telemetriai inicializáló](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span><span class="sxs-lookup"><span data-stu-id="59231-192">toodo this, [set up a telemetry initializer](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span></span>

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

<span data-ttu-id="59231-193">A hello web app inicializáló Global.asax.cs például:</span><span class="sxs-lookup"><span data-stu-id="59231-193">In hello web app initializer such as Global.asax.cs:</span></span>

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

<span data-ttu-id="59231-194">Minden új TelemetryClients hello tulajdonság értékét adja meg, hogy automatikusan hozzáadják.</span><span class="sxs-lookup"><span data-stu-id="59231-194">All new TelemetryClients automatically add hello property value you specify.</span></span> <span data-ttu-id="59231-195">Egyéni telemetriai események felülbírálhatják hello alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="59231-195">Individual telemetry events can override hello default values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59231-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59231-196">Next steps</span></span>
   - [<span data-ttu-id="59231-197">Felhasználók, munkamenetek, események</span><span class="sxs-lookup"><span data-stu-id="59231-197">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
   - [<span data-ttu-id="59231-198">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="59231-198">Funnels</span></span>](usage-funnels.md)
   - [<span data-ttu-id="59231-199">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="59231-199">Retention</span></span>](app-insights-usage-retention.md)
   - [<span data-ttu-id="59231-200">Felhasználói folyamatok</span><span class="sxs-lookup"><span data-stu-id="59231-200">User Flows</span></span>](app-insights-usage-flows.md)
   - [<span data-ttu-id="59231-201">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="59231-201">Workbooks</span></span>](app-insights-usage-workbooks.md)
   - [<span data-ttu-id="59231-202">Felhasználói környezet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="59231-202">Add user context</span></span>](app-insights-usage-send-user-context.md)

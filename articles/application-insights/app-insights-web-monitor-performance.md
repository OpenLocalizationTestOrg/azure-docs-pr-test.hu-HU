---
title: "Az alkalmazás állapotának és az Application insights szolgáltatással használatának figyelése"
description: "Ismerkedés az Application insights szolgáltatással. Használati, rendelkezésre állásának és a helyszíni vagy a Microsoft Azure-alkalmazások teljesítményének elemzése."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/25/2015
ms.author: bwren
ms.openlocfilehash: 5b7b1f4a53cd2624ee8e2ab684ba6ba63252674f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-performance-in-web-applications"></a><span data-ttu-id="80bd7-104">Webalkalmazások teljesítményének monitorozása</span><span class="sxs-lookup"><span data-stu-id="80bd7-104">Monitor performance in web applications</span></span>


<span data-ttu-id="80bd7-105">Győződjön meg arról, hogy az alkalmazás teljesítménye, és tájékozódhat a gyorsan esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="80bd7-105">Make sure your application is performing well, and find out quickly about any failures.</span></span> <span data-ttu-id="80bd7-106">[Az Application Insights] [ start] adja meg a teljesítménybeli problémák és kivételek, és és az alapvető okok diagnosztizálásában.</span><span class="sxs-lookup"><span data-stu-id="80bd7-106">[Application Insights][start] will tell you about any performance issues and exceptions, and help you find and diagnose the root causes.</span></span>

<span data-ttu-id="80bd7-107">Az Application Insights figyelheti a Java és az ASP.NET webalkalmazások és szolgáltatások, WCF-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="80bd7-107">Application Insights can monitor both Java and ASP.NET web applications and services, WCF services.</span></span> <span data-ttu-id="80bd7-108">Ezek lehetnek üzemeltethető a helyszínen, a virtuális gépek, illetve a Microsoft Azure-webhelyekre.</span><span class="sxs-lookup"><span data-stu-id="80bd7-108">They can be hosted on-premises, on virtual machines, or as Microsoft Azure websites.</span></span> 

<span data-ttu-id="80bd7-109">Ügyféloldali Application Insights telemetria a weblapok és az eszközt, beleértve az iOS, Android és Windows Áruházbeli alkalmazások számos végre.</span><span class="sxs-lookup"><span data-stu-id="80bd7-109">On the client side, Application Insights can take telemetry from web pages and a wide variety of devices including iOS, Android and Windows Store apps.</span></span>

>[!Note]
> <span data-ttu-id="80bd7-110">Hajtottunk egy új felhasználói élmény keresése lassú lapok végrehajtása a webalkalmazásban érhető el.</span><span class="sxs-lookup"><span data-stu-id="80bd7-110">We have made a new experience available for finding slow performing pages in your web application.</span></span> <span data-ttu-id="80bd7-111">Ha Ön nem jogosult az elérésére, engedélyezze a kép beállításainak konfigurálásával a [előnézeti panelen](app-insights-previews.md).</span><span class="sxs-lookup"><span data-stu-id="80bd7-111">If you don't have access to it, enable it by configuring your preview options with the [Preview blade](app-insights-previews.md).</span></span> <span data-ttu-id="80bd7-112">Olvassa el az új felhasználói élmény a [keresse meg és hárítsa el a interaktív teljesítményt vizsgálat szűk keresztmetszetek](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span><span class="sxs-lookup"><span data-stu-id="80bd7-112">Read about this new experience in [Find and fix performance bottlenecks with the interactive Performance investigation](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span></span>

## <span data-ttu-id="80bd7-113"><a name="setup"></a>Teljesítmény figyelése</span><span class="sxs-lookup"><span data-stu-id="80bd7-113"><a name="setup"></a>Set up performance monitoring</span></span>
<span data-ttu-id="80bd7-114">Ha még nem még hozzáadta az Application Insights a projekthez (Ha nincs beállítva a ApplicationInsights.config), válasszon egyet az alábbi módszerrel lehet hozzálátni:</span><span class="sxs-lookup"><span data-stu-id="80bd7-114">If you haven't yet added Application Insights to your project (that is, if it doesn't have ApplicationInsights.config), choose one of these ways to get started:</span></span>

* [<span data-ttu-id="80bd7-115">ASP.NET-webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="80bd7-115">ASP.NET web apps</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="80bd7-116">Kivételfigyelés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="80bd7-116">Add exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
  * [<span data-ttu-id="80bd7-117">A függőségi figyelés felvétele</span><span class="sxs-lookup"><span data-stu-id="80bd7-117">Add dependency monitoring</span></span>](app-insights-monitor-performance-live-website-now.md)
* [<span data-ttu-id="80bd7-118">J2EE webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="80bd7-118">J2EE web apps</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="80bd7-119">A függőségi figyelés felvétele</span><span class="sxs-lookup"><span data-stu-id="80bd7-119">Add dependency monitoring</span></span>](app-insights-java-agent.md)

## <span data-ttu-id="80bd7-120"><a name="view"></a>Teljesítménymértékeket felfedezése</span><span class="sxs-lookup"><span data-stu-id="80bd7-120"><a name="view"></a>Exploring performance metrics</span></span>
<span data-ttu-id="80bd7-121">A [az Azure-portálon](https://portal.azure.com), keresse meg az Application Insights-erőforrást az alkalmazáshoz beállított.</span><span class="sxs-lookup"><span data-stu-id="80bd7-121">In [the Azure portal](https://portal.azure.com), browse to the Application Insights resource that you set up for your application.</span></span> <span data-ttu-id="80bd7-122">Az Áttekintés panel alapvető teljesítményadatokat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="80bd7-122">The overview blade shows basic performance data:</span></span>

<span data-ttu-id="80bd7-123">Kattintson a részletek megtekintéséhez, és hosszabb ideig eredmények megtekintése érdekében bármelyik olyan diagram.</span><span class="sxs-lookup"><span data-stu-id="80bd7-123">Click any chart to see more detail, and to see results for a longer period.</span></span> <span data-ttu-id="80bd7-124">Kattintson például a kérések csempe, és válassza ki egy időtartományt:</span><span class="sxs-lookup"><span data-stu-id="80bd7-124">For example, click the Requests tile and then select a time range:</span></span>

![Kattintson a további adatok, és válasszon egy időtartományt](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

<span data-ttu-id="80bd7-126">Kattintson a diagram mely metrikák azt jeleníti meg, vagy új diagram hozzáadása, és válassza ki a metrikák kiválasztásához:</span><span class="sxs-lookup"><span data-stu-id="80bd7-126">Click a chart to choose which metrics it displays, or add a new chart and select its metrics:</span></span>

![Kattintson egy grafikonon metrikák kiválasztásához](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> <span data-ttu-id="80bd7-128">**Törölje a jelet a metrikák** a teljes kijelölt elérhető.</span><span class="sxs-lookup"><span data-stu-id="80bd7-128">**Uncheck all the metrics** to see the full selection that is available.</span></span> <span data-ttu-id="80bd7-129">A metrikák sorolhatók csoportok; egy csoport minden tagja van kijelölve, csak a többi a csoportnak a tagjaira jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="80bd7-129">The metrics fall into groups; when any member of a group is selected, only the other members of that group appear.</span></span>

## <span data-ttu-id="80bd7-130"><a name="metrics"></a>Mire azt minden középérték?</span><span class="sxs-lookup"><span data-stu-id="80bd7-130"><a name="metrics"></a>What does it all mean?</span></span> <span data-ttu-id="80bd7-131">Teljesítmény csempék és jelentések</span><span class="sxs-lookup"><span data-stu-id="80bd7-131">Performance tiles and reports</span></span>
<span data-ttu-id="80bd7-132">Nincsenek is elérhetővé teljesítménymutatók.</span><span class="sxs-lookup"><span data-stu-id="80bd7-132">There are various performance metrics you can get.</span></span> <span data-ttu-id="80bd7-133">Kezdjük azokkal, amely az alkalmazás panel alapértelmezés szerint megjelenik.</span><span class="sxs-lookup"><span data-stu-id="80bd7-133">Let's start with those that appear by default on the application blade.</span></span>

### <a name="requests"></a><span data-ttu-id="80bd7-134">Kérelmek</span><span class="sxs-lookup"><span data-stu-id="80bd7-134">Requests</span></span>
<span data-ttu-id="80bd7-135">Egy megadott időszakban fogadott HTTP-kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="80bd7-135">The number of HTTP requests received in a specified period.</span></span> <span data-ttu-id="80bd7-136">Hasonlítsa ezt össze az eredményeket egyéb jelentések megtekintéséhez a terhelés, hogyan viselkedik az alkalmazás függ.</span><span class="sxs-lookup"><span data-stu-id="80bd7-136">Compare this with the results on other reports to see how your app behaves as the load varies.</span></span>

<span data-ttu-id="80bd7-137">HTTP-kérések összes GET vagy POST kérelem lapok, adatok és lemezképek tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="80bd7-137">HTTP requests include all GET or POST requests for pages, data, and images.</span></span>

<span data-ttu-id="80bd7-138">Kattintson a csempére lekérésére adott URL-címek száma.</span><span class="sxs-lookup"><span data-stu-id="80bd7-138">Click on the tile to get counts for specific URLs.</span></span>

### <a name="average-response-time"></a><span data-ttu-id="80bd7-139">Átlagos válaszidő</span><span class="sxs-lookup"><span data-stu-id="80bd7-139">Average response time</span></span>
<span data-ttu-id="80bd7-140">Írja be az alkalmazás és a válaszban visszaadott webes kérelem közötti időt méri.</span><span class="sxs-lookup"><span data-stu-id="80bd7-140">Measures the time between a web request entering your application and the response being returned.</span></span>

<span data-ttu-id="80bd7-141">A pontok megjelenítése mozgó átlagos.</span><span class="sxs-lookup"><span data-stu-id="80bd7-141">The points show a moving average.</span></span> <span data-ttu-id="80bd7-142">Ha nagy mennyiségű kérést, előfordulhat, egyes, amelyek eltérnek az átlagos egy nyilvánvaló csúcs nélkül, vagy a grafikonon merítsük.</span><span class="sxs-lookup"><span data-stu-id="80bd7-142">If there are a lot of requests, there might be some that deviate from the average without an obvious peak or dip in the graph.</span></span>

<span data-ttu-id="80bd7-143">Szokatlan csúcsait keresi.</span><span class="sxs-lookup"><span data-stu-id="80bd7-143">Look for unusual peaks.</span></span> <span data-ttu-id="80bd7-144">Általában várt válaszideje nőtt a kérelmeket az értéke.</span><span class="sxs-lookup"><span data-stu-id="80bd7-144">In general, expect response time to rise with a rise in requests.</span></span> <span data-ttu-id="80bd7-145">Ha a megnövekedhet le aránytalanul nagy, előfordulhat, hogy az alkalmazás szerezze például a Processzor- vagy egy szolgáltatást, használja a kapacitás erőforrás korlátozni.</span><span class="sxs-lookup"><span data-stu-id="80bd7-145">If the rise is disproportionate, your app might be hitting a resource limit such as CPU or the capacity of a service it uses.</span></span>

<span data-ttu-id="80bd7-146">A megadott URL-címek alkalommal csempén kattintson.</span><span class="sxs-lookup"><span data-stu-id="80bd7-146">Click the tile to get times for specific URLs.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a><span data-ttu-id="80bd7-147">A leghosszabb kérelmek</span><span class="sxs-lookup"><span data-stu-id="80bd7-147">Slowest requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

<span data-ttu-id="80bd7-148">Jeleníti meg, mely kérelmek teljesítményhangolás kell.</span><span class="sxs-lookup"><span data-stu-id="80bd7-148">Shows which requests might need performance tuning.</span></span>

### <a name="failed-requests"></a><span data-ttu-id="80bd7-149">Sikertelen kérelmek</span><span class="sxs-lookup"><span data-stu-id="80bd7-149">Failed requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

<span data-ttu-id="80bd7-150">Kérelmek kivételt váltott ki a nem kezelt kivételek számát.</span><span class="sxs-lookup"><span data-stu-id="80bd7-150">A count of requests that threw uncaught exceptions.</span></span>

<span data-ttu-id="80bd7-151">Kattintson a csempére kattintva megtekintheti az adott hibák részleteit, és válassza ki az egyes kérést a részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="80bd7-151">Click the tile to see the details of specific failures, and select an individual request to see its detail.</span></span> 

<span data-ttu-id="80bd7-152">Egyéni ellenőrzési hibák csak egy reprezentatív minta megmarad.</span><span class="sxs-lookup"><span data-stu-id="80bd7-152">Only a representative sample of failures is retained for individual inspection.</span></span>

### <a name="other-metrics"></a><span data-ttu-id="80bd7-153">Más metrikákkal</span><span class="sxs-lookup"><span data-stu-id="80bd7-153">Other metrics</span></span>
<span data-ttu-id="80bd7-154">Hogy más metrikákkal jeleníti meg, kattintson egy grafikonon, és törölje a jelölést a rendelkezésre álló teljes megjelenítéséhez a metrikák beállítása.</span><span class="sxs-lookup"><span data-stu-id="80bd7-154">To see what other metrics you can display, click a graph, and then deselect all the metrics to see the full available set.</span></span> <span data-ttu-id="80bd7-155">(I) kattintva megtekintheti az egyes metrika definíciója.</span><span class="sxs-lookup"><span data-stu-id="80bd7-155">Click (i) to see each metric's definition.</span></span>

![Törölje a teljes című szakaszban láthat minden metrikák](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

<span data-ttu-id="80bd7-157">Bármely metrika kijelölése letiltja a többi, amelyek nem jelennek meg ugyanabban a diagramban.</span><span class="sxs-lookup"><span data-stu-id="80bd7-157">Selecting any metric disables the others that can't appear on the same chart.</span></span>

## <a name="set-alerts"></a><span data-ttu-id="80bd7-158">Riasztások beállítása</span><span class="sxs-lookup"><span data-stu-id="80bd7-158">Set alerts</span></span>
<span data-ttu-id="80bd7-159">Rendkívüli értékek a metrika az e-mailben értesítést kapjon, adjon hozzá egy riasztást.</span><span class="sxs-lookup"><span data-stu-id="80bd7-159">To be notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="80bd7-160">Ön választhatja az e-mailt küldhet a rendszergazdák, vagy az adott e-mail címekre.</span><span class="sxs-lookup"><span data-stu-id="80bd7-160">You can choose either to send the email to the account administrators, or to specific email addresses.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

<span data-ttu-id="80bd7-161">Állítsa be az erőforrást, mielőtt a többi tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="80bd7-161">Set the resource before the other properties.</span></span> <span data-ttu-id="80bd7-162">Ne válassza ki a webtesztben erőforrásokat, ha a teljesítmény vagy a használati metrikák állíthatók be riasztások.</span><span class="sxs-lookup"><span data-stu-id="80bd7-162">Don't choose the webtest resources if you want to set alerts on performance or usage metrics.</span></span>

<span data-ttu-id="80bd7-163">Ügyeljen arra, ügyeljen az egységeket, amelyekben azonosítóját, írja be a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="80bd7-163">Be careful to note the units in which you're asked to enter the threshold value.</span></span>

<span data-ttu-id="80bd7-164">*A riasztási hozzáadása gomb nem látható.*</span><span class="sxs-lookup"><span data-stu-id="80bd7-164">*I don't see the Add Alert button.*</span></span> <span data-ttu-id="80bd7-165">-Az Ez a csoport olyan fiókhoz, amelyhez csak olvasási hozzáféréssel?</span><span class="sxs-lookup"><span data-stu-id="80bd7-165">- Is this a group account to which you have read-only access?</span></span> <span data-ttu-id="80bd7-166">Kérje meg a fiók rendszergazdájához.</span><span class="sxs-lookup"><span data-stu-id="80bd7-166">Check with the account administrator.</span></span>

## <span data-ttu-id="80bd7-167"><a name="diagnosis"></a>Problémák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="80bd7-167"><a name="diagnosis"></a>Diagnosing issues</span></span>
<span data-ttu-id="80bd7-168">Az alábbiakban néhány tippek megkereséséhez és teljesítménnyel kapcsolatos problémák diagnosztizálásához használható:</span><span class="sxs-lookup"><span data-stu-id="80bd7-168">Here are a few tips for finding and diagnosing performance issues:</span></span>

* <span data-ttu-id="80bd7-169">Állítson be [webalkalmazás-tesztek] [ availability] értesítést, ha a webhely nem működik, vagy helytelenül vagy lassan válaszol.</span><span class="sxs-lookup"><span data-stu-id="80bd7-169">Set up [web tests][availability] to be alerted if your web site goes down or responds incorrectly or slowly.</span></span> 
* <span data-ttu-id="80bd7-170">Hasonlítsa össze a más metrikákkal, ha a kapcsolódó betöltése sikertelen vagy lassú válasz a kérelmek számát.</span><span class="sxs-lookup"><span data-stu-id="80bd7-170">Compare the Request count with other metrics to see if failures or slow response are related to load.</span></span>
* <span data-ttu-id="80bd7-171">[INSERT, és nyomkövetési utasítások keresési] [ diagnostic] érdekében a rögzítési ponthoz problémák a kódban.</span><span class="sxs-lookup"><span data-stu-id="80bd7-171">[Insert and search trace statements][diagnostic] in your code to help pinpoint problems.</span></span>
* <span data-ttu-id="80bd7-172">A webalkalmazás műveletet a figyelheti [metrikák adatfolyamot][livestream].</span><span class="sxs-lookup"><span data-stu-id="80bd7-172">Monitor your Web app in operation with [Live Metrics Stream][livestream].</span></span>
* <span data-ttu-id="80bd7-173">A .net alkalmazás állapotának rögzítésére [pillanatkép hibakereső][snapshot].</span><span class="sxs-lookup"><span data-stu-id="80bd7-173">Capture the state of your .Net application with [Snapshot Debugger][snapshot].</span></span>

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-performance-investigation"></a><span data-ttu-id="80bd7-174">Keresse meg és hárítsa el szűk keresztmetszetek interaktív teljesítményt vizsgálat</span><span class="sxs-lookup"><span data-stu-id="80bd7-174">Find and fix performance bottlenecks with an interactive performance investigation</span></span>

<span data-ttu-id="80bd7-175">Az új Application Insights interaktív teljesítményt vizsgálat segítségével keresse meg a webalkalmazás összteljesítmény lassítja területéhez.</span><span class="sxs-lookup"><span data-stu-id="80bd7-175">You can use the new Application Insights interactive performance investigation to locate areas of your Web app that are slowing down overall performance.</span></span> <span data-ttu-id="80bd7-176">Is gyorsan keresés adott lapok, amelyek lassítja, és használja a [profilkészítési eszköz](app-insights-profiler.md) van-e az adott lapok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="80bd7-176">You can quickly find specific pages that are slowing down, and use the [Profiling tool](app-insights-profiler.md) to see if there is a correlation between these pages.</span></span>

### <a name="create-a-list-of-slow-performing-pages"></a><span data-ttu-id="80bd7-177">Lassú teljesítő oldalak listájának létrehozása</span><span class="sxs-lookup"><span data-stu-id="80bd7-177">Create a list of slow performing pages</span></span> 

<span data-ttu-id="80bd7-178">Az első lépés a teljesítménnyel kapcsolatos problémák keresése lassú válaszol oldalak listájának.</span><span class="sxs-lookup"><span data-stu-id="80bd7-178">The first step for finding performance issues is to get a list of the slow responding pages.</span></span> <span data-ttu-id="80bd7-179">Az alábbi képernyőfelvételhez mutatja be, a teljesítmény panel használatával alaposabb kivizsgálása lehetséges oldalak listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="80bd7-179">The screen shot below demonstrates using the Performance blade to get a list of potential pages to investigate further.</span></span> <span data-ttu-id="80bd7-180">Gyorsan megtekintheti ezen a lapon, hogy történt egy slow-down az alkalmazás válaszidő körülbelül 6:00 PM és újra körülbelül 10 óra.</span><span class="sxs-lookup"><span data-stu-id="80bd7-180">You can quickly see from this page that there was a slow-down in the response time of the app at approximately 6:00 PM and again at approximately 10 PM.</span></span> <span data-ttu-id="80bd7-181">Láthatja, hogy az ügyfél-részletek művelet volt-e az egyes hosszú ideig futó műveletek egy közepes válaszidő 507.05 ideje (MS).</span><span class="sxs-lookup"><span data-stu-id="80bd7-181">You can also see that the GET customer/details operation had some long running operations with a median response time of 507.05 milliseconds.</span></span> 

![Alkalmazásteljesítmény elemzések interaktív](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a><span data-ttu-id="80bd7-183">Bizonyos lapokon részletekbe menően tárhatják</span><span class="sxs-lookup"><span data-stu-id="80bd7-183">Drill down on specific pages</span></span>

<span data-ttu-id="80bd7-184">Miután az alkalmazás teljesítményének pillanatképet, kaphat további részleteket a lassú végez műveleteket.</span><span class="sxs-lookup"><span data-stu-id="80bd7-184">Once you have a snapshot of your app's performance, you can get more details on specific slow-performing operations.</span></span> <span data-ttu-id="80bd7-185">Kattintson az összes műveletet a listában, a részletek megtekintéséhez a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="80bd7-185">Click on any operation in the list to see the details as shown below.</span></span> <span data-ttu-id="80bd7-186">A diagram látható, ha a teljesítmény a függőség alapszik.</span><span class="sxs-lookup"><span data-stu-id="80bd7-186">From the chart you can see if the performance was based on a dependency.</span></span> <span data-ttu-id="80bd7-187">Hány felhasználó észlelt a különböző válaszidejét is megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="80bd7-187">You can also see how many users experienced the various response times.</span></span> 

![Application Insights műveletek panel](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a><span data-ttu-id="80bd7-189">Egy adott időszakra vonatkozóan a részletekbe menően tárhatják</span><span class="sxs-lookup"><span data-stu-id="80bd7-189">Drill down on a specific time period</span></span>

<span data-ttu-id="80bd7-190">Ha azonosított egy pont a ideje kivizsgálni, részletekbe menően tárhatják nézze meg a műveleteket, amelyek oka lehet, hogy a teljesítmény slow-down még tovább.</span><span class="sxs-lookup"><span data-stu-id="80bd7-190">After you have identified a point in time to investigate, drill down even further to look at the specific operations that might have caused the performance slow-down.</span></span> <span data-ttu-id="80bd7-191">Amikor az adott időben kattint kap a lap részletei alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="80bd7-191">When you click on a specific point in time you get the details of the page as shown below.</span></span> <span data-ttu-id="80bd7-192">Az alábbi példában látható is egy adott időszakban, valamint a kiszolgáló válaszkódot és a művelet időtartama felsorolt műveleteket.</span><span class="sxs-lookup"><span data-stu-id="80bd7-192">In the example below you can see the operations listed for a given time period along with the server response codes and the operation duration.</span></span> <span data-ttu-id="80bd7-193">Lehetősége van a TFS munkaelem megnyitása, ha szeretné ezt az információt küld a fejlesztői csapat URL-címét is.</span><span class="sxs-lookup"><span data-stu-id="80bd7-193">You also have the url for opening a TFS work item if you need to send this information to your development team.</span></span>

![Application Insights időszelet](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a><span data-ttu-id="80bd7-195">Egy bizonyos művelet részletekbe menően tárhatják</span><span class="sxs-lookup"><span data-stu-id="80bd7-195">Drill down on a specific operation</span></span>

<span data-ttu-id="80bd7-196">Ha azonosított egy pont a ideje kivizsgálni, részletekbe menően tárhatják nézze meg a műveleteket, amelyek oka lehet, hogy a teljesítmény slow-down még tovább.</span><span class="sxs-lookup"><span data-stu-id="80bd7-196">After you have identified a point in time to investigate, drill down even further to look at the specific operations that might have caused the performance slow-down.</span></span> <span data-ttu-id="80bd7-197">Kattintson a művelet a listában, a művelet részleteinek megtekintéséhez a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="80bd7-197">Click on an operation from the list to see the details of the operation as shown below.</span></span> <span data-ttu-id="80bd7-198">Ebben a példában láthatja, hogy a művelet végrehajtása sikertelen volt, és az Application Insights megadta a kivételt váltott ki. az alkalmazás részleteit.</span><span class="sxs-lookup"><span data-stu-id="80bd7-198">In this example you can see that the operation failed, and Application Insights has provided the details of the exception the application threw.</span></span> <span data-ttu-id="80bd7-199">Ezen a panelen újra, a TFS munkaelem könnyedén létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="80bd7-199">Again, you can easily create a TFS work item from this blade.</span></span>

![Application Insights művelet panel](./media/app-insights-web-monitor-performance/performance3.png)


## <span data-ttu-id="80bd7-201"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80bd7-201"><a name="next"></a>Next steps</span></span>
<span data-ttu-id="80bd7-202">[Webalkalmazás-tesztek] [ availability] -rendelkezik a webes kérelmek világszerte a rendszeres időközönként az alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="80bd7-202">[Web tests][availability] - Have web requests sent to your application at regular intervals from around the world.</span></span>

<span data-ttu-id="80bd7-203">[Rögzítése, és diagnosztikai nyomkövetési keresési] [ diagnostic] - nyomkövetési hívások beszúrása és az eredményeket a rögzítési ponthoz problémák keletkezik.</span><span class="sxs-lookup"><span data-stu-id="80bd7-203">[Capture and search diagnostic traces][diagnostic] - Insert trace calls and sift through the results to pinpoint issues.</span></span>

<span data-ttu-id="80bd7-204">[Használat nyomon követése] [ usage] -megtudhatja, hogyan személyek használhassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="80bd7-204">[Usage tracking][usage] - Find out how people use your application.</span></span>

<span data-ttu-id="80bd7-205">[Hibaelhárítási] [ qna] - és a kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="80bd7-205">[Troubleshooting][qna] - and Q & A</span></span>



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md




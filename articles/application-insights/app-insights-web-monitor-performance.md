---
title: "aaaMonitor az alkalmazás állapotának és használatának az Application insights szolgáltatással"
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
ms.openlocfilehash: 9230a6e65e5afb00c36122ff1d1de01ba19cd7f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-performance-in-web-applications"></a><span data-ttu-id="2b9f8-104">Webalkalmazások teljesítményének monitorozása</span><span class="sxs-lookup"><span data-stu-id="2b9f8-104">Monitor performance in web applications</span></span>


<span data-ttu-id="2b9f8-105">Győződjön meg arról, hogy az alkalmazás teljesítménye, és tájékozódhat a gyorsan esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-105">Make sure your application is performing well, and find out quickly about any failures.</span></span> <span data-ttu-id="2b9f8-106">[Az Application Insights] [ start] adja meg a teljesítménybeli problémák és kivételek, és keresse meg és diagnosztizálásához hello súgó kiváltó okok.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-106">[Application Insights][start] will tell you about any performance issues and exceptions, and help you find and diagnose hello root causes.</span></span>

<span data-ttu-id="2b9f8-107">Az Application Insights figyelheti a Java és az ASP.NET webalkalmazások és szolgáltatások, WCF-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-107">Application Insights can monitor both Java and ASP.NET web applications and services, WCF services.</span></span> <span data-ttu-id="2b9f8-108">Ezek lehetnek üzemeltethető a helyszínen, a virtuális gépek, illetve a Microsoft Azure-webhelyekre.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-108">They can be hosted on-premises, on virtual machines, or as Microsoft Azure websites.</span></span> 

<span data-ttu-id="2b9f8-109">Hello ügyféloldalon az Application Insights telemetria a weblapok és az eszközt, beleértve az iOS, Android és Windows Áruházbeli alkalmazások számos vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-109">On hello client side, Application Insights can take telemetry from web pages and a wide variety of devices including iOS, Android and Windows Store apps.</span></span>

>[!Note]
> <span data-ttu-id="2b9f8-110">Hajtottunk egy új felhasználói élmény keresése lassú lapok végrehajtása a webalkalmazásban érhető el.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-110">We have made a new experience available for finding slow performing pages in your web application.</span></span> <span data-ttu-id="2b9f8-111">Ha nem rendelkezik hozzáféréssel tooit, engedélyezheti a hello a kép beállításainak konfigurálásával [előnézeti panelen](app-insights-previews.md).</span><span class="sxs-lookup"><span data-stu-id="2b9f8-111">If you don't have access tooit, enable it by configuring your preview options with hello [Preview blade](app-insights-previews.md).</span></span> <span data-ttu-id="2b9f8-112">Olvassa el az új felhasználói élmény a [keresse meg és hárítsa el szűk keresztmetszetek hello interaktív teljesítményt vizsgálat](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span><span class="sxs-lookup"><span data-stu-id="2b9f8-112">Read about this new experience in [Find and fix performance bottlenecks with hello interactive Performance investigation](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span></span>

## <span data-ttu-id="2b9f8-113"><a name="setup"></a>Teljesítmény figyelése</span><span class="sxs-lookup"><span data-stu-id="2b9f8-113"><a name="setup"></a>Set up performance monitoring</span></span>
<span data-ttu-id="2b9f8-114">Ha még nem vett Application Insights tooyour (Ez azt jelenti, ha nincs beállítva a ApplicationInsights.config) projektre, válassza ki az alábbi módszerek valamelyikével tooget lépések:</span><span class="sxs-lookup"><span data-stu-id="2b9f8-114">If you haven't yet added Application Insights tooyour project (that is, if it doesn't have ApplicationInsights.config), choose one of these ways tooget started:</span></span>

* [<span data-ttu-id="2b9f8-115">ASP.NET-webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="2b9f8-115">ASP.NET web apps</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="2b9f8-116">Kivételfigyelés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2b9f8-116">Add exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
  * [<span data-ttu-id="2b9f8-117">A függőségi figyelés felvétele</span><span class="sxs-lookup"><span data-stu-id="2b9f8-117">Add dependency monitoring</span></span>](app-insights-monitor-performance-live-website-now.md)
* [<span data-ttu-id="2b9f8-118">J2EE webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="2b9f8-118">J2EE web apps</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="2b9f8-119">A függőségi figyelés felvétele</span><span class="sxs-lookup"><span data-stu-id="2b9f8-119">Add dependency monitoring</span></span>](app-insights-java-agent.md)

## <span data-ttu-id="2b9f8-120"><a name="view"></a>Teljesítménymértékeket felfedezése</span><span class="sxs-lookup"><span data-stu-id="2b9f8-120"><a name="view"></a>Exploring performance metrics</span></span>
<span data-ttu-id="2b9f8-121">A [hello Azure-portálon](https://portal.azure.com), keresse meg az alkalmazáshoz beállított toohello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-121">In [hello Azure portal](https://portal.azure.com), browse toohello Application Insights resource that you set up for your application.</span></span> <span data-ttu-id="2b9f8-122">hello áttekintése panel alapvető teljesítményadatait jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="2b9f8-122">hello overview blade shows basic performance data:</span></span>

<span data-ttu-id="2b9f8-123">Kattintson a bármely diagram toosee további információkhoz juthat, és toosee eredmények hosszabb ideig.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-123">Click any chart toosee more detail, and toosee results for a longer period.</span></span> <span data-ttu-id="2b9f8-124">Kattintson például a hello kérések csempe, és válassza a időtartomány:</span><span class="sxs-lookup"><span data-stu-id="2b9f8-124">For example, click hello Requests tile and then select a time range:</span></span>

![Kattintson a toomore adatok között, és válasszon egy időtartományt](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

<span data-ttu-id="2b9f8-126">Kattintson a diagram toochoose mely metrikák azt jeleníti meg, vagy új diagram hozzáadása, és válassza ki a metrikák:</span><span class="sxs-lookup"><span data-stu-id="2b9f8-126">Click a chart toochoose which metrics it displays, or add a new chart and select its metrics:</span></span>

![Kattintson egy grafikonon toochoose metrikák](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> <span data-ttu-id="2b9f8-128">**Törölje az összes hello metrikát** toosee hello teljes kijelölés elérhető.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-128">**Uncheck all hello metrics** toosee hello full selection that is available.</span></span> <span data-ttu-id="2b9f8-129">hello metrikák sorolhatók csoportok; Ha egy csoport minden tagja van kijelölve, csak hello csoport többi tagjára jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-129">hello metrics fall into groups; when any member of a group is selected, only hello other members of that group appear.</span></span>

## <span data-ttu-id="2b9f8-130"><a name="metrics"></a>Mire azt minden középérték?</span><span class="sxs-lookup"><span data-stu-id="2b9f8-130"><a name="metrics"></a>What does it all mean?</span></span> <span data-ttu-id="2b9f8-131">Teljesítmény csempék és jelentések</span><span class="sxs-lookup"><span data-stu-id="2b9f8-131">Performance tiles and reports</span></span>
<span data-ttu-id="2b9f8-132">Nincsenek is elérhetővé teljesítménymutatók.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-132">There are various performance metrics you can get.</span></span> <span data-ttu-id="2b9f8-133">Kezdjük az alábbiakhoz alapértelmezés szerint a hello alkalmazás panelen jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-133">Let's start with those that appear by default on hello application blade.</span></span>

### <a name="requests"></a><span data-ttu-id="2b9f8-134">Kérelmek</span><span class="sxs-lookup"><span data-stu-id="2b9f8-134">Requests</span></span>
<span data-ttu-id="2b9f8-135">egy megadott időszakban fogadott HTTP-kérelmek száma hello.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-135">hello number of HTTP requests received in a specified period.</span></span> <span data-ttu-id="2b9f8-136">Hasonlítsa össze a más jelentések toosee, hogyan viselkedik az alkalmazás, mivel változik a terhelés hello hello eredményt.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-136">Compare this with hello results on other reports toosee how your app behaves as hello load varies.</span></span>

<span data-ttu-id="2b9f8-137">HTTP-kérések összes GET vagy POST kérelem lapok, adatok és lemezképek tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-137">HTTP requests include all GET or POST requests for pages, data, and images.</span></span>

<span data-ttu-id="2b9f8-138">Kattintson a hello csempe tooget érintett adott URL-címek.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-138">Click on hello tile tooget counts for specific URLs.</span></span>

### <a name="average-response-time"></a><span data-ttu-id="2b9f8-139">Átlagos válaszidő</span><span class="sxs-lookup"><span data-stu-id="2b9f8-139">Average response time</span></span>
<span data-ttu-id="2b9f8-140">Intézkedések hello között eltelt idő egy webes kéréssel, írja be az alkalmazás- és hello választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-140">Measures hello time between a web request entering your application and hello response being returned.</span></span>

<span data-ttu-id="2b9f8-141">hello pontok megjelenítése mozgó átlagos.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-141">hello points show a moving average.</span></span> <span data-ttu-id="2b9f8-142">Ha nagy mennyiségű kérést, előfordulhat, egyes hello átlagos nélkül egy nyilvánvaló csúcs eltérnek vagy hello graph mártsuk.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-142">If there are a lot of requests, there might be some that deviate from hello average without an obvious peak or dip in hello graph.</span></span>

<span data-ttu-id="2b9f8-143">Szokatlan csúcsait keresi.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-143">Look for unusual peaks.</span></span> <span data-ttu-id="2b9f8-144">Általában várt válasz idő toorise rendelkező kérelmek növekedését.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-144">In general, expect response time toorise with a rise in requests.</span></span> <span data-ttu-id="2b9f8-145">Ha hello megnövekedhet le aránytalanul nagy, előfordulhat, hogy az alkalmazás elérés például a Processzor- vagy hello kapacitás használ szolgáltatás erőforrás korlátozni.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-145">If hello rise is disproportionate, your app might be hitting a resource limit such as CPU or hello capacity of a service it uses.</span></span>

<span data-ttu-id="2b9f8-146">Kattintson a hello csempe tooget többször megadott URL-címek.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-146">Click hello tile tooget times for specific URLs.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a><span data-ttu-id="2b9f8-147">A leghosszabb kérelmek</span><span class="sxs-lookup"><span data-stu-id="2b9f8-147">Slowest requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

<span data-ttu-id="2b9f8-148">Jeleníti meg, mely kérelmek teljesítményhangolás kell.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-148">Shows which requests might need performance tuning.</span></span>

### <a name="failed-requests"></a><span data-ttu-id="2b9f8-149">Sikertelen kérelmek</span><span class="sxs-lookup"><span data-stu-id="2b9f8-149">Failed requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

<span data-ttu-id="2b9f8-150">Kérelmek kivételt váltott ki a nem kezelt kivételek számát.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-150">A count of requests that threw uncaught exceptions.</span></span>

<span data-ttu-id="2b9f8-151">Kattintson a hello csempe toosee hello részleteit vonatkozó hibákat, és válassza ki az egyes toosee a részletek.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-151">Click hello tile toosee hello details of specific failures, and select an individual request toosee its detail.</span></span> 

<span data-ttu-id="2b9f8-152">Egyéni ellenőrzési hibák csak egy reprezentatív minta megmarad.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-152">Only a representative sample of failures is retained for individual inspection.</span></span>

### <a name="other-metrics"></a><span data-ttu-id="2b9f8-153">Más metrikákkal</span><span class="sxs-lookup"><span data-stu-id="2b9f8-153">Other metrics</span></span>
<span data-ttu-id="2b9f8-154">toosee milyen más metrikákkal jeleníti meg, kattintson egy grafikonon, és törölje a jelölést minden hello metrikák toosee hello teljes elérhető beállítás.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-154">toosee what other metrics you can display, click a graph, and then deselect all hello metrics toosee hello full available set.</span></span> <span data-ttu-id="2b9f8-155">Kattintson (i) toosee mindegyik metrikát definíciója.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-155">Click (i) toosee each metric's definition.</span></span>

![Kijelölésének megszüntetése az összes metrikák toosee hello teljes](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

<span data-ttu-id="2b9f8-157">A metrika letiltja a hello kiválasztása mások számára, amelyek nem szerepelnek a hello ugyanabban a diagramban.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-157">Selecting any metric disables hello others that can't appear on hello same chart.</span></span>

## <a name="set-alerts"></a><span data-ttu-id="2b9f8-158">Riasztások beállítása</span><span class="sxs-lookup"><span data-stu-id="2b9f8-158">Set alerts</span></span>
<span data-ttu-id="2b9f8-159">rendkívüli értékek a metrika az e-mailben értesítést toobe adja hozzá a riasztást.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-159">toobe notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="2b9f8-160">Kiválaszthatja a toosend hello e-mail toohello a rendszergazdák vagy toospecific e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-160">You can choose either toosend hello email toohello account administrators, or toospecific email addresses.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

<span data-ttu-id="2b9f8-161">Mielőtt hello hello erőforrás más tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-161">Set hello resource before hello other properties.</span></span> <span data-ttu-id="2b9f8-162">Ne válassza ki a hello webtesztben erőforrásokat, ha azt szeretné, hogy a teljesítmény vagy a szoftverhasználati mérési adatok tooset riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-162">Don't choose hello webtest resources if you want tooset alerts on performance or usage metrics.</span></span>

<span data-ttu-id="2b9f8-163">Lehet, amelyben kéri tooenter hello küszöbérték gondos toonote hello egység.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-163">Be careful toonote hello units in which you're asked tooenter hello threshold value.</span></span>

<span data-ttu-id="2b9f8-164">*Hello Hozzáadás riasztási gomb nem látható.*</span><span class="sxs-lookup"><span data-stu-id="2b9f8-164">*I don't see hello Add Alert button.*</span></span> <span data-ttu-id="2b9f8-165">-Van ez a csoport fiók toowhich csak olvasási hozzáféréssel rendelkezik?</span><span class="sxs-lookup"><span data-stu-id="2b9f8-165">- Is this a group account toowhich you have read-only access?</span></span> <span data-ttu-id="2b9f8-166">Kérje meg a fiókadminisztrátort hello.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-166">Check with hello account administrator.</span></span>

## <span data-ttu-id="2b9f8-167"><a name="diagnosis"></a>Problémák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="2b9f8-167"><a name="diagnosis"></a>Diagnosing issues</span></span>
<span data-ttu-id="2b9f8-168">Az alábbiakban néhány tippek megkereséséhez és teljesítménnyel kapcsolatos problémák diagnosztizálásához használható:</span><span class="sxs-lookup"><span data-stu-id="2b9f8-168">Here are a few tips for finding and diagnosing performance issues:</span></span>

* <span data-ttu-id="2b9f8-169">Állítson be [webalkalmazás-tesztek] [ availability] toobe riasztást kapni, ha a webhely nem működik, vagy helytelenül vagy lassan válaszol.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-169">Set up [web tests][availability] toobe alerted if your web site goes down or responds incorrectly or slowly.</span></span> 
* <span data-ttu-id="2b9f8-170">Hasonlítsa össze a más metrikák toosee hello kérelmek száma, ha sikertelen vagy lassú válasz a kapcsolódó tooload.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-170">Compare hello Request count with other metrics toosee if failures or slow response are related tooload.</span></span>
* <span data-ttu-id="2b9f8-171">[INSERT, és nyomkövetési utasítások keresési] [ diagnostic] a kód toohelp a rögzítési ponthoz problémákat.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-171">[Insert and search trace statements][diagnostic] in your code toohelp pinpoint problems.</span></span>
* <span data-ttu-id="2b9f8-172">A webalkalmazás műveletet a figyelheti [metrikák adatfolyamot][livestream].</span><span class="sxs-lookup"><span data-stu-id="2b9f8-172">Monitor your Web app in operation with [Live Metrics Stream][livestream].</span></span>
* <span data-ttu-id="2b9f8-173">A .net alkalmazás hello állapot rögzítése [pillanatkép hibakereső][snapshot].</span><span class="sxs-lookup"><span data-stu-id="2b9f8-173">Capture hello state of your .Net application with [Snapshot Debugger][snapshot].</span></span>

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-performance-investigation"></a><span data-ttu-id="2b9f8-174">Keresse meg és hárítsa el szűk keresztmetszetek interaktív teljesítményt vizsgálat</span><span class="sxs-lookup"><span data-stu-id="2b9f8-174">Find and fix performance bottlenecks with an interactive performance investigation</span></span>

<span data-ttu-id="2b9f8-175">Hello új Application Insights interaktív teljesítményt vizsgálat toolocate területeit webalkalmazás lassítja általános teljesítménye is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-175">You can use hello new Application Insights interactive performance investigation toolocate areas of your Web app that are slowing down overall performance.</span></span> <span data-ttu-id="2b9f8-176">Is gyorsan keresés adott lapok, amelyek lassítja, és használja a hello [profilkészítési eszköz](app-insights-profiler.md) toosee, ha az adott lapok közötti összefüggés.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-176">You can quickly find specific pages that are slowing down, and use hello [Profiling tool](app-insights-profiler.md) toosee if there is a correlation between these pages.</span></span>

### <a name="create-a-list-of-slow-performing-pages"></a><span data-ttu-id="2b9f8-177">Lassú teljesítő oldalak listájának létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b9f8-177">Create a list of slow performing pages</span></span> 

<span data-ttu-id="2b9f8-178">hello első lépése a teljesítményproblémák kereséshez tooget hello lassan válaszol a lapok listája.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-178">hello first step for finding performance issues is tooget a list of hello slow responding pages.</span></span> <span data-ttu-id="2b9f8-179">alább hello képernyőfelvétel mutatja be, hello teljesítmény panel tooget lehetséges oldalak tooinvestigate további listájának használatával.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-179">hello screen shot below demonstrates using hello Performance blade tooget a list of potential pages tooinvestigate further.</span></span> <span data-ttu-id="2b9f8-180">Gyorsan megtekintheti ezen a lapon, hogy történt egy slow-down hello válaszidő hello alkalmazás körülbelül 6:00 PM és újra körülbelül 10 óra.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-180">You can quickly see from this page that there was a slow-down in hello response time of hello app at approximately 6:00 PM and again at approximately 10 PM.</span></span> <span data-ttu-id="2b9f8-181">Láthatja, hogy hello ügyfél-részletek művelet volt, néhány hosszú futású műveleteket és a egy közepes válaszidő 507.05 ideje (MS).</span><span class="sxs-lookup"><span data-stu-id="2b9f8-181">You can also see that hello GET customer/details operation had some long running operations with a median response time of 507.05 milliseconds.</span></span> 

![Alkalmazásteljesítmény elemzések interaktív](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a><span data-ttu-id="2b9f8-183">Bizonyos lapokon részletekbe menően tárhatják</span><span class="sxs-lookup"><span data-stu-id="2b9f8-183">Drill down on specific pages</span></span>

<span data-ttu-id="2b9f8-184">Miután az alkalmazás teljesítményének pillanatképet, kaphat további részleteket a lassú végez műveleteket.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-184">Once you have a snapshot of your app's performance, you can get more details on specific slow-performing operations.</span></span> <span data-ttu-id="2b9f8-185">Kattintson az összes műveletet hello lista toosee hello részletei alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-185">Click on any operation in hello list toosee hello details as shown below.</span></span> <span data-ttu-id="2b9f8-186">Hello diagram láthatja, ha hello teljesítmény a függőség alapszik.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-186">From hello chart you can see if hello performance was based on a dependency.</span></span> <span data-ttu-id="2b9f8-187">Hány felhasználó tapasztalt hello különböző válaszidejét is látható.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-187">You can also see how many users experienced hello various response times.</span></span> 

![Application Insights műveletek panel](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a><span data-ttu-id="2b9f8-189">Egy adott időszakra vonatkozóan a részletekbe menően tárhatják</span><span class="sxs-lookup"><span data-stu-id="2b9f8-189">Drill down on a specific time period</span></span>

<span data-ttu-id="2b9f8-190">Ha azonosított egy adott idő tooinvestigate ponthoz, részletekbe menően tárhatják hello hello teljesítmény slow-down oka lehet, hogy bizonyos műveletek, akár további toolook.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-190">After you have identified a point in time tooinvestigate, drill down even further toolook at hello specific operations that might have caused hello performance slow-down.</span></span> <span data-ttu-id="2b9f8-191">Az adott időben kattint hello részleteit hello oldalon lent látható módon nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-191">When you click on a specific point in time you get hello details of hello page as shown below.</span></span> <span data-ttu-id="2b9f8-192">Hello az alábbi példa látható egy adott időszakban, valamint hello server válaszkódot és hello művelet időtartama felsorolt hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-192">In hello example below you can see hello operations listed for a given time period along with hello server response codes and hello operation duration.</span></span> <span data-ttu-id="2b9f8-193">A TFS munkaelem megnyitása, ha az információ tooyour fejlesztői csapat toosend szüksége van hello URL-címét is.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-193">You also have hello url for opening a TFS work item if you need toosend this information tooyour development team.</span></span>

![Application Insights időszelet](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a><span data-ttu-id="2b9f8-195">Egy bizonyos művelet részletekbe menően tárhatják</span><span class="sxs-lookup"><span data-stu-id="2b9f8-195">Drill down on a specific operation</span></span>

<span data-ttu-id="2b9f8-196">Ha azonosított egy adott idő tooinvestigate ponthoz, részletekbe menően tárhatják hello hello teljesítmény slow-down oka lehet, hogy bizonyos műveletek, akár további toolook.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-196">After you have identified a point in time tooinvestigate, drill down even further toolook at hello specific operations that might have caused hello performance slow-down.</span></span> <span data-ttu-id="2b9f8-197">Kattintson az egy művelet hello lista toosee hello adatait hello művelet alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-197">Click on an operation from hello list toosee hello details of hello operation as shown below.</span></span> <span data-ttu-id="2b9f8-198">Ebben a példában láthatja, hogy hello művelet végrehajtása sikertelen volt, és az Application Insights nyújtott hello hello részleteit kivétel hello alkalmazás kivételt váltott ki.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-198">In this example you can see that hello operation failed, and Application Insights has provided hello details of hello exception hello application threw.</span></span> <span data-ttu-id="2b9f8-199">Ezen a panelen újra, a TFS munkaelem könnyedén létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-199">Again, you can easily create a TFS work item from this blade.</span></span>

![Application Insights művelet panel](./media/app-insights-web-monitor-performance/performance3.png)


## <span data-ttu-id="2b9f8-201"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b9f8-201"><a name="next"></a>Next steps</span></span>
<span data-ttu-id="2b9f8-202">[Webalkalmazás-tesztek] [ availability] -webes kérelmek elküldött tooyour alkalmazás hello világ a rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-202">[Web tests][availability] - Have web requests sent tooyour application at regular intervals from around hello world.</span></span>

<span data-ttu-id="2b9f8-203">[Rögzítése, és diagnosztikai nyomkövetési keresési] [ diagnostic] - nyomkövetési hívások beszúrása és hello eredmények toopinpoint problémák keletkezik.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-203">[Capture and search diagnostic traces][diagnostic] - Insert trace calls and sift through hello results toopinpoint issues.</span></span>

<span data-ttu-id="2b9f8-204">[Használat nyomon követése] [ usage] -megtudhatja, hogyan személyek használhassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2b9f8-204">[Usage tracking][usage] - Find out how people use your application.</span></span>

<span data-ttu-id="2b9f8-205">[Hibaelhárítási] [ qna] - és a kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="2b9f8-205">[Troubleshooting][qna] - and Q & A</span></span>



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




---
title: "a JavaScript aaaAzure Application Insights webalkalmazás-alkalmazások |} Microsoft Docs"
description: "Lekérheti a lapmegtekintések és a munkamenetek számát, a webes ügyfél adatait, és nyomon követheti a használati mintákat. Kivételeket és teljesítményproblémákat észlelhet a JavaScript weblapokon."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 3b710d09-6ab4-4004-b26a-4fa840039500
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 986db3c3776471f9f8556f4e09f2d02aad022549
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-web-pages"></a><span data-ttu-id="8220a-104">Application Insights weblapokhoz</span><span class="sxs-lookup"><span data-stu-id="8220a-104">Application Insights for web pages</span></span>
<span data-ttu-id="8220a-105">Tájékozódhat hello teljesítmény és a weblap vagy az alkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="8220a-105">Find out about hello performance and usage of your web page or app.</span></span> <span data-ttu-id="8220a-106">Ha ad hozzá [Application Insights](app-insights-overview.md) tooyour lap parancsfájl, kapott időzítés lap terhelések és AJAX hívások, számok és böngésző kivételeket és a AJAX hibák, valamint a felhasználók és a munkamenet számok részleteit.</span><span class="sxs-lookup"><span data-stu-id="8220a-106">If you add [Application Insights](app-insights-overview.md) tooyour page script, you get timings of page loads and AJAX calls, counts and details of browser exceptions and AJAX failures, as well as users and session counts.</span></span> <span data-ttu-id="8220a-107">Ezek mindegyike szegmentálható lap, ügyfél operációs rendszere és böngészőverziója, földrajzi hely és más dimenziók alapján.</span><span class="sxs-lookup"><span data-stu-id="8220a-107">All these can be segmented by page, client OS and browser version, geo location, and other dimensions.</span></span> <span data-ttu-id="8220a-108">Beállíthat riasztásokat a hibaszámokról és a lassú lapbetöltésekről.</span><span class="sxs-lookup"><span data-stu-id="8220a-108">You can set alerts on failure counts or slow page loading.</span></span> <span data-ttu-id="8220a-109">És a JavaScript-kód nyomkövetési hívások beszúrásával nyomon követheti a weblap alkalmazás különböző funkcióit hello használata.</span><span class="sxs-lookup"><span data-stu-id="8220a-109">And by inserting trace calls in your JavaScript code, you can track how hello different features of your web page application are used.</span></span>

<span data-ttu-id="8220a-110">Az Application Insights bármely weblappal használható – csak egy rövid JavaScript-kódot kell hozzáadnia.</span><span class="sxs-lookup"><span data-stu-id="8220a-110">Application Insights can be used with any web pages - you just add a short piece of JavaScript.</span></span> <span data-ttu-id="8220a-111">Ha a webszolgáltatása [Java](app-insights-java-get-started.md) vagy [ASP.NET](app-insights-asp-net.md), telemetriát integrálhat a kiszolgálóról és az ügyfelekről.</span><span class="sxs-lookup"><span data-stu-id="8220a-111">If your web service is [Java](app-insights-java-get-started.md) or [ASP.NET](app-insights-asp-net.md), you can integrate telemetry from your server and clients.</span></span>

![A portal.azure.com címen nyissa meg az alkalmazás erőforrását, és kattintson a Böngésző elemre](./media/app-insights-javascript/03.png)

<span data-ttu-id="8220a-113">Előfizetés túl kell[Microsoft Azure](https://azure.com).</span><span class="sxs-lookup"><span data-stu-id="8220a-113">You need a subscription too[Microsoft Azure](https://azure.com).</span></span> <span data-ttu-id="8220a-114">Ha olyan szervezeti előfizetéssel rendelkezik, kérje a Microsoft Account tooit hello tulajdonos tooadd.</span><span class="sxs-lookup"><span data-stu-id="8220a-114">If your team has an organizational subscription, ask hello owner tooadd your Microsoft Account tooit.</span></span> <span data-ttu-id="8220a-115">A fejlesztés és a kis mértékű használat nem kerül semmibe.</span><span class="sxs-lookup"><span data-stu-id="8220a-115">Development and small-scale use won't cost anything.</span></span>

## <a name="set-up-application-insights-for-your-web-page"></a><span data-ttu-id="8220a-116">Az Application Insights beállítása a weboldalához</span><span class="sxs-lookup"><span data-stu-id="8220a-116">Set up Application Insights for your web page</span></span>
<span data-ttu-id="8220a-117">Adjon hozzá hello betöltő kód részlet tooyour weblapok, az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="8220a-117">Add hello loader code snippet tooyour web pages, as follows.</span></span>

### <a name="open-or-create-application-insights-resource"></a><span data-ttu-id="8220a-118">Application Insights-erőforrás megnyitása vagy létrehozása</span><span class="sxs-lookup"><span data-stu-id="8220a-118">Open or create Application Insights resource</span></span>
<span data-ttu-id="8220a-119">hello Application Insights-erőforrás, ahol megjelenik a lapon teljesítmény- és használati adatait.</span><span class="sxs-lookup"><span data-stu-id="8220a-119">hello Application Insights resource is where data about your page's performance and usage is displayed.</span></span> 

<span data-ttu-id="8220a-120">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8220a-120">Sign into [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="8220a-121">Ha már beállította az alkalmazás hello kiszolgálóoldali figyelést, már rendelkezik egy erőforrás:</span><span class="sxs-lookup"><span data-stu-id="8220a-121">If you already set up monitoring for hello server side of your app, you already have a resource:</span></span>

![Válassza a Tallózás, Fejlesztői szolgáltatások, Application Insights lehetőséget.](./media/app-insights-javascript/01-find.png)

<span data-ttu-id="8220a-123">Ha nincs erőforrása, hozza létre azt:</span><span class="sxs-lookup"><span data-stu-id="8220a-123">If you don't have one, create it:</span></span>

![Válassza az Új, Fejlesztői szolgáltatások, Application Insights lehetőséget.](./media/app-insights-javascript/01-create.png)

<span data-ttu-id="8220a-125">*Máris kérdései vannak?*</span><span class="sxs-lookup"><span data-stu-id="8220a-125">*Questions already?*</span></span> <span data-ttu-id="8220a-126">[További információk erőforrások létrehozásáról](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="8220a-126">[More about creating a resource](app-insights-create-new-resource.md).</span></span>

### <a name="add-hello-sdk-script-tooyour-app-or-web-pages"></a><span data-ttu-id="8220a-127">Hello SDK parancsfájl tooyour alkalmazás vagy a weblapok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8220a-127">Add hello SDK script tooyour app or web pages</span></span>
<span data-ttu-id="8220a-128">A gyors üzembe helyezés hello parancsfájl weblapok érhető el:</span><span class="sxs-lookup"><span data-stu-id="8220a-128">In Quick Start, get hello script for web pages:</span></span>

![Az alkalmazás áttekintése paneljén válassza a gyors üzembe helyezés, majd kód toomonitor a weblapokat.](./media/app-insights-javascript/02-monitor-web-page.png)

<span data-ttu-id="8220a-131">Hello előtt hello parancsprogram beszúrása `</head>` címke tootrack kívánt minden oldalon.</span><span class="sxs-lookup"><span data-stu-id="8220a-131">Insert hello script just before hello `</head>` tag of every page you want tootrack.</span></span> <span data-ttu-id="8220a-132">Ha a webhely mesterlapra, nem adhat meg hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="8220a-132">If your website has a master page, you can put hello script there.</span></span> <span data-ttu-id="8220a-133">Példa:</span><span class="sxs-lookup"><span data-stu-id="8220a-133">For example:</span></span>

* <span data-ttu-id="8220a-134">Egy ASP.NET MVC-projektben a következő helyre helyezné a szkriptet: `View\Shared\_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="8220a-134">In an ASP.NET MVC project, you'd put it in `View\Shared\_Layout.cshtml`</span></span>
* <span data-ttu-id="8220a-135">Nyissa meg a SharePoint-webhely hello Vezérlőpultján, [helybeállításokat / mesterlap](app-insights-sharepoint.md).</span><span class="sxs-lookup"><span data-stu-id="8220a-135">In a SharePoint site, on hello control panel, open [Site Settings / Master Page](app-insights-sharepoint.md).</span></span>

<span data-ttu-id="8220a-136">hello hello instrumentation kulcs, amely arra utasítja a hello adatok tooyour Application Insights-erőforrást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8220a-136">hello script contains hello instrumentation key that directs hello data tooyour Application Insights resource.</span></span> 

<span data-ttu-id="8220a-137">([Hello parancsfájl ismerteti. ](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))</span><span class="sxs-lookup"><span data-stu-id="8220a-137">([Deeper explanation of hello script.](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))</span></span>

<span data-ttu-id="8220a-138">*(Ha jól ismert weblapkeretrendszert használ, keressen Application Insights-adaptereket. Például van [egy AngularJS modul](http://ngmodules.org/modules/angular-appinsights).)*</span><span class="sxs-lookup"><span data-stu-id="8220a-138">*(If you're using a well-known web page framework, look around for Application Insights adaptors. For example, there's [an AngularJS module](http://ngmodules.org/modules/angular-appinsights).)*</span></span>

## <a name="detailed-configuration"></a><span data-ttu-id="8220a-139">Részletes konfiguráció</span><span class="sxs-lookup"><span data-stu-id="8220a-139">Detailed configuration</span></span>
<span data-ttu-id="8220a-140">Több [paramétert](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) beállíthat, de a legtöbb esetben erre nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="8220a-140">There are several [parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) you can set, though in most cases, you shouldn't need to.</span></span> <span data-ttu-id="8220a-141">Például tiltsa le vagy hello / lapmegtekintés (tooreduce forgalom) jelentett Ajax-hívások száma.</span><span class="sxs-lookup"><span data-stu-id="8220a-141">For example, you can disable or limit hello number of Ajax calls reported per page view (tooreduce traffic).</span></span> <span data-ttu-id="8220a-142">Vagy a hibakeresési mód toohave telemetriai áthelyezés gyorsan hello-feldolgozási folyamaton keresztül nélkül kötegelni alatt állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="8220a-142">Or you can set debug mode toohave telemetry move rapidly through hello pipeline without being batched.</span></span>

<span data-ttu-id="8220a-143">tooset ezeket a paramétereket, keresse meg a sort a hello kódrészletet, és adja meg vesszővel tagolt több elemet:</span><span class="sxs-lookup"><span data-stu-id="8220a-143">tooset these parameters, look for this line in hello code snippet, and add more comma-separated items after it:</span></span>

    })({
      instrumentationKey: "..."
      // Insert here
    });

<span data-ttu-id="8220a-144">Hello [felhasználható paraméterek](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8220a-144">hello [available parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) include:</span></span>

    // Send telemetry immediately without batching.
    // Remember tooremove this when no longer required, as it
    // can affect browser performance.
    enableDebug: boolean,

    // Don't log browser exceptions.
    disableExceptionTracking: boolean,

    // Don't log ajax calls.
    disableAjaxTracking: boolean,

    // Limit number of Ajax calls logged, tooreduce traffic.
    maxAjaxCallsPerView: 10, // default is 500

    // Time page load up tooexecution of first trackPageView().
    overridePageViewDuration: boolean,

    // Set these dynamically for an authenticated user.
    appUserId: string,
    accountId: string,



## <span data-ttu-id="8220a-145"><a name="run"></a>Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="8220a-145"><a name="run"></a>Run your app</span></span>
<span data-ttu-id="8220a-146">A webes alkalmazás futtatását, használja a toogenerate telemetriai adatokat, és várjon egy pár másodpercen közben.</span><span class="sxs-lookup"><span data-stu-id="8220a-146">Run your web app, use it a while toogenerate telemetry, and wait a few seconds.</span></span> <span data-ttu-id="8220a-147">Továbbra is fut hello segítségével **F5** billentyűt a fejlesztői számítógépén vagy közzététele és teheti a felhasználóknak össze azt.</span><span class="sxs-lookup"><span data-stu-id="8220a-147">You can either run it using hello **F5** key on your development machine, or publish it and let users play with it.</span></span>

<span data-ttu-id="8220a-148">Ha azt szeretné, hogy a webes alkalmazás küld tooApplication Insights toocheck hello telemetriai, a böngésző hibakeresési eszközök használata (**F12** sok böngészőkben).</span><span class="sxs-lookup"><span data-stu-id="8220a-148">If you want toocheck hello telemetry that a web app is sending tooApplication Insights, use your browser's debugging tools (**F12** on many browsers).</span></span> <span data-ttu-id="8220a-149">Toodc.services.visualstudio.com adatokat küldi el.</span><span class="sxs-lookup"><span data-stu-id="8220a-149">Data is sent toodc.services.visualstudio.com.</span></span>

## <a name="explore-your-browser-performance-data"></a><span data-ttu-id="8220a-150">A böngésző teljesítményadatainak feltárása</span><span class="sxs-lookup"><span data-stu-id="8220a-150">Explore your browser performance data</span></span>
<span data-ttu-id="8220a-151">Megnyitás hello böngésző panel tooshow összesíteni a teljesítményadatokat a felhasználók böngészőjének.</span><span class="sxs-lookup"><span data-stu-id="8220a-151">Open hello Browser blade tooshow aggregated performance data from your users' browsers.</span></span>

![A portal.azure.com címen nyissa meg az alkalmazás erőforrását, és kattintson a Beállítások, Böngésző lehetőségre](./media/app-insights-javascript/03.png)

<span data-ttu-id="8220a-153">*Még nincsenek adatok? Kattintson a **frissítése** hello oldal hello tetején. Még mindig semmi? Lásd: [Hibaelhárítás](app-insights-troubleshoot-faq.md).*</span><span class="sxs-lookup"><span data-stu-id="8220a-153">*No data yet? Click **Refresh** at hello top of hello page. Still nothing? See [Troubleshooting](app-insights-troubleshoot-faq.md).*</span></span>

<span data-ttu-id="8220a-154">hello böngésző panel van egy [Metrikaböngésző panel](app-insights-metrics-explorer.md) előre beállított szűrők és diagram beállításokat.</span><span class="sxs-lookup"><span data-stu-id="8220a-154">hello Browser blade is a [Metrics Explorer blade](app-insights-metrics-explorer.md) with preset filters and chart selections.</span></span> <span data-ttu-id="8220a-155">Ha szeretné, és menti a hello eredmény kedvencként szerkesztheti hello időtartományt, szűrők és diagram konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="8220a-155">You can edit hello time range, filters, and chart configuration if you want, and save hello result as a favorite.</span></span> <span data-ttu-id="8220a-156">Kattintson a **visszaállítása** tooget hátsó toohello eredeti panel konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="8220a-156">Click **Restore defaults** tooget back toohello original blade configuration.</span></span>

## <a name="page-load-performance"></a><span data-ttu-id="8220a-157">Lapbetöltés teljesítménye</span><span class="sxs-lookup"><span data-stu-id="8220a-157">Page load performance</span></span>
<span data-ttu-id="8220a-158">Hello felső a szegmentált lapbetöltési idők-diagram.</span><span class="sxs-lookup"><span data-stu-id="8220a-158">At hello top is a segmented chart of page load times.</span></span> <span data-ttu-id="8220a-159">hello hello diagram magassága az alkalmazást a a felhasználók böngészőjének hello átlagos idő tooload és megjelenítendő lapok jelöli.</span><span class="sxs-lookup"><span data-stu-id="8220a-159">hello total height of hello chart represents hello average time tooload and display pages from your app in your users' browsers.</span></span> <span data-ttu-id="8220a-160">hello idő mérése a hello böngésző hello kezdeti HTTP-kérelem küld, amíg az összes szinkron betöltési események dolgozott, beleértve a elrendezés és a parancsfájlok futtatását.</span><span class="sxs-lookup"><span data-stu-id="8220a-160">hello time is measured from when hello browser sends hello initial HTTP request until all synchronous load events have been processed, including layout and running scripts.</span></span> <span data-ttu-id="8220a-161">Ebben nem tartoznak bele az aszinkron feladatok, például a kijelzők betöltése az AJAX hívásokból.</span><span class="sxs-lookup"><span data-stu-id="8220a-161">It doesn't include asynchronous tasks such as loading web parts from AJAX calls.</span></span>

<span data-ttu-id="8220a-162">hello diagram szegmensek hello összes lap betöltési ideje az hello [W3C határozza szabvány időzítés](http://www.w3.org/TR/navigation-timing/#processing-model).</span><span class="sxs-lookup"><span data-stu-id="8220a-162">hello chart segments hello total page load time into hello [standard timings defined by W3C](http://www.w3.org/TR/navigation-timing/#processing-model).</span></span> 

![](./media/app-insights-javascript/08-client-split.png)

<span data-ttu-id="8220a-163">Vegye figyelembe, hogy hello *hálózati csatlakozás* idő gyakran alacsonyabb, mint a várt, akkor az átlagos keresztül elérhető hello toohello összes kérelem.</span><span class="sxs-lookup"><span data-stu-id="8220a-163">Note that hello *network connect* time is often lower than you might expect, because it's an average over all requests from hello browser toohello server.</span></span> <span data-ttu-id="8220a-164">Sok egyes kérelmeket 0 connect időt igénybe, mert már van egy aktív kapcsolat toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="8220a-164">Many individual requests have a connect time of 0 because there is already an active connection toohello server.</span></span>

### <a name="slow-loading"></a><span data-ttu-id="8220a-165">Lassú a betöltés?</span><span class="sxs-lookup"><span data-stu-id="8220a-165">Slow loading?</span></span>
<span data-ttu-id="8220a-166">A lassú lapbetöltések a felhasználók elégedetlenségének fő forrásai.</span><span class="sxs-lookup"><span data-stu-id="8220a-166">Slow page loads are a major source of dissatisfaction for your users.</span></span> <span data-ttu-id="8220a-167">Hello diagram lassúlap betölti azt jelzi, ha a rendszer egyszerű toodo néhány diagnosztikai kutatási.</span><span class="sxs-lookup"><span data-stu-id="8220a-167">If hello chart indicates slow page loads, it's easy toodo some diagnostic research.</span></span>

<span data-ttu-id="8220a-168">hello a diagram összes lap terhelések hello átlaga mutatja az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8220a-168">hello chart shows hello average of all page loads in your app.</span></span> <span data-ttu-id="8220a-169">Ha hello probléma toosee korlátozódik tooparticular lapok, megjelenését további hello panel le, ahol a rács által szegmentált lap URL-cím:</span><span class="sxs-lookup"><span data-stu-id="8220a-169">toosee if hello problem is confined tooparticular pages, look further down hello blade, where there's a grid segmented by page URL:</span></span>

![](./media/app-insights-javascript/09-page-perf.png)

<span data-ttu-id="8220a-170">Figyelje meg hello nézet oldalszám és a szórást.</span><span class="sxs-lookup"><span data-stu-id="8220a-170">Notice hello page view count and standard deviation.</span></span> <span data-ttu-id="8220a-171">Ha hello oldalszám nagyon alacsony, majd hello a probléma nem befolyásolja a felhasználók sokkal.</span><span class="sxs-lookup"><span data-stu-id="8220a-171">If hello page count is very low, then hello issue isn't affecting users much.</span></span> <span data-ttu-id="8220a-172">A magas szórás (a saját magát összehasonlítható toohello átlagos) azt jelzi, hogy nagy mennyiségű egyedi mérések közötti eltérés.</span><span class="sxs-lookup"><span data-stu-id="8220a-172">A high standard deviation (comparable toohello average itself) indicates a lot of variation between individual measurements.</span></span>

<span data-ttu-id="8220a-173">**Nagyítsa ki az egyik URL-címet és az egyik lapmegtekintést.**</span><span class="sxs-lookup"><span data-stu-id="8220a-173">**Zoom in on one URL and one page view.**</span></span> <span data-ttu-id="8220a-174">Kattintson a lap neve toosee egy panel böngésző diagramok szűrt csak toothat URL-címének; majd a nézet egy példánya.</span><span class="sxs-lookup"><span data-stu-id="8220a-174">Click any page name toosee a blade of browser charts filtered just toothat URL; and then on an instance of a page view.</span></span>

![](./media/app-insights-javascript/35.png)

<span data-ttu-id="8220a-175">Kattintson a `...` , hogy az esemény tulajdonságainak teljes listáját, vagy vizsgálja meg a hello Ajax-hívások, illetve a kapcsolódó eseményeket.</span><span class="sxs-lookup"><span data-stu-id="8220a-175">Click `...` for a full list of properties for that event, or inspect hello Ajax calls and related events.</span></span> <span data-ttu-id="8220a-176">Lassú Ajax-hívások hatással hello betöltési ideje Általános lapon, ha azok szinkron.</span><span class="sxs-lookup"><span data-stu-id="8220a-176">Slow Ajax calls affect hello overall page load time if they are synchronous.</span></span> <span data-ttu-id="8220a-177">Kapcsolódó események tartalmazzák a hello kiszolgálói kérelmek azonos URL-CÍMMEL (Ha a webkiszolgálón állított be az Application Insights).</span><span class="sxs-lookup"><span data-stu-id="8220a-177">Related events include server requests for hello same URL (if you've set up Application Insights on your web server).</span></span>

<span data-ttu-id="8220a-178">**Lapteljesítmény az idő függvényében.**</span><span class="sxs-lookup"><span data-stu-id="8220a-178">**Page performance over time.**</span></span> <span data-ttu-id="8220a-179">Eközben hello böngészők panelen átalakítása hello lapmegtekintés betöltési ideje rács egy sor diagram toosee Ha csúcsait adott időpontokban:</span><span class="sxs-lookup"><span data-stu-id="8220a-179">Back at hello Browsers blade, change hello Page View Load Time grid into a line chart toosee if there were peaks at particular times:</span></span>

![Kattintson a hello head hello rács, és egy új diagram típusának kiválasztása](./media/app-insights-javascript/10-page-perf-area.png)

<span data-ttu-id="8220a-181">**Szegmentáljon más dimenziók alapján.**</span><span class="sxs-lookup"><span data-stu-id="8220a-181">**Segment by other dimensions.**</span></span> <span data-ttu-id="8220a-182">Lehet, hogy a lapok lassabb tooload szerepelnek a böngésző, az ügyfél operációs rendszer vagy a felhasználó adott nyelvterület?</span><span class="sxs-lookup"><span data-stu-id="8220a-182">Maybe your pages are slower tooload on a particular browser, client OS, or user locality?</span></span> <span data-ttu-id="8220a-183">Új diagram hozzáadása és hello kísérletezhet **csoportosítási** dimenzió.</span><span class="sxs-lookup"><span data-stu-id="8220a-183">Add a new chart and experiment with hello **Group-by** dimension.</span></span>

![](./media/app-insights-javascript/21.png)

## <a name="ajax-performance"></a><span data-ttu-id="8220a-184">AJAX teljesítménye</span><span class="sxs-lookup"><span data-stu-id="8220a-184">AJAX Performance</span></span>
<span data-ttu-id="8220a-185">Győződjön meg arról, hogy a weblapjain lévő összes AJAX hívás teljesítménye megfelelő.</span><span class="sxs-lookup"><span data-stu-id="8220a-185">Make sure any AJAX calls in your web pages are performing well.</span></span> <span data-ttu-id="8220a-186">Ezek gyakran használt toofill részét képezik a lap aszinkron módon történik.</span><span class="sxs-lookup"><span data-stu-id="8220a-186">They are often used toofill parts of your page asynchronously.</span></span> <span data-ttu-id="8220a-187">Bár hello Általános lap töltődik be azonnal, a felhasználók sikerült kell további javulás is meghiúsult: üres a kijelzők indítás őket az adatok tooappear vár.</span><span class="sxs-lookup"><span data-stu-id="8220a-187">Although hello overall page might load promptly, your users could be frustrated by staring at blank web parts, waiting for data tooappear in them.</span></span>

<span data-ttu-id="8220a-188">A weblap AJAX-hívások szerint függőségeinek hello böngészők panelen jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="8220a-188">AJAX calls made from your web page are shown on hello Browsers blade as dependencies.</span></span>

<span data-ttu-id="8220a-189">Nincsenek összefoglaló diagramok hello hello panel felső részén:</span><span class="sxs-lookup"><span data-stu-id="8220a-189">There are summary charts in hello upper part of hello blade:</span></span>

![](./media/app-insights-javascript/31.png)

<span data-ttu-id="8220a-190">és részletes rácsok lejjebb:</span><span class="sxs-lookup"><span data-stu-id="8220a-190">and detailed grids lower down:</span></span>

![](./media/app-insights-javascript/33.png)

<span data-ttu-id="8220a-191">Kattintson valamelyik sorra a részletekért.</span><span class="sxs-lookup"><span data-stu-id="8220a-191">Click any row for specific details.</span></span>

> [!NOTE]
> <span data-ttu-id="8220a-192">Ha törli a hello böngészők szűrő hello panelen, a kiszolgáló és az AJAX-függőségek szerepelnek a diagramokat.</span><span class="sxs-lookup"><span data-stu-id="8220a-192">If you delete hello Browsers filter on hello blade, both server and AJAX dependencies are included in these charts.</span></span> <span data-ttu-id="8220a-193">Kattintson az alapértelmezett értékek visszaállítása tooreconfigure hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="8220a-193">Click Restore Defaults tooreconfigure hello filter.</span></span>
> 
> 

<span data-ttu-id="8220a-194">**a sikertelen Ajax-hívások toodrill** görgessen lefelé toohello függőségi hibák rács, és kattintson az egy sor toosee olyan specifikus példányai.</span><span class="sxs-lookup"><span data-stu-id="8220a-194">**toodrill into failed Ajax calls** scroll down toohello Dependency failures grid, and then click a row toosee specific instances.</span></span>

![](./media/app-insights-javascript/37.png)


<span data-ttu-id="8220a-195">Kattintson a `...` az Ajax-hívások hello teljes telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="8220a-195">Click `...` for hello full telemetry for an Ajax call.</span></span>

### <a name="no-ajax-calls-reported"></a><span data-ttu-id="8220a-196">Nincsenek Ajax hívások jelentve?</span><span class="sxs-lookup"><span data-stu-id="8220a-196">No Ajax calls reported?</span></span>
<span data-ttu-id="8220a-197">AJAX-hívások bármely hello parancsfájlt a weblapot HTTP/HTTPS hívásoknak tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8220a-197">Ajax calls include any HTTP/HTTPS  calls made from hello script of your web page.</span></span> <span data-ttu-id="8220a-198">Ha nem látja ezeket a jelentett, ellenőrizze, hogy hello kódrészletet nem állítja be a hello `disableAjaxTracking` vagy `maxAjaxCallsPerView` [paraméterek](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span><span class="sxs-lookup"><span data-stu-id="8220a-198">If you don't see them reported, check that hello code snippet doesn't set hello `disableAjaxTracking` or `maxAjaxCallsPerView` [parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span></span>

## <a name="browser-exceptions"></a><span data-ttu-id="8220a-199">Böngészőkivételek</span><span class="sxs-lookup"><span data-stu-id="8220a-199">Browser exceptions</span></span>
<span data-ttu-id="8220a-200">Hello böngészők paneljén van egy kivételek összegző diagram, és a kivétel típusok további hello panel le.</span><span class="sxs-lookup"><span data-stu-id="8220a-200">On hello Browsers blade, there's an exceptions summary chart, and a grid of exception types further down hello blade.</span></span>

![](./media/app-insights-javascript/39.png)

<span data-ttu-id="8220a-201">Böngésző kivételek jelentett nem látható, ha ellenőrizze, hogy hello kódrészletet nem állítja be a hello `disableExceptionTracking` [paraméter](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span><span class="sxs-lookup"><span data-stu-id="8220a-201">If you don't see browser exceptions reported, check that hello code snippet doesn't set hello `disableExceptionTracking` [parameter](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span></span>

## <a name="inspect-individual-page-view-events"></a><span data-ttu-id="8220a-202">Egyéni lapmegtekintési események vizsgálata</span><span class="sxs-lookup"><span data-stu-id="8220a-202">Inspect individual page view events</span></span>

<span data-ttu-id="8220a-203">Általában az Application Insights elemzi a lapmegtekintési telemetriát, és Ön csak összegző jelentéseket tekinthet meg, amelyek az összes felhasználó átlagát tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="8220a-203">Usually page view telemetry is analyzed by Application Insights and you see only cumulative reports, averaged over all your users.</span></span> <span data-ttu-id="8220a-204">De a hibakereséshez az egyes lapmegtekintési eseményeket is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="8220a-204">But for debugging purposes, you can also look at individual page view events.</span></span>

<span data-ttu-id="8220a-205">Hello diagnosztikai Search paneljét állítson be szűrőt tooPage nézet.</span><span class="sxs-lookup"><span data-stu-id="8220a-205">In hello Diagnostic Search blade, set Filters tooPage View.</span></span>

![](./media/app-insights-javascript/12-search-pages.png)

<span data-ttu-id="8220a-206">Válassza ki a bármely esemény toosee további információkhoz juthat.</span><span class="sxs-lookup"><span data-stu-id="8220a-206">Select any event toosee more detail.</span></span> <span data-ttu-id="8220a-207">Hello részleteit megjelenítő oldalon kattintson "..." toosee is további információkhoz juthat.</span><span class="sxs-lookup"><span data-stu-id="8220a-207">In hello details page, click "..." toosee even more detail.</span></span>

> [!NOTE]
> <span data-ttu-id="8220a-208">Ha [keresési](app-insights-diagnostic-search.md), figyelje meg, hogy rendelkezik-e toomatch teljes szó: "Abou" és "vebben" nem egyeznek meg "Kapcsolatos".</span><span class="sxs-lookup"><span data-stu-id="8220a-208">If you use [Search](app-insights-diagnostic-search.md), notice that you have toomatch whole words: "Abou" and "bout" do not match "About".</span></span>
> 
> 

<span data-ttu-id="8220a-209">Is használhatja hello hatékony [Log Analytics lekérdezési nyelv](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) toosearch lapmegtekintéseiről.</span><span class="sxs-lookup"><span data-stu-id="8220a-209">You can also use hello powerful [Log Analytics query language](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) toosearch page views.</span></span>

### <a name="page-view-properties"></a><span data-ttu-id="8220a-210">Lapmegtekintési tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="8220a-210">Page view properties</span></span>
* <span data-ttu-id="8220a-211">**Lapmegtekintés időtartama**</span><span class="sxs-lookup"><span data-stu-id="8220a-211">**Page view duration**</span></span> 
  
  * <span data-ttu-id="8220a-212">Alapértelmezés szerint hello alkalommal vesz tooload hello lap, és az ügyfél kérelem toofull töltse be a (beleértve a kiegészítő fájlokat, de kivételével aszinkron feladatokhoz, mint az Ajax-hívások).</span><span class="sxs-lookup"><span data-stu-id="8220a-212">By default, hello time it takes tooload hello page, from client request toofull load (including auxiliary files but excluding asynchronous tasks such as Ajax calls).</span></span> 
  * <span data-ttu-id="8220a-213">Ha `overridePageViewDuration` a hello [lap konfigurációs](#detailed-configuration), hello első ügyfél kérést tooexecution a hello közötti távolság `trackPageView`.</span><span class="sxs-lookup"><span data-stu-id="8220a-213">If you set `overridePageViewDuration` in hello [page configuration](#detailed-configuration), hello interval between client request tooexecution of hello first `trackPageView`.</span></span> <span data-ttu-id="8220a-214">Ha trackPageView szokásos pozícióját hello parancsfájl hello inicializálás után helyezi át, akkor egy másik értéket fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="8220a-214">If you moved trackPageView from its usual position after hello initialization of hello script, it will reflect a different value.</span></span>
  * <span data-ttu-id="8220a-215">Ha `overridePageViewDuration` be van, egy argumentum hello megadott időtartamot `trackPageView()` hívja, majd hello argumentum értékét használja.</span><span class="sxs-lookup"><span data-stu-id="8220a-215">If `overridePageViewDuration` is set and a duration argument is provided in hello `trackPageView()` call, then hello argument value is used instead.</span></span> 

## <a name="custom-page-counts"></a><span data-ttu-id="8220a-216">Egyéni lapok száma</span><span class="sxs-lookup"><span data-stu-id="8220a-216">Custom page counts</span></span>
<span data-ttu-id="8220a-217">Lapok alapértelmezés szerint minden alkalommal, amikor egy új lap betölti a hello az ügyfél böngészője történik.</span><span class="sxs-lookup"><span data-stu-id="8220a-217">By default, a page count occurs each time a new page loads into hello client browser.</span></span>  <span data-ttu-id="8220a-218">De érdemes lehet további Lapmegtekintések toocount.</span><span class="sxs-lookup"><span data-stu-id="8220a-218">But you might want toocount additional page views.</span></span> <span data-ttu-id="8220a-219">Például egy lap lapok megjelenítheti benne lévő tartalom, és toocount oldal szüksége, amikor hello felhasználói lapok vált.</span><span class="sxs-lookup"><span data-stu-id="8220a-219">For example, a page might display its content in tabs and you want toocount a page when hello user switches tabs.</span></span> <span data-ttu-id="8220a-220">Vagy JavaScript-kód hello lap előfordulhat, hogy új tartalom betöltése hello böngésző URL-cím megváltoztatása nélkül.</span><span class="sxs-lookup"><span data-stu-id="8220a-220">Or JavaScript code in hello page might load new content without changing hello browser's URL.</span></span>

<span data-ttu-id="8220a-221">Szúrja be a JavaScript hívás pontján hello megfelelő az Ügyfélkód:</span><span class="sxs-lookup"><span data-stu-id="8220a-221">Insert a JavaScript call like this at hello appropriate point in your client code:</span></span>

    appInsights.trackPageView(myPageName);

<span data-ttu-id="8220a-222">hello lapnév tartalmazhat hello azonos karaktert tartalmaz, mint egy URL-címet, de semmi után "#" vagy "?" figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="8220a-222">hello page name can contain hello same characters as a URL, but anything after "#" or "?" is ignored.</span></span>

## <a name="usage-tracking"></a><span data-ttu-id="8220a-223">Használatkövetés</span><span class="sxs-lookup"><span data-stu-id="8220a-223">Usage tracking</span></span>
<span data-ttu-id="8220a-224">Szeretné, hogy ki a felhasználók mit alkalmazása toofind?</span><span class="sxs-lookup"><span data-stu-id="8220a-224">Want toofind out what your users do with your app?</span></span>

* [<span data-ttu-id="8220a-225">Információk a használatkövetésről</span><span class="sxs-lookup"><span data-stu-id="8220a-225">Learn about usage tracking</span></span>](app-insights-web-track-usage.md)
* <span data-ttu-id="8220a-226">[Információk az egyéni eseményekről és a mérőszám API-ról](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="8220a-226">[Learn about custom events and metrics API](app-insights-api-custom-events-metrics.md).</span></span>

## <span data-ttu-id="8220a-227"><a name="video"></a>Videó</span><span class="sxs-lookup"><span data-stu-id="8220a-227"><a name="video"></a> Video</span></span>


> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]



## <span data-ttu-id="8220a-228"><a name="next"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8220a-228"><a name="next"></a> Next steps</span></span>
* [<span data-ttu-id="8220a-229">Használat követése</span><span class="sxs-lookup"><span data-stu-id="8220a-229">Track usage</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="8220a-230">Egyéni események és a mérőszámok</span><span class="sxs-lookup"><span data-stu-id="8220a-230">Custom events and metrics</span></span>](app-insights-api-custom-events-metrics.md)
* [<span data-ttu-id="8220a-231">Összeállítás, mérés, tanulás</span><span class="sxs-lookup"><span data-stu-id="8220a-231">Build-measure-learn</span></span>](app-insights-web-track-usage.md)


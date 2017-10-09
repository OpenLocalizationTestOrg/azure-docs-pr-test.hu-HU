---
title: "az Azure web app teljesítmény aaaMonitor |} Microsoft Docs"
description: "Alkalmazásteljesítmény figyelése Azure-webappok esetében. Diagrambetöltési és -válaszidők függőségi információk és beállított, teljesítménnyel kapcsolatos riasztások."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: d1083254e5c504b18f2ac5ae2368610dc2790436
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-web-app-performance"></a><span data-ttu-id="9aff6-104">Azure-webapp teljesítményének figyelése</span><span class="sxs-lookup"><span data-stu-id="9aff6-104">Monitor Azure web app performance</span></span>
<span data-ttu-id="9aff6-105">A hello [Azure Portal](https://portal.azure.com) alkalmazásteljesítmény-figyelés állíthat be a [Azure-webalkalmazásokban](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9aff6-105">In hello [Azure Portal](https://portal.azure.com) you can set up application performance monitoring for your [Azure web apps](../app-service-web/app-service-web-overview.md).</span></span> <span data-ttu-id="9aff6-106">[Az Azure Application Insights](app-insights-overview.md) eszközök az alkalmazás toosend telemetriai adatainak a tevékenységek toohello Application Insights szolgáltatás, hol van tárolva, és elemzése.</span><span class="sxs-lookup"><span data-stu-id="9aff6-106">[Azure Application Insights](app-insights-overview.md) instruments your app toosend telemetry about its activities toohello Application Insights service, where it is stored and analyzed.</span></span> <span data-ttu-id="9aff6-107">Itt metrika diagramok és a keresési eszközök használható toohelp eseményadatokat, a teljesítmény javítása és használati értékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="9aff6-107">There, metric charts and search tools can be used toohelp diagnose issues, improve performance, and assess usage.</span></span>

## <a name="run-time-or-build-time"></a><span data-ttu-id="9aff6-108">Futási idő vagy felépítési idő</span><span class="sxs-lookup"><span data-stu-id="9aff6-108">Run time or build time</span></span>
<span data-ttu-id="9aff6-109">A figyelés tagolása hello alkalmazás két módon konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="9aff6-109">You can configure monitoring by instrumenting hello app in either of two ways:</span></span>

* <span data-ttu-id="9aff6-110">**Futási idő** – Kiválaszthat egy teljesítményfigyelési bővítményt, amikor a webapp már működik.</span><span class="sxs-lookup"><span data-stu-id="9aff6-110">**Run-time** - You can select a performance monitoring extension when your web app is already live.</span></span> <span data-ttu-id="9aff6-111">Nem szükséges toorebuild, vagy telepítse újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9aff6-111">It isn't necessary toorebuild or re-install your app.</span></span> <span data-ttu-id="9aff6-112">Olyan standard csomagkészletet kap, amely figyeli a válaszidőt, a sikerességi arányokat, a kivételeket, a függőségeket stb.</span><span class="sxs-lookup"><span data-stu-id="9aff6-112">You get a standard set of packages that monitor response times, success rates, exceptions, dependencies, and so on.</span></span> 
* <span data-ttu-id="9aff6-113">**Felépítési idő** – Telepíthet egy csomagot fejlesztés alatt álló alkalmazásába.</span><span class="sxs-lookup"><span data-stu-id="9aff6-113">**Build time** - You can install a package in your app in development.</span></span> <span data-ttu-id="9aff6-114">Ez a lehetőség jóval sokoldalúbb.</span><span class="sxs-lookup"><span data-stu-id="9aff6-114">This option is more versatile.</span></span> <span data-ttu-id="9aff6-115">A hozzáadása toohello azonos standard csomagok, kód toocustomize hello telemetriai vagy írhat toosend saját telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="9aff6-115">In addition toohello same standard packages, you can write code toocustomize hello telemetry or toosend your own telemetry.</span></span> <span data-ttu-id="9aff6-116">Meghatározott tevékenységek vagy rekord események alapján történik az alkalmazástartomány toohello szemantikáját bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="9aff6-116">You can log specific activities or record events according toohello semantics of your app domain.</span></span> 

## <a name="run-time-instrumentation-with-application-insights"></a><span data-ttu-id="9aff6-117">Futási idő kialakítása az Application Insights segítségével</span><span class="sxs-lookup"><span data-stu-id="9aff6-117">Run time instrumentation with Application Insights</span></span>
<span data-ttu-id="9aff6-118">Ha már futtat egy webappot az Azure-ban, a kéréseket és a hibák gyakoriságát a rendszer már figyeli.</span><span class="sxs-lookup"><span data-stu-id="9aff6-118">If you're already running a web app in Azure, you already get some monitoring: request and error rates.</span></span> <span data-ttu-id="9aff6-119">Az Application Insights tooget további, például a válaszidőt, figyelési hívások toodependencies, intelligens észlelési és hello hatékony Log Analytics lekérdezési nyelv hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="9aff6-119">Add Application Insights tooget more, such as response times, monitoring calls toodependencies, smart detection, and hello powerful Log Analytics query language.</span></span> 

1. <span data-ttu-id="9aff6-120">**Válassza ki az Application Insights** hello Azure Vezérlőpult a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9aff6-120">**Select Application Insights** in hello Azure control panel for your web app.</span></span>
   
    ![A Figyelés területen válassza az Application Insights lehetőséget](./media/app-insights-azure-web-apps/05-extend.png)
   
   * <span data-ttu-id="9aff6-122">Válasszon toocreate egy új erőforrást, kivéve, ha korábban már beállított egy Application Insights-erőforrást az alkalmazás egy másik útvonalanként.</span><span class="sxs-lookup"><span data-stu-id="9aff6-122">Choose toocreate a new resource, unless you already set up an Application Insights resource for this app by another route.</span></span>
2. <span data-ttu-id="9aff6-123">**Alakítsa ki webappját** az Application Insights telepítése után.</span><span class="sxs-lookup"><span data-stu-id="9aff6-123">**Instrument your web app** after Application Insights has been installed.</span></span> 
   
    ![Webapp kialakítása](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)

   <span data-ttu-id="9aff6-125">**Engedélyezze az ügyféloldali figyelést** a lapmegtekintések és a felhasználók telemetriai adatainak gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="9aff6-125">**Enable client side monitoring** for page view and user telemetry.</span></span>

   * <span data-ttu-id="9aff6-126">Válassza a Beállítások > Alkalmazásbeállítások lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9aff6-126">Select Settings > Application Settings</span></span>
   * <span data-ttu-id="9aff6-127">Az alkalmazásbeállításoknál adjon meg egy új kulcs-érték párt:</span><span class="sxs-lookup"><span data-stu-id="9aff6-127">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="9aff6-128">Kulcs: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="9aff6-128">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="9aff6-129">Érték: `true`</span><span class="sxs-lookup"><span data-stu-id="9aff6-129">Value: `true`</span></span>
   * <span data-ttu-id="9aff6-130">**Mentés** beállítások hello és **indítsa újra a** az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9aff6-130">**Save** hello settings and **Restart** your app.</span></span>
3. <span data-ttu-id="9aff6-131">**Figyelje alkalmazását**.</span><span class="sxs-lookup"><span data-stu-id="9aff6-131">**Monitor your app**.</span></span>  <span data-ttu-id="9aff6-132">[Expore hello adatok](#explore-the-data).</span><span class="sxs-lookup"><span data-stu-id="9aff6-132">[Expore hello data](#explore-the-data).</span></span>

<span data-ttu-id="9aff6-133">Később Ha azt szeretné, az Application insights szolgáltatással hello alkalmazást hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="9aff6-133">Later, you can build hello app with Application Insights if you want.</span></span>

<span data-ttu-id="9aff6-134">*Hogyan távolítsa el az Application Insights, vagy toosending tooanother erőforrást váltani?*</span><span class="sxs-lookup"><span data-stu-id="9aff6-134">*How do I remove Application Insights, or switch toosending tooanother resource?*</span></span>

* <span data-ttu-id="9aff6-135">Az Azure, nyissa meg hello webalkalmazás vezérlő panelen, és a fejlesztői eszközök, nyissa meg a **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="9aff6-135">In Azure, open hello web app control blade, and under Development Tools, open **Extensions**.</span></span> <span data-ttu-id="9aff6-136">Hello Application Insights-bővítmény törlése.</span><span class="sxs-lookup"><span data-stu-id="9aff6-136">Delete hello Application Insights extension.</span></span> <span data-ttu-id="9aff6-137">A figyelés, válassza ki az Application Insights és hozhat létre, vagy válassza ki a kívánt hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="9aff6-137">Then under Monitoring, choose Application Insights and create or select hello resource you want.</span></span>

## <a name="build-hello-app-with-application-insights"></a><span data-ttu-id="9aff6-138">Hozza létre hello alkalmazását az Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="9aff6-138">Build hello app with Application Insights</span></span>
<span data-ttu-id="9aff6-139">Ha telepít egy SDK-t alkalmazásába, az Application Insights részletesebb telemetriát biztosít.</span><span class="sxs-lookup"><span data-stu-id="9aff6-139">Application Insights can provide more detailed telemetry by installing an SDK into your app.</span></span> <span data-ttu-id="9aff6-140">Például gyűjthet nyomkövetési naplókat, [egyéni telemetriát írhat](app-insights-api-custom-events-metrics.md), és részletesebb, kivételjelentéseket kaphat.</span><span class="sxs-lookup"><span data-stu-id="9aff6-140">In particular, you can collect trace logs, [write custom telemetry](app-insights-api-custom-events-metrics.md), and get more detailed exception reports.</span></span>

1. <span data-ttu-id="9aff6-141">A **Visual Studióban** (2013 2. frissítés vagy újabb) konfigurálja az Application Insightst a projektjéhez.</span><span class="sxs-lookup"><span data-stu-id="9aff6-141">**In Visual Studio** (2013 update 2 or later), configure Application Insights for your project.</span></span>

    <span data-ttu-id="9aff6-142">Kattintson a jobb gombbal a hello webes projektet, és válassza ki **Hozzáadás > Application Insights** vagy **konfigurálja az Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="9aff6-142">Right-click hello web project, and select **Add > Application Insights** or **Configure Application Insights**.</span></span>
   
    ![Kattintson a jobb gombbal a hello webes projektet, és válassza ki a telepítése és konfigurálása az Application Insights](./media/app-insights-azure-web-apps/03-add.png)
   
    <span data-ttu-id="9aff6-144">Ha megkérdezi, hogy a toosign, hello hitelesítő adatait az Azure-fiókot használni.</span><span class="sxs-lookup"><span data-stu-id="9aff6-144">If you're asked toosign in, use hello credentials for your Azure account.</span></span>
   
    <span data-ttu-id="9aff6-145">hello művelet két hatásai vannak:</span><span class="sxs-lookup"><span data-stu-id="9aff6-145">hello operation has two effects:</span></span>
   
   1. <span data-ttu-id="9aff6-146">Létrehoz az Azure-ban egy Application Insights-erőforrást, ahol tárolja, elemzi és megjeleníti a telemetriát.</span><span class="sxs-lookup"><span data-stu-id="9aff6-146">Creates an Application Insights resource in Azure, where telemetry is stored, analyzed and displayed.</span></span>
   2. <span data-ttu-id="9aff6-147">Hozzáadja a hello Application Insights NuGet csomag tooyour kódja (Ha még nem létezik), és beállítja toosend telemetriai toohello Azure-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="9aff6-147">Adds hello Application Insights NuGet package tooyour code (if it isn't there already), and configures it toosend telemetry toohello Azure resource.</span></span>
2. <span data-ttu-id="9aff6-148">**Hello telemetriai tesztelése** futó hello alkalmazás a fejlesztési számítógépen (F5).</span><span class="sxs-lookup"><span data-stu-id="9aff6-148">**Test hello telemetry** by running hello app in your development machine (F5).</span></span>
3. <span data-ttu-id="9aff6-149">**Hello alkalmazás közzététele** tooAzure hello a szokásos módon.</span><span class="sxs-lookup"><span data-stu-id="9aff6-149">**Publish hello app** tooAzure in hello usual way.</span></span> 

<span data-ttu-id="9aff6-150">*Hogyan toosending tooa különböző Application Insights-erőforrást váltani?*</span><span class="sxs-lookup"><span data-stu-id="9aff6-150">*How do I switch toosending tooa different Application Insights resource?*</span></span>

* <span data-ttu-id="9aff6-151">A Visual Studióban, kattintson a jobb gombbal a projekt hello, válassza ki a **konfigurálja az Application Insights** , és válassza ki a kívánt hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="9aff6-151">In Visual Studio, right-click hello project, choose **Configure Application Insights** and choose hello resource you want.</span></span> <span data-ttu-id="9aff6-152">Hello beállítás toocreate egy új erőforrást beolvasni.</span><span class="sxs-lookup"><span data-stu-id="9aff6-152">You get hello option toocreate a new resource.</span></span> <span data-ttu-id="9aff6-153">Építse újra, és ismételten helyezze üzembe.</span><span class="sxs-lookup"><span data-stu-id="9aff6-153">Rebuild and redeploy.</span></span>

## <a name="explore-hello-data"></a><span data-ttu-id="9aff6-154">Hello adatokba</span><span class="sxs-lookup"><span data-stu-id="9aff6-154">Explore hello data</span></span>
1. <span data-ttu-id="9aff6-155">A webes alkalmazás Vezérlőpult hello Application Insights paneljén láthatja élő metrika, amely kérések és hibák megjelenik egy második vagy kettő lépett fel.</span><span class="sxs-lookup"><span data-stu-id="9aff6-155">On hello Application Insights blade of your web app control panel, you see Live Metrics, which shows requests and failures within a second or two of them occurring.</span></span> <span data-ttu-id="9aff6-156">Hasznos funkció, ha ismételten teszi közzé alkalmazását, mert azonnal látható minden probléma.</span><span class="sxs-lookup"><span data-stu-id="9aff6-156">It's very useful display when you're republishing your app - you can see any problems immediately.</span></span>
2. <span data-ttu-id="9aff6-157">Kattintson a toohello teljes Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="9aff6-157">Click through toohello full Application Insights resource.</span></span>

    ![Végiglépkedés kattintással](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    <span data-ttu-id="9aff6-159">Az Azure-erőforrásnavigációból közvetlenül is odaléphet.</span><span class="sxs-lookup"><span data-stu-id="9aff6-159">You can also go there either directly from Azure resource navigation.</span></span>

1. <span data-ttu-id="9aff6-160">A diagram tooget kattintson a további részletek:</span><span class="sxs-lookup"><span data-stu-id="9aff6-160">Click through any chart tooget more detail:</span></span>
   
    ![Hello Application Insights – áttekintés paneljén kattintson a diagram](./media/app-insights-azure-web-apps/07-dependency.png)
   
    <span data-ttu-id="9aff6-162">[Testreszabhatja a mérőszámpaneleket](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="9aff6-162">You can [customize metrics blades](app-insights-metrics-explorer.md).</span></span>
2. <span data-ttu-id="9aff6-163">Kattintson a további toosee események és azok tulajdonságait:</span><span class="sxs-lookup"><span data-stu-id="9aff6-163">Click through further toosee individual events and their properties:</span></span>
   
    ![Kattintson egy esemény típusa tooopen keresés típushoz szűrve](./media/app-insights-azure-web-apps/08-requests.png)
   
    <span data-ttu-id="9aff6-165">Figyelje meg, hogy az összes tulajdonság hivatkozás tooopen "..." hello.</span><span class="sxs-lookup"><span data-stu-id="9aff6-165">Notice hello "..." link tooopen all properties.</span></span>
   
    <span data-ttu-id="9aff6-166">[Testreszabhatja a kereséseket](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="9aff6-166">You can [customize searches](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="9aff6-167">A telemetriai adatok keresztüli hatékonyabb keresések használja hello [Log Analytics lekérdezési nyelv](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="9aff6-167">For more powerful searches over your telemetry, use hello [Log Analytics query language](app-insights-analytics-tour.md).</span></span>

## <a name="more-telemetry"></a><span data-ttu-id="9aff6-168">További telemetria</span><span class="sxs-lookup"><span data-stu-id="9aff6-168">More telemetry</span></span>

* [<span data-ttu-id="9aff6-169">Weblapbetöltési adatok</span><span class="sxs-lookup"><span data-stu-id="9aff6-169">Web page load data</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="9aff6-170">Egyéni telemetria</span><span class="sxs-lookup"><span data-stu-id="9aff6-170">Custom telemetry</span></span>](app-insights-api-custom-events-metrics.md)

## <a name="video"></a><span data-ttu-id="9aff6-171">Videó</span><span class="sxs-lookup"><span data-stu-id="9aff6-171">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="9aff6-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9aff6-172">Next steps</span></span>
* <span data-ttu-id="9aff6-173">[Futtassa az élő app hello Profilkészítő](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="9aff6-173">[Run hello profiler on your live app](app-insights-profiler.md).</span></span>
* <span data-ttu-id="9aff6-174">[Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) – az Azure Functions figyelése az Application Insights segítségével</span><span class="sxs-lookup"><span data-stu-id="9aff6-174">[Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) - monitor Azure Functions with Application Insights</span></span>
* <span data-ttu-id="9aff6-175">[Engedélyezze az Azure diagnostics](app-insights-azure-diagnostics.md) küldött toobe tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="9aff6-175">[Enable Azure diagnostics](app-insights-azure-diagnostics.md) toobe sent tooApplication Insights.</span></span>
* <span data-ttu-id="9aff6-176">[Szolgáltatás állapotának metrikát](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.</span><span class="sxs-lookup"><span data-stu-id="9aff6-176">[Monitor service health metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
* <span data-ttu-id="9aff6-177">[Riasztási értesítéseket kaphat](../monitoring-and-diagnostics/insights-receive-alert-notifications.md), ha működési események történnek vagy a mérőszámok átlépnek egy küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="9aff6-177">[Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) whenever operational events happen or metrics cross a threshold.</span></span>
* <span data-ttu-id="9aff6-178">Használjon [Application Insights JavaScript alkalmazásokkal és weblapok](app-insights-javascript.md) tooget ügyfél telemetriai adat, amely egy weblap hello böngészőkkel történő.</span><span class="sxs-lookup"><span data-stu-id="9aff6-178">Use [Application Insights for JavaScript apps and web pages](app-insights-javascript.md) tooget client telemetry from hello browsers that visit a web page.</span></span>
* <span data-ttu-id="9aff6-179">[Állítsa be a rendelkezésre állási webteszt](app-insights-monitor-web-app-availability.md) toobe riasztást kapni, ha a webhely nem működik.</span><span class="sxs-lookup"><span data-stu-id="9aff6-179">[Set up Availability web tests](app-insights-monitor-web-app-availability.md) toobe alerted if your site is down.</span></span>


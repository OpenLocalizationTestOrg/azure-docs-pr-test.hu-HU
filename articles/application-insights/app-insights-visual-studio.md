---
title: "az Azure Application insights szolgáltatással, a Visual Studio aaaDebug alkalmazások |} Microsoft Docs"
description: "Webalkalmazások teljesítményelemzése és diagnosztikája hibakeresés közben és éles környezetben."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/7/2017
ms.author: bwren
ms.openlocfilehash: 20491fbe4505bf719039e5d1c220b1afec01db25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-applications-with-azure-application-insights-in-visual-studio"></a><span data-ttu-id="e701c-103">Az alkalmazások az Azure Application insights szolgáltatással, a Visual Studio hibakeresési</span><span class="sxs-lookup"><span data-stu-id="e701c-103">Debug your applications with Azure Application Insights in Visual Studio</span></span>
<span data-ttu-id="e701c-104">A Visual Studio 2015-ös és újabb verzióiban elemezheti az ASP.NET-webalkalmazások teljesítményét és diagnosztizálhatja a problémákat hibakeresés közben és éles környezetben is az [Azure Application Insights](app-insights-overview.md) telemetriájával.</span><span class="sxs-lookup"><span data-stu-id="e701c-104">In Visual Studio (2015 and later), you can analyze performance and diagnose issues in your ASP.NET web app both in debugging and in production, using telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="e701c-105">Ha az ASP.NET-webalkalmazás Visual Studio 2017 használatával létrehozott, vagy később, hello Application Insights SDK már rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e701c-105">If you created your ASP.NET web app using Visual Studio 2017 or later, it already has hello Application Insights SDK.</span></span> <span data-ttu-id="e701c-106">Egyéb módon, hogy még nem tette meg, ha [Application Insights tooyour alkalmazás hozzáadása](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="e701c-106">Otherwise, if you haven't done so already, [add Application Insights tooyour app](app-insights-asp-net.md).</span></span>

<span data-ttu-id="e701c-107">toomonitor az alkalmazás éles van, amikor általában megjelenített hello Application Insights telemetria hello [Azure-portálon](https://portal.azure.com), ahol riasztások beállítását, és erőteljes figyelőeszközökből alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="e701c-107">toomonitor your app when it's in live production, you normally view hello Application Insights telemetry in hello [Azure portal](https://portal.azure.com), where you can set alerts and apply powerful monitoring tools.</span></span> <span data-ttu-id="e701c-108">De hibakeresési információ is kereshet és elemezheti a Visual Studio hello telemetriai.</span><span class="sxs-lookup"><span data-stu-id="e701c-108">But for debugging, you can also search and analyze hello telemetry in Visual Studio.</span></span> <span data-ttu-id="e701c-109">Visual Studio tooanalyze telemetriai használhatja a munkakörnyezeti helyet a pedig a hibakeresés fut a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="e701c-109">You can use Visual Studio tooanalyze telemetry both from your production site and from debugging runs on your development machine.</span></span> <span data-ttu-id="e701c-110">Hello utóbbi esetben elemezheti hibakeresési fut, akkor is, ha még hello SDK toosend telemetriai toohello Azure-portálon még nincs konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e701c-110">In hello latter case, you can analyze debugging runs even if you haven't yet configured hello SDK toosend telemetry toohello Azure portal.</span></span> 

## <span data-ttu-id="e701c-111"><a name="run"></a> Hibakeresés a projektben</span><span class="sxs-lookup"><span data-stu-id="e701c-111"><a name="run"></a> Debug your project</span></span>
<span data-ttu-id="e701c-112">Futtassa a webalkalmazást helyi hibakeresési módban az F5 billentyűvel.</span><span class="sxs-lookup"><span data-stu-id="e701c-112">Run your web app in local debug mode by using F5.</span></span> <span data-ttu-id="e701c-113">Nyissa meg a különböző oldalakhoz toogenerate néhány telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="e701c-113">Open different pages toogenerate some telemetry.</span></span>

<span data-ttu-id="e701c-114">A Visual Studióban tekintse meg a projekt hello Application Insights modul által naplózott eseményeket hello számát.</span><span class="sxs-lookup"><span data-stu-id="e701c-114">In Visual Studio, you see a count of hello events that have been logged by hello Application Insights module in your project.</span></span>

![A Visual Studio hello Application Insights gombon hibakeresés során.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

<span data-ttu-id="e701c-116">Kattintson a gomb toosearch a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="e701c-116">Click this button toosearch your telemetry.</span></span> 

## <a name="application-insights-search"></a><span data-ttu-id="e701c-117">Application Insights-keresés</span><span class="sxs-lookup"><span data-stu-id="e701c-117">Application Insights search</span></span>
<span data-ttu-id="e701c-118">hello Application Insights keresési ablak naplózott eseményeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e701c-118">hello Application Insights Search window shows events that have been logged.</span></span> <span data-ttu-id="e701c-119">(Ha az Application Insights beállításakor bejelentkezett tooAzure, kereshet hello Azure-portálon azonos események hello.)</span><span class="sxs-lookup"><span data-stu-id="e701c-119">(If you signed in tooAzure when you set up Application Insights, you can search hello same events in hello Azure portal.)</span></span>

![Kattintson a jobb gombbal a projekt hello, és válassza ki az Application Insights keresése](./media/app-insights-visual-studio/34.png)

> [!NOTE] 
> <span data-ttu-id="e701c-121">Válassza ki, vagy kapcsolja ki a szűrők után gombra hello keresési hello keresési szövegmező hello végén.</span><span class="sxs-lookup"><span data-stu-id="e701c-121">After you select or deselect filters, click hello Search button at hello end of hello text search field.</span></span>
>

<span data-ttu-id="e701c-122">hello szabad szöveges keresés hello események mezőinek működik.</span><span class="sxs-lookup"><span data-stu-id="e701c-122">hello free text search works on any fields in hello events.</span></span> <span data-ttu-id="e701c-123">Például keressen rá az oldal; hello URL-cím része például az ügyfél város; tulajdonság értéke hello vagy vagy a nyomkövetési napló szavakat.</span><span class="sxs-lookup"><span data-stu-id="e701c-123">For example, search for part of hello URL of a page; or hello value of a property such as client city; or specific words in a trace log.</span></span>

<span data-ttu-id="e701c-124">Kattintson a bármely esemény toosee részletes tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="e701c-124">Click any event toosee its detailed properties.</span></span>

<span data-ttu-id="e701c-125">Kérelmek tooyour web app alkalmazásban kattintva toohello kód.</span><span class="sxs-lookup"><span data-stu-id="e701c-125">For requests tooyour web app, you can click through toohello code.</span></span>

![A kérelem részleteinek kattintson toohello kód](./media/app-insights-visual-studio/31.png)

<span data-ttu-id="e701c-127">Kapcsolódó elemek is megnyithatja toohelp diagnosztizálhatja a sikertelen kérelmek vagy kivételeket.</span><span class="sxs-lookup"><span data-stu-id="e701c-127">You can also open related items toohelp diagnose failed requests or exceptions.</span></span>

![A kérelem részleteinek görgessen lefelé toorelated elemek](./media/app-insights-visual-studio/41.png)

## <a name="view-exceptions-and-failed-requests"></a><span data-ttu-id="e701c-129">Kivételek megtekintése és a sikertelen kérelmek</span><span class="sxs-lookup"><span data-stu-id="e701c-129">View exceptions and failed requests</span></span>
<span data-ttu-id="e701c-130">Jelentések megjelenítése kivétel hello keresési ablak.</span><span class="sxs-lookup"><span data-stu-id="e701c-130">Exception reports show in hello Search window.</span></span> <span data-ttu-id="e701c-131">(Egyes régebbi típusú ASP.NET-alkalmazást, hogy túl[kivétel figyelés beállítása](app-insights-asp-net-exceptions.md) hello keretrendszer által kezelt kivételek toosee.)</span><span class="sxs-lookup"><span data-stu-id="e701c-131">(In some older types of ASP.NET application, you have too[set up exception monitoring](app-insights-asp-net-exceptions.md) toosee exceptions that are handled by hello framework.)</span></span>

<span data-ttu-id="e701c-132">Kattintson egy kivétel tooget hívásláncnak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="e701c-132">Click an exception tooget a stack trace.</span></span> <span data-ttu-id="e701c-133">Ha hello kód hello alkalmazás meg nyitva, a Visual Studio, kattinthat keresztül a hello verem nyomkövetési toohello vonatkozó kódsort hello.</span><span class="sxs-lookup"><span data-stu-id="e701c-133">If hello code of hello app is open in Visual Studio, you can click through from hello stack trace toohello relevant line of hello code.</span></span>

![Kivétel híváslánca](./media/app-insights-visual-studio/17.png)

## <a name="view-request-and-exception-summaries-in-hello-code"></a><span data-ttu-id="e701c-135">Hello kódban kérelem és a kivétel összesítéseket</span><span class="sxs-lookup"><span data-stu-id="e701c-135">View request and exception summaries in hello code</span></span>
<span data-ttu-id="e701c-136">A kód fókuszban sorban fölött mindegyik eseménykezelő metódus hello lásd: hello kérelmek és az Application Insights hello elmúlt 24 órás naplózásáról kivételek számát.</span><span class="sxs-lookup"><span data-stu-id="e701c-136">In hello Code Lens line above each handler method, you see a count of hello requests and exceptions logged by Application Insights in hello past 24 h.</span></span>

![Kivétel híváslánca](./media/app-insights-visual-studio/21.png)

> [!NOTE] 
> <span data-ttu-id="e701c-138">Kód fókuszban adatokat jelenít meg az Application Insights csak ha [konfigurálva az alkalmazás toosend telemetriai toohello Application Insights portál](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="e701c-138">Code Lens shows Application Insights data only if you have [configured your app toosend telemetry toohello Application Insights portal](app-insights-asp-net.md).</span></span>
>

[<span data-ttu-id="e701c-139">További információk az Application Insights-telemetriáról a Code Lensben</span><span class="sxs-lookup"><span data-stu-id="e701c-139">More about Application Insights in Code Lens</span></span>](app-insights-visual-studio-codelens.md)

## <a name="trends"></a><span data-ttu-id="e701c-140">Trendek</span><span class="sxs-lookup"><span data-stu-id="e701c-140">Trends</span></span>
<span data-ttu-id="e701c-141">A Trendek használatával megjelenítheti az alkalmazás időbeni működésének a módját.</span><span class="sxs-lookup"><span data-stu-id="e701c-141">Trends is a tool for visualizing how your app behaves over time.</span></span> 

<span data-ttu-id="e701c-142">Válasszon **felfedezés Telemetriai trendek** hello az Application Insights eszköztárgomb vagy az Application Insights keresési ablak.</span><span class="sxs-lookup"><span data-stu-id="e701c-142">Choose **Explore Telemetry Trends** from hello Application Insights toolbar button or Application Insights Search window.</span></span> <span data-ttu-id="e701c-143">Válasszon egyet a öt közös lekérdezések tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="e701c-143">Choose one of five common queries tooget started.</span></span> <span data-ttu-id="e701c-144">A különböző adatkészleteket telemetriatípusok, időintervallumok és egyéb tulajdonságok szerint elemezheti.</span><span class="sxs-lookup"><span data-stu-id="e701c-144">You can analyze different datasets based on telemetry types, time ranges, and other properties.</span></span> 

<span data-ttu-id="e701c-145">az adatok toofind rendellenességeinek válasszon egyet az hello anomáliadetektálási beállítások hello "Nézet típusa" legördülő lista alatt.</span><span class="sxs-lookup"><span data-stu-id="e701c-145">toofind anomalies in your data, choose one of hello anomaly options under hello "View Type" dropdown.</span></span> <span data-ttu-id="e701c-146">hello szűrési beállítások hello ablak hello alján legyen a könnyen toohone a meghatározott fájlcsoportokat a telemetriai adatot.</span><span class="sxs-lookup"><span data-stu-id="e701c-146">hello filtering options at hello bottom of hello window make it easy toohone in on specific subsets of your telemetry.</span></span>

![Trendek](./media/app-insights-visual-studio/51.png)

<span data-ttu-id="e701c-148">[További információ a Trendekről](app-insights-visual-studio-trends.md).</span><span class="sxs-lookup"><span data-stu-id="e701c-148">[More about Trends](app-insights-visual-studio-trends.md).</span></span>

## <a name="local-monitoring"></a><span data-ttu-id="e701c-149">Helyi figyelés</span><span class="sxs-lookup"><span data-stu-id="e701c-149">Local monitoring</span></span>
<span data-ttu-id="e701c-150">(A Visual Studio 2015 Update 2) Ha még nincs konfigurálva hello SDK toosend telemetriai toohello Application Insights portál (úgy, hogy nincs instrumentation kulcs applicationinsights.config) hello diagnosztika ablak a legújabb hibakeresési munkamenetben telemetriai adatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e701c-150">(From Visual Studio 2015 Update 2) If you haven't configured hello SDK toosend telemetry toohello Application Insights portal (so that there is no instrumentation key in ApplicationInsights.config) then hello diagnostics window displays telemetry from your latest debugging session.</span></span> 

<span data-ttu-id="e701c-151">Ez akkor javasolt, ha már közzétette az alkalmazás korábbi verzióit.</span><span class="sxs-lookup"><span data-stu-id="e701c-151">This is desirable if you have already published a previous version of your app.</span></span> <span data-ttu-id="e701c-152">Nem szeretné, hogy a hibakeresési munkamenetek toobe a hello telemetriai keverve hello hello telemetriai adatokat az Application Insights portál hello közzétett alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e701c-152">You don't want hello telemetry from your debugging sessions toobe mixed up with hello telemetry on hello Application Insights portal from hello published app.</span></span>

<span data-ttu-id="e701c-153">Ez akkor is hasznos, ha, hogy bizonyos [egyéni telemetria](app-insights-api-custom-events-metrics.md) megjeleníteni kívánt toodebug telemetriai toohello portal elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="e701c-153">It's also useful if you have some [custom telemetry](app-insights-api-custom-events-metrics.md) that you want toodebug before sending telemetry toohello portal.</span></span>

* <span data-ttu-id="e701c-154">*Először az Application Insights toosend telemetriai toohello portal teljesen beállítása. De most csak a Visual Studio toosee hello telemetriai adatokat.*</span><span class="sxs-lookup"><span data-stu-id="e701c-154">*At first, I fully configured Application Insights toosend telemetry toohello portal. But now I'd like toosee hello telemetry only in Visual Studio.*</span></span>
  
  * <span data-ttu-id="e701c-155">Hello keresési ablak beállításaiban van egy beállítás toosearch helyi diagnosztika akkor is, ha az alkalmazás küld telemetriai toohello portal.</span><span class="sxs-lookup"><span data-stu-id="e701c-155">In hello Search window's Settings, there's an option toosearch local diagnostics even if your app sends telemetry toohello portal.</span></span>
  * <span data-ttu-id="e701c-156">küldési toohello portal, hello sor megjegyzésbe toostop telemetriai `<instrumentationkey>...` az ApplicationInsights.config. Ha készen áll a toosend telemetriai toohello portal újra már kódjánál levő megjegyzésjeleket.</span><span class="sxs-lookup"><span data-stu-id="e701c-156">toostop telemetry being sent toohello portal, comment out hello line `<instrumentationkey>...` from ApplicationInsights.config. When you're ready toosend telemetry toohello portal again, uncomment it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e701c-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e701c-157">Next steps</span></span>
|  |  |
| --- | --- |
| <span data-ttu-id="e701c-158">**[További adatok hozzáadása](app-insights-asp-net-more.md)**</span><span class="sxs-lookup"><span data-stu-id="e701c-158">**[Add more data](app-insights-asp-net-more.md)**</span></span><br/><span data-ttu-id="e701c-159">Figyelheti a használatot, az elérhetőséget, a függőségeket és a kivételeket.</span><span class="sxs-lookup"><span data-stu-id="e701c-159">Monitor usage, availability, dependencies, exceptions.</span></span> <span data-ttu-id="e701c-160">Integrálhatja a nyomkövetéseket naplózási keretrendszerekből.</span><span class="sxs-lookup"><span data-stu-id="e701c-160">Integrate traces from logging frameworks.</span></span> <span data-ttu-id="e701c-161">Egyéni telemetriát írhat.</span><span class="sxs-lookup"><span data-stu-id="e701c-161">Write custom telemetry.</span></span> |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| <span data-ttu-id="e701c-163">**[Hello Application Insights portál használata](app-insights-dashboards.md)**</span><span class="sxs-lookup"><span data-stu-id="e701c-163">**[Working with hello Application Insights portal](app-insights-dashboards.md)**</span></span><br/><span data-ttu-id="e701c-164">Irányítópultok, hatékony elemzési és diagnosztikai eszközöket, riasztások, egy élő kapcsolatfüggőségi térképének megjelenítéséhez az alkalmazás, és exportált telemetriai adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e701c-164">View dashboards, powerful diagnostic and analytic tools, alerts, a live dependency map of your application, and exported telemetry data.</span></span> |![Visual Studio](./media/app-insights-visual-studio/62.png) |


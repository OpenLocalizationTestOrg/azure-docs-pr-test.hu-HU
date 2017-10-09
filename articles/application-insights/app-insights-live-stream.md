---
title: "egyéni metrikákkal és diagnosztika Azure Application Insights a metrikák adatfolyam aaaLive |} Microsoft Docs"
description: "A webalkalmazás egyéni metrikák valós idejű figyelése és a hibák, a nyomkövetések és az események élő adatcsatornára eseményadatokat."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: bwren
ms.openlocfilehash: 68ddfbf387379ea778c20280c4ec96baa06d3273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a><span data-ttu-id="f7e8a-103">Metrikák adatfolyamot: Figyelő & Diagnosztizálás és 1 másodperc késleltetés</span><span class="sxs-lookup"><span data-stu-id="f7e8a-103">Live Metrics Stream: Monitor & Diagnose with 1-second latency</span></span> 

<span data-ttu-id="f7e8a-104">Az élő, éles a webes alkalmazás hello ki szívveréseket szív mintavételi származó élő metrikák adatfolyam használatával [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f7e8a-104">Probe hello beating heart of your live, in-production web application by using Live Metrics Stream from [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="f7e8a-105">Válassza ki, és szűrheti a metrikák és a teljesítmény számlálók toowatch valós idejű semmilyen zavart tooyour szolgáltatás nélkül.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-105">Select and filter metrics and performance counters toowatch in real time, without any disturbance tooyour service.</span></span> <span data-ttu-id="f7e8a-106">Vizsgálja meg a minta sikertelen kérelmek és kivételek híváslánc megjelenik.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-106">Inspect stack traces from sample failed requests and exceptions.</span></span> <span data-ttu-id="f7e8a-107">Együtt [Profilkészítő](app-insights-profiler.md), [pillanatkép hibakereső](app-insights-snapshot-debugger.md), és [Teljesítménytesztelés](app-insights-monitor-web-app-availability.md#performance-tests), metrikák adatfolyamot egy hatékony és nem zavarja a munkában diagnosztikai eszköz biztosít a élő webhelyet.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-107">Together with [Profiler](app-insights-profiler.md), [Snapshot debugger](app-insights-snapshot-debugger.md), and [performance testing](app-insights-monitor-web-app-availability.md#performance-tests),  Live Metrics Stream provides a powerful and non-invasive diagnostic tool for your live web site.</span></span>

<span data-ttu-id="f7e8a-108">Az élő metrikák adatfolyam-továbbítás segítségével:</span><span class="sxs-lookup"><span data-stu-id="f7e8a-108">With Live Metrics Stream, you can:</span></span>

* <span data-ttu-id="f7e8a-109">Egy javítást érvényesítése közben megjelent, amelyet figyeli a teljesítmény és a hiba számát.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-109">Validate a fix while it is released, by watching performance and failure counts.</span></span>
* <span data-ttu-id="f7e8a-110">Hello hatásának tesztelése terhelések, és a problémák diagnosztizálásához tekintse meg élő.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-110">Watch hello effect of test loads, and diagnose issues live.</span></span> 
* <span data-ttu-id="f7e8a-111">Adott teszt munkamenetek összpontosítson, vagy kiválasztásával, és azt szeretné, hogy toowatch hello metrikák szűrés szűrheti ismert problémákkal kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-111">Focus on particular test sessions or filter out known issues, by selecting and filtering hello metrics you want toowatch.</span></span>
* <span data-ttu-id="f7e8a-112">Első kivétel nyomkövetési adatokat, akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-112">Get exception traces as they happen.</span></span>
* <span data-ttu-id="f7e8a-113">A szűrők kísérlet toofind hello legfontosabb KPI-k.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-113">Experiment with filters toofind hello most relevant KPIs.</span></span>
* <span data-ttu-id="f7e8a-114">Bármely Windows-teljesítmény számláló élő figyelése.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-114">Monitor any Windows performance counter live.</span></span>
* <span data-ttu-id="f7e8a-115">Egy kiszolgálót, amelynek problémák azonosítását, és minden hello, KPI-t vagy élő adatcsatorna toojust szűrheti, hogy a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-115">Easily identify a server that is having issues, and filter all hello KPI/live feed toojust that server.</span></span>

<span data-ttu-id="f7e8a-116">[![Élő adatfolyam metrikák videó](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)</span><span class="sxs-lookup"><span data-stu-id="f7e8a-116">[![Live Metrics Stream video](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)</span></span>

<span data-ttu-id="f7e8a-117">Metrikák élő adatfolyam érhető el jelenleg a helyszínen futtatott ASP.NET-alkalmazásokra vagy hello felhő.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-117">Live Metrics Stream is currently available on ASP.NET apps running on-premises or in hello Cloud.</span></span> 

## <a name="get-started"></a><span data-ttu-id="f7e8a-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="f7e8a-118">Get started</span></span>

1. <span data-ttu-id="f7e8a-119">Ha még nem [telepítve az Application Insights](app-insights-asp-net.md) az ASP.NET web app alkalmazásban vagy [Windows server alkalmazás](app-insights-windows-services.md), most tegye.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-119">If you haven't yet [installed Application Insights](app-insights-asp-net.md) in your ASP.NET web app or [Windows server app](app-insights-windows-services.md), do that now.</span></span> 
2. <span data-ttu-id="f7e8a-120">**Frissítés toohello legújabb verziója** hello Application Insights csomag.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-120">**Update toohello latest version** of hello Application Insights package.</span></span> <span data-ttu-id="f7e8a-121">A Visual Studióban, kattintson jobb gombbal a projektre, és válassza a **kezelése Nuget-csomagok**.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-121">In Visual Studio, right-click your project and choose **Manage Nuget packages**.</span></span> <span data-ttu-id="f7e8a-122">Megnyitás hello **frissítések** lapon jelölje **közé tartoznak az előzetes**, és válassza ki az összes hello Microsoft.ApplicationInsights.* csomagokat.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-122">Open hello **Updates** tab, check **Include prerelease**, and select all hello Microsoft.ApplicationInsights.* packages.</span></span>

    <span data-ttu-id="f7e8a-123">Helyezze ismét üzembe alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-123">Redeploy your app.</span></span>

3. <span data-ttu-id="f7e8a-124">A hello [Azure-portálon](https://portal.azure.com), nyissa meg a hello Application Insights-erőforrást az alkalmazáshoz, és nyissa meg az élő adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-124">In hello [Azure portal](https://portal.azure.com), open hello Application Insights resource for your app, and then open Live Stream.</span></span>

4. <span data-ttu-id="f7e8a-125">[Biztonságos hello vezérlőcsatorna](#secure-the-control-channel) ha bizalmas adatok, például ügyfélnevek használhatja a szűrők.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-125">[Secure hello control channel](#secure-the-control-channel) if you might use sensitive data such as customer names in your filters.</span></span>


![Hello áttekintése paneljén kattintson az élő adatfolyam](./media/app-insights-live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a><span data-ttu-id="f7e8a-127">Nincs adat?</span><span class="sxs-lookup"><span data-stu-id="f7e8a-127">No data?</span></span> <span data-ttu-id="f7e8a-128">Ellenőrizze a kiszolgáló tűzfal</span><span class="sxs-lookup"><span data-stu-id="f7e8a-128">Check your server firewall</span></span>

<span data-ttu-id="f7e8a-129">Ellenőrizze a hello [kimenő portok a metrikák élő adatfolyam](app-insights-ip-addresses.md#outgoing-ports) megnyitott hello tűzfal-kiszolgálókhoz.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-129">Check hello [outgoing ports for Live Metrics Stream](app-insights-ip-addresses.md#outgoing-ports) are open in hello firewall of your servers.</span></span> 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a><span data-ttu-id="f7e8a-130">Eltérések a Metrikaböngésző és az elemzések adatfolyamot metrikák?</span><span class="sxs-lookup"><span data-stu-id="f7e8a-130">How does Live Metrics Stream differ from Metrics Explorer and Analytics?</span></span>

| |<span data-ttu-id="f7e8a-131">Élő stream</span><span class="sxs-lookup"><span data-stu-id="f7e8a-131">Live Stream</span></span> | <span data-ttu-id="f7e8a-132">Metrikaböngésző és elemzés</span><span class="sxs-lookup"><span data-stu-id="f7e8a-132">Metrics Explorer and Analytics</span></span> |
|---|---|---|
|<span data-ttu-id="f7e8a-133">Késés</span><span class="sxs-lookup"><span data-stu-id="f7e8a-133">Latency</span></span>|<span data-ttu-id="f7e8a-134">Egy második belül megjelenített adatok</span><span class="sxs-lookup"><span data-stu-id="f7e8a-134">Data displayed within one second</span></span>|<span data-ttu-id="f7e8a-135">Perc alatt összesített értéket</span><span class="sxs-lookup"><span data-stu-id="f7e8a-135">Aggregated over minutes</span></span>|
|<span data-ttu-id="f7e8a-136">Visszatartás nem</span><span class="sxs-lookup"><span data-stu-id="f7e8a-136">No retention</span></span>|<span data-ttu-id="f7e8a-137">Adatok továbbra is fennáll, amíg ez a hello diagramra, és utána eldobja</span><span class="sxs-lookup"><span data-stu-id="f7e8a-137">Data persists while it's on hello chart, and is then discarded</span></span>|[<span data-ttu-id="f7e8a-138">Az adatok 90 napig őrzi meg</span><span class="sxs-lookup"><span data-stu-id="f7e8a-138">Data retained for 90 days</span></span>](app-insights-data-retention-privacy.md#how-long-is-the-data-kept)|
|<span data-ttu-id="f7e8a-139">Az igény szerinti</span><span class="sxs-lookup"><span data-stu-id="f7e8a-139">On demand</span></span>|<span data-ttu-id="f7e8a-140">Amíg nyissa meg a metrikák élő adatok továbbítja adatfolyamként</span><span class="sxs-lookup"><span data-stu-id="f7e8a-140">Data is streamed while you open Live Metrics</span></span>|<span data-ttu-id="f7e8a-141">Az adatokat küldi el, amikor hello SDK telepítése és engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f7e8a-141">Data is sent whenever hello SDK is installed and enabled</span></span>|
|<span data-ttu-id="f7e8a-142">Ingyenes</span><span class="sxs-lookup"><span data-stu-id="f7e8a-142">Free</span></span>|<span data-ttu-id="f7e8a-143">Az élő adatfolyam adatok ingyenesek</span><span class="sxs-lookup"><span data-stu-id="f7e8a-143">There is no charge for Live Stream data</span></span>|<span data-ttu-id="f7e8a-144">Tulajdonos túl[díjszabása](app-insights-pricing.md)</span><span class="sxs-lookup"><span data-stu-id="f7e8a-144">Subject too[pricing](app-insights-pricing.md)</span></span>
|<span data-ttu-id="f7e8a-145">Mintavételezés</span><span class="sxs-lookup"><span data-stu-id="f7e8a-145">Sampling</span></span>|<span data-ttu-id="f7e8a-146">Az összes kijelölt metrikák és számlálók továbbít.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-146">All selected metrics and counters are transmitted.</span></span> <span data-ttu-id="f7e8a-147">Hibák és híváslánc megjelenik mintát.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-147">Failures and stack traces are sampled.</span></span> <span data-ttu-id="f7e8a-148">TelemetryProcessors nem érvényesek.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-148">TelemetryProcessors are not applied.</span></span>|<span data-ttu-id="f7e8a-149">Lehet, hogy események [mintát](app-insights-api-filtering-sampling.md)</span><span class="sxs-lookup"><span data-stu-id="f7e8a-149">Events may be [sampled](app-insights-api-filtering-sampling.md)</span></span>|
|<span data-ttu-id="f7e8a-150">Vezérlőcsatorna</span><span class="sxs-lookup"><span data-stu-id="f7e8a-150">Control channel</span></span>|<span data-ttu-id="f7e8a-151">Szűrő vezérlő jelek toohello SDK küldése.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-151">Filter control signals are sent toohello SDK.</span></span> <span data-ttu-id="f7e8a-152">Javasoljuk, hogy [a csatornát](#secure-channel).</span><span class="sxs-lookup"><span data-stu-id="f7e8a-152">We recommend you [secure this channel](#secure-channel).</span></span>|<span data-ttu-id="f7e8a-153">Kommunikációs még csak egyirányú, toohello portál</span><span class="sxs-lookup"><span data-stu-id="f7e8a-153">Communication is one-way, toohello portal</span></span>|


## <a name="select-and-filter-your-metrics"></a><span data-ttu-id="f7e8a-154">Válassza ki, és a metrikák szűrése</span><span class="sxs-lookup"><span data-stu-id="f7e8a-154">Select and filter your metrics</span></span>

<span data-ttu-id="f7e8a-155">(Klasszikus található ASP.NET-alkalmazások hello SDK legújabb.)</span><span class="sxs-lookup"><span data-stu-id="f7e8a-155">(Available on classic ASP.NET apps with hello latest SDK.)</span></span>

<span data-ttu-id="f7e8a-156">Egyéni KPI élő tetszőleges szűrők alkalmazásával bármely Application Insights telemetria hello portálról figyelheti.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-156">You can monitor custom KPI live by applying arbitrary filters on any Application Insights telemetry from hello portal.</span></span> <span data-ttu-id="f7e8a-157">Kattintson a hello szűrővezérlő bemutatja, amikor Ön rámutatáskor hello diagramok bármelyikét.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-157">Click hello filter control that shows when you mouse-over any of hello charts.</span></span> <span data-ttu-id="f7e8a-158">a következő diagram hello van egy egyéni kérelem száma KPI URL-cím és időtartama attribútumok szűrőkkel ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-158">hello following chart is plotting a custom Request count KPI with filters on URL and Duration attributes.</span></span> <span data-ttu-id="f7e8a-159">Ellenőrizze a szűrőket a hello adatfolyam Preview szakasz bemutatja egy élő adatcsatorna telemetriai adatot, amely megfelel az idő megadott bármikor hello feltételeket.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-159">Validate your filters with hello Stream Preview section that shows a live feed of telemetry that matches hello criteria you have specified at any point in time.</span></span> 

![Egyéni kérelem KPI](./media/app-insights-live-stream/live-stream-filteredMetric.png)

<span data-ttu-id="f7e8a-161">Figyelheti a roleservice száma.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-161">You can monitor a value different from Count.</span></span> <span data-ttu-id="f7e8a-162">hello beállítások függ hello adatfolyam, amely lehet bármely Application Insights telemetria: kérelmek, függőségek, kivételek, nyomkövetések, eseményeket és metrikákat.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-162">hello options depend on hello type of stream, which could be any Application Insights telemetry: requests, dependencies, exceptions, traces, events, or metrics.</span></span> <span data-ttu-id="f7e8a-163">Azok a saját [egyéni mérési](app-insights-api-custom-events-metrics.md#properties):</span><span class="sxs-lookup"><span data-stu-id="f7e8a-163">It can be your own [custom measurement](app-insights-api-custom-events-metrics.md#properties):</span></span>

![Érték-beállítások](./media/app-insights-live-stream/live-stream-valueoptions.png)

<span data-ttu-id="f7e8a-165">Ezenkívül tooApplication Insights telemetria, is figyelheti bármely Windows teljesítményszámláló jelölje ki, amely hello adatfolyam lehetőségek közül, és hello teljesítményszámláló hello helynév.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-165">In addition tooApplication Insights telemetry, you can also monitor any Windows performance counter by selecting that from hello stream options, and providing hello name of hello performance counter.</span></span>

<span data-ttu-id="f7e8a-166">Élő metrikák összesítése két időpontokban: helyileg az egyes kiszolgálókon, majd a összes kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-166">Live metrics are aggregated at two points: locally on each server, and then across all servers.</span></span> <span data-ttu-id="f7e8a-167">Hello alapértelmezett bármelyik hello megfelelő legördülő listákat található egyéb beállítások segítségével módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-167">You can change hello default at either by selecting other options in hello respective drop-downs.</span></span>

## <a name="sample-telemetry-custom-live-diagnostic-events"></a><span data-ttu-id="f7e8a-168">A minta Telemetriai: Egyéni élő diagnosztikai eseményei</span><span class="sxs-lookup"><span data-stu-id="f7e8a-168">Sample Telemetry: Custom Live Diagnostic Events</span></span>
<span data-ttu-id="f7e8a-169">Alapértelmezés szerint hello élő események jelennek meg a sikertelen kérelmek és függőségi hívások esetében, kivételek, eseményeket, valamint nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-169">By default, hello live feed of events shows samples of failed requests and dependency calls, exceptions, events, and traces.</span></span> <span data-ttu-id="f7e8a-170">Kattintson a hello ikon toosee alkalmazott hello szűrőfeltételt bármikor idő.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-170">Click hello filter icon toosee hello applied criteria at any point in time.</span></span> 

![Alapértelmezett működés közbeni adatcsatorna](./media/app-insights-live-stream/live-stream-eventsdefault.png)

<span data-ttu-id="f7e8a-172">Mint a metrikákat, megadhatja bármilyen tetszőleges feltételek tooany hello Application Insights telemetria típusú.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-172">As with metrics, you can specify any arbitrary criteria tooany of hello Application Insights telemetry types.</span></span> <span data-ttu-id="f7e8a-173">Ebben a példában azt kiválasztása adott kérelem sikertelen, a nyomkövetések és az események.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-173">In this example, we are selecting specific request failures, traces, and events.</span></span> <span data-ttu-id="f7e8a-174">Azt is kiválasztásával, kivételeket és a függőségi hibák.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-174">We are also selecting all exceptions and dependency failures.</span></span>

![Egyéni élő adatcsatorna](./media/app-insights-live-stream/live-stream-events.png)

<span data-ttu-id="f7e8a-176">Megjegyzés: Jelenleg, kivétel üzenetalapú feltétel hello legkülső kivétel üzenetét használja.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-176">Note: Currently, for Exception message-based criteria, use hello outermost exception message.</span></span> <span data-ttu-id="f7e8a-177">Az előző példában hello toofilter kimenő hello jóindulatú kivétel a belső kivétel üzenet (követi hello "<--" elválasztó) "a program hello ügyfél bontotta a kapcsolatot."</span><span class="sxs-lookup"><span data-stu-id="f7e8a-177">In hello preceding example, toofilter out hello benign exception with inner exception message (follows hello "<--" delimiter) "hello client disconnected."</span></span> <span data-ttu-id="f7e8a-178">Használjon egy üzenet nem-"Hibaüzenet kérés tartalma" feltételeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-178">use a message not-contains "Error reading request content" criteria.</span></span>

<span data-ttu-id="f7e8a-179">A Részletek területen hello hello levő elem hírcsatorna élő kattintással.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-179">See hello details of an item in hello live feed by clicking it.</span></span> <span data-ttu-id="f7e8a-180">Akár szüneteltetheti is, vagy kattintson a hírcsatorna hello **szüneteltetése** vagy egyszerűen a lefelé görgetéshez használható, vagy elemet.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-180">You can pause hello feed either by clicking **Pause** or simply scrolling down, or clicking an item.</span></span> <span data-ttu-id="f7e8a-181">Élő adatcsatorna után görgessen a hátsó toohello felső folytatódik, és hello számláló elemek kattintva gyűjtött során fel lett függesztve.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-181">Live feed will resume after you scroll back toohello top, or by clicking hello counter of items collected while it was paused.</span></span>

![Élő hibák mintát](./media/app-insights-live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a><span data-ttu-id="f7e8a-183">Szűrés a server-példány</span><span class="sxs-lookup"><span data-stu-id="f7e8a-183">Filter by server instance</span></span>

<span data-ttu-id="f7e8a-184">Ha azt szeretné, hogy egy adott kiszolgálói szerepkör példánya toomonitor, kiszolgáló szerint szűrheti.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-184">If you want toomonitor a particular server role instance, you can filter by server.</span></span>

![Élő hibák mintát](./media/app-insights-live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a><span data-ttu-id="f7e8a-186">SDK-követelmények</span><span class="sxs-lookup"><span data-stu-id="f7e8a-186">SDK Requirements</span></span>
<span data-ttu-id="f7e8a-187">Egyéni élő metrikák adatfolyama verzió 2.4.0-beta2 érhető el vagy az újabb [Web Application Insights SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span><span class="sxs-lookup"><span data-stu-id="f7e8a-187">Custom Live Metrics Stream is available with version 2.4.0-beta2 or newer of [Application Insights SDK for web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span></span> <span data-ttu-id="f7e8a-188">Ne felejtse el a NuGet-Csomagkezelő tooselect "Tartalmaznak Prerelease" kapcsolót.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-188">Remember tooselect "Include Prerelease" option from NuGet package manager.</span></span>

## <a name="secure-hello-control-channel"></a><span data-ttu-id="f7e8a-189">Biztonságos hello vezérlőcsatorna</span><span class="sxs-lookup"><span data-stu-id="f7e8a-189">Secure hello control channel</span></span>
<span data-ttu-id="f7e8a-190">hello megadott egyéni szűrők feltételek hátsó toohello élő metrikák összetevő küldése az Application Insights SDK hello.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-190">hello custom filters criteria you specify are sent back toohello Live Metrics component in hello Application Insights SDK.</span></span> <span data-ttu-id="f7e8a-191">hello szűrők tartalmazhat customerIDs például bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-191">hello filters could potentially contain sensitive information such as customerIDs.</span></span> <span data-ttu-id="f7e8a-192">A titkos API-kulcs biztonságos továbbá toohello instrumentation kulcs hello csatorna végezheti el.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-192">You can make hello channel secure with a secret API key in addition toohello instrumentation key.</span></span>
### <a name="create-an-api-key"></a><span data-ttu-id="f7e8a-193">API-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="f7e8a-193">Create an API Key</span></span>

![Api-kulcs létrehozása](./media/app-insights-live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-tooconfiguration"></a><span data-ttu-id="f7e8a-195">API-kulcs tooConfiguration hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f7e8a-195">Add API key tooConfiguration</span></span>
<span data-ttu-id="f7e8a-196">Hello applicationinsights.config fájlban adja hozzá hello AuthenticationApiKey toohello QuickPulseTelemetryModule:</span><span class="sxs-lookup"><span data-stu-id="f7e8a-196">In hello applicationinsights.config file, add hello AuthenticationApiKey toohello QuickPulseTelemetryModule:</span></span>
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add> 

```
<span data-ttu-id="f7e8a-197">Vagy a kódban, állítsa be a hello QuickPulseTelemetryModule:</span><span class="sxs-lookup"><span data-stu-id="f7e8a-197">Or in code, set it on hello QuickPulseTelemetryModule:</span></span>

``` C#

    module.AuthenticationApiKey = "YOUR-API-KEY-HERE";

```

<span data-ttu-id="f7e8a-198">Ha ismeri fel, és az összes csatlakoztatott kiszolgálók hello megbízható, megpróbálhatja hello egyéni szűrők hitelesített hello csatorna nélkül.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-198">However, if you recognize and trust all hello connected servers, you can try hello custom filters without hello authenticated channel.</span></span> <span data-ttu-id="f7e8a-199">Ez a beállítás hat hónapig érhető el.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-199">This option is available for six months.</span></span> <span data-ttu-id="f7e8a-200">Ez a felülbírálás kell egyszer minden új munkamenet, vagy ha új kiszolgáló online elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-200">This override is required once every new session, or when a new server comes online.</span></span>

![Élő metrikák hitelesítési beállítások](./media/app-insights-live-stream/live-stream-auth.png)

>[!NOTE]
><span data-ttu-id="f7e8a-202">Erősen ajánlott beállítani a hello hitelesített csatornát hello szűrési feltételeket a potenciálisan bizalmas adatok, például a CustomerID megadása előtt.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-202">We strongly recommend that you set up hello authenticated channel before entering potentially sensitive information like CustomerID in hello filter criteria.</span></span>
>

## <a name="generating-a-performance-test-load"></a><span data-ttu-id="f7e8a-203">Egy teszt víruskeresőké létrehozása</span><span class="sxs-lookup"><span data-stu-id="f7e8a-203">Generating a performance test load</span></span>

<span data-ttu-id="f7e8a-204">Ha azt szeretné, egy terheléselosztási toowatch hello hatásának növelése érdekében használjon hello teljesítményteszt panel.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-204">If you want toowatch hello effect of a load increase, use hello Performance Test blade.</span></span> <span data-ttu-id="f7e8a-205">Egyidejű felhasználók száma kérelmeinek szimulálja azt.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-205">It simulates requests from a number of simultaneous users.</span></span> <span data-ttu-id="f7e8a-206">Vagy "manuális tesztek" Futtatás (ping-vizsgálatok) egyetlen URL-címet, vagy futtatható egy [többlépéses webteszt teljesítmény](app-insights-monitor-web-app-availability.md#multi-step-web-tests) töltse fel (a hello azonos módon, mint a rendelkezésre állási tesztelése).</span><span class="sxs-lookup"><span data-stu-id="f7e8a-206">It can run either "manual tests" (ping tests) of a single URL, or it can run a [multi-step web performance test](app-insights-monitor-web-app-availability.md#multi-step-web-tests) that you upload (in hello same way as an availability test).</span></span>

> [!TIP]
> <span data-ttu-id="f7e8a-207">Hello teljesítményteszt létrehozása után nyissa meg a hello tesztelése, és élő adatfolyam panel külön Windows hello.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-207">After you create hello performance test, open hello test and hello Live Stream blade in separate windows.</span></span> <span data-ttu-id="f7e8a-208">Láthatja, mikor hello várólistára teljesítmény teszt elindul, és tekintse meg élő adatfolyam: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-208">You can see when hello queued performance test starts, and watch live stream at hello same time.</span></span>
>


## <a name="troubleshooting"></a><span data-ttu-id="f7e8a-209">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="f7e8a-209">Troubleshooting</span></span>

<span data-ttu-id="f7e8a-210">Nincs adat?</span><span class="sxs-lookup"><span data-stu-id="f7e8a-210">No data?</span></span> <span data-ttu-id="f7e8a-211">Ha az alkalmazás egy védett hálózati: metrikák élő adatfolyam használ a különböző IP-címeket, mint más Application Insights telemetria.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-211">If your application is in a protected network: Live Metrics Stream uses a different IP addresses than other Application Insights telemetry.</span></span> <span data-ttu-id="f7e8a-212">Győződjön meg arról, hogy [adott IP-címek](app-insights-ip-addresses.md) nyitva a tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="f7e8a-212">Make sure [those IP addresses](app-insights-ip-addresses.md) are open in your firewall.</span></span>



## <a name="next-steps"></a><span data-ttu-id="f7e8a-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f7e8a-213">Next steps</span></span>
* [<span data-ttu-id="f7e8a-214">Az Application insights szolgáltatással megfigyelési kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="f7e8a-214">Monitoring usage with Application Insights</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="f7e8a-215">Diagnosztikai keresés</span><span class="sxs-lookup"><span data-stu-id="f7e8a-215">Using Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="f7e8a-216">Profilkészítő</span><span class="sxs-lookup"><span data-stu-id="f7e8a-216">Profiler</span></span>](app-insights-profiler.md)
* [<span data-ttu-id="f7e8a-217">Pillanatkép hibakereső</span><span class="sxs-lookup"><span data-stu-id="f7e8a-217">Snapshot debugger</span></span>](app-insights-snapshot-debugger.md)

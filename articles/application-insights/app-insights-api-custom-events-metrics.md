---
title: "egyéni események és metrikák Hirdetéselemző API-t aaaApplication |} Microsoft Docs"
description: "Helyezze be néhány sornyi kódot az eszköz vagy egy asztali alkalmazás, a weblap vagy egy szolgáltatást, tootrack használatának és problémák elemzéséhez."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 80400495-c67b-4468-a92e-abf49793a54d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/17/2017
ms.author: bwren
ms.openlocfilehash: f3d207a47bb4825efda806a19dd0c26540db7bdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a><span data-ttu-id="65256-103">Application Insights API egyéni események és metrikák</span><span class="sxs-lookup"><span data-stu-id="65256-103">Application Insights API for custom events and metrics</span></span>

<span data-ttu-id="65256-104">Szúrja be néhány sornyi kódot az alkalmazás toofind ki a felhasználók tevékenységeit vele, vagy toohelp eseményadatokat.</span><span class="sxs-lookup"><span data-stu-id="65256-104">Insert a few lines of code in your application toofind out what users are doing with it, or toohelp diagnose issues.</span></span> <span data-ttu-id="65256-105">Eszköz- és asztali alkalmazások, a webes ügyfelekkel és a webkiszolgálók telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="65256-105">You can send telemetry from device and desktop apps, web clients, and web servers.</span></span> <span data-ttu-id="65256-106">Használjon hello [Azure Application Insights](app-insights-overview.md) alapvető telemetriai API toosend egyéni események és metrikák és a saját szabványos telemetriai verziói.</span><span class="sxs-lookup"><span data-stu-id="65256-106">Use hello [Azure Application Insights](app-insights-overview.md) core telemetry API toosend custom events and metrics, and your own versions of standard telemetry.</span></span> <span data-ttu-id="65256-107">Ez az API van hello azonos API-t, hogy az Application Insights-adatgyűjtők használja hello szabvány.</span><span class="sxs-lookup"><span data-stu-id="65256-107">This API is hello same API that hello standard Application Insights data collectors use.</span></span>

## <a name="api-summary"></a><span data-ttu-id="65256-108">API összefoglaló</span><span class="sxs-lookup"><span data-stu-id="65256-108">API summary</span></span>
<span data-ttu-id="65256-109">hello API egységes néhány kisebb eltérések mellett minden platformon.</span><span class="sxs-lookup"><span data-stu-id="65256-109">hello API is uniform across all platforms, apart from a few small variations.</span></span>

| <span data-ttu-id="65256-110">Módszer</span><span class="sxs-lookup"><span data-stu-id="65256-110">Method</span></span> | <span data-ttu-id="65256-111">A használt</span><span class="sxs-lookup"><span data-stu-id="65256-111">Used for</span></span> |
| --- | --- |
| [`TrackPageView`](#page-views) |<span data-ttu-id="65256-112">Lapok, képernyők, paneleken vagy űrlap.</span><span class="sxs-lookup"><span data-stu-id="65256-112">Pages, screens, blades, or forms.</span></span> |
| [`TrackEvent`](#trackevent) |<span data-ttu-id="65256-113">Felhasználói műveletek és más események.</span><span class="sxs-lookup"><span data-stu-id="65256-113">User actions and other events.</span></span> <span data-ttu-id="65256-114">Használt tootrack felhasználói viselkedés vagy toomonitor teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="65256-114">Used tootrack user behavior or toomonitor performance.</span></span> |
| [`TrackMetric`](#trackmetric) |<span data-ttu-id="65256-115">Például a várólista-hosszúságok meggátolják TELJESÍTMÉNYMÉRÉSEK toospecific események nem kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="65256-115">Performance measurements such as queue lengths not related toospecific events.</span></span> |
| [`TrackException`](#trackexception) |<span data-ttu-id="65256-116">Naplózási kivételek elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="65256-116">Logging exceptions for diagnosis.</span></span> <span data-ttu-id="65256-117">Nyomkövetés, ahonnan fordulhat elő, a kapcsolat tooother események és vizsgálja meg a híváslánc megjelenik.</span><span class="sxs-lookup"><span data-stu-id="65256-117">Trace where they occur in relation tooother events and examine stack traces.</span></span> |
| [`TrackRequest`](#trackrequest) |<span data-ttu-id="65256-118">A naplózás hello gyakorisága és időtartama a kiszolgálói kérelmek Teljesítményelemzési célokat.</span><span class="sxs-lookup"><span data-stu-id="65256-118">Logging hello frequency and duration of server requests for performance analysis.</span></span> |
| [`TrackTrace`](#tracktrace) |<span data-ttu-id="65256-119">Diagnosztikai naplózás.</span><span class="sxs-lookup"><span data-stu-id="65256-119">Diagnostic log messages.</span></span> <span data-ttu-id="65256-120">Külső naplókat is rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="65256-120">You can also capture third-party logs.</span></span> |
| [`TrackDependency`](#trackdependency) |<span data-ttu-id="65256-121">A naplózás hello időtartama és gyakorisága hívások tooexternal összetevők, amelyek az alkalmazás függ.</span><span class="sxs-lookup"><span data-stu-id="65256-121">Logging hello duration and frequency of calls tooexternal components that your app depends on.</span></span> |

<span data-ttu-id="65256-122">Is [tulajdonságok és metrikákat](#properties) toomost az adott telemetriai hívások.</span><span class="sxs-lookup"><span data-stu-id="65256-122">You can [attach properties and metrics](#properties) toomost of these telemetry calls.</span></span>

## <span data-ttu-id="65256-123"><a name="prep"></a>Előkészületek</span><span class="sxs-lookup"><span data-stu-id="65256-123"><a name="prep"></a>Before you start</span></span>
<span data-ttu-id="65256-124">Ha egy hivatkozás még nem rendelkezik az Application Insights SDK:</span><span class="sxs-lookup"><span data-stu-id="65256-124">If you don't have a reference on Application Insights SDK yet:</span></span>

* <span data-ttu-id="65256-125">Hello Application Insights SDK tooyour projekt hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="65256-125">Add hello Application Insights SDK tooyour project:</span></span>

  * [<span data-ttu-id="65256-126">ASP.NET-projekt</span><span class="sxs-lookup"><span data-stu-id="65256-126">ASP.NET project</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="65256-127">Java-projekt</span><span class="sxs-lookup"><span data-stu-id="65256-127">Java project</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="65256-128">Az összes weboldal JavaScript</span><span class="sxs-lookup"><span data-stu-id="65256-128">JavaScript in each webpage</span></span>](app-insights-javascript.md) 
* <span data-ttu-id="65256-129">Az eszköz vagy a web server kódjában a következők:</span><span class="sxs-lookup"><span data-stu-id="65256-129">In your device or web server code, include:</span></span>

    <span data-ttu-id="65256-130">*C#:*`using Microsoft.ApplicationInsights;`</span><span class="sxs-lookup"><span data-stu-id="65256-130">*C#:* `using Microsoft.ApplicationInsights;`</span></span>

    <span data-ttu-id="65256-131">*Visual Basic:*`Imports Microsoft.ApplicationInsights`</span><span class="sxs-lookup"><span data-stu-id="65256-131">*Visual Basic:* `Imports Microsoft.ApplicationInsights`</span></span>

    <span data-ttu-id="65256-132">*Java:*`import com.microsoft.applicationinsights.TelemetryClient;`</span><span class="sxs-lookup"><span data-stu-id="65256-132">*Java:* `import com.microsoft.applicationinsights.TelemetryClient;`</span></span>

## <a name="constructing-a-telemetryclient-instance"></a><span data-ttu-id="65256-133">Egy TelemetryClient példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="65256-133">Constructing a TelemetryClient instance</span></span>
<span data-ttu-id="65256-134">Példányának összeállításához `TelemetryClient` (csak a JavaScript weblapok):</span><span class="sxs-lookup"><span data-stu-id="65256-134">Construct an instance of `TelemetryClient` (except in JavaScript in webpages):</span></span>

<span data-ttu-id="65256-135">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-135">*C#*</span></span>

    private TelemetryClient telemetry = new TelemetryClient();

<span data-ttu-id="65256-136">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="65256-136">*Visual Basic*</span></span>

    Private Dim telemetry As New TelemetryClient

<span data-ttu-id="65256-137">*Java*</span><span class="sxs-lookup"><span data-stu-id="65256-137">*Java*</span></span>

    private TelemetryClient telemetry = new TelemetryClient();

<span data-ttu-id="65256-138">TelemetryClient szálbiztos.</span><span class="sxs-lookup"><span data-stu-id="65256-138">TelemetryClient is thread-safe.</span></span>

<span data-ttu-id="65256-139">Azt javasoljuk, hogy az alkalmazás minden modul TelemetryClient-példány használata.</span><span class="sxs-lookup"><span data-stu-id="65256-139">We recommend that you use an instance of TelemetryClient for each module of your app.</span></span> <span data-ttu-id="65256-140">Például előfordulhat, hogy egy TelemetryClient példány a webszolgáltatás tooreport bejövő HTTP-kérelmek és egy másik egy köztes osztály tooreport üzleti logika események.</span><span class="sxs-lookup"><span data-stu-id="65256-140">For instance, you may have one TelemetryClient instance in your web service tooreport incoming HTTP requests, and another in a middleware class tooreport business logic events.</span></span> <span data-ttu-id="65256-141">Beállítható például `TelemetryClient.Context.User.Id` tootrack felhasználók és a munkamenetek, vagy `TelemetryClient.Context.Device.Id` tooidentify hello gép.</span><span class="sxs-lookup"><span data-stu-id="65256-141">You can set properties such as `TelemetryClient.Context.User.Id` tootrack users and sessions, or `TelemetryClient.Context.Device.Id` tooidentify hello machine.</span></span> <span data-ttu-id="65256-142">Ez az információ a csatolt tooall az eseményeket, amelyek hello példány küld.</span><span class="sxs-lookup"><span data-stu-id="65256-142">This information is attached tooall events that hello instance sends.</span></span>

## <a name="trackevent"></a><span data-ttu-id="65256-143">TrackEvent</span><span class="sxs-lookup"><span data-stu-id="65256-143">TrackEvent</span></span>
<span data-ttu-id="65256-144">Az Application Insights egy *egyéni esemény* a megjeleníthetők adatpont van [Metrikaböngésző](app-insights-metrics-explorer.md) egy összesített száma, és a [diagnosztikai keresési](app-insights-diagnostic-search.md) önálló események.</span><span class="sxs-lookup"><span data-stu-id="65256-144">In Application Insights, a *custom event* is a data point that you can display in [Metrics Explorer](app-insights-metrics-explorer.md) as an aggregated count, and in [Diagnostic Search](app-insights-diagnostic-search.md) as individual occurrences.</span></span> <span data-ttu-id="65256-145">(Ez nem a kapcsolódó tooMVC vagy más keretrendszer "események.")</span><span class="sxs-lookup"><span data-stu-id="65256-145">(It isn't related tooMVC or other framework "events.")</span></span>

<span data-ttu-id="65256-146">Helyezze be `TrackEvent` hívja be a kódját toocount különféle eseményeket.</span><span class="sxs-lookup"><span data-stu-id="65256-146">Insert `TrackEvent` calls in your code toocount various events.</span></span> <span data-ttu-id="65256-147">Milyen gyakran válassza ki a felhasználók egy adott szolgáltatással, milyen gyakran azok meghatározott célok elérése, vagy lehet, hogy milyen gyakran akkor hibák adott típusú.</span><span class="sxs-lookup"><span data-stu-id="65256-147">How often users choose a particular feature, how often they achieve particular goals, or maybe how often they make particular types of mistakes.</span></span>

<span data-ttu-id="65256-148">Például játék alkalmazásban elküldeni egy eseményt, amikor egy felhasználó wins hello játék:</span><span class="sxs-lookup"><span data-stu-id="65256-148">For example, in a game app, send an event whenever a user wins hello game:</span></span>

<span data-ttu-id="65256-149">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="65256-149">*JavaScript*</span></span>

    appInsights.trackEvent("WinGame");

<span data-ttu-id="65256-150">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-150">*C#*</span></span>

    telemetry.TrackEvent("WinGame");

<span data-ttu-id="65256-151">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="65256-151">*Visual Basic*</span></span>

    telemetry.TrackEvent("WinGame")

<span data-ttu-id="65256-152">*Java*</span><span class="sxs-lookup"><span data-stu-id="65256-152">*Java*</span></span>

    telemetry.trackEvent("WinGame");

### <a name="view-your-events-in-hello-microsoft-azure-portal"></a><span data-ttu-id="65256-153">Az események megtekintése hello Microsoft Azure portálon</span><span class="sxs-lookup"><span data-stu-id="65256-153">View your events in hello Microsoft Azure portal</span></span>
<span data-ttu-id="65256-154">Nyissa meg a toosee az események száma egy [Metrikaböngésző](app-insights-metrics-explorer.md) panelen új diagram hozzáadása, és válassza ki **események**.</span><span class="sxs-lookup"><span data-stu-id="65256-154">toosee a count of your events, open a [Metrics Explorer](app-insights-metrics-explorer.md) blade, add a new chart, and select **Events**.</span></span>  

![Tekintse meg az egyéni események száma](./media/app-insights-api-custom-events-metrics/01-custom.png)

<span data-ttu-id="65256-156">különböző események számát toocompare hello beállítása hello diagramtípus túl**rács**, és az esemény neve:</span><span class="sxs-lookup"><span data-stu-id="65256-156">toocompare hello counts of different events, set hello chart type too**Grid**, and group by event name:</span></span>

![Hello diagramtípus és csoportosítási beállítása](./media/app-insights-api-custom-events-metrics/07-grid.png)

<span data-ttu-id="65256-158">Hello rácson kattintson az esemény neve toosee egyedi adott esemény előfordulása.</span><span class="sxs-lookup"><span data-stu-id="65256-158">On hello grid, click through an event name toosee individual occurrences of that event.</span></span> <span data-ttu-id="65256-159">toosee több részletességi - kattintson bármely előfordulása hello listában.</span><span class="sxs-lookup"><span data-stu-id="65256-159">toosee more detail - click any occurrence in hello list.</span></span>

![Részletezés hello események](./media/app-insights-api-custom-events-metrics/03-instances.png)

<span data-ttu-id="65256-161">a meghatározott események keresési vagy a Metrikaböngésző, set hello panel szűrő toohello esemény nevét, amely kíváncsiak vagyunk toofocus:</span><span class="sxs-lookup"><span data-stu-id="65256-161">toofocus on specific events in either Search or Metrics Explorer, set hello blade's filter toohello event names that you're interested in:</span></span>

![Nyissa meg a szűrők, bontsa ki az esemény nevét, és válasszon egy vagy több érték](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a><span data-ttu-id="65256-163">Egyéni események Analytics</span><span class="sxs-lookup"><span data-stu-id="65256-163">Custom events in Analytics</span></span>

<span data-ttu-id="65256-164">hello telemetriai érhető el hello `customEvents` a tábla [Application Insights Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="65256-164">hello telemetry is available in hello `customEvents` table in [Application Insights Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="65256-165">Minden egyes sorára túl jelenti. a hívás`trackEvent(..)` az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="65256-165">Each row represents a call too`trackEvent(..)` in your app.</span></span> 

<span data-ttu-id="65256-166">Ha [mintavételi](app-insights-sampling.md) működik, hello elemek tulajdonság jeleníti meg egy értéket 1-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="65256-166">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="65256-167">Példa az elemek száma a == 10 azt jelenti, hogy a 10 hívások tootrackEvent() hello mintavételi folyamat csak akkor továbbítódnak, ezek egyikét.</span><span class="sxs-lookup"><span data-stu-id="65256-167">For example itemCount==10 means that of 10 calls tootrackEvent(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="65256-168">egyéni események megfelelő száma tooget, kell használnia ezért használható kóddal például `customEvent | summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="65256-168">tooget a correct count of custom events, you should use therefore use code such as `customEvent | summarize sum(itemCount)`.</span></span>


## <a name="trackmetric"></a><span data-ttu-id="65256-169">TrackMetric</span><span class="sxs-lookup"><span data-stu-id="65256-169">TrackMetric</span></span>

<span data-ttu-id="65256-170">Az Application Insights is diagram, amelyek nem csatlakoztatott tooparticular események metrikákat.</span><span class="sxs-lookup"><span data-stu-id="65256-170">Application Insights can chart metrics that are not attached tooparticular events.</span></span> <span data-ttu-id="65256-171">Például felügyelheti a várólista hossza rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="65256-171">For example, you could monitor a queue length at regular intervals.</span></span> <span data-ttu-id="65256-172">A metrika hello egyedi mérések kevésbé fontos, mint a hello változata és a trendeket, és így statisztikai diagramok hasznosak.</span><span class="sxs-lookup"><span data-stu-id="65256-172">With metrics, hello individual measurements are of less interest than hello variations and trends, and so statistical charts are useful.</span></span>

<span data-ttu-id="65256-173">A rendezés toosend metrikák tooApplication Insights, használhatja a hello `TrackMetric(..)` API.</span><span class="sxs-lookup"><span data-stu-id="65256-173">In order toosend metrics tooApplication Insights, you can use hello `TrackMetric(..)` API.</span></span> <span data-ttu-id="65256-174">Számos két módon toosend metrikát.</span><span class="sxs-lookup"><span data-stu-id="65256-174">There are two ways toosend a metric:</span></span> 

* <span data-ttu-id="65256-175">Egyetlen értéket.</span><span class="sxs-lookup"><span data-stu-id="65256-175">Single value.</span></span> <span data-ttu-id="65256-176">Minden alkalommal, amikor az alkalmazás hajt végre egy mérték, küldött megfelelő érték hello tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="65256-176">Every time you perform a measurement in your application, you send hello corresponding value tooApplication Insights.</span></span> <span data-ttu-id="65256-177">Tegyük fel például, hogy rendelkezik-e a tárolóban lévő elemek száma hello leíró metrikát.</span><span class="sxs-lookup"><span data-stu-id="65256-177">For example, assume that you have a metric describing hello number of items in a container.</span></span> <span data-ttu-id="65256-178">Egy adott időszakon belül mindhárom elem put hello tárolóba, és két elem távolítsa el.</span><span class="sxs-lookup"><span data-stu-id="65256-178">During a particular time period, you first put three items into hello container and then you remove two items.</span></span> <span data-ttu-id="65256-179">Ennek megfelelően meghívta `TrackMetric` kétszer: először a hello értéket átadja `3` és majd hello érték `-2`.</span><span class="sxs-lookup"><span data-stu-id="65256-179">Accordingly, you would call `TrackMetric` twice: first passing hello value `3` and then hello value `-2`.</span></span> <span data-ttu-id="65256-180">Az Application Insights mindkét értéket tárolja az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="65256-180">Application Insights stores both values on your behalf.</span></span> 

* <span data-ttu-id="65256-181">Összesítést.</span><span class="sxs-lookup"><span data-stu-id="65256-181">Aggregation.</span></span> <span data-ttu-id="65256-182">Az metrikák használatakor minden egyetlen mérési ritkán érdekében áll.</span><span class="sxs-lookup"><span data-stu-id="65256-182">When working with metrics, every single measurement is rarely of interest.</span></span> <span data-ttu-id="65256-183">Ehelyett fontos Mi történt egy adott időszakon belül összegzését.</span><span class="sxs-lookup"><span data-stu-id="65256-183">Instead a summary of what happened during a particular time period is important.</span></span> <span data-ttu-id="65256-184">Ilyen összegzését nevezik _összesítési_.</span><span class="sxs-lookup"><span data-stu-id="65256-184">Such a summary is called _aggregation_.</span></span> <span data-ttu-id="65256-185">A fenti példa hello, hello összesített metrika sum időszak a `1` , hello szám hello metrika értékek `2`.</span><span class="sxs-lookup"><span data-stu-id="65256-185">In hello above example, hello aggregate metric sum for that time period is `1` and hello count of hello metric values is `2`.</span></span> <span data-ttu-id="65256-186">Hello összesítési módszer használata esetén csak indításakor `TrackMetric` időtartam és a küldési hello összesített időértékeket egy alkalommal.</span><span class="sxs-lookup"><span data-stu-id="65256-186">When using hello aggregation approach, you only invoke `TrackMetric` once per time period and send hello aggregate values.</span></span> <span data-ttu-id="65256-187">Ez az ajánlott megközelítést alkalmazva, mivel jelentősen csökkentheti a hello költség, és a teljesítmény szerint munkaterhelés kevesebb adatküldés tooApplication Insights pontok továbbra is az összes vonatkozó információk összegyűjtése közben hello.</span><span class="sxs-lookup"><span data-stu-id="65256-187">This is hello recommended approach since it can significantly reduce hello cost and performance overhead by sending fewer data points tooApplication Insights, while still collecting all relevant information.</span></span>

### <a name="examples"></a><span data-ttu-id="65256-188">Példák:</span><span class="sxs-lookup"><span data-stu-id="65256-188">Examples:</span></span>

#### <a name="single-values"></a><span data-ttu-id="65256-189">Egyetlen értékek</span><span class="sxs-lookup"><span data-stu-id="65256-189">Single values</span></span>

<span data-ttu-id="65256-190">toosend a egyetlen metrika értékét:</span><span class="sxs-lookup"><span data-stu-id="65256-190">toosend a single metric value:</span></span>

<span data-ttu-id="65256-191">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="65256-191">*JavaScript*</span></span>

 ```Javascript
     appInsights.trackMetric("queueLength", 42.0);
 ```

<span data-ttu-id="65256-192">*C#, Java*</span><span class="sxs-lookup"><span data-stu-id="65256-192">*C#, Java*</span></span>

```C#
    var sample = new MetricTelemetry();
    sample.Name = "metric name";
    sample.Value = 42.3;
    telemetryClient.TrackMetric(sample);
```

#### <a name="aggregating-metrics"></a><span data-ttu-id="65256-193">Metrikák összesítése</span><span class="sxs-lookup"><span data-stu-id="65256-193">Aggregating metrics</span></span>

<span data-ttu-id="65256-194">Mielőtt elküldené a alkalmazást, a tooreduce sávszélesség, a költségek és tooimprove teljesítmény tooaggregate metrikák ajánlott.</span><span class="sxs-lookup"><span data-stu-id="65256-194">It is recommended tooaggregate metrics before sending them from your app, tooreduce bandwidth, cost and tooimprove performance.</span></span>
<span data-ttu-id="65256-195">Itt látható egy példa összesítése kódot:</span><span class="sxs-lookup"><span data-stu-id="65256-195">Here is an example of aggregating code:</span></span>

<span data-ttu-id="65256-196">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-196">*C#*</span></span>

```C#
using System;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;

namespace MetricAggregationExample
{
    /// <summary>
    /// Aggregates metric values for a single time period.
    /// </summary>
    internal class MetricAggregator
    {
        private SpinLock _trackLock = new SpinLock();

        public DateTimeOffset StartTimestamp    { get; }
        public int Count                        { get; private set; }
        public double Sum                       { get; private set; }
        public double SumOfSquares              { get; private set; }
        public double Min                       { get; private set; }
        public double Max                       { get; private set; }
        public double Average                   { get { return (Count == 0) ? 0 : (Sum / Count); } }
        public double Variance                  { get { return (Count == 0) ? 0 : (SumOfSquares / Count)
                                                                                  - (Average * Average); } }
        public double StandardDeviation         { get { return Math.Sqrt(Variance); } }

        public MetricAggregator(DateTimeOffset startTimestamp)
        {
            this.StartTimestamp = startTimestamp;
        }

        public void TrackValue(double value)
        {
            bool lockAcquired = false;

            try
            {
                _trackLock.Enter(ref lockAcquired);

                if ((Count == 0) || (value < Min))  { Min = value; }
                if ((Count == 0) || (value > Max))  { Max = value; }
                Count++;
                Sum += value;
                SumOfSquares += value * value;
            }
            finally
            {
                if (lockAcquired)
                {
                    _trackLock.Exit();
                }
            }
        }
    }   // internal class MetricAggregator

    /// <summary>
    /// Accepts metric values and sends hello aggregated values at 1-minute intervals.
    /// </summary>
    public sealed class Metric : IDisposable
    {
        private static readonly TimeSpan AggregationPeriod = TimeSpan.FromSeconds(60);

        private bool _isDisposed = false;
        private MetricAggregator _aggregator = null;
        private readonly TelemetryClient _telemetryClient;

        public string Name { get; }

        public Metric(string name, TelemetryClient telemetryClient)
        {
            this.Name = name ?? "null";
            this._aggregator = new MetricAggregator(DateTimeOffset.UtcNow);
            this._telemetryClient = telemetryClient ?? throw new ArgumentNullException(nameof(telemetryClient));

            Task.Run(this.AggregatorLoopAsync);
        }

        public void TrackValue(double value)
        {
            MetricAggregator currAggregator = _aggregator;
            if (currAggregator != null)
            {
                currAggregator.TrackValue(value);
            }
        }

        private async Task AggregatorLoopAsync()
        {
            while (_isDisposed == false)
            {
                try
                {
                    // Wait for end end of hello aggregation period:
                    await Task.Delay(AggregationPeriod).ConfigureAwait(continueOnCapturedContext: false);

                    // Atomically snap hello current aggregation:
                    MetricAggregator nextAggregator = new MetricAggregator(DateTimeOffset.UtcNow);
                    MetricAggregator prevAggregator = Interlocked.Exchange(ref _aggregator, nextAggregator);

                    // Only send anything is at least one value was measured:
                    if (prevAggregator != null && prevAggregator.Count > 0)
                    {
                        // Compute hello actual aggregation period length:
                        TimeSpan aggPeriod = nextAggregator.StartTimestamp - prevAggregator.StartTimestamp;
                        if (aggPeriod.TotalMilliseconds < 1)
                        {
                            aggPeriod = TimeSpan.FromMilliseconds(1);
                        }

                        // Construct hello metric telemetry item and send:
                        var aggregatedMetricTelemetry = new MetricTelemetry(
                                Name,
                                prevAggregator.Count,
                                prevAggregator.Sum,
                                prevAggregator.Min,
                                prevAggregator.Max,
                                prevAggregator.StandardDeviation);
                        aggregatedMetricTelemetry.Properties["AggregationPeriod"] = aggPeriod.ToString("c");

                        _telemetryClient.Track(aggregatedMetricTelemetry);
                    }
                }
                catch(Exception ex)
                {
                    // log ex as appropriate for your application
                }
            }
        }

        void IDisposable.Dispose()
        {
            _isDisposed = true;
            _aggregator = null;
        }
    }   // public sealed class Metric
}
```

### <a name="custom-metrics-in-metrics-explorer"></a><span data-ttu-id="65256-197">Egyéni metrikák a Metrikaböngészőben</span><span class="sxs-lookup"><span data-stu-id="65256-197">Custom metrics in Metrics Explorer</span></span>

<span data-ttu-id="65256-198">toosee hello eredményeivel, nyissa meg a Metrikaböngésző, és új diagram hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="65256-198">toosee hello results, open Metrics Explorer and add a new chart.</span></span> <span data-ttu-id="65256-199">Hello diagram tooshow a metrika szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="65256-199">Edit hello chart tooshow your metric.</span></span>

> [!NOTE]
> <span data-ttu-id="65256-200">Az egyéni metrika is igénybe vehet néhány percet tooappear elérhető hello listájában.</span><span class="sxs-lookup"><span data-stu-id="65256-200">Your custom metric might take several minutes tooappear in hello list of available metrics.</span></span>
>

![Új diagram hozzáadása vagy a diagram az egyéni, válassza ki és a metrika](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a><span data-ttu-id="65256-202">Egyéni metrikák Analytics</span><span class="sxs-lookup"><span data-stu-id="65256-202">Custom metrics in Analytics</span></span>

<span data-ttu-id="65256-203">hello telemetriai érhető el hello `customMetrics` a tábla [Application Insights Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="65256-203">hello telemetry is available in hello `customMetrics` table in [Application Insights Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="65256-204">Minden egyes sorára túl jelenti. a hívás`trackMetric(..)` az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="65256-204">Each row represents a call too`trackMetric(..)` in your app.</span></span>
* <span data-ttu-id="65256-205">`valueSum`-Ez az hello mérések hello összege.</span><span class="sxs-lookup"><span data-stu-id="65256-205">`valueSum` - This is hello sum of hello measurements.</span></span> <span data-ttu-id="65256-206">tooget hello átlagos értékének nullával `valueCount`.</span><span class="sxs-lookup"><span data-stu-id="65256-206">tooget hello mean value, divide by `valueCount`.</span></span>
* <span data-ttu-id="65256-207">`valueCount`-Ez volt összesítése mérések számát hello `trackMetric(..)` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="65256-207">`valueCount` - hello number of measurements that were aggregated into this `trackMetric(..)` call.</span></span>

## <a name="page-views"></a><span data-ttu-id="65256-208">Lapmegtekintések</span><span class="sxs-lookup"><span data-stu-id="65256-208">Page views</span></span>
<span data-ttu-id="65256-209">Egy eszköz vagy a weblap alkalmazásban lap nézet telemetriai küldi alapértelmezés szerint ha egyes képernyőit vagy lapon be van töltve.</span><span class="sxs-lookup"><span data-stu-id="65256-209">In a device or webpage app, page view telemetry is sent by default when each screen or page is loaded.</span></span> <span data-ttu-id="65256-210">De további vagy különböző időpontokban, hogy tootrack Lapmegtekintések módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="65256-210">But you can change that tootrack page views at additional or different times.</span></span> <span data-ttu-id="65256-211">Például megjeleníti a tabulátorokat vagy paneleken alkalmazásban, érdemes tootrack oldal amikor hello felhasználó megnyit egy új panelen.</span><span class="sxs-lookup"><span data-stu-id="65256-211">For example, in an app that displays tabs or blades, you might want tootrack a page whenever hello user opens a new blade.</span></span>

![Használati fókuszban panelen – áttekintés](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

<span data-ttu-id="65256-213">Felhasználó és a munkamenet adatküldést, tulajdonságai lap nézetek, valamint úgy hello felhasználó és a munkamenet diagramok lap nézet telemetriai esetén életben származnak.</span><span class="sxs-lookup"><span data-stu-id="65256-213">User and session data is sent as properties along with page views, so hello user and session charts come alive when there is page view telemetry.</span></span>

### <a name="custom-page-views"></a><span data-ttu-id="65256-214">Egyéni Lapmegtekintések</span><span class="sxs-lookup"><span data-stu-id="65256-214">Custom page views</span></span>
<span data-ttu-id="65256-215">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="65256-215">*JavaScript*</span></span>

    appInsights.trackPageView("tab1");

<span data-ttu-id="65256-216">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-216">*C#*</span></span>

    telemetry.TrackPageView("GameReviewPage");

<span data-ttu-id="65256-217">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="65256-217">*Visual Basic*</span></span>

    telemetry.TrackPageView("GameReviewPage")


<span data-ttu-id="65256-218">Ha különböző HTML-lapok belül több lap van, hello URL-címe túl is megadhatja:</span><span class="sxs-lookup"><span data-stu-id="65256-218">If you have several tabs within different HTML pages, you can specify hello URL too:</span></span>

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a><span data-ttu-id="65256-219">Lapmegtekintések időzítése</span><span class="sxs-lookup"><span data-stu-id="65256-219">Timing page views</span></span>
<span data-ttu-id="65256-220">Alapértelmezés szerint a hello idők jelentése szerint **lapmegtekintés betöltési ideje** vannak mért hello böngésző hello kérelmet küld, amíg hello böngésző lap betöltési esemény nevezik.</span><span class="sxs-lookup"><span data-stu-id="65256-220">By default, hello times reported as **Page view load time** are measured from when hello browser sends hello request, until hello browser's page load event is called.</span></span>

<span data-ttu-id="65256-221">Ehelyett lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="65256-221">Instead, you can either:</span></span>

* <span data-ttu-id="65256-222">Az explicit időtartamot hello [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) hívja: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.</span><span class="sxs-lookup"><span data-stu-id="65256-222">Set an explicit duration in hello [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) call: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.</span></span>
* <span data-ttu-id="65256-223">Hello oldalhívás nézet időzítési használja `startTrackPage` és `stopTrackPage`.</span><span class="sxs-lookup"><span data-stu-id="65256-223">Use hello page view timing calls `startTrackPage` and `stopTrackPage`.</span></span>

<span data-ttu-id="65256-224">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="65256-224">*JavaScript*</span></span>

    // toostart timing a page:
    appInsights.startTrackPage("Page1");

<span data-ttu-id="65256-225">...</span><span class="sxs-lookup"><span data-stu-id="65256-225">...</span></span>

    // toostop timing and log hello page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

<span data-ttu-id="65256-226">hello hello első paraméter társítja hello start használható nevét, és állítsa le a hívások.</span><span class="sxs-lookup"><span data-stu-id="65256-226">hello name that you use as hello first parameter associates hello start and stop calls.</span></span> <span data-ttu-id="65256-227">Alapértelmezés szerint toohello aktuális lap neve.</span><span class="sxs-lookup"><span data-stu-id="65256-227">It defaults toohello current page name.</span></span>

<span data-ttu-id="65256-228">hello eredményül kapott lapbetöltési jelenik meg a Metrikaböngészőben időtartamok vannak származtatva hello hello időközétől start, és állítsa le a hívásokat.</span><span class="sxs-lookup"><span data-stu-id="65256-228">hello resulting page load durations displayed in Metrics Explorer are derived from hello interval between hello start and stop calls.</span></span> <span data-ttu-id="65256-229">Az működik tooyou milyen időköz ténylegesen idő.</span><span class="sxs-lookup"><span data-stu-id="65256-229">It's up tooyou what interval you actually time.</span></span>

### <a name="page-telemetry-in-analytics"></a><span data-ttu-id="65256-230">Az elemzés lap telemetria</span><span class="sxs-lookup"><span data-stu-id="65256-230">Page telemetry in Analytics</span></span>

<span data-ttu-id="65256-231">A [Analytics](app-insights-analytics.md) két tábla browser műveletek adatait megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="65256-231">In [Analytics](app-insights-analytics.md) two tables show data from browser operations:</span></span>

* <span data-ttu-id="65256-232">Hello `pageViews` tábla hello URL-cím és a lap címét kapcsolatos adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="65256-232">hello `pageViews` table contains data about hello URL and page title</span></span>
* <span data-ttu-id="65256-233">Hello `browserTimings` tábla ügyfél teljesítményét, kapcsolatos adatokat tartalmaz, például a hello igénybe vett idő tooprocess hello bejövő adatok</span><span class="sxs-lookup"><span data-stu-id="65256-233">hello `browserTimings` table contains data about client performance, such as hello time taken tooprocess hello incoming data</span></span>

<span data-ttu-id="65256-234">toofind mennyi ideig hello böngésző veszi tooprocess különböző oldalain:</span><span class="sxs-lookup"><span data-stu-id="65256-234">toofind how long hello browser takes tooprocess different pages:</span></span>

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

<span data-ttu-id="65256-235">a különböző böngészők toodiscover a hello popularities:</span><span class="sxs-lookup"><span data-stu-id="65256-235">toodiscover hello popularities of different browsers:</span></span>

```
pageViews | summarize count() by client_Browser
```

<span data-ttu-id="65256-236">tooassociate oldalhívás nézetek tooAJAX, csatlakozik a függőségek:</span><span class="sxs-lookup"><span data-stu-id="65256-236">tooassociate page views tooAJAX calls, join with dependencies:</span></span>

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a><span data-ttu-id="65256-237">TrackRequest</span><span class="sxs-lookup"><span data-stu-id="65256-237">TrackRequest</span></span>
<span data-ttu-id="65256-238">hello server SDK TrackRequest toolog HTTP-kérelmek használja.</span><span class="sxs-lookup"><span data-stu-id="65256-238">hello server SDK uses TrackRequest toolog HTTP requests.</span></span>

<span data-ttu-id="65256-239">Akkor is is meghívhatja, ha azt szeretné, hogy toosimulate kérelmek környezetben ahol hello webes szolgáltatás modul fut nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="65256-239">You can also call it yourself if you want toosimulate requests in a context where you don't have hello web service module running.</span></span>

<span data-ttu-id="65256-240">Azonban hello ajánlott módja toosend – kéréstelemetria ahol hello kérelem úgy működik, mint egy <a href="#operation-context">műveletkörnyezetet</a>.</span><span class="sxs-lookup"><span data-stu-id="65256-240">However, hello recommended way toosend request telemetry is where hello request acts as an <a href="#operation-context">operation context</a>.</span></span>

## <a name="operation-context"></a><span data-ttu-id="65256-241">A művelet a környezetben</span><span class="sxs-lookup"><span data-stu-id="65256-241">Operation context</span></span>
<span data-ttu-id="65256-242">Telemetria elemek együtt is társíthat, csatolásával toothem egy közös műveletet.</span><span class="sxs-lookup"><span data-stu-id="65256-242">You can associate telemetry items together by attaching toothem a common operation ID.</span></span> <span data-ttu-id="65256-243">hello normál kérés-nyomkövetési modul elvégzi ezt a kivételeket és eseményeket, amelyek küldött HTTP-kérelem feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="65256-243">hello standard request-tracking module does this for exceptions and other events that are sent while an HTTP request is being processed.</span></span> <span data-ttu-id="65256-244">A [keresési](app-insights-diagnostic-search.md) és [Analytics](app-insights-analytics.md), hello azonosító tooeasily keresés használata hello kérelemhez társított eseményeket.</span><span class="sxs-lookup"><span data-stu-id="65256-244">In [Search](app-insights-diagnostic-search.md) and [Analytics](app-insights-analytics.md), you can use hello ID tooeasily find any events associated with hello request.</span></span>

<span data-ttu-id="65256-245">hello legegyszerűbb módja tooset hello azonosító tooset egy művelet környezet ebben a mintában használatával:</span><span class="sxs-lookup"><span data-stu-id="65256-245">hello easiest way tooset hello ID is tooset an operation context by using this pattern:</span></span>

<span data-ttu-id="65256-246">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-246">*C#*</span></span>

```C#
// Establish an operation context and associated telemetry item:
using (var operation = telemetry.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use hello same operation ID.
    ...
    telemetry.TrackTrace(...); // or other Track* calls
    ...
    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetry.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

<span data-ttu-id="65256-247">Egy művelet környezet beállítása mellett `StartOperation` telemetriai megadott hello típusú elem létrehozása.</span><span class="sxs-lookup"><span data-stu-id="65256-247">Along with setting an operation context, `StartOperation` creates a telemetry item of hello type that you specify.</span></span> <span data-ttu-id="65256-248">Hello telemetriai meg eldobásakor hello műveletet, vagy ha explicit módon hívja elemet elküldi `StopOperation`.</span><span class="sxs-lookup"><span data-stu-id="65256-248">It sends hello telemetry item when you dispose hello operation, or if you explicitly call `StopOperation`.</span></span> <span data-ttu-id="65256-249">Ha `RequestTelemetry` hello telemetriai típusként időtartama túllépte az időkorlátot toohello időközétől kezdési és befejezési van beállítva.</span><span class="sxs-lookup"><span data-stu-id="65256-249">If you use `RequestTelemetry` as hello telemetry type, its duration is set toohello timed interval between start and stop.</span></span>

<span data-ttu-id="65256-250">Művelet környezetek nem ágyazhatók egymásba.</span><span class="sxs-lookup"><span data-stu-id="65256-250">Operation contexts can't be nested.</span></span> <span data-ttu-id="65256-251">Ha már van egy művelet környezetben, akkor minden tartalmazott hello elem, létre hello elem is tartozik Azonosítóval `StartOperation`.</span><span class="sxs-lookup"><span data-stu-id="65256-251">If there is already an operation context, then its ID is associated with all hello contained items, including hello item created with `StartOperation`.</span></span>

<span data-ttu-id="65256-252">A keresés hello műveletkörnyezetet használt toocreate hello **kapcsolódó elemek** listája:</span><span class="sxs-lookup"><span data-stu-id="65256-252">In Search, hello operation context is used toocreate hello **Related Items** list:</span></span>

![Kapcsolódó elemek](./media/app-insights-api-custom-events-metrics/21.png)

<span data-ttu-id="65256-254">[Alkalmazás-insights – egyéni-műveletek-tracking.md] követési egyéni műveletek további információt talál.</span><span class="sxs-lookup"><span data-stu-id="65256-254">See [application-insights-custom-operations-tracking.md] for more information on custom operations tracking.</span></span>

### <a name="requests-in-analytics"></a><span data-ttu-id="65256-255">Az elemzés kérelmek</span><span class="sxs-lookup"><span data-stu-id="65256-255">Requests in Analytics</span></span> 

<span data-ttu-id="65256-256">A [Application Insights Analytics](app-insights-analytics.md), megjelenítése kéri fel a hello `requests` tábla.</span><span class="sxs-lookup"><span data-stu-id="65256-256">In [Application Insights Analytics](app-insights-analytics.md), requests show up in hello `requests` table.</span></span>

<span data-ttu-id="65256-257">Ha [mintavételi](app-insights-sampling.md) van a művelet hello elemek tulajdonság értékét fogja megjeleníteni a 1-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="65256-257">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property will show a value greater than 1.</span></span> <span data-ttu-id="65256-258">Példa az elemek száma a == 10 azt jelenti, hogy a 10 hívások tootrackRequest() hello mintavételi folyamat csak akkor továbbítódnak, ezek egyikét.</span><span class="sxs-lookup"><span data-stu-id="65256-258">For example itemCount==10 means that of 10 calls tootrackRequest(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="65256-259">tooget kérelmek és átlagos időtartama megfelelő száma szegmentált kérelem neve, a kódot használja, mint:</span><span class="sxs-lookup"><span data-stu-id="65256-259">tooget a correct count of requests and average duration segmented by request names, use code such as:</span></span>

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a><span data-ttu-id="65256-260">TrackException</span><span class="sxs-lookup"><span data-stu-id="65256-260">TrackException</span></span>
<span data-ttu-id="65256-261">Kivételek tooApplication Insights küldeni:</span><span class="sxs-lookup"><span data-stu-id="65256-261">Send exceptions tooApplication Insights:</span></span>

* <span data-ttu-id="65256-262">túl[megszámolni](app-insights-metrics-explorer.md), mint utalhat, hogy egy probléma hello gyakorisága.</span><span class="sxs-lookup"><span data-stu-id="65256-262">too[count them](app-insights-metrics-explorer.md), as an indication of hello frequency of a problem.</span></span>
* <span data-ttu-id="65256-263">túl[vizsgálja meg az egyes előfordulások](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="65256-263">too[examine individual occurrences](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="65256-264">hello jelentései tartalmazzák a hello híváslánc megjelenik.</span><span class="sxs-lookup"><span data-stu-id="65256-264">hello reports include hello stack traces.</span></span>

<span data-ttu-id="65256-265">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-265">*C#*</span></span>

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

<span data-ttu-id="65256-266">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="65256-266">*JavaScript*</span></span>

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }

<span data-ttu-id="65256-267">hello SDK-k kivételeket sok automatikusan, így nem mindig toocall TrackException explicit módon.</span><span class="sxs-lookup"><span data-stu-id="65256-267">hello SDKs catch many exceptions automatically, so you don't always have toocall TrackException explicitly.</span></span>

* <span data-ttu-id="65256-268">ASP.NET: [írhat kódot toocatch kivételek](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="65256-268">ASP.NET: [Write code toocatch exceptions](app-insights-asp-net-exceptions.md).</span></span>
* <span data-ttu-id="65256-269">J2EE: [kivételek automatikusan észlelt](app-insights-java-get-started.md#exceptions-and-request-failures).</span><span class="sxs-lookup"><span data-stu-id="65256-269">J2EE: [Exceptions are caught automatically](app-insights-java-get-started.md#exceptions-and-request-failures).</span></span>
* <span data-ttu-id="65256-270">JavaScript: Kivételek automatikusan észlelt.</span><span class="sxs-lookup"><span data-stu-id="65256-270">JavaScript: Exceptions are caught automatically.</span></span> <span data-ttu-id="65256-271">Ha azt szeretné, hogy toodisable automatikus gyűjtemény, adja hozzá egy sor toohello kódrészletet a weblapok beillesztett:</span><span class="sxs-lookup"><span data-stu-id="65256-271">If you want toodisable automatic collection, add a line toohello code snippet that you insert in your webpages:</span></span>

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a><span data-ttu-id="65256-272">Az elemzés kivételek</span><span class="sxs-lookup"><span data-stu-id="65256-272">Exceptions in Analytics</span></span>

<span data-ttu-id="65256-273">A [Application Insights Analytics](app-insights-analytics.md), kivételek jelennek meg hello `exceptions` tábla.</span><span class="sxs-lookup"><span data-stu-id="65256-273">In [Application Insights Analytics](app-insights-analytics.md), exceptions show up in hello `exceptions` table.</span></span>

<span data-ttu-id="65256-274">Ha [mintavételi](app-insights-sampling.md) működik, hello `itemCount` tulajdonság jeleníti meg egy értéket 1-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="65256-274">If [sampling](app-insights-sampling.md) is in operation, hello `itemCount` property shows a value greater than 1.</span></span> <span data-ttu-id="65256-275">Példa az elemek száma a == 10 azt jelenti, hogy a 10 hívások tootrackException() hello mintavételi folyamat csak akkor továbbítódnak, ezek egyikét.</span><span class="sxs-lookup"><span data-stu-id="65256-275">For example itemCount==10 means that of 10 calls tootrackException(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="65256-276">tooget kivételek megfelelő száma szegmentált által kivétel típusa, például a kód használata:</span><span class="sxs-lookup"><span data-stu-id="65256-276">tooget a correct count of exceptions segmented by type of exception, use code such as:</span></span>

```
exceptions | summarize sum(itemCount) by type
```

<span data-ttu-id="65256-277">Nagy része külön változók már kibontása verem adatokat, de lehet lekérni a egymástól hello fontos hello `details` struktúra tooget további.</span><span class="sxs-lookup"><span data-stu-id="65256-277">Most of hello important stack information is already extracted into separate variables, but you can pull apart hello `details` structure tooget more.</span></span> <span data-ttu-id="65256-278">Mivel ez a struktúra dinamikus, akkor hello toohello eredménytípus várt kell konvertálni.</span><span class="sxs-lookup"><span data-stu-id="65256-278">Since this structure is dynamic, you should cast hello result toohello type you expect.</span></span> <span data-ttu-id="65256-279">Példa:</span><span class="sxs-lookup"><span data-stu-id="65256-279">For example:</span></span>

```AIQL
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

<span data-ttu-id="65256-280">a kapcsolódó kérések tooassociate kivételek használjuk:</span><span class="sxs-lookup"><span data-stu-id="65256-280">tooassociate exceptions with their related requests, use a join:</span></span>

```
exceptions
| join (requests) on operation_Id 
```

## <a name="tracktrace"></a><span data-ttu-id="65256-281">TrackTrace</span><span class="sxs-lookup"><span data-stu-id="65256-281">TrackTrace</span></span>
<span data-ttu-id="65256-282">Használjon TrackTrace toohelp Insights egy "navigációs eljárást" tooApplication elküldésével problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="65256-282">Use TrackTrace toohelp diagnose problems by sending a "breadcrumb trail" tooApplication Insights.</span></span> <span data-ttu-id="65256-283">Adattömbök diagnosztikai adatok küldése, és vizsgálja meg azokat a [diagnosztikai keresési](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="65256-283">You can send chunks of diagnostic data and inspect them in [Diagnostic Search](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="65256-284">[Naplófájl adapterek](app-insights-asp-net-trace-logs.md) ezen API toosend külső naplók toohello a portálon.</span><span class="sxs-lookup"><span data-stu-id="65256-284">[Log adapters](app-insights-asp-net-trace-logs.md) use this API toosend third-party logs toohello portal.</span></span>

<span data-ttu-id="65256-285">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-285">*C#*</span></span>

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);


<span data-ttu-id="65256-286">Üzenet tartalma kereshet, de (ellentétben a tulajdonságértékek) nem lehet szűrni rajta.</span><span class="sxs-lookup"><span data-stu-id="65256-286">You can search on message content, but (unlike property values) you can't filter on it.</span></span>

<span data-ttu-id="65256-287">hello méretkorlátot `message` sokkal nagyobb, mint tulajdonságok hello vonatkozó korlátozást.</span><span class="sxs-lookup"><span data-stu-id="65256-287">hello size limit on `message` is much higher than hello limit on properties.</span></span>
<span data-ttu-id="65256-288">TrackTrace előnye, hogy viszonylag hosszú adatok helyezhetik hello üzenetben.</span><span class="sxs-lookup"><span data-stu-id="65256-288">An advantage of TrackTrace is that you can put relatively long data in hello message.</span></span> <span data-ttu-id="65256-289">Például lehet kódolása nincs POST-adatokat.</span><span class="sxs-lookup"><span data-stu-id="65256-289">For example, you can encode POST data there.</span></span>  

<span data-ttu-id="65256-290">Emellett egy súlyossági szint tooyour üzenetet is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="65256-290">In addition, you can add a severity level tooyour message.</span></span> <span data-ttu-id="65256-291">És egyéb telemetriai adatok, például tulajdonság értékek toohelp szűrheti vagy más-más részhalmazához nyomkövetési keresési adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="65256-291">And, like other telemetry, you can add property values toohelp you filter or search for different sets of traces.</span></span> <span data-ttu-id="65256-292">Példa:</span><span class="sxs-lookup"><span data-stu-id="65256-292">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

<span data-ttu-id="65256-293">A [keresési](app-insights-diagnostic-search.md), majd könnyen szűrhetők összes köszönőüzenetei egy adott súlyossági szint tooa adott adatbázis is.</span><span class="sxs-lookup"><span data-stu-id="65256-293">In [Search](app-insights-diagnostic-search.md), you can then easily filter out all hello messages of a particular severity level that relate tooa particular database.</span></span>


### <a name="traces-in-analytics"></a><span data-ttu-id="65256-294">Nyomok Analytics</span><span class="sxs-lookup"><span data-stu-id="65256-294">Traces in Analytics</span></span>

<span data-ttu-id="65256-295">A [Application Insights Analytics](app-insights-analytics.md), tooTrackTrace megjelenítése hívja a hello `traces` tábla.</span><span class="sxs-lookup"><span data-stu-id="65256-295">In [Application Insights Analytics](app-insights-analytics.md), calls tooTrackTrace show up in hello `traces` table.</span></span>

<span data-ttu-id="65256-296">Ha [mintavételi](app-insights-sampling.md) működik, hello elemek tulajdonság jeleníti meg egy értéket 1-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="65256-296">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="65256-297">Példa az elemek száma a == 10 azt jelenti, hogy 10 meghívja túl`trackTrace()`, hello mintavételi folyamat csak akkor továbbítódnak, ezek egyikét.</span><span class="sxs-lookup"><span data-stu-id="65256-297">For example itemCount==10 means that of 10 calls too`trackTrace()`, hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="65256-298">a megfelelő nyomkövetési indított hívások száma tooget, kell ezért kódot használja, mint `traces | summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="65256-298">tooget a correct count of trace calls, you should use therefore code such as `traces | summarize sum(itemCount)`.</span></span>

## <a name="trackdependency"></a><span data-ttu-id="65256-299">TrackDependency</span><span class="sxs-lookup"><span data-stu-id="65256-299">TrackDependency</span></span>
<span data-ttu-id="65256-300">Használjon hello TrackDependency hívja tootrack hello válaszidejét és sikerességi arányát hívások tooan külső kódnak küldött kódot.</span><span class="sxs-lookup"><span data-stu-id="65256-300">Use hello TrackDependency call tootrack hello response times and success rates of calls tooan external piece of code.</span></span> <span data-ttu-id="65256-301">hello eredmények hello hello portál függőségi diagramjain jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="65256-301">hello results appear in hello dependency charts in hello portal.</span></span>

```C#
var success = false;
var startTime = DateTime.UtcNow;
var timer = System.Diagnostics.Stopwatch.StartNew();
try
{
    success = dependency.Call();
}
finally
{
    timer.Stop();
    telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
}
```

<span data-ttu-id="65256-302">Ne feledje SDK tartalmazza hello kiszolgálón egy [függőségi modul](app-insights-asp-net-dependencies.md) , amely észleli, és bizonyos függőségi hívások automatikusan – például toodatabases és REST API-k nyomon követi.</span><span class="sxs-lookup"><span data-stu-id="65256-302">Remember that hello server SDKs include a [dependency module](app-insights-asp-net-dependencies.md) that discovers and tracks certain dependency calls automatically--for example, toodatabases and REST APIs.</span></span> <span data-ttu-id="65256-303">Lehetősége van egy ügynök a kiszolgáló toomake hello modul tooinstall működik.</span><span class="sxs-lookup"><span data-stu-id="65256-303">You have tooinstall an agent on your server toomake hello module work.</span></span> <span data-ttu-id="65256-304">Ha automatikus követési hello tootrack hívások nem dolgozza fel, vagy ha nem szeretné, hogy tooinstall hello ügynök használhatja a hívást.</span><span class="sxs-lookup"><span data-stu-id="65256-304">You use this call if you want tootrack calls that hello automated tracking doesn't catch, or if you don't want tooinstall hello agent.</span></span>

<span data-ttu-id="65256-305">hello normál függőségi követése modul ki tooturn szerkesztése [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) és hello hivatkozás törlése túl`DependencyCollector.DependencyTrackingTelemetryModule`.</span><span class="sxs-lookup"><span data-stu-id="65256-305">tooturn off hello standard dependency-tracking module, edit [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) and delete hello reference too`DependencyCollector.DependencyTrackingTelemetryModule`.</span></span>

### <a name="dependencies-in-analytics"></a><span data-ttu-id="65256-306">Az elemzés függőségek</span><span class="sxs-lookup"><span data-stu-id="65256-306">Dependencies in Analytics</span></span>

<span data-ttu-id="65256-307">A [Application Insights Analytics](app-insights-analytics.md), hello megjelennek trackDependency hívások `dependencies` tábla.</span><span class="sxs-lookup"><span data-stu-id="65256-307">In [Application Insights Analytics](app-insights-analytics.md), trackDependency calls show up in hello `dependencies` table.</span></span>

<span data-ttu-id="65256-308">Ha [mintavételi](app-insights-sampling.md) működik, hello elemek tulajdonság jeleníti meg egy értéket 1-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="65256-308">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="65256-309">Példa az elemek száma a == 10 azt jelenti, hogy a 10 hívások tootrackDependency() hello mintavételi folyamat csak akkor továbbítódnak, ezek egyikét.</span><span class="sxs-lookup"><span data-stu-id="65256-309">For example itemCount==10 means that of 10 calls tootrackDependency(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="65256-310">tooget függőségek megfelelő száma szegmentált cél összetevő, például a kód használata:</span><span class="sxs-lookup"><span data-stu-id="65256-310">tooget a correct count of dependencies segmented by target component, use code such as:</span></span>

```
dependencies | summarize sum(itemCount) by target
```

<span data-ttu-id="65256-311">a kapcsolódó kérések tooassociate függőségek használjuk:</span><span class="sxs-lookup"><span data-stu-id="65256-311">tooassociate dependencies with their related requests, use a join:</span></span>

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a><span data-ttu-id="65256-312">Könyvelési adatok</span><span class="sxs-lookup"><span data-stu-id="65256-312">Flushing data</span></span>
<span data-ttu-id="65256-313">Általában a hello SDK néha kiválasztott toominimize hello gyakorolt hello felhasználói adatok küldése.</span><span class="sxs-lookup"><span data-stu-id="65256-313">Normally, hello SDK sends data at times chosen toominimize hello impact on hello user.</span></span> <span data-ttu-id="65256-314">Azonban bizonyos esetekben érdemes tooflush hello puffer – például egy alkalmazás, amely leállítja SDK hello használata.</span><span class="sxs-lookup"><span data-stu-id="65256-314">However, in some cases, you might want tooflush hello buffer--for example, if you are using hello SDK in an application that shuts down.</span></span>

<span data-ttu-id="65256-315">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-315">*C#*</span></span>

    telemetry.Flush();

    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(1000);

<span data-ttu-id="65256-316">Vegye figyelembe, hogy hello függvény hello az aszinkron [server telemetriai csatorna](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).</span><span class="sxs-lookup"><span data-stu-id="65256-316">Note that hello function is asynchronous for hello [server telemetry channel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).</span></span>

## <a name="authenticated-users"></a><span data-ttu-id="65256-317">Hitelesített felhasználók</span><span class="sxs-lookup"><span data-stu-id="65256-317">Authenticated users</span></span>
<span data-ttu-id="65256-318">A webes alkalmazás a felhasználók (alapértelmezés) azonosítja a cookie-k.</span><span class="sxs-lookup"><span data-stu-id="65256-318">In a web app, users are (by default) identified by cookies.</span></span> <span data-ttu-id="65256-319">Ha az alkalmazás hozzáférésük egy másik számítógép vagy a böngésző, vagy ha ezek a cookie-k törléséhez előfordulhat, hogy számba lehet vennni a felhasználó egynél többször.</span><span class="sxs-lookup"><span data-stu-id="65256-319">A user might be counted more than once if they access your app from a different machine or browser, or if they delete cookies.</span></span>

<span data-ttu-id="65256-320">A felhasználók bejelentkeznek tooyour alkalmazást, ha kaphat a pontosabb számát úgy, hogy hitelesített hello Felhasználóazonosító hello böngésző kódban:</span><span class="sxs-lookup"><span data-stu-id="65256-320">If users sign in tooyour app, you can get a more accurate count by setting hello authenticated user ID in hello browser code:</span></span>

<span data-ttu-id="65256-321">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="65256-321">*JavaScript*</span></span>

```JS
// Called when my app has identified hello user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

<span data-ttu-id="65256-322">Az ASP.NET-webalkalmazás MVC alkalmazás, például:</span><span class="sxs-lookup"><span data-stu-id="65256-322">In an ASP.NET web MVC application, for example:</span></span>

<span data-ttu-id="65256-323">*RAZOR*</span><span class="sxs-lookup"><span data-stu-id="65256-323">*Razor*</span></span>

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

<span data-ttu-id="65256-324">Nem szükséges toouse hello felhasználói tényleges bejelentkezési nevet.</span><span class="sxs-lookup"><span data-stu-id="65256-324">It isn't necessary toouse hello user's actual sign-in name.</span></span> <span data-ttu-id="65256-325">Csak rendelkezik toobe, amely egyedi toothat felhasználói Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="65256-325">It only has toobe an ID that is unique toothat user.</span></span> <span data-ttu-id="65256-326">Nem tartalmazhatnak szóközöket és hello karakterek `,;=|`.</span><span class="sxs-lookup"><span data-stu-id="65256-326">It must not include spaces or any of hello characters `,;=|`.</span></span>

<span data-ttu-id="65256-327">hello felhasználói azonosító is állítsa be a munkamenet-cookie-ban, és toohello kiszolgáló küldött.</span><span class="sxs-lookup"><span data-stu-id="65256-327">hello user ID is also set in a session cookie and sent toohello server.</span></span> <span data-ttu-id="65256-328">Ha hello server SDK telepítve van, a hello hitelesített felhasználói azonosító hello környezeti tulajdonságok az ügyfél- és telemetria részeként küldött.</span><span class="sxs-lookup"><span data-stu-id="65256-328">If hello server SDK is installed, hello authenticated user ID is sent as part of hello context properties of both client and server telemetry.</span></span> <span data-ttu-id="65256-329">Majd végezhet, és keresse meg azt.</span><span class="sxs-lookup"><span data-stu-id="65256-329">You can then filter and search on it.</span></span>

<span data-ttu-id="65256-330">Ha az alkalmazás felhasználók csoportosítja a fiókok, is átadhatja hello fiók azonosítója (a hello ugyanaz a karakter korlátozások).</span><span class="sxs-lookup"><span data-stu-id="65256-330">If your app groups users into accounts, you can also pass an identifier for hello account (with hello same character restrictions).</span></span>

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

<span data-ttu-id="65256-331">A [Metrikaböngésző](app-insights-metrics-explorer.md), létrehozhat egy diagram, amely megjeleníti **hitelesített felhasználók,**, és **felhasználói fiókok**.</span><span class="sxs-lookup"><span data-stu-id="65256-331">In [Metrics Explorer](app-insights-metrics-explorer.md), you can create a chart that counts **Users, Authenticated**, and **User accounts**.</span></span>

<span data-ttu-id="65256-332">Emellett [keresési](app-insights-diagnostic-search.md) az ügyfél adatpontok adott felhasználói neveket és fiókok.</span><span class="sxs-lookup"><span data-stu-id="65256-332">You can also [search](app-insights-diagnostic-search.md) for client data points with specific user names and accounts.</span></span>

## <span data-ttu-id="65256-333"><a name="properties"></a>Szűrés, keresést és az adatok példájaként tulajdonságok felhasználásával</span><span class="sxs-lookup"><span data-stu-id="65256-333"><a name="properties"></a>Filtering, searching, and segmenting your data by using properties</span></span>
<span data-ttu-id="65256-334">Tulajdonságok és mérések tooyour események (és is toometrics, Lapmegtekintések, kivételeket és egyéb telemetriai adatokat) csatolhat.</span><span class="sxs-lookup"><span data-stu-id="65256-334">You can attach properties and measurements tooyour events (and also toometrics, page views, exceptions, and other telemetry data).</span></span>

<span data-ttu-id="65256-335">*Tulajdonságok* használható toofilter a telemetriai adatokat a használati jelentésekben hello karakterlánc-értékek.</span><span class="sxs-lookup"><span data-stu-id="65256-335">*Properties* are string values that you can use toofilter your telemetry in hello usage reports.</span></span> <span data-ttu-id="65256-336">Például ha az alkalmazás biztosít több játékok, csatolhat hello játék tooeach esemény hello neve, hogy láthassa, melyik játékok népszerűbbnek.</span><span class="sxs-lookup"><span data-stu-id="65256-336">For example, if your app provides several games, you can attach hello name of hello game tooeach event so that you can see which games are more popular.</span></span>

<span data-ttu-id="65256-337">A karakterlánc hossza hello 8192 korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="65256-337">There's a limit of 8192 on hello string length.</span></span> <span data-ttu-id="65256-338">(Ha azt szeretné, hogy toosend nagy méretű adattömböket írnak, használja a hello üzenet paramétert a [TrackTrace](#track-trace).)</span><span class="sxs-lookup"><span data-stu-id="65256-338">(If you want toosend large chunks of data, use hello message parameter of [TrackTrace](#track-trace).)</span></span>

<span data-ttu-id="65256-339">*Metrikák* számértékek jelenítheti meg grafikusan.</span><span class="sxs-lookup"><span data-stu-id="65256-339">*Metrics* are numeric values that can be presented graphically.</span></span> <span data-ttu-id="65256-340">Érdemes például toosee fokozatos növekedése hello eredmények, amelyek a gamers elérése esetén.</span><span class="sxs-lookup"><span data-stu-id="65256-340">For example, you might want toosee if there's a gradual increase in hello scores that your gamers achieve.</span></span> <span data-ttu-id="65256-341">hello grafikonon is lehet szegmentált által küldött hello eseménnyel, hogy megkaphassa tulajdonságok külön hello, vagy a halmozott diagramok a különböző játékok számára.</span><span class="sxs-lookup"><span data-stu-id="65256-341">hello graphs can be segmented by hello properties that are sent with hello event, so that you can get separate or stacked graphs for different games.</span></span>

<span data-ttu-id="65256-342">A metrika értékek toobe megfelelően jelenik meg akkor nagyobb vagy egyenlő too0 kell lennie.</span><span class="sxs-lookup"><span data-stu-id="65256-342">For metric values toobe correctly displayed, they should be greater than or equal too0.</span></span>

<span data-ttu-id="65256-343">Van azonban néhány [tulajdonságait, a tulajdonságértékeket és a metrikák hello száma vonatkozó korlátozások](#limits) használható.</span><span class="sxs-lookup"><span data-stu-id="65256-343">There are some [limits on hello number of properties, property values, and metrics](#limits) that you can use.</span></span>

<span data-ttu-id="65256-344">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="65256-344">*JavaScript*</span></span>

    appInsights.trackEvent
      ("WinGame",
         // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );

    appInsights.trackPageView
        ("page name", "http://fabrikam.com/pageurl.html",
          // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );


<span data-ttu-id="65256-345">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-345">*C#*</span></span>

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics);


<span data-ttu-id="65256-346">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="65256-346">*Visual Basic*</span></span>

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics)


<span data-ttu-id="65256-347">*Java*</span><span class="sxs-lookup"><span data-stu-id="65256-347">*Java*</span></span>

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> <span data-ttu-id="65256-348">Mi gondoskodunk nem toolog személyazonosításra alkalmas adatok tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="65256-348">Take care not toolog personally identifiable information in properties.</span></span>
>
>

<span data-ttu-id="65256-349">*Ha követte a metrikák*, nyissa meg a Metrikaböngésző, majd válasszon hello metrika hello **egyéni** csoport:</span><span class="sxs-lookup"><span data-stu-id="65256-349">*If you used metrics*, open Metrics Explorer and select hello metric from hello **Custom** group:</span></span>

![Nyissa meg a Metrikaböngésző, jelölje be hello diagram, és válassza ki a hello metrika](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> <span data-ttu-id="65256-351">Ha a metrika nem jelenik meg, vagy ha hello **egyéni** fejléc nem létezik, Bezárás hello kijelölés panelt, és próbálkozzon újra később.</span><span class="sxs-lookup"><span data-stu-id="65256-351">If your metric doesn't appear, or if hello **Custom** heading isn't there, close hello selection blade and try again later.</span></span> <span data-ttu-id="65256-352">Metrikák néha időigényes egy órával toobe összesítve hello-feldolgozási folyamaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="65256-352">Metrics can sometimes take an hour toobe aggregated through hello pipeline.</span></span>

<span data-ttu-id="65256-353">*Ha követte a tulajdonságok és a metrikák*, hello metrika szegmentálja hello tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="65256-353">*If you used properties and metrics*, segment hello metric by hello property:</span></span>

![Állítsa be a csoportosítás, és válassza a hello tulajdonság szerinti csoportosítás](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

<span data-ttu-id="65256-355">*A diagnosztikai keresési*, megtekintheti a hello tulajdonságok és az esemény egyedi előfordulása.</span><span class="sxs-lookup"><span data-stu-id="65256-355">*In Diagnostic Search*, you can view hello properties and metrics of individual occurrences of an event.</span></span>

![Válasszon ki egy példányt, és adja meg a "..."](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

<span data-ttu-id="65256-357">Használjon hello **keresési** mezőben toosee esemény eseményeket, amelyek egy adott tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="65256-357">Use hello **Search** field toosee event occurrences that have a particular property value.</span></span>

![Írja be a keresőkifejezést keresési rendszerbe](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

<span data-ttu-id="65256-359">[További információ a keresési kifejezések](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="65256-359">[Learn more about search expressions](app-insights-diagnostic-search.md).</span></span>

### <a name="alternative-way-tooset-properties-and-metrics"></a><span data-ttu-id="65256-360">Alternatív módot tooset tulajdonságok és metrikák</span><span class="sxs-lookup"><span data-stu-id="65256-360">Alternative way tooset properties and metrics</span></span>
<span data-ttu-id="65256-361">Sokkal kényelmesebb, ha egy külön objektumban esemény hello paraméterek hozhatja létre:</span><span class="sxs-lookup"><span data-stu-id="65256-361">If it's more convenient, you can collect hello parameters of an event in a separate object:</span></span>

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> <span data-ttu-id="65256-362">Ne használja fel a hello telemetriai elem példányt (`event` ebben a példában) toocall Track*() több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="65256-362">Don't reuse hello same telemetry item instance (`event` in this example) toocall Track*() multiple times.</span></span> <span data-ttu-id="65256-363">Emiatt a helytelen konfiguráció küldött telemetriai toobe.</span><span class="sxs-lookup"><span data-stu-id="65256-363">This may cause telemetry toobe sent with incorrect configuration.</span></span>
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a><span data-ttu-id="65256-364">Egyéni mértékek és az elemzés tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="65256-364">Custom measurements and properties in Analytics</span></span>

<span data-ttu-id="65256-365">A [Analytics](app-insights-analytics.md), egyéni metrikákkal és tulajdonságok megjelenítése hello `customMeasurements` és `customDimensions` telemetriai rekordokban attribútumait.</span><span class="sxs-lookup"><span data-stu-id="65256-365">In [Analytics](app-insights-analytics.md), custom metrics and properties show in hello `customMeasurements` and `customDimensions` attributes of each telemetry record.</span></span>

<span data-ttu-id="65256-366">Például hozzáadta "játék" tooyour – kéréstelemetria nevű tulajdonságot, ha a lekérdezés különböző értékek "játék" hello előfordulását száma, és egyéni metrika "pontszám" hello hello átlaga megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="65256-366">For example, if you have added a property named "game" tooyour request telemetry, this query counts hello occurrences of different values of "game", and show hello average of hello custom metric "score":</span></span>

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

<span data-ttu-id="65256-367">Figyelje meg, hogy:</span><span class="sxs-lookup"><span data-stu-id="65256-367">Notice that:</span></span>

* <span data-ttu-id="65256-368">Egy érték hello customDimensions vagy customMeasurements JSON kibontásakor dinamikus típust, és úgy kell alakítania azt `tostring` vagy `todouble`.</span><span class="sxs-lookup"><span data-stu-id="65256-368">When you extract a value from hello customDimensions or customMeasurements JSON, it has dynamic type, and so you must cast it `tostring` or `todouble`.</span></span>
* <span data-ttu-id="65256-369">hello lehetőségét figyelembe tootake [mintavételi](app-insights-sampling.md), használjon `sum(itemCount)`, nem `count()`.</span><span class="sxs-lookup"><span data-stu-id="65256-369">tootake account of hello possibility of [sampling](app-insights-sampling.md), you should use `sum(itemCount)`, not `count()`.</span></span>



## <span data-ttu-id="65256-370"><a name="timed"></a>Időzítési események</span><span class="sxs-lookup"><span data-stu-id="65256-370"><a name="timed"></a> Timing events</span></span>
<span data-ttu-id="65256-371">Néha szükség toochart mennyi ideig tart tooperform a műveletet.</span><span class="sxs-lookup"><span data-stu-id="65256-371">Sometimes you want toochart how long it takes tooperform an action.</span></span> <span data-ttu-id="65256-372">Például érdemes tooknow mennyi ideig felhasználók el egy játékban tooconsider lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="65256-372">For example, you might want tooknow how long users take tooconsider choices in a game.</span></span> <span data-ttu-id="65256-373">Ehhez használhatja hello mérési paraméter.</span><span class="sxs-lookup"><span data-stu-id="65256-373">You can use hello measurement parameter for this.</span></span>

<span data-ttu-id="65256-374">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-374">*C#*</span></span>

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform hello timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send hello event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);



## <span data-ttu-id="65256-375"><a name="defaults"></a>Egyéni telemetria alapértelmezett tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="65256-375"><a name="defaults"></a>Default properties for custom telemetry</span></span>
<span data-ttu-id="65256-376">Ha tooset alapértelmezett tulajdonságértékek egyes hello egyéni események írást, akkor TelemetryClient példányt teheti meg.</span><span class="sxs-lookup"><span data-stu-id="65256-376">If you want tooset default property values for some of hello custom events that you write, you can set them in a TelemetryClient instance.</span></span> <span data-ttu-id="65256-377">Csatolt tooevery telemetriai elemet, hogy az ügyfél által küldött.</span><span class="sxs-lookup"><span data-stu-id="65256-377">They are attached tooevery telemetry item that's sent from that client.</span></span>

<span data-ttu-id="65256-378">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-378">*C#*</span></span>

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame");

<span data-ttu-id="65256-379">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="65256-379">*Visual Basic*</span></span>

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame")

<span data-ttu-id="65256-380">*Java*</span><span class="sxs-lookup"><span data-stu-id="65256-380">*Java*</span></span>

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");



<span data-ttu-id="65256-381">Egyéni telemetria hívások hello az alapértelmezett érték a tulajdonság szótár is felülírására.</span><span class="sxs-lookup"><span data-stu-id="65256-381">Individual telemetry calls can override hello default values in their property dictionaries.</span></span>

<span data-ttu-id="65256-382">*JavaScript a webes ügyfelek*, [JavaScript telemetriai inicializálók használja](#js-initializer).</span><span class="sxs-lookup"><span data-stu-id="65256-382">*For JavaScript web clients*, [use JavaScript telemetry initializers](#js-initializer).</span></span>

<span data-ttu-id="65256-383">*tooadd tulajdonságok tooall telemetriai*, beleértve a szabványos gyűjtési modulok hello adatait [megvalósítása `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).</span><span class="sxs-lookup"><span data-stu-id="65256-383">*tooadd properties tooall telemetry*, including hello data from standard collection modules, [implement `ITelemetryInitializer`](app-insights-api-filtering-sampling.md#add-properties).</span></span>

## <a name="sampling-filtering-and-processing-telemetry"></a><span data-ttu-id="65256-384">A mintavételi, szűrési és telemetriai adatainak feldolgozása</span><span class="sxs-lookup"><span data-stu-id="65256-384">Sampling, filtering, and processing telemetry</span></span>
<span data-ttu-id="65256-385">Írhat kódot tooprocess a telemetriai adatok hello SDK való továbbítás előtt.</span><span class="sxs-lookup"><span data-stu-id="65256-385">You can write code tooprocess your telemetry before it's sent from hello SDK.</span></span> <span data-ttu-id="65256-386">hello feldolgozási hello szabványos telemetriai modulok, például HTTP kérelem adatgyűjtési és -függőség gyűjtemény által küldött adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="65256-386">hello processing includes data that's sent from hello standard telemetry modules, such as HTTP request collection and dependency collection.</span></span>

<span data-ttu-id="65256-387">[Adja hozzá a Tulajdonságok](app-insights-api-filtering-sampling.md#add-properties) implementálásával tootelemetry `ITelemetryInitializer`.</span><span class="sxs-lookup"><span data-stu-id="65256-387">[Add properties](app-insights-api-filtering-sampling.md#add-properties) tootelemetry by implementing `ITelemetryInitializer`.</span></span> <span data-ttu-id="65256-388">Például hozzáadhat verziószámok vagy számított értékeket más tulajdonságai közül.</span><span class="sxs-lookup"><span data-stu-id="65256-388">For example, you can add version numbers or values that are calculated from other properties.</span></span>

<span data-ttu-id="65256-389">[Szűrés](app-insights-api-filtering-sampling.md#filtering) módosíthatja vagy elveti a telemetriai adatok elküldés előtt a hello SDK implementálásával `ITelemetryProcesor`.</span><span class="sxs-lookup"><span data-stu-id="65256-389">[Filtering](app-insights-api-filtering-sampling.md#filtering) can modify or discard telemetry before it's sent from hello SDK by implementing `ITelemetryProcesor`.</span></span> <span data-ttu-id="65256-390">Megadhatja, mi küldik vagy elvetett, de a metrikákat hello hatással a tooaccount rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="65256-390">You control what is sent or discarded, but you have tooaccount for hello effect on your metrics.</span></span> <span data-ttu-id="65256-391">Attól függően, hogy hogyan elveti elemek elveszhetnek hello képességét toonavigate kapcsolódó elemek között.</span><span class="sxs-lookup"><span data-stu-id="65256-391">Depending on how you discard items, you might lose hello ability toonavigate between related items.</span></span>

<span data-ttu-id="65256-392">[A mintavételi](app-insights-api-filtering-sampling.md) csomagolt megoldás tooreduce hello kötet az alkalmazás toohello portál által küldött adatokat.</span><span class="sxs-lookup"><span data-stu-id="65256-392">[Sampling](app-insights-api-filtering-sampling.md) is a packaged solution tooreduce hello volume of data that's sent from your app toohello portal.</span></span> <span data-ttu-id="65256-393">Igen, az nem befolyásolja a megjelenő hello metrikákat.</span><span class="sxs-lookup"><span data-stu-id="65256-393">It does so without affecting hello displayed metrics.</span></span> <span data-ttu-id="65256-394">És az hajtja végre, például kivételek, a kérelmek és az oldalmegtekintéseket kapcsolódó elemek közötti navigáljon a képes toodiagnose problémák befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="65256-394">And it does so without affecting your ability toodiagnose problems by navigating between related items such as exceptions, requests, and page views.</span></span>

<span data-ttu-id="65256-395">[További információk](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="65256-395">[Learn more](app-insights-api-filtering-sampling.md).</span></span>

## <a name="disabling-telemetry"></a><span data-ttu-id="65256-396">Telemetria letiltása</span><span class="sxs-lookup"><span data-stu-id="65256-396">Disabling telemetry</span></span>
<span data-ttu-id="65256-397">túl*dinamikusan leállítására és elindítására* hello összegyűjtése és telemetriai adatok továbbítása:</span><span class="sxs-lookup"><span data-stu-id="65256-397">too*dynamically stop and start* hello collection and transmission of telemetry:</span></span>

<span data-ttu-id="65256-398">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-398">*C#*</span></span>

```C#

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

<span data-ttu-id="65256-399">túl*tiltsa le a kiválasztott szabványos gyűjtők*– például teljesítményszámlálókat, HTTP-kérelmek vagy függőségek--törlése vagy hello megfelelő sorok megjegyzéssé [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Ehhez, például ha toosend saját TrackRequest adatokat.</span><span class="sxs-lookup"><span data-stu-id="65256-399">too*disable selected standard collectors*--for example, performance counters, HTTP requests, or dependencies--delete or comment out hello relevant lines in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). You can do this, for example, if you want toosend your own TrackRequest data.</span></span>

## <span data-ttu-id="65256-400"><a name="debug"></a>Fejlesztői mód</span><span class="sxs-lookup"><span data-stu-id="65256-400"><a name="debug"></a>Developer mode</span></span>
<span data-ttu-id="65256-401">Hibakeresés során a rendszer hasznos toohave a telemetriai adatok sürgős hello-feldolgozási folyamaton keresztül, hogy az eredmények azonnal láthatók.</span><span class="sxs-lookup"><span data-stu-id="65256-401">During debugging, it's useful toohave your telemetry expedited through hello pipeline so that you can see results immediately.</span></span> <span data-ttu-id="65256-402">Akkor is get további üzeneteket, amelyek segítségével nyomon követni a hello telemetriai problémákat.</span><span class="sxs-lookup"><span data-stu-id="65256-402">You also get additional messages that help you trace any problems with hello telemetry.</span></span> <span data-ttu-id="65256-403">Kapcsolja ki a termelési, mert az alkalmazás lassíthatja.</span><span class="sxs-lookup"><span data-stu-id="65256-403">Switch it off in production, because it may slow down your app.</span></span>

<span data-ttu-id="65256-404">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-404">*C#*</span></span>

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

<span data-ttu-id="65256-405">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="65256-405">*Visual Basic*</span></span>

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <span data-ttu-id="65256-406"><a name="ikey"></a>A kijelölt egyéni telemetria hello instrumentation kulcs beállítása</span><span class="sxs-lookup"><span data-stu-id="65256-406"><a name="ikey"></a> Setting hello instrumentation key for selected custom telemetry</span></span>
<span data-ttu-id="65256-407">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-407">*C#*</span></span>

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <span data-ttu-id="65256-408"><a name="dynamic-ikey"></a>Dinamikus instrumentation kulcs</span><span class="sxs-lookup"><span data-stu-id="65256-408"><a name="dynamic-ikey"></a> Dynamic instrumentation key</span></span>
<span data-ttu-id="65256-409">felfelé telemetriai fejlesztési, tesztelési és éles környezetben, a keverése tooavoid is [hozzon létre külön Application Insights-erőforrások](app-insights-create-new-resource.md) és azok kulcsait hello környezettől függően módosítsa.</span><span class="sxs-lookup"><span data-stu-id="65256-409">tooavoid mixing up telemetry from development, test, and production environments, you can [create separate Application Insights resources](app-insights-create-new-resource.md) and change their keys, depending on hello environment.</span></span>

<span data-ttu-id="65256-410">Helyett hello instrumentation kulcs lekérése hello konfigurációs fájlt, beállíthatja a kódban.</span><span class="sxs-lookup"><span data-stu-id="65256-410">Instead of getting hello instrumentation key from hello configuration file, you can set it in your code.</span></span> <span data-ttu-id="65256-411">Egy inicializálási metódust, például egy ASP.NET-szolgáltatásban Global.aspx.cs osztályból hello kulcs meg:</span><span class="sxs-lookup"><span data-stu-id="65256-411">Set hello key in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

<span data-ttu-id="65256-412">*C#*</span><span class="sxs-lookup"><span data-stu-id="65256-412">*C#*</span></span>

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

<span data-ttu-id="65256-413">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="65256-413">*JavaScript*</span></span>

    appInsights.config.instrumentationKey = myKey;



<span data-ttu-id="65256-414">A weboldalakon, érdemes lehet a tooset azt hello webkiszolgáló állapota, nem pedig szó kódolási hello parancsfájlba.</span><span class="sxs-lookup"><span data-stu-id="65256-414">In webpages, you might want tooset it from hello web server's state, rather than coding it literally into hello script.</span></span> <span data-ttu-id="65256-415">Például a egy weblap ASP.NET alkalmazás hozott létre:</span><span class="sxs-lookup"><span data-stu-id="65256-415">For example, in a webpage generated in an ASP.NET app:</span></span>

<span data-ttu-id="65256-416">*JavaScript Razor*</span><span class="sxs-lookup"><span data-stu-id="65256-416">*JavaScript in Razor*</span></span>

    <script type="text/javascript">
    // Standard Application Insights webpage script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="telemetrycontext"></a><span data-ttu-id="65256-417">TelemetryContext</span><span class="sxs-lookup"><span data-stu-id="65256-417">TelemetryContext</span></span>
<span data-ttu-id="65256-418">TelemetryClient rendelkezik egy környezeti tulajdonság, amely az összes telemetriai adatokkal együtt küldött értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="65256-418">TelemetryClient has a Context property, which contains values that are sent along with all telemetry data.</span></span> <span data-ttu-id="65256-419">Általában állította hello szabványos telemetriai modulok, de is beállíthatja azokat saját maga.</span><span class="sxs-lookup"><span data-stu-id="65256-419">They are normally set by hello standard telemetry modules, but you can also set them yourself.</span></span> <span data-ttu-id="65256-420">Példa:</span><span class="sxs-lookup"><span data-stu-id="65256-420">For example:</span></span>

    telemetry.Context.Operation.Name = "MyOperationName";

<span data-ttu-id="65256-421">Ha ezek bármelyike saját magának, fontolja meg a hello a megfelelő sor eltávolítása [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), így a és hello szabványos értékek nem Zavarba.</span><span class="sxs-lookup"><span data-stu-id="65256-421">If you set any of these values yourself, consider removing hello relevant line from [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), so that your values and hello standard values don't get confused.</span></span>

* <span data-ttu-id="65256-422">**Az összetevő**: hello alkalmazást és annak verzióját.</span><span class="sxs-lookup"><span data-stu-id="65256-422">**Component**: hello app and its version.</span></span>
* <span data-ttu-id="65256-423">**Eszköz**: hello alkalmazást futtató hello eszközzel kapcsolatos adatok.</span><span class="sxs-lookup"><span data-stu-id="65256-423">**Device**: Data about hello device where hello app is running.</span></span> <span data-ttu-id="65256-424">(A webalkalmazásokban, ez a hello vagy hello telemetriai által küldött ügyfél-eszközkövetelmények.)</span><span class="sxs-lookup"><span data-stu-id="65256-424">(In web apps, this is hello server or client device that hello telemetry is sent from.)</span></span>
* <span data-ttu-id="65256-425">**InstrumentationKey**: hello Application Insights-erőforrást, ahol a hello telemetriai adatok jelennek meg az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="65256-425">**InstrumentationKey**: hello Application Insights resource in Azure where hello telemetry appear.</span></span> <span data-ttu-id="65256-426">Ez általában felvételre az ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="65256-426">It's usually picked up from ApplicationInsights.config.</span></span>
* <span data-ttu-id="65256-427">**Hely**: hello hello eszköz földrajzi helye.</span><span class="sxs-lookup"><span data-stu-id="65256-427">**Location**: hello geographic location of hello device.</span></span>
* <span data-ttu-id="65256-428">**A művelet**: A webalkalmazásokban, hello aktuális HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="65256-428">**Operation**: In web apps, hello current HTTP request.</span></span> <span data-ttu-id="65256-429">Más típusú alkalmazás állíthat be a toogroup események együtt.</span><span class="sxs-lookup"><span data-stu-id="65256-429">In other app types, you can set this toogroup events together.</span></span>
  * <span data-ttu-id="65256-430">**Azonosító**: egy generált érték különböző események hibához, így minden esetben a diagnosztikai keresési vizsgálja meg, ha található kapcsolódó elemeket.</span><span class="sxs-lookup"><span data-stu-id="65256-430">**Id**: A generated value that correlates different events, so that when you inspect any event in Diagnostic Search, you can find related items.</span></span>
  * <span data-ttu-id="65256-431">**Név**: azonosítót, általában hello URL-cím hello HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="65256-431">**Name**: An identifier, usually hello URL of hello HTTP request.</span></span>
  * <span data-ttu-id="65256-432">**SyntheticSource**: Ha nem null értékű vagy üres, karakterlánc, amely azt jelzi, hogy hello adatforrás hello kérelem néven azonosított egy robot vagy webes tesztet.</span><span class="sxs-lookup"><span data-stu-id="65256-432">**SyntheticSource**: If not null or empty, a string that indicates that hello source of hello request has been identified as a robot or web test.</span></span> <span data-ttu-id="65256-433">Alapértelmezés szerint az ki lesz zárva a Metrikaböngészőben számításból.</span><span class="sxs-lookup"><span data-stu-id="65256-433">By default, it is excluded from calculations in Metrics Explorer.</span></span>
* <span data-ttu-id="65256-434">**Tulajdonságok**: az összes telemetriai adatokat küldött tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="65256-434">**Properties**: Properties that are sent with all telemetry data.</span></span> <span data-ttu-id="65256-435">Az egyes követése * hívások felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="65256-435">It can be overridden in individual Track* calls.</span></span>
* <span data-ttu-id="65256-436">**Munkamenet**: hello felhasználói munkamenet.</span><span class="sxs-lookup"><span data-stu-id="65256-436">**Session**: hello user's session.</span></span> <span data-ttu-id="65256-437">hello azonosító értéke tooa generált érték, amely megváltozik, miközben hello felhasználó nem volt aktív egy ideig.</span><span class="sxs-lookup"><span data-stu-id="65256-437">hello ID is set tooa generated value, which is changed when hello user has not been active for a while.</span></span>
* <span data-ttu-id="65256-438">**Felhasználói**: felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="65256-438">**User**: User information.</span></span>

## <a name="limits"></a><span data-ttu-id="65256-439">Korlátok</span><span class="sxs-lookup"><span data-stu-id="65256-439">Limits</span></span>
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

<span data-ttu-id="65256-440">Szerezze meg a hello adatok sávszélesség-korlátjának, használjon tooavoid [mintavételi](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="65256-440">tooavoid hitting hello data rate limit, use [sampling](app-insights-sampling.md).</span></span>

<span data-ttu-id="65256-441">toodetermine hogyan tartják hosszú adatokat, lásd: [az adatmegőrzés és az adatvédelmi](app-insights-data-retention-privacy.md).</span><span class="sxs-lookup"><span data-stu-id="65256-441">toodetermine how long data is kept, see [Data retention and privacy](app-insights-data-retention-privacy.md).</span></span>

## <a name="reference-docs"></a><span data-ttu-id="65256-442">Segédanyagok</span><span class="sxs-lookup"><span data-stu-id="65256-442">Reference docs</span></span>
* [<span data-ttu-id="65256-443">ASP.NET-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="65256-443">ASP.NET reference</span></span>](https://msdn.microsoft.com/library/dn817570.aspx)
* [<span data-ttu-id="65256-444">Java dokumentáció</span><span class="sxs-lookup"><span data-stu-id="65256-444">Java reference</span></span>](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [<span data-ttu-id="65256-445">JavaScript-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="65256-445">JavaScript reference</span></span>](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [<span data-ttu-id="65256-446">Android SDK</span><span class="sxs-lookup"><span data-stu-id="65256-446">Android SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Android)
* [<span data-ttu-id="65256-447">iOS SDK</span><span class="sxs-lookup"><span data-stu-id="65256-447">iOS SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a><span data-ttu-id="65256-448">SDK-kód</span><span class="sxs-lookup"><span data-stu-id="65256-448">SDK code</span></span>
* [<span data-ttu-id="65256-449">Az ASP.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="65256-449">ASP.NET Core SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [<span data-ttu-id="65256-450">ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="65256-450">ASP.NET 5</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [<span data-ttu-id="65256-451">Windows Server csomagok</span><span class="sxs-lookup"><span data-stu-id="65256-451">Windows Server packages</span></span>](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [<span data-ttu-id="65256-452">Java SDK</span><span class="sxs-lookup"><span data-stu-id="65256-452">Java SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Java)
* [<span data-ttu-id="65256-453">JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="65256-453">JavaScript SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-JS)
* [<span data-ttu-id="65256-454">Összes platform</span><span class="sxs-lookup"><span data-stu-id="65256-454">All platforms</span></span>](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a><span data-ttu-id="65256-455">Kérdések</span><span class="sxs-lookup"><span data-stu-id="65256-455">Questions</span></span>
* <span data-ttu-id="65256-456">*Milyen kivételek előfordulhat, hogy throw Track_() hívások?*</span><span class="sxs-lookup"><span data-stu-id="65256-456">*What exceptions might Track_() calls throw?*</span></span>

    <span data-ttu-id="65256-457">nincs.</span><span class="sxs-lookup"><span data-stu-id="65256-457">None.</span></span> <span data-ttu-id="65256-458">Toowrap nem kell őket try-catch záradékban.</span><span class="sxs-lookup"><span data-stu-id="65256-458">You don't need toowrap them in try-catch clauses.</span></span> <span data-ttu-id="65256-459">Ha hello SDK problémákat tapasztal, üzenetek naplózza a hello hibakeresési konzol kimeneti és – ha hello üzenetek lekérése keresztül--diagnosztikai keresési.</span><span class="sxs-lookup"><span data-stu-id="65256-459">If hello SDK encounters problems, it will log messages in hello debug console output and--if hello messages get through--in Diagnostic Search.</span></span>
* <span data-ttu-id="65256-460">*Van egy REST API tooget adatok hello portálról?*</span><span class="sxs-lookup"><span data-stu-id="65256-460">*Is there a REST API tooget data from hello portal?*</span></span>

    <span data-ttu-id="65256-461">Igen, hello [adatelérési API](https://dev.applicationinsights.io/).</span><span class="sxs-lookup"><span data-stu-id="65256-461">Yes, hello [data access API](https://dev.applicationinsights.io/).</span></span> <span data-ttu-id="65256-462">Más módokon tooextract adatok közé tartoznak a [Analytics tooPower BI exportálása](app-insights-export-power-bi.md) és [a folyamatos exportálás](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="65256-462">Other ways tooextract data include [export from Analytics tooPower BI](app-insights-export-power-bi.md) and [continuous export](app-insights-export-telemetry.md).</span></span>

## <span data-ttu-id="65256-463"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65256-463"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="65256-464">Keresési események és a naplókat</span><span class="sxs-lookup"><span data-stu-id="65256-464">Search events and logs</span></span>](app-insights-diagnostic-search.md)

* [<span data-ttu-id="65256-465">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="65256-465">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)



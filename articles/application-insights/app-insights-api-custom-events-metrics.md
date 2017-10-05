---
title: "Application Insights API egyéni események és metrikák |} Microsoft Docs"
description: "Néhány sornyi kód beszúrása a eszköz- vagy asztali alkalmazások, a képernyőn látható weblapon vagy a szolgáltatás használatának nyomon követése és eseményadatokat."
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
ms.openlocfilehash: e94c50de51612243386d89c5e0b3178a4f9cbd38
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a><span data-ttu-id="d6743-103">Application Insights API egyéni események és metrikák</span><span class="sxs-lookup"><span data-stu-id="d6743-103">Application Insights API for custom events and metrics</span></span>

<span data-ttu-id="d6743-104">Helyezze be néhány sornyi kódot az alkalmazásban, ha szeretné tudni, a felhasználók tevékenységeit vele, vagy problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="d6743-104">Insert a few lines of code in your application to find out what users are doing with it, or to help diagnose issues.</span></span> <span data-ttu-id="d6743-105">Eszköz- és asztali alkalmazások, a webes ügyfelekkel és a webkiszolgálók telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="d6743-105">You can send telemetry from device and desktop apps, web clients, and web servers.</span></span> <span data-ttu-id="d6743-106">Használja a [Azure Application Insights](app-insights-overview.md) alapvető telemetriai API küldése egyéni események és metrikák és a saját szabványos telemetriai verziói.</span><span class="sxs-lookup"><span data-stu-id="d6743-106">Use the [Azure Application Insights](app-insights-overview.md) core telemetry API to send custom events and metrics, and your own versions of standard telemetry.</span></span> <span data-ttu-id="d6743-107">Ez az API az azonos API-t a szabványos Application Insights-adatgyűjtők használó.</span><span class="sxs-lookup"><span data-stu-id="d6743-107">This API is the same API that the standard Application Insights data collectors use.</span></span>

## <a name="api-summary"></a><span data-ttu-id="d6743-108">API összefoglaló</span><span class="sxs-lookup"><span data-stu-id="d6743-108">API summary</span></span>
<span data-ttu-id="d6743-109">Az API-t egységes néhány kisebb eltérések mellett minden platformon.</span><span class="sxs-lookup"><span data-stu-id="d6743-109">The API is uniform across all platforms, apart from a few small variations.</span></span>

| <span data-ttu-id="d6743-110">Módszer</span><span class="sxs-lookup"><span data-stu-id="d6743-110">Method</span></span> | <span data-ttu-id="d6743-111">A használt</span><span class="sxs-lookup"><span data-stu-id="d6743-111">Used for</span></span> |
| --- | --- |
| [`TrackPageView`](#page-views) |<span data-ttu-id="d6743-112">Lapok, képernyők, paneleken vagy űrlap.</span><span class="sxs-lookup"><span data-stu-id="d6743-112">Pages, screens, blades, or forms.</span></span> |
| [`TrackEvent`](#trackevent) |<span data-ttu-id="d6743-113">Felhasználói műveletek és más események.</span><span class="sxs-lookup"><span data-stu-id="d6743-113">User actions and other events.</span></span> <span data-ttu-id="d6743-114">Vagy a felhasználó viselkedésének nyomon, vagy teljesítményének figyelésére használható.</span><span class="sxs-lookup"><span data-stu-id="d6743-114">Used to track user behavior or to monitor performance.</span></span> |
| [`TrackMetric`](#trackmetric) |<span data-ttu-id="d6743-115">Például a várólista-hosszúságok meggátolják nem kapcsolódik az adott események TELJESÍTMÉNYMÉRÉSEK.</span><span class="sxs-lookup"><span data-stu-id="d6743-115">Performance measurements such as queue lengths not related to specific events.</span></span> |
| [`TrackException`](#trackexception) |<span data-ttu-id="d6743-116">Naplózási kivételek elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="d6743-116">Logging exceptions for diagnosis.</span></span> <span data-ttu-id="d6743-117">Nyomkövetés, ahonnan merülnek fel az eseményeket és vizsgálja meg a híváslánc megjelenik-e.</span><span class="sxs-lookup"><span data-stu-id="d6743-117">Trace where they occur in relation to other events and examine stack traces.</span></span> |
| [`TrackRequest`](#trackrequest) |<span data-ttu-id="d6743-118">A naplózás a gyakoriság és a kiszolgálói kérelmek Teljesítményelemzési célokat időtartamát.</span><span class="sxs-lookup"><span data-stu-id="d6743-118">Logging the frequency and duration of server requests for performance analysis.</span></span> |
| [`TrackTrace`](#tracktrace) |<span data-ttu-id="d6743-119">Diagnosztikai naplózás.</span><span class="sxs-lookup"><span data-stu-id="d6743-119">Diagnostic log messages.</span></span> <span data-ttu-id="d6743-120">Külső naplókat is rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="d6743-120">You can also capture third-party logs.</span></span> |
| [`TrackDependency`](#trackdependency) |<span data-ttu-id="d6743-121">A naplózás a telepítések időtartamát és az alkalmazás függ külső összetevőket hívások gyakorisága.</span><span class="sxs-lookup"><span data-stu-id="d6743-121">Logging the duration and frequency of calls to external components that your app depends on.</span></span> |

<span data-ttu-id="d6743-122">Is [tulajdonságok és metrikákat](#properties) telemetriai hívásokat a legtöbb.</span><span class="sxs-lookup"><span data-stu-id="d6743-122">You can [attach properties and metrics](#properties) to most of these telemetry calls.</span></span>

## <span data-ttu-id="d6743-123"><a name="prep"></a>Előkészületek</span><span class="sxs-lookup"><span data-stu-id="d6743-123"><a name="prep"></a>Before you start</span></span>
<span data-ttu-id="d6743-124">Ha egy hivatkozás még nem rendelkezik az Application Insights SDK:</span><span class="sxs-lookup"><span data-stu-id="d6743-124">If you don't have a reference on Application Insights SDK yet:</span></span>

* <span data-ttu-id="d6743-125">Az Application Insights SDK hozzáadása a projekthez:</span><span class="sxs-lookup"><span data-stu-id="d6743-125">Add the Application Insights SDK to your project:</span></span>

  * [<span data-ttu-id="d6743-126">ASP.NET-projekt</span><span class="sxs-lookup"><span data-stu-id="d6743-126">ASP.NET project</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="d6743-127">Java-projekt</span><span class="sxs-lookup"><span data-stu-id="d6743-127">Java project</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="d6743-128">Az összes weboldal JavaScript</span><span class="sxs-lookup"><span data-stu-id="d6743-128">JavaScript in each webpage</span></span>](app-insights-javascript.md) 
* <span data-ttu-id="d6743-129">Az eszköz vagy a web server kódjában a következők:</span><span class="sxs-lookup"><span data-stu-id="d6743-129">In your device or web server code, include:</span></span>

    <span data-ttu-id="d6743-130">*C#:*`using Microsoft.ApplicationInsights;`</span><span class="sxs-lookup"><span data-stu-id="d6743-130">*C#:* `using Microsoft.ApplicationInsights;`</span></span>

    <span data-ttu-id="d6743-131">*Visual Basic:*`Imports Microsoft.ApplicationInsights`</span><span class="sxs-lookup"><span data-stu-id="d6743-131">*Visual Basic:* `Imports Microsoft.ApplicationInsights`</span></span>

    <span data-ttu-id="d6743-132">*Java:*`import com.microsoft.applicationinsights.TelemetryClient;`</span><span class="sxs-lookup"><span data-stu-id="d6743-132">*Java:* `import com.microsoft.applicationinsights.TelemetryClient;`</span></span>

## <a name="constructing-a-telemetryclient-instance"></a><span data-ttu-id="d6743-133">Egy TelemetryClient példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6743-133">Constructing a TelemetryClient instance</span></span>
<span data-ttu-id="d6743-134">Példányának összeállításához `TelemetryClient` (csak a JavaScript weblapok):</span><span class="sxs-lookup"><span data-stu-id="d6743-134">Construct an instance of `TelemetryClient` (except in JavaScript in webpages):</span></span>

<span data-ttu-id="d6743-135">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-135">*C#*</span></span>

    private TelemetryClient telemetry = new TelemetryClient();

<span data-ttu-id="d6743-136">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="d6743-136">*Visual Basic*</span></span>

    Private Dim telemetry As New TelemetryClient

<span data-ttu-id="d6743-137">*Java*</span><span class="sxs-lookup"><span data-stu-id="d6743-137">*Java*</span></span>

    private TelemetryClient telemetry = new TelemetryClient();

<span data-ttu-id="d6743-138">TelemetryClient szálbiztos.</span><span class="sxs-lookup"><span data-stu-id="d6743-138">TelemetryClient is thread-safe.</span></span>

<span data-ttu-id="d6743-139">Azt javasoljuk, hogy az alkalmazás minden modul TelemetryClient-példány használata.</span><span class="sxs-lookup"><span data-stu-id="d6743-139">We recommend that you use an instance of TelemetryClient for each module of your app.</span></span> <span data-ttu-id="d6743-140">Például előfordulhat, hogy egy TelemetryClient példánya a bejövő HTTP-kérelmekre, és a köztes osztály egy másik jelentés üzleti logika események jelentheti a webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d6743-140">For instance, you may have one TelemetryClient instance in your web service to report incoming HTTP requests, and another in a middleware class to report business logic events.</span></span> <span data-ttu-id="d6743-141">Beállítható például `TelemetryClient.Context.User.Id` nyomon követéséhez a felhasználók és a munkamenetek, vagy `TelemetryClient.Context.Device.Id` gépének azonosítását.</span><span class="sxs-lookup"><span data-stu-id="d6743-141">You can set properties such as `TelemetryClient.Context.User.Id` to track users and sessions, or `TelemetryClient.Context.Device.Id` to identify the machine.</span></span> <span data-ttu-id="d6743-142">Ez az információ minden események által küldött a példány van csatolva.</span><span class="sxs-lookup"><span data-stu-id="d6743-142">This information is attached to all events that the instance sends.</span></span>

## <a name="trackevent"></a><span data-ttu-id="d6743-143">TrackEvent</span><span class="sxs-lookup"><span data-stu-id="d6743-143">TrackEvent</span></span>
<span data-ttu-id="d6743-144">Az Application Insights egy *egyéni esemény* a megjeleníthetők adatpont van [Metrikaböngésző](app-insights-metrics-explorer.md) egy összesített száma, és a [diagnosztikai keresési](app-insights-diagnostic-search.md) önálló események.</span><span class="sxs-lookup"><span data-stu-id="d6743-144">In Application Insights, a *custom event* is a data point that you can display in [Metrics Explorer](app-insights-metrics-explorer.md) as an aggregated count, and in [Diagnostic Search](app-insights-diagnostic-search.md) as individual occurrences.</span></span> <span data-ttu-id="d6743-145">(Ez nem kapcsolódó MVC vagy más keretrendszer "események.")</span><span class="sxs-lookup"><span data-stu-id="d6743-145">(It isn't related to MVC or other framework "events.")</span></span>

<span data-ttu-id="d6743-146">Helyezze be `TrackEvent` hívja be a kódot a különböző események száma.</span><span class="sxs-lookup"><span data-stu-id="d6743-146">Insert `TrackEvent` calls in your code to count various events.</span></span> <span data-ttu-id="d6743-147">Milyen gyakran válassza ki a felhasználók egy adott szolgáltatással, milyen gyakran azok meghatározott célok elérése, vagy lehet, hogy milyen gyakran akkor hibák adott típusú.</span><span class="sxs-lookup"><span data-stu-id="d6743-147">How often users choose a particular feature, how often they achieve particular goals, or maybe how often they make particular types of mistakes.</span></span>

<span data-ttu-id="d6743-148">Például játék alkalmazásban elküldeni egy eseményt, amikor egy felhasználó nyer:</span><span class="sxs-lookup"><span data-stu-id="d6743-148">For example, in a game app, send an event whenever a user wins the game:</span></span>

<span data-ttu-id="d6743-149">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="d6743-149">*JavaScript*</span></span>

    appInsights.trackEvent("WinGame");

<span data-ttu-id="d6743-150">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-150">*C#*</span></span>

    telemetry.TrackEvent("WinGame");

<span data-ttu-id="d6743-151">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="d6743-151">*Visual Basic*</span></span>

    telemetry.TrackEvent("WinGame")

<span data-ttu-id="d6743-152">*Java*</span><span class="sxs-lookup"><span data-stu-id="d6743-152">*Java*</span></span>

    telemetry.trackEvent("WinGame");

### <a name="view-your-events-in-the-microsoft-azure-portal"></a><span data-ttu-id="d6743-153">Az események megtekintése a Microsoft Azure portálon</span><span class="sxs-lookup"><span data-stu-id="d6743-153">View your events in the Microsoft Azure portal</span></span>
<span data-ttu-id="d6743-154">Az események számát megtekintéséhez nyissa meg a [Metrikaböngésző](app-insights-metrics-explorer.md) panelen új diagram hozzáadása, és válassza ki **események**.</span><span class="sxs-lookup"><span data-stu-id="d6743-154">To see a count of your events, open a [Metrics Explorer](app-insights-metrics-explorer.md) blade, add a new chart, and select **Events**.</span></span>  

![Tekintse meg az egyéni események száma](./media/app-insights-api-custom-events-metrics/01-custom.png)

<span data-ttu-id="d6743-156">Hasonlítsa össze a különböző események számát, állítsa a diagramtípus **rács**, és az esemény neve:</span><span class="sxs-lookup"><span data-stu-id="d6743-156">To compare the counts of different events, set the chart type to **Grid**, and group by event name:</span></span>

![Adja meg a diagram típusát és a csoportosítás](./media/app-insights-api-custom-events-metrics/07-grid.png)

<span data-ttu-id="d6743-158">A rács kattintson az esemény a nevét, tekintse meg az esemény egyedi előfordulását.</span><span class="sxs-lookup"><span data-stu-id="d6743-158">On the grid, click through an event name to see individual occurrences of that event.</span></span> <span data-ttu-id="d6743-159">További részletek - kattintson bármely előfordulásainak kívánt számát, a listában.</span><span class="sxs-lookup"><span data-stu-id="d6743-159">To see more detail - click any occurrence in the list.</span></span>

![Az események áthatoló részletezést](./media/app-insights-api-custom-events-metrics/03-instances.png)

<span data-ttu-id="d6743-161">Adott események keresési vagy a Metrikaböngésző összpontosíthat, állítsa be a panelt szűrő érdekli, az esemény nevét:</span><span class="sxs-lookup"><span data-stu-id="d6743-161">To focus on specific events in either Search or Metrics Explorer, set the blade's filter to the event names that you're interested in:</span></span>

![Nyissa meg a szűrők, bontsa ki az esemény nevét, és válasszon egy vagy több érték](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a><span data-ttu-id="d6743-163">Egyéni események Analytics</span><span class="sxs-lookup"><span data-stu-id="d6743-163">Custom events in Analytics</span></span>

<span data-ttu-id="d6743-164">A telemetriai adatok érhető el a `customEvents` a tábla [Application Insights Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="d6743-164">The telemetry is available in the `customEvents` table in [Application Insights Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="d6743-165">Minden egyes hívásakor jelöl `trackEvent(..)` az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d6743-165">Each row represents a call to `trackEvent(..)` in your app.</span></span> 

<span data-ttu-id="d6743-166">Ha [mintavételi](app-insights-sampling.md) működik, az elemek száma tulajdonság jeleníti meg egy értéket 1-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="d6743-166">If [sampling](app-insights-sampling.md) is in operation, the itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="d6743-167">Példa az elemek száma a == 10 azt jelenti, hogy a trackevent() függvény 10 hívások, a mintavételi folyamat csak akkor továbbítódnak, ezek egyikét.</span><span class="sxs-lookup"><span data-stu-id="d6743-167">For example itemCount==10 means that of 10 calls to trackEvent(), the sampling process only transmitted one of them.</span></span> <span data-ttu-id="d6743-168">Ahhoz, hogy helyes-e az egyéni események számát, használjon ezért használható kóddal például `customEvent | summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="d6743-168">To get a correct count of custom events, you should use therefore use code such as `customEvent | summarize sum(itemCount)`.</span></span>


## <a name="trackmetric"></a><span data-ttu-id="d6743-169">TrackMetric</span><span class="sxs-lookup"><span data-stu-id="d6743-169">TrackMetric</span></span>

<span data-ttu-id="d6743-170">Az Application Insights is diagram, amelyek nem kapcsolódnak adott események metrikák.</span><span class="sxs-lookup"><span data-stu-id="d6743-170">Application Insights can chart metrics that are not attached to particular events.</span></span> <span data-ttu-id="d6743-171">Például felügyelheti a várólista hossza rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="d6743-171">For example, you could monitor a queue length at regular intervals.</span></span> <span data-ttu-id="d6743-172">A metrika egyedi mérések kevésbé fontos, mint a változások és a trendeket, és így statisztikai diagramok lehetnek hasznosak.</span><span class="sxs-lookup"><span data-stu-id="d6743-172">With metrics, the individual measurements are of less interest than the variations and trends, and so statistical charts are useful.</span></span>

<span data-ttu-id="d6743-173">Metrikák küldhet az Application Insights részére, használhatja a `TrackMetric(..)` API.</span><span class="sxs-lookup"><span data-stu-id="d6743-173">In order to send metrics to Application Insights, you can use the `TrackMetric(..)` API.</span></span> <span data-ttu-id="d6743-174">Metrika küldendő két módja van:</span><span class="sxs-lookup"><span data-stu-id="d6743-174">There are two ways to send a metric:</span></span> 

* <span data-ttu-id="d6743-175">Egyetlen értéket.</span><span class="sxs-lookup"><span data-stu-id="d6743-175">Single value.</span></span> <span data-ttu-id="d6743-176">Minden alkalommal, amikor az alkalmazás hajt végre egy mérték, az Application Insights küld a megfelelő értékkel.</span><span class="sxs-lookup"><span data-stu-id="d6743-176">Every time you perform a measurement in your application, you send the corresponding value to Application Insights.</span></span> <span data-ttu-id="d6743-177">Tegyük fel például, hogy rendelkezik-e a tárolóban lévő elemek száma leíró metrikát.</span><span class="sxs-lookup"><span data-stu-id="d6743-177">For example, assume that you have a metric describing the number of items in a container.</span></span> <span data-ttu-id="d6743-178">Egy adott időszakon belül mindhárom elem helyezze a tárolóba, és két elem távolítsa el.</span><span class="sxs-lookup"><span data-stu-id="d6743-178">During a particular time period, you first put three items into the container and then you remove two items.</span></span> <span data-ttu-id="d6743-179">Ennek megfelelően meghívta `TrackMetric` kétszer: először átadja a érték `3` , majd értéke `-2`.</span><span class="sxs-lookup"><span data-stu-id="d6743-179">Accordingly, you would call `TrackMetric` twice: first passing the value `3` and then the value `-2`.</span></span> <span data-ttu-id="d6743-180">Az Application Insights mindkét értéket tárolja az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="d6743-180">Application Insights stores both values on your behalf.</span></span> 

* <span data-ttu-id="d6743-181">Összesítést.</span><span class="sxs-lookup"><span data-stu-id="d6743-181">Aggregation.</span></span> <span data-ttu-id="d6743-182">Az metrikák használatakor minden egyetlen mérési ritkán érdekében áll.</span><span class="sxs-lookup"><span data-stu-id="d6743-182">When working with metrics, every single measurement is rarely of interest.</span></span> <span data-ttu-id="d6743-183">Ehelyett fontos Mi történt egy adott időszakon belül összegzését.</span><span class="sxs-lookup"><span data-stu-id="d6743-183">Instead a summary of what happened during a particular time period is important.</span></span> <span data-ttu-id="d6743-184">Ilyen összegzését nevezik _összesítési_.</span><span class="sxs-lookup"><span data-stu-id="d6743-184">Such a summary is called _aggregation_.</span></span> <span data-ttu-id="d6743-185">A fenti példában az adott időszakra vonatkozó összesített metrika összegük van `1` , a szám a metrika értékének `2`.</span><span class="sxs-lookup"><span data-stu-id="d6743-185">In the above example, the aggregate metric sum for that time period is `1` and the count of the metric values is `2`.</span></span> <span data-ttu-id="d6743-186">Az összesítési módszer használata esetén csak indításakor `TrackMetric` egyszer egy adott időszakra vonatkozóan, valamint az összesített értékek küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6743-186">When using the aggregation approach, you only invoke `TrackMetric` once per time period and send the aggregate values.</span></span> <span data-ttu-id="d6743-187">Ez az az ajánlott módszer, mivel jelentősen csökkentheti a költségek és a teljesítmény terhet elküldésével kevesebb adatpontok Application insights részére, továbbra is az összes vonatkozó információk összegyűjtése közben.</span><span class="sxs-lookup"><span data-stu-id="d6743-187">This is the recommended approach since it can significantly reduce the cost and performance overhead by sending fewer data points to Application Insights, while still collecting all relevant information.</span></span>

### <a name="examples"></a><span data-ttu-id="d6743-188">Példák:</span><span class="sxs-lookup"><span data-stu-id="d6743-188">Examples:</span></span>

#### <a name="single-values"></a><span data-ttu-id="d6743-189">Egyetlen értékek</span><span class="sxs-lookup"><span data-stu-id="d6743-189">Single values</span></span>

<span data-ttu-id="d6743-190">Küldése a egyetlen metrika értékét:</span><span class="sxs-lookup"><span data-stu-id="d6743-190">To send a single metric value:</span></span>

<span data-ttu-id="d6743-191">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="d6743-191">*JavaScript*</span></span>

 ```Javascript
     appInsights.trackMetric("queueLength", 42.0);
 ```

<span data-ttu-id="d6743-192">*C#, Java*</span><span class="sxs-lookup"><span data-stu-id="d6743-192">*C#, Java*</span></span>

```C#
    var sample = new MetricTelemetry();
    sample.Name = "metric name";
    sample.Value = 42.3;
    telemetryClient.TrackMetric(sample);
```

#### <a name="aggregating-metrics"></a><span data-ttu-id="d6743-193">Metrikák összesítése</span><span class="sxs-lookup"><span data-stu-id="d6743-193">Aggregating metrics</span></span>

<span data-ttu-id="d6743-194">Javasoljuk, hogy összesített metrikák mielőtt elküldi őket az alkalmazásból, a sávszélesség csökkentése érdekében költségeket, és a teljesítmény javítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="d6743-194">It is recommended to aggregate metrics before sending them from your app, to reduce bandwidth, cost and to improve performance.</span></span>
<span data-ttu-id="d6743-195">Itt látható egy példa összesítése kódot:</span><span class="sxs-lookup"><span data-stu-id="d6743-195">Here is an example of aggregating code:</span></span>

<span data-ttu-id="d6743-196">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-196">*C#*</span></span>

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
    /// Accepts metric values and sends the aggregated values at 1-minute intervals.
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
                    // Wait for end end of the aggregation period:
                    await Task.Delay(AggregationPeriod).ConfigureAwait(continueOnCapturedContext: false);

                    // Atomically snap the current aggregation:
                    MetricAggregator nextAggregator = new MetricAggregator(DateTimeOffset.UtcNow);
                    MetricAggregator prevAggregator = Interlocked.Exchange(ref _aggregator, nextAggregator);

                    // Only send anything is at least one value was measured:
                    if (prevAggregator != null && prevAggregator.Count > 0)
                    {
                        // Compute the actual aggregation period length:
                        TimeSpan aggPeriod = nextAggregator.StartTimestamp - prevAggregator.StartTimestamp;
                        if (aggPeriod.TotalMilliseconds < 1)
                        {
                            aggPeriod = TimeSpan.FromMilliseconds(1);
                        }

                        // Construct the metric telemetry item and send:
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

### <a name="custom-metrics-in-metrics-explorer"></a><span data-ttu-id="d6743-197">Egyéni metrikák a Metrikaböngészőben</span><span class="sxs-lookup"><span data-stu-id="d6743-197">Custom metrics in Metrics Explorer</span></span>

<span data-ttu-id="d6743-198">Az eredmények megtekintéséhez nyissa meg a Metrikaböngésző, és új diagram hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="d6743-198">To see the results, open Metrics Explorer and add a new chart.</span></span> <span data-ttu-id="d6743-199">A diagram megjelenítése a metrika szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6743-199">Edit the chart to show your metric.</span></span>

> [!NOTE]
> <span data-ttu-id="d6743-200">Az egyéni metrika megjelennek az elérhető mérőszámok listája több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="d6743-200">Your custom metric might take several minutes to appear in the list of available metrics.</span></span>
>

![Új diagram hozzáadása vagy a diagram az egyéni, válassza ki és a metrika](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a><span data-ttu-id="d6743-202">Egyéni metrikák Analytics</span><span class="sxs-lookup"><span data-stu-id="d6743-202">Custom metrics in Analytics</span></span>

<span data-ttu-id="d6743-203">A telemetriai adatok érhető el a `customMetrics` a tábla [Application Insights Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="d6743-203">The telemetry is available in the `customMetrics` table in [Application Insights Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="d6743-204">Minden egyes hívásakor jelöl `trackMetric(..)` az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d6743-204">Each row represents a call to `trackMetric(..)` in your app.</span></span>
* <span data-ttu-id="d6743-205">`valueSum`-Ez az a mérések összege.</span><span class="sxs-lookup"><span data-stu-id="d6743-205">`valueSum` - This is the sum of the measurements.</span></span> <span data-ttu-id="d6743-206">Ahhoz, hogy a középérték, nullával `valueCount`.</span><span class="sxs-lookup"><span data-stu-id="d6743-206">To get the mean value, divide by `valueCount`.</span></span>
* <span data-ttu-id="d6743-207">`valueCount`-A volt összesíti ebbe a mérések számát `trackMetric(..)` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="d6743-207">`valueCount` - The number of measurements that were aggregated into this `trackMetric(..)` call.</span></span>

## <a name="page-views"></a><span data-ttu-id="d6743-208">Lapmegtekintések</span><span class="sxs-lookup"><span data-stu-id="d6743-208">Page views</span></span>
<span data-ttu-id="d6743-209">Egy eszköz vagy a weblap alkalmazásban lap nézet telemetriai küldi alapértelmezés szerint ha egyes képernyőit vagy lapon be van töltve.</span><span class="sxs-lookup"><span data-stu-id="d6743-209">In a device or webpage app, page view telemetry is sent by default when each screen or page is loaded.</span></span> <span data-ttu-id="d6743-210">De további vagy különböző időpontokban Lapmegtekintések nyomon követésére, hogy módosítható.</span><span class="sxs-lookup"><span data-stu-id="d6743-210">But you can change that to track page views at additional or different times.</span></span> <span data-ttu-id="d6743-211">Például egy alkalmazást, amely megjeleníti a tabulátorokat vagy paneleken, a kívánt nyomon követése lap, amikor a felhasználó megnyit egy új panelen.</span><span class="sxs-lookup"><span data-stu-id="d6743-211">For example, in an app that displays tabs or blades, you might want to track a page whenever the user opens a new blade.</span></span>

![Használati fókuszban panelen – áttekintés](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

<span data-ttu-id="d6743-213">Felhasználó és a munkamenet adatküldést Lapmegtekintések, valamint tulajdonságként, a felhasználó és a munkamenet diagramok származnak életben lap nézet telemetriai adat esetén.</span><span class="sxs-lookup"><span data-stu-id="d6743-213">User and session data is sent as properties along with page views, so the user and session charts come alive when there is page view telemetry.</span></span>

### <a name="custom-page-views"></a><span data-ttu-id="d6743-214">Egyéni Lapmegtekintések</span><span class="sxs-lookup"><span data-stu-id="d6743-214">Custom page views</span></span>
<span data-ttu-id="d6743-215">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="d6743-215">*JavaScript*</span></span>

    appInsights.trackPageView("tab1");

<span data-ttu-id="d6743-216">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-216">*C#*</span></span>

    telemetry.TrackPageView("GameReviewPage");

<span data-ttu-id="d6743-217">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="d6743-217">*Visual Basic*</span></span>

    telemetry.TrackPageView("GameReviewPage")


<span data-ttu-id="d6743-218">Ha másik HTML-lapok belül több lap van, megadhatja az URL-cím túl:</span><span class="sxs-lookup"><span data-stu-id="d6743-218">If you have several tabs within different HTML pages, you can specify the URL too:</span></span>

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a><span data-ttu-id="d6743-219">Lapmegtekintések időzítése</span><span class="sxs-lookup"><span data-stu-id="d6743-219">Timing page views</span></span>
<span data-ttu-id="d6743-220">Alapértelmezés szerint a idők jelentése szerint **lapmegtekintés betöltési ideje** mérik a, amikor a böngésző elküldi a kérelmet, amíg a böngésző lap betöltési esemény nevezik.</span><span class="sxs-lookup"><span data-stu-id="d6743-220">By default, the times reported as **Page view load time** are measured from when the browser sends the request, until the browser's page load event is called.</span></span>

<span data-ttu-id="d6743-221">Ehelyett lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="d6743-221">Instead, you can either:</span></span>

* <span data-ttu-id="d6743-222">Egy explicit időtartamot beállítani a [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) hívja: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.</span><span class="sxs-lookup"><span data-stu-id="d6743-222">Set an explicit duration in the [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) call: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.</span></span>
* <span data-ttu-id="d6743-223">Az oldal nézetben hívások időzítése `startTrackPage` és `stopTrackPage`.</span><span class="sxs-lookup"><span data-stu-id="d6743-223">Use the page view timing calls `startTrackPage` and `stopTrackPage`.</span></span>

<span data-ttu-id="d6743-224">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="d6743-224">*JavaScript*</span></span>

    // To start timing a page:
    appInsights.startTrackPage("Page1");

<span data-ttu-id="d6743-225">...</span><span class="sxs-lookup"><span data-stu-id="d6743-225">...</span></span>

    // To stop timing and log the page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

<span data-ttu-id="d6743-226">A név első paraméterként használó társítja a kezdési és befejezési hívásokat.</span><span class="sxs-lookup"><span data-stu-id="d6743-226">The name that you use as the first parameter associates the start and stop calls.</span></span> <span data-ttu-id="d6743-227">Alapértelmezés szerint az aktuális lap neve.</span><span class="sxs-lookup"><span data-stu-id="d6743-227">It defaults to the current page name.</span></span>

<span data-ttu-id="d6743-228">Az eredményül kapott lap betöltési időtartamok Metrikaböngésző jelenik meg a kezdési és befejezési hívások időközétől származik.</span><span class="sxs-lookup"><span data-stu-id="d6743-228">The resulting page load durations displayed in Metrics Explorer are derived from the interval between the start and stop calls.</span></span> <span data-ttu-id="d6743-229">Már Önnek milyen időköz ténylegesen idő.</span><span class="sxs-lookup"><span data-stu-id="d6743-229">It's up to you what interval you actually time.</span></span>

### <a name="page-telemetry-in-analytics"></a><span data-ttu-id="d6743-230">Az elemzés lap telemetria</span><span class="sxs-lookup"><span data-stu-id="d6743-230">Page telemetry in Analytics</span></span>

<span data-ttu-id="d6743-231">A [Analytics](app-insights-analytics.md) két tábla browser műveletek adatait megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="d6743-231">In [Analytics](app-insights-analytics.md) two tables show data from browser operations:</span></span>

* <span data-ttu-id="d6743-232">A `pageViews` táblázat az URL-cím és a lap címét kapcsolatos adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d6743-232">The `pageViews` table contains data about the URL and page title</span></span>
* <span data-ttu-id="d6743-233">A `browserTimings` tábla ügyfél teljesítményét, például a bejövő adatok feldolgozásához szükséges idő kapcsolatos adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d6743-233">The `browserTimings` table contains data about client performance, such as the time taken to process the incoming data</span></span>

<span data-ttu-id="d6743-234">Kereséséhez, mennyi időt vesz igénybe a böngészőben a különböző oldalakhoz feldolgozásához:</span><span class="sxs-lookup"><span data-stu-id="d6743-234">To find how long the browser takes to process different pages:</span></span>

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

<span data-ttu-id="d6743-235">A különböző böngészők popularities észlelése:</span><span class="sxs-lookup"><span data-stu-id="d6743-235">To discover the popularities of different browsers:</span></span>

```
pageViews | summarize count() by client_Browser
```

<span data-ttu-id="d6743-236">Rendelje hozzá a lapmegtekintések AJAX-hívások, csatlakozik a függőségek:</span><span class="sxs-lookup"><span data-stu-id="d6743-236">To associate page views to AJAX calls, join with dependencies:</span></span>

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a><span data-ttu-id="d6743-237">TrackRequest</span><span class="sxs-lookup"><span data-stu-id="d6743-237">TrackRequest</span></span>
<span data-ttu-id="d6743-238">A kiszolgáló SDK TrackRequest használja a HTTP-kérések naplózására.</span><span class="sxs-lookup"><span data-stu-id="d6743-238">The server SDK uses TrackRequest to log HTTP requests.</span></span>

<span data-ttu-id="d6743-239">Akkor is is meghívhatja, ha szeretné szimulálni a kéréseket a környezetében fut webszolgáltatás modul esetében nem.</span><span class="sxs-lookup"><span data-stu-id="d6743-239">You can also call it yourself if you want to simulate requests in a context where you don't have the web service module running.</span></span>

<span data-ttu-id="d6743-240">Azonban az ajánlott módszer a kérelem telemetriai adatokat küldhet, ahol a kérelem úgy működik, mint egy <a href="#operation-context">műveletkörnyezetet</a>.</span><span class="sxs-lookup"><span data-stu-id="d6743-240">However, the recommended way to send request telemetry is where the request acts as an <a href="#operation-context">operation context</a>.</span></span>

## <a name="operation-context"></a><span data-ttu-id="d6743-241">A művelet a környezetben</span><span class="sxs-lookup"><span data-stu-id="d6743-241">Operation context</span></span>
<span data-ttu-id="d6743-242">Telemetria elemek együtt is társíthat, hozzájuk rendelve egy közös műveletet.</span><span class="sxs-lookup"><span data-stu-id="d6743-242">You can associate telemetry items together by attaching to them a common operation ID.</span></span> <span data-ttu-id="d6743-243">A normál kérés-nyomkövetési modul elvégzi ezt a kivételeket és eseményeket, amelyek küldött HTTP-kérelem feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="d6743-243">The standard request-tracking module does this for exceptions and other events that are sent while an HTTP request is being processed.</span></span> <span data-ttu-id="d6743-244">A [keresési](app-insights-diagnostic-search.md) és [Analytics](app-insights-analytics.md), az Azonosítót használva követheti könnyedén megtalálhatja a kérelemhez társított eseményeket.</span><span class="sxs-lookup"><span data-stu-id="d6743-244">In [Search](app-insights-diagnostic-search.md) and [Analytics](app-insights-analytics.md), you can use the ID to easily find any events associated with the request.</span></span>

<span data-ttu-id="d6743-245">A legegyszerűbb módja a azonosítóját, hogy egy művelet környezetben állította ezt a mintát használja:</span><span class="sxs-lookup"><span data-stu-id="d6743-245">The easiest way to set the ID is to set an operation context by using this pattern:</span></span>

<span data-ttu-id="d6743-246">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-246">*C#*</span></span>

```C#
// Establish an operation context and associated telemetry item:
using (var operation = telemetry.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use the same operation ID.
    ...
    telemetry.TrackTrace(...); // or other Track* calls
    ...
    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetry.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

<span data-ttu-id="d6743-247">Egy művelet környezet beállítása mellett `StartOperation` hoz létre a megadott típusú telemetriai elemet.</span><span class="sxs-lookup"><span data-stu-id="d6743-247">Along with setting an operation context, `StartOperation` creates a telemetry item of the type that you specify.</span></span> <span data-ttu-id="d6743-248">A telemetriai adatok elemet elküldi meg azzal, hogy a művelet, vagy ha explicit módon hívja `StopOperation`.</span><span class="sxs-lookup"><span data-stu-id="d6743-248">It sends the telemetry item when you dispose the operation, or if you explicitly call `StopOperation`.</span></span> <span data-ttu-id="d6743-249">Ha `RequestTelemetry` a telemetria-típusként időtartama közötti kezdési és befejezési rendszeres időközönkénti értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="d6743-249">If you use `RequestTelemetry` as the telemetry type, its duration is set to the timed interval between start and stop.</span></span>

<span data-ttu-id="d6743-250">Művelet környezetek nem ágyazhatók egymásba.</span><span class="sxs-lookup"><span data-stu-id="d6743-250">Operation contexts can't be nested.</span></span> <span data-ttu-id="d6743-251">Ha már van egy művelet környezetben, akkor Azonosítóval társítva a benne lévő elemek, például a létrehozott elemek `StartOperation`.</span><span class="sxs-lookup"><span data-stu-id="d6743-251">If there is already an operation context, then its ID is associated with all the contained items, including the item created with `StartOperation`.</span></span>

<span data-ttu-id="d6743-252">A Keresés a műveletkörnyezetet létrehozásához használt a **kapcsolódó elemek** listája:</span><span class="sxs-lookup"><span data-stu-id="d6743-252">In Search, the operation context is used to create the **Related Items** list:</span></span>

![Kapcsolódó elemek](./media/app-insights-api-custom-events-metrics/21.png)

<span data-ttu-id="d6743-254">[Alkalmazás-insights – egyéni-műveletek-tracking.md] követési egyéni műveletek további információt talál.</span><span class="sxs-lookup"><span data-stu-id="d6743-254">See [application-insights-custom-operations-tracking.md] for more information on custom operations tracking.</span></span>

### <a name="requests-in-analytics"></a><span data-ttu-id="d6743-255">Az elemzés kérelmek</span><span class="sxs-lookup"><span data-stu-id="d6743-255">Requests in Analytics</span></span> 

<span data-ttu-id="d6743-256">A [Application Insights Analytics](app-insights-analytics.md), megjelenítése kéri fel a a `requests` tábla.</span><span class="sxs-lookup"><span data-stu-id="d6743-256">In [Application Insights Analytics](app-insights-analytics.md), requests show up in the `requests` table.</span></span>

<span data-ttu-id="d6743-257">Ha [mintavételi](app-insights-sampling.md) van a művelet az elemek száma tulajdonság értékét fogja megjeleníteni a 1-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="d6743-257">If [sampling](app-insights-sampling.md) is in operation, the itemCount property will show a value greater than 1.</span></span> <span data-ttu-id="d6743-258">Példa az elemek száma a == 10 azt jelenti, hogy trackRequest() 10 hívások, a mintavételi folyamat csak akkor továbbítódnak, ezek egyikét.</span><span class="sxs-lookup"><span data-stu-id="d6743-258">For example itemCount==10 means that of 10 calls to trackRequest(), the sampling process only transmitted one of them.</span></span> <span data-ttu-id="d6743-259">Kérelmek és átlagos időtartama szegmentált kérelem neve helyes számát, amelyet kódot, mint:</span><span class="sxs-lookup"><span data-stu-id="d6743-259">To get a correct count of requests and average duration segmented by request names, use code such as:</span></span>

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a><span data-ttu-id="d6743-260">TrackException</span><span class="sxs-lookup"><span data-stu-id="d6743-260">TrackException</span></span>
<span data-ttu-id="d6743-261">Kivételek küldése az Application Insights:</span><span class="sxs-lookup"><span data-stu-id="d6743-261">Send exceptions to Application Insights:</span></span>

* <span data-ttu-id="d6743-262">A [megszámolni](app-insights-metrics-explorer.md), mint arra utal, hogy egy probléma gyakorisága.</span><span class="sxs-lookup"><span data-stu-id="d6743-262">To [count them](app-insights-metrics-explorer.md), as an indication of the frequency of a problem.</span></span>
* <span data-ttu-id="d6743-263">A [vizsgálja meg az egyes előfordulások](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="d6743-263">To [examine individual occurrences](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="d6743-264">A jelentései tartalmazzák a híváslánc megjelenik.</span><span class="sxs-lookup"><span data-stu-id="d6743-264">The reports include the stack traces.</span></span>

<span data-ttu-id="d6743-265">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-265">*C#*</span></span>

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

<span data-ttu-id="d6743-266">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="d6743-266">*JavaScript*</span></span>

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }

<span data-ttu-id="d6743-267">Az SDK-k kivételeket sok automatikusan, így nem mindig kell TrackException explicit módon hívja.</span><span class="sxs-lookup"><span data-stu-id="d6743-267">The SDKs catch many exceptions automatically, so you don't always have to call TrackException explicitly.</span></span>

* <span data-ttu-id="d6743-268">ASP.NET: [írhat kódot a kivételeket](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="d6743-268">ASP.NET: [Write code to catch exceptions](app-insights-asp-net-exceptions.md).</span></span>
* <span data-ttu-id="d6743-269">J2EE: [kivételek automatikusan észlelt](app-insights-java-get-started.md#exceptions-and-request-failures).</span><span class="sxs-lookup"><span data-stu-id="d6743-269">J2EE: [Exceptions are caught automatically](app-insights-java-get-started.md#exceptions-and-request-failures).</span></span>
* <span data-ttu-id="d6743-270">JavaScript: Kivételek automatikusan észlelt.</span><span class="sxs-lookup"><span data-stu-id="d6743-270">JavaScript: Exceptions are caught automatically.</span></span> <span data-ttu-id="d6743-271">Ha le szeretné tiltani automatikus gyűjtemény, vonal felvétele a weblapok beillesztett a kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="d6743-271">If you want to disable automatic collection, add a line to the code snippet that you insert in your webpages:</span></span>

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a><span data-ttu-id="d6743-272">Az elemzés kivételek</span><span class="sxs-lookup"><span data-stu-id="d6743-272">Exceptions in Analytics</span></span>

<span data-ttu-id="d6743-273">A [Application Insights Analytics](app-insights-analytics.md), kivételek jelennek meg a `exceptions` tábla.</span><span class="sxs-lookup"><span data-stu-id="d6743-273">In [Application Insights Analytics](app-insights-analytics.md), exceptions show up in the `exceptions` table.</span></span>

<span data-ttu-id="d6743-274">Ha [mintavételi](app-insights-sampling.md) működik, a `itemCount` tulajdonság jeleníti meg egy értéket 1-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="d6743-274">If [sampling](app-insights-sampling.md) is in operation, the `itemCount` property shows a value greater than 1.</span></span> <span data-ttu-id="d6743-275">Példa az elemek száma a == 10 azt jelenti, hogy trackException() 10 hívások, a mintavételi folyamat csak akkor továbbítódnak, ezek egyikét.</span><span class="sxs-lookup"><span data-stu-id="d6743-275">For example itemCount==10 means that of 10 calls to trackException(), the sampling process only transmitted one of them.</span></span> <span data-ttu-id="d6743-276">Kivétel típusa által szegmentált kivételek megfelelő számát, amelyet kódot, mint:</span><span class="sxs-lookup"><span data-stu-id="d6743-276">To get a correct count of exceptions segmented by type of exception, use code such as:</span></span>

```
exceptions | summarize sum(itemCount) by type
```

<span data-ttu-id="d6743-277">A legtöbb fontos verem információ már kibontása külön változók, de lekérni a egymástól a `details` struktúra szeretné még többet.</span><span class="sxs-lookup"><span data-stu-id="d6743-277">Most of the important stack information is already extracted into separate variables, but you can pull apart the `details` structure to get more.</span></span> <span data-ttu-id="d6743-278">Mivel ez a struktúra dinamikus, akkor az eredmény, a várt típus kell konvertálni.</span><span class="sxs-lookup"><span data-stu-id="d6743-278">Since this structure is dynamic, you should cast the result to the type you expect.</span></span> <span data-ttu-id="d6743-279">Példa:</span><span class="sxs-lookup"><span data-stu-id="d6743-279">For example:</span></span>

```AIQL
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

<span data-ttu-id="d6743-280">Kapcsolódó kérésük kivételek társítani, használja az illesztés:</span><span class="sxs-lookup"><span data-stu-id="d6743-280">To associate exceptions with their related requests, use a join:</span></span>

```
exceptions
| join (requests) on operation_Id 
```

## <a name="tracktrace"></a><span data-ttu-id="d6743-281">TrackTrace</span><span class="sxs-lookup"><span data-stu-id="d6743-281">TrackTrace</span></span>
<span data-ttu-id="d6743-282">Használjon TrackTrace problémák diagnosztizálása az Application Insights "navigációs nyomokat" elküldésével.</span><span class="sxs-lookup"><span data-stu-id="d6743-282">Use TrackTrace to help diagnose problems by sending a "breadcrumb trail" to Application Insights.</span></span> <span data-ttu-id="d6743-283">Adattömbök diagnosztikai adatok küldése, és vizsgálja meg azokat a [diagnosztikai keresési](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="d6743-283">You can send chunks of diagnostic data and inspect them in [Diagnostic Search](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="d6743-284">[Naplófájl adapterek](app-insights-asp-net-trace-logs.md) külső naplókat küld a portál az API segítségével.</span><span class="sxs-lookup"><span data-stu-id="d6743-284">[Log adapters](app-insights-asp-net-trace-logs.md) use this API to send third-party logs to the portal.</span></span>

<span data-ttu-id="d6743-285">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-285">*C#*</span></span>

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);


<span data-ttu-id="d6743-286">Üzenet tartalma kereshet, de (ellentétben a tulajdonságértékek) nem lehet szűrni rajta.</span><span class="sxs-lookup"><span data-stu-id="d6743-286">You can search on message content, but (unlike property values) you can't filter on it.</span></span>

<span data-ttu-id="d6743-287">A méretkorlát a `message` sokkal nagyobb, mint a Tulajdonságok vonatkozó korlátozást.</span><span class="sxs-lookup"><span data-stu-id="d6743-287">The size limit on `message` is much higher than the limit on properties.</span></span>
<span data-ttu-id="d6743-288">TrackTrace előnye, hogy viszonylag hosszú adatok helyezhetik az üzenetben.</span><span class="sxs-lookup"><span data-stu-id="d6743-288">An advantage of TrackTrace is that you can put relatively long data in the message.</span></span> <span data-ttu-id="d6743-289">Például lehet kódolása nincs POST-adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6743-289">For example, you can encode POST data there.</span></span>  

<span data-ttu-id="d6743-290">Emellett egy súlyossági szintet adhat hozzá az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="d6743-290">In addition, you can add a severity level to your message.</span></span> <span data-ttu-id="d6743-291">És egyéb telemetriai adatok, például értékeket is hozzáadhat tulajdonság segítségével szűrőt, vagy keressen a nyomkövetési más-más részhalmazához.</span><span class="sxs-lookup"><span data-stu-id="d6743-291">And, like other telemetry, you can add property values to help you filter or search for different sets of traces.</span></span> <span data-ttu-id="d6743-292">Példa:</span><span class="sxs-lookup"><span data-stu-id="d6743-292">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

<span data-ttu-id="d6743-293">A [keresési](app-insights-diagnostic-search.md), majd egyszerűen kiszűrheti az összes üzenet egy adott súlyossági szintet az adott adatbázishoz kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="d6743-293">In [Search](app-insights-diagnostic-search.md), you can then easily filter out all the messages of a particular severity level that relate to a particular database.</span></span>


### <a name="traces-in-analytics"></a><span data-ttu-id="d6743-294">Nyomok Analytics</span><span class="sxs-lookup"><span data-stu-id="d6743-294">Traces in Analytics</span></span>

<span data-ttu-id="d6743-295">A [Application Insights Analytics](app-insights-analytics.md), TrackTrace hívások jelennek meg a `traces` tábla.</span><span class="sxs-lookup"><span data-stu-id="d6743-295">In [Application Insights Analytics](app-insights-analytics.md), calls to TrackTrace show up in the `traces` table.</span></span>

<span data-ttu-id="d6743-296">Ha [mintavételi](app-insights-sampling.md) működik, az elemek száma tulajdonság jeleníti meg egy értéket 1-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="d6743-296">If [sampling](app-insights-sampling.md) is in operation, the itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="d6743-297">Példa az elemek száma == 10 azt jelenti, hogy 10 hívásainak `trackTrace()`, a mintavételi folyamat csak akkor továbbítódnak, ezek egyikét.</span><span class="sxs-lookup"><span data-stu-id="d6743-297">For example itemCount==10 means that of 10 calls to `trackTrace()`, the sampling process only transmitted one of them.</span></span> <span data-ttu-id="d6743-298">Ahhoz, hogy a helyes nyomkövetési indított hívások száma., kell ezért kódot használja, mint `traces | summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="d6743-298">To get a correct count of trace calls, you should use therefore code such as `traces | summarize sum(itemCount)`.</span></span>

## <a name="trackdependency"></a><span data-ttu-id="d6743-299">TrackDependency</span><span class="sxs-lookup"><span data-stu-id="d6743-299">TrackDependency</span></span>
<span data-ttu-id="d6743-300">A TrackDependency hívás segítségével nyomon követheti a válaszidejét és sikerességi arányát kód külső kódnak küldött hívások.</span><span class="sxs-lookup"><span data-stu-id="d6743-300">Use the TrackDependency call to track the response times and success rates of calls to an external piece of code.</span></span> <span data-ttu-id="d6743-301">A portál függőségi diagramjain jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="d6743-301">The results appear in the dependency charts in the portal.</span></span>

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

<span data-ttu-id="d6743-302">Ne feledje, hogy a kiszolgáló SDK-k tartalmaz egy [függőségi modul](app-insights-asp-net-dependencies.md) , amely észleli, és nyomon követi az egyes függőségi hívások automatikusan – például adatbázisok és a REST API-k.</span><span class="sxs-lookup"><span data-stu-id="d6743-302">Remember that the server SDKs include a [dependency module](app-insights-asp-net-dependencies.md) that discovers and tracks certain dependency calls automatically--for example, to databases and REST APIs.</span></span> <span data-ttu-id="d6743-303">Kell egy ügynököt telepít a kiszolgálót, és ellenőrizze a modul működik.</span><span class="sxs-lookup"><span data-stu-id="d6743-303">You have to install an agent on your server to make the module work.</span></span> <span data-ttu-id="d6743-304">Ha szeretné nyomon követni a hívásokat, amely a automatizált követési nem dolgozza fel, vagy ha nem kívánja telepíteni az ügynököt a hívás használja.</span><span class="sxs-lookup"><span data-stu-id="d6743-304">You use this call if you want to track calls that the automated tracking doesn't catch, or if you don't want to install the agent.</span></span>

<span data-ttu-id="d6743-305">A szabványos függőségi követése modul kikapcsolásához szerkesztése [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) , és törölje a hivatkozása `DependencyCollector.DependencyTrackingTelemetryModule`.</span><span class="sxs-lookup"><span data-stu-id="d6743-305">To turn off the standard dependency-tracking module, edit [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) and delete the reference to `DependencyCollector.DependencyTrackingTelemetryModule`.</span></span>

### <a name="dependencies-in-analytics"></a><span data-ttu-id="d6743-306">Az elemzés függőségek</span><span class="sxs-lookup"><span data-stu-id="d6743-306">Dependencies in Analytics</span></span>

<span data-ttu-id="d6743-307">A [Application Insights Analytics](app-insights-analytics.md), trackDependency hívások megjelenítése a `dependencies` tábla.</span><span class="sxs-lookup"><span data-stu-id="d6743-307">In [Application Insights Analytics](app-insights-analytics.md), trackDependency calls show up in the `dependencies` table.</span></span>

<span data-ttu-id="d6743-308">Ha [mintavételi](app-insights-sampling.md) működik, az elemek száma tulajdonság jeleníti meg egy értéket 1-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="d6743-308">If [sampling](app-insights-sampling.md) is in operation, the itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="d6743-309">Példa az elemek száma a == 10 azt jelenti, hogy trackDependency() 10 hívások, a mintavételi folyamat csak akkor továbbítódnak, ezek egyikét.</span><span class="sxs-lookup"><span data-stu-id="d6743-309">For example itemCount==10 means that of 10 calls to trackDependency(), the sampling process only transmitted one of them.</span></span> <span data-ttu-id="d6743-310">Cél összetevő szegmentált függőségek megfelelő számát, amelyet kódot, mint:</span><span class="sxs-lookup"><span data-stu-id="d6743-310">To get a correct count of dependencies segmented by target component, use code such as:</span></span>

```
dependencies | summarize sum(itemCount) by target
```

<span data-ttu-id="d6743-311">Kapcsolódó kérésük függőségek társítani, használja az illesztés:</span><span class="sxs-lookup"><span data-stu-id="d6743-311">To associate dependencies with their related requests, use a join:</span></span>

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a><span data-ttu-id="d6743-312">Könyvelési adatok</span><span class="sxs-lookup"><span data-stu-id="d6743-312">Flushing data</span></span>
<span data-ttu-id="d6743-313">Általában az SDK küldi az adatokat a felhasználó gyakorolt hatás minimalizálása érdekében időnként választott.</span><span class="sxs-lookup"><span data-stu-id="d6743-313">Normally, the SDK sends data at times chosen to minimize the impact on the user.</span></span> <span data-ttu-id="d6743-314">Azonban bizonyos esetekben érdemes kiüríteni a puffer – például ha egy alkalmazás, amely leállítja az SDK-t használ.</span><span class="sxs-lookup"><span data-stu-id="d6743-314">However, in some cases, you might want to flush the buffer--for example, if you are using the SDK in an application that shuts down.</span></span>

<span data-ttu-id="d6743-315">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-315">*C#*</span></span>

    telemetry.Flush();

    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(1000);

<span data-ttu-id="d6743-316">Vegye figyelembe, hogy a függvény az aszinkron a [server telemetriai csatorna](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).</span><span class="sxs-lookup"><span data-stu-id="d6743-316">Note that the function is asynchronous for the [server telemetry channel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).</span></span>

## <a name="authenticated-users"></a><span data-ttu-id="d6743-317">Hitelesített felhasználók</span><span class="sxs-lookup"><span data-stu-id="d6743-317">Authenticated users</span></span>
<span data-ttu-id="d6743-318">A webes alkalmazás a felhasználók (alapértelmezés) azonosítja a cookie-k.</span><span class="sxs-lookup"><span data-stu-id="d6743-318">In a web app, users are (by default) identified by cookies.</span></span> <span data-ttu-id="d6743-319">Ha az alkalmazás hozzáférésük egy másik számítógép vagy a böngésző, vagy ha ezek a cookie-k törléséhez előfordulhat, hogy számba lehet vennni a felhasználó egynél többször.</span><span class="sxs-lookup"><span data-stu-id="d6743-319">A user might be counted more than once if they access your app from a different machine or browser, or if they delete cookies.</span></span>

<span data-ttu-id="d6743-320">Ha az alkalmazás a felhasználók bejelentkeznek, kaphat pontosabb számát úgy, hogy a hitelesített felhasználói Azonosítóját a böngésző-kódban:</span><span class="sxs-lookup"><span data-stu-id="d6743-320">If users sign in to your app, you can get a more accurate count by setting the authenticated user ID in the browser code:</span></span>

<span data-ttu-id="d6743-321">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="d6743-321">*JavaScript*</span></span>

```JS
// Called when my app has identified the user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

<span data-ttu-id="d6743-322">Az ASP.NET-webalkalmazás MVC alkalmazás, például:</span><span class="sxs-lookup"><span data-stu-id="d6743-322">In an ASP.NET web MVC application, for example:</span></span>

<span data-ttu-id="d6743-323">*RAZOR*</span><span class="sxs-lookup"><span data-stu-id="d6743-323">*Razor*</span></span>

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

<span data-ttu-id="d6743-324">Nem használható a felhasználó tényleges bejelentkezési név szükséges.</span><span class="sxs-lookup"><span data-stu-id="d6743-324">It isn't necessary to use the user's actual sign-in name.</span></span> <span data-ttu-id="d6743-325">Csak akkor nem lehet, hogy a felhasználó egyedi Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="d6743-325">It only has to be an ID that is unique to that user.</span></span> <span data-ttu-id="d6743-326">Nem tartalmazhatnak szóközöket és a karakterek `,;=|`.</span><span class="sxs-lookup"><span data-stu-id="d6743-326">It must not include spaces or any of the characters `,;=|`.</span></span>

<span data-ttu-id="d6743-327">A felhasználói azonosító is beállította egy munkamenet-cookie-ban és a kiszolgálónak küld el.</span><span class="sxs-lookup"><span data-stu-id="d6743-327">The user ID is also set in a session cookie and sent to the server.</span></span> <span data-ttu-id="d6743-328">Ha a kiszolgáló SDK telepítve van, a hitelesített felhasználói azonosító küldött ügyfél- és telemetria környezeti tulajdonságainak részeként.</span><span class="sxs-lookup"><span data-stu-id="d6743-328">If the server SDK is installed, the authenticated user ID is sent as part of the context properties of both client and server telemetry.</span></span> <span data-ttu-id="d6743-329">Majd végezhet, és keresse meg azt.</span><span class="sxs-lookup"><span data-stu-id="d6743-329">You can then filter and search on it.</span></span>

<span data-ttu-id="d6743-330">Ha az alkalmazás felhasználók csoportosítja a fiókok, a fiók (az azonos karakter korlátozások) azonosítót is átadhatja.</span><span class="sxs-lookup"><span data-stu-id="d6743-330">If your app groups users into accounts, you can also pass an identifier for the account (with the same character restrictions).</span></span>

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

<span data-ttu-id="d6743-331">A [Metrikaböngésző](app-insights-metrics-explorer.md), létrehozhat egy diagram, amely megjeleníti **hitelesített felhasználók,**, és **felhasználói fiókok**.</span><span class="sxs-lookup"><span data-stu-id="d6743-331">In [Metrics Explorer](app-insights-metrics-explorer.md), you can create a chart that counts **Users, Authenticated**, and **User accounts**.</span></span>

<span data-ttu-id="d6743-332">Emellett [keresési](app-insights-diagnostic-search.md) az ügyfél adatpontok adott felhasználói neveket és fiókok.</span><span class="sxs-lookup"><span data-stu-id="d6743-332">You can also [search](app-insights-diagnostic-search.md) for client data points with specific user names and accounts.</span></span>

## <span data-ttu-id="d6743-333"><a name="properties"></a>Szűrés, keresést és az adatok példájaként tulajdonságok felhasználásával</span><span class="sxs-lookup"><span data-stu-id="d6743-333"><a name="properties"></a>Filtering, searching, and segmenting your data by using properties</span></span>
<span data-ttu-id="d6743-334">Tulajdonságok és mérések csatlakoztatni az események (szabadon is csatlakozva lapon a nézetek, kivételeket és egyéb telemetriai adatokat).</span><span class="sxs-lookup"><span data-stu-id="d6743-334">You can attach properties and measurements to your events (and also to metrics, page views, exceptions, and other telemetry data).</span></span>

<span data-ttu-id="d6743-335">*Tulajdonságok* olyan karakterlánc értékek, amelyek segítségével a telemetriai adatokat a használati jelentések szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="d6743-335">*Properties* are string values that you can use to filter your telemetry in the usage reports.</span></span> <span data-ttu-id="d6743-336">Például ha az alkalmazás biztosít több játékok, csatolhat a játék neve minden esemény, hogy láthassa, melyik játékok népszerűbbnek.</span><span class="sxs-lookup"><span data-stu-id="d6743-336">For example, if your app provides several games, you can attach the name of the game to each event so that you can see which games are more popular.</span></span>

<span data-ttu-id="d6743-337">A karakterlánc hossza 8192 korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="d6743-337">There's a limit of 8192 on the string length.</span></span> <span data-ttu-id="d6743-338">(Nagy méretű adattömböket írnak küldeni, használja az üzenet paramétere [TrackTrace](#track-trace).)</span><span class="sxs-lookup"><span data-stu-id="d6743-338">(If you want to send large chunks of data, use the message parameter of [TrackTrace](#track-trace).)</span></span>

<span data-ttu-id="d6743-339">*Metrikák* számértékek jelenítheti meg grafikusan.</span><span class="sxs-lookup"><span data-stu-id="d6743-339">*Metrics* are numeric values that can be presented graphically.</span></span> <span data-ttu-id="d6743-340">Például előfordulhat, hogy kívánt van-e a eredmények, amelyek a gamers elérése fokozatos növekedését.</span><span class="sxs-lookup"><span data-stu-id="d6743-340">For example, you might want to see if there's a gradual increase in the scores that your gamers achieve.</span></span> <span data-ttu-id="d6743-341">Az eseményhez küldött tulajdonságait is szegmentált a diagramokon, így is ki lehet külön vagy a halmozott diagramok a különböző játékok számára.</span><span class="sxs-lookup"><span data-stu-id="d6743-341">The graphs can be segmented by the properties that are sent with the event, so that you can get separate or stacked graphs for different games.</span></span>

<span data-ttu-id="d6743-342">A metrika értékek megfelelően megjeleníteni össze kell kisebb 0-nál nagyobb.</span><span class="sxs-lookup"><span data-stu-id="d6743-342">For metric values to be correctly displayed, they should be greater than or equal to 0.</span></span>

<span data-ttu-id="d6743-343">Van azonban néhány [tulajdonságait, a tulajdonságértékeket és a metrikák száma vonatkozó korlátozások](#limits) használható.</span><span class="sxs-lookup"><span data-stu-id="d6743-343">There are some [limits on the number of properties, property values, and metrics](#limits) that you can use.</span></span>

<span data-ttu-id="d6743-344">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="d6743-344">*JavaScript*</span></span>

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


<span data-ttu-id="d6743-345">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-345">*C#*</span></span>

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send the event:
    telemetry.TrackEvent("WinGame", properties, metrics);


<span data-ttu-id="d6743-346">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="d6743-346">*Visual Basic*</span></span>

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send the event:
    telemetry.TrackEvent("WinGame", properties, metrics)


<span data-ttu-id="d6743-347">*Java*</span><span class="sxs-lookup"><span data-stu-id="d6743-347">*Java*</span></span>

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> <span data-ttu-id="d6743-348">Ügyeljen arra, nem tulajdonságok-e jelentkezni személyes azonosításra alkalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6743-348">Take care not to log personally identifiable information in properties.</span></span>
>
>

<span data-ttu-id="d6743-349">*Ha követte a metrikák*, nyissa meg a Metrikaböngésző, és válassza ki a a a **egyéni** csoport:</span><span class="sxs-lookup"><span data-stu-id="d6743-349">*If you used metrics*, open Metrics Explorer and select the metric from the **Custom** group:</span></span>

![Nyissa meg a Metrikaböngésző, válassza ki a diagramot, és válassza ki a](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> <span data-ttu-id="d6743-351">Ha a metrika nem jelenik meg, vagy ha a **egyéni** fejléc nem létezik, zárja be a kijelölés panelt, és próbálkozzon újra később.</span><span class="sxs-lookup"><span data-stu-id="d6743-351">If your metric doesn't appear, or if the **Custom** heading isn't there, close the selection blade and try again later.</span></span> <span data-ttu-id="d6743-352">Metrikák néha időigényes egy órával összesítse a feldolgozási folyamaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="d6743-352">Metrics can sometimes take an hour to be aggregated through the pipeline.</span></span>

<span data-ttu-id="d6743-353">*Ha követte a tulajdonságok és a metrikák*, szegmentálja a metrika a tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="d6743-353">*If you used properties and metrics*, segment the metric by the property:</span></span>

![Állítsa be a csoportosítás, majd válassza ki a tulajdonság szerinti csoportosítás](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

<span data-ttu-id="d6743-355">*A diagnosztikai keresési*, megtekintheti a tulajdonságok és az esemény egyedi előfordulását.</span><span class="sxs-lookup"><span data-stu-id="d6743-355">*In Diagnostic Search*, you can view the properties and metrics of individual occurrences of an event.</span></span>

![Válasszon ki egy példányt, és adja meg a "..."](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

<span data-ttu-id="d6743-357">Használja a **keresési** mező esemény eseményeket, amelyek egy adott tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="d6743-357">Use the **Search** field to see event occurrences that have a particular property value.</span></span>

![Írja be a keresőkifejezést keresési rendszerbe](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

<span data-ttu-id="d6743-359">[További információ a keresési kifejezések](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="d6743-359">[Learn more about search expressions](app-insights-diagnostic-search.md).</span></span>

### <a name="alternative-way-to-set-properties-and-metrics"></a><span data-ttu-id="d6743-360">Tulajdonságok és a metrikák alternatív módja</span><span class="sxs-lookup"><span data-stu-id="d6743-360">Alternative way to set properties and metrics</span></span>
<span data-ttu-id="d6743-361">Sokkal kényelmesebb, ha egy külön objektumban esemény paramétereinek hozhatja létre:</span><span class="sxs-lookup"><span data-stu-id="d6743-361">If it's more convenient, you can collect the parameters of an event in a separate object:</span></span>

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> <span data-ttu-id="d6743-362">A telemetriai adatok elem példányt nem használja fel (`event` ebben a példában) Track*() hívása többször.</span><span class="sxs-lookup"><span data-stu-id="d6743-362">Don't reuse the same telemetry item instance (`event` in this example) to call Track*() multiple times.</span></span> <span data-ttu-id="d6743-363">Emiatt a telemetriai adatok küldését a helytelen konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d6743-363">This may cause telemetry to be sent with incorrect configuration.</span></span>
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a><span data-ttu-id="d6743-364">Egyéni mértékek és az elemzés tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="d6743-364">Custom measurements and properties in Analytics</span></span>

<span data-ttu-id="d6743-365">A [Analytics](app-insights-analytics.md), egyéni metrikákkal és tulajdonságok megjelenítése a `customMeasurements` és `customDimensions` telemetriai rekordokban attribútumait.</span><span class="sxs-lookup"><span data-stu-id="d6743-365">In [Analytics](app-insights-analytics.md), custom metrics and properties show in the `customMeasurements` and `customDimensions` attributes of each telemetry record.</span></span>

<span data-ttu-id="d6743-366">Például hozzáadta a – kéréstelemetria "játék" nevű tulajdonságot, ha a lekérdezés eltérő értékeiből álló "játék" előfordulások száma, és az egyéni metrika "pontszám" átlaga megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="d6743-366">For example, if you have added a property named "game" to your request telemetry, this query counts the occurrences of different values of "game", and show the average of the custom metric "score":</span></span>

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

<span data-ttu-id="d6743-367">Figyelje meg, hogy:</span><span class="sxs-lookup"><span data-stu-id="d6743-367">Notice that:</span></span>

* <span data-ttu-id="d6743-368">Egy érték a customDimensions vagy customMeasurements JSON kibontásakor dinamikus típust, és úgy kell alakítania azt `tostring` vagy `todouble`.</span><span class="sxs-lookup"><span data-stu-id="d6743-368">When you extract a value from the customDimensions or customMeasurements JSON, it has dynamic type, and so you must cast it `tostring` or `todouble`.</span></span>
* <span data-ttu-id="d6743-369">Annak a lehetőségét figyelembe [mintavételi](app-insights-sampling.md), használjon `sum(itemCount)`, nem `count()`.</span><span class="sxs-lookup"><span data-stu-id="d6743-369">To take account of the possibility of [sampling](app-insights-sampling.md), you should use `sum(itemCount)`, not `count()`.</span></span>



## <span data-ttu-id="d6743-370"><a name="timed"></a>Időzítési események</span><span class="sxs-lookup"><span data-stu-id="d6743-370"><a name="timed"></a> Timing events</span></span>
<span data-ttu-id="d6743-371">Egyes esetekben kívánt diagram, hogy mennyi ideig tart egy műveletet.</span><span class="sxs-lookup"><span data-stu-id="d6743-371">Sometimes you want to chart how long it takes to perform an action.</span></span> <span data-ttu-id="d6743-372">Például előfordulhat, hogy szeretné tudni, hogy a felhasználók mennyi ideig kell figyelembe venni a választási lehetőségek egy játékban hajtsa végre a megfelelő.</span><span class="sxs-lookup"><span data-stu-id="d6743-372">For example, you might want to know how long users take to consider choices in a game.</span></span> <span data-ttu-id="d6743-373">Ehhez használhatja a mérési paraméter.</span><span class="sxs-lookup"><span data-stu-id="d6743-373">You can use the measurement parameter for this.</span></span>

<span data-ttu-id="d6743-374">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-374">*C#*</span></span>

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform the timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send the event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);



## <span data-ttu-id="d6743-375"><a name="defaults"></a>Egyéni telemetria alapértelmezett tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="d6743-375"><a name="defaults"></a>Default properties for custom telemetry</span></span>
<span data-ttu-id="d6743-376">Ha tulajdonságértékeit állítja be alapértelmezett néhány, az egyéni események írást, akkor TelemetryClient példányt teheti meg.</span><span class="sxs-lookup"><span data-stu-id="d6743-376">If you want to set default property values for some of the custom events that you write, you can set them in a TelemetryClient instance.</span></span> <span data-ttu-id="d6743-377">Minden telemetriai elemet, hogy az ügyfél által küldött vannak csatolva.</span><span class="sxs-lookup"><span data-stu-id="d6743-377">They are attached to every telemetry item that's sent from that client.</span></span>

<span data-ttu-id="d6743-378">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-378">*C#*</span></span>

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with the context property:
    gameTelemetry.TrackEvent("WinGame");

<span data-ttu-id="d6743-379">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="d6743-379">*Visual Basic*</span></span>

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with the context property:
    gameTelemetry.TrackEvent("WinGame")

<span data-ttu-id="d6743-380">*Java*</span><span class="sxs-lookup"><span data-stu-id="d6743-380">*Java*</span></span>

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");



<span data-ttu-id="d6743-381">Egyéni telemetria hívások felülbírálhatja az alapértelmezett értékeket a tulajdonság szótárak.</span><span class="sxs-lookup"><span data-stu-id="d6743-381">Individual telemetry calls can override the default values in their property dictionaries.</span></span>

<span data-ttu-id="d6743-382">*JavaScript a webes ügyfelek*, [JavaScript telemetriai inicializálók használja](#js-initializer).</span><span class="sxs-lookup"><span data-stu-id="d6743-382">*For JavaScript web clients*, [use JavaScript telemetry initializers](#js-initializer).</span></span>

<span data-ttu-id="d6743-383">*Tulajdonságok hozzáadása az összes telemetriai adat*, beleértve a szabványos gyűjtési modulok adatait [megvalósítása `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).</span><span class="sxs-lookup"><span data-stu-id="d6743-383">*To add properties to all telemetry*, including the data from standard collection modules, [implement `ITelemetryInitializer`](app-insights-api-filtering-sampling.md#add-properties).</span></span>

## <a name="sampling-filtering-and-processing-telemetry"></a><span data-ttu-id="d6743-384">A mintavételi, szűrési és telemetriai adatainak feldolgozása</span><span class="sxs-lookup"><span data-stu-id="d6743-384">Sampling, filtering, and processing telemetry</span></span>
<span data-ttu-id="d6743-385">Írhat kódot a telemetriai adatok feldolgozásához, az SDK-ból elküldés előtt.</span><span class="sxs-lookup"><span data-stu-id="d6743-385">You can write code to process your telemetry before it's sent from the SDK.</span></span> <span data-ttu-id="d6743-386">A feldolgozás része a szabványos telemetriai modulok, például HTTP kérelem adatgyűjtési és -függőség gyűjtemény által küldött adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6743-386">The processing includes data that's sent from the standard telemetry modules, such as HTTP request collection and dependency collection.</span></span>

<span data-ttu-id="d6743-387">[Adja hozzá a Tulajdonságok](app-insights-api-filtering-sampling.md#add-properties) való alkalmazásával telemetriai `ITelemetryInitializer`.</span><span class="sxs-lookup"><span data-stu-id="d6743-387">[Add properties](app-insights-api-filtering-sampling.md#add-properties) to telemetry by implementing `ITelemetryInitializer`.</span></span> <span data-ttu-id="d6743-388">Például hozzáadhat verziószámok vagy számított értékeket más tulajdonságai közül.</span><span class="sxs-lookup"><span data-stu-id="d6743-388">For example, you can add version numbers or values that are calculated from other properties.</span></span>

<span data-ttu-id="d6743-389">[Szűrés](app-insights-api-filtering-sampling.md#filtering) módosíthatja vagy vesse el a telemetriai adatokat az SDK-ból implementálásával elküldése előtt `ITelemetryProcesor`.</span><span class="sxs-lookup"><span data-stu-id="d6743-389">[Filtering](app-insights-api-filtering-sampling.md#filtering) can modify or discard telemetry before it's sent from the SDK by implementing `ITelemetryProcesor`.</span></span> <span data-ttu-id="d6743-390">Megadhatja, mi küldik vagy elvetett, de a fiókot használja a metrikákat, hatással van.</span><span class="sxs-lookup"><span data-stu-id="d6743-390">You control what is sent or discarded, but you have to account for the effect on your metrics.</span></span> <span data-ttu-id="d6743-391">Attól függően, hogy hogyan elveti elemek kapcsolódó elemek közötti navigáláshoz képes elveszhetnek.</span><span class="sxs-lookup"><span data-stu-id="d6743-391">Depending on how you discard items, you might lose the ability to navigate between related items.</span></span>

<span data-ttu-id="d6743-392">[A mintavételi](app-insights-api-filtering-sampling.md) egy csomagolt megoldás a portálra az alkalmazás által küldött adatok mennyisége csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="d6743-392">[Sampling](app-insights-api-filtering-sampling.md) is a packaged solution to reduce the volume of data that's sent from your app to the portal.</span></span> <span data-ttu-id="d6743-393">Igen, a megjelenő metrikák befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="d6743-393">It does so without affecting the displayed metrics.</span></span> <span data-ttu-id="d6743-394">És így nem befolyásolja a problémák diagnosztizálásához például kivételek, a kérelmek és az oldalmegtekintéseket kapcsolódó elemek közötti lépjen arra a képességére kezeli.</span><span class="sxs-lookup"><span data-stu-id="d6743-394">And it does so without affecting your ability to diagnose problems by navigating between related items such as exceptions, requests, and page views.</span></span>

<span data-ttu-id="d6743-395">[További információk](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="d6743-395">[Learn more](app-insights-api-filtering-sampling.md).</span></span>

## <a name="disabling-telemetry"></a><span data-ttu-id="d6743-396">Telemetria letiltása</span><span class="sxs-lookup"><span data-stu-id="d6743-396">Disabling telemetry</span></span>
<span data-ttu-id="d6743-397">A *dinamikusan leállítására és elindítására* a gyűjtemény és a telemetriai adatok továbbítása:</span><span class="sxs-lookup"><span data-stu-id="d6743-397">To *dynamically stop and start* the collection and transmission of telemetry:</span></span>

<span data-ttu-id="d6743-398">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-398">*C#*</span></span>

```C#

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

<span data-ttu-id="d6743-399">A *tiltsa le a kiválasztott szabványos gyűjtők*– például teljesítményszámlálókat, HTTP-kérelmek vagy függőségek--törlése vagy a megfelelő sorok megjegyzéssé [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Ehhez, például, ha azt szeretné, hogy a saját TrackRequest adatküldéshez.</span><span class="sxs-lookup"><span data-stu-id="d6743-399">To *disable selected standard collectors*--for example, performance counters, HTTP requests, or dependencies--delete or comment out the relevant lines in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). You can do this, for example, if you want to send your own TrackRequest data.</span></span>

## <span data-ttu-id="d6743-400"><a name="debug"></a>Fejlesztői mód</span><span class="sxs-lookup"><span data-stu-id="d6743-400"><a name="debug"></a>Developer mode</span></span>
<span data-ttu-id="d6743-401">Hibakeresési, az hasznos lehet a telemetriai adatok sürgős keresztül, hogy az eredmények azonnal láthatók.</span><span class="sxs-lookup"><span data-stu-id="d6743-401">During debugging, it's useful to have your telemetry expedited through the pipeline so that you can see results immediately.</span></span> <span data-ttu-id="d6743-402">Akkor is get további üzeneteket, amelyek segítségével nyomon követni a telemetriai adatok problémákat.</span><span class="sxs-lookup"><span data-stu-id="d6743-402">You also get additional messages that help you trace any problems with the telemetry.</span></span> <span data-ttu-id="d6743-403">Kapcsolja ki a termelési, mert az alkalmazás lassíthatja.</span><span class="sxs-lookup"><span data-stu-id="d6743-403">Switch it off in production, because it may slow down your app.</span></span>

<span data-ttu-id="d6743-404">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-404">*C#*</span></span>

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

<span data-ttu-id="d6743-405">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="d6743-405">*Visual Basic*</span></span>

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <span data-ttu-id="d6743-406"><a name="ikey"></a>A kijelölt egyéni telemetria instrumentation kulcs beállítása</span><span class="sxs-lookup"><span data-stu-id="d6743-406"><a name="ikey"></a> Setting the instrumentation key for selected custom telemetry</span></span>
<span data-ttu-id="d6743-407">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-407">*C#*</span></span>

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <span data-ttu-id="d6743-408"><a name="dynamic-ikey"></a>Dinamikus instrumentation kulcs</span><span class="sxs-lookup"><span data-stu-id="d6743-408"><a name="dynamic-ikey"></a> Dynamic instrumentation key</span></span>
<span data-ttu-id="d6743-409">Felfelé telemetriai fejlesztési, tesztelési és éles környezetben keverése elkerüléséhez is [hozzon létre külön Application Insights-erőforrások](app-insights-create-new-resource.md) és azok kulcsait a környezettől függően módosítsa.</span><span class="sxs-lookup"><span data-stu-id="d6743-409">To avoid mixing up telemetry from development, test, and production environments, you can [create separate Application Insights resources](app-insights-create-new-resource.md) and change their keys, depending on the environment.</span></span>

<span data-ttu-id="d6743-410">Helyett a instrumentation kulcs lekérése a konfigurációs fájlban, beállíthatja a kódban.</span><span class="sxs-lookup"><span data-stu-id="d6743-410">Instead of getting the instrumentation key from the configuration file, you can set it in your code.</span></span> <span data-ttu-id="d6743-411">A kulcs egy inicializálási metódust, például egy ASP.NET-szolgáltatásban Global.aspx.cs osztályból meg:</span><span class="sxs-lookup"><span data-stu-id="d6743-411">Set the key in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

<span data-ttu-id="d6743-412">*C#*</span><span class="sxs-lookup"><span data-stu-id="d6743-412">*C#*</span></span>

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

<span data-ttu-id="d6743-413">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="d6743-413">*JavaScript*</span></span>

    appInsights.config.instrumentationKey = myKey;



<span data-ttu-id="d6743-414">A weboldalakon érdemes lehet, hogy állítson be úgy a webalkalmazás-kiszolgáló állapota, nem pedig kódolási szó a parancsprogramba a.</span><span class="sxs-lookup"><span data-stu-id="d6743-414">In webpages, you might want to set it from the web server's state, rather than coding it literally into the script.</span></span> <span data-ttu-id="d6743-415">Például a egy weblap ASP.NET alkalmazás hozott létre:</span><span class="sxs-lookup"><span data-stu-id="d6743-415">For example, in a webpage generated in an ASP.NET app:</span></span>

<span data-ttu-id="d6743-416">*JavaScript Razor*</span><span class="sxs-lookup"><span data-stu-id="d6743-416">*JavaScript in Razor*</span></span>

    <script type="text/javascript">
    // Standard Application Insights webpage script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="telemetrycontext"></a><span data-ttu-id="d6743-417">TelemetryContext</span><span class="sxs-lookup"><span data-stu-id="d6743-417">TelemetryContext</span></span>
<span data-ttu-id="d6743-418">TelemetryClient rendelkezik egy környezeti tulajdonság, amely az összes telemetriai adatokkal együtt küldött értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d6743-418">TelemetryClient has a Context property, which contains values that are sent along with all telemetry data.</span></span> <span data-ttu-id="d6743-419">Általában állította a szabványos telemetriai modulok, de is beállíthatja azokat saját maga.</span><span class="sxs-lookup"><span data-stu-id="d6743-419">They are normally set by the standard telemetry modules, but you can also set them yourself.</span></span> <span data-ttu-id="d6743-420">Példa:</span><span class="sxs-lookup"><span data-stu-id="d6743-420">For example:</span></span>

    telemetry.Context.Operation.Name = "MyOperationName";

<span data-ttu-id="d6743-421">Ha ezek bármelyike saját magának, fontolja meg a megfelelő sor eltávolítása [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), így a és a szabványos értékek nem Zavarba.</span><span class="sxs-lookup"><span data-stu-id="d6743-421">If you set any of these values yourself, consider removing the relevant line from [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), so that your values and the standard values don't get confused.</span></span>

* <span data-ttu-id="d6743-422">**Az összetevő**: az alkalmazás és annak verzióját.</span><span class="sxs-lookup"><span data-stu-id="d6743-422">**Component**: The app and its version.</span></span>
* <span data-ttu-id="d6743-423">**Eszköz**: adatok az eszközről, amelyen fut az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d6743-423">**Device**: Data about the device where the app is running.</span></span> <span data-ttu-id="d6743-424">(A webalkalmazásokban, ez az a kiszolgáló vagy a telemetriai adatok által küldött ügyfél-eszköz.)</span><span class="sxs-lookup"><span data-stu-id="d6743-424">(In web apps, this is the server or client device that the telemetry is sent from.)</span></span>
* <span data-ttu-id="d6743-425">**InstrumentationKey**: az Application Insights-erőforrás hol jelenjenek meg a telemetriai adatok Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d6743-425">**InstrumentationKey**: The Application Insights resource in Azure where the telemetry appear.</span></span> <span data-ttu-id="d6743-426">Ez általában felvételre az ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="d6743-426">It's usually picked up from ApplicationInsights.config.</span></span>
* <span data-ttu-id="d6743-427">**Hely**: az eszköz földrajzi helye.</span><span class="sxs-lookup"><span data-stu-id="d6743-427">**Location**: The geographic location of the device.</span></span>
* <span data-ttu-id="d6743-428">**A művelet**: A webalkalmazásokban, az aktuális HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="d6743-428">**Operation**: In web apps, the current HTTP request.</span></span> <span data-ttu-id="d6743-429">A más típusú alkalmazás állíthat események együtt.</span><span class="sxs-lookup"><span data-stu-id="d6743-429">In other app types, you can set this to group events together.</span></span>
  * <span data-ttu-id="d6743-430">**Azonosító**: egy generált érték különböző események hibához, így minden esetben a diagnosztikai keresési vizsgálja meg, ha található kapcsolódó elemeket.</span><span class="sxs-lookup"><span data-stu-id="d6743-430">**Id**: A generated value that correlates different events, so that when you inspect any event in Diagnostic Search, you can find related items.</span></span>
  * <span data-ttu-id="d6743-431">**Név**: azonosítót, általában az URL-cím a HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="d6743-431">**Name**: An identifier, usually the URL of the HTTP request.</span></span>
  * <span data-ttu-id="d6743-432">**SyntheticSource**: Ha nem null értékű vagy üres karakterlánc, amely azt jelzi, hogy a kérelem forrását robot vagy a webalkalmazás tesztjének néven azonosított.</span><span class="sxs-lookup"><span data-stu-id="d6743-432">**SyntheticSource**: If not null or empty, a string that indicates that the source of the request has been identified as a robot or web test.</span></span> <span data-ttu-id="d6743-433">Alapértelmezés szerint az ki lesz zárva a Metrikaböngészőben számításból.</span><span class="sxs-lookup"><span data-stu-id="d6743-433">By default, it is excluded from calculations in Metrics Explorer.</span></span>
* <span data-ttu-id="d6743-434">**Tulajdonságok**: az összes telemetriai adatokat küldött tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="d6743-434">**Properties**: Properties that are sent with all telemetry data.</span></span> <span data-ttu-id="d6743-435">Az egyes követése * hívások felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="d6743-435">It can be overridden in individual Track* calls.</span></span>
* <span data-ttu-id="d6743-436">**Munkamenet**: A felhasználói munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="d6743-436">**Session**: The user's session.</span></span> <span data-ttu-id="d6743-437">Az azonosító értéke egy generált érték, amely megváltozik, miközben a felhasználó nem volt aktív egy ideig.</span><span class="sxs-lookup"><span data-stu-id="d6743-437">The ID is set to a generated value, which is changed when the user has not been active for a while.</span></span>
* <span data-ttu-id="d6743-438">**Felhasználói**: felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6743-438">**User**: User information.</span></span>

## <a name="limits"></a><span data-ttu-id="d6743-439">Korlátok</span><span class="sxs-lookup"><span data-stu-id="d6743-439">Limits</span></span>
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

<span data-ttu-id="d6743-440">Szerezze meg a sávszélesség-korlátjának elkerüléséhez használja [mintavételi](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="d6743-440">To avoid hitting the data rate limit, use [sampling](app-insights-sampling.md).</span></span>

<span data-ttu-id="d6743-441">Annak meghatározásához, hogy mennyi ideig megtartja adatokat, lásd: [az adatmegőrzés és az adatvédelmi](app-insights-data-retention-privacy.md).</span><span class="sxs-lookup"><span data-stu-id="d6743-441">To determine how long data is kept, see [Data retention and privacy](app-insights-data-retention-privacy.md).</span></span>

## <a name="reference-docs"></a><span data-ttu-id="d6743-442">Segédanyagok</span><span class="sxs-lookup"><span data-stu-id="d6743-442">Reference docs</span></span>
* [<span data-ttu-id="d6743-443">ASP.NET-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="d6743-443">ASP.NET reference</span></span>](https://msdn.microsoft.com/library/dn817570.aspx)
* [<span data-ttu-id="d6743-444">Java dokumentáció</span><span class="sxs-lookup"><span data-stu-id="d6743-444">Java reference</span></span>](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [<span data-ttu-id="d6743-445">JavaScript-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="d6743-445">JavaScript reference</span></span>](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [<span data-ttu-id="d6743-446">Android SDK</span><span class="sxs-lookup"><span data-stu-id="d6743-446">Android SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Android)
* [<span data-ttu-id="d6743-447">iOS SDK</span><span class="sxs-lookup"><span data-stu-id="d6743-447">iOS SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a><span data-ttu-id="d6743-448">SDK-kód</span><span class="sxs-lookup"><span data-stu-id="d6743-448">SDK code</span></span>
* [<span data-ttu-id="d6743-449">Az ASP.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="d6743-449">ASP.NET Core SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [<span data-ttu-id="d6743-450">ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="d6743-450">ASP.NET 5</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [<span data-ttu-id="d6743-451">Windows Server csomagok</span><span class="sxs-lookup"><span data-stu-id="d6743-451">Windows Server packages</span></span>](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [<span data-ttu-id="d6743-452">Java SDK</span><span class="sxs-lookup"><span data-stu-id="d6743-452">Java SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Java)
* [<span data-ttu-id="d6743-453">JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="d6743-453">JavaScript SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-JS)
* [<span data-ttu-id="d6743-454">Összes platform</span><span class="sxs-lookup"><span data-stu-id="d6743-454">All platforms</span></span>](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a><span data-ttu-id="d6743-455">Kérdések</span><span class="sxs-lookup"><span data-stu-id="d6743-455">Questions</span></span>
* <span data-ttu-id="d6743-456">*Milyen kivételek előfordulhat, hogy throw Track_() hívások?*</span><span class="sxs-lookup"><span data-stu-id="d6743-456">*What exceptions might Track_() calls throw?*</span></span>

    <span data-ttu-id="d6743-457">nincs.</span><span class="sxs-lookup"><span data-stu-id="d6743-457">None.</span></span> <span data-ttu-id="d6743-458">Ezeket csomagolásához a try-catch záradékban nem kell.</span><span class="sxs-lookup"><span data-stu-id="d6743-458">You don't need to wrap them in try-catch clauses.</span></span> <span data-ttu-id="d6743-459">Ha az SDK problémákat tapasztal, üzenetek naplózza a hibakeresési konzol kimeneti és – ha az üzenetek beolvasása használatával – diagnosztikai keresésben.</span><span class="sxs-lookup"><span data-stu-id="d6743-459">If the SDK encounters problems, it will log messages in the debug console output and--if the messages get through--in Diagnostic Search.</span></span>
* <span data-ttu-id="d6743-460">*Van egy REST API-t adatok beszerzése a portálról?*</span><span class="sxs-lookup"><span data-stu-id="d6743-460">*Is there a REST API to get data from the portal?*</span></span>

    <span data-ttu-id="d6743-461">Igen, a [adatelérési API](https://dev.applicationinsights.io/).</span><span class="sxs-lookup"><span data-stu-id="d6743-461">Yes, the [data access API](https://dev.applicationinsights.io/).</span></span> <span data-ttu-id="d6743-462">Adatok kinyerése segítségével [Analytics exportálása a Power bi-bA](app-insights-export-power-bi.md) és [a folyamatos exportálás](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="d6743-462">Other ways to extract data include [export from Analytics to Power BI](app-insights-export-power-bi.md) and [continuous export](app-insights-export-telemetry.md).</span></span>

## <span data-ttu-id="d6743-463"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6743-463"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="d6743-464">Keresési események és a naplókat</span><span class="sxs-lookup"><span data-stu-id="d6743-464">Search events and logs</span></span>](app-insights-diagnostic-search.md)

* [<span data-ttu-id="d6743-465">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="d6743-465">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)



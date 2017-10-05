---
title: "Keresés használata az Azure Application Insightsban |} Microsoft Docs"
description: "Keresés és szűrés nyers telemetriaadatok küldött a webes alkalmazást."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: e2d12f807756b778a64920b12a66fba184a99844
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="using-search-in-application-insights"></a><span data-ttu-id="09f86-103">Az Application Insightsban keresés használata</span><span class="sxs-lookup"><span data-stu-id="09f86-103">Using Search in Application Insights</span></span>
<span data-ttu-id="09f86-104">Keresés csak a [Application Insights](app-insights-overview.md) található és egyéni telemetriai elemek, például Lapmegtekintések, kivételek, így megismerkedhet vagy kérelmek használható.</span><span class="sxs-lookup"><span data-stu-id="09f86-104">Search is a feature of [Application Insights](app-insights-overview.md) that you use to find and explore individual telemetry items, such as page views, exceptions, or web requests.</span></span> <span data-ttu-id="09f86-105">És naplókivonatokat és eseményeket, amelyek rendelkeznek a kódolt megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="09f86-105">And you can view log traces and events that you have coded.</span></span>

<span data-ttu-id="09f86-106">(Az adatok a összetettebb lekérdezések, használjon [Analytics](app-insights-analytics-tour.md).)</span><span class="sxs-lookup"><span data-stu-id="09f86-106">(For more complex queries over your data, use [Analytics](app-insights-analytics-tour.md).)</span></span>

## <a name="where-do-you-see-search"></a><span data-ttu-id="09f86-107">Ha látja keresési?</span><span class="sxs-lookup"><span data-stu-id="09f86-107">Where do you see Search?</span></span>
### <a name="in-the-azure-portal"></a><span data-ttu-id="09f86-108">Az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="09f86-108">In the Azure portal</span></span>
<span data-ttu-id="09f86-109">Az Application Insights – áttekintés panel az alkalmazás a explicit módon nyithatja diagnosztikai keresés:</span><span class="sxs-lookup"><span data-stu-id="09f86-109">You can open diagnostic search explicitly from the Application Insights Overview blade of your application:</span></span>

![Diagnosztikai keresés megnyitása](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

<span data-ttu-id="09f86-111">Néhány diagramok és a rács elemek kattintva is megnyílik.</span><span class="sxs-lookup"><span data-stu-id="09f86-111">It also opens when you click through some charts and grid items.</span></span> <span data-ttu-id="09f86-112">Ebben az esetben a szűrők vannak előre beállított összpontosítani a kijelölt elem típusát.</span><span class="sxs-lookup"><span data-stu-id="09f86-112">In this case, its filters are pre-set to focus on the type of item you selected.</span></span> 

<span data-ttu-id="09f86-113">Például az Áttekintés panel nincs kérelmek válaszideje által besorolt sávdiagram.</span><span class="sxs-lookup"><span data-stu-id="09f86-113">For example, on the Overview blade, there's a bar chart of requests classified by response time.</span></span> <span data-ttu-id="09f86-114">Kattintson a teljesítmény széles egyedi adott válasz időtartományba esik tartalmazó lista megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="09f86-114">Click through a performance range to see a list of individual requests in that response time range:</span></span>

![Kattintson a kérelem teljesítmény](./media/app-insights-diagnostic-search/07-open-from-filters.png)

<span data-ttu-id="09f86-116">A fő diagnosztikai keresési szövegtörzse listáját telemetriai elemek - kiszolgálói kérelmek lapon a nézetek, egyéni események, amelyek rendelkeznek a kódolt és így tovább.</span><span class="sxs-lookup"><span data-stu-id="09f86-116">The main body of Diagnostic Search is a list of telemetry items - server requests, page views, custom events that you have coded, and so on.</span></span> <span data-ttu-id="09f86-117">A lista tetején van egy összefoglaló táblázat időbeli események száma.</span><span class="sxs-lookup"><span data-stu-id="09f86-117">At the top of the list is a summary chart showing counts of events over time.</span></span>

<span data-ttu-id="09f86-118">Kattintson a frissítés gombra új esemény lekérdezésekor.</span><span class="sxs-lookup"><span data-stu-id="09f86-118">Click Refresh to get new events.</span></span>

### <a name="in-visual-studio"></a><span data-ttu-id="09f86-119">Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="09f86-119">In Visual Studio</span></span>

<span data-ttu-id="09f86-120">A Visual Studio esetében is az Application Insights keresési ablak.</span><span class="sxs-lookup"><span data-stu-id="09f86-120">In Visual Studio, there's also an Application Insights Search window.</span></span> <span data-ttu-id="09f86-121">Legtöbb célszerű akkor hibakeresése alkalmazás által generált telemetriai események megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="09f86-121">It's most useful for displaying telemetry events generated by the application that you're debugging.</span></span> <span data-ttu-id="09f86-122">Azonban akkor is megjelenhet a közzétett alkalmazást az Azure-portálon az összegyűjtött események.</span><span class="sxs-lookup"><span data-stu-id="09f86-122">But it can also show the events collected from your published app at the Azure portal.</span></span>

<span data-ttu-id="09f86-123">Nyissa meg a keresési ablak a Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="09f86-123">Open the Search window in Visual Studio:</span></span>

![A Visual Studio Application Insights keresés megnyitása](./media/app-insights-diagnostic-search/32.png)

<span data-ttu-id="09f86-125">A keresési ablak a webes portálhoz hasonló jellemzőkkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="09f86-125">The Search window has features similar to the web portal:</span></span>

![Visual Studio Application Insights – keresési ablak](./media/app-insights-diagnostic-search/34.png)

<span data-ttu-id="09f86-127">A Track művelet lapon érhető el egy kérelem vagy egy nézet megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="09f86-127">The Track Operation tab is available when you open a request or a page view.</span></span> <span data-ttu-id="09f86-128">Egy "művelet" egyetlen kérelemmel vagy a lap nézetre kapcsolódó események sorozata.</span><span class="sxs-lookup"><span data-stu-id="09f86-128">An 'operation' is a sequence of events that is associated with to a single request or page view.</span></span> <span data-ttu-id="09f86-129">Például függőségi hívások esetében, kivételek, nyomkövetési naplókat és egyéni események része lehet egyetlen művelettel.</span><span class="sxs-lookup"><span data-stu-id="09f86-129">For example, dependency calls, exceptions, trace logs, and custom events might be part of a single operation.</span></span> <span data-ttu-id="09f86-130">A művelet nyomon követése lap grafikonját időzítése és ezeket az eseményeket a kérelem vagy a lap nézet viszonyítva időtartama.</span><span class="sxs-lookup"><span data-stu-id="09f86-130">The Track Operation tab shows graphically the timing and duration of these events in relation to the request or page view.</span></span> 

## <a name="inspect-individual-items"></a><span data-ttu-id="09f86-131">Vizsgálja meg az egyes elemek</span><span class="sxs-lookup"><span data-stu-id="09f86-131">Inspect individual items</span></span>
<span data-ttu-id="09f86-132">Válassza ki bármelyik telemetriai elemre kulcsmezők megtekintéséhez és a kapcsolódó elemek.</span><span class="sxs-lookup"><span data-stu-id="09f86-132">Select any telemetry item to see key fields and related items.</span></span> <span data-ttu-id="09f86-133">Ha meg szeretné tekinteni a mezők teljes készletét, kattintson a "...".</span><span class="sxs-lookup"><span data-stu-id="09f86-133">If you want to see the full set of fields, click "...".</span></span> 

![Kattintson az új munkaelemre vonatkozóan, módosítsa a mezőket, és kattintson az OK gombra.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a><span data-ttu-id="09f86-135">Szűrő eseménytípusok</span><span class="sxs-lookup"><span data-stu-id="09f86-135">Filter event types</span></span>
<span data-ttu-id="09f86-136">Nyissa meg a szűrő panelre, és válassza ki a megjeleníteni kívánt típusait.</span><span class="sxs-lookup"><span data-stu-id="09f86-136">Open the Filter blade and choose the event types you want to see.</span></span> <span data-ttu-id="09f86-137">(Ha később szeretné visszaállítani a szűrőket, amellyel megnyitni a panelt, kattintson a visszaállítás elemre.)</span><span class="sxs-lookup"><span data-stu-id="09f86-137">(If, later, you want to restore the filters with which you opened the blade, click Reset.)</span></span>

![Kattintson a szűrő és telemetriai kijelölve](./media/app-insights-diagnostic-search/02-filter-req.png)

<span data-ttu-id="09f86-139">Az esemény típusok a következők:</span><span class="sxs-lookup"><span data-stu-id="09f86-139">The event types are:</span></span>

* <span data-ttu-id="09f86-140">**Nyomkövetési** - [diagnosztikai naplók](app-insights-asp-net-trace-logs.md) TrackTrace, log4Net, NLog és System.Diagnostic.Trace hívások beleértve.</span><span class="sxs-lookup"><span data-stu-id="09f86-140">**Trace** - [Diagnostic logs](app-insights-asp-net-trace-logs.md) including TrackTrace, log4Net, NLog, and System.Diagnostic.Trace calls.</span></span>
* <span data-ttu-id="09f86-141">**Kérelem** -HTTP-kérések a kiszolgálóalkalmazás, beleértve a lapok, parancsfájlok, képek, stílus fájlokat és adatokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="09f86-141">**Request** - HTTP requests received by your server application, including pages, scripts, images, style files, and data.</span></span> <span data-ttu-id="09f86-142">Ezek az események létrehozásához használt a kérelem-válasz áttekintő diagramok.</span><span class="sxs-lookup"><span data-stu-id="09f86-142">These events are used to create the request and response overview charts.</span></span>
* <span data-ttu-id="09f86-143">**Lapmegtekintés** - [a webes ügyfél által küldött Telemetriai](app-insights-javascript.md), felhasznált lap megtekintése jelentések létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="09f86-143">**Page View** - [Telemetry sent by the web client](app-insights-javascript.md), used to create page view reports.</span></span> 
* <span data-ttu-id="09f86-144">**Egyéni esemény** – Ha a trackevent() függvény annak érdekében, hogy hívásainak beszúrt [megfigyeléséhez](app-insights-api-custom-events-metrics.md), Itt kereshet bennük.</span><span class="sxs-lookup"><span data-stu-id="09f86-144">**Custom Event** - If you inserted calls to TrackEvent() in order to [monitor usage](app-insights-api-custom-events-metrics.md), you can search them here.</span></span>
* <span data-ttu-id="09f86-145">**Kivétel** – fellépő nem kezelt [kivételek a kiszolgálón](app-insights-asp-net-exceptions.md), valamint azokat, amelyeket a TrackException() használatával jelentkezik.</span><span class="sxs-lookup"><span data-stu-id="09f86-145">**Exception** - Uncaught [exceptions in the server](app-insights-asp-net-exceptions.md), and those that you log by using TrackException().</span></span>
* <span data-ttu-id="09f86-146">**A függőségi** - [a kiszolgálóalkalmazás hívásait](app-insights-asp-net-dependencies.md) más szolgáltatások, például REST API-k vagy adatbázisok és AJAX hívásait a [Ügyfélkód](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="09f86-146">**Dependency** - [Calls from your server application](app-insights-asp-net-dependencies.md) to other services such as REST APIs or databases, and AJAX calls from your [client code](app-insights-javascript.md).</span></span>
* <span data-ttu-id="09f86-147">**Rendelkezésre állási** -eredményeinek [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="09f86-147">**Availability** - Results of [availability tests](app-insights-monitor-web-app-availability.md).</span></span>

## <a name="filter-on-property-values"></a><span data-ttu-id="09f86-148">A tulajdonságértékek szűrése</span><span class="sxs-lookup"><span data-stu-id="09f86-148">Filter on property values</span></span>
<span data-ttu-id="09f86-149">A tulajdonságok értékeit események jeleníthetők meg.</span><span class="sxs-lookup"><span data-stu-id="09f86-149">You can filter events on the values of their properties.</span></span> <span data-ttu-id="09f86-150">A rendelkezésre álló tulajdonságok a kijelölt esemény típusoktól függnek.</span><span class="sxs-lookup"><span data-stu-id="09f86-150">The available properties depend on the event types you selected.</span></span> 

<span data-ttu-id="09f86-151">Válasszon például adott válaszkód kéréseket.</span><span class="sxs-lookup"><span data-stu-id="09f86-151">For example, pick out requests with a specific response code.</span></span> 

![Bontsa ki a tulajdonságot, és adjon meg értéket](./media/app-insights-diagnostic-search/03-response500.png)

<span data-ttu-id="09f86-153">Ugyanaz a hatása, mint minden érték kiválasztása nem egy adott tulajdonság értékének rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="09f86-153">Choosing no values of a particular property has the same effect as choosing all values.</span></span> <span data-ttu-id="09f86-154">Vált ki a a tulajdonságon alapuló szűrőt.</span><span class="sxs-lookup"><span data-stu-id="09f86-154">It switches off filtering on that property.</span></span>

### <a name="narrow-your-search"></a><span data-ttu-id="09f86-155">A keresés szűkítéséhez</span><span class="sxs-lookup"><span data-stu-id="09f86-155">Narrow your search</span></span>
<span data-ttu-id="09f86-156">Figyelje meg, hogy a számlálás szűrőértékek jobb mutatja hány előfordulások nincs aktuális szűrt készletében.</span><span class="sxs-lookup"><span data-stu-id="09f86-156">Notice that the counts to the right of the filter values show how many occurrences there are in the current filtered set.</span></span> 

<span data-ttu-id="09f86-157">Az ebben a példában is egyértelmű, hogy a "Rpt/alkalmazottak" kérése az "500" hibák többségét eredményezi:</span><span class="sxs-lookup"><span data-stu-id="09f86-157">In this example, it's clear that the 'Rpt/Employees' request results in most of the '500' errors:</span></span>

![Bontsa ki a tulajdonságot, és adjon meg értéket](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-the-same-property"></a><span data-ttu-id="09f86-159">Az ugyanahhoz a tulajdonsághoz olyan esemény megkeresése</span><span class="sxs-lookup"><span data-stu-id="09f86-159">Find events with the same property</span></span>
<span data-ttu-id="09f86-160">Keresse meg elemeinek tulajdonság ugyanarra az értékre:</span><span class="sxs-lookup"><span data-stu-id="09f86-160">Find all the items with the same property value:</span></span>

![Kattintson a jobb gombbal egy tulajdonság](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-the-data"></a><span data-ttu-id="09f86-162">Az adatok keresése</span><span class="sxs-lookup"><span data-stu-id="09f86-162">Search the data</span></span>

> [!NOTE]
> <span data-ttu-id="09f86-163">Összetett lekérdezéseket írhat, nyissa meg a [ **Analytics** ](app-insights-analytics-tour.md) a keresési panel tetején.</span><span class="sxs-lookup"><span data-stu-id="09f86-163">To write more complex queries, open [**Analytics**](app-insights-analytics-tour.md) from the top of the Search blade.</span></span>
> 

<span data-ttu-id="09f86-164">A feltételek bármelyike tulajdonságértéket kereshet.</span><span class="sxs-lookup"><span data-stu-id="09f86-164">You can search for terms in any of the property values.</span></span> <span data-ttu-id="09f86-165">Ez különösen akkor hasznos, ha írt az [egyéni események](app-insights-api-custom-events-metrics.md) tulajdonság értékekkel.</span><span class="sxs-lookup"><span data-stu-id="09f86-165">This is particularly useful if you have written [custom events](app-insights-api-custom-events-metrics.md) with property values.</span></span> 

<span data-ttu-id="09f86-166">Előfordulhat, hogy be szeretné állítani a tartományon, mint rövidebb tartományban keresések gyorsabbak időpontot.</span><span class="sxs-lookup"><span data-stu-id="09f86-166">You might want to set a time range, as searches over a shorter range are faster.</span></span> 

![Diagnosztikai keresés megnyitása](./media/app-insights-diagnostic-search/appinsights-311search.png)

<span data-ttu-id="09f86-168">Teljes szavak, nem karakterláncrész keresése.</span><span class="sxs-lookup"><span data-stu-id="09f86-168">Search for complete words, not substrings.</span></span> <span data-ttu-id="09f86-169">Tegye idézőjelek közé tegye a speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="09f86-169">Use quotation marks to enclose special characters.</span></span>

| <span data-ttu-id="09f86-170">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="09f86-170">string</span></span> | <span data-ttu-id="09f86-171">van *nem* által talált</span><span class="sxs-lookup"><span data-stu-id="09f86-171">is *not* found by</span></span> | <span data-ttu-id="09f86-172">Ezek találja</span><span class="sxs-lookup"><span data-stu-id="09f86-172">but these do find it</span></span> |
| --- | --- | --- |
| <span data-ttu-id="09f86-173">HomeController.About</span><span class="sxs-lookup"><span data-stu-id="09f86-173">HomeController.About</span></span> |<span data-ttu-id="09f86-174">otthoni</span><span class="sxs-lookup"><span data-stu-id="09f86-174">home</span></span><br/><span data-ttu-id="09f86-175">Tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="09f86-175">controller</span></span><br/><span data-ttu-id="09f86-176">Kimenő</span><span class="sxs-lookup"><span data-stu-id="09f86-176">out</span></span> | <span data-ttu-id="09f86-177">homecontroller</span><span class="sxs-lookup"><span data-stu-id="09f86-177">homecontroller</span></span><br/><span data-ttu-id="09f86-178">tudnivalók</span><span class="sxs-lookup"><span data-stu-id="09f86-178">about</span></span><br/><span data-ttu-id="09f86-179">"homecontroller.about"</span><span class="sxs-lookup"><span data-stu-id="09f86-179">"homecontroller.about"</span></span>|
|<span data-ttu-id="09f86-180">Egyesült Államok</span><span class="sxs-lookup"><span data-stu-id="09f86-180">United States</span></span>|<span data-ttu-id="09f86-181">UNI</span><span class="sxs-lookup"><span data-stu-id="09f86-181">Uni</span></span><br/><span data-ttu-id="09f86-182">TED</span><span class="sxs-lookup"><span data-stu-id="09f86-182">ted</span></span>|<span data-ttu-id="09f86-183">Egyesült</span><span class="sxs-lookup"><span data-stu-id="09f86-183">united</span></span><br/><span data-ttu-id="09f86-184">állapotok</span><span class="sxs-lookup"><span data-stu-id="09f86-184">states</span></span><br/><span data-ttu-id="09f86-185">Egyesült Államok és</span><span class="sxs-lookup"><span data-stu-id="09f86-185">united AND states</span></span><br/><span data-ttu-id="09f86-186">"az Amerikai Egyesült Államok"</span><span class="sxs-lookup"><span data-stu-id="09f86-186">"united states"</span></span>

<span data-ttu-id="09f86-187">Az alábbiakban a keresési kifejezésre:</span><span class="sxs-lookup"><span data-stu-id="09f86-187">Here are the search expressions you can use:</span></span>

| <span data-ttu-id="09f86-188">Mintalekérdezés</span><span class="sxs-lookup"><span data-stu-id="09f86-188">Sample query</span></span> | <span data-ttu-id="09f86-189">Következmény</span><span class="sxs-lookup"><span data-stu-id="09f86-189">Effect</span></span> |
| --- | --- |
| `apple` |<span data-ttu-id="09f86-190">Összes esemény található egyéb mezőjének tartalmaznia a word "apple" időtartomány</span><span class="sxs-lookup"><span data-stu-id="09f86-190">Find all events in the time range whose fields include the word "apple"</span></span> |
| `apple AND banana` |<span data-ttu-id="09f86-191">Megkeresése, amelynek mindkét szavakat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="09f86-191">Find events that contain both words.</span></span> <span data-ttu-id="09f86-192">A tőkéhez "és", nem használható "és".</span><span class="sxs-lookup"><span data-stu-id="09f86-192">Use capital "AND", not "and".</span></span> |
| `apple OR banana`<br/>`apple banana` |<span data-ttu-id="09f86-193">Megkeresése, amelynek vagy szót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="09f86-193">Find events that contain either word.</span></span> <span data-ttu-id="09f86-194">"Vagy", nem használható "vagy".</span><span class="sxs-lookup"><span data-stu-id="09f86-194">Use "OR", not "or".</span></span><br/><span data-ttu-id="09f86-195">Rövid alak.</span><span class="sxs-lookup"><span data-stu-id="09f86-195">Short form.</span></span> |
| `apple NOT banana` |<span data-ttu-id="09f86-196">Esemény megkeresése, amelyek tartalmaznak egy szót, de a másik nem.</span><span class="sxs-lookup"><span data-stu-id="09f86-196">Find events that contain one word but not the other.</span></span> |



## <a name="sampling"></a><span data-ttu-id="09f86-197">Mintavételezés</span><span class="sxs-lookup"><span data-stu-id="09f86-197">Sampling</span></span>
<span data-ttu-id="09f86-198">Ha az alkalmazás nagy mennyiségű telemetriai adatokat hoz létre (és az ASP.NET SDK verzió 2.0.0-beta3 használ vagy újabb), a adaptív mintavételi modul automatikusan csökkenti a Portal reprezentatív része események küldése által küldött.</span><span class="sxs-lookup"><span data-stu-id="09f86-198">If your app generates a lot of telemetry (and you are using the ASP.NET SDK version 2.0.0-beta3 or later), the adaptive sampling module automatically reduces the volume that is sent to the portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="09f86-199">Azonban a kérésben kapcsolódó események kiválasztva, vagy nincs kijelölve csoportosan, hogy a kapcsolódó események közti léphet.</span><span class="sxs-lookup"><span data-stu-id="09f86-199">However, events that are related to the same request are selected or deselected as a group, so that you can navigate between related events.</span></span> 

<span data-ttu-id="09f86-200">[Ismerkedés a mintavételezéssel](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="09f86-200">[Learn about sampling](app-insights-sampling.md).</span></span>



## <a name="create-work-item"></a><span data-ttu-id="09f86-201">Munkaelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="09f86-201">Create work item</span></span>
<span data-ttu-id="09f86-202">A Githubból vagy a Visual Studio Team Services programhiba bármely telemetriai eleme az adatokkal hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="09f86-202">You can create a bug in GitHub or Visual Studio Team Services with the details from any telemetry item.</span></span> 

![Kattintson az új munkaelemre vonatkozóan, módosítsa a mezőket, és kattintson az OK gombra.](./media/app-insights-diagnostic-search/42.png)

<span data-ttu-id="09f86-204">Ebben az esetben először a rendszer felkéri a Team Services-fiók és a projekt kapcsolat konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="09f86-204">The first time you do this, you are asked to configure a link to your Team Services account and project.</span></span>

![Töltse ki az URL-CÍMÉT a Team Services-kiszolgáló és a projekt nevét, majd kattintson az Engedélyezés parancsra](./media/app-insights-diagnostic-search/41.png)

<span data-ttu-id="09f86-206">(Beállíthatja úgy is a hivatkozás a munkaelemek panelen.)</span><span class="sxs-lookup"><span data-stu-id="09f86-206">(You can also configure the link on the Work Items blade.)</span></span>

## <a name="save-your-search"></a><span data-ttu-id="09f86-207">A Keresés mentése</span><span class="sxs-lookup"><span data-stu-id="09f86-207">Save your search</span></span>
<span data-ttu-id="09f86-208">Ha azt szeretné, az összes szűrő beállítása, a Kedvencek közé mentheti a keresést.</span><span class="sxs-lookup"><span data-stu-id="09f86-208">When you've set all the filters you want, you can save the search as a favorite.</span></span> <span data-ttu-id="09f86-209">Ha a szervezeti fiók dolgozunk, is eldöntheti, hogy az azt megosztása más csoport tagjai.</span><span class="sxs-lookup"><span data-stu-id="09f86-209">If you work in an organizational account, you can choose whether to share it with other team members.</span></span>

![Kattintson a kedvenc, állítson be a nevet és kattintson a Mentés gombra](./media/app-insights-diagnostic-search/08-favorite-save.png)

<span data-ttu-id="09f86-211">A Keresés újra, hogy **keresse fel a áttekintése panel** , és nyissa meg a Kedvencek:</span><span class="sxs-lookup"><span data-stu-id="09f86-211">To see the search again, **go to the overview blade** and open Favorites:</span></span>

![Kedvencek csempe](./media/app-insights-diagnostic-search/09-favorite-get.png)

<span data-ttu-id="09f86-213">Ha mentett relatív időtartomány, újra megnyitni a panelt a legfrissebb adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="09f86-213">If you saved with Relative time range, the re-opened blade has the latest data.</span></span> <span data-ttu-id="09f86-214">Ha mentett abszolút időtartomány, látni ugyanazokhoz az adatokhoz minden alkalommal.</span><span class="sxs-lookup"><span data-stu-id="09f86-214">If you saved with Absolute time range, you see the same data every time.</span></span> <span data-ttu-id="09f86-215">("Relatív" nem használható kedvenc menteni szeretné, ha az időtartományt kattintson a fejléc, és a beállítása egy időtartományt, amely nem egy egyéni tartományt.)</span><span class="sxs-lookup"><span data-stu-id="09f86-215">(If 'Relative' isn't available when you want to save a favorite, click Time Range in the header, and set a time range that isn't a custom range.)</span></span>

## <a name="send-more-telemetry-to-application-insights"></a><span data-ttu-id="09f86-216">További telemetriai adatokat küldhet az Application Insights részére</span><span class="sxs-lookup"><span data-stu-id="09f86-216">Send more telemetry to Application Insights</span></span>
<span data-ttu-id="09f86-217">Az Application Insights SDK által küldött out-of-az-box telemetriai, mellett a következő műveletek végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="09f86-217">In addition to the out-of-the-box telemetry sent by Application Insights SDK, you can:</span></span>

* <span data-ttu-id="09f86-218">A kedvenc naplózási keretrendszer a naplózási nyomkövetés rögzítése a [.NET](app-insights-asp-net-trace-logs.md) vagy [Java](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="09f86-218">Capture log traces from your favorite logging framework in [.NET](app-insights-asp-net-trace-logs.md) or [Java](app-insights-java-trace-logs.md).</span></span> <span data-ttu-id="09f86-219">Ez azt jelenti, hogy a naplókivonatokat közötti keresésre, és a kivizsgált Lapmegtekintések, kivételeket és eseményeket.</span><span class="sxs-lookup"><span data-stu-id="09f86-219">This means you can search through your log traces and correlate them with page views, exceptions, and other events.</span></span> 
* <span data-ttu-id="09f86-220">[Kód írása](app-insights-api-custom-events-metrics.md) egyéni események, a lapmegtekintések és a kivételeket.</span><span class="sxs-lookup"><span data-stu-id="09f86-220">[Write code](app-insights-api-custom-events-metrics.md) to send custom events, page views, and exceptions.</span></span> 

<span data-ttu-id="09f86-221">[Útmutató a naplók és egyéni telemetriai adatokat küldhet az Application Insights](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="09f86-221">[Learn how to send logs and custom telemetry to Application Insights](app-insights-asp-net-trace-logs.md).</span></span>

## <span data-ttu-id="09f86-222"><a name="questions"></a>A KÉRDÉSEK ÉS VÁLASZOK</span><span class="sxs-lookup"><span data-stu-id="09f86-222"><a name="questions"></a>Q & A</span></span>
### <span data-ttu-id="09f86-223"><a name="limits"></a>Mennyi adatot megmarad?</span><span class="sxs-lookup"><span data-stu-id="09f86-223"><a name="limits"></a>How much data is retained?</span></span>

<span data-ttu-id="09f86-224">Tekintse meg a [korlátok összegzés](app-insights-pricing.md#limits-summary).</span><span class="sxs-lookup"><span data-stu-id="09f86-224">See the [Limits summary](app-insights-pricing.md#limits-summary).</span></span>

### <a name="how-can-i-see-post-data-in-my-server-requests"></a><span data-ttu-id="09f86-225">Honnan látom POST-adatokat a saját kiszolgáló kérések?</span><span class="sxs-lookup"><span data-stu-id="09f86-225">How can I see POST data in my server requests?</span></span>
<span data-ttu-id="09f86-226">Automatikusan azt ne naplózza a POST-adatokat, de használhat [TrackTrace vagy a napló hívások](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="09f86-226">We don't log the POST data automatically, but you can use [TrackTrace or log calls](app-insights-asp-net-trace-logs.md).</span></span> <span data-ttu-id="09f86-227">Az üzenet paraméter helyezze el a POST-adatokat.</span><span class="sxs-lookup"><span data-stu-id="09f86-227">Put the POST data in the message parameter.</span></span> <span data-ttu-id="09f86-228">Nem lehet szűrést végezni az üzenet tulajdonságai alapján szűrhet ugyanúgy, de a méretkorlátot hosszabb.</span><span class="sxs-lookup"><span data-stu-id="09f86-228">You can't filter on the message in the same way you can filter on properties, but the size limit is longer.</span></span>

## <a name="video"></a><span data-ttu-id="09f86-229">Videó</span><span class="sxs-lookup"><span data-stu-id="09f86-229">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <span data-ttu-id="09f86-230"><a name="add"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09f86-230"><a name="add"></a>Next steps</span></span>
* [<span data-ttu-id="09f86-231">Összetett lekérdezések írás Analytics</span><span class="sxs-lookup"><span data-stu-id="09f86-231">Write complex queries in Analytics</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="09f86-232">Naplók és egyéni telemetriai adatokat küldhet az Application Insights</span><span class="sxs-lookup"><span data-stu-id="09f86-232">Send logs and custom telemetry to Application Insights</span></span>](app-insights-asp-net-trace-logs.md)
* [<span data-ttu-id="09f86-233">Rendelkezésre állási és reakcióidőt tesztek beállítása</span><span class="sxs-lookup"><span data-stu-id="09f86-233">Set up availability and responsiveness tests</span></span>](app-insights-monitor-web-app-availability.md)
* [<span data-ttu-id="09f86-234">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="09f86-234">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)

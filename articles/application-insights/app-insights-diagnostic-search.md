---
title: "Keresés a Azure Application Insights aaaUsing |} Microsoft Docs"
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
ms.openlocfilehash: df2b0eb017ad48bcdc6ef57d8dff207d120143b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-search-in-application-insights"></a><span data-ttu-id="e7a32-103">Az Application Insightsban keresés használata</span><span class="sxs-lookup"><span data-stu-id="e7a32-103">Using Search in Application Insights</span></span>
<span data-ttu-id="e7a32-104">Keresés csak a [Application Insights](app-insights-overview.md) alkalmazni toofind és egyéni telemetriai elemek, például Lapmegtekintések, kivételek, így megismerkedhet vagy webalkalmazás-kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="e7a32-104">Search is a feature of [Application Insights](app-insights-overview.md) that you use toofind and explore individual telemetry items, such as page views, exceptions, or web requests.</span></span> <span data-ttu-id="e7a32-105">És naplókivonatokat és eseményeket, amelyek rendelkeznek a kódolt megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e7a32-105">And you can view log traces and events that you have coded.</span></span>

<span data-ttu-id="e7a32-106">(Az adatok a összetettebb lekérdezések, használjon [Analytics](app-insights-analytics-tour.md).)</span><span class="sxs-lookup"><span data-stu-id="e7a32-106">(For more complex queries over your data, use [Analytics](app-insights-analytics-tour.md).)</span></span>

## <a name="where-do-you-see-search"></a><span data-ttu-id="e7a32-107">Ha látja keresési?</span><span class="sxs-lookup"><span data-stu-id="e7a32-107">Where do you see Search?</span></span>
### <a name="in-hello-azure-portal"></a><span data-ttu-id="e7a32-108">A hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e7a32-108">In hello Azure portal</span></span>
<span data-ttu-id="e7a32-109">Diagnosztikai keresési explicit módon megnyithatja az alkalmazás hello Application Insights – áttekintés paneljéről:</span><span class="sxs-lookup"><span data-stu-id="e7a32-109">You can open diagnostic search explicitly from hello Application Insights Overview blade of your application:</span></span>

![Diagnosztikai keresés megnyitása](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

<span data-ttu-id="e7a32-111">Néhány diagramok és a rács elemek kattintva is megnyílik.</span><span class="sxs-lookup"><span data-stu-id="e7a32-111">It also opens when you click through some charts and grid items.</span></span> <span data-ttu-id="e7a32-112">Ebben az esetben a szűrők vannak előre beállított toofocus hello típusú kijelölt elemhez.</span><span class="sxs-lookup"><span data-stu-id="e7a32-112">In this case, its filters are pre-set toofocus on hello type of item you selected.</span></span> 

<span data-ttu-id="e7a32-113">Például a hello áttekintése panelen nincs kérelmek válaszideje által besorolt sávdiagram.</span><span class="sxs-lookup"><span data-stu-id="e7a32-113">For example, on hello Overview blade, there's a bar chart of requests classified by response time.</span></span> <span data-ttu-id="e7a32-114">Kattintson a teljesítmény tartomány toosee tartalmazó lista egyes abban a válaszidő tartomány:</span><span class="sxs-lookup"><span data-stu-id="e7a32-114">Click through a performance range toosee a list of individual requests in that response time range:</span></span>

![Kattintson a kérelem teljesítmény](./media/app-insights-diagnostic-search/07-open-from-filters.png)

<span data-ttu-id="e7a32-116">hello fő diagnosztikai keresési szövegtörzse listáját telemetriai elemek - kiszolgálói kérelmek lapon a nézetek, egyéni események, amelyek rendelkeznek a kódolt és így tovább.</span><span class="sxs-lookup"><span data-stu-id="e7a32-116">hello main body of Diagnostic Search is a list of telemetry items - server requests, page views, custom events that you have coded, and so on.</span></span> <span data-ttu-id="e7a32-117">Hello hello lista elejére egy összefoglaló táblázat számát is események adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="e7a32-117">At hello top of hello list is a summary chart showing counts of events over time.</span></span>

<span data-ttu-id="e7a32-118">Kattintson a frissítés tooget új eseményeket.</span><span class="sxs-lookup"><span data-stu-id="e7a32-118">Click Refresh tooget new events.</span></span>

### <a name="in-visual-studio"></a><span data-ttu-id="e7a32-119">Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="e7a32-119">In Visual Studio</span></span>

<span data-ttu-id="e7a32-120">A Visual Studio esetében is az Application Insights keresési ablak.</span><span class="sxs-lookup"><span data-stu-id="e7a32-120">In Visual Studio, there's also an Application Insights Search window.</span></span> <span data-ttu-id="e7a32-121">A leghasznosabb, amely akkor hibakeresése hello alkalmazás által létrehozott telemetriai események megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="e7a32-121">It's most useful for displaying telemetry events generated by hello application that you're debugging.</span></span> <span data-ttu-id="e7a32-122">Azonban akkor is megjelenhet hello események gyűjtött hello Azure-portálon a közzétett alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e7a32-122">But it can also show hello events collected from your published app at hello Azure portal.</span></span>

<span data-ttu-id="e7a32-123">Visual Studio hello keresési ablak megnyitása:</span><span class="sxs-lookup"><span data-stu-id="e7a32-123">Open hello Search window in Visual Studio:</span></span>

![A Visual Studio Application Insights keresés megnyitása](./media/app-insights-diagnostic-search/32.png)

<span data-ttu-id="e7a32-125">hello keresési ablak szolgáltatások hasonló toohello webes portál esetében:</span><span class="sxs-lookup"><span data-stu-id="e7a32-125">hello Search window has features similar toohello web portal:</span></span>

![Visual Studio Application Insights – keresési ablak](./media/app-insights-diagnostic-search/34.png)

<span data-ttu-id="e7a32-127">hello követése művelet lapon érhető el egy kérelem vagy egy nézet megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="e7a32-127">hello Track Operation tab is available when you open a request or a page view.</span></span> <span data-ttu-id="e7a32-128">Egy "művelet" tooa egyetlen kérelemmel vagy a lap nézetben kapcsolódó események sorozata.</span><span class="sxs-lookup"><span data-stu-id="e7a32-128">An 'operation' is a sequence of events that is associated with tooa single request or page view.</span></span> <span data-ttu-id="e7a32-129">Például függőségi hívások esetében, kivételek, nyomkövetési naplókat és egyéni események része lehet egyetlen művelettel.</span><span class="sxs-lookup"><span data-stu-id="e7a32-129">For example, dependency calls, exceptions, trace logs, and custom events might be part of a single operation.</span></span> <span data-ttu-id="e7a32-130">hello követése művelet lapon megjelenik a grafikusan hello ütemezése és ezek az események kapcsolat toohello kérelemmel vagy a lap nézetben időtartama.</span><span class="sxs-lookup"><span data-stu-id="e7a32-130">hello Track Operation tab shows graphically hello timing and duration of these events in relation toohello request or page view.</span></span> 

## <a name="inspect-individual-items"></a><span data-ttu-id="e7a32-131">Vizsgálja meg az egyes elemek</span><span class="sxs-lookup"><span data-stu-id="e7a32-131">Inspect individual items</span></span>
<span data-ttu-id="e7a32-132">Válassza ki a telemetriai elem toosee kulcsmezők és a kapcsolódó elemek.</span><span class="sxs-lookup"><span data-stu-id="e7a32-132">Select any telemetry item toosee key fields and related items.</span></span> <span data-ttu-id="e7a32-133">Ha azt szeretné, hogy toosee hello teljes mezők halmaza, kattintson a "...".</span><span class="sxs-lookup"><span data-stu-id="e7a32-133">If you want toosee hello full set of fields, click "...".</span></span> 

![Kattintson az új munkaelemre vonatkozóan, hello mezők szerkesztéséhez, és kattintson az OK gombra.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a><span data-ttu-id="e7a32-135">Szűrő eseménytípusok</span><span class="sxs-lookup"><span data-stu-id="e7a32-135">Filter event types</span></span>
<span data-ttu-id="e7a32-136">Nyissa meg a hello szűrő panelre, és válassza a hello esemény meg kell adnia toosee szeretné.</span><span class="sxs-lookup"><span data-stu-id="e7a32-136">Open hello Filter blade and choose hello event types you want toosee.</span></span> <span data-ttu-id="e7a32-137">(, Később, amellyel hello panelen megnyitott toorestore hello szűrők, kattintson a alaphelyzetbe állítása.)</span><span class="sxs-lookup"><span data-stu-id="e7a32-137">(If, later, you want toorestore hello filters with which you opened hello blade, click Reset.)</span></span>

![Kattintson a szűrő és telemetriai kijelölve](./media/app-insights-diagnostic-search/02-filter-req.png)

<span data-ttu-id="e7a32-139">hello esemény típusok a következők:</span><span class="sxs-lookup"><span data-stu-id="e7a32-139">hello event types are:</span></span>

* <span data-ttu-id="e7a32-140">**Nyomkövetési** - [diagnosztikai naplók](app-insights-asp-net-trace-logs.md) TrackTrace, log4Net, NLog és System.Diagnostic.Trace hívások beleértve.</span><span class="sxs-lookup"><span data-stu-id="e7a32-140">**Trace** - [Diagnostic logs](app-insights-asp-net-trace-logs.md) including TrackTrace, log4Net, NLog, and System.Diagnostic.Trace calls.</span></span>
* <span data-ttu-id="e7a32-141">**Kérelem** -HTTP-kérések a kiszolgálóalkalmazás, beleértve a lapok, parancsfájlok, képek, stílus fájlokat és adatokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="e7a32-141">**Request** - HTTP requests received by your server application, including pages, scripts, images, style files, and data.</span></span> <span data-ttu-id="e7a32-142">Ezek az események használt toocreate hello kérelem-válasz áttekintő diagramok.</span><span class="sxs-lookup"><span data-stu-id="e7a32-142">These events are used toocreate hello request and response overview charts.</span></span>
* <span data-ttu-id="e7a32-143">**Lapmegtekintés** - [hello webes ügyfél által küldött Telemetriai](app-insights-javascript.md), használt toocreate lap jelentések megtekintése.</span><span class="sxs-lookup"><span data-stu-id="e7a32-143">**Page View** - [Telemetry sent by hello web client](app-insights-javascript.md), used toocreate page view reports.</span></span> 
* <span data-ttu-id="e7a32-144">**Egyéni esemény** – ha beszúrt hívások tooTrackEvent() sorrendben túl[megfigyeléséhez](app-insights-api-custom-events-metrics.md), Itt kereshet bennük.</span><span class="sxs-lookup"><span data-stu-id="e7a32-144">**Custom Event** - If you inserted calls tooTrackEvent() in order too[monitor usage](app-insights-api-custom-events-metrics.md), you can search them here.</span></span>
* <span data-ttu-id="e7a32-145">**Kivétel** – fellépő nem kezelt [kivételek hello Server](app-insights-asp-net-exceptions.md), valamint azokat, amelyeket a TrackException() használatával jelentkezik.</span><span class="sxs-lookup"><span data-stu-id="e7a32-145">**Exception** - Uncaught [exceptions in hello server](app-insights-asp-net-exceptions.md), and those that you log by using TrackException().</span></span>
* <span data-ttu-id="e7a32-146">**A függőségi** - [a kiszolgálóalkalmazás hívásait](app-insights-asp-net-dependencies.md) tooother szolgáltatások, például REST API-k vagy az adatbázisok, és az AJAX-hívásai a [Ügyfélkód](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="e7a32-146">**Dependency** - [Calls from your server application](app-insights-asp-net-dependencies.md) tooother services such as REST APIs or databases, and AJAX calls from your [client code](app-insights-javascript.md).</span></span>
* <span data-ttu-id="e7a32-147">**Rendelkezésre állási** -eredményeinek [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="e7a32-147">**Availability** - Results of [availability tests](app-insights-monitor-web-app-availability.md).</span></span>

## <a name="filter-on-property-values"></a><span data-ttu-id="e7a32-148">A tulajdonságértékek szűrése</span><span class="sxs-lookup"><span data-stu-id="e7a32-148">Filter on property values</span></span>
<span data-ttu-id="e7a32-149">A tulajdonságok értékeit hello események jeleníthetők meg.</span><span class="sxs-lookup"><span data-stu-id="e7a32-149">You can filter events on hello values of their properties.</span></span> <span data-ttu-id="e7a32-150">rendelkezésre álló tulajdonságok hello kiválasztott hello eseménytípusok függ.</span><span class="sxs-lookup"><span data-stu-id="e7a32-150">hello available properties depend on hello event types you selected.</span></span> 

<span data-ttu-id="e7a32-151">Válasszon például adott válaszkód kéréseket.</span><span class="sxs-lookup"><span data-stu-id="e7a32-151">For example, pick out requests with a specific response code.</span></span> 

![Bontsa ki a tulajdonságot, és adjon meg értéket](./media/app-insights-diagnostic-search/03-response500.png)

<span data-ttu-id="e7a32-153">Ugyanaz, mint minden értékek érvényesíti hello kiválasztása nem egy adott tulajdonság értékének rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e7a32-153">Choosing no values of a particular property has hello same effect as choosing all values.</span></span> <span data-ttu-id="e7a32-154">Vált ki a a tulajdonságon alapuló szűrőt.</span><span class="sxs-lookup"><span data-stu-id="e7a32-154">It switches off filtering on that property.</span></span>

### <a name="narrow-your-search"></a><span data-ttu-id="e7a32-155">A keresés szűkítéséhez</span><span class="sxs-lookup"><span data-stu-id="e7a32-155">Narrow your search</span></span>
<span data-ttu-id="e7a32-156">Figyelje meg, hogy hello száma sarkában hello szűrőértékek toohello megjelenítése hány előfordulások nem szerepelnek a hello aktuális szűrt készletében.</span><span class="sxs-lookup"><span data-stu-id="e7a32-156">Notice that hello counts toohello right of hello filter values show how many occurrences there are in hello current filtered set.</span></span> 

<span data-ttu-id="e7a32-157">Az ebben a példában is egyértelmű hello Rpt/alkalmazottak kérés hello "500" hibák többségét eredményezi:</span><span class="sxs-lookup"><span data-stu-id="e7a32-157">In this example, it's clear that hello 'Rpt/Employees' request results in most of hello '500' errors:</span></span>

![Bontsa ki a tulajdonságot, és adjon meg értéket](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-hello-same-property"></a><span data-ttu-id="e7a32-159">Esemény megkeresése, a hello ugyanahhoz a tulajdonsághoz</span><span class="sxs-lookup"><span data-stu-id="e7a32-159">Find events with hello same property</span></span>
<span data-ttu-id="e7a32-160">Összes hello található elemek hello azonos tulajdonság értéke:</span><span class="sxs-lookup"><span data-stu-id="e7a32-160">Find all hello items with hello same property value:</span></span>

![Kattintson a jobb gombbal egy tulajdonság](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-hello-data"></a><span data-ttu-id="e7a32-162">Keresési hello adatok</span><span class="sxs-lookup"><span data-stu-id="e7a32-162">Search hello data</span></span>

> [!NOTE]
> <span data-ttu-id="e7a32-163">toowrite összetettebb lekérdezések, nyissa meg [ **Analytics** ](app-insights-analytics-tour.md) a hello felső hello keresési panelről.</span><span class="sxs-lookup"><span data-stu-id="e7a32-163">toowrite more complex queries, open [**Analytics**](app-insights-analytics-tour.md) from hello top of hello Search blade.</span></span>
> 

<span data-ttu-id="e7a32-164">Bármely hello tulajdonságértékek feltételeket is kereshet.</span><span class="sxs-lookup"><span data-stu-id="e7a32-164">You can search for terms in any of hello property values.</span></span> <span data-ttu-id="e7a32-165">Ez különösen akkor hasznos, ha írt az [egyéni események](app-insights-api-custom-events-metrics.md) tulajdonság értékekkel.</span><span class="sxs-lookup"><span data-stu-id="e7a32-165">This is particularly useful if you have written [custom events](app-insights-api-custom-events-metrics.md) with property values.</span></span> 

<span data-ttu-id="e7a32-166">Érdemes lehet egy időtartományt, mint rövidebb tartományban keresések gyorsabbak tooset.</span><span class="sxs-lookup"><span data-stu-id="e7a32-166">You might want tooset a time range, as searches over a shorter range are faster.</span></span> 

![Diagnosztikai keresés megnyitása](./media/app-insights-diagnostic-search/appinsights-311search.png)

<span data-ttu-id="e7a32-168">Teljes szavak, nem karakterláncrész keresése.</span><span class="sxs-lookup"><span data-stu-id="e7a32-168">Search for complete words, not substrings.</span></span> <span data-ttu-id="e7a32-169">Idézőjelek tooenclose különleges karakterek használhatók.</span><span class="sxs-lookup"><span data-stu-id="e7a32-169">Use quotation marks tooenclose special characters.</span></span>

| <span data-ttu-id="e7a32-170">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e7a32-170">string</span></span> | <span data-ttu-id="e7a32-171">van *nem* által talált</span><span class="sxs-lookup"><span data-stu-id="e7a32-171">is *not* found by</span></span> | <span data-ttu-id="e7a32-172">Ezek találja</span><span class="sxs-lookup"><span data-stu-id="e7a32-172">but these do find it</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e7a32-173">HomeController.About</span><span class="sxs-lookup"><span data-stu-id="e7a32-173">HomeController.About</span></span> |<span data-ttu-id="e7a32-174">otthoni</span><span class="sxs-lookup"><span data-stu-id="e7a32-174">home</span></span><br/><span data-ttu-id="e7a32-175">Tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="e7a32-175">controller</span></span><br/><span data-ttu-id="e7a32-176">Kimenő</span><span class="sxs-lookup"><span data-stu-id="e7a32-176">out</span></span> | <span data-ttu-id="e7a32-177">homecontroller</span><span class="sxs-lookup"><span data-stu-id="e7a32-177">homecontroller</span></span><br/><span data-ttu-id="e7a32-178">tudnivalók</span><span class="sxs-lookup"><span data-stu-id="e7a32-178">about</span></span><br/><span data-ttu-id="e7a32-179">"homecontroller.about"</span><span class="sxs-lookup"><span data-stu-id="e7a32-179">"homecontroller.about"</span></span>|
|<span data-ttu-id="e7a32-180">Egyesült Államok</span><span class="sxs-lookup"><span data-stu-id="e7a32-180">United States</span></span>|<span data-ttu-id="e7a32-181">UNI</span><span class="sxs-lookup"><span data-stu-id="e7a32-181">Uni</span></span><br/><span data-ttu-id="e7a32-182">TED</span><span class="sxs-lookup"><span data-stu-id="e7a32-182">ted</span></span>|<span data-ttu-id="e7a32-183">Egyesült</span><span class="sxs-lookup"><span data-stu-id="e7a32-183">united</span></span><br/><span data-ttu-id="e7a32-184">állapotok</span><span class="sxs-lookup"><span data-stu-id="e7a32-184">states</span></span><br/><span data-ttu-id="e7a32-185">Egyesült Államok és</span><span class="sxs-lookup"><span data-stu-id="e7a32-185">united AND states</span></span><br/><span data-ttu-id="e7a32-186">"az Amerikai Egyesült Államok"</span><span class="sxs-lookup"><span data-stu-id="e7a32-186">"united states"</span></span>

<span data-ttu-id="e7a32-187">Az alábbiakban hello keresési kifejezéseket is használhat:</span><span class="sxs-lookup"><span data-stu-id="e7a32-187">Here are hello search expressions you can use:</span></span>

| <span data-ttu-id="e7a32-188">Mintalekérdezés</span><span class="sxs-lookup"><span data-stu-id="e7a32-188">Sample query</span></span> | <span data-ttu-id="e7a32-189">Következmény</span><span class="sxs-lookup"><span data-stu-id="e7a32-189">Effect</span></span> |
| --- | --- |
| `apple` |<span data-ttu-id="e7a32-190">Összes esemény található egyéb mezőjének tartalmaznia hello word "apple" hello időtartomány</span><span class="sxs-lookup"><span data-stu-id="e7a32-190">Find all events in hello time range whose fields include hello word "apple"</span></span> |
| `apple AND banana` |<span data-ttu-id="e7a32-191">Megkeresése, amelynek mindkét szavakat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="e7a32-191">Find events that contain both words.</span></span> <span data-ttu-id="e7a32-192">A tőkéhez "és", nem használható "és".</span><span class="sxs-lookup"><span data-stu-id="e7a32-192">Use capital "AND", not "and".</span></span> |
| `apple OR banana`<br/>`apple banana` |<span data-ttu-id="e7a32-193">Megkeresése, amelynek vagy szót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e7a32-193">Find events that contain either word.</span></span> <span data-ttu-id="e7a32-194">"Vagy", nem használható "vagy".</span><span class="sxs-lookup"><span data-stu-id="e7a32-194">Use "OR", not "or".</span></span><br/><span data-ttu-id="e7a32-195">Rövid alak.</span><span class="sxs-lookup"><span data-stu-id="e7a32-195">Short form.</span></span> |
| `apple NOT banana` |<span data-ttu-id="e7a32-196">Esemény megkeresése, amelyek tartalmaznak egy szót, de nem hello más.</span><span class="sxs-lookup"><span data-stu-id="e7a32-196">Find events that contain one word but not hello other.</span></span> |



## <a name="sampling"></a><span data-ttu-id="e7a32-197">Mintavételezés</span><span class="sxs-lookup"><span data-stu-id="e7a32-197">Sampling</span></span>
<span data-ttu-id="e7a32-198">Ha az alkalmazás nagy mennyiségű telemetriai adatokat hoz létre (hello ASP.NET SDK verzió 2.0.0-beta3 használ, és vagy újabb), hello adaptív mintavételi modul automatikusan csökkenti toohello portal reprezentatív része események küldése által küldött hello kötet.</span><span class="sxs-lookup"><span data-stu-id="e7a32-198">If your app generates a lot of telemetry (and you are using hello ASP.NET SDK version 2.0.0-beta3 or later), hello adaptive sampling module automatically reduces hello volume that is sent toohello portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="e7a32-199">Azonban események, amelyek kapcsolódó toohello kérésben kiválasztva, vagy nincs kijelölve csoportosan, hogy a kapcsolódó események közti léphet.</span><span class="sxs-lookup"><span data-stu-id="e7a32-199">However, events that are related toohello same request are selected or deselected as a group, so that you can navigate between related events.</span></span> 

<span data-ttu-id="e7a32-200">[Ismerkedés a mintavételezéssel](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="e7a32-200">[Learn about sampling](app-insights-sampling.md).</span></span>



## <a name="create-work-item"></a><span data-ttu-id="e7a32-201">Munkaelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="e7a32-201">Create work item</span></span>
<span data-ttu-id="e7a32-202">A Githubból vagy a Visual Studio Team Services programhiba bármely telemetriai elemből hello adatokkal hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="e7a32-202">You can create a bug in GitHub or Visual Studio Team Services with hello details from any telemetry item.</span></span> 

![Kattintson az új munkaelemre vonatkozóan, hello mezők szerkesztéséhez, és kattintson az OK gombra.](./media/app-insights-diagnostic-search/42.png)

<span data-ttu-id="e7a32-204">hello ehhez, a rendszer felkéri tooconfigure először egy hivatkozás tooyour Team Services fiókját, és a projekt.</span><span class="sxs-lookup"><span data-stu-id="e7a32-204">hello first time you do this, you are asked tooconfigure a link tooyour Team Services account and project.</span></span>

![Töltse ki a hello URL-CÍMÉT a Team Services-kiszolgáló és a hello projekt nevét, majd kattintson az Engedélyezés parancsra](./media/app-insights-diagnostic-search/41.png)

<span data-ttu-id="e7a32-206">(Beállíthatja úgy is hello hivatkozás hello munkaelemek panelen.)</span><span class="sxs-lookup"><span data-stu-id="e7a32-206">(You can also configure hello link on hello Work Items blade.)</span></span>

## <a name="save-your-search"></a><span data-ttu-id="e7a32-207">A Keresés mentése</span><span class="sxs-lookup"><span data-stu-id="e7a32-207">Save your search</span></span>
<span data-ttu-id="e7a32-208">Miután beállított összes hello szűrők azt szeretné, hello keresési mentheti a Kedvencek közé.</span><span class="sxs-lookup"><span data-stu-id="e7a32-208">When you've set all hello filters you want, you can save hello search as a favorite.</span></span> <span data-ttu-id="e7a32-209">Egy szervezeti fiók dolgozik, dönthet úgy, hogy tooshare, a többi csoport tagjaival.</span><span class="sxs-lookup"><span data-stu-id="e7a32-209">If you work in an organizational account, you can choose whether tooshare it with other team members.</span></span>

![Kattintson a kedvenc, állítson be hello nevet és kattintson a Mentés gombra](./media/app-insights-diagnostic-search/08-favorite-save.png)

<span data-ttu-id="e7a32-211">Ebben az esetben a toosee hello keresési **lépjen toohello áttekintése panel** , és nyissa meg a Kedvencek:</span><span class="sxs-lookup"><span data-stu-id="e7a32-211">toosee hello search again, **go toohello overview blade** and open Favorites:</span></span>

![Kedvencek csempe](./media/app-insights-diagnostic-search/09-favorite-get.png)

<span data-ttu-id="e7a32-213">Ha mentett relatív időtartomány, a hello újra megnyitni panel hello legfrissebb adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e7a32-213">If you saved with Relative time range, hello re-opened blade has hello latest data.</span></span> <span data-ttu-id="e7a32-214">Ha mentette az abszolút időtartomány, hello látható minden egyes ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="e7a32-214">If you saved with Absolute time range, you see hello same data every time.</span></span> <span data-ttu-id="e7a32-215">("Relatív" érhető el, ha azt szeretné, hogy toosave kedvenc, ha az időtartományt hello fejlécben kattintson, és a beállítása egy időtartományt, amely nem egy egyéni tartományt.)</span><span class="sxs-lookup"><span data-stu-id="e7a32-215">(If 'Relative' isn't available when you want toosave a favorite, click Time Range in hello header, and set a time range that isn't a custom range.)</span></span>

## <a name="send-more-telemetry-tooapplication-insights"></a><span data-ttu-id="e7a32-216">További telemetriai adatokat küldhet tooApplication Insights</span><span class="sxs-lookup"><span data-stu-id="e7a32-216">Send more telemetry tooApplication Insights</span></span>
<span data-ttu-id="e7a32-217">Továbbá toohello out-of-az-box telemetriai Application Insights SDK által küldött, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="e7a32-217">In addition toohello out-of-the-box telemetry sent by Application Insights SDK, you can:</span></span>

* <span data-ttu-id="e7a32-218">A kedvenc naplózási keretrendszer a naplózási nyomkövetés rögzítése a [.NET](app-insights-asp-net-trace-logs.md) vagy [Java](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="e7a32-218">Capture log traces from your favorite logging framework in [.NET](app-insights-asp-net-trace-logs.md) or [Java](app-insights-java-trace-logs.md).</span></span> <span data-ttu-id="e7a32-219">Ez azt jelenti, hogy a naplókivonatokat közötti keresésre, és a kivizsgált Lapmegtekintések, kivételeket és eseményeket.</span><span class="sxs-lookup"><span data-stu-id="e7a32-219">This means you can search through your log traces and correlate them with page views, exceptions, and other events.</span></span> 
* <span data-ttu-id="e7a32-220">[Kód írása](app-insights-api-custom-events-metrics.md) toosend egyéni események Lapmegtekintések és kivételeket.</span><span class="sxs-lookup"><span data-stu-id="e7a32-220">[Write code](app-insights-api-custom-events-metrics.md) toosend custom events, page views, and exceptions.</span></span> 

<span data-ttu-id="e7a32-221">[Ismerje meg, hogy miként naplózza az toosend és egyéni telemetriai tooApplication Insights](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="e7a32-221">[Learn how toosend logs and custom telemetry tooApplication Insights](app-insights-asp-net-trace-logs.md).</span></span>

## <span data-ttu-id="e7a32-222"><a name="questions"></a>A KÉRDÉSEK ÉS VÁLASZOK</span><span class="sxs-lookup"><span data-stu-id="e7a32-222"><a name="questions"></a>Q & A</span></span>
### <span data-ttu-id="e7a32-223"><a name="limits"></a>Mennyi adatot megmarad?</span><span class="sxs-lookup"><span data-stu-id="e7a32-223"><a name="limits"></a>How much data is retained?</span></span>

<span data-ttu-id="e7a32-224">Lásd: hello [korlátok összegzés](app-insights-pricing.md#limits-summary).</span><span class="sxs-lookup"><span data-stu-id="e7a32-224">See hello [Limits summary](app-insights-pricing.md#limits-summary).</span></span>

### <a name="how-can-i-see-post-data-in-my-server-requests"></a><span data-ttu-id="e7a32-225">Honnan látom POST-adatokat a saját kiszolgáló kérések?</span><span class="sxs-lookup"><span data-stu-id="e7a32-225">How can I see POST data in my server requests?</span></span>
<span data-ttu-id="e7a32-226">Automatikusan azt ne naplózza a hello POST-adatokat, de használhat [TrackTrace vagy a napló hívások](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="e7a32-226">We don't log hello POST data automatically, but you can use [TrackTrace or log calls](app-insights-asp-net-trace-logs.md).</span></span> <span data-ttu-id="e7a32-227">Helyezze el hello üzenet paraméter hello POST-adatokat.</span><span class="sxs-lookup"><span data-stu-id="e7a32-227">Put hello POST data in hello message parameter.</span></span> <span data-ttu-id="e7a32-228">Meg nem lehet szűrést végezni a hello üdvözlőüzenetére azonos módon szűrheti a tulajdonságok, de már hello méretkorlátját.</span><span class="sxs-lookup"><span data-stu-id="e7a32-228">You can't filter on hello message in hello same way you can filter on properties, but hello size limit is longer.</span></span>

## <a name="video"></a><span data-ttu-id="e7a32-229">Videó</span><span class="sxs-lookup"><span data-stu-id="e7a32-229">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <span data-ttu-id="e7a32-230"><a name="add"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e7a32-230"><a name="add"></a>Next steps</span></span>
* [<span data-ttu-id="e7a32-231">Összetett lekérdezések írás Analytics</span><span class="sxs-lookup"><span data-stu-id="e7a32-231">Write complex queries in Analytics</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="e7a32-232">Naplók és egyéni telemetriai adatokat küldeni tooApplication Insights</span><span class="sxs-lookup"><span data-stu-id="e7a32-232">Send logs and custom telemetry tooApplication Insights</span></span>](app-insights-asp-net-trace-logs.md)
* [<span data-ttu-id="e7a32-233">Rendelkezésre állási és reakcióidőt tesztek beállítása</span><span class="sxs-lookup"><span data-stu-id="e7a32-233">Set up availability and responsiveness tests</span></span>](app-insights-monitor-web-app-availability.md)
* [<span data-ttu-id="e7a32-234">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="e7a32-234">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)

---
title: "aaaAnalytics - hello hatékony keresési eszköz az Azure Application Insights |} Microsoft Docs"
description: "Elemzés, hello hatékony diagnosztikai eszköz az Application Insights áttekintése. "
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: d2b41e2fff7cc786e11fa3dfe94fc46f1b86e9eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analytics-in-application-insights"></a><span data-ttu-id="c7539-103">Az Application Insightsban elemzés</span><span class="sxs-lookup"><span data-stu-id="c7539-103">Analytics in Application Insights</span></span>
<span data-ttu-id="c7539-104">[Elemzés](app-insights-analytics.md) hello hatékony keresési funkciója [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c7539-104">[Analytics](app-insights-analytics.md) is hello powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="c7539-105">Ezeken a lapokon a Log Analytics lekérdezési nyelv ismertetik.</span><span class="sxs-lookup"><span data-stu-id="c7539-105">These pages describe the Log Analytics query language.</span></span> 

* <span data-ttu-id="c7539-106">**[Hello bevezető videó](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="c7539-106">**[Watch hello introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="c7539-107">**[Tesztelése szimulált adataink Analytics meghajtóján](https://analytics.applicationinsights.io/demo)**  Ha az alkalmazás még nem küld adatokat tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="c7539-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data tooApplication Insights yet.</span></span>
* <span data-ttu-id="c7539-108">**[SQL-felhasználók lap cheat](https://aka.ms/sql-analytics)**  hello leggyakoribb idioms lefordítja.</span><span class="sxs-lookup"><span data-stu-id="c7539-108">**[SQL-users' cheat sheet](https://aka.ms/sql-analytics)** translates hello most common idioms.</span></span>
* <span data-ttu-id="c7539-109">**[Nyelvi referencia](app-insights-analytics-reference.md)**  megtudhatja, hogyan összes toouse hello hatékony szolgáltatásainak hello Log Analytics lekérdezési nyelv.</span><span class="sxs-lookup"><span data-stu-id="c7539-109">**[Language Reference](app-insights-analytics-reference.md)** Learn how toouse all hello powerful features of hello Log Analytics query language.</span></span>


## <a name="queries-in-analytics"></a><span data-ttu-id="c7539-110">Az elemzési lekérdezések</span><span class="sxs-lookup"><span data-stu-id="c7539-110">Queries in Analytics</span></span>
<span data-ttu-id="c7539-111">Egy tipikus lekérdezés egy *forrás* tábla több követ *operátorok* elválasztott `|`.</span><span class="sxs-lookup"><span data-stu-id="c7539-111">A typical query is a *source* table followed by a series of *operators* separated by `|`.</span></span> 

<span data-ttu-id="c7539-112">Például keressük meg milyen alkalommal nap hello polgárainak Hyderabad, próbálja meg a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c7539-112">For example, let's find out what time of day hello citizens of Hyderabad try our web app.</span></span> <span data-ttu-id="c7539-113">És nincs el, amíg nézzük meg, milyen eredménykódok visszaadott tootheir HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="c7539-113">And while we're there, let's see what result codes are returned tootheir HTTP requests.</span></span> 

```AIQL
requests
| where timestamp > ago(30d)
| summarize ClientCount = dcount(client_IP) by bin(timestamp, 1h), resultCode
| extend LocalTime = timestamp - 4h
| order by LocalTime desc
| render barchart
```

<span data-ttu-id="c7539-114">A Microsoft száma különböző ügyfél IP-címeket, csoportosíthatja őket hello óránként hello napon keresztül hello az elmúlt 7 napban.</span><span class="sxs-lookup"><span data-stu-id="c7539-114">We count distinct client IP addresses, grouping them by hello hour of hello day over hello past 7 days.</span></span> 

> [!NOTE]
> <span data-ttu-id="c7539-115">tooget eredmények kívül hello előző 24 órát, "időbélyeg" explicit módon a lekérdezést, vagy a hello idő tartomány legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="c7539-115">tooget results outside hello previous 24h, either include 'timestamp' explicitly in your query, or use hello time range drop-down menu.</span></span>
>

<span data-ttu-id="c7539-116">Az eszközterület-diagram megjelenítési hello eredmények toostack választani a különböző válaszkódot hello hello eredmények most megjelenítéséhez:</span><span class="sxs-lookup"><span data-stu-id="c7539-116">Let's display hello results with hello bar chart presentation, choosing toostack hello results from different response codes:</span></span>

![Válassza ki a sávdiagram, x és y-tengelyek, majd Szegmentálás](./media/app-insights-analytics/020.png)

<span data-ttu-id="c7539-118">Tűnik, az alkalmazás a legnépszerűbb lunchtime és Hyderabad beágyazása-időpontja.</span><span class="sxs-lookup"><span data-stu-id="c7539-118">Looks like our app is most popular at lunchtime and bed-time in Hyderabad.</span></span> <span data-ttu-id="c7539-119">(És 500 kódok kell felülvizsgáljuk.)</span><span class="sxs-lookup"><span data-stu-id="c7539-119">(And we should investigate those 500 codes.)</span></span>

<span data-ttu-id="c7539-120">Van még hatékonyabb statisztikai műveletek:</span><span class="sxs-lookup"><span data-stu-id="c7539-120">There are also powerful statistical operations:</span></span>

![Statisztikai lekérdezés eredményei](./media/app-insights-analytics/025.png)

<span data-ttu-id="c7539-122">hello nyelv számos vonzó lehetőséggel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="c7539-122">hello language has many attractive features:</span></span>


* <span data-ttu-id="c7539-123">[Szűrő](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) által a mezőket, beleértve az egyéni tulajdonságok és a metrikák a nyers app telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="c7539-123">[Filter](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) your raw app telemetry by any fields, including your custom properties and metrics.</span></span>
* <span data-ttu-id="c7539-124">[Csatlakozás](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) több táblázatot – korrelálja Lapmegtekintések, függőségi hívások esetében, kivételeket és naplókivonatokat kéri.</span><span class="sxs-lookup"><span data-stu-id="c7539-124">[Join](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) multiple tables – correlate requests with page views, dependency calls, exceptions and log traces.</span></span>
* <span data-ttu-id="c7539-125">Hatékony statisztikai [összesítések](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span><span class="sxs-lookup"><span data-stu-id="c7539-125">Powerful statistical [aggregations](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span></span>
* <span data-ttu-id="c7539-126">Ugyanolyan nagy teljesítményűek, mint az SQL, de az összetett lekérdezésekhez sokkal könnyebben: beágyazási utasítások helyett Ön pipe hello adatait egy elemi művelet toohello mellett.</span><span class="sxs-lookup"><span data-stu-id="c7539-126">Just as powerful as SQL, but much easier for complex queries: instead of nesting statements, you pipe hello data from one elementary operation toohello next.</span></span>
* <span data-ttu-id="c7539-127">Közvetlen és erőteljes képi megjelenítések.</span><span class="sxs-lookup"><span data-stu-id="c7539-127">Immediate and powerful visualizations.</span></span>
* <span data-ttu-id="c7539-128">[PIN-kód diagramokat tooAzure irányítópultok](app-insights-analytics-using.md#pin-to-dashboard).</span><span class="sxs-lookup"><span data-stu-id="c7539-128">[Pin charts tooAzure dashboards](app-insights-analytics-using.md#pin-to-dashboard).</span></span>
* <span data-ttu-id="c7539-129">[Exportálja a lekérdezések tooPower BI](app-insights-analytics-using.md#export-to-power-bi).</span><span class="sxs-lookup"><span data-stu-id="c7539-129">[Export queries tooPower BI](app-insights-analytics-using.md#export-to-power-bi).</span></span>
* <span data-ttu-id="c7539-130">Van egy [REST API](https://dev.applicationinsights.io/) használható toorun lekérdezések programozott módon, például a Powershellből.</span><span class="sxs-lookup"><span data-stu-id="c7539-130">There's a [REST API](https://dev.applicationinsights.io/) that you can use toorun queries programmatically, for example from Powershell.</span></span>


## <a name="connect-tooyour-application-insights-data"></a><span data-ttu-id="c7539-131">Csatlakozás tooyour Application Insights adatainak</span><span class="sxs-lookup"><span data-stu-id="c7539-131">Connect tooyour Application Insights data</span></span>
<span data-ttu-id="c7539-132">Nyissa meg az alkalmazás Analytics [áttekintése panel](app-insights-dashboards.md) az Application Insightsban:</span><span class="sxs-lookup"><span data-stu-id="c7539-132">Open Analytics from your app's [overview blade](app-insights-dashboards.md) in Application Insights:</span></span> 

![Nyissa meg portal.azure.com, nyissa meg az Application Insights-erőforrást, majd kattintson a elemzés.](./media/app-insights-analytics/001.png)


## <a name="video"></a><span data-ttu-id="c7539-134">Videó</span><span class="sxs-lookup"><span data-stu-id="c7539-134">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]



## <a name="query-examples"></a><span data-ttu-id="c7539-135">lekérdezés példák</span><span class="sxs-lookup"><span data-stu-id="c7539-135">Query examples</span></span>

<span data-ttu-id="c7539-136">Próbálja meg forgatókönyvek tooillustrate hello energiagazdálkodási Analytics használatával:</span><span class="sxs-lookup"><span data-stu-id="c7539-136">Try these walkthroughs tooillustrate hello power of using Analytics:</span></span>

 *  [<span data-ttu-id="c7539-137">A teljesítményt és lépés automatikus diagnosztikát egyszer a kérelmek időtartamok</span><span class="sxs-lookup"><span data-stu-id="c7539-137">Automatic diagnostics of spikes and step jumps in requests durations</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Automatic%20diagnostics%20of%20sudden%20spikes%20or%20step%20jumps%20in%20requests%20duration&shared=true)
 *  [<span data-ttu-id="c7539-138">Az adatsorozat elemzés teljesítmény degradations elemzése</span><span class="sxs-lookup"><span data-stu-id="c7539-138">Analyzing performance degradations with time series analysis</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20performance%20degradations%20with%20time%20series%20analysis&shared=true)
 *  [<span data-ttu-id="c7539-139">Alkalmazáshibák autocluster és diffpatterns elemzése</span><span class="sxs-lookup"><span data-stu-id="c7539-139">Analyzing application failures with autocluster and diffpatterns</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20application%20failures%20with%20autocluster%20and%20diffpatterns&shared=true)
 *  [<span data-ttu-id="c7539-140">Speciális alakzat észlelések az adatsorozat elemzés</span><span class="sxs-lookup"><span data-stu-id="c7539-140">Advanced shape detections with time series analysis</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Advanced%20shape%20detection%20with%20time%20series%20analysis&shared=true)
 *  [<span data-ttu-id="c7539-141">Mozgó ablak Műveletek tooanalyze Alkalmazáshasználat (működés közbeni MAU/DAU stb) használatával</span><span class="sxs-lookup"><span data-stu-id="c7539-141">Using sliding window operations tooanalyze application usage (rolling MAU/DAU etc)</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Using%20sliding%20window%20calculations%20to%20analyze%20usage%20metrics:%20rolling%20MAU~2FDAU%20and%20cohorts&shared=true)
 *  <span data-ttu-id="c7539-142">[Az szolgáltatás megszakadása észlelési hibakeresési naplók elemzése alapján](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).</span><span class="sxs-lookup"><span data-stu-id="c7539-142">[Detection of service disruptions based on analysis of debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) and a matching blog post [here](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).</span></span>
 *  <span data-ttu-id="c7539-143">[Profilkészítési alkalmazások teljesítményének egyszerű hibakeresési naplók](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)</span><span class="sxs-lookup"><span data-stu-id="c7539-143">[Profiling applications’ performance using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)</span></span>
 *  <span data-ttu-id="c7539-144">[Egyes lépéseinél a egyszerű hibakeresési naplók segítségével folyamata hello időtartama mérési](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)</span><span class="sxs-lookup"><span data-stu-id="c7539-144">[Measuring hello duration for each step in your code flow using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)</span></span>
 *  <span data-ttu-id="c7539-145">[Egyidejű használata egyszerű hibakeresési naplók elemzése](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)</span><span class="sxs-lookup"><span data-stu-id="c7539-145">[Analyzing concurrency using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)</span></span>



## <a name="next-steps"></a><span data-ttu-id="c7539-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c7539-146">Next steps</span></span>
* <span data-ttu-id="c7539-147">Javasoljuk, hogy a kiindulási pont hello [nyelvi bemutató](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="c7539-147">We recommend you start with hello [language tour](app-insights-analytics-tour.md).</span></span> 
* <span data-ttu-id="c7539-148">További részletek [Analytics segítségével](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="c7539-148">More about [using Analytics](app-insights-analytics-using.md).</span></span> 
* <span data-ttu-id="c7539-149">[Nyelvi referencia](app-insights-analytics-reference.md).</span><span class="sxs-lookup"><span data-stu-id="c7539-149">[Language reference](app-insights-analytics-reference.md).</span></span> 

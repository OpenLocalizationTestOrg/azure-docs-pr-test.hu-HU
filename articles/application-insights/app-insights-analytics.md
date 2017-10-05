---
title: "Elemzés - a hatékony keresési eszköz az Azure Application Insights |} Microsoft Docs"
description: "Elemzés, a hatékony diagnosztikai eszköz az Application Insights áttekintése. "
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
ms.openlocfilehash: 8174745a00a107eea648b223a00466b6a7f37331
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="analytics-in-application-insights"></a><span data-ttu-id="f71e3-103">Az Application Insightsban elemzés</span><span class="sxs-lookup"><span data-stu-id="f71e3-103">Analytics in Application Insights</span></span>
<span data-ttu-id="f71e3-104">[Elemzés](app-insights-analytics.md) a hatékony keresési funkciója [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f71e3-104">[Analytics](app-insights-analytics.md) is the powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="f71e3-105">Ezeken a lapokon a Log Analytics lekérdezési nyelv ismertetik.</span><span class="sxs-lookup"><span data-stu-id="f71e3-105">These pages describe the Log Analytics query language.</span></span> 

* <span data-ttu-id="f71e3-106">**[A bevezető videó](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="f71e3-106">**[Watch the introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="f71e3-107">**[Tesztelése szimulált adataink Analytics meghajtóján](https://analytics.applicationinsights.io/demo)**  Ha az alkalmazás nem adatok küldése az Application Insights még.</span><span class="sxs-lookup"><span data-stu-id="f71e3-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data to Application Insights yet.</span></span>
* <span data-ttu-id="f71e3-108">**[SQL-felhasználók lap cheat](https://aka.ms/sql-analytics)**  fordítja le a leggyakrabban használt idioms.</span><span class="sxs-lookup"><span data-stu-id="f71e3-108">**[SQL-users' cheat sheet](https://aka.ms/sql-analytics)** translates the most common idioms.</span></span>
* <span data-ttu-id="f71e3-109">**[Nyelvi referencia](app-insights-analytics-reference.md)**  a hatékony funkciók Log Analytics lekérdezési nyelv használata.</span><span class="sxs-lookup"><span data-stu-id="f71e3-109">**[Language Reference](app-insights-analytics-reference.md)** Learn how to use all the powerful features of the Log Analytics query language.</span></span>


## <a name="queries-in-analytics"></a><span data-ttu-id="f71e3-110">Az elemzési lekérdezések</span><span class="sxs-lookup"><span data-stu-id="f71e3-110">Queries in Analytics</span></span>
<span data-ttu-id="f71e3-111">Egy tipikus lekérdezés egy *forrás* tábla több követ *operátorok* elválasztott `|`.</span><span class="sxs-lookup"><span data-stu-id="f71e3-111">A typical query is a *source* table followed by a series of *operators* separated by `|`.</span></span> 

<span data-ttu-id="f71e3-112">Például keressük meg melyik szakában Hyderabad polgárainak próbálkozzon a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f71e3-112">For example, let's find out what time of day the citizens of Hyderabad try our web app.</span></span> <span data-ttu-id="f71e3-113">És ott el, amíg nézzük meg, milyen eredménykódok a rendszer visszairányítja a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="f71e3-113">And while we're there, let's see what result codes are returned to their HTTP requests.</span></span> 

```AIQL
requests
| where timestamp > ago(30d)
| summarize ClientCount = dcount(client_IP) by bin(timestamp, 1h), resultCode
| extend LocalTime = timestamp - 4h
| order by LocalTime desc
| render barchart
```

<span data-ttu-id="f71e3-114">A Microsoft száma különböző ügyfél IP-címeket, az elmúlt 7 napban a nap óránkénti bontásban csoportosíthatja őket.</span><span class="sxs-lookup"><span data-stu-id="f71e3-114">We count distinct client IP addresses, grouping them by the hour of the day over the past 7 days.</span></span> 

> [!NOTE]
> <span data-ttu-id="f71e3-115">Az előző 24 órás kívül eredményekhez juthat, "időbélyeg" explicit módon a lekérdezésben, vagy a idő tartomány legördülő menüben.</span><span class="sxs-lookup"><span data-stu-id="f71e3-115">To get results outside the previous 24h, either include 'timestamp' explicitly in your query, or use the time range drop-down menu.</span></span>
>

<span data-ttu-id="f71e3-116">Az eredmények és a sávdiagram bemutató, válassza a verem különböző válaszkódot eredményeinek most megjelenítéséhez:</span><span class="sxs-lookup"><span data-stu-id="f71e3-116">Let's display the results with the bar chart presentation, choosing to stack the results from different response codes:</span></span>

![Válassza ki a sávdiagram, x és y-tengelyek, majd Szegmentálás](./media/app-insights-analytics/020.png)

<span data-ttu-id="f71e3-118">Tűnik, az alkalmazás a legnépszerűbb lunchtime és Hyderabad beágyazása-időpontja.</span><span class="sxs-lookup"><span data-stu-id="f71e3-118">Looks like our app is most popular at lunchtime and bed-time in Hyderabad.</span></span> <span data-ttu-id="f71e3-119">(És 500 kódok kell felülvizsgáljuk.)</span><span class="sxs-lookup"><span data-stu-id="f71e3-119">(And we should investigate those 500 codes.)</span></span>

<span data-ttu-id="f71e3-120">Van még hatékonyabb statisztikai műveletek:</span><span class="sxs-lookup"><span data-stu-id="f71e3-120">There are also powerful statistical operations:</span></span>

![Statisztikai lekérdezés eredményei](./media/app-insights-analytics/025.png)

<span data-ttu-id="f71e3-122">A nyelv számos vonzó lehetőséggel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="f71e3-122">The language has many attractive features:</span></span>


* <span data-ttu-id="f71e3-123">[Szűrő](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) által a mezőket, beleértve az egyéni tulajdonságok és a metrikák a nyers app telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="f71e3-123">[Filter](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) your raw app telemetry by any fields, including your custom properties and metrics.</span></span>
* <span data-ttu-id="f71e3-124">[Csatlakozás](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) több táblázatot – korrelálja Lapmegtekintések, függőségi hívások esetében, kivételeket és naplókivonatokat kéri.</span><span class="sxs-lookup"><span data-stu-id="f71e3-124">[Join](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) multiple tables – correlate requests with page views, dependency calls, exceptions and log traces.</span></span>
* <span data-ttu-id="f71e3-125">Hatékony statisztikai [összesítések](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span><span class="sxs-lookup"><span data-stu-id="f71e3-125">Powerful statistical [aggregations](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span></span>
* <span data-ttu-id="f71e3-126">Ugyanolyan nagy teljesítményűek, mint az SQL, de az összetett lekérdezésekhez sokkal könnyebben: beágyazási utasítások helyett átadhatja az adatokat a következő két elemi művelet.</span><span class="sxs-lookup"><span data-stu-id="f71e3-126">Just as powerful as SQL, but much easier for complex queries: instead of nesting statements, you pipe the data from one elementary operation to the next.</span></span>
* <span data-ttu-id="f71e3-127">Közvetlen és erőteljes képi megjelenítések.</span><span class="sxs-lookup"><span data-stu-id="f71e3-127">Immediate and powerful visualizations.</span></span>
* <span data-ttu-id="f71e3-128">[PIN-kód Azure irányítópultok diagramok](app-insights-analytics-using.md#pin-to-dashboard).</span><span class="sxs-lookup"><span data-stu-id="f71e3-128">[Pin charts to Azure dashboards](app-insights-analytics-using.md#pin-to-dashboard).</span></span>
* <span data-ttu-id="f71e3-129">[Lekérdezések exportálásáról a Power bi-bA](app-insights-analytics-using.md#export-to-power-bi).</span><span class="sxs-lookup"><span data-stu-id="f71e3-129">[Export queries to Power BI](app-insights-analytics-using.md#export-to-power-bi).</span></span>
* <span data-ttu-id="f71e3-130">Van egy [REST API](https://dev.applicationinsights.io/) használható lekérdezések futtatása programozott módon, például a Powershellből.</span><span class="sxs-lookup"><span data-stu-id="f71e3-130">There's a [REST API](https://dev.applicationinsights.io/) that you can use to run queries programmatically, for example from Powershell.</span></span>


## <a name="connect-to-your-application-insights-data"></a><span data-ttu-id="f71e3-131">Csatlakozás az Application Insights adatokhoz</span><span class="sxs-lookup"><span data-stu-id="f71e3-131">Connect to your Application Insights data</span></span>
<span data-ttu-id="f71e3-132">Nyissa meg az alkalmazás Analytics [áttekintése panel](app-insights-dashboards.md) az Application Insightsban:</span><span class="sxs-lookup"><span data-stu-id="f71e3-132">Open Analytics from your app's [overview blade](app-insights-dashboards.md) in Application Insights:</span></span> 

![Nyissa meg portal.azure.com, nyissa meg az Application Insights-erőforrást, majd kattintson a elemzés.](./media/app-insights-analytics/001.png)


## <a name="video"></a><span data-ttu-id="f71e3-134">Videó</span><span class="sxs-lookup"><span data-stu-id="f71e3-134">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]



## <a name="query-examples"></a><span data-ttu-id="f71e3-135">lekérdezés példák</span><span class="sxs-lookup"><span data-stu-id="f71e3-135">Query examples</span></span>

<span data-ttu-id="f71e3-136">Ezek a forgatókönyvek próbálja hatványra emelésének Analytics segítségével mutatja be:</span><span class="sxs-lookup"><span data-stu-id="f71e3-136">Try these walkthroughs to illustrate the power of using Analytics:</span></span>

 *  [<span data-ttu-id="f71e3-137">A teljesítményt és lépés automatikus diagnosztikát egyszer a kérelmek időtartamok</span><span class="sxs-lookup"><span data-stu-id="f71e3-137">Automatic diagnostics of spikes and step jumps in requests durations</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Automatic%20diagnostics%20of%20sudden%20spikes%20or%20step%20jumps%20in%20requests%20duration&shared=true)
 *  [<span data-ttu-id="f71e3-138">Az adatsorozat elemzés teljesítmény degradations elemzése</span><span class="sxs-lookup"><span data-stu-id="f71e3-138">Analyzing performance degradations with time series analysis</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20performance%20degradations%20with%20time%20series%20analysis&shared=true)
 *  [<span data-ttu-id="f71e3-139">Alkalmazáshibák autocluster és diffpatterns elemzése</span><span class="sxs-lookup"><span data-stu-id="f71e3-139">Analyzing application failures with autocluster and diffpatterns</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20application%20failures%20with%20autocluster%20and%20diffpatterns&shared=true)
 *  [<span data-ttu-id="f71e3-140">Speciális alakzat észlelések az adatsorozat elemzés</span><span class="sxs-lookup"><span data-stu-id="f71e3-140">Advanced shape detections with time series analysis</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Advanced%20shape%20detection%20with%20time%20series%20analysis&shared=true)
 *  [<span data-ttu-id="f71e3-141">Mozgó ablak műveletek segítségével (működés közbeni MAU/DAU stb.) az alkalmazás használatának elemzése</span><span class="sxs-lookup"><span data-stu-id="f71e3-141">Using sliding window operations to analyze application usage (rolling MAU/DAU etc)</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Using%20sliding%20window%20calculations%20to%20analyze%20usage%20metrics:%20rolling%20MAU~2FDAU%20and%20cohorts&shared=true)
 *  <span data-ttu-id="f71e3-142">[Az szolgáltatás megszakadása észlelési hibakeresési naplók elemzése alapján](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).</span><span class="sxs-lookup"><span data-stu-id="f71e3-142">[Detection of service disruptions based on analysis of debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) and a matching blog post [here](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).</span></span>
 *  <span data-ttu-id="f71e3-143">[Profilkészítési alkalmazások teljesítményének egyszerű hibakeresési naplók](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)</span><span class="sxs-lookup"><span data-stu-id="f71e3-143">[Profiling applications’ performance using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)</span></span>
 *  <span data-ttu-id="f71e3-144">[Az időtartamot a egyszerű hibakeresési naplók segítségével kód folyamat minden lépése a mérési](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)</span><span class="sxs-lookup"><span data-stu-id="f71e3-144">[Measuring the duration for each step in your code flow using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)</span></span>
 *  <span data-ttu-id="f71e3-145">[Egyidejű használata egyszerű hibakeresési naplók elemzése](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)</span><span class="sxs-lookup"><span data-stu-id="f71e3-145">[Analyzing concurrency using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)</span></span>



## <a name="next-steps"></a><span data-ttu-id="f71e3-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f71e3-146">Next steps</span></span>
* <span data-ttu-id="f71e3-147">Javasoljuk, hogy a kiindulási pont a [nyelvi bemutató](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="f71e3-147">We recommend you start with the [language tour](app-insights-analytics-tour.md).</span></span> 
* <span data-ttu-id="f71e3-148">További részletek [Analytics segítségével](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="f71e3-148">More about [using Analytics](app-insights-analytics-using.md).</span></span> 
* <span data-ttu-id="f71e3-149">[Nyelvi referencia](app-insights-analytics-reference.md).</span><span class="sxs-lookup"><span data-stu-id="f71e3-149">[Language reference](app-insights-analytics-reference.md).</span></span> 

---
title: "Az Azure Application Insightsban észlelési intelligens |} Microsoft Docs"
description: "Az Application Insights az alkalmazás telemetriai adatot automatikus mélyreható elemzésével és figyelmezteti, potenciális problémákat."
services: application-insights
documentationcenter: windows
author: rakefetj
manager: carmonm
ms.assetid: 2eeb4a35-c7a1-49f7-9b68-4f4b860938b2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.openlocfilehash: f203b2a532ea721d9797c67a4750896e3ab2b9f7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="smart-detection-in-application-insights"></a><span data-ttu-id="a0f39-103">Az Application Insightsban intelligens észlelése</span><span class="sxs-lookup"><span data-stu-id="a0f39-103">Smart Detection in Application Insights</span></span>
 <span data-ttu-id="a0f39-104">Intelligens észlelési automatikusan figyelmezteti, mert ez teljesítményproblémákat okozhat a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a0f39-104">Smart Detection automatically warns you of potential performance problems in your web application.</span></span> <span data-ttu-id="a0f39-105">Proaktív, amelyet az alkalmazás küld a telemetriai adatok elemzését végez [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a0f39-105">It performs proactive analysis of the telemetry that your app sends to [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="a0f39-106">Ha nincs, hirtelen megnövekedhet a meghibásodás arányának vagy rendellenes minták ügyfél vagy kiszolgáló teljesítményét, akkor figyelmeztetést kap.</span><span class="sxs-lookup"><span data-stu-id="a0f39-106">If there is a sudden rise in failure rates, or abnormal patterns in client or server performance, you get an alert.</span></span> <span data-ttu-id="a0f39-107">Ez a funkció nincs konfiguráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="a0f39-107">This feature needs no configuration.</span></span> <span data-ttu-id="a0f39-108">Ha az alkalmazás küld elég mérési adat működik.</span><span class="sxs-lookup"><span data-stu-id="a0f39-108">It operates if your application sends enough telemetry.</span></span>

<span data-ttu-id="a0f39-109">Intelligens észlelési riasztásokat érheti el az e-maileket kapni a, mind az intelligens észlelési paneljén.</span><span class="sxs-lookup"><span data-stu-id="a0f39-109">You can access Smart Detection alerts both from the emails you receive, and from the Smart Detection blade.</span></span>

## <a name="review-your-smart-detections"></a><span data-ttu-id="a0f39-110">Tekintse át az intelligens észlelések</span><span class="sxs-lookup"><span data-stu-id="a0f39-110">Review your Smart Detections</span></span>
<span data-ttu-id="a0f39-111">Két módon észlelések fel tud deríteni:</span><span class="sxs-lookup"><span data-stu-id="a0f39-111">You can discover detections in two ways:</span></span>

* <span data-ttu-id="a0f39-112">**E-mailt kapni** az Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a0f39-112">**You receive an email** from Application Insights.</span></span> <span data-ttu-id="a0f39-113">Például a következő:</span><span class="sxs-lookup"><span data-stu-id="a0f39-113">Here's a typical example:</span></span>
  
    ![E-mail értesítés](./media/app-insights-proactive-diagnostics/03.png)
  
    <span data-ttu-id="a0f39-115">A nagy gombra kattintva nyissa meg a további információkhoz juthat a portálon.</span><span class="sxs-lookup"><span data-stu-id="a0f39-115">Click the big button to open more detail in the portal.</span></span>
* <span data-ttu-id="a0f39-116">**Az intelligens észlelési csempe** a az alkalmazás áttekintése panel a legújabb riasztások számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a0f39-116">**The Smart Detection tile** on your app's overview blade shows a count of recent alerts.</span></span> <span data-ttu-id="a0f39-117">Kattintson a csempére kattintva megtekintheti a legújabb riasztások listáját.</span><span class="sxs-lookup"><span data-stu-id="a0f39-117">Click the tile to see a list of recent alerts.</span></span>

![Legutóbbi nézetet észlelések](./media/app-insights-proactive-diagnostics/04.png)

<span data-ttu-id="a0f39-119">Válasszon ki egy riasztást, a részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a0f39-119">Select an alert to see its details.</span></span>

## <a name="what-problems-are-detected"></a><span data-ttu-id="a0f39-120">Milyen problémákat észlelt?</span><span class="sxs-lookup"><span data-stu-id="a0f39-120">What problems are detected?</span></span>
<span data-ttu-id="a0f39-121">Észlelési három fő típusba sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="a0f39-121">There are three kinds of detection:</span></span>

* <span data-ttu-id="a0f39-122">[Észlelési - hiba rendellenességeket intelligens](app-insights-proactive-failure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="a0f39-122">[Smart detection - Failure Anomalies](app-insights-proactive-failure-diagnostics.md).</span></span> <span data-ttu-id="a0f39-123">Gépi tanulási a sikertelen kérelmek az alkalmazás várható sebesség beállításához használjuk, betöltés és más tényezők használatával történik.</span><span class="sxs-lookup"><span data-stu-id="a0f39-123">We use machine learning to set the expected rate of failed requests for your app, correlating with load and other factors.</span></span> <span data-ttu-id="a0f39-124">Ha a hibaaránya kerül, a várt boríték kívül, riasztást kapni.</span><span class="sxs-lookup"><span data-stu-id="a0f39-124">If the failure rate goes outside the expected envelope, we send an alert.</span></span>
* <span data-ttu-id="a0f39-125">[Észlelési - Teljesítményanomáliákat intelligens](app-insights-proactive-performance-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="a0f39-125">[Smart detection - Performance Anomalies](app-insights-proactive-performance-diagnostics.md).</span></span> <span data-ttu-id="a0f39-126">Ha egy művelet vagy függőségi időtartamot válaszideje az lassítja kiinduló képest, vagy ha azt azonosítani a rendellenes mintát válaszidő vagy a lapbetöltési idő értesítéseket kap.</span><span class="sxs-lookup"><span data-stu-id="a0f39-126">You get notifications if response time of an operation or dependency duration is slowing down compared to historical baseline or if we identify an anomalous pattern in response time or page load time.</span></span>   
* <span data-ttu-id="a0f39-127">[Észlelési - Azure Cloud Service problémák intelligens](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/).</span><span class="sxs-lookup"><span data-stu-id="a0f39-127">[Smart detection - Azure Cloud Service issues](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/).</span></span> <span data-ttu-id="a0f39-128">Riasztásokat kaphat, ha az alkalmazás üzemel Azure Cloud Services és a szerepkör példánya van indítási hibák, gyakori újrahasznosítása vagy futásidejű összeomlik.</span><span class="sxs-lookup"><span data-stu-id="a0f39-128">You get alerts if your app is hosted in Azure Cloud Services and a role instance has startup failures, frequent recycling, or runtime crashes.</span></span>

<span data-ttu-id="a0f39-129">(A súgó minden értesítés hivatkozásokra kattintva a kapcsolódó cikkekben.)</span><span class="sxs-lookup"><span data-stu-id="a0f39-129">(The help links in each notification take you to the relevant articles.)</span></span>

## <a name="video"></a><span data-ttu-id="a0f39-130">Videó</span><span class="sxs-lookup"><span data-stu-id="a0f39-130">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="a0f39-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0f39-131">Next steps</span></span>
<span data-ttu-id="a0f39-132">A diagnosztikai eszközök segítségével vizsgálja meg az alkalmazás a telemetriai adatok:</span><span class="sxs-lookup"><span data-stu-id="a0f39-132">These diagnostic tools help you inspect the telemetry from your app:</span></span>

* [<span data-ttu-id="a0f39-133">Metrika explorer</span><span class="sxs-lookup"><span data-stu-id="a0f39-133">Metric explorer</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="a0f39-134">Keresési ablak</span><span class="sxs-lookup"><span data-stu-id="a0f39-134">Search explorer</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="a0f39-135">Elemzés - hatékony lekérdezési nyelv</span><span class="sxs-lookup"><span data-stu-id="a0f39-135">Analytics - powerful query language</span></span>](app-insights-analytics-tour.md)

<span data-ttu-id="a0f39-136">Intelligens észlelési teljesen automatikus.</span><span class="sxs-lookup"><span data-stu-id="a0f39-136">Smart Detection is completely automatic.</span></span> <span data-ttu-id="a0f39-137">De lehet, hogy milyen néhány további riasztások beállítása?</span><span class="sxs-lookup"><span data-stu-id="a0f39-137">But maybe you'd like to set up some more alerts?</span></span>

* [<span data-ttu-id="a0f39-138">Manuálisan konfigurált metrika riasztások</span><span class="sxs-lookup"><span data-stu-id="a0f39-138">Manually configured metric alerts</span></span>](app-insights-alerts.md)
* [<span data-ttu-id="a0f39-139">A webteszt rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="a0f39-139">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md) 


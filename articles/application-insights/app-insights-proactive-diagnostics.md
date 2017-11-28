---
title: "Azure Application Insights az észlelést aaaSmart |} Microsoft Docs"
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
ms.openlocfilehash: f794476088fc69154eda2077b7a5cdc769fab3a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="smart-detection-in-application-insights"></a><span data-ttu-id="cc4c8-103">Az Application Insightsban intelligens észlelése</span><span class="sxs-lookup"><span data-stu-id="cc4c8-103">Smart Detection in Application Insights</span></span>
 <span data-ttu-id="cc4c8-104">Intelligens észlelési automatikusan figyelmezteti, mert ez teljesítményproblémákat okozhat a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-104">Smart Detection automatically warns you of potential performance problems in your web application.</span></span> <span data-ttu-id="cc4c8-105">Az alkalmazás túl küldi hello telemetriai adatot proaktív elemzés végrehajtása[Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cc4c8-105">It performs proactive analysis of hello telemetry that your app sends too[Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="cc4c8-106">Ha nincs, hirtelen megnövekedhet a meghibásodás arányának vagy rendellenes minták ügyfél vagy kiszolgáló teljesítményét, akkor figyelmeztetést kap.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-106">If there is a sudden rise in failure rates, or abnormal patterns in client or server performance, you get an alert.</span></span> <span data-ttu-id="cc4c8-107">Ez a funkció nincs konfiguráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-107">This feature needs no configuration.</span></span> <span data-ttu-id="cc4c8-108">Ha az alkalmazás küld elég mérési adat működik.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-108">It operates if your application sends enough telemetry.</span></span>

<span data-ttu-id="cc4c8-109">Intelligens észlelési riasztásokat érheti el a hello e-maileket kapni, sem pedig a hello intelligens észlelési panelen.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-109">You can access Smart Detection alerts both from hello emails you receive, and from hello Smart Detection blade.</span></span>

## <a name="review-your-smart-detections"></a><span data-ttu-id="cc4c8-110">Tekintse át az intelligens észlelések</span><span class="sxs-lookup"><span data-stu-id="cc4c8-110">Review your Smart Detections</span></span>
<span data-ttu-id="cc4c8-111">Két módon észlelések fel tud deríteni:</span><span class="sxs-lookup"><span data-stu-id="cc4c8-111">You can discover detections in two ways:</span></span>

* <span data-ttu-id="cc4c8-112">**E-mailt kapni** az Application Insights.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-112">**You receive an email** from Application Insights.</span></span> <span data-ttu-id="cc4c8-113">Például a következő:</span><span class="sxs-lookup"><span data-stu-id="cc4c8-113">Here's a typical example:</span></span>
  
    ![E-mail értesítés](./media/app-insights-proactive-diagnostics/03.png)
  
    <span data-ttu-id="cc4c8-115">Kattintson a hello nagy gomb tooopen részletesebb hello portálon.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-115">Click hello big button tooopen more detail in hello portal.</span></span>
* <span data-ttu-id="cc4c8-116">**hello intelligens észlelési csempe** a az alkalmazás áttekintése panel a legújabb riasztások számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-116">**hello Smart Detection tile** on your app's overview blade shows a count of recent alerts.</span></span> <span data-ttu-id="cc4c8-117">Kattintson a hello csempe toosee legújabb riasztások listája.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-117">Click hello tile toosee a list of recent alerts.</span></span>

![Legutóbbi nézetet észlelések](./media/app-insights-proactive-diagnostics/04.png)

<span data-ttu-id="cc4c8-119">Egy riasztás toosee az adatok kijelöléséhez.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-119">Select an alert toosee its details.</span></span>

## <a name="what-problems-are-detected"></a><span data-ttu-id="cc4c8-120">Milyen problémákat észlelt?</span><span class="sxs-lookup"><span data-stu-id="cc4c8-120">What problems are detected?</span></span>
<span data-ttu-id="cc4c8-121">Észlelési három fő típusba sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="cc4c8-121">There are three kinds of detection:</span></span>

* <span data-ttu-id="cc4c8-122">[Észlelési - hiba rendellenességeket intelligens](app-insights-proactive-failure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="cc4c8-122">[Smart detection - Failure Anomalies](app-insights-proactive-failure-diagnostics.md).</span></span> <span data-ttu-id="cc4c8-123">Gépi tanulás használjuk tooset hello várható sebesség a sikertelen kérelmek az alkalmazás terhelés és más tényezők használatával történik.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-123">We use machine learning tooset hello expected rate of failed requests for your app, correlating with load and other factors.</span></span> <span data-ttu-id="cc4c8-124">Ha hello hibaaránya hello várt boríték kívül kerül, riasztást kapni.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-124">If hello failure rate goes outside hello expected envelope, we send an alert.</span></span>
* <span data-ttu-id="cc4c8-125">[Észlelési - Teljesítményanomáliákat intelligens](app-insights-proactive-performance-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="cc4c8-125">[Smart detection - Performance Anomalies](app-insights-proactive-performance-diagnostics.md).</span></span> <span data-ttu-id="cc4c8-126">Ha egy művelet vagy függőségi időtartamot válaszideje csökkentik a összehasonlított toohistorical eredeti, vagy ha azt egy rendellenes mintát azonosítása válaszidő vagy a lapbetöltési idő értesítéseket kap.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-126">You get notifications if response time of an operation or dependency duration is slowing down compared toohistorical baseline or if we identify an anomalous pattern in response time or page load time.</span></span>   
* <span data-ttu-id="cc4c8-127">[Észlelési - Azure Cloud Service problémák intelligens](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/).</span><span class="sxs-lookup"><span data-stu-id="cc4c8-127">[Smart detection - Azure Cloud Service issues](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/).</span></span> <span data-ttu-id="cc4c8-128">Riasztásokat kaphat, ha az alkalmazás üzemel Azure Cloud Services és a szerepkör példánya van indítási hibák, gyakori újrahasznosítása vagy futásidejű összeomlik.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-128">You get alerts if your app is hosted in Azure Cloud Services and a role instance has startup failures, frequent recycling, or runtime crashes.</span></span>

<span data-ttu-id="cc4c8-129">(hello súgó minden értesítés hivatkozások toohello vonatkozó cikkek.)</span><span class="sxs-lookup"><span data-stu-id="cc4c8-129">(hello help links in each notification take you toohello relevant articles.)</span></span>

## <a name="video"></a><span data-ttu-id="cc4c8-130">Videó</span><span class="sxs-lookup"><span data-stu-id="cc4c8-130">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="cc4c8-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc4c8-131">Next steps</span></span>
<span data-ttu-id="cc4c8-132">A diagnosztikai eszközök segítségével vizsgálja meg az alkalmazásból hello telemetriai:</span><span class="sxs-lookup"><span data-stu-id="cc4c8-132">These diagnostic tools help you inspect hello telemetry from your app:</span></span>

* [<span data-ttu-id="cc4c8-133">Metrika explorer</span><span class="sxs-lookup"><span data-stu-id="cc4c8-133">Metric explorer</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="cc4c8-134">Keresési ablak</span><span class="sxs-lookup"><span data-stu-id="cc4c8-134">Search explorer</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="cc4c8-135">Elemzés - hatékony lekérdezési nyelv</span><span class="sxs-lookup"><span data-stu-id="cc4c8-135">Analytics - powerful query language</span></span>](app-insights-analytics-tour.md)

<span data-ttu-id="cc4c8-136">Intelligens észlelési teljesen automatikus.</span><span class="sxs-lookup"><span data-stu-id="cc4c8-136">Smart Detection is completely automatic.</span></span> <span data-ttu-id="cc4c8-137">De lehet, hogy milyen tooset fel néhány további riasztások?</span><span class="sxs-lookup"><span data-stu-id="cc4c8-137">But maybe you'd like tooset up some more alerts?</span></span>

* [<span data-ttu-id="cc4c8-138">Manuálisan konfigurált metrika riasztások</span><span class="sxs-lookup"><span data-stu-id="cc4c8-138">Manually configured metric alerts</span></span>](app-insights-alerts.md)
* [<span data-ttu-id="cc4c8-139">A webteszt rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="cc4c8-139">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md) 


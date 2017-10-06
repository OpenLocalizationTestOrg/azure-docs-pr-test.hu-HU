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
# <a name="smart-detection-in-application-insights"></a>Az Application Insightsban intelligens észlelése
 Intelligens észlelési automatikusan figyelmezteti, mert ez teljesítményproblémákat okozhat a webalkalmazásban. Az alkalmazás túl küldi hello telemetriai adatot proaktív elemzés végrehajtása[Application Insights](app-insights-overview.md). Ha nincs, hirtelen megnövekedhet a meghibásodás arányának vagy rendellenes minták ügyfél vagy kiszolgáló teljesítményét, akkor figyelmeztetést kap. Ez a funkció nincs konfiguráció szükséges. Ha az alkalmazás küld elég mérési adat működik.

Intelligens észlelési riasztásokat érheti el a hello e-maileket kapni, sem pedig a hello intelligens észlelési panelen.

## <a name="review-your-smart-detections"></a>Tekintse át az intelligens észlelések
Két módon észlelések fel tud deríteni:

* **E-mailt kapni** az Application Insights. Például a következő:
  
    ![E-mail értesítés](./media/app-insights-proactive-diagnostics/03.png)
  
    Kattintson a hello nagy gomb tooopen részletesebb hello portálon.
* **hello intelligens észlelési csempe** a az alkalmazás áttekintése panel a legújabb riasztások számát jeleníti meg. Kattintson a hello csempe toosee legújabb riasztások listája.

![Legutóbbi nézetet észlelések](./media/app-insights-proactive-diagnostics/04.png)

Egy riasztás toosee az adatok kijelöléséhez.

## <a name="what-problems-are-detected"></a>Milyen problémákat észlelt?
Észlelési három fő típusba sorolhatók:

* [Észlelési - hiba rendellenességeket intelligens](app-insights-proactive-failure-diagnostics.md). Gépi tanulás használjuk tooset hello várható sebesség a sikertelen kérelmek az alkalmazás terhelés és más tényezők használatával történik. Ha hello hibaaránya hello várt boríték kívül kerül, riasztást kapni.
* [Észlelési - Teljesítményanomáliákat intelligens](app-insights-proactive-performance-diagnostics.md). Ha egy művelet vagy függőségi időtartamot válaszideje csökkentik a összehasonlított toohistorical eredeti, vagy ha azt egy rendellenes mintát azonosítása válaszidő vagy a lapbetöltési idő értesítéseket kap.   
* [Észlelési - Azure Cloud Service problémák intelligens](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/). Riasztásokat kaphat, ha az alkalmazás üzemel Azure Cloud Services és a szerepkör példánya van indítási hibák, gyakori újrahasznosítása vagy futásidejű összeomlik.

(hello súgó minden értesítés hivatkozások toohello vonatkozó cikkek.)

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Következő lépések
A diagnosztikai eszközök segítségével vizsgálja meg az alkalmazásból hello telemetriai:

* [Metrika explorer](app-insights-metrics-explorer.md)
* [Keresési ablak](app-insights-diagnostic-search.md)
* [Elemzés - hatékony lekérdezési nyelv](app-insights-analytics-tour.md)

Intelligens észlelési teljesen automatikus. De lehet, hogy milyen tooset fel néhány további riasztások?

* [Manuálisan konfigurált metrika riasztások](app-insights-alerts.md)
* [A webteszt rendelkezésre állása](app-insights-monitor-web-app-availability.md) 


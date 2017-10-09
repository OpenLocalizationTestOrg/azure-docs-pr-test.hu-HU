---
title: "több előny az Azure Application Insights aaaGet |} Microsoft Docs"
description: "Első lépések az Application insights szolgáltatással, miután ez ismerje meg a hello szolgáltatások összegzését."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: bwren
ms.openlocfilehash: 2023728afcf5aa5ecab8b957c8517d4872668765
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="more-telemetry-from-application-insights"></a>Az Application Insights további telemetria
Miután [hozzáadott Application Insights tooyour ASP.NET kódot](app-insights-asp-net.md), nincsenek teheti tooget néhány dolgot még több telemetriai. 

| Műveletek | Amihez jut|
|---|---|
|(IIS-kiszolgálókkal) [Állapotmonitor telepítése](http://go.microsoft.com/fwlink/?LinkId=506648) a minden egyes kiszolgáló-számítógépén.<br/>(Az azure web apps) A hello Azure Vezérlőpulton hello webalkalmazás hello Application Insights paneljének megnyitásához.| [**Teljesítményszámlálók**](app-insights-performance-counters.md)<br/>[**Kivételek** ](app-insights-asp-net-exceptions.md) – részletes kivételeseményekhez megadhat veremkiíratásokat,<br/>[**Függőségek**](app-insights-asp-net-dependencies.md)|
|[Hello JavaScript részlet tooyour weblapok hozzáadása](app-insights-javascript.md)|[Teljesítmény lapon](app-insights-web-track-usage.md), böngésző kivételeket, az AJAX-teljesítmény. Az ügyféloldali egyéni telemetriai adatokat.|
|[Rendelkezésre állási webteszt létrehozása](app-insights-monitor-web-app-availability.md)|Riasztásokat kaphat, ha a webhely nem érhető el.|
|[Győződjön meg arról buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) MSBuild állítja elő|[Build annotationsin metrika diagramok](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Egyéni események és metrikák írása](app-insights-api-custom-events-metrics.md)|Az üzleti eseményeket és metrikákat száma, nyomon követheti a részletes használati és még sok más.|
|[Élő webhelyét profilját](https://aka.ms/AIProfilerPreview)|Az élő webalkalmazását részletes függvény időzítés|







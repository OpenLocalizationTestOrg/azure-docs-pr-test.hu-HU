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
# <a name="analytics-in-application-insights"></a>Az Application Insightsban elemzés
[Elemzés](app-insights-analytics.md) hello hatékony keresési funkciója [Application Insights](app-insights-overview.md). Ezeken a lapokon a Log Analytics lekérdezési nyelv ismertetik. 

* **[Hello bevezető videó](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Tesztelése szimulált adataink Analytics meghajtóján](https://analytics.applicationinsights.io/demo)**  Ha az alkalmazás még nem küld adatokat tooApplication Insights.
* **[SQL-felhasználók lap cheat](https://aka.ms/sql-analytics)**  hello leggyakoribb idioms lefordítja.
* **[Nyelvi referencia](app-insights-analytics-reference.md)**  megtudhatja, hogyan összes toouse hello hatékony szolgáltatásainak hello Log Analytics lekérdezési nyelv.


## <a name="queries-in-analytics"></a>Az elemzési lekérdezések
Egy tipikus lekérdezés egy *forrás* tábla több követ *operátorok* elválasztott `|`. 

Például keressük meg milyen alkalommal nap hello polgárainak Hyderabad, próbálja meg a webalkalmazást. És nincs el, amíg nézzük meg, milyen eredménykódok visszaadott tootheir HTTP-kérelmekre. 

```AIQL
requests
| where timestamp > ago(30d)
| summarize ClientCount = dcount(client_IP) by bin(timestamp, 1h), resultCode
| extend LocalTime = timestamp - 4h
| order by LocalTime desc
| render barchart
```

A Microsoft száma különböző ügyfél IP-címeket, csoportosíthatja őket hello óránként hello napon keresztül hello az elmúlt 7 napban. 

> [!NOTE]
> tooget eredmények kívül hello előző 24 órát, "időbélyeg" explicit módon a lekérdezést, vagy a hello idő tartomány legördülő menüből.
>

Az eszközterület-diagram megjelenítési hello eredmények toostack választani a különböző válaszkódot hello hello eredmények most megjelenítéséhez:

![Válassza ki a sávdiagram, x és y-tengelyek, majd Szegmentálás](./media/app-insights-analytics/020.png)

Tűnik, az alkalmazás a legnépszerűbb lunchtime és Hyderabad beágyazása-időpontja. (És 500 kódok kell felülvizsgáljuk.)

Van még hatékonyabb statisztikai műveletek:

![Statisztikai lekérdezés eredményei](./media/app-insights-analytics/025.png)

hello nyelv számos vonzó lehetőséggel rendelkezik:


* [Szűrő](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) által a mezőket, beleértve az egyéni tulajdonságok és a metrikák a nyers app telemetriai adatokat.
* [Csatlakozás](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) több táblázatot – korrelálja Lapmegtekintések, függőségi hívások esetében, kivételeket és naplókivonatokat kéri.
* Hatékony statisztikai [összesítések](https://docs.loganalytics.io/learn/tutorials/aggregations.html).
* Ugyanolyan nagy teljesítményűek, mint az SQL, de az összetett lekérdezésekhez sokkal könnyebben: beágyazási utasítások helyett Ön pipe hello adatait egy elemi művelet toohello mellett.
* Közvetlen és erőteljes képi megjelenítések.
* [PIN-kód diagramokat tooAzure irányítópultok](app-insights-analytics-using.md#pin-to-dashboard).
* [Exportálja a lekérdezések tooPower BI](app-insights-analytics-using.md#export-to-power-bi).
* Van egy [REST API](https://dev.applicationinsights.io/) használható toorun lekérdezések programozott módon, például a Powershellből.


## <a name="connect-tooyour-application-insights-data"></a>Csatlakozás tooyour Application Insights adatainak
Nyissa meg az alkalmazás Analytics [áttekintése panel](app-insights-dashboards.md) az Application Insightsban: 

![Nyissa meg portal.azure.com, nyissa meg az Application Insights-erőforrást, majd kattintson a elemzés.](./media/app-insights-analytics/001.png)


## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]



## <a name="query-examples"></a>lekérdezés példák

Próbálja meg forgatókönyvek tooillustrate hello energiagazdálkodási Analytics használatával:

 *  [A teljesítményt és lépés automatikus diagnosztikát egyszer a kérelmek időtartamok](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Automatic%20diagnostics%20of%20sudden%20spikes%20or%20step%20jumps%20in%20requests%20duration&shared=true)
 *  [Az adatsorozat elemzés teljesítmény degradations elemzése](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20performance%20degradations%20with%20time%20series%20analysis&shared=true)
 *  [Alkalmazáshibák autocluster és diffpatterns elemzése](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20application%20failures%20with%20autocluster%20and%20diffpatterns&shared=true)
 *  [Speciális alakzat észlelések az adatsorozat elemzés](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Advanced%20shape%20detection%20with%20time%20series%20analysis&shared=true)
 *  [Mozgó ablak Műveletek tooanalyze Alkalmazáshasználat (működés közbeni MAU/DAU stb) használatával](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Using%20sliding%20window%20calculations%20to%20analyze%20usage%20metrics:%20rolling%20MAU~2FDAU%20and%20cohorts&shared=true)
 *  [Az szolgáltatás megszakadása észlelési hibakeresési naplók elemzése alapján](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).
 *  [Profilkészítési alkalmazások teljesítményének egyszerű hibakeresési naplók](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)
 *  [Egyes lépéseinél a egyszerű hibakeresési naplók segítségével folyamata hello időtartama mérési](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)
 *  [Egyidejű használata egyszerű hibakeresési naplók elemzése](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) és egy megfelelő blogbejegyzést [Itt](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)



## <a name="next-steps"></a>Következő lépések
* Javasoljuk, hogy a kiindulási pont hello [nyelvi bemutató](app-insights-analytics-tour.md). 
* További részletek [Analytics segítségével](app-insights-analytics-using.md). 
* [Nyelvi referencia](app-insights-analytics-reference.md). 

---
title: Mi az Azure Application Insights aaa? | Microsoft Docs
description: "Alkalmazásteljesítmény-felügyelet és élő webalkalmazások használatának nyomon követése.  Észlelheti, osztályozhatja és diagnosztizálhatja a problémákat, valamint megismerheti, hogy a felhasználók miként használják alkalmazását."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 379721d1-0f82-445a-b416-45b94cb969ec
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/14/2017
ms.author: bwren
ms.openlocfilehash: d2596f53a36991fcd08551e6395ece68a5801e39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-application-insights"></a>Mi az Application Insights?
Az Application Insights egy bővíthető és több platformon működő alkalmazásteljesítmény-felügyeleti (APM) szolgáltatás webfejlesztőknek. Ezzel toomonitor élő webalkalmazásokat. Automatikusan felismeri a teljesítményanomáliákat. Ez magában foglalja a hatékony analytics eszközök toohelp problémákat és a felhasználók számára ténylegesen elvégezni az alkalmazás toounderstand diagnosztizálásához.  Úgy van kialakítva, hogy folyamatosan teljesítményük és használhatóságuk javításában toohelp. Az alkalmazások működését platformokon, beleértve a .NET, Node.js és J2EE számos, a helyben tárolt vagy hello felhőben. Integrálható a devOps folyamat, és a csatlakozási pontok tooa különböző Fejlesztőeszközök rendelkezik.

![Felhasználói tevékenységek statisztikáit ábrázolhatja diagramon, vagy konkrét eseményeket elemezhet.](./media/app-insights-overview/00-sample.png)

[Vessen egy pillantást hello bevezetés animáció](https://www.youtube.com/watch?v=fX2NtGrh-Y0).

## <a name="how-does-application-insights-work"></a>Hogyan működik az Application Insights?
Kis instrumentation csomag telepítése az alkalmazásban, és állítsa be az Application Insights-erőforrás hello Microsoft Azure-portálon. hello instrumentation figyeli az alkalmazást, és elküldi a telemetriai adatok toohello portálon. (hello alkalmazások bárhol futhatnak - nem rendelkezik Azure-ban üzemeltetett toobe.)

Nem csak hello webszolgáltatási alkalmazás, de bármely háttér összetevőket is beállíthatják, és JavaScript hello maguk hello weblapokon. 

![Application Insights instrumentation az alkalmazás küld telemetriai tooyour Application Insights-erőforrást.](./media/app-insights-overview/01-scheme.png)


Ezenkívül Ön lekéréses a telemetriai adatok, például teljesítményszámlálók, az Azure diagnostics vagy Docker naplók hello állomás környezetből. Webes tesztjeinek használatát, amely rendszeresen küld szintetikus kérelmek tooyour webszolgáltatás is állíthat be.

Az összes telemetriai adatokat ezekbe az adatfolyamokba integrálva vannak az Azure-portálon, ahol tanúsítványszűrést alkalmazhat hatékony hello elemzési és keresési eszközök toohello nyers adatok.


### <a name="whats-hello-overhead"></a>Mi az a hello terhelés?
hello gyakorolt hatása az alkalmazás nem nagyon nagy. A nem blokkoló nyomkövetési hívásokat a rendszer kötegeli, és a küldés külön szálakon történik.

## <a name="what-does-application-insights-monitor"></a>Mit figyel az Application Insights?

Az Application Insights hello fejlesztőcsoportunk, hogy tudomásul veszi, hogyan működik-e az alkalmazást, és hogyan használatos toohelp célja. A szolgáltatás az alábbiakat figyeli:

* **Kérések sebessége, válaszidők és hibaarányok** – megtudhatja, hogy mely lapok, mely napszakokban a legnépszerűbbek, és hol találhatók a felhasználók. Megtekintheti, hogy mely lapok teljesítenek a legjobban. Ha több kérés esetén a válaszidők és a hibaarányok értéke megnő, valószínűleg erőforrás-gazdálkodási hibáról van szó. 
* **Függőségi értékek, válaszidők és hibaarányok** – megtudhatja, hogy mely külső szolgáltatások okoznak lassulást.
* **Kivételek** - elemzése hello összesített statisztikák, vagy válasszon olyan specifikus példányai, és elemezze hello veremkiíratási adataival és a kapcsolódó kérések. A kiszolgálói és a böngészői kivételekről egyaránt készül jelentés.
* **Lapmegtekintések és betöltési teljesítmény** – a felhasználói böngészők jelentése alapján készül.
* Weblapokról származó **AJAX-hívások** – értékek, válaszidők és hibaarányok.
* **Felhasználók és munkamenetek száma**.
* Windows vagy Linux rendszerű kiszolgálói gépekről származó **teljesítményszámlálók**, például processzor-, memória- és hálózathasználat. 
* Dockerből vagy Azure-ból származó **gazdadiagnosztika**. 
* Alkalmazásból származó **nyomkövetési naplók diagnosztikája** – megállapíthatja a nyomkövetési események és a kérések korrelációját.
* **Egyéni események és metrikák** hogy írni saját kezűleg hello ügyfél vagy kiszolgáló-kódban, tootrack üzleti események például elemek értékesített vagy megnyert játékok.

## <a name="where-do-i-see-my-telemetry"></a>Hol láthatók a telemetriai adatok?

Nincsenek bőven módon tooexplore adatait. Olvassa el az alábbi cikkeket:

|  |  |
| --- | --- |
| [**Intelligens észlelés és manuális riasztások**](app-insights-proactive-diagnostics.md)<br/>Riasztások automatikus igazítja tooyour alkalmazás normál mintákat keressen az eseményindító és telemetriai adatokat, amikor szükség van kívül hello szokásos mintát. [Riasztásokat is beállíthat](app-insights-alerts.md) egyéni vagy Standard mérőszámok adott szintjeire. |![Példa a riasztásokra](./media/app-insights-overview/alerts-tn.png) |
| [**Alkalmazástérkép**](app-insights-app-map.md)<br/>az alkalmazás metrikáit és a riasztások hello összetevői. |![Alkalmazástérkép](./media/app-insights-overview/appmap-tn.png)  |
| [**Profilkészítő**](app-insights-profiler.md)<br/>Vizsgálja meg a mintában szereplő kérelmek hello végrehajtási profilok. |![Profilkészítő](./media/app-insights-overview/profiler.png) |
| [**Használatelemzés**](app-insights-usage-overview.md)<br/>Felhasználószegmentálás és -megtartás elemzése.|![Megtartási eszköz](./media/app-insights-overview/retention.png) |
| [**Példányadatok diagnosztikai keresése**](app-insights-diagnostic-search.md)<br/>Események keresése és szűrése, például kérések, kivételek, függőségi hívások, naplókivonatok és lapmegtekintések.  |![Telemetriai adatok keresése](./media/app-insights-overview/search-tn.png) |
| [**Összesített adatok metrikaböngészője**](app-insights-metrics-explorer.md)<br/>Összesített adatok – például kérés- és hibaarányok, valamint kivételek, válaszidők és lapbetöltési idők – böngészése, szűrése és szegmentálása. |![Mérőszámok](./media/app-insights-overview/metrics-tn.png) |
| [**Irányítópultok**](app-insights-dashboards.md#dashboards)<br/>Különböző erőforrásokból származó adatokat fűzhet össze és oszthat meg másokkal. Nagy több összetevőt alkalmazások, valamint a folyamatos megjelenítési hello team helyiségben. |![Példa az irányítópultokra](./media/app-insights-overview/dashboard-tn.png) |
| [**Élő metrikastream**](app-insights-live-stream.md)<br/>Amikor telepít egy új buildverziót, tekintse meg a közel valós idejű teljesítmény mutatók toomake meg arról, hogy minden megfelelően működik-e. |![Példa a valós idejű metrikákra](./media/app-insights-overview/live-metrics-tn.png) |
| [**Elemzés**](app-insights-analytics.md)<br/>A hatékony lekérdezési nyelvnek köszönhetően válaszokat kaphat az alkalmazás teljesítményére és használatára vonatkozó legégetőbb kérdésekre. |![Példa az elemzésre](./media/app-insights-overview/analytics-tn.png) |
| [**Visual Studio**](app-insights-visual-studio.md)<br/>Lásd: teljesítményadatokat hello kódban. Nyissa meg a toocode híváslánc megjelenik az.|![Visual Studio](./media/app-insights-overview/visual-studio-tn.png) |
| [**Pillanatkép-hibakereső**](app-insights-snapshot-debugger.md)<br/>A működés közbeni műveletekről készült pillanatképek hibakeresése paraméterértékekkel.|![Visual Studio](./media/app-insights-overview/snapshot.png) |
| [**Power BI**](app-insights-export-power-bi.md)<br/>Integrálhatja a használati metrikákat más üzleti intelligenciával.| ![Power BI](./media/app-insights-overview/power-bi.png)|
| [**REST API**](https://dev.applicationinsights.io/)<br/>Írhat kódot toorun lekérdezések a metrikák és a nyers adatok.| ![REST API](./media/app-insights-overview/rest-tn.png) |
| [**Folyamatos exportálás**](app-insights-export-telemetry.md)<br/>Nyers adatok toostorage, amint megérkeznek tömeges exportálása. |![Exportálás](./media/app-insights-overview/export-tn.png) |

## <a name="how-do-i-use-application-insights"></a>Hogyan használható az Application Insights?

### <a name="monitor"></a>Figyelés
Telepítse az Application Insightsot az alkalmazásba, állítsa be a [rendelkezésre állási webes teszteket](app-insights-monitor-web-app-availability.md), és az alábbiakra nyílik lehetőség:

* Állítson be egy [irányítópult](app-insights-dashboards.md) a csapat hely tookeep követheti a terhelés, válaszidejét és a függőségek hello teljesítményét egy, a lapon a terhelés és AJAX-hívások.
* Fedezze fel leglassabb hello és a legtöbb sikertelen kérelem.
* A Watch [élő adatfolyam](app-insights-live-stream.md) új kiadását, azonnal kapcsolatos romlását tooknow telepítésekor.

### <a name="detect-diagnose"></a>Észlelés, diagnosztizálás
Riasztások fogadásakor vagy problémák észlelésekor:

* Felmérheti, hogy hány felhasználó érintett.
* Elvégezheti a kivételek, a függőségi hívások és a nyomkövetési adatok korrelációját.
* A profilkészítő, a pillanatképek, a veremkiíratások és a nyomkövetési naplók vizsgálata.

### <a name="build-measure-learn"></a>Fejlesztés, mérés, tapasztalatszerzés
[Hello hatékonyságának mérésére](app-insights-usage-overview.md) egyes új szolgáltatások telepítése.

* Tervezze meg toomeasure hogy az ügyfelek hogyan használják az új UX vagy üzleti szolgáltatásokat.
* Egyéni telemetriai adatokat vehet fel a kódba.
* Alapszintű hello következő fejlesztési ciklus a rögzített bizonyító adatok a a telemetriai adatokból.

## <a name="get-started"></a>Bevezetés
Az Application Insights a Microsoft Azure és a telemetriai adatok gazdája számos szolgáltatás nem továbbítja a elemzés és bemutató hello egyike. Így ahhoz, hogy bármi más, szüksége lesz egy előfizetés túl[Microsoft Azure](http://azure.com). Az ingyenes toosign működik-e, és ha úgy dönt, alapszintű hello [terv árképzési](https://azure.microsoft.com/pricing/details/application-insights/) az Application Insights használata díjmentes mindaddig, amíg az alkalmazás toohave jelentős használati nőtt. Ha a szervezet már rendelkezik előfizetéssel, akkor hozzáadhatja a Microsoft-fiók tooit.

Számos módon tooget elindult. Kezdje azzal, amelyik Önnek a legmegfelelőbb. Később hello mások is hozzáadhat.

* **At futásidejű: állíthatnak be a webalkalmazás hello kiszolgálón.** Ezzel elkerülheti a frissítési toohello kódja. Rendszergazdai hozzáférés tooyour kiszolgálóra van szükség.
  * [**IIS a helyszínen vagy egy virtuális gépen**](app-insights-monitor-performance-live-website-now.md)
  * [**Azure-webalkalmazás vagy virtuális gép**](app-insights-monitor-performance-live-website-now.md)
  * [**J2EE**](app-insights-java-live.md)
* **Fejlesztési időpontban: vegye fel az Application Insights tooyour kódot.** Lehetővé teszi a toowrite egyéni telemetria és tooinstrument háttér- és asztali alkalmazások.
  * [Visual Studio](app-insights-asp-net.md) 2013 2. frissítés vagy újabb.
  * Java és [Eclipse](app-insights-java-eclipse.md) vagy [más eszközök](app-insights-java-get-started.md)
  * [Node.js](app-insights-nodejs.md)
  * [Más platformok](app-insights-platforms.md)
* **[Vizsgálhatja a weblapokat](app-insights-javascript.md)** lapmegtekintés, AJAX-használat és egyéb ügyféloldali telemetria tekintetében.
* **[Rendelkezésre állási tesztek](app-insights-monitor-web-app-availability.md)** – rendszeresen pingelheti webhelyét kiszolgálóinkról.


## <a name="next-steps"></a>Következő lépések
Első lépések futtatáskor:

* [IIS-kiszolgáló](app-insights-monitor-performance-live-website-now.md)
* [J2EE-kiszolgáló](app-insights-java-live.md)

Első lépések fejlesztéskor:

* [ASP.NET](app-insights-asp-net.md)
* [Java](app-insights-java-get-started.md)
* [Node.js](app-insights-nodejs.md)

## <a name="support-and-feedback"></a>Támogatás és visszajelzés
* Kérdések és problémák:
  * [Hibaelhárítás][qna]
  * [MSDN-fórum](https://social.msdn.microsoft.com/Forums/vstudio/home?forum=ApplicationInsights)
  * [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* Javaslatok:
  * [UserVoice-on](https://visualstudio.uservoice.com/forums/357324)
* Blog:
  * [Application Insights blog](https://azure.microsoft.com/blog/tag/application-insights)

## <a name="videos"></a>Videók

[![Animált bevezetés](./media/app-insights-overview/video-front-1.png)](https://www.youtube.com/watch?v=fX2NtGrh-Y0)

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

<!--Link references-->

[android]: https://github.com/Microsoft/ApplicationInsights-Android
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[desktop]: app-insights-windows-desktop.md
[detect]: app-insights-detect-triage-diagnose.md
[greenbrown]: app-insights-asp-net.md
[ios]: https://github.com/Microsoft/ApplicationInsights-iOS
[java]: app-insights-java-get-started.md
[knowUsers]: app-insights-web-track-usage.md
[platforms]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md

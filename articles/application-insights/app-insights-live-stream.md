---
title: "egyéni metrikákkal és diagnosztika Azure Application Insights a metrikák adatfolyam aaaLive |} Microsoft Docs"
description: "A webalkalmazás egyéni metrikák valós idejű figyelése és a hibák, a nyomkövetések és az események élő adatcsatornára eseményadatokat."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: bwren
ms.openlocfilehash: 68ddfbf387379ea778c20280c4ec96baa06d3273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a>Metrikák adatfolyamot: Figyelő & Diagnosztizálás és 1 másodperc késleltetés 

Az élő, éles a webes alkalmazás hello ki szívveréseket szív mintavételi származó élő metrikák adatfolyam használatával [Application Insights](app-insights-overview.md). Válassza ki, és szűrheti a metrikák és a teljesítmény számlálók toowatch valós idejű semmilyen zavart tooyour szolgáltatás nélkül. Vizsgálja meg a minta sikertelen kérelmek és kivételek híváslánc megjelenik. Együtt [Profilkészítő](app-insights-profiler.md), [pillanatkép hibakereső](app-insights-snapshot-debugger.md), és [Teljesítménytesztelés](app-insights-monitor-web-app-availability.md#performance-tests), metrikák adatfolyamot egy hatékony és nem zavarja a munkában diagnosztikai eszköz biztosít a élő webhelyet.

Az élő metrikák adatfolyam-továbbítás segítségével:

* Egy javítást érvényesítése közben megjelent, amelyet figyeli a teljesítmény és a hiba számát.
* Hello hatásának tesztelése terhelések, és a problémák diagnosztizálásához tekintse meg élő. 
* Adott teszt munkamenetek összpontosítson, vagy kiválasztásával, és azt szeretné, hogy toowatch hello metrikák szűrés szűrheti ismert problémákkal kapcsolatban.
* Első kivétel nyomkövetési adatokat, akkor fordulhat elő.
* A szűrők kísérlet toofind hello legfontosabb KPI-k.
* Bármely Windows-teljesítmény számláló élő figyelése.
* Egy kiszolgálót, amelynek problémák azonosítását, és minden hello, KPI-t vagy élő adatcsatorna toojust szűrheti, hogy a kiszolgáló.

[![Élő adatfolyam metrikák videó](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

Metrikák élő adatfolyam érhető el jelenleg a helyszínen futtatott ASP.NET-alkalmazásokra vagy hello felhő. 

## <a name="get-started"></a>Bevezetés

1. Ha még nem [telepítve az Application Insights](app-insights-asp-net.md) az ASP.NET web app alkalmazásban vagy [Windows server alkalmazás](app-insights-windows-services.md), most tegye. 
2. **Frissítés toohello legújabb verziója** hello Application Insights csomag. A Visual Studióban, kattintson jobb gombbal a projektre, és válassza a **kezelése Nuget-csomagok**. Megnyitás hello **frissítések** lapon jelölje **közé tartoznak az előzetes**, és válassza ki az összes hello Microsoft.ApplicationInsights.* csomagokat.

    Helyezze ismét üzembe alkalmazását.

3. A hello [Azure-portálon](https://portal.azure.com), nyissa meg a hello Application Insights-erőforrást az alkalmazáshoz, és nyissa meg az élő adatfolyam.

4. [Biztonságos hello vezérlőcsatorna](#secure-the-control-channel) ha bizalmas adatok, például ügyfélnevek használhatja a szűrők.


![Hello áttekintése paneljén kattintson az élő adatfolyam](./media/app-insights-live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a>Nincs adat? Ellenőrizze a kiszolgáló tűzfal

Ellenőrizze a hello [kimenő portok a metrikák élő adatfolyam](app-insights-ip-addresses.md#outgoing-ports) megnyitott hello tűzfal-kiszolgálókhoz. 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a>Eltérések a Metrikaböngésző és az elemzések adatfolyamot metrikák?

| |Élő stream | Metrikaböngésző és elemzés |
|---|---|---|
|Késés|Egy második belül megjelenített adatok|Perc alatt összesített értéket|
|Visszatartás nem|Adatok továbbra is fennáll, amíg ez a hello diagramra, és utána eldobja|[Az adatok 90 napig őrzi meg](app-insights-data-retention-privacy.md#how-long-is-the-data-kept)|
|Az igény szerinti|Amíg nyissa meg a metrikák élő adatok továbbítja adatfolyamként|Az adatokat küldi el, amikor hello SDK telepítése és engedélyezése|
|Ingyenes|Az élő adatfolyam adatok ingyenesek|Tulajdonos túl[díjszabása](app-insights-pricing.md)
|Mintavételezés|Az összes kijelölt metrikák és számlálók továbbít. Hibák és híváslánc megjelenik mintát. TelemetryProcessors nem érvényesek.|Lehet, hogy események [mintát](app-insights-api-filtering-sampling.md)|
|Vezérlőcsatorna|Szűrő vezérlő jelek toohello SDK küldése. Javasoljuk, hogy [a csatornát](#secure-channel).|Kommunikációs még csak egyirányú, toohello portál|


## <a name="select-and-filter-your-metrics"></a>Válassza ki, és a metrikák szűrése

(Klasszikus található ASP.NET-alkalmazások hello SDK legújabb.)

Egyéni KPI élő tetszőleges szűrők alkalmazásával bármely Application Insights telemetria hello portálról figyelheti. Kattintson a hello szűrővezérlő bemutatja, amikor Ön rámutatáskor hello diagramok bármelyikét. a következő diagram hello van egy egyéni kérelem száma KPI URL-cím és időtartama attribútumok szűrőkkel ábrázolásához. Ellenőrizze a szűrőket a hello adatfolyam Preview szakasz bemutatja egy élő adatcsatorna telemetriai adatot, amely megfelel az idő megadott bármikor hello feltételeket. 

![Egyéni kérelem KPI](./media/app-insights-live-stream/live-stream-filteredMetric.png)

Figyelheti a roleservice száma. hello beállítások függ hello adatfolyam, amely lehet bármely Application Insights telemetria: kérelmek, függőségek, kivételek, nyomkövetések, eseményeket és metrikákat. Azok a saját [egyéni mérési](app-insights-api-custom-events-metrics.md#properties):

![Érték-beállítások](./media/app-insights-live-stream/live-stream-valueoptions.png)

Ezenkívül tooApplication Insights telemetria, is figyelheti bármely Windows teljesítményszámláló jelölje ki, amely hello adatfolyam lehetőségek közül, és hello teljesítményszámláló hello helynév.

Élő metrikák összesítése két időpontokban: helyileg az egyes kiszolgálókon, majd a összes kiszolgáló között. Hello alapértelmezett bármelyik hello megfelelő legördülő listákat található egyéb beállítások segítségével módosíthatja.

## <a name="sample-telemetry-custom-live-diagnostic-events"></a>A minta Telemetriai: Egyéni élő diagnosztikai eseményei
Alapértelmezés szerint hello élő események jelennek meg a sikertelen kérelmek és függőségi hívások esetében, kivételek, eseményeket, valamint nyomkövetési adatokat. Kattintson a hello ikon toosee alkalmazott hello szűrőfeltételt bármikor idő. 

![Alapértelmezett működés közbeni adatcsatorna](./media/app-insights-live-stream/live-stream-eventsdefault.png)

Mint a metrikákat, megadhatja bármilyen tetszőleges feltételek tooany hello Application Insights telemetria típusú. Ebben a példában azt kiválasztása adott kérelem sikertelen, a nyomkövetések és az események. Azt is kiválasztásával, kivételeket és a függőségi hibák.

![Egyéni élő adatcsatorna](./media/app-insights-live-stream/live-stream-events.png)

Megjegyzés: Jelenleg, kivétel üzenetalapú feltétel hello legkülső kivétel üzenetét használja. Az előző példában hello toofilter kimenő hello jóindulatú kivétel a belső kivétel üzenet (követi hello "<--" elválasztó) "a program hello ügyfél bontotta a kapcsolatot." Használjon egy üzenet nem-"Hibaüzenet kérés tartalma" feltételeket tartalmaz.

A Részletek területen hello hello levő elem hírcsatorna élő kattintással. Akár szüneteltetheti is, vagy kattintson a hírcsatorna hello **szüneteltetése** vagy egyszerűen a lefelé görgetéshez használható, vagy elemet. Élő adatcsatorna után görgessen a hátsó toohello felső folytatódik, és hello számláló elemek kattintva gyűjtött során fel lett függesztve.

![Élő hibák mintát](./media/app-insights-live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a>Szűrés a server-példány

Ha azt szeretné, hogy egy adott kiszolgálói szerepkör példánya toomonitor, kiszolgáló szerint szűrheti.

![Élő hibák mintát](./media/app-insights-live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a>SDK-követelmények
Egyéni élő metrikák adatfolyama verzió 2.4.0-beta2 érhető el vagy az újabb [Web Application Insights SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). Ne felejtse el a NuGet-Csomagkezelő tooselect "Tartalmaznak Prerelease" kapcsolót.

## <a name="secure-hello-control-channel"></a>Biztonságos hello vezérlőcsatorna
hello megadott egyéni szűrők feltételek hátsó toohello élő metrikák összetevő küldése az Application Insights SDK hello. hello szűrők tartalmazhat customerIDs például bizalmas adatokat. A titkos API-kulcs biztonságos továbbá toohello instrumentation kulcs hello csatorna végezheti el.
### <a name="create-an-api-key"></a>API-kulcs létrehozása

![Api-kulcs létrehozása](./media/app-insights-live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-tooconfiguration"></a>API-kulcs tooConfiguration hozzáadása
Hello applicationinsights.config fájlban adja hozzá hello AuthenticationApiKey toohello QuickPulseTelemetryModule:
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add> 

```
Vagy a kódban, állítsa be a hello QuickPulseTelemetryModule:

``` C#

    module.AuthenticationApiKey = "YOUR-API-KEY-HERE";

```

Ha ismeri fel, és az összes csatlakoztatott kiszolgálók hello megbízható, megpróbálhatja hello egyéni szűrők hitelesített hello csatorna nélkül. Ez a beállítás hat hónapig érhető el. Ez a felülbírálás kell egyszer minden új munkamenet, vagy ha új kiszolgáló online elérhető lesz.

![Élő metrikák hitelesítési beállítások](./media/app-insights-live-stream/live-stream-auth.png)

>[!NOTE]
>Erősen ajánlott beállítani a hello hitelesített csatornát hello szűrési feltételeket a potenciálisan bizalmas adatok, például a CustomerID megadása előtt.
>

## <a name="generating-a-performance-test-load"></a>Egy teszt víruskeresőké létrehozása

Ha azt szeretné, egy terheléselosztási toowatch hello hatásának növelése érdekében használjon hello teljesítményteszt panel. Egyidejű felhasználók száma kérelmeinek szimulálja azt. Vagy "manuális tesztek" Futtatás (ping-vizsgálatok) egyetlen URL-címet, vagy futtatható egy [többlépéses webteszt teljesítmény](app-insights-monitor-web-app-availability.md#multi-step-web-tests) töltse fel (a hello azonos módon, mint a rendelkezésre állási tesztelése).

> [!TIP]
> Hello teljesítményteszt létrehozása után nyissa meg a hello tesztelése, és élő adatfolyam panel külön Windows hello. Láthatja, mikor hello várólistára teljesítmény teszt elindul, és tekintse meg élő adatfolyam: hello ugyanannyi időt vesz igénybe.
>


## <a name="troubleshooting"></a>Hibaelhárítás

Nincs adat? Ha az alkalmazás egy védett hálózati: metrikák élő adatfolyam használ a különböző IP-címeket, mint más Application Insights telemetria. Győződjön meg arról, hogy [adott IP-címek](app-insights-ip-addresses.md) nyitva a tűzfalon.



## <a name="next-steps"></a>Következő lépések
* [Az Application insights szolgáltatással megfigyelési kihasználtsága](app-insights-web-track-usage.md)
* [Diagnosztikai keresés](app-insights-diagnostic-search.md)
* [Profilkészítő](app-insights-profiler.md)
* [Pillanatkép hibakereső](app-insights-snapshot-debugger.md)

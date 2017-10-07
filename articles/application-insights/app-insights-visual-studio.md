---
title: "az Azure Application insights szolgáltatással, a Visual Studio aaaDebug alkalmazások |} Microsoft Docs"
description: "Webalkalmazások teljesítményelemzése és diagnosztikája hibakeresés közben és éles környezetben."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/7/2017
ms.author: bwren
ms.openlocfilehash: 20491fbe4505bf719039e5d1c220b1afec01db25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-applications-with-azure-application-insights-in-visual-studio"></a>Az alkalmazások az Azure Application insights szolgáltatással, a Visual Studio hibakeresési
A Visual Studio 2015-ös és újabb verzióiban elemezheti az ASP.NET-webalkalmazások teljesítményét és diagnosztizálhatja a problémákat hibakeresés közben és éles környezetben is az [Azure Application Insights](app-insights-overview.md) telemetriájával.

Ha az ASP.NET-webalkalmazás Visual Studio 2017 használatával létrehozott, vagy később, hello Application Insights SDK már rendelkezik. Egyéb módon, hogy még nem tette meg, ha [Application Insights tooyour alkalmazás hozzáadása](app-insights-asp-net.md).

toomonitor az alkalmazás éles van, amikor általában megjelenített hello Application Insights telemetria hello [Azure-portálon](https://portal.azure.com), ahol riasztások beállítását, és erőteljes figyelőeszközökből alkalmazni. De hibakeresési információ is kereshet és elemezheti a Visual Studio hello telemetriai. Visual Studio tooanalyze telemetriai használhatja a munkakörnyezeti helyet a pedig a hibakeresés fut a fejlesztési számítógépén. Hello utóbbi esetben elemezheti hibakeresési fut, akkor is, ha még hello SDK toosend telemetriai toohello Azure-portálon még nincs konfigurálva. 

## <a name="run"></a> Hibakeresés a projektben
Futtassa a webalkalmazást helyi hibakeresési módban az F5 billentyűvel. Nyissa meg a különböző oldalakhoz toogenerate néhány telemetriai adatokat.

A Visual Studióban tekintse meg a projekt hello Application Insights modul által naplózott eseményeket hello számát.

![A Visual Studio hello Application Insights gombon hibakeresés során.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Kattintson a gomb toosearch a telemetriai adatokat. 

## <a name="application-insights-search"></a>Application Insights-keresés
hello Application Insights keresési ablak naplózott eseményeket jeleníti meg. (Ha az Application Insights beállításakor bejelentkezett tooAzure, kereshet hello Azure-portálon azonos események hello.)

![Kattintson a jobb gombbal a projekt hello, és válassza ki az Application Insights keresése](./media/app-insights-visual-studio/34.png)

> [!NOTE] 
> Válassza ki, vagy kapcsolja ki a szűrők után gombra hello keresési hello keresési szövegmező hello végén.
>

hello szabad szöveges keresés hello események mezőinek működik. Például keressen rá az oldal; hello URL-cím része például az ügyfél város; tulajdonság értéke hello vagy vagy a nyomkövetési napló szavakat.

Kattintson a bármely esemény toosee részletes tulajdonságát.

Kérelmek tooyour web app alkalmazásban kattintva toohello kód.

![A kérelem részleteinek kattintson toohello kód](./media/app-insights-visual-studio/31.png)

Kapcsolódó elemek is megnyithatja toohelp diagnosztizálhatja a sikertelen kérelmek vagy kivételeket.

![A kérelem részleteinek görgessen lefelé toorelated elemek](./media/app-insights-visual-studio/41.png)

## <a name="view-exceptions-and-failed-requests"></a>Kivételek megtekintése és a sikertelen kérelmek
Jelentések megjelenítése kivétel hello keresési ablak. (Egyes régebbi típusú ASP.NET-alkalmazást, hogy túl[kivétel figyelés beállítása](app-insights-asp-net-exceptions.md) hello keretrendszer által kezelt kivételek toosee.)

Kattintson egy kivétel tooget hívásláncnak megfelelően. Ha hello kód hello alkalmazás meg nyitva, a Visual Studio, kattinthat keresztül a hello verem nyomkövetési toohello vonatkozó kódsort hello.

![Kivétel híváslánca](./media/app-insights-visual-studio/17.png)

## <a name="view-request-and-exception-summaries-in-hello-code"></a>Hello kódban kérelem és a kivétel összesítéseket
A kód fókuszban sorban fölött mindegyik eseménykezelő metódus hello lásd: hello kérelmek és az Application Insights hello elmúlt 24 órás naplózásáról kivételek számát.

![Kivétel híváslánca](./media/app-insights-visual-studio/21.png)

> [!NOTE] 
> Kód fókuszban adatokat jelenít meg az Application Insights csak ha [konfigurálva az alkalmazás toosend telemetriai toohello Application Insights portál](app-insights-asp-net.md).
>

[További információk az Application Insights-telemetriáról a Code Lensben](app-insights-visual-studio-codelens.md)

## <a name="trends"></a>Trendek
A Trendek használatával megjelenítheti az alkalmazás időbeni működésének a módját. 

Válasszon **felfedezés Telemetriai trendek** hello az Application Insights eszköztárgomb vagy az Application Insights keresési ablak. Válasszon egyet a öt közös lekérdezések tooget elindult. A különböző adatkészleteket telemetriatípusok, időintervallumok és egyéb tulajdonságok szerint elemezheti. 

az adatok toofind rendellenességeinek válasszon egyet az hello anomáliadetektálási beállítások hello "Nézet típusa" legördülő lista alatt. hello szűrési beállítások hello ablak hello alján legyen a könnyen toohone a meghatározott fájlcsoportokat a telemetriai adatot.

![Trendek](./media/app-insights-visual-studio/51.png)

[További információ a Trendekről](app-insights-visual-studio-trends.md).

## <a name="local-monitoring"></a>Helyi figyelés
(A Visual Studio 2015 Update 2) Ha még nincs konfigurálva hello SDK toosend telemetriai toohello Application Insights portál (úgy, hogy nincs instrumentation kulcs applicationinsights.config) hello diagnosztika ablak a legújabb hibakeresési munkamenetben telemetriai adatokat jeleníti meg. 

Ez akkor javasolt, ha már közzétette az alkalmazás korábbi verzióit. Nem szeretné, hogy a hibakeresési munkamenetek toobe a hello telemetriai keverve hello hello telemetriai adatokat az Application Insights portál hello közzétett alkalmazás.

Ez akkor is hasznos, ha, hogy bizonyos [egyéni telemetria](app-insights-api-custom-events-metrics.md) megjeleníteni kívánt toodebug telemetriai toohello portal elküldése előtt.

* *Először az Application Insights toosend telemetriai toohello portal teljesen beállítása. De most csak a Visual Studio toosee hello telemetriai adatokat.*
  
  * Hello keresési ablak beállításaiban van egy beállítás toosearch helyi diagnosztika akkor is, ha az alkalmazás küld telemetriai toohello portal.
  * küldési toohello portal, hello sor megjegyzésbe toostop telemetriai `<instrumentationkey>...` az ApplicationInsights.config. Ha készen áll a toosend telemetriai toohello portal újra már kódjánál levő megjegyzésjeleket.


## <a name="next-steps"></a>Következő lépések
|  |  |
| --- | --- |
| **[További adatok hozzáadása](app-insights-asp-net-more.md)**<br/>Figyelheti a használatot, az elérhetőséget, a függőségeket és a kivételeket. Integrálhatja a nyomkövetéseket naplózási keretrendszerekből. Egyéni telemetriát írhat. |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| **[Hello Application Insights portál használata](app-insights-dashboards.md)**<br/>Irányítópultok, hatékony elemzési és diagnosztikai eszközöket, riasztások, egy élő kapcsolatfüggőségi térképének megjelenítéséhez az alkalmazás, és exportált telemetriai adatok megtekintéséhez. |![Visual Studio](./media/app-insights-visual-studio/62.png) |


---
title: az Application Insights az ASP.NET Core aaaAzure |} Microsoft Docs
description: "Webalkalmazások rendelkezésre állását, teljesítményét és használatának a figyelheti."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: a7a27f9eef1daec5b0deae9fd88906e646980659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-aspnet-core"></a>ASP.Net Core-hoz készült Application Insights
[Az Application Insights](app-insights-overview.md) lehetővé teszi, hogy a webalkalmazás rendelkezésre állását, teljesítményét és használatának figyelése. Hello visszajelzést kap hello teljesítmény és az alkalmazást a hello hatékonyságát helyettesítő minden fejlesztési életciklus során hello tervezési hello irányának kapcsolatos megalapozott döntések tehet.

![Példa](./media/app-insights-asp-net-core/sample.png)

Szüksége lesz egy előfizetés [Microsoft Azure](http://azure.com). Jelentkezzen be egy Microsoft-fiókkal – ez tartozhat a Windowshoz, az XBox Live-hoz vagy egyéb Microsoft felhőszolgáltatásokhoz. Előfordulhat, hogy a csoport egy szervezeti előfizetés tooAzure: kérje meg a hello tulajdonos tooadd tooit, a Microsoft-fiókjával.

## <a name="getting-started"></a>Bevezetés

* A Visual Studio Solution Explorerben kattintson jobb gombbal a projektre, és válassza ki **konfigurálja az Application Insights**, vagy **Hozzáadás > Application Insights**. [További információk](app-insights-asp-net.md).
* Ha nem látja ezeket menüparancsai, hajtsa végre a hello [manuális első lépések útmutató](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started). Ha akkor esetleg toodo Ez a projekt egy Visual Studio verziója előtt 2017 hozták létre.

## <a name="using-application-insights"></a>Az Application Insights használata
Jelentkezzen be a hello [Microsoft Azure-portálon](https://portal.azure.com), jelölje be **összes erőforrás** vagy **Application Insights**, majd válassza ki az alkalmazáshoz létrehozott toomonitor hello erőforrás.

Egy külön böngészőablakban használja az alkalmazást egy ideig. Látni fogja, hello Application Insights diagramok szereplő adatok. (Lehetséges, hogy frissítési tooclick.) Nem lesznek csak kisebb mennyiségű adatot közben kidolgozása, de a diagramokat valóban származnak életben tegye közzé az alkalmazást, és a sok felhasználóval rendelkezik. 

hello – Áttekintés lapon látható legfontosabb teljesítményi diagramok: kiszolgáló válaszideje, lapbetöltési idő és a sikertelen kérelmek száma. Kattintson a diagram toosee további diagramokat és adatokat.

Nézetek hello portálon három fő kategóriába sorolhatók:

* [Metrikaböngésző](app-insights-metrics-explorer.md) mutatja, diagramok és a metrikák és számát, például a válaszidőt, hiba díjszabás vagy metrikák Ön hozza létre a hello [API](app-insights-api-custom-events-metrics.md). Szűrő és a szegmens hello adatait az alkalmazás és a felhasználóknak szeretné jobban megismerni a tulajdonság értékek tooget.
* [Keresés Explorer](app-insights-diagnostic-search.md) felsorolja az egyes események, például bizonyos kérésekre, kivételek, naplókivonatokat vagy saját kezűleg hello segítségével létrehozott események [API](app-insights-api-custom-events-metrics.md). Szűrje és hello események kereséséhez, és keresse meg a kapcsolódó események tooinvestigate problémák között.
* [Elemzés](app-insights-analytics.md) lehetővé teszi, hogy az SQL-szerű lekérdezéseket futtatnia a telemetriai adatokat, és a hatékony analitikai és diagnosztikai eszköz.

## <a name="alerts"></a>Riasztások
* Automatikusan elérhetővé [proaktív diagnosztikai riasztások](app-insights-proactive-diagnostics.md) , amely közli, hiba sebességét és más metrikákkal rendellenes változásaira vonatkozó.
* Állítson be [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md) tootest a webhely folyamatosan világszerte helyeket, és az e-mailek, ahogy bármely tesztelése sikertelen.
* Állítson be [metrika riasztások](app-insights-monitor-web-app-availability.md) tooknow, ha például a válaszidők vagy kivétel díjszabás metrikák külső elfogadható határértékeket.

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a>Nyílt forráskód
[Olvassa el és toohello kód közreműködési lehetőségek](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a>Következő lépések
* [Adja hozzá a telemetriai adatok tooyour weblapok](app-insights-javascript.md) toomonitor lapon használati és teljesítményadatokat.
* [Függőségek figyelése](app-insights-asp-net-dependencies.md) toosee, ha a többi, SQL vagy más külső erőforrások lassítja meg.
* [Hello API-t használó](app-insights-api-custom-events-metrics.md) toosend saját eseményeket és metrikákat az alkalmazás teljesítményét és használatának részletes nézet.
* [Rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md) ellenőrizze az alkalmazását folyamatosan hello world körül. 


---
title: "aaaAzure Application Insights for Windows server és a feldolgozói szerepkörök |} Microsoft Docs"
description: "Adja hozzá manuálisan a hello Application Insights SDK tooyour ASP.NET alkalmazások tooanalyze használatát, rendelkezésre állását és teljesítményét."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 64643ef637195d10f87fc6020a77169bca66c1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manually-configure-application-insights-for-net-applications"></a>Az Application Insights manuális beállítása a .NET-alkalmazásokhoz

Konfigurálható [Application Insights](app-insights-overview.md) toomonitor számos alkalmazások vagy alkalmazás-szerepkörök, összetevők és mikroszolgáltatások létrehozására. A webalkalmazásokhoz és -szolgáltatásokhoz a Visual Studio [egylépéses konfigurációs lehetőséget ](app-insights-asp-net.md) biztosít. Más típusú .NET-alkalmazásokhoz, például a háttérkiszolgálói szerepkörökhöz vagy az asztali alkalmazásokhoz beállíthatja manuálisan az Application Insightsot.

![Példa teljesítményfigyelő diagramokra](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a>Előkészületek

A következők szükségesek:

* Előfizetés túl[Microsoft Azure](http://azure.com). Ha a csapat vagy szervezet Azure-előfizetéssel, hello tulajdonosa adhat hozzá, tooit, használja a [Microsoft-fiók](http://live.com).
* Visual Studio 2013 vagy újabb.

## <a name="add"></a>1. Application Insights-erőforrások választása

hello "resource", ahol az adatok gyűjtése és hello Azure-portálon jelenik meg. E szüksége toodecide toocreate egy új, vagy egy meglévő fájlmegosztás.

### <a name="part-of-a-larger-app-use-existing-resource"></a>Egy nagyobb alkalmazás része: létező erőforrás használata

Ha a webes alkalmazás több összetevői – például egy előtér-webalkalmazást és egy vagy több háttér-szolgáltatásaihoz - vannak, akkor a telemetriai adatokat küldjön az összes hello összetevők toohello ugyanazt az erőforrást. Ezzel lehetővé teszi egy önálló alkalmazás térképen toobe, és könnyebben lehetséges tootrace egy összetevő tooanother kérése.

Tehát ha most már figyelés az alkalmazás más összetevői, majd csak használata hello azonos erőforrás.

Nyissa meg a hello erőforrás hello [Azure-portálon](https://portal.azure.com/). 

### <a name="self-contained-app-create-a-new-resource"></a>Önálló alkalmazás: Új erőforrás létrehozása

Ha új alkalmazás hello független tooother alkalmazások, azt a saját erőforrás kell rendelkeznie.

Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/), és hozzon létre egy új Application Insights-erőforrást. Válassza ki az ASP.NET hello alkalmazás típusként.

![Kattintson az Új, majd az Application Insights lehetőségre](./media/app-insights-windows-services/01-new-asp.png)

hello választott alkalmazástípus hello erőforrás paneleken tartalmának hello alapértelmezett beállítása.

## <a name="2-copy-hello-instrumentation-key"></a>2. Hello Instrumentation kulcs másolása
hello kulcs hello erőforrás azonosítja. Lesz telepíti, amint az SDK-val hello rendelés toodirect adatok toohello erőforrás.

![Kattintson a Tulajdonságok parancsra, válassza ki a hello kulcs, használja a ctrl + C](./media/app-insights-windows-services/02-props-asp.png)

## <a name="sdk"></a>3. Az alkalmazás hello Application Insights csomag telepítése
Telepítése és konfigurálása hello Application Insights csomagot dolgozik hello platform függ. 

1. A Visual Studióban kattintson a jobb gombbal a projektjére, és válassza a **Manage NuGet Packages (NuGet-csomagok kezelése)** lehetőséget.
   
    ![Kattintson a jobb gombbal a hello projektet, és válassza ki a Nuget-csomagok kezelése](./media/app-insights-windows-services/03-nuget.png)
2. Telepítse az Application Insights csomagot hello Windows server-alkalmazások esetén "Microsoft.ApplicationInsights.WindowsServer."
   
    ![Az „Application Insights” kifejezés keresése](./media/app-insights-windows-services/04-ai-nuget.png)
   
    *Melyik verzió?*

    Ellenőrizze **közé tartoznak az előzetes** Ha azt szeretné, tootry a legújabb funkciókat. hello dokumentumokat és blogok vegye figyelembe, hogy szükséges-e előzetes verzióját.
    
    *Használhatok más csomagokat is?*
   
    Igen. Ha azt szeretné csak toouse hello API toosend saját telemetriai, válassza a "Microsoft.ApplicationInsights". hello Windows Server csomag hello API mellett egyéb csomagok, például a teljesítményszámlálók gyűjteményét a és a függőségi figyelő egy száma is. 

### <a name="tooupgrade-toofuture-package-versions"></a>tooupgrade toofuture alkalmazáscsomag-verziók
Azt a kiadási hello SDK idő tootime az új verzióját.

tooupgrade tooa [hello csomag új kiadási](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), nyissa meg újra a NuGet-Csomagkezelőt, és a telepített csomagok szűrésére. Jelölje ki a **Microsoft.ApplicationInsights.WindowsServer** lehetőséget, és válassza az **Upgrade** (Frissítés) lehetőséget.

Ha végzett a testreszabások tooApplicationInsights.config, egy példányának mentése, frissítése, és ezt követően a változtatások egyesítése hello új verzió előtt.

## <a name="4-send-telemetry"></a>4. Telemetria küldése
**Ha csak hello API csomag telepítése:**

* Beállíthat hello instrumentation kulcs a kódban, például `main()`: 
  
    `TelemetryConfiguration.Active.InstrumentationKey = "`*az Ön kulcsa*`";` 
* [Saját API-jával hello telemetriai írási](app-insights-api-custom-events-metrics.md#ikey).

**Ha telepítette a többi Application Insights csomagot** tetszés szerint használhatja hello .config fájl tooset hello instrumentation kulcs:

* Szerkessze az ApplicationInsights.config (amely hello NuGet telepítése felvették). Szúrja be a záró címke hello előtt:
  
    `<InstrumentationKey>`*hello instrumentation kulcs másolt*`</InstrumentationKey>`
* Győződjön meg arról, hogy túl van-e beállítva a hello tulajdonságait a Solution Explorer ApplicationInsights.config**Build művelet = tartalom másolása tooOutput Directory másolási =**.

Ha azt szeretné, hogy túl hasznos tooset hello instrumentation kulcs kódban[kapcsoló hello kulcs eltérő konfigurációk](app-insights-separate-resources.md). Hello kulcs kód állítja be, ha nincs tooset legyen hello `.config` fájlt.

## <a name="run"></a> A projekt futtatása
Használjon hello **F5** toorun az alkalmazás, és próbálja ki: különböző nyílt lapok toogenerate néhány telemetriai adatokat.

A Visual Studio látni fogja, az elküldött hello események száma.

![Események száma a Visual Studióban](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <a name="monitor"></a> A telemetriai adatok megtekintése
Térjen vissza a toohello [Azure-portálon](https://portal.azure.com/) , és keresse meg a tooyour Application Insights-erőforrást.

Keresse meg hello áttekintő diagramok adatokat. Először csak egy vagy két pontot lát. Példa:

![Kattintson a toomore adatok](./media/app-insights-windows-services/12-first-perf.png)

Kattintson a diagram toosee keresztül metrikák részletes. [További információk a metrikákról.](app-insights-web-monitor-performance.md)

### <a name="no-data"></a>Nincs adat?
* Hello alkalmazást, a különböző oldalakhoz megnyitása, hogy néhány telemetriai generál használni.
* Nyissa meg hello [keresési](app-insights-diagnostic-search.md) csempe, toosee események. Egyes esetekben szükséges események közben hosszabb egy kis tooget hello metrikák-feldolgozási folyamaton keresztül.
* Várjon néhány másodpercet, és kattintson a **Frissítés** lehetőségre. Diagramok rendszeresen frissítse magát, de is frissítheti manuálisan Ha eredménykészletre várakozik egyes adatok tooshow.
* Lásd: [Hibaelhárítás](app-insights-troubleshoot-faq.md).

## <a name="publish-your-app"></a>Az alkalmazás közzététele
Most már telepítheti az alkalmazáskiszolgáló tooyour, vagy tooAzure és figyelési hello adat gyűlik össze.

![Visual Studio toopublish az alkalmazás használata](./media/app-insights-windows-services/15-publish.png)

Ha hibakeresési módban futtatja, telemetriai végezhető hello-feldolgozási folyamaton keresztül, hogy másodpercen belül szereplő adatokat kell megjelennie. Ha Kiadás konfigurációban telepíti az alkalmazását, az adatok lassabban gyűlnek.

### <a name="no-data-after-you-publish-tooyour-server"></a>Nincs adat tooyour server közzététele után?
Nyissa meg a portokat a kimenő forgalom számára a kiszolgáló tűzfalán. Lásd: [ezen a lapon](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) szükséges címek hello listája 

### <a name="trouble-on-your-build-server"></a>Probléma adódott a lemezképfájl-kiszolgálóján?
Tekintse meg [ezt a Hibaelhárítási cikket](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [!NOTE]
> Az alkalmazás nagy mennyiségű telemetriai adatokat állít elő, ha hello adaptív mintavételi modul automatikusan toohello portal reprezentatív része események küldése által küldött hello kötet csökkenti. Azonban események, amelyek kapcsolódó toohello kérésben lesz kiválasztva vagy nincs kijelölve csoportosan, hogy a kapcsolódó események közti léphet. 
> [Ismerkedés a mintavételezéssel](app-insights-sampling.md).
> 
> 

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Következő lépések
* [Adja hozzá a további telemetriai](app-insights-asp-net-more.md) tooget hello 360 fok nézet az alkalmazás.


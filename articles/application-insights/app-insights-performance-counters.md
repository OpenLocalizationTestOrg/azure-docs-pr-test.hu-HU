---
title: "az Application Insightsban aaaPerformance számlálók |} Microsoft Docs"
description: "A figyelő rendszer és az Application Insights egyéni .NET teljesítményszámlálók."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: bwren
ms.openlocfilehash: 0a51c225f1d1124c9e7fe89f34e747cb26a3589e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="system-performance-counters-in-application-insights"></a>Az Application Insightsban rendszerteljesítmény-számlálók
A Windows számos biztosít [teljesítményszámlálók](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) például a Processzor Foglaltság, memória, lemez és hálózat használatának. Is definiálhat a saját. [Az Application Insights](app-insights-overview.md) megjelenítheti a teljesítményszámlálók, ha az alkalmazás fut az IIS egy helyszíni gazdagép vagy virtuális gépek toowhich, rendszergazdai hozzáféréssel rendelkeznek. hello diagramok hello erőforrások elérhető tooyour élő alkalmazás megadása, és segít a tooidentify kiszolgálópéldányok közötti egyenetlen terhelés.

Teljesítményszámlálók hello kiszolgálók panel, amelyen a szegmensek egy táblázatot tartalmaz kiszolgálópéldány jelennek meg.

![Az Application Insightsban jelentett teljesítményszámlálók](./media/app-insights-performance-counters/counters-by-server-instance.png)

(A teljesítményszámlálók nem érhetők el az Azure Web Apps. Azonban úgy is [küldése az Azure Diagnostics tooApplication Insights](app-insights-azure-diagnostics.md).)

## <a name="view-counters"></a>Nézet számlálók
hello kiszolgálók paneljét teljesítményszámlálók alapértelmezett készletét jeleníti meg. 

toosee más számlálók hello kiszolgálók panel hello diagramokat, vagy nyisson meg egy új [Metrikaböngésző](app-insights-metrics-explorer.md) panelt, és adja hozzá az új diagramokat. 

Amikor a diagram szerkesztése hello elérhető számlálók metrikák szerepelnek.

![Az Application Insightsban jelentett teljesítményszámlálók](./media/app-insights-performance-counters/choose-performance-counters.png)

toosee a leghasznosabb diagramokat az egyik helyen, hozzon létre egy [irányítópult](app-insights-dashboards.md) és tooit rögzítheti őket.

## <a name="add-counters"></a>Számlálók hozzáadása
Ha szeretné hello teljesítményszámláló mérőszámokat, mert a webkiszolgáló az Application Insights SDK hello nem gyűjt hello listája nem látható. Beállíthatja, toodo stb.

1. Megtudhatja, milyen számlálók érhetők el a kiszolgálón hello kiszolgálón a PowerShell-parancs segítségével:
   
    `Get-Counter -ListSet *`
   
    (Lásd: [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Nyissa meg az ApplicationInsights.config.
   
   * Az Application Insights tooyour alkalmazást felvette a fejlesztés során, ha a projekt ApplicationInsights.config szerkesztése, és hozza létre a azt tooyour kiszolgálók.
   * Ha futásidőben használt állapotfigyelő tooinstrument egy webalkalmazást, ApplicationInsights.config található hello gyökérkönyvtárában hello alkalmazást az IIS-ben. Frissíteni nincs összes server-példány.
3. Hello teljesítmény adatgyűjtő irányelv szerkesztése:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

Rögzítheti a szabványos számlálók, mind azok saját kezűleg megvalósítását. `\Objects\Processes`Példa egy szabványos számláló az összes Windows rendszereken érhető el. `\Sales(photo)\# Items Sold`Íme egy egyéni számláló, előfordulhat, hogy egy webszolgáltatás-bővítmény kell végrehajtani. 

hello formátuma `\Category(instance)\Counter"`, vagy nem rendelkezik a példányok kategóriákat, csak `\Category\Counter`.

`ReportAs`a számláló neve, amely nem egyezik a szükséges `[a-zA-Z()/-_ \.]+` -Ez azt jelenti, hogy tartalmazzák a hello beállítása a következő karaktereket: betűkből kerek zárójeleket, törtvonallal, kötőjelet, aláhúzásjelet, terület, pont.

Ha megad egy példányát, akkor gyűjtenek "CounterInstanceName" hello dimenzió jelentett metrikát.

### <a name="collecting-performance-counters-in-code"></a>A kódban teljesítményszámlálók gyűjtése
toocollect rendszerteljesítmény teljesítményszámlálók, és küldje el tooApplication Insights, az alábbi hello részlet módosíthatja:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

Vagy annak hello ugyanaz a létrehozott egyéni metrikák:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a>Az elemzés teljesítményszámlálók
Kereshet és a teljesítmény számláló jelentések megjelenítéséhez [Analytics](app-insights-analytics.md).

Hello **performanceCounters** séma közzététele hello `category`, `counter` nevét, és `instance` minden teljesítményszámláló nevét.  Hello telemetriai minden alkalmazáshoz az alkalmazás csak hello számlálók látható. Például toosee milyen számlálók érhetők el: 

![Az Application Insights analytics teljesítményszámlálók](./media/app-insights-performance-counters/analytics-performance-counters.png)

(Itt "Példány" hivatkozik a teljesítményszámláló-példány toohello, nem hello szerepkör- vagy virtuálisgép-példányt. hello teljesítményszámlálójának példánynevét általában szegmensek például a processzor kihasználtsága számlálók hello folyamat vagy az alkalmazás hello név szerint.)

a diagram a rendelkezésre álló memória hello legutóbbi időszak alatt tooget: 

![Az Application Insights Analytics memória timechart](./media/app-insights-performance-counters/analytics-available-memory.png)

Egyéb telemetriai adatokat, például **performanceCounters** egy olyan oszlop is van `cloud_RoleInstance` azt jelzi, hogy hello hello futtató kiszolgálópéldány, amelyen fut az alkalmazás identitását. Például toocompare hello az alkalmazás teljesítményével kapcsolatos különböző gépeken hello: 

![Az Application Insights Analytics szerepkör példánya szegmentált teljesítmény](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>Az ASP.NET és az Application Insights száma
*Mi az a hello kivétel arányról és kivételek hello különbségének?*

* *Kivétel arány* rendszer teljesítményszámláló van. hello CLR álló összes hello kezelt és kezeletlen kivételek fellépése, amelyek fel vannak, és elosztja a mintavételi időközben a hello összesen hello időköz hello hosszát. Application Insights SDK hello ennek gyűjti, és elküldi azt toohello portál.
* *Kivételek* hello számát hello mintavételi időszakban hello diagram hello portál által fogadott TrackException jelentések van. Ez magában foglalja, csak hello kezelt kivételek ahol TrackException hívja be a kódját, és nem tartalmazza az összes írt [nem kezelt kivételek](app-insights-asp-net-exceptions.md). 

## <a name="alerts"></a>Riasztások
Például a más metrikákkal is [riasztás beállításához](app-insights-alerts.md) toowarn, ha megfelelően teljesítményszámláló kívül korlátozni, adjon meg. Hello riasztások panel megnyitásához, majd kattintson a riasztás hozzáadása.

## <a name="next"></a>Következő lépések
* [A függőségi nyomon követése](app-insights-asp-net-dependencies.md)
* [Kivétel követése](app-insights-asp-net-exceptions.md)


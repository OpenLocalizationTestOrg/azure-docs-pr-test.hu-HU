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
# <a name="system-performance-counters-in-application-insights"></a><span data-ttu-id="b3f3e-103">Az Application Insightsban rendszerteljesítmény-számlálók</span><span class="sxs-lookup"><span data-stu-id="b3f3e-103">System performance counters in Application Insights</span></span>
<span data-ttu-id="b3f3e-104">A Windows számos biztosít [teljesítményszámlálók](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) például a Processzor Foglaltság, memória, lemez és hálózat használatának.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-104">Windows provides a wide variety of [performance counters](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) such as CPU occupancy, memory, disk, and network usage.</span></span> <span data-ttu-id="b3f3e-105">Is definiálhat a saját.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-105">You can also define your own.</span></span> <span data-ttu-id="b3f3e-106">[Az Application Insights](app-insights-overview.md) megjelenítheti a teljesítményszámlálók, ha az alkalmazás fut az IIS egy helyszíni gazdagép vagy virtuális gépek toowhich, rendszergazdai hozzáféréssel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-106">[Application Insights](app-insights-overview.md) can show these performance counters if your application is running under IIS on an on-premises host or virtual machine toowhich you have administrative access.</span></span> <span data-ttu-id="b3f3e-107">hello diagramok hello erőforrások elérhető tooyour élő alkalmazás megadása, és segít a tooidentify kiszolgálópéldányok közötti egyenetlen terhelés.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-107">hello charts indicate hello resources available tooyour live application, and can help tooidentify unbalanced load between server instances.</span></span>

<span data-ttu-id="b3f3e-108">Teljesítményszámlálók hello kiszolgálók panel, amelyen a szegmensek egy táblázatot tartalmaz kiszolgálópéldány jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-108">Performance counters appear in hello Servers blade, which includes a table that segments by server instance.</span></span>

![Az Application Insightsban jelentett teljesítményszámlálók](./media/app-insights-performance-counters/counters-by-server-instance.png)

<span data-ttu-id="b3f3e-110">(A teljesítményszámlálók nem érhetők el az Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-110">(Performance counters aren't available for Azure Web Apps.</span></span> <span data-ttu-id="b3f3e-111">Azonban úgy is [küldése az Azure Diagnostics tooApplication Insights](app-insights-azure-diagnostics.md).)</span><span class="sxs-lookup"><span data-stu-id="b3f3e-111">But you can [send Azure Diagnostics tooApplication Insights](app-insights-azure-diagnostics.md).)</span></span>

## <a name="view-counters"></a><span data-ttu-id="b3f3e-112">Nézet számlálók</span><span class="sxs-lookup"><span data-stu-id="b3f3e-112">View counters</span></span>
<span data-ttu-id="b3f3e-113">hello kiszolgálók paneljét teljesítményszámlálók alapértelmezett készletét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-113">hello Servers blade shows a default set of performance counters.</span></span> 

<span data-ttu-id="b3f3e-114">toosee más számlálók hello kiszolgálók panel hello diagramokat, vagy nyisson meg egy új [Metrikaböngésző](app-insights-metrics-explorer.md) panelt, és adja hozzá az új diagramokat.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-114">toosee other counters, either edit hello charts on hello Servers blade, or open a new [Metrics Explorer](app-insights-metrics-explorer.md) blade and add new charts.</span></span> 

<span data-ttu-id="b3f3e-115">Amikor a diagram szerkesztése hello elérhető számlálók metrikák szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-115">hello available counters are listed as metrics when you edit a chart.</span></span>

![Az Application Insightsban jelentett teljesítményszámlálók](./media/app-insights-performance-counters/choose-performance-counters.png)

<span data-ttu-id="b3f3e-117">toosee a leghasznosabb diagramokat az egyik helyen, hozzon létre egy [irányítópult](app-insights-dashboards.md) és tooit rögzítheti őket.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-117">toosee all your most useful charts in one place, create a [dashboard](app-insights-dashboards.md) and pin them tooit.</span></span>

## <a name="add-counters"></a><span data-ttu-id="b3f3e-118">Számlálók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b3f3e-118">Add counters</span></span>
<span data-ttu-id="b3f3e-119">Ha szeretné hello teljesítményszámláló mérőszámokat, mert a webkiszolgáló az Application Insights SDK hello nem gyűjt hello listája nem látható.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-119">If hello performance counter you want isn't shown in hello list of metrics, that's because hello Application Insights SDK isn't collecting it in your web server.</span></span> <span data-ttu-id="b3f3e-120">Beállíthatja, toodo stb.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-120">You can configure it toodo so.</span></span>

1. <span data-ttu-id="b3f3e-121">Megtudhatja, milyen számlálók érhetők el a kiszolgálón hello kiszolgálón a PowerShell-parancs segítségével:</span><span class="sxs-lookup"><span data-stu-id="b3f3e-121">Find out what counters are available in your server by using this PowerShell command at hello server:</span></span>
   
    `Get-Counter -ListSet *`
   
    <span data-ttu-id="b3f3e-122">(Lásd: [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)</span><span class="sxs-lookup"><span data-stu-id="b3f3e-122">(See [`Get-Counter`](https://technet.microsoft.com/library/hh849685.aspx).)</span></span>
2. <span data-ttu-id="b3f3e-123">Nyissa meg az ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-123">Open ApplicationInsights.config.</span></span>
   
   * <span data-ttu-id="b3f3e-124">Az Application Insights tooyour alkalmazást felvette a fejlesztés során, ha a projekt ApplicationInsights.config szerkesztése, és hozza létre a azt tooyour kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-124">If you added Application Insights tooyour app during development, edit ApplicationInsights.config in your project, and then re-deploy it tooyour servers.</span></span>
   * <span data-ttu-id="b3f3e-125">Ha futásidőben használt állapotfigyelő tooinstrument egy webalkalmazást, ApplicationInsights.config található hello gyökérkönyvtárában hello alkalmazást az IIS-ben.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-125">If you used Status Monitor tooinstrument a web app at runtime, find ApplicationInsights.config in hello root directory of hello app in IIS.</span></span> <span data-ttu-id="b3f3e-126">Frissíteni nincs összes server-példány.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-126">Update it there in each server instance.</span></span>
3. <span data-ttu-id="b3f3e-127">Hello teljesítmény adatgyűjtő irányelv szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="b3f3e-127">Edit hello performance collector directive:</span></span>
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

<span data-ttu-id="b3f3e-128">Rögzítheti a szabványos számlálók, mind azok saját kezűleg megvalósítását.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-128">You can capture both standard counters and those you have implemented yourself.</span></span> <span data-ttu-id="b3f3e-129">`\Objects\Processes`Példa egy szabványos számláló az összes Windows rendszereken érhető el.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-129">`\Objects\Processes` is an example of a standard counter, available on all Windows systems.</span></span> <span data-ttu-id="b3f3e-130">`\Sales(photo)\# Items Sold`Íme egy egyéni számláló, előfordulhat, hogy egy webszolgáltatás-bővítmény kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-130">`\Sales(photo)\# Items Sold` is an example of a custom counter that might be implemented in a web service.</span></span> 

<span data-ttu-id="b3f3e-131">hello formátuma `\Category(instance)\Counter"`, vagy nem rendelkezik a példányok kategóriákat, csak `\Category\Counter`.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-131">hello format is `\Category(instance)\Counter"`, or for categories that don't have instances, just `\Category\Counter`.</span></span>

<span data-ttu-id="b3f3e-132">`ReportAs`a számláló neve, amely nem egyezik a szükséges `[a-zA-Z()/-_ \.]+` -Ez azt jelenti, hogy tartalmazzák a hello beállítása a következő karaktereket: betűkből kerek zárójeleket, törtvonallal, kötőjelet, aláhúzásjelet, terület, pont.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-132">`ReportAs` is required for counter names that do not match `[a-zA-Z()/-_ \.]+` - that is, they contain characters that are not in hello following sets: letters, round brackets, forward slash, hyphen, underscore, space, dot.</span></span>

<span data-ttu-id="b3f3e-133">Ha megad egy példányát, akkor gyűjtenek "CounterInstanceName" hello dimenzió jelentett metrikát.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-133">If you specify an instance, it will be collected as a dimension "CounterInstanceName" of hello reported metric.</span></span>

### <a name="collecting-performance-counters-in-code"></a><span data-ttu-id="b3f3e-134">A kódban teljesítményszámlálók gyűjtése</span><span class="sxs-lookup"><span data-stu-id="b3f3e-134">Collecting performance counters in code</span></span>
<span data-ttu-id="b3f3e-135">toocollect rendszerteljesítmény teljesítményszámlálók, és küldje el tooApplication Insights, az alábbi hello részlet módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="b3f3e-135">toocollect system performance counters and send them tooApplication Insights, you can adapt hello snippet below:</span></span>


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

<span data-ttu-id="b3f3e-136">Vagy annak hello ugyanaz a létrehozott egyéni metrikák:</span><span class="sxs-lookup"><span data-stu-id="b3f3e-136">Or you can do hello same thing with custom metrics you created:</span></span>

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a><span data-ttu-id="b3f3e-137">Az elemzés teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="b3f3e-137">Performance counters in Analytics</span></span>
<span data-ttu-id="b3f3e-138">Kereshet és a teljesítmény számláló jelentések megjelenítéséhez [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="b3f3e-138">You can search and display performance counter reports in [Analytics](app-insights-analytics.md).</span></span>

<span data-ttu-id="b3f3e-139">Hello **performanceCounters** séma közzététele hello `category`, `counter` nevét, és `instance` minden teljesítményszámláló nevét.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-139">hello **performanceCounters** schema exposes hello `category`, `counter` name, and `instance` name of each performance counter.</span></span>  <span data-ttu-id="b3f3e-140">Hello telemetriai minden alkalmazáshoz az alkalmazás csak hello számlálók látható.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-140">In hello telemetry for each application, you’ll see only hello counters for that application.</span></span> <span data-ttu-id="b3f3e-141">Például toosee milyen számlálók érhetők el:</span><span class="sxs-lookup"><span data-stu-id="b3f3e-141">For example, toosee what counters are available:</span></span> 

![Az Application Insights analytics teljesítményszámlálók](./media/app-insights-performance-counters/analytics-performance-counters.png)

<span data-ttu-id="b3f3e-143">(Itt "Példány" hivatkozik a teljesítményszámláló-példány toohello, nem hello szerepkör- vagy virtuálisgép-példányt.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-143">('Instance' here refers toohello performance counter instance,  not hello role or server machine instance.</span></span> <span data-ttu-id="b3f3e-144">hello teljesítményszámlálójának példánynevét általában szegmensek például a processzor kihasználtsága számlálók hello folyamat vagy az alkalmazás hello név szerint.)</span><span class="sxs-lookup"><span data-stu-id="b3f3e-144">hello performance counter instance name typically segments counters such as processor time by hello name of hello process or application.)</span></span>

<span data-ttu-id="b3f3e-145">a diagram a rendelkezésre álló memória hello legutóbbi időszak alatt tooget:</span><span class="sxs-lookup"><span data-stu-id="b3f3e-145">tooget a chart of available memory over hello recent period:</span></span> 

![Az Application Insights Analytics memória timechart](./media/app-insights-performance-counters/analytics-available-memory.png)

<span data-ttu-id="b3f3e-147">Egyéb telemetriai adatokat, például **performanceCounters** egy olyan oszlop is van `cloud_RoleInstance` azt jelzi, hogy hello hello futtató kiszolgálópéldány, amelyen fut az alkalmazás identitását.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-147">Like other telemetry, **performanceCounters** also has a column `cloud_RoleInstance` that indicates hello identity of hello host server instance on which your app is running.</span></span> <span data-ttu-id="b3f3e-148">Például toocompare hello az alkalmazás teljesítményével kapcsolatos különböző gépeken hello:</span><span class="sxs-lookup"><span data-stu-id="b3f3e-148">For example, toocompare hello performance of your app on hello different machines:</span></span> 

![Az Application Insights Analytics szerepkör példánya szegmentált teljesítmény](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a><span data-ttu-id="b3f3e-150">Az ASP.NET és az Application Insights száma</span><span class="sxs-lookup"><span data-stu-id="b3f3e-150">ASP.NET and Application Insights counts</span></span>
<span data-ttu-id="b3f3e-151">*Mi az a hello kivétel arányról és kivételek hello különbségének?*</span><span class="sxs-lookup"><span data-stu-id="b3f3e-151">*What's hello difference between hello Exception rate and Exceptions metrics?*</span></span>

* <span data-ttu-id="b3f3e-152">*Kivétel arány* rendszer teljesítményszámláló van.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-152">*Exception rate* is a system performance counter.</span></span> <span data-ttu-id="b3f3e-153">hello CLR álló összes hello kezelt és kezeletlen kivételek fellépése, amelyek fel vannak, és elosztja a mintavételi időközben a hello összesen hello időköz hello hosszát.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-153">hello CLR counts all hello handled and unhandled exceptions that are thrown, and divides hello total in a sampling interval by hello length of hello interval.</span></span> <span data-ttu-id="b3f3e-154">Application Insights SDK hello ennek gyűjti, és elküldi azt toohello portál.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-154">hello Application Insights SDK collects this result and sends it toohello portal.</span></span>
* <span data-ttu-id="b3f3e-155">*Kivételek* hello számát hello mintavételi időszakban hello diagram hello portál által fogadott TrackException jelentések van.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-155">*Exceptions* is a count of hello TrackException reports received by hello portal in hello sampling interval of hello chart.</span></span> <span data-ttu-id="b3f3e-156">Ez magában foglalja, csak hello kezelt kivételek ahol TrackException hívja be a kódját, és nem tartalmazza az összes írt [nem kezelt kivételek](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="b3f3e-156">It includes only hello handled exceptions where you have written TrackException calls in your code, and doesn't include all [unhandled exceptions](app-insights-asp-net-exceptions.md).</span></span> 

## <a name="alerts"></a><span data-ttu-id="b3f3e-157">Riasztások</span><span class="sxs-lookup"><span data-stu-id="b3f3e-157">Alerts</span></span>
<span data-ttu-id="b3f3e-158">Például a más metrikákkal is [riasztás beállításához](app-insights-alerts.md) toowarn, ha megfelelően teljesítményszámláló kívül korlátozni, adjon meg.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-158">Like other metrics, you can [set an alert](app-insights-alerts.md) toowarn you if a performance counter goes outside a limit you specify.</span></span> <span data-ttu-id="b3f3e-159">Hello riasztások panel megnyitásához, majd kattintson a riasztás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="b3f3e-159">Open hello Alerts blade and click Add Alert.</span></span>

## <span data-ttu-id="b3f3e-160"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3f3e-160"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="b3f3e-161">A függőségi nyomon követése</span><span class="sxs-lookup"><span data-stu-id="b3f3e-161">Dependency tracking</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="b3f3e-162">Kivétel követése</span><span class="sxs-lookup"><span data-stu-id="b3f3e-162">Exception tracking</span></span>](app-insights-asp-net-exceptions.md)


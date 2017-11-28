---
title: "Az Application Insights teljesítményszámlálók |} Microsoft Docs"
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
ms.openlocfilehash: 038d6e051be8112b9264e7efa6485965d11e32c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="system-performance-counters-in-application-insights"></a><span data-ttu-id="44550-103">Az Application Insightsban rendszerteljesítmény-számlálók</span><span class="sxs-lookup"><span data-stu-id="44550-103">System performance counters in Application Insights</span></span>
<span data-ttu-id="44550-104">A Windows számos biztosít [teljesítményszámlálók](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) például a Processzor Foglaltság, memória, lemez és hálózat használatának.</span><span class="sxs-lookup"><span data-stu-id="44550-104">Windows provides a wide variety of [performance counters](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) such as CPU occupancy, memory, disk, and network usage.</span></span> <span data-ttu-id="44550-105">Is definiálhat a saját.</span><span class="sxs-lookup"><span data-stu-id="44550-105">You can also define your own.</span></span> <span data-ttu-id="44550-106">[Az Application Insights](app-insights-overview.md) megjelenítheti a teljesítményszámlálók Ha az alkalmazás fut az IIS a helyi gazdagép vagy virtuális gépet, amely rendszergazdai hozzáféréssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="44550-106">[Application Insights](app-insights-overview.md) can show these performance counters if your application is running under IIS on an on-premises host or virtual machine to which you have administrative access.</span></span> <span data-ttu-id="44550-107">A diagramok jelzi az élő alkalmazás számára elérhető erőforrások, és segít a kiszolgálópéldányok közötti egyenetlen terhelés azonosításához.</span><span class="sxs-lookup"><span data-stu-id="44550-107">The charts indicate the resources available to your live application, and can help to identify unbalanced load between server instances.</span></span>

<span data-ttu-id="44550-108">Teljesítményszámlálók jelennek meg a kiszolgálók panel, amelyen a kiszolgálópéldány az adott szegmens egy táblázatot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="44550-108">Performance counters appear in the Servers blade, which includes a table that segments by server instance.</span></span>

![Az Application Insightsban jelentett teljesítményszámlálók](./media/app-insights-performance-counters/counters-by-server-instance.png)

<span data-ttu-id="44550-110">(A teljesítményszámlálók nem érhetők el az Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="44550-110">(Performance counters aren't available for Azure Web Apps.</span></span> <span data-ttu-id="44550-111">Azonban úgy is [Azure Diagnostics küldése az Application Insights](app-insights-azure-diagnostics.md).)</span><span class="sxs-lookup"><span data-stu-id="44550-111">But you can [send Azure Diagnostics to Application Insights](app-insights-azure-diagnostics.md).)</span></span>

## <a name="view-counters"></a><span data-ttu-id="44550-112">Nézet számlálók</span><span class="sxs-lookup"><span data-stu-id="44550-112">View counters</span></span>
<span data-ttu-id="44550-113">A kiszolgálók panel teljesítményszámlálók alapértelmezett készletét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="44550-113">The Servers blade shows a default set of performance counters.</span></span> 

<span data-ttu-id="44550-114">Más számlálók megtekintéséhez vagy a kiszolgálók panel diagramokat, vagy nyisson meg egy új [Metrikaböngésző](app-insights-metrics-explorer.md) panelt, és adja hozzá az új diagramokat.</span><span class="sxs-lookup"><span data-stu-id="44550-114">To see other counters, either edit the charts on the Servers blade, or open a new [Metrics Explorer](app-insights-metrics-explorer.md) blade and add new charts.</span></span> 

<span data-ttu-id="44550-115">Amikor a diagram szerkesztése az elérhető számlálók metrikák szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="44550-115">The available counters are listed as metrics when you edit a chart.</span></span>

![Az Application Insightsban jelentett teljesítményszámlálók](./media/app-insights-performance-counters/choose-performance-counters.png)

<span data-ttu-id="44550-117">Tekintse meg a leghasznosabb diagramokat az egyik helyen, hozzon létre egy [irányítópult](app-insights-dashboards.md) és rögzítheti őket hozzá.</span><span class="sxs-lookup"><span data-stu-id="44550-117">To see all your most useful charts in one place, create a [dashboard](app-insights-dashboards.md) and pin them to it.</span></span>

## <a name="add-counters"></a><span data-ttu-id="44550-118">Számlálók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="44550-118">Add counters</span></span>
<span data-ttu-id="44550-119">Ha a kívánt teljesítményszámláló metrikák listája nem látható, ennek oka a webkiszolgálón az Application Insights SDK nem gyűjt.</span><span class="sxs-lookup"><span data-stu-id="44550-119">If the performance counter you want isn't shown in the list of metrics, that's because the Application Insights SDK isn't collecting it in your web server.</span></span> <span data-ttu-id="44550-120">Erre konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="44550-120">You can configure it to do so.</span></span>

1. <span data-ttu-id="44550-121">Megtudhatja, milyen számlálók érhetők el a kiszolgálón a kiszolgáló a PowerShell-parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="44550-121">Find out what counters are available in your server by using this PowerShell command at the server:</span></span>
   
    `Get-Counter -ListSet *`
   
    <span data-ttu-id="44550-122">(Lásd: [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)</span><span class="sxs-lookup"><span data-stu-id="44550-122">(See [`Get-Counter`](https://technet.microsoft.com/library/hh849685.aspx).)</span></span>
2. <span data-ttu-id="44550-123">Nyissa meg az ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="44550-123">Open ApplicationInsights.config.</span></span>
   
   * <span data-ttu-id="44550-124">Az Application Insights az alkalmazáshoz, a fejlesztés során ApplicationInsights.config szerkesztése a projektben, és majd helyezze újra üzembe, hogy a kiszolgálókhoz.</span><span class="sxs-lookup"><span data-stu-id="44550-124">If you added Application Insights to your app during development, edit ApplicationInsights.config in your project, and then re-deploy it to your servers.</span></span>
   * <span data-ttu-id="44550-125">Ha a állapotfigyelő állíthatnak be futtatás közben egy webalkalmazást, ApplicationInsights.config található az IIS-ben az alkalmazás gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="44550-125">If you used Status Monitor to instrument a web app at runtime, find ApplicationInsights.config in the root directory of the app in IIS.</span></span> <span data-ttu-id="44550-126">Frissíteni nincs összes server-példány.</span><span class="sxs-lookup"><span data-stu-id="44550-126">Update it there in each server instance.</span></span>
3. <span data-ttu-id="44550-127">A teljesítmény adatgyűjtő irányelv szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="44550-127">Edit the performance collector directive:</span></span>
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

<span data-ttu-id="44550-128">Rögzítheti a szabványos számlálók, mind azok saját kezűleg megvalósítását.</span><span class="sxs-lookup"><span data-stu-id="44550-128">You can capture both standard counters and those you have implemented yourself.</span></span> <span data-ttu-id="44550-129">`\Objects\Processes`Példa egy szabványos számláló az összes Windows rendszereken érhető el.</span><span class="sxs-lookup"><span data-stu-id="44550-129">`\Objects\Processes` is an example of a standard counter, available on all Windows systems.</span></span> <span data-ttu-id="44550-130">`\Sales(photo)\# Items Sold`Íme egy egyéni számláló, előfordulhat, hogy egy webszolgáltatás-bővítmény kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="44550-130">`\Sales(photo)\# Items Sold` is an example of a custom counter that might be implemented in a web service.</span></span> 

<span data-ttu-id="44550-131">A formátum `\Category(instance)\Counter"`, vagy nem rendelkezik a példányok kategóriákat, csak `\Category\Counter`.</span><span class="sxs-lookup"><span data-stu-id="44550-131">The format is `\Category(instance)\Counter"`, or for categories that don't have instances, just `\Category\Counter`.</span></span>

<span data-ttu-id="44550-132">`ReportAs`a számláló neve, amely nem egyezik a szükséges `[a-zA-Z()/-_ \.]+` -Ez azt jelenti, hogy azok karakterek, amelyek nem szerepelnek a következő készletek: betűkből kerek zárójeleket, törtvonallal, kötőjelet, aláhúzásjelet, terület, pont.</span><span class="sxs-lookup"><span data-stu-id="44550-132">`ReportAs` is required for counter names that do not match `[a-zA-Z()/-_ \.]+` - that is, they contain characters that are not in the following sets: letters, round brackets, forward slash, hyphen, underscore, space, dot.</span></span>

<span data-ttu-id="44550-133">Ha megad egy példányát, akkor a jelentésben szereplő metrikája "CounterInstanceName" dimenziónak gyűjtenek.</span><span class="sxs-lookup"><span data-stu-id="44550-133">If you specify an instance, it will be collected as a dimension "CounterInstanceName" of the reported metric.</span></span>

### <a name="collecting-performance-counters-in-code"></a><span data-ttu-id="44550-134">A kódban teljesítményszámlálók gyűjtése</span><span class="sxs-lookup"><span data-stu-id="44550-134">Collecting performance counters in code</span></span>
<span data-ttu-id="44550-135">A rendszer a teljesítményszámlálók adatainak összegyűjtése, és küldje el az Application Insights, az alábbi részlet módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="44550-135">To collect system performance counters and send them to Application Insights, you can adapt the snippet below:</span></span>


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

<span data-ttu-id="44550-136">Vagy is elvégezheti a létrehozott egyéni metrikákat az ugyanaz:</span><span class="sxs-lookup"><span data-stu-id="44550-136">Or you can do the same thing with custom metrics you created:</span></span>

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a><span data-ttu-id="44550-137">Az elemzés teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="44550-137">Performance counters in Analytics</span></span>
<span data-ttu-id="44550-138">Kereshet és a teljesítmény számláló jelentések megjelenítéséhez [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="44550-138">You can search and display performance counter reports in [Analytics](app-insights-analytics.md).</span></span>

<span data-ttu-id="44550-139">A **performanceCounters** séma elérhetővé teszi a `category`, `counter` nevét, és `instance` minden teljesítményszámláló nevét.</span><span class="sxs-lookup"><span data-stu-id="44550-139">The **performanceCounters** schema exposes the `category`, `counter` name, and `instance` name of each performance counter.</span></span>  <span data-ttu-id="44550-140">A telemetriai minden alkalmazáshoz csak a számláló az alkalmazás látható.</span><span class="sxs-lookup"><span data-stu-id="44550-140">In the telemetry for each application, you’ll see only the counters for that application.</span></span> <span data-ttu-id="44550-141">Például hogy milyen számlálók érhetők el:</span><span class="sxs-lookup"><span data-stu-id="44550-141">For example, to see what counters are available:</span></span> 

![Az Application Insights analytics teljesítményszámlálók](./media/app-insights-performance-counters/analytics-performance-counters.png)

<span data-ttu-id="44550-143">(Itt "Példány" hivatkozik a teljesítményszámláló-példány, nem a szerepkör- vagy virtuálisgép-példányt.</span><span class="sxs-lookup"><span data-stu-id="44550-143">('Instance' here refers to the performance counter instance,  not the role or server machine instance.</span></span> <span data-ttu-id="44550-144">A teljesítményszámlálójának példánynevét általában szegmensek számlálók például a processzor kihasználtsága a folyamat vagy az alkalmazás nevével.)</span><span class="sxs-lookup"><span data-stu-id="44550-144">The performance counter instance name typically segments counters such as processor time by the name of the process or application.)</span></span>

<span data-ttu-id="44550-145">A rendelkezésre álló memória diagram beolvasása a legutóbbi időszakban:</span><span class="sxs-lookup"><span data-stu-id="44550-145">To get a chart of available memory over the recent period:</span></span> 

![Az Application Insights Analytics memória timechart](./media/app-insights-performance-counters/analytics-available-memory.png)

<span data-ttu-id="44550-147">Egyéb telemetriai adatokat, például **performanceCounters** egy olyan oszlop is van `cloud_RoleInstance` azt jelzi, hogy a futtató kiszolgálópéldány, amelyen fut az alkalmazás identitását.</span><span class="sxs-lookup"><span data-stu-id="44550-147">Like other telemetry, **performanceCounters** also has a column `cloud_RoleInstance` that indicates the identity of the host server instance on which your app is running.</span></span> <span data-ttu-id="44550-148">Ha például a az alkalmazás teljesítményével kapcsolatos különböző gépeken összehasonlítandó:</span><span class="sxs-lookup"><span data-stu-id="44550-148">For example, to compare the performance of your app on the different machines:</span></span> 

![Az Application Insights Analytics szerepkör példánya szegmentált teljesítmény](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a><span data-ttu-id="44550-150">Az ASP.NET és az Application Insights száma</span><span class="sxs-lookup"><span data-stu-id="44550-150">ASP.NET and Application Insights counts</span></span>
<span data-ttu-id="44550-151">*Mi az a különbség a kivétel arányról és kivételek között?*</span><span class="sxs-lookup"><span data-stu-id="44550-151">*What's the difference between the Exception rate and Exceptions metrics?*</span></span>

* <span data-ttu-id="44550-152">*Kivétel arány* rendszer teljesítményszámláló van.</span><span class="sxs-lookup"><span data-stu-id="44550-152">*Exception rate* is a system performance counter.</span></span> <span data-ttu-id="44550-153">A közös nyelvi futtató környezet összes a kezelt és kezeletlen kivételt okozott, és a mintavételi időközben a teljes elosztja a időközt hosszának száma.</span><span class="sxs-lookup"><span data-stu-id="44550-153">The CLR counts all the handled and unhandled exceptions that are thrown, and divides the total in a sampling interval by the length of the interval.</span></span> <span data-ttu-id="44550-154">Az Application Insights SDK ennek gyűjt, és elküldi a portálon.</span><span class="sxs-lookup"><span data-stu-id="44550-154">The Application Insights SDK collects this result and sends it to the portal.</span></span>
* <span data-ttu-id="44550-155">*Kivételek* a mintavételi időközben a diagram a portál által fogadott TrackException jelentések száma.</span><span class="sxs-lookup"><span data-stu-id="44550-155">*Exceptions* is a count of the TrackException reports received by the portal in the sampling interval of the chart.</span></span> <span data-ttu-id="44550-156">Ez magában foglalja a csak a kezelt kivételek ahol TrackException hívja be a kódját, és nem tartalmazza az összes írt [nem kezelt kivételek](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="44550-156">It includes only the handled exceptions where you have written TrackException calls in your code, and doesn't include all [unhandled exceptions](app-insights-asp-net-exceptions.md).</span></span> 

## <a name="alerts"></a><span data-ttu-id="44550-157">Riasztások</span><span class="sxs-lookup"><span data-stu-id="44550-157">Alerts</span></span>
<span data-ttu-id="44550-158">Például a más metrikákkal is [riasztás beállításához](app-insights-alerts.md) figyelmezteti, ha a teljesítményszámláló megadott maximális kívül kerül.</span><span class="sxs-lookup"><span data-stu-id="44550-158">Like other metrics, you can [set an alert](app-insights-alerts.md) to warn you if a performance counter goes outside a limit you specify.</span></span> <span data-ttu-id="44550-159">Nyissa meg a riasztások panelen, majd kattintson a riasztás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="44550-159">Open the Alerts blade and click Add Alert.</span></span>

## <span data-ttu-id="44550-160"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="44550-160"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="44550-161">A függőségi nyomon követése</span><span class="sxs-lookup"><span data-stu-id="44550-161">Dependency tracking</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="44550-162">Kivétel követése</span><span class="sxs-lookup"><span data-stu-id="44550-162">Exception tracking</span></span>](app-insights-asp-net-exceptions.md)


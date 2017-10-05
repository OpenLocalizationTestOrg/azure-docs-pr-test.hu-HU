---
title: "Irányítópultok és az Azure Application Insights navigációs |} Microsoft Docs"
description: "A kulcs APM diagramok és a lekérdezések nézetek létrehozása."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 9987f04e7e71df5fe10c8bc209a390cb940ec4f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a><span data-ttu-id="6072b-103">Navigációs és az Application Insights portálon irányítópultok</span><span class="sxs-lookup"><span data-stu-id="6072b-103">Navigation and Dashboards in the Application Insights portal</span></span>
<span data-ttu-id="6072b-104">Miután [Application Insights beállítása a projekten](app-insights-overview.md), az alkalmazás teljesítmény- és használati telemetriai adatokat fog megjelenni az Application Insights-erőforrás a projekt a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6072b-104">After you have [set up Application Insights on your project](app-insights-overview.md), telemetry data about your app's performance and usage will appear in your project's Application Insights resource in the [Azure portal](https://portal.azure.com).</span></span>

## <a name="find-your-telemetry"></a><span data-ttu-id="6072b-105">A telemetriai adat található</span><span class="sxs-lookup"><span data-stu-id="6072b-105">Find your telemetry</span></span>
<span data-ttu-id="6072b-106">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) , és keresse meg az Application Insights-erőforrást, amelyet az alkalmazás hozott létre.</span><span class="sxs-lookup"><span data-stu-id="6072b-106">Sign in to the [Azure portal](https://portal.azure.com) and navigate to the Application Insights resource that you created for your app.</span></span>

![Kattintson a Tallózás gombra, válassza ki az Application Insights, majd az alkalmazást.](./media/app-insights-dashboards/00-start.png)

<span data-ttu-id="6072b-108">Az Áttekintés panel (oldal) az alkalmazás az alkalmazás fő diagnosztikai metrikáinak összegzését jeleníti meg, és egy átjárót a portál más szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="6072b-108">The overview blade (page) for your app shows a summary of the key diagnostic metrics of your app, and is a gateway to the other features of the portal.</span></span>

![A telemetriai adatok megtekintéséhez fő útvonalak](./media/app-insights-dashboards/010-oview.png)

<span data-ttu-id="6072b-110">Testre szabhatja a diagramok és rácsok és irányítópulton rögzítheti őket.</span><span class="sxs-lookup"><span data-stu-id="6072b-110">You can customize any of the charts and grids and pin them to a dashboard.</span></span> <span data-ttu-id="6072b-111">Ily módon helyezheti együtt a kulcs telemetriai adatokat más alkalmazásokból központi irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="6072b-111">That way, you can bring together the key telemetry from different apps on a central dashboard.</span></span>

## <a name="dashboards"></a><span data-ttu-id="6072b-112">Irányítópultok</span><span class="sxs-lookup"><span data-stu-id="6072b-112">Dashboards</span></span>
<span data-ttu-id="6072b-113">Először is látni való bejelentkezés után a [Microsoft Azure-portálon](https://portal.azure.com) olyan irányítópult.</span><span class="sxs-lookup"><span data-stu-id="6072b-113">The first thing you see after you sign in to the [Microsoft Azure portal](https://portal.azure.com) is a dashboard.</span></span> <span data-ttu-id="6072b-114">Itt helyezheti együtt a diagramok, amelyek számára fontos összes az Azure-erőforrások, például a telemetriai adatok között [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6072b-114">Here you can bring together the charts that are most important to you across all your Azure resources, including telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

![Személyre szabott irányítópultot.](./media/app-insights-dashboards/31.png)

1. <span data-ttu-id="6072b-116">**Navigáljon a konkrét erőforrásokat** például az alkalmazás az Application Insightsban: a bal oldali sávon használja.</span><span class="sxs-lookup"><span data-stu-id="6072b-116">**Navigate to specific resources** such as your app in Application Insights: Use the left bar.</span></span>
2. <span data-ttu-id="6072b-117">**Térjen vissza az aktuális irányítópulton**, vagy más használt nézetek váltani: a legördülő menü bal felső használatát.</span><span class="sxs-lookup"><span data-stu-id="6072b-117">**Return to the current dashboard**, or switch to other recent views: Use the drop-down menu at top left.</span></span>
3. <span data-ttu-id="6072b-118">**Váltás az irányítópultok**: a legördülő menü használatát az irányítópult-cím</span><span class="sxs-lookup"><span data-stu-id="6072b-118">**Switch dashboards**: Use the drop-down menu on the dashboard title</span></span>
4. <span data-ttu-id="6072b-119">**Létrehozására, szerkesztésére és irányítópultok megosztása** irányítópult eszköztárán.</span><span class="sxs-lookup"><span data-stu-id="6072b-119">**Create, edit, and share dashboards** in the dashboard toolbar.</span></span>
5. <span data-ttu-id="6072b-120">**Az irányítópult szerkesztése**: rámutat egy csempe majd a felső sávon használatával helyezze át, testre, illetve nem távolítható el.</span><span class="sxs-lookup"><span data-stu-id="6072b-120">**Edit the dashboard**: Hover over a tile and then use its top bar to move, customize, or remove it.</span></span>

## <a name="add-to-a-dashboard"></a><span data-ttu-id="6072b-121">Irányítópult felvétele</span><span class="sxs-lookup"><span data-stu-id="6072b-121">Add to a dashboard</span></span>
<span data-ttu-id="6072b-122">A panel vagy diagramok készletét, amely különösen fontos van szüksége, amikor rögzíthető egy másolatát az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="6072b-122">When you're looking at a blade or set of charts that's particularly interesting, you can pin a copy of it to the dashboard.</span></span> <span data-ttu-id="6072b-123">Megjelenik a legközelebb van vissza.</span><span class="sxs-lookup"><span data-stu-id="6072b-123">You'll see it next time you return there.</span></span>

![PIN-kód egy diagram, akkor mutat, és kattintson a "...", a fejlécben.](./media/app-insights-dashboards/33.png)

1. <span data-ttu-id="6072b-125">PIN-kód a diagram az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="6072b-125">Pin chart to dashboard.</span></span> <span data-ttu-id="6072b-126">A diagram másolata megjelenik az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="6072b-126">A copy of the chart appears on the dashboard.</span></span>
2. <span data-ttu-id="6072b-127">PIN-kód a teljes panelen az irányítópult - csempe, amely keresztül kattintson az irányítópulton megjelenő.</span><span class="sxs-lookup"><span data-stu-id="6072b-127">Pin the whole blade to the dashboard - it appears on the dashboard as a tile that you can click through.</span></span>
3. <span data-ttu-id="6072b-128">Kattintson a bal felső az aktuális irányítópulton való visszatéréshez.</span><span class="sxs-lookup"><span data-stu-id="6072b-128">Click the top left corner to return to the current dashboard.</span></span> <span data-ttu-id="6072b-129">A legördülő menü segítségével majd térjen vissza az aktuális nézet.</span><span class="sxs-lookup"><span data-stu-id="6072b-129">Then you can use the drop-down menu to return to the current view.</span></span>

<span data-ttu-id="6072b-130">Figyelje meg, hogy diagramok csempék vannak csoportosítva: egy csempe tartalmazhat egynél több diagram.</span><span class="sxs-lookup"><span data-stu-id="6072b-130">Notice that charts are grouped into tiles: a tile can contain more than one chart.</span></span> <span data-ttu-id="6072b-131">A teljes csempe az irányítópulton rögzíti.</span><span class="sxs-lookup"><span data-stu-id="6072b-131">You pin the whole tile to the dashboard.</span></span>

<span data-ttu-id="6072b-132">A rendszer automatikusan frissíti a diagram, amelyek elengedhetetlenek az időtartományt a diagram gyakorisággal:</span><span class="sxs-lookup"><span data-stu-id="6072b-132">The chart is automatically refreshed with a frequency that depends on the chart's time range:</span></span>

* <span data-ttu-id="6072b-133">Idő legfeljebb 1 óra között: 5 percenként frissítése</span><span class="sxs-lookup"><span data-stu-id="6072b-133">Time range up to 1 hour: Refresh every 5 minutes</span></span>
* <span data-ttu-id="6072b-134">1-24 óránként időtartománynak: 15 percenként frissítése</span><span class="sxs-lookup"><span data-stu-id="6072b-134">Time range 1 - 24 hours: Refresh every 15 minutes</span></span>
* <span data-ttu-id="6072b-135">24 óra fent időtartománynak: (időtartománynak) 60.</span><span class="sxs-lookup"><span data-stu-id="6072b-135">Time range above 24 hours: (Time range)/60.</span></span>

### <a name="pin-any-query-in-analytics"></a><span data-ttu-id="6072b-136">A lekérdezés Analytics PIN-kód</span><span class="sxs-lookup"><span data-stu-id="6072b-136">Pin any query in Analytics</span></span>
<span data-ttu-id="6072b-137">Is [PIN-kód Analytics](app-insights-analytics-using.md#pin-to-dashboard) a diagramok egy [megosztott](#share-dashboards-with-your-team) irányítópult.</span><span class="sxs-lookup"><span data-stu-id="6072b-137">You can also [pin Analytics](app-insights-analytics-using.md#pin-to-dashboard) charts to a [shared](#share-dashboards-with-your-team) dashboard.</span></span> <span data-ttu-id="6072b-138">Ez lehetővé teszi, hogy bármilyen tetszőleges lekérdezés mellé a standard metrikák diagramokat hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="6072b-138">This allows you to add charts of any arbitrary query alongside the standard metrics.</span></span> 

<span data-ttu-id="6072b-139">Automatikusan újraszámítás óránként.</span><span class="sxs-lookup"><span data-stu-id="6072b-139">Results are automatically recalculated every hour.</span></span> <span data-ttu-id="6072b-140">Kattintson a diagram hatására újraszámította az azonnal frissítési ikonjára.</span><span class="sxs-lookup"><span data-stu-id="6072b-140">Click the Refresh icon on the chart to recalculate immediately.</span></span> <span data-ttu-id="6072b-141">(Böngésző frissítési nem újraszámítása.)</span><span class="sxs-lookup"><span data-stu-id="6072b-141">(Browser refresh doesn't recalculate.)</span></span>

## <a name="adjust-a-tile-on-the-dashboard"></a><span data-ttu-id="6072b-142">Egy csempe az irányítópulton beállítása</span><span class="sxs-lookup"><span data-stu-id="6072b-142">Adjust a tile on the dashboard</span></span>
<span data-ttu-id="6072b-143">Ha egy csempére az irányítópulton, beállíthatja azt.</span><span class="sxs-lookup"><span data-stu-id="6072b-143">Once a tile is on the dashboard, you can adjust it.</span></span>

![Ahhoz, hogy szerkesztheti a diagram mutasson.](./media/app-insights-dashboards/36.png)

1. <span data-ttu-id="6072b-145">A diagram hozzáadása a csempén.</span><span class="sxs-lookup"><span data-stu-id="6072b-145">Add a chart to the tile.</span></span>
2. <span data-ttu-id="6072b-146">Állítsa be a metrika, csoportosítási dimenzió és diagram stílusát (táblázat, diagramhoz).</span><span class="sxs-lookup"><span data-stu-id="6072b-146">Set the metric, group-by dimension and style (table, graph) of a chart.</span></span>
3. <span data-ttu-id="6072b-147">Húzza a diagram a nagyításhoz; keresztül a visszavonási gombra a timespan; a csempére a diagramok szűrő tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="6072b-147">Drag across the diagram to zoom in; click the undo button to reset the timespan; set filter properties for the charts on the tile.</span></span>
4. <span data-ttu-id="6072b-148">Állítsa be a csempe neve.</span><span class="sxs-lookup"><span data-stu-id="6072b-148">Set tile title.</span></span>

<span data-ttu-id="6072b-149">Az Áttekintés panelen szolgáltatásból rögzített mozaiklapok-nál több szerkesztési lehetősége van a metrika explorer paneleken szolgáltatásból rögzített mozaiklapok.</span><span class="sxs-lookup"><span data-stu-id="6072b-149">Tiles pinned from metric explorer blades have more editing options than tiles pinned from an Overview blade.</span></span>

<span data-ttu-id="6072b-150">Az eredeti csempét rögzített folyamatot nem befolyásolja a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="6072b-150">The original tile that you pinned isn't affected by your edits.</span></span>

## <a name="switch-between-dashboards"></a><span data-ttu-id="6072b-151">Irányítópultok közötti váltáshoz</span><span class="sxs-lookup"><span data-stu-id="6072b-151">Switch between dashboards</span></span>
<span data-ttu-id="6072b-152">Egynél több irányítópult mentése és a kettő közötti váltás.</span><span class="sxs-lookup"><span data-stu-id="6072b-152">You can save more than one dashboard and switch between them.</span></span> <span data-ttu-id="6072b-153">A diagram vagy a panel rögzíti, amikor azok az aktuális irányítópulton van adva.</span><span class="sxs-lookup"><span data-stu-id="6072b-153">When you pin a chart or blade, they're added to the current dashboard.</span></span>

![Irányítópultok közötti váltáshoz kattintson az irányítópulton, és válasszon ki egy mentett irányítópultot.](./media/app-insights-dashboards/32.png)

<span data-ttu-id="6072b-157">Például lehetséges, hogy a teljes képernyős megjelenő a csapat helyiségben, és egy másik általános fejlesztési egy irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="6072b-157">For example, you might have one dashboard for displaying full screen in the team room, and another for general development.</span></span>

<span data-ttu-id="6072b-158">Az irányítópult egy panel csempe jelenik meg: a gombra kattintva nyissa meg a paneljét.</span><span class="sxs-lookup"><span data-stu-id="6072b-158">On the dashboard, a blade appears as a tile: click it to go to the blade.</span></span> <span data-ttu-id="6072b-159">A diagram a diagram az eredeti helyére replikálja.</span><span class="sxs-lookup"><span data-stu-id="6072b-159">A chart replicates the chart in its original location.</span></span>

![Nyissa meg a panelt, amely egy csempére kattint](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a><span data-ttu-id="6072b-161">Irányítópultok megosztása</span><span class="sxs-lookup"><span data-stu-id="6072b-161">Share dashboards</span></span>
<span data-ttu-id="6072b-162">Ha létrehozott egy irányítópultot, más felhasználókkal megoszthatja azt.</span><span class="sxs-lookup"><span data-stu-id="6072b-162">When you've created a dashboard, you can share it with other users.</span></span>

![Az irányítópult-fejlécben kattintson a megosztás](./media/app-insights-dashboards/41.png)

<span data-ttu-id="6072b-164">További tudnivalók [szerepkörök és hozzáférés-vezérlés](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="6072b-164">Learn about [Roles and access control](app-insights-resources-roles-access-control.md).</span></span>

## <a name="app-navigation"></a><span data-ttu-id="6072b-165">Alkalmazás navigációs</span><span class="sxs-lookup"><span data-stu-id="6072b-165">App navigation</span></span>
<span data-ttu-id="6072b-166">Az Áttekintés panel az alkalmazással kapcsolatos további információkat az átjárót.</span><span class="sxs-lookup"><span data-stu-id="6072b-166">The overview blade is the gateway to more information about your app.</span></span>

* <span data-ttu-id="6072b-167">**A diagram vagy a csempe** – kattintson a csempére vagy a diagramon megjelenő információk kapcsolatos részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="6072b-167">**Any chart or tile** - Click any tile or chart to see more detail about what it displays.</span></span>

### <a name="overview-blade-buttons"></a><span data-ttu-id="6072b-168">Áttekintés panel gombok</span><span class="sxs-lookup"><span data-stu-id="6072b-168">Overview blade buttons</span></span>
![Áttekintés panel felső navigációs sáv](./media/app-insights-dashboards/app-overview-top-nav.png)

* <span data-ttu-id="6072b-170">[**Metrikaböngésző** ](app-insights-metrics-explorer.md) -teljesítményt és a használati saját diagramokat.</span><span class="sxs-lookup"><span data-stu-id="6072b-170">[**Metrics Explorer**](app-insights-metrics-explorer.md) - Create your own charts of performance and usage.</span></span>
* <span data-ttu-id="6072b-171">[**Keresési** ](app-insights-diagnostic-search.md) - vizsgálja meg az esemény, például a kérelmekről, kivételek, olyan specifikus példányai, vagy a nyomkövetési naplófájl.</span><span class="sxs-lookup"><span data-stu-id="6072b-171">[**Search**](app-insights-diagnostic-search.md) - Investigate specific instances of events such as requests, exceptions, or log traces.</span></span>
* <span data-ttu-id="6072b-172">[**Elemzés** ](app-insights-analytics.md) -hatékony a lekérdezések a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="6072b-172">[**Analytics**](app-insights-analytics.md) - Powerful queries over your telemetry.</span></span>
* <span data-ttu-id="6072b-173">**Időtartománynak** -állítsa be a a panelen a diagramok szerint jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6072b-173">**Time range** - Adjust the range displayed by all the charts on the blade.</span></span>
* <span data-ttu-id="6072b-174">**Törlés** -törli az Application Insights-erőforrást az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="6072b-174">**Delete** - Delete the Application Insights resource for this app.</span></span> <span data-ttu-id="6072b-175">Meg kell is távolítsa el az Application Insights csomagokat az alkalmazás kódját, vagy szerkesztheti a [instrumentation kulcs](app-insights-create-new-resource.md#copy-the-instrumentation-key) át tudja irányítani a telemetria bekapcsolásával egy másik Application Insights-erőforrást az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6072b-175">You should also either remove the Application Insights packages from your app code, or edit the [instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) in your app to direct telemetry to a different Application Insights resource.</span></span>

### <a name="essentials-tab"></a><span data-ttu-id="6072b-176">Alapvető erőforrások lapon</span><span class="sxs-lookup"><span data-stu-id="6072b-176">Essentials tab</span></span>
* <span data-ttu-id="6072b-177">[Instrumentation kulcs](app-insights-create-new-resource.md#copy-the-instrumentation-key) -azonosítja az alkalmazás-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="6072b-177">[Instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) - Identifies this app resource.</span></span>
* <span data-ttu-id="6072b-178">Tarifacsomag - teheti a rendelkezésre álló és beállított kötet caps szolgáltatásait.</span><span class="sxs-lookup"><span data-stu-id="6072b-178">Pricing - Make features available and set volume caps.</span></span>

### <a name="app-navigation-bar"></a><span data-ttu-id="6072b-179">Alkalmazás navigációs sáv</span><span class="sxs-lookup"><span data-stu-id="6072b-179">App navigation bar</span></span>
![Bal oldali navigációs sáv](./media/app-insights-dashboards/app-left-nav-bar.png)

* <span data-ttu-id="6072b-181">**Áttekintés** – térjen vissza az Áttekintés panelen.</span><span class="sxs-lookup"><span data-stu-id="6072b-181">**Overview** - Return to the app overview blade.</span></span>
* <span data-ttu-id="6072b-182">**Tevékenységnapló** -riasztások és az Azure felügyeleti események.</span><span class="sxs-lookup"><span data-stu-id="6072b-182">**Activity log** - Alerts and Azure administrative events.</span></span>
* <span data-ttu-id="6072b-183">[**Hozzáférés-vezérlés** ](app-insights-resources-roles-access-control.md) -hozzáférést biztosítson a csoport tagjai, míg más.</span><span class="sxs-lookup"><span data-stu-id="6072b-183">[**Access control**](app-insights-resources-roles-access-control.md) - Provide access to team members and others.</span></span>
* <span data-ttu-id="6072b-184">[**Címkék** ](../azure-resource-manager/resource-group-using-tags.md) -címkék használata az alkalmazás másokkal csoportosításához.</span><span class="sxs-lookup"><span data-stu-id="6072b-184">[**Tags**](../azure-resource-manager/resource-group-using-tags.md) - Use tags to group your app with others.</span></span>

<span data-ttu-id="6072b-185">VIZSGÁLJA MEG</span><span class="sxs-lookup"><span data-stu-id="6072b-185">INVESTIGATE</span></span>

* <span data-ttu-id="6072b-186">[**Alkalmazás-hozzárendelés** ](app-insights-app-map.md) -Active térkép, amely az alkalmazás összetevői a függőségi adatokat származik.</span><span class="sxs-lookup"><span data-stu-id="6072b-186">[**Application map**](app-insights-app-map.md) - Active map showing the components of your application, derived from the dependency information.</span></span>
* <span data-ttu-id="6072b-187">[**Észlelési intelligens** ](app-insights-proactive-diagnostics.md) -tekintse át a legutóbbi teljesítményével kapcsolatos riasztások.</span><span class="sxs-lookup"><span data-stu-id="6072b-187">[**Smart Detection**](app-insights-proactive-diagnostics.md) - Review recent performance alerts.</span></span>
* <span data-ttu-id="6072b-188">[**Élő Stream** ](app-insights-live-stream.md) – A rögzített szinte azonnali mérőszámokat, akkor hasznos, ha egy új build telepítése vagy a hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="6072b-188">[**Live Stream**](app-insights-live-stream.md) - A fixed set of near-instant metrics, useful when deploying a new build or debugging.</span></span>
* <span data-ttu-id="6072b-189">[**Rendelkezésre állás / a webalkalmazás-tesztek** ](app-insights-monitor-web-app-availability.md) -rendszeres kérelmeket küldeni a webalkalmazás a world.* körül</span><span class="sxs-lookup"><span data-stu-id="6072b-189">[**Availability / Web tests**](app-insights-monitor-web-app-availability.md) - Send regular requests to your web app from around the world.*</span></span>
* <span data-ttu-id="6072b-190">[**Hibák, a teljesítmény** ](app-insights-web-monitor-performance.md) -kivételek, hiba díjszabás és válaszidejének a kéréseket az alkalmazáshoz, és az alkalmazásnak, hogy a érkező kéréseket [függőségek](app-insights-asp-net-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="6072b-190">[**Failures, Performance**](app-insights-web-monitor-performance.md) - Exceptions, failure rates and response times for requests to your app and for requests from your app to [dependencies](app-insights-asp-net-dependencies.md).</span></span>
* <span data-ttu-id="6072b-191">[**Teljesítmény** ](app-insights-web-monitor-performance.md) -válaszidőt, függőség válaszidejét.</span><span class="sxs-lookup"><span data-stu-id="6072b-191">[**Performance**](app-insights-web-monitor-performance.md) - Response time, dependency response times.</span></span>
* <span data-ttu-id="6072b-192">[Kiszolgálók](app-insights-web-monitor-performance.md) -teljesítményszámlálókat.</span><span class="sxs-lookup"><span data-stu-id="6072b-192">[Servers](app-insights-web-monitor-performance.md) - Performance counters.</span></span> <span data-ttu-id="6072b-193">Ha elérhető, [Állapotmonitor telepítése](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="6072b-193">Available if you [install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="6072b-194">**Böngésző** -nézet és AJAX teljesítmény lapon.</span><span class="sxs-lookup"><span data-stu-id="6072b-194">**Browser** - Page view and AJAX performance.</span></span> <span data-ttu-id="6072b-195">Ha elérhető, [állíthatnak be a weblapok](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="6072b-195">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>
* <span data-ttu-id="6072b-196">**Használati** -lapon nézet, a felhasználó és a munkamenet számát.</span><span class="sxs-lookup"><span data-stu-id="6072b-196">**Usage** - Page view, user, and session counts.</span></span> <span data-ttu-id="6072b-197">Ha elérhető, [állíthatnak be a weblapok](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="6072b-197">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>

<span data-ttu-id="6072b-198">KONFIGURÁLÁSA</span><span class="sxs-lookup"><span data-stu-id="6072b-198">CONFIGURE</span></span>

* <span data-ttu-id="6072b-199">**Első lépések** -beágyazott oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="6072b-199">**Getting started** - inline tutorial.</span></span>
* <span data-ttu-id="6072b-200">**Tulajdonságok** -instrumentation kulcs, az előfizetés és az erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="6072b-200">**Properties** - instrumentation key, subscription and resource id.</span></span>
* <span data-ttu-id="6072b-201">[Riasztások](app-insights-alerts.md) -riasztási konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="6072b-201">[Alerts](app-insights-alerts.md) - metric alert configuration.</span></span>
* <span data-ttu-id="6072b-202">[A folyamatos exportálás](app-insights-export-telemetry.md) -konfigurálása az Azure storage telemetriai adatainak exportálása.</span><span class="sxs-lookup"><span data-stu-id="6072b-202">[Continuous export](app-insights-export-telemetry.md) - configure export of telemetry to Azure storage.</span></span>
* <span data-ttu-id="6072b-203">[A teljesítmény tesztelése](app-insights-monitor-web-app-availability.md#performance-tests) -állítsa be a webhely szintetikus terhelése.</span><span class="sxs-lookup"><span data-stu-id="6072b-203">[Performance testing](app-insights-monitor-web-app-availability.md#performance-tests) - set up a synthetic load on your website.</span></span>
* <span data-ttu-id="6072b-204">[Kvóta és árképzési](app-insights-pricing.md) és [adatfeldolgozást mintavételi](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="6072b-204">[Quota and pricing](app-insights-pricing.md) and [ingestion sampling](app-insights-sampling.md).</span></span>
* <span data-ttu-id="6072b-205">**API-hozzáférés** -létrehozása [kiadási jegyzetek](app-insights-annotations.md) és az adatok hozzáférés az API-hoz.</span><span class="sxs-lookup"><span data-stu-id="6072b-205">**API Access** - Create [release annotations](app-insights-annotations.md) and for the Data Access API.</span></span>
* <span data-ttu-id="6072b-206">[**Munkaelemek** ](app-insights-diagnostic-search.md#create-work-item) -nyomkövetési rendszer, így hibák telemetriai vizsgálatakor hozhat létre a munkájukhoz való csatlakozásban.</span><span class="sxs-lookup"><span data-stu-id="6072b-206">[**Work Items**](app-insights-diagnostic-search.md#create-work-item) - Connect to a work tracking system so that you can create bugs while inspecting telemetry.</span></span>

<span data-ttu-id="6072b-207">BEÁLLÍTÁSOK</span><span class="sxs-lookup"><span data-stu-id="6072b-207">SETTINGS</span></span>

* <span data-ttu-id="6072b-208">[**Zárolja** ](../azure-resource-manager/resource-group-lock-resources.md) -Azure-erőforrások zárolása</span><span class="sxs-lookup"><span data-stu-id="6072b-208">[**Locks**](../azure-resource-manager/resource-group-lock-resources.md) - lock Azure resources</span></span>
* <span data-ttu-id="6072b-209">[**Automatizálási parancsfájl** ](app-insights-powershell.md) -exportálása olyan az Azure erőforrás-definícióval, így használhatja a sablont létrehozni az új erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="6072b-209">[**Automation script**](app-insights-powershell.md) - export a definition of the Azure resource so that you can use it as a template to create new resources.</span></span>


## <a name="video"></a><span data-ttu-id="6072b-210">Videó</span><span class="sxs-lookup"><span data-stu-id="6072b-210">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="6072b-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6072b-211">Next steps</span></span>

|  |  |
| --- | --- |
| [<span data-ttu-id="6072b-212">Metrikaböngésző</span><span class="sxs-lookup"><span data-stu-id="6072b-212">Metrics explorer</span></span>](app-insights-metrics-explorer.md)<br/><span data-ttu-id="6072b-213">Szűrő és a szegmens metrikák</span><span class="sxs-lookup"><span data-stu-id="6072b-213">Filter and segment metrics</span></span> |![Keresés – példa](./media/app-insights-dashboards/64.png) |
| [<span data-ttu-id="6072b-215">Diagnosztikai keresése</span><span class="sxs-lookup"><span data-stu-id="6072b-215">Diagnostic search</span></span>](app-insights-diagnostic-search.md)<br/><span data-ttu-id="6072b-216">Található és vizsgálja meg az események, kapcsolódó események és hibák létrehozása</span><span class="sxs-lookup"><span data-stu-id="6072b-216">Find and inspect events, related events, and create bugs</span></span> |![Keresés – példa](./media/app-insights-dashboards/61.png) |
| [<span data-ttu-id="6072b-218">Elemzés</span><span class="sxs-lookup"><span data-stu-id="6072b-218">Analytics</span></span>](app-insights-analytics.md)<br/><span data-ttu-id="6072b-219">Hatékony lekérdezési nyelv</span><span class="sxs-lookup"><span data-stu-id="6072b-219">Powerful query language</span></span> |![Keresés – példa](./media/app-insights-dashboards/63.png) |

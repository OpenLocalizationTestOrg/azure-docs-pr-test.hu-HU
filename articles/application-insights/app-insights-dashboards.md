---
title: "aaaDashboards és navigációs hello Azure Application Insights |} Microsoft Docs"
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
ms.openlocfilehash: 58811388205643bb672e0405b3226f12d0f447a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="navigation-and-dashboards-in-hello-application-insights-portal"></a><span data-ttu-id="473ba-103">Navigációs és irányítópultok a hello Application Insights portál</span><span class="sxs-lookup"><span data-stu-id="473ba-103">Navigation and Dashboards in hello Application Insights portal</span></span>
<span data-ttu-id="473ba-104">Miután [Application Insights beállítása a projekten](app-insights-overview.md), az alkalmazás teljesítmény- és használati telemetriai adatokat jelennek meg a projekt Application Insights-erőforrás a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="473ba-104">After you have [set up Application Insights on your project](app-insights-overview.md), telemetry data about your app's performance and usage will appear in your project's Application Insights resource in hello [Azure portal](https://portal.azure.com).</span></span>

## <a name="find-your-telemetry"></a><span data-ttu-id="473ba-105">A telemetriai adat található</span><span class="sxs-lookup"><span data-stu-id="473ba-105">Find your telemetry</span></span>
<span data-ttu-id="473ba-106">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) , és keresse meg a toohello Application Insights-erőforrást, amelyet az alkalmazás hozott létre.</span><span class="sxs-lookup"><span data-stu-id="473ba-106">Sign in toohello [Azure portal](https://portal.azure.com) and navigate toohello Application Insights resource that you created for your app.</span></span>

![Kattintson a Tallózás gombra, válassza ki az Application Insights, majd az alkalmazást.](./media/app-insights-dashboards/00-start.png)

<span data-ttu-id="473ba-108">hello áttekintése panelen (oldal) az alkalmazás hello diagnosztikai alapvető metrikákat az alkalmazás összegzését jeleníti meg, és egy átjáró toohello hello portál egyéb szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="473ba-108">hello overview blade (page) for your app shows a summary of hello key diagnostic metrics of your app, and is a gateway toohello other features of hello portal.</span></span>

![A fő útvonalak tooview a telemetria](./media/app-insights-dashboards/010-oview.png)

<span data-ttu-id="473ba-110">Testre szabhatja hello diagramok és rácsok és tooa irányítópulton rögzítheti őket.</span><span class="sxs-lookup"><span data-stu-id="473ba-110">You can customize any of hello charts and grids and pin them tooa dashboard.</span></span> <span data-ttu-id="473ba-111">Ily módon helyezheti együtt hello kulcs telemetriai adatokat a különböző alkalmazások az központi irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="473ba-111">That way, you can bring together hello key telemetry from different apps on a central dashboard.</span></span>

## <a name="dashboards"></a><span data-ttu-id="473ba-112">Irányítópultok</span><span class="sxs-lookup"><span data-stu-id="473ba-112">Dashboards</span></span>
<span data-ttu-id="473ba-113">hello thing először megjelenik a bejelentkezést toohello [Microsoft Azure-portálon](https://portal.azure.com) olyan irányítópult.</span><span class="sxs-lookup"><span data-stu-id="473ba-113">hello first thing you see after you sign in toohello [Microsoft Azure portal](https://portal.azure.com) is a dashboard.</span></span> <span data-ttu-id="473ba-114">Itt helyezheti együtt hello diagramok, amelyek a legfontosabb tooyou közötti összes az Azure-erőforrások, például a telemetriai [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="473ba-114">Here you can bring together hello charts that are most important tooyou across all your Azure resources, including telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

![Személyre szabott irányítópultot.](./media/app-insights-dashboards/31.png)

1. <span data-ttu-id="473ba-116">**Keresse meg a toospecific erőforrások** például az alkalmazás az Application Insightsban: használata hello bal oldali sávon.</span><span class="sxs-lookup"><span data-stu-id="473ba-116">**Navigate toospecific resources** such as your app in Application Insights: Use hello left bar.</span></span>
2. <span data-ttu-id="473ba-117">**Visszatérési toohello aktuális irányítópult**, vagy váltson tooother használt nézetek: használata hello a bal felső legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="473ba-117">**Return toohello current dashboard**, or switch tooother recent views: Use hello drop-down menu at top left.</span></span>
3. <span data-ttu-id="473ba-118">**Váltás az irányítópultok**: használata hello legördülő menüjének hello irányítópult cím</span><span class="sxs-lookup"><span data-stu-id="473ba-118">**Switch dashboards**: Use hello drop-down menu on hello dashboard title</span></span>
4. <span data-ttu-id="473ba-119">**Létrehozására, szerkesztésére és irányítópultok megosztása** hello irányítópult eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="473ba-119">**Create, edit, and share dashboards** in hello dashboard toolbar.</span></span>
5. <span data-ttu-id="473ba-120">**Hello irányítópult szerkesztése**: rámutat egy csempére, majd a felső menüsoron toomove, testreszabása, illetve nem távolítható el.</span><span class="sxs-lookup"><span data-stu-id="473ba-120">**Edit hello dashboard**: Hover over a tile and then use its top bar toomove, customize, or remove it.</span></span>

## <a name="add-tooa-dashboard"></a><span data-ttu-id="473ba-121">Adja hozzá a tooa irányítópult</span><span class="sxs-lookup"><span data-stu-id="473ba-121">Add tooa dashboard</span></span>
<span data-ttu-id="473ba-122">Egy panel vagy diagramok készletét, amely különösen fontos van szüksége, amikor toohello irányítópult egy példányát is rögzíti.</span><span class="sxs-lookup"><span data-stu-id="473ba-122">When you're looking at a blade or set of charts that's particularly interesting, you can pin a copy of it toohello dashboard.</span></span> <span data-ttu-id="473ba-123">Megjelenik a legközelebb van vissza.</span><span class="sxs-lookup"><span data-stu-id="473ba-123">You'll see it next time you return there.</span></span>

![toopin diagramot, akkor mutat, és kattintson a "..." hello fejlécben.](./media/app-insights-dashboards/33.png)

1. <span data-ttu-id="473ba-125">PIN-kód diagram toodashboard.</span><span class="sxs-lookup"><span data-stu-id="473ba-125">Pin chart toodashboard.</span></span> <span data-ttu-id="473ba-126">Hello diagram másolata hello irányítópulton megjelenik.</span><span class="sxs-lookup"><span data-stu-id="473ba-126">A copy of hello chart appears on hello dashboard.</span></span>
2. <span data-ttu-id="473ba-127">PIN-kód hello teljes panel toohello irányítópult - megjelenő hello irányítópult csempe, amely keresztül is kattinthat.</span><span class="sxs-lookup"><span data-stu-id="473ba-127">Pin hello whole blade toohello dashboard - it appears on hello dashboard as a tile that you can click through.</span></span>
3. <span data-ttu-id="473ba-128">Kattintson a hello bal felső sarokban tooreturn toohello aktuális irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="473ba-128">Click hello top left corner tooreturn toohello current dashboard.</span></span> <span data-ttu-id="473ba-129">Majd hello legördülő menü tooreturn toohello aktuális nézet használható.</span><span class="sxs-lookup"><span data-stu-id="473ba-129">Then you can use hello drop-down menu tooreturn toohello current view.</span></span>

<span data-ttu-id="473ba-130">Figyelje meg, hogy diagramok csempék vannak csoportosítva: egy csempe tartalmazhat egynél több diagram.</span><span class="sxs-lookup"><span data-stu-id="473ba-130">Notice that charts are grouped into tiles: a tile can contain more than one chart.</span></span> <span data-ttu-id="473ba-131">Hello teljes csempe toohello irányítópult rögzíti.</span><span class="sxs-lookup"><span data-stu-id="473ba-131">You pin hello whole tile toohello dashboard.</span></span>

<span data-ttu-id="473ba-132">a rendszer automatikusan frissíti hello diagram, amelyek elengedhetetlenek az hello diagram időtartomány gyakorisággal:</span><span class="sxs-lookup"><span data-stu-id="473ba-132">hello chart is automatically refreshed with a frequency that depends on hello chart's time range:</span></span>

* <span data-ttu-id="473ba-133">Időtartománynak be az too1: 5 percenként frissítése</span><span class="sxs-lookup"><span data-stu-id="473ba-133">Time range up too1 hour: Refresh every 5 minutes</span></span>
* <span data-ttu-id="473ba-134">1-24 óránként időtartománynak: 15 percenként frissítése</span><span class="sxs-lookup"><span data-stu-id="473ba-134">Time range 1 - 24 hours: Refresh every 15 minutes</span></span>
* <span data-ttu-id="473ba-135">24 óra fent időtartománynak: (időtartománynak) 60.</span><span class="sxs-lookup"><span data-stu-id="473ba-135">Time range above 24 hours: (Time range)/60.</span></span>

### <a name="pin-any-query-in-analytics"></a><span data-ttu-id="473ba-136">A lekérdezés Analytics PIN-kód</span><span class="sxs-lookup"><span data-stu-id="473ba-136">Pin any query in Analytics</span></span>
<span data-ttu-id="473ba-137">Emellett [PIN-kód Analytics](app-insights-analytics-using.md#pin-to-dashboard) diagramok tooa [megosztott](#share-dashboards-with-your-team) irányítópult.</span><span class="sxs-lookup"><span data-stu-id="473ba-137">You can also [pin Analytics](app-insights-analytics-using.md#pin-to-dashboard) charts tooa [shared](#share-dashboards-with-your-team) dashboard.</span></span> <span data-ttu-id="473ba-138">Ez lehetővé teszi bármilyen tetszőleges lekérdezés mellett hello szabványos metrikák tooadd diagramokat.</span><span class="sxs-lookup"><span data-stu-id="473ba-138">This allows you tooadd charts of any arbitrary query alongside hello standard metrics.</span></span> 

<span data-ttu-id="473ba-139">Automatikusan újraszámítás óránként.</span><span class="sxs-lookup"><span data-stu-id="473ba-139">Results are automatically recalculated every hour.</span></span> <span data-ttu-id="473ba-140">Kattintson a hello frissítési ikonjára hello diagram toorecalculate azonnal.</span><span class="sxs-lookup"><span data-stu-id="473ba-140">Click hello Refresh icon on hello chart toorecalculate immediately.</span></span> <span data-ttu-id="473ba-141">(Böngésző frissítési nem újraszámítása.)</span><span class="sxs-lookup"><span data-stu-id="473ba-141">(Browser refresh doesn't recalculate.)</span></span>

## <a name="adjust-a-tile-on-hello-dashboard"></a><span data-ttu-id="473ba-142">Állítsa be a hello Irányítópulton egy csempére</span><span class="sxs-lookup"><span data-stu-id="473ba-142">Adjust a tile on hello dashboard</span></span>
<span data-ttu-id="473ba-143">Ha egy csempére hello irányítópulton, beállíthatja azt.</span><span class="sxs-lookup"><span data-stu-id="473ba-143">Once a tile is on hello dashboard, you can adjust it.</span></span>

![Mutasson a rendelés tooedit diagram azt.](./media/app-insights-dashboards/36.png)

1. <span data-ttu-id="473ba-145">Adjon hozzá egy diagram toohello követ.</span><span class="sxs-lookup"><span data-stu-id="473ba-145">Add a chart toohello tile.</span></span>
2. <span data-ttu-id="473ba-146">Hello metrika, csoportosítási dimenzió és egy diagram stílusát (táblázat, diagramhoz) beállítása.</span><span class="sxs-lookup"><span data-stu-id="473ba-146">Set hello metric, group-by dimension and style (table, graph) of a chart.</span></span>
3. <span data-ttu-id="473ba-147">Húzza keresztül hello diagram toozoom Kattintson a hello visszavonási gomb tooreset hello timespan; a hello csempére hello diagramok szűrő tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="473ba-147">Drag across hello diagram toozoom in; click hello undo button tooreset hello timespan; set filter properties for hello charts on hello tile.</span></span>
4. <span data-ttu-id="473ba-148">Állítsa be a csempe neve.</span><span class="sxs-lookup"><span data-stu-id="473ba-148">Set tile title.</span></span>

<span data-ttu-id="473ba-149">Az Áttekintés panelen szolgáltatásból rögzített mozaiklapok-nál több szerkesztési lehetősége van a metrika explorer paneleken szolgáltatásból rögzített mozaiklapok.</span><span class="sxs-lookup"><span data-stu-id="473ba-149">Tiles pinned from metric explorer blades have more editing options than tiles pinned from an Overview blade.</span></span>

<span data-ttu-id="473ba-150">hello eredeti csempét rögzített folyamatot nem befolyásolja a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="473ba-150">hello original tile that you pinned isn't affected by your edits.</span></span>

## <a name="switch-between-dashboards"></a><span data-ttu-id="473ba-151">Irányítópultok közötti váltáshoz</span><span class="sxs-lookup"><span data-stu-id="473ba-151">Switch between dashboards</span></span>
<span data-ttu-id="473ba-152">Egynél több irányítópult mentése és a kettő közötti váltás.</span><span class="sxs-lookup"><span data-stu-id="473ba-152">You can save more than one dashboard and switch between them.</span></span> <span data-ttu-id="473ba-153">Amikor a diagram vagy a panelen, azokat a toohello aktuális irányítópult éppen hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="473ba-153">When you pin a chart or blade, they're added toohello current dashboard.</span></span>

![tooswitch közötti irányítópultok, irányítópulton kattintson, és válasszon ki egy mentett irányítópultot.](./media/app-insights-dashboards/32.png)

<span data-ttu-id="473ba-157">Például lehetséges, hogy a teljes képernyős megjelenő hello team helyet, és egy másik általános fejlesztési egy irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="473ba-157">For example, you might have one dashboard for displaying full screen in hello team room, and another for general development.</span></span>

<span data-ttu-id="473ba-158">Hello irányítópult panelek csempe jelenik meg: toogo toohello panelen kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="473ba-158">On hello dashboard, a blade appears as a tile: click it toogo toohello blade.</span></span> <span data-ttu-id="473ba-159">A diagram hello diagram az eredeti helyére replikálja.</span><span class="sxs-lookup"><span data-stu-id="473ba-159">A chart replicates hello chart in its original location.</span></span>

![Kattintson egy csempe tooopen hello panel képviseli](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a><span data-ttu-id="473ba-161">Irányítópultok megosztása</span><span class="sxs-lookup"><span data-stu-id="473ba-161">Share dashboards</span></span>
<span data-ttu-id="473ba-162">Ha létrehozott egy irányítópultot, más felhasználókkal megoszthatja azt.</span><span class="sxs-lookup"><span data-stu-id="473ba-162">When you've created a dashboard, you can share it with other users.</span></span>

![Hello irányítópult fejlécben kattintson a megosztás](./media/app-insights-dashboards/41.png)

<span data-ttu-id="473ba-164">További tudnivalók [szerepkörök és hozzáférés-vezérlés](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="473ba-164">Learn about [Roles and access control](app-insights-resources-roles-access-control.md).</span></span>

## <a name="app-navigation"></a><span data-ttu-id="473ba-165">Alkalmazás navigációs</span><span class="sxs-lookup"><span data-stu-id="473ba-165">App navigation</span></span>
<span data-ttu-id="473ba-166">hello áttekintése panel hello átjáró toomore információkat az alkalmazásról.</span><span class="sxs-lookup"><span data-stu-id="473ba-166">hello overview blade is hello gateway toomore information about your app.</span></span>

* <span data-ttu-id="473ba-167">**A diagram vagy a csempe** – kattintson a csempére vagy diagram toosee megjelenő információk további részleteit.</span><span class="sxs-lookup"><span data-stu-id="473ba-167">**Any chart or tile** - Click any tile or chart toosee more detail about what it displays.</span></span>

### <a name="overview-blade-buttons"></a><span data-ttu-id="473ba-168">Áttekintés panel gombok</span><span class="sxs-lookup"><span data-stu-id="473ba-168">Overview blade buttons</span></span>
![Áttekintés panel felső navigációs sáv](./media/app-insights-dashboards/app-overview-top-nav.png)

* <span data-ttu-id="473ba-170">[**Metrikaböngésző** ](app-insights-metrics-explorer.md) -teljesítményt és a használati saját diagramokat.</span><span class="sxs-lookup"><span data-stu-id="473ba-170">[**Metrics Explorer**](app-insights-metrics-explorer.md) - Create your own charts of performance and usage.</span></span>
* <span data-ttu-id="473ba-171">[**Keresési** ](app-insights-diagnostic-search.md) - vizsgálja meg az esemény, például a kérelmekről, kivételek, olyan specifikus példányai, vagy a nyomkövetési naplófájl.</span><span class="sxs-lookup"><span data-stu-id="473ba-171">[**Search**](app-insights-diagnostic-search.md) - Investigate specific instances of events such as requests, exceptions, or log traces.</span></span>
* <span data-ttu-id="473ba-172">[**Elemzés** ](app-insights-analytics.md) -hatékony a lekérdezések a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="473ba-172">[**Analytics**](app-insights-analytics.md) - Powerful queries over your telemetry.</span></span>
* <span data-ttu-id="473ba-173">**Időtartománynak** -megjeleníti azokat az összes hello diagramok hello panelen állítsa be hello.</span><span class="sxs-lookup"><span data-stu-id="473ba-173">**Time range** - Adjust hello range displayed by all hello charts on hello blade.</span></span>
* <span data-ttu-id="473ba-174">**Törlés** -hello Application Insights-erőforrást az alkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="473ba-174">**Delete** - Delete hello Application Insights resource for this app.</span></span> <span data-ttu-id="473ba-175">Meg kell is távolítsa el az Application Insights csomagok hello az alkalmazás kódját, vagy hello szerkesztése [instrumentation kulcs](app-insights-create-new-resource.md#copy-the-instrumentation-key) a az alkalmazás toodirect telemetriai tooa különböző Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="473ba-175">You should also either remove hello Application Insights packages from your app code, or edit hello [instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) in your app toodirect telemetry tooa different Application Insights resource.</span></span>

### <a name="essentials-tab"></a><span data-ttu-id="473ba-176">Alapvető erőforrások lapon</span><span class="sxs-lookup"><span data-stu-id="473ba-176">Essentials tab</span></span>
* <span data-ttu-id="473ba-177">[Instrumentation kulcs](app-insights-create-new-resource.md#copy-the-instrumentation-key) -azonosítja az alkalmazás-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="473ba-177">[Instrumentation key](app-insights-create-new-resource.md#copy-the-instrumentation-key) - Identifies this app resource.</span></span>
* <span data-ttu-id="473ba-178">Tarifacsomag - teheti a rendelkezésre álló és beállított kötet caps szolgáltatásait.</span><span class="sxs-lookup"><span data-stu-id="473ba-178">Pricing - Make features available and set volume caps.</span></span>

### <a name="app-navigation-bar"></a><span data-ttu-id="473ba-179">Alkalmazás navigációs sáv</span><span class="sxs-lookup"><span data-stu-id="473ba-179">App navigation bar</span></span>
![Bal oldali navigációs sáv](./media/app-insights-dashboards/app-left-nav-bar.png)

* <span data-ttu-id="473ba-181">**Áttekintés** -visszatérési toohello áttekintése panelen.</span><span class="sxs-lookup"><span data-stu-id="473ba-181">**Overview** - Return toohello app overview blade.</span></span>
* <span data-ttu-id="473ba-182">**Tevékenységnapló** -riasztások és az Azure felügyeleti események.</span><span class="sxs-lookup"><span data-stu-id="473ba-182">**Activity log** - Alerts and Azure administrative events.</span></span>
* <span data-ttu-id="473ba-183">[**Hozzáférés-vezérlés** ](app-insights-resources-roles-access-control.md) -hozzáférés tooteam tagok és mások számára.</span><span class="sxs-lookup"><span data-stu-id="473ba-183">[**Access control**](app-insights-resources-roles-access-control.md) - Provide access tooteam members and others.</span></span>
* <span data-ttu-id="473ba-184">[**Címkék** ](../azure-resource-manager/resource-group-using-tags.md) -címkéket toogroup mások az alkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="473ba-184">[**Tags**](../azure-resource-manager/resource-group-using-tags.md) - Use tags toogroup your app with others.</span></span>

<span data-ttu-id="473ba-185">VIZSGÁLJA MEG</span><span class="sxs-lookup"><span data-stu-id="473ba-185">INVESTIGATE</span></span>

* <span data-ttu-id="473ba-186">[**Alkalmazás-hozzárendelés** ](app-insights-app-map.md) -Active térkép, amely az alkalmazás összetevői hello származó hello függőségi adatokat.</span><span class="sxs-lookup"><span data-stu-id="473ba-186">[**Application map**](app-insights-app-map.md) - Active map showing hello components of your application, derived from hello dependency information.</span></span>
* <span data-ttu-id="473ba-187">[**Észlelési intelligens** ](app-insights-proactive-diagnostics.md) -tekintse át a legutóbbi teljesítményével kapcsolatos riasztások.</span><span class="sxs-lookup"><span data-stu-id="473ba-187">[**Smart Detection**](app-insights-proactive-diagnostics.md) - Review recent performance alerts.</span></span>
* <span data-ttu-id="473ba-188">[**Élő Stream** ](app-insights-live-stream.md) – A rögzített szinte azonnali mérőszámokat, akkor hasznos, ha egy új build telepítése vagy a hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="473ba-188">[**Live Stream**](app-insights-live-stream.md) - A fixed set of near-instant metrics, useful when deploying a new build or debugging.</span></span>
* <span data-ttu-id="473ba-189">[**Rendelkezésre állás / a webalkalmazás-tesztek** ](app-insights-monitor-web-app-availability.md) -küldési kérelmek rendszeres tooyour webalkalmazást az hello world.* körül</span><span class="sxs-lookup"><span data-stu-id="473ba-189">[**Availability / Web tests**](app-insights-monitor-web-app-availability.md) - Send regular requests tooyour web app from around hello world.*</span></span>
* <span data-ttu-id="473ba-190">[**Hibák, a teljesítmény** ](app-insights-web-monitor-performance.md) -kivételek, a hiba sebességét és a válasz alkalommal kérelmek tooyour alkalmazás és az alkalmazás a érkező kéréseket túl[függőségek](app-insights-asp-net-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="473ba-190">[**Failures, Performance**](app-insights-web-monitor-performance.md) - Exceptions, failure rates and response times for requests tooyour app and for requests from your app too[dependencies](app-insights-asp-net-dependencies.md).</span></span>
* <span data-ttu-id="473ba-191">[**Teljesítmény** ](app-insights-web-monitor-performance.md) -válaszidőt, függőség válaszidejét.</span><span class="sxs-lookup"><span data-stu-id="473ba-191">[**Performance**](app-insights-web-monitor-performance.md) - Response time, dependency response times.</span></span>
* <span data-ttu-id="473ba-192">[Kiszolgálók](app-insights-web-monitor-performance.md) -teljesítményszámlálókat.</span><span class="sxs-lookup"><span data-stu-id="473ba-192">[Servers](app-insights-web-monitor-performance.md) - Performance counters.</span></span> <span data-ttu-id="473ba-193">Ha elérhető, [Állapotmonitor telepítése](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="473ba-193">Available if you [install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="473ba-194">**Böngésző** -nézet és AJAX teljesítmény lapon.</span><span class="sxs-lookup"><span data-stu-id="473ba-194">**Browser** - Page view and AJAX performance.</span></span> <span data-ttu-id="473ba-195">Ha elérhető, [állíthatnak be a weblapok](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="473ba-195">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>
* <span data-ttu-id="473ba-196">**Használati** -lapon nézet, a felhasználó és a munkamenet számát.</span><span class="sxs-lookup"><span data-stu-id="473ba-196">**Usage** - Page view, user, and session counts.</span></span> <span data-ttu-id="473ba-197">Ha elérhető, [állíthatnak be a weblapok](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="473ba-197">Available if you [instrument your web pages](app-insights-javascript.md).</span></span>

<span data-ttu-id="473ba-198">KONFIGURÁLÁSA</span><span class="sxs-lookup"><span data-stu-id="473ba-198">CONFIGURE</span></span>

* <span data-ttu-id="473ba-199">**Első lépések** -beágyazott oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="473ba-199">**Getting started** - inline tutorial.</span></span>
* <span data-ttu-id="473ba-200">**Tulajdonságok** -instrumentation kulcs, az előfizetés és az erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="473ba-200">**Properties** - instrumentation key, subscription and resource id.</span></span>
* <span data-ttu-id="473ba-201">[Riasztások](app-insights-alerts.md) -riasztási konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="473ba-201">[Alerts](app-insights-alerts.md) - metric alert configuration.</span></span>
* <span data-ttu-id="473ba-202">[A folyamatos exportálás](app-insights-export-telemetry.md) -exportálási telemetriai tooAzure tárolás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="473ba-202">[Continuous export](app-insights-export-telemetry.md) - configure export of telemetry tooAzure storage.</span></span>
* <span data-ttu-id="473ba-203">[A teljesítmény tesztelése](app-insights-monitor-web-app-availability.md#performance-tests) -állítsa be a webhely szintetikus terhelése.</span><span class="sxs-lookup"><span data-stu-id="473ba-203">[Performance testing](app-insights-monitor-web-app-availability.md#performance-tests) - set up a synthetic load on your website.</span></span>
* <span data-ttu-id="473ba-204">[Kvóta és árképzési](app-insights-pricing.md) és [adatfeldolgozást mintavételi](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="473ba-204">[Quota and pricing](app-insights-pricing.md) and [ingestion sampling](app-insights-sampling.md).</span></span>
* <span data-ttu-id="473ba-205">**API-hozzáférés** -létrehozása [kiadási jegyzetek](app-insights-annotations.md) és hello Data Access API számára.</span><span class="sxs-lookup"><span data-stu-id="473ba-205">**API Access** - Create [release annotations](app-insights-annotations.md) and for hello Data Access API.</span></span>
* <span data-ttu-id="473ba-206">[**Munkaelemek** ](app-insights-diagnostic-search.md#create-work-item) -nyomkövetési rendszer, így Ön hibák telemetriai vizsgálatakor tooa munkahelyi csatlakozás.</span><span class="sxs-lookup"><span data-stu-id="473ba-206">[**Work Items**](app-insights-diagnostic-search.md#create-work-item) - Connect tooa work tracking system so that you can create bugs while inspecting telemetry.</span></span>

<span data-ttu-id="473ba-207">BEÁLLÍTÁSOK</span><span class="sxs-lookup"><span data-stu-id="473ba-207">SETTINGS</span></span>

* <span data-ttu-id="473ba-208">[**Zárolja** ](../azure-resource-manager/resource-group-lock-resources.md) -Azure-erőforrások zárolása</span><span class="sxs-lookup"><span data-stu-id="473ba-208">[**Locks**](../azure-resource-manager/resource-group-lock-resources.md) - lock Azure resources</span></span>
* <span data-ttu-id="473ba-209">[**Automatizálási parancsfájl** ](app-insights-powershell.md) -exportálása hello Azure-erőforrás meghatározását, hogy a sablon toocreate új erőforrásként használja.</span><span class="sxs-lookup"><span data-stu-id="473ba-209">[**Automation script**](app-insights-powershell.md) - export a definition of hello Azure resource so that you can use it as a template toocreate new resources.</span></span>


## <a name="video"></a><span data-ttu-id="473ba-210">Videó</span><span class="sxs-lookup"><span data-stu-id="473ba-210">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="473ba-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="473ba-211">Next steps</span></span>

|  |  |
| --- | --- |
| [<span data-ttu-id="473ba-212">Metrikaböngésző</span><span class="sxs-lookup"><span data-stu-id="473ba-212">Metrics explorer</span></span>](app-insights-metrics-explorer.md)<br/><span data-ttu-id="473ba-213">Szűrő és a szegmens metrikák</span><span class="sxs-lookup"><span data-stu-id="473ba-213">Filter and segment metrics</span></span> |![Keresés – példa](./media/app-insights-dashboards/64.png) |
| [<span data-ttu-id="473ba-215">Diagnosztikai keresése</span><span class="sxs-lookup"><span data-stu-id="473ba-215">Diagnostic search</span></span>](app-insights-diagnostic-search.md)<br/><span data-ttu-id="473ba-216">Található és vizsgálja meg az események, kapcsolódó események és hibák létrehozása</span><span class="sxs-lookup"><span data-stu-id="473ba-216">Find and inspect events, related events, and create bugs</span></span> |![Keresés – példa](./media/app-insights-dashboards/61.png) |
| [<span data-ttu-id="473ba-218">Elemzés</span><span class="sxs-lookup"><span data-stu-id="473ba-218">Analytics</span></span>](app-insights-analytics.md)<br/><span data-ttu-id="473ba-219">Hatékony lekérdezési nyelv</span><span class="sxs-lookup"><span data-stu-id="473ba-219">Powerful query language</span></span> |![Keresés – példa](./media/app-insights-dashboards/63.png) |

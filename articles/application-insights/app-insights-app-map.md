---
title: "Azure Application Insights térképre aaaApplication |} Microsoft Docs"
description: "A vizuális megjelenítését hello alkalmazások összetevői között fennálló feliratú KPI-k és riasztásokkal."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 96ab753a100ea53ec7d367e3559b6622ab6fd182
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-map-in-application-insights"></a><span data-ttu-id="7f472-103">Az Application Insightsban alkalmazás-hozzárendelés</span><span class="sxs-lookup"><span data-stu-id="7f472-103">Application Map in Application Insights</span></span>
<span data-ttu-id="7f472-104">A [Azure Application Insights](app-insights-overview.md), alkalmazás-hozzárendelés egy elrendezésének hello függőségi viszonyok az alkalmazás-összetevő.</span><span class="sxs-lookup"><span data-stu-id="7f472-104">In [Azure Application Insights](app-insights-overview.md), Application Map is a visual layout of hello dependency relationships of your application components.</span></span> <span data-ttu-id="7f472-105">Minden egyes összetevő látható KPI-k toohelp terhelés, teljesítmény, hibák és figyelmeztetések, például azt tapasztalja, hogy valamelyik összetevő a teljesítményprobléma vagy hiba okozza.</span><span class="sxs-lookup"><span data-stu-id="7f472-105">Each component shows KPIs such as load, performance, failures, and alerts, toohelp you discover any component causing a performance issue or failure.</span></span> <span data-ttu-id="7f472-106">Kattinthat a minden összetevő toomore keresztül részletes diagnosztikai, például az Application Insights események.</span><span class="sxs-lookup"><span data-stu-id="7f472-106">You can click through from any component toomore detailed diagnostics, such as Application Insights events.</span></span> <span data-ttu-id="7f472-107">Ha az alkalmazás Azure-szolgáltatásokat használja, is kattinthat tooAzure diagnosztika, például az SQL Database Advisor-javaslatokra keresztül.</span><span class="sxs-lookup"><span data-stu-id="7f472-107">If your app uses Azure services, you can also click through tooAzure diagnostics, such as SQL Database Advisor recommendations.</span></span>

<span data-ttu-id="7f472-108">Más típusú diagramokkal, például PIN-kód egy alkalmazás térkép toohello Azure irányítópult, amelyen is teljes körűen használható.</span><span class="sxs-lookup"><span data-stu-id="7f472-108">Like other charts, you can pin an application map toohello Azure dashboard, where it is fully functional.</span></span> 

## <a name="open-hello-application-map"></a><span data-ttu-id="7f472-109">Nyissa meg hello alkalmazás-hozzárendelés</span><span class="sxs-lookup"><span data-stu-id="7f472-109">Open hello application map</span></span>
<span data-ttu-id="7f472-110">Nyissa meg hello térkép hello áttekintése paneljén az alkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="7f472-110">Open hello map from hello overview blade for your application:</span></span>

![Nyissa meg az alkalmazás-leképezés](./media/app-insights-app-map/01.png)

![alkalmazás-leképezés](./media/app-insights-app-map/02.png)

<span data-ttu-id="7f472-113">hello térkép mutatja:</span><span class="sxs-lookup"><span data-stu-id="7f472-113">hello map shows:</span></span>

* <span data-ttu-id="7f472-114">Rendelkezésre állási tesztek</span><span class="sxs-lookup"><span data-stu-id="7f472-114">Availability tests</span></span>
* <span data-ttu-id="7f472-115">Ügyféloldali összetevő (JavaScript SDK hello figyelés)</span><span class="sxs-lookup"><span data-stu-id="7f472-115">Client-side component (monitored with hello JavaScript SDK)</span></span>
* <span data-ttu-id="7f472-116">Kiszolgálóoldali összetevő</span><span class="sxs-lookup"><span data-stu-id="7f472-116">Server-side component</span></span>
* <span data-ttu-id="7f472-117">Függőségek hello ügyfél és kiszolgáló-összetevők</span><span class="sxs-lookup"><span data-stu-id="7f472-117">Dependencies of hello client and server components</span></span>

<span data-ttu-id="7f472-118">Bontsa ki, és a függőségi hivatkozás elemcsoportok:</span><span class="sxs-lookup"><span data-stu-id="7f472-118">You can expand and collapse dependency link groups:</span></span>

![összecsukása](./media/app-insights-app-map/03.png)

<span data-ttu-id="7f472-120">Ha egy típust (SQL, HTTP stb.) a számos függőségi, jelennek meg a csoportosított.</span><span class="sxs-lookup"><span data-stu-id="7f472-120">If you have many dependencies of one type (SQL, HTTP etc.), they may appear grouped.</span></span> 

![a csoportosított függőségek](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a><span data-ttu-id="7f472-122">Problémák</span><span class="sxs-lookup"><span data-stu-id="7f472-122">Spot problems</span></span>
<span data-ttu-id="7f472-123">Minden csomópont vonatkozó teljesítménymutatói, többek között az adott összetevő a terhelés, teljesítmény és hiba díjszabás hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7f472-123">Each node has relevant performance indicators, such as hello load, performance, and failure rates for that component.</span></span> 

<span data-ttu-id="7f472-124">Figyelmeztetés ikon jelölje ki a lehetséges problémákat.</span><span class="sxs-lookup"><span data-stu-id="7f472-124">Warning icons highlight possible problems.</span></span> <span data-ttu-id="7f472-125">Egy narancssárga figyelmeztetést azt jelenti, hogy nincsenek sikertelen kérelmek Lapmegtekintések vagy függőségi hívások esetében.</span><span class="sxs-lookup"><span data-stu-id="7f472-125">An orange warning means there are failures in requests, page views or dependency calls.</span></span> <span data-ttu-id="7f472-126">Piros azt jelenti, hogy egy több mint 5 % hibaaránya.</span><span class="sxs-lookup"><span data-stu-id="7f472-126">Red means a failure rate above 5%.</span></span> <span data-ttu-id="7f472-127">Ha azt szeretné, tooadjust ezeket a küszöbértékeket, nyissa meg a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="7f472-127">If you want tooadjust these thresholds, open Options.</span></span>

![Hiba ikonok](./media/app-insights-app-map/04.png)

<span data-ttu-id="7f472-129">Aktív riasztásainak is megjelenítése fel:</span><span class="sxs-lookup"><span data-stu-id="7f472-129">Active alerts also show up:</span></span> 

![aktív riasztások](./media/app-insights-app-map/05.png)

<span data-ttu-id="7f472-131">SQL Azure használ, nincs-e egy ikon, amely mutatja, hogy mikor van kapcsolatos javaslatok hogyan javíthatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="7f472-131">If you use SQL Azure, there's an icon that shows when there are recommendations on how you can improve performance.</span></span> 

![az Azure javaslat](./media/app-insights-app-map/06.png)

<span data-ttu-id="7f472-133">Kattintson a bármely ikon tooget további részletek:</span><span class="sxs-lookup"><span data-stu-id="7f472-133">Click any icon tooget more details:</span></span>

![az Azure javaslat](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a><span data-ttu-id="7f472-135">Diagnosztikai kattintson keresztül</span><span class="sxs-lookup"><span data-stu-id="7f472-135">Diagnostic click through</span></span>
<span data-ttu-id="7f472-136">Hello térképen hello csomópontok mindegyikének kínál a célzott kattintson ide a diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="7f472-136">Each of hello nodes on hello map offers targeted click through for diagnostics.</span></span> <span data-ttu-id="7f472-137">hello beállítások hello csomópont hello típusától függően változhatnak.</span><span class="sxs-lookup"><span data-stu-id="7f472-137">hello options vary depending on hello type of hello node.</span></span>

![kiszolgáló beállításai](./media/app-insights-app-map/09.png)

<span data-ttu-id="7f472-139">Az Azure-ban szolgáltatott összetevők hello lehetőségek között a közvetlen hivatkozások toothem.</span><span class="sxs-lookup"><span data-stu-id="7f472-139">For components that are hosted in Azure, hello options include direct links toothem.</span></span>

## <a name="filters-and-time-range"></a><span data-ttu-id="7f472-140">Szűrők és időtartomány</span><span class="sxs-lookup"><span data-stu-id="7f472-140">Filters and time range</span></span>
<span data-ttu-id="7f472-141">Alapértelmezés szerint a hello térkép összes hello érhető el adat a kiválasztott időtartomány hello foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="7f472-141">By default, hello map summarizes all hello data available for hello chosen time range.</span></span> <span data-ttu-id="7f472-142">De tooinclude csak bizonyos műveletek nevének vagy függőségek szűrheti.</span><span class="sxs-lookup"><span data-stu-id="7f472-142">But you can filter it tooinclude only specific operation names or dependencies.</span></span>

* <span data-ttu-id="7f472-143">A művelet neve: Ide tartoznak a lapmegtekintések, és kiszolgálóoldali kéréstípusok.</span><span class="sxs-lookup"><span data-stu-id="7f472-143">Operation name: This includes both page views and server-side request types.</span></span> <span data-ttu-id="7f472-144">Ezzel a lehetőséggel hello térképen jeleníti meg hello KPI hello kiszolgáló-vagy ügyféloldali csomópont csak a kiválasztott hello műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="7f472-144">With this option, hello map shows hello KPI on hello server/client-side node for hello selected operations only.</span></span> <span data-ttu-id="7f472-145">Azt illusztrálja, hello függőségek hívása a műveletek adott hello kontextusban.</span><span class="sxs-lookup"><span data-stu-id="7f472-145">It shows hello dependencies called in hello context of those specific operations.</span></span>
* <span data-ttu-id="7f472-146">Függőség alapnévvel: Ez magában foglalja a hello AJAX böngésző és kiszolgálóoldali függőségei is.</span><span class="sxs-lookup"><span data-stu-id="7f472-146">Dependency base name: This includes hello AJAX browser dependencies and server-side dependencies.</span></span> <span data-ttu-id="7f472-147">Ha egyéni függőségi telemetria hello TrackDependency API a jelentéssel is szerepelnek itt.</span><span class="sxs-lookup"><span data-stu-id="7f472-147">If you report custom dependency telemetry with hello TrackDependency API, they also appear here.</span></span> <span data-ttu-id="7f472-148">Hello függőségek tooshow hello térképen választhatja ki.</span><span class="sxs-lookup"><span data-stu-id="7f472-148">You can select hello dependencies tooshow on hello map.</span></span> <span data-ttu-id="7f472-149">Ez a beállítás jelenleg nem végez kiszolgálóoldali kérelmek hello vagy hello ügyféloldali Lapmegtekintések.</span><span class="sxs-lookup"><span data-stu-id="7f472-149">Currently this selection does not filter hello server-side requests, or hello client-side page views.</span></span>

![Szűrők beállítása](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a><span data-ttu-id="7f472-151">Szűrők mentése</span><span class="sxs-lookup"><span data-stu-id="7f472-151">Save filters</span></span>
<span data-ttu-id="7f472-152">toosave hello szűrőt, a PIN-kód hello szűrt nézet alakzatot egy [irányítópult](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="7f472-152">toosave hello filters you have applied, pin hello filtered view onto a [dashboard](app-insights-dashboards.md).</span></span>

![PIN-kód toodashboard](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a><span data-ttu-id="7f472-154">Hiba ablaktábla</span><span class="sxs-lookup"><span data-stu-id="7f472-154">Error pane</span></span>
<span data-ttu-id="7f472-155">Ha egy csomópont hello leképezés gombra kattint, egy hiba ablaktáblán hello jobb oldali az adott csomópont hibák összefoglalása jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7f472-155">When you click a node in hello map, an error pane is displayed on hello right-hand side summarizing failures for that node.</span></span> <span data-ttu-id="7f472-156">Hibák először Műveletazonosító szerint csoportosítva, és ezután csoportosítva probléma azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="7f472-156">Failures are grouped first by operation ID and then grouped by problem ID.</span></span>

![Hiba ablaktábla](./media/app-insights-app-map/error-pane.png)

<span data-ttu-id="7f472-158">Kattintson a hiba viszi toohello legutóbbi példányát, hogy a hibát.</span><span class="sxs-lookup"><span data-stu-id="7f472-158">Clicking on a failure takes you toohello most recent instance of that failure.</span></span>

## <a name="resource-health"></a><span data-ttu-id="7f472-159">Erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="7f472-159">Resource health</span></span>
<span data-ttu-id="7f472-160">Az egyes erőforrástípusok erőforrás állapota hello hello hiba ablaktábla tetején jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7f472-160">For some resource types, resource health is displayed at hello top of hello error pane.</span></span> <span data-ttu-id="7f472-161">Például egy SQL-csomópontra kattintva megjelenik hello adatbázis állapotának és az riasztások kiváltó rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7f472-161">For example, clicking a SQL node will show hello database health and any alerts that have fired.</span></span>

![Erőforrás állapota](./media/app-insights-app-map/resource-health.png)

<span data-ttu-id="7f472-163">Hello erőforrás neve tooview szabványos áttekintése metrikák az adott erőforrás gombra.</span><span class="sxs-lookup"><span data-stu-id="7f472-163">You can click hello resource name tooview standard overview metrics for that resource.</span></span>

## <a name="end-to-end-system-app-maps"></a><span data-ttu-id="7f472-164">Végpontok közötti rendszer app maps</span><span class="sxs-lookup"><span data-stu-id="7f472-164">End-to-end system app maps</span></span>

<span data-ttu-id="7f472-165">*SDK 2.3-as vagy újabb verziója szükséges*</span><span class="sxs-lookup"><span data-stu-id="7f472-165">*Requires SDK version 2.3 or higher*</span></span>

<span data-ttu-id="7f472-166">Több összetevőt - e az alkalmazás például egy háttér-szolgáltatás továbbá akkor toohello webalkalmazás - is jeleníti meg az összes egy integrált alkalmazás térkép őket.</span><span class="sxs-lookup"><span data-stu-id="7f472-166">If your application has several components - for example, a back-end service in addition toohello web app - then you can show them all on one integrated app map.</span></span>

![Szűrők beállítása](./media/app-insights-app-map/multi-component-app-map.png)

<span data-ttu-id="7f472-168">hello app térkép csomópontok megkeresi a következő bármely HTTP függőségi hívások az Application Insights SDK telepítve hello kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="7f472-168">hello app map finds server nodes by following any HTTP dependency calls made between servers with hello Application Insights SDK installed.</span></span> <span data-ttu-id="7f472-169">Minden egyes Application Insights-erőforrás feltételezett toocontain egy kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="7f472-169">Each Application Insights resource is assumed toocontain one server.</span></span>

### <a name="multi-role-app-map-preview"></a><span data-ttu-id="7f472-170">Több szerepkör app térkép (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="7f472-170">Multi-role app map (preview)</span></span>

<span data-ttu-id="7f472-171">hello preview több szerepkör app leképezés funkció lehetővé teszi toouse hello app térkép adatok toohello küldése több kiszolgáló ugyanazon az Application Insights-erőforrás / instrumentation kulcs.</span><span class="sxs-lookup"><span data-stu-id="7f472-171">hello preview multi-role app map feature allows you toouse hello app map with multiple servers sending data toohello same Application Insights resource  / instrumentation key.</span></span> <span data-ttu-id="7f472-172">Hello leképezés kiszolgálók vannak szegmentált hello cloud_RoleName tulajdonság telemetriai elemekre.</span><span class="sxs-lookup"><span data-stu-id="7f472-172">Servers in hello map are segmented by hello cloud_RoleName property on telemetry items.</span></span> <span data-ttu-id="7f472-173">Állítsa be *alkalmazás több szerepkör-hozzárendelés* túl*a* a hello az előzetes verziójú funkciók panel tooenable ezt a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="7f472-173">Set *Multi-role Application Map* too*On* from hello Previews blade tooenable this configuration.</span></span>

<span data-ttu-id="7f472-174">Ez a megközelítés kívánatos lehet egy micro-services-alkalmazás, illetve egyéb forgatókönyvek, ahová toocorrelate események egyetlen Application Insights-erőforrás belül több kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="7f472-174">This approach may be desired in a micro-services application, or in other scenarios where you want toocorrelate events across multiple servers within a single Application Insights resource.</span></span>

## <a name="video"></a><span data-ttu-id="7f472-175">Videó</span><span class="sxs-lookup"><span data-stu-id="7f472-175">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a><span data-ttu-id="7f472-176">Visszajelzés</span><span class="sxs-lookup"><span data-stu-id="7f472-176">Feedback</span></span>
<span data-ttu-id="7f472-177">Adjon visszajelzést hello portál visszajelzési lehetőség használatával.</span><span class="sxs-lookup"><span data-stu-id="7f472-177">Please provide feedback through hello portal feedback option.</span></span>

![MapLink-1 kép](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a><span data-ttu-id="7f472-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f472-179">Next steps</span></span>

* [<span data-ttu-id="7f472-180">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7f472-180">Azure portal</span></span>](https://portal.azure.com)

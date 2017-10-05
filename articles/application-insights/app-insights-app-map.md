---
title: "Alkalmazás-hozzárendelés Azure Application insightsban |} Microsoft Docs"
description: "Az alkalmazások összetevői között fennálló vizuális megjelenítését feliratú KPI-k és riasztásokkal."
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
ms.openlocfilehash: 207526b7a675f92134d045ebefb9a372749bce92
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="application-map-in-application-insights"></a><span data-ttu-id="09edc-103">Az Application Insightsban alkalmazás-hozzárendelés</span><span class="sxs-lookup"><span data-stu-id="09edc-103">Application Map in Application Insights</span></span>
<span data-ttu-id="09edc-104">A [Azure Application Insights](app-insights-overview.md), alkalmazás-hozzárendelés az alkalmazás-összetevői közötti függőségi kapcsolatokat visual elrendezés.</span><span class="sxs-lookup"><span data-stu-id="09edc-104">In [Azure Application Insights](app-insights-overview.md), Application Map is a visual layout of the dependency relationships of your application components.</span></span> <span data-ttu-id="09edc-105">Minden egyes összetevő KPI-k például a terhelés, teljesítmény, hibák és figyelmeztetések, segítségével megkeresheti az adott teljesítményprobléma vagy hiba, amely összetevők közül bármelyik jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="09edc-105">Each component shows KPIs such as load, performance, failures, and alerts, to help you discover any component causing a performance issue or failure.</span></span> <span data-ttu-id="09edc-106">Kattintva keresztül valamelyik összetevő a részletesebb diagnosztikai, például az Application Insights események.</span><span class="sxs-lookup"><span data-stu-id="09edc-106">You can click through from any component to more detailed diagnostics, such as Application Insights events.</span></span> <span data-ttu-id="09edc-107">Ha az alkalmazás Azure-szolgáltatásokat használja, akkor is átkattintással is Azure Diagnostics, például az SQL Database Advisor-javaslatokra.</span><span class="sxs-lookup"><span data-stu-id="09edc-107">If your app uses Azure services, you can also click through to Azure diagnostics, such as SQL Database Advisor recommendations.</span></span>

<span data-ttu-id="09edc-108">Más típusú diagramokkal, például PIN-kód egy alkalmazás-hozzárendelés az Azure irányítópultot, ahol is teljes körűen használható.</span><span class="sxs-lookup"><span data-stu-id="09edc-108">Like other charts, you can pin an application map to the Azure dashboard, where it is fully functional.</span></span> 

## <a name="open-the-application-map"></a><span data-ttu-id="09edc-109">Nyissa meg az alkalmazás-hozzárendelés</span><span class="sxs-lookup"><span data-stu-id="09edc-109">Open the application map</span></span>
<span data-ttu-id="09edc-110">Nyissa meg a térképen az alkalmazáshoz – Áttekintés paneljéről:</span><span class="sxs-lookup"><span data-stu-id="09edc-110">Open the map from the overview blade for your application:</span></span>

![Nyissa meg az alkalmazás-leképezés](./media/app-insights-app-map/01.png)

![alkalmazás-leképezés](./media/app-insights-app-map/02.png)

<span data-ttu-id="09edc-113">A térkép mutatja:</span><span class="sxs-lookup"><span data-stu-id="09edc-113">The map shows:</span></span>

* <span data-ttu-id="09edc-114">Rendelkezésre állási tesztek</span><span class="sxs-lookup"><span data-stu-id="09edc-114">Availability tests</span></span>
* <span data-ttu-id="09edc-115">Ügyféloldali összetevő (figyeli a JavaScript SDK-val)</span><span class="sxs-lookup"><span data-stu-id="09edc-115">Client-side component (monitored with the JavaScript SDK)</span></span>
* <span data-ttu-id="09edc-116">Kiszolgálóoldali összetevő</span><span class="sxs-lookup"><span data-stu-id="09edc-116">Server-side component</span></span>
* <span data-ttu-id="09edc-117">Az ügyfél és kiszolgáló összetevők függőségei</span><span class="sxs-lookup"><span data-stu-id="09edc-117">Dependencies of the client and server components</span></span>

<span data-ttu-id="09edc-118">Bontsa ki, és a függőségi hivatkozás elemcsoportok:</span><span class="sxs-lookup"><span data-stu-id="09edc-118">You can expand and collapse dependency link groups:</span></span>

![összecsukása](./media/app-insights-app-map/03.png)

<span data-ttu-id="09edc-120">Ha egy típust (SQL, HTTP stb.) a számos függőségi, jelennek meg a csoportosított.</span><span class="sxs-lookup"><span data-stu-id="09edc-120">If you have many dependencies of one type (SQL, HTTP etc.), they may appear grouped.</span></span> 

![a csoportosított függőségek](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a><span data-ttu-id="09edc-122">Problémák</span><span class="sxs-lookup"><span data-stu-id="09edc-122">Spot problems</span></span>
<span data-ttu-id="09edc-123">Minden fürtcsomópont vonatkozó teljesítménymutatói, többek között a terhelés, teljesítmény és hiba sebesség az adott összetevő.</span><span class="sxs-lookup"><span data-stu-id="09edc-123">Each node has relevant performance indicators, such as the load, performance, and failure rates for that component.</span></span> 

<span data-ttu-id="09edc-124">Figyelmeztetés ikon jelölje ki a lehetséges problémákat.</span><span class="sxs-lookup"><span data-stu-id="09edc-124">Warning icons highlight possible problems.</span></span> <span data-ttu-id="09edc-125">Egy narancssárga figyelmeztetést azt jelenti, hogy nincsenek sikertelen kérelmek Lapmegtekintések vagy függőségi hívások esetében.</span><span class="sxs-lookup"><span data-stu-id="09edc-125">An orange warning means there are failures in requests, page views or dependency calls.</span></span> <span data-ttu-id="09edc-126">Piros azt jelenti, hogy egy több mint 5 % hibaaránya.</span><span class="sxs-lookup"><span data-stu-id="09edc-126">Red means a failure rate above 5%.</span></span> <span data-ttu-id="09edc-127">Ha azt szeretné, hogy lehetőség ezen küszöbértékek módosítására, nyissa meg a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="09edc-127">If you want to adjust these thresholds, open Options.</span></span>

![Hiba ikonok](./media/app-insights-app-map/04.png)

<span data-ttu-id="09edc-129">Aktív riasztásainak is megjelenítése fel:</span><span class="sxs-lookup"><span data-stu-id="09edc-129">Active alerts also show up:</span></span> 

![aktív riasztások](./media/app-insights-app-map/05.png)

<span data-ttu-id="09edc-131">SQL Azure használ, nincs-e egy ikon, amely mutatja, hogy mikor van kapcsolatos javaslatok hogyan javíthatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="09edc-131">If you use SQL Azure, there's an icon that shows when there are recommendations on how you can improve performance.</span></span> 

![az Azure javaslat](./media/app-insights-app-map/06.png)

<span data-ttu-id="09edc-133">Kattintson a bármely ikonra a további részletek:</span><span class="sxs-lookup"><span data-stu-id="09edc-133">Click any icon to get more details:</span></span>

![az Azure javaslat](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a><span data-ttu-id="09edc-135">Diagnosztikai kattintson keresztül</span><span class="sxs-lookup"><span data-stu-id="09edc-135">Diagnostic click through</span></span>
<span data-ttu-id="09edc-136">A csomópontok a térképen kínál a célzott kattintson ide a diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="09edc-136">Each of the nodes on the map offers targeted click through for diagnostics.</span></span> <span data-ttu-id="09edc-137">A beállítások a csomópont típusától függően eltérnek.</span><span class="sxs-lookup"><span data-stu-id="09edc-137">The options vary depending on the type of the node.</span></span>

![kiszolgáló beállításai](./media/app-insights-app-map/09.png)

<span data-ttu-id="09edc-139">Az Azure-ban szolgáltatott összetevők a választható lehetőségek őket mutató közvetlen hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="09edc-139">For components that are hosted in Azure, the options include direct links to them.</span></span>

## <a name="filters-and-time-range"></a><span data-ttu-id="09edc-140">Szűrők és időtartomány</span><span class="sxs-lookup"><span data-stu-id="09edc-140">Filters and time range</span></span>
<span data-ttu-id="09edc-141">Alapértelmezés szerint a térkép összes érhető el a kiválasztott időtartomány adatok összegzése látható.</span><span class="sxs-lookup"><span data-stu-id="09edc-141">By default, the map summarizes all the data available for the chosen time range.</span></span> <span data-ttu-id="09edc-142">De szűrheti is, hogy csak a konkrét műveletek nevének és a függőségek.</span><span class="sxs-lookup"><span data-stu-id="09edc-142">But you can filter it to include only specific operation names or dependencies.</span></span>

* <span data-ttu-id="09edc-143">A művelet neve: Ide tartoznak a lapmegtekintések, és kiszolgálóoldali kéréstípusok.</span><span class="sxs-lookup"><span data-stu-id="09edc-143">Operation name: This includes both page views and server-side request types.</span></span> <span data-ttu-id="09edc-144">Ezzel a beállítással a térkép a kiszolgáló-vagy ügyféloldali csomóponton csak a kijelölt műveletek a fő Teljesítménymutatói jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="09edc-144">With this option, the map shows the KPI on the server/client-side node for the selected operations only.</span></span> <span data-ttu-id="09edc-145">Azt mutatja, hogy a függőségek hívása a műveletek adott kontextusban.</span><span class="sxs-lookup"><span data-stu-id="09edc-145">It shows the dependencies called in the context of those specific operations.</span></span>
* <span data-ttu-id="09edc-146">Függőség alapnévvel: Ez magában foglalja a AJAX böngésző és kiszolgálóoldali függőségei is.</span><span class="sxs-lookup"><span data-stu-id="09edc-146">Dependency base name: This includes the AJAX browser dependencies and server-side dependencies.</span></span> <span data-ttu-id="09edc-147">Ha készít jelentést az egyéni függőségi telemetria TrackDependency API-val, is szerepelnek itt.</span><span class="sxs-lookup"><span data-stu-id="09edc-147">If you report custom dependency telemetry with the TrackDependency API, they also appear here.</span></span> <span data-ttu-id="09edc-148">Kiválaszthatja a függőségeket a térképen megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="09edc-148">You can select the dependencies to show on the map.</span></span> <span data-ttu-id="09edc-149">Ez a beállítás jelenleg nem végez a kiszolgálóoldali kérelmeket, vagy az ügyféloldali Lapmegtekintések.</span><span class="sxs-lookup"><span data-stu-id="09edc-149">Currently this selection does not filter the server-side requests, or the client-side page views.</span></span>

![Szűrők beállítása](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a><span data-ttu-id="09edc-151">Szűrők mentése</span><span class="sxs-lookup"><span data-stu-id="09edc-151">Save filters</span></span>
<span data-ttu-id="09edc-152">Szeretné menteni a szűrőt, PIN-kód alakzatot szűrt nézet egy [irányítópult](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="09edc-152">To save the filters you have applied, pin the filtered view onto a [dashboard](app-insights-dashboards.md).</span></span>

![Rögzítés az irányítópulton](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a><span data-ttu-id="09edc-154">Hiba ablaktábla</span><span class="sxs-lookup"><span data-stu-id="09edc-154">Error pane</span></span>
<span data-ttu-id="09edc-155">Ha a leképezés egy csomópont gombra kattint, egy hiba ablaktáblán a jobb oldalon, akiknél probléma van a csomópont összefoglalójához jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="09edc-155">When you click a node in the map, an error pane is displayed on the right-hand side summarizing failures for that node.</span></span> <span data-ttu-id="09edc-156">Hibák először Műveletazonosító szerint csoportosítva, és ezután csoportosítva probléma azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="09edc-156">Failures are grouped first by operation ID and then grouped by problem ID.</span></span>

![Hiba ablaktábla](./media/app-insights-app-map/error-pane.png)

<span data-ttu-id="09edc-158">Kattintson a hiba vesz igénybe, hogy az adott hiba utolsó példányát.</span><span class="sxs-lookup"><span data-stu-id="09edc-158">Clicking on a failure takes you to the most recent instance of that failure.</span></span>

## <a name="resource-health"></a><span data-ttu-id="09edc-159">Erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="09edc-159">Resource health</span></span>
<span data-ttu-id="09edc-160">Az egyes erőforrástípusok erőforrás állapota a hiba ablaktábla tetején jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="09edc-160">For some resource types, resource health is displayed at the top of the error pane.</span></span> <span data-ttu-id="09edc-161">Például egy SQL-csomópontra kattintva jelennek meg az adatbázis állapotának és az riasztások kiváltó rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="09edc-161">For example, clicking a SQL node will show the database health and any alerts that have fired.</span></span>

![Erőforrás állapota](./media/app-insights-app-map/resource-health.png)

<span data-ttu-id="09edc-163">Az erőforrás nevét, az adott erőforrás szabványos áttekintése metrikák megtekintéséhez rákattinthat.</span><span class="sxs-lookup"><span data-stu-id="09edc-163">You can click the resource name to view standard overview metrics for that resource.</span></span>

## <a name="end-to-end-system-app-maps"></a><span data-ttu-id="09edc-164">Végpontok közötti rendszer app maps</span><span class="sxs-lookup"><span data-stu-id="09edc-164">End-to-end system app maps</span></span>

<span data-ttu-id="09edc-165">*SDK 2.3-as vagy újabb verziója szükséges*</span><span class="sxs-lookup"><span data-stu-id="09edc-165">*Requires SDK version 2.3 or higher*</span></span>

<span data-ttu-id="09edc-166">Ha az alkalmazás több részből áll – például egy háttér-szolgáltatás emellett a webes alkalmazás -, akkor is megjeleníthetők az összes egy integrált alkalmazás térképen.</span><span class="sxs-lookup"><span data-stu-id="09edc-166">If your application has several components - for example, a back-end service in addition to the web app - then you can show them all on one integrated app map.</span></span>

![Szűrők beállítása](./media/app-insights-app-map/multi-component-app-map.png)

<span data-ttu-id="09edc-168">Az alkalmazás térkép csomópontok bármely HTTP függőségi hívások esetében az Application Insights SDK telepítve a kiszolgálók közötti következő talál.</span><span class="sxs-lookup"><span data-stu-id="09edc-168">The app map finds server nodes by following any HTTP dependency calls made between servers with the Application Insights SDK installed.</span></span> <span data-ttu-id="09edc-169">Minden egyes Application Insights-erőforrás feltételezett, hogy egy kiszolgálót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="09edc-169">Each Application Insights resource is assumed to contain one server.</span></span>

### <a name="multi-role-app-map-preview"></a><span data-ttu-id="09edc-170">Több szerepkör app térkép (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="09edc-170">Multi-role app map (preview)</span></span>

<span data-ttu-id="09edc-171">Több szerepkör app térkép előnézet lehetővé teszi, hogy az alkalmazás térkép használja az adatok küldése az Application Insights-erőforrások több kiszolgáló / instrumentation kulcs.</span><span class="sxs-lookup"><span data-stu-id="09edc-171">The preview multi-role app map feature allows you to use the app map with multiple servers sending data to the same Application Insights resource  / instrumentation key.</span></span> <span data-ttu-id="09edc-172">A térkép kiszolgálók vannak szegmentált telemetriai elemek cloud_RoleName tulajdonság által.</span><span class="sxs-lookup"><span data-stu-id="09edc-172">Servers in the map are segmented by the cloud_RoleName property on telemetry items.</span></span> <span data-ttu-id="09edc-173">Állítsa be *alkalmazás több szerepkör-hozzárendelés* való *a* ahhoz, hogy ez a konfiguráció az előzetes verziójú funkciók paneljén.</span><span class="sxs-lookup"><span data-stu-id="09edc-173">Set *Multi-role Application Map* to *On* from the Previews blade to enable this configuration.</span></span>

<span data-ttu-id="09edc-174">Ez a megközelítés kívánatos lehet egy micro-szolgáltatások alkalmazás, vagy a más helyzetekben, ahol szeretné események összefüggéseket egyetlen Application Insights-erőforrás belül több kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="09edc-174">This approach may be desired in a micro-services application, or in other scenarios where you want to correlate events across multiple servers within a single Application Insights resource.</span></span>

## <a name="video"></a><span data-ttu-id="09edc-175">Videó</span><span class="sxs-lookup"><span data-stu-id="09edc-175">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a><span data-ttu-id="09edc-176">Visszajelzés</span><span class="sxs-lookup"><span data-stu-id="09edc-176">Feedback</span></span>
<span data-ttu-id="09edc-177">Adja meg a portál visszajelzési lehetőség visszajelzései.</span><span class="sxs-lookup"><span data-stu-id="09edc-177">Please provide feedback through the portal feedback option.</span></span>

![MapLink-1 kép](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a><span data-ttu-id="09edc-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09edc-179">Next steps</span></span>

* [<span data-ttu-id="09edc-180">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="09edc-180">Azure portal</span></span>](https://portal.azure.com)

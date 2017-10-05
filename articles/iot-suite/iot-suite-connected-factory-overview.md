---
title: "Az Azure IoT Suite csatlakoztatott gyár áttekintése | Microsoft Docs"
description: "Az Azure IoT Suite előre konfigurált csatlakoztatott gyár megoldásának ismertetése."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: d502c8e2e2715899279f6ebcf7ed89c19a1bb9a6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-the-connected-factory-preconfigured-solution"></a><span data-ttu-id="97030-103">Az előre konfigurált csatlakoztatott gyár első lépései</span><span class="sxs-lookup"><span data-stu-id="97030-103">Get started with the connected factory preconfigured solution</span></span>

<span data-ttu-id="97030-104">Az Azure IoT Suite [előre konfigurált megoldások][lnk-preconfigured-solutions] több Azure IoT-szolgáltatást kombinálnak, hogy általános IoT üzleti forgatókönyveket megvalósító végpontok közötti megoldásokat nyújtsanak.</span><span class="sxs-lookup"><span data-stu-id="97030-104">Azure IoT Suite [preconfigured solutions][lnk-preconfigured-solutions] combine multiple Azure IoT services to deliver end-to-end solutions that implement common IoT business scenarios.</span></span> <span data-ttu-id="97030-105">Az előre konfigurált *csatlakoztatott gyár* megoldás csatlakozik ipari eszközeihez, és megfigyeli azokat.</span><span class="sxs-lookup"><span data-stu-id="97030-105">The *connected factory* preconfigured solution connects to and monitors your industrial devices.</span></span> <span data-ttu-id="97030-106">Ez a megoldás az eszközökről származó adatstream elemzésére, ezáltal pedig az üzemeltetés hatékonyságának és nyereségességének növelésére használható.</span><span class="sxs-lookup"><span data-stu-id="97030-106">You can use the solution to analyze the stream of data from your devices and to drive operational productivity and profitability.</span></span>

<span data-ttu-id="97030-107">Az oktatóprogram bemutatja, hogyan építheti ki az előre konfigurált csatlakoztatott gyár megoldást.</span><span class="sxs-lookup"><span data-stu-id="97030-107">This tutorial shows you how to provision the connected factory preconfigured solution.</span></span> <span data-ttu-id="97030-108">Valamint az előre konfigurált megoldás alapvető funkcióin is végigvezeti.</span><span class="sxs-lookup"><span data-stu-id="97030-108">It also walks you through the basic features of the preconfigured solution.</span></span> <span data-ttu-id="97030-109">Ezek közül számos funkcióhoz a megoldás *irányítópultján* keresztül férhet hozzá, amelyet a rendszer az előre konfigurált megoldás részeként telepít:</span><span class="sxs-lookup"><span data-stu-id="97030-109">You can access many of these features from the solution *dashboard* that deploys as part of the preconfigured solution:</span></span>

![Az előre konfigurált csatlakoztatott gyár megoldás irányítópultja][img-cf-home]

<span data-ttu-id="97030-111">Az oktatóanyag elvégzéséhez aktív Azure-előfizetésre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="97030-111">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="97030-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="97030-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="97030-113">További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="97030-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
> 
> 

## <a name="provision-the-solution"></a><span data-ttu-id="97030-114">A megoldás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="97030-114">Provision the solution</span></span>

1. <span data-ttu-id="97030-115">Jelentkezzen be az azureiotsuite.com címre az Azure-fiókja hitelesítő adataival, majd kattintson a „**+**” elemre egy megoldás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="97030-115">Log on to azureiotsuite.com using your Azure account credentials, and click "**+**" to create a solution.</span></span>
2. <span data-ttu-id="97030-116">Kattintson a **Kiválasztás** elemre a **Csatlakoztatott gyár** csempén.</span><span class="sxs-lookup"><span data-stu-id="97030-116">Click **Select** on the **Connected factory** tile.</span></span>
3. <span data-ttu-id="97030-117">Adja meg a **Megoldásnevet** az előre konfigurált csatlakoztatott gyár megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="97030-117">Enter a **Solution name** for your connected factory preconfigured solution.</span></span>
4. <span data-ttu-id="97030-118">Válassza ki a megoldás kiépítéséhez használni kívánt **Előfizetést** és **Régiót**.</span><span class="sxs-lookup"><span data-stu-id="97030-118">Select the **Subscription** and **Region** you want to use to provision the solution.</span></span>
5. <span data-ttu-id="97030-119">Kattintson a **Megoldás létrehozása** gombra a kiépítés elkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="97030-119">Click **Create Solution** to begin the provisioning process.</span></span> <span data-ttu-id="97030-120">Ez a folyamat általában több percig tart.</span><span class="sxs-lookup"><span data-stu-id="97030-120">This process typically takes several minutes to run.</span></span>

### <a name="while-you-wait-for-the-provisioning-process-to-complete"></a><span data-ttu-id="97030-121">Amíg a kiépítési folyamat befejeződésére vár</span><span class="sxs-lookup"><span data-stu-id="97030-121">While you wait for the provisioning process to complete</span></span>

1. <span data-ttu-id="97030-122">Kattintson a megoldás **Kiépítési** állapotát jelző csempére.</span><span class="sxs-lookup"><span data-stu-id="97030-122">Click the tile for your solution with **Provisioning** status.</span></span>
2. <span data-ttu-id="97030-123">Megtekintheti a **Kiépítési állapotokat**, miközben az Azure-szolgáltatások telepítése megtörténik az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="97030-123">Notice the **Provisioning states** as Azure services are deployed in your Azure subscription.</span></span>
3. <span data-ttu-id="97030-124">A kiépítés befejezése után az állapot **Kész** értékre változik.</span><span class="sxs-lookup"><span data-stu-id="97030-124">Once provisioning completes, the status changes to **Ready**.</span></span>
4. <span data-ttu-id="97030-125">Kattintson a csempére, és a jobb oldali panelen láthatja a megoldás részleteit.</span><span class="sxs-lookup"><span data-stu-id="97030-125">Click the tile to see the details of your solution in the right-hand pane.</span></span>

> [!NOTE]
> <span data-ttu-id="97030-126">Ha problémái vannak az előre konfigurált megoldás telepítésekor, tekintse meg az [Engedélyek az azureiotsuite.com webhelyen][lnk-permissions] és a [Csatlakoztatott gyár – GYIK](iot-suite-faq-cf.md) fejezetet.</span><span class="sxs-lookup"><span data-stu-id="97030-126">If you encounter issues deploying the preconfigured solution, review [Permissions on the azureiotsuite.com site][lnk-permissions] and the [Connected factory FAQ](iot-suite-faq-cf.md).</span></span> <span data-ttu-id="97030-127">Ha a problémák továbbra is fennállnak, hozzon létre egy szolgáltatásjegyet a [portálon][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="97030-127">If the issues persist, create a service ticket on the [portal][lnk-portal].</span></span>

<span data-ttu-id="97030-128">Hiányol bizonyos részleteket a megoldásával kapcsolatban?</span><span class="sxs-lookup"><span data-stu-id="97030-128">Are there details you'd expect to see that aren't listed for your solution?</span></span> <span data-ttu-id="97030-129">A [felhasználói visszajelzési webhelyen](https://feedback.azure.com/forums/321918-azure-iot) elküldheti a szolgáltatásokkal kapcsolatos javaslatait.</span><span class="sxs-lookup"><span data-stu-id="97030-129">Give us feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="97030-130">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="97030-130">Scenario overview</span></span>

<span data-ttu-id="97030-131">Amikor üzembe helyezi az előre konfigurált csatlakoztatott gyár megoldást, az előre fel van töltve olyan erőforrásokkal, amelyekkel elvégezhető egy általános ipari forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="97030-131">When you deploy the connected factory preconfigured solution, it is prepopulated with resources that enable you to step through a common industrial scenario.</span></span> <span data-ttu-id="97030-132">Ebben a forgatókönyvben a megoldáshoz csatlakozó több üzem ad le jelentést a teljes eszközhatékonyság (overall equipment efficiency, OEE) és a fő teljesítménymutatók (KPI) kiszámításához szükséges adatértékeket.</span><span class="sxs-lookup"><span data-stu-id="97030-132">In this scenario, several factories connected to the solution report the data values required to compute overall equipment efficiency (OEE) and key performance indicators (KPIs).</span></span> <span data-ttu-id="97030-133">A következő forgatókönyvek bemutatják, hogyan végezheti el a következőket:</span><span class="sxs-lookup"><span data-stu-id="97030-133">The following sections show you how to:</span></span>

* <span data-ttu-id="97030-134">Gyárakra, gyártósorokra, állomásokra vonatkozó OEE- és KPI-értékek figyelése</span><span class="sxs-lookup"><span data-stu-id="97030-134">Monitor factory, production lines, station OEE, and KPI values</span></span>
* <span data-ttu-id="97030-135">Az eszközök által létrehozott telemetriaadatok elemzése az Azure Time Series Insights segítségével</span><span class="sxs-lookup"><span data-stu-id="97030-135">Analyze the telemetry data generated from these devices using Azure Time Series Insights</span></span>
* <span data-ttu-id="97030-136">Reagálás a riasztásokra a problémák felszámolása érdekében</span><span class="sxs-lookup"><span data-stu-id="97030-136">Act on alerts to fix issues</span></span>

<span data-ttu-id="97030-137">A forgatókönyv fő előnye, hogy az összes műveletet távolról végezheti el, a megoldás irányítópultjáról.</span><span class="sxs-lookup"><span data-stu-id="97030-137">A key feature of this scenario is that you can perform all these actions remotely from the solution dashboard.</span></span> <span data-ttu-id="97030-138">Nincs szüksége hozzá az eszközök fizikai címére.</span><span class="sxs-lookup"><span data-stu-id="97030-138">You do not need physical access to the devices.</span></span>

## <a name="view-the-solution-dashboard"></a><span data-ttu-id="97030-139">A megoldás irányítópultjának megtekintése</span><span class="sxs-lookup"><span data-stu-id="97030-139">View the solution dashboard</span></span>

<span data-ttu-id="97030-140">A megoldás irányítópultján kezelheti az üzembe helyezett megoldást.</span><span class="sxs-lookup"><span data-stu-id="97030-140">The solution dashboard enables you to manage the deployed solution.</span></span> <span data-ttu-id="97030-141">Az irányítópult egy globális gyárkonfiguráció hierarchikus megjelenítése,</span><span class="sxs-lookup"><span data-stu-id="97030-141">It is a hierarchical representation of a global factory configuration.</span></span> <span data-ttu-id="97030-142">amelyen megtekintheti például az OEE-ket és KPI-ket, és új csomópontokat tehet közzé a telemetriához és műveleti riasztásokhoz.</span><span class="sxs-lookup"><span data-stu-id="97030-142">For example, you can view OEE and KPIs, publish new nodes for telemetry and action alerts.</span></span>

1. <span data-ttu-id="97030-143">Amikor a kiépítés befejeződött, és az előre konfigurált megoldás csempéje **Kész** állapotot jelez, válassza az **Indítás** gombot a csatlakoztatott gyár portál új lapon való megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="97030-143">When the provisioning is complete and the tile for your preconfigured solution indicates **Ready**, choose **Launch** to open your connected factory solution portal in a new tab.</span></span>

    ![Az előre konfigurált megoldás indítása][img-launch-solution]

1. <span data-ttu-id="97030-145">Alapértelmezés szerint a megoldás portálja az *irányítópultot* jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="97030-145">By default, the solution portal shows the *dashboard*.</span></span> <span data-ttu-id="97030-146">A portál más területeire az oldal bal oldali menüjével navigálhat.</span><span class="sxs-lookup"><span data-stu-id="97030-146">To navigate to other areas of the portal, use the menu on the left-hand side of the page.</span></span>

    ![Az előre konfigurált csatlakoztatott gyár megoldás irányítópultja][cf-img-menu]

<span data-ttu-id="97030-148">Az irányítópult az alábbi információkat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="97030-148">The dashboard displays the following information:</span></span>

* <span data-ttu-id="97030-149">A **Factory list** (Gyárak listája) panelt, amelyen ellenőrizhető a megoldás állapota, helye és aktuális termelési konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="97030-149">A **Factory list** panel that shows the status, location, and current production configuration in the solution.</span></span> <span data-ttu-id="97030-150">A megoldás első futtatásakor már rendelkezésre áll néhány szimulált eszköz.</span><span class="sxs-lookup"><span data-stu-id="97030-150">When you first run the solution, there are a number of simulated devices.</span></span> <span data-ttu-id="97030-151">A gyártósor-szimuláció három valós OPC UA-kiszolgálót tartalmaz gyártósoronként, amelyek szimulált feladatokat hajtanak végre és adatokat osztanak meg.</span><span class="sxs-lookup"><span data-stu-id="97030-151">The production line simulation is composed of three real OPC UA servers per production line that perform simulated tasks and share data.</span></span> <span data-ttu-id="97030-152">Az OPC UA architektúráról a [Csatlakoztatott gyár – GYIK](iot-suite-faq-cf.md) fejezetben talál további információt.</span><span class="sxs-lookup"><span data-stu-id="97030-152">For more information about OPC UA, see the [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>
* <span data-ttu-id="97030-153">A megoldáshoz csatlakoztatott egyes eszközök helyét megjelenítő **térképet**.</span><span class="sxs-lookup"><span data-stu-id="97030-153">A **map** that displays the location of each device connected to the solution.</span></span> <span data-ttu-id="97030-154">A megoldás a Bing Maps API-val képes megjeleníteni az információkat a térképen.</span><span class="sxs-lookup"><span data-stu-id="97030-154">The solution can use the Bing Maps API to plot information on the map.</span></span> <span data-ttu-id="97030-155">Ha az előfizetéshez engedélyezve van a Bing Maps Enterprise API használata, a rendszer automatikusan ezt használja.</span><span class="sxs-lookup"><span data-stu-id="97030-155">If your subscription is enabled for Bing Maps Enterprise API, then this feature is used automatically.</span></span> <span data-ttu-id="97030-156">Ha nincs engedélyezve, a [gyakori kérdésekből][lnk-faq] megtudhatja, hogyan teheti dinamikussá a térképet.</span><span class="sxs-lookup"><span data-stu-id="97030-156">If not, see the [FAQ][lnk-faq] to learn how to make the map dynamic.</span></span>
* <span data-ttu-id="97030-157">Egy **Alerts** (Riasztások) panelt, amely riasztásokat jelenít meg, ha egy telemetria- vagy OEE/KPI-érték meghalad egy adott küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="97030-157">An **Alerts** panel that displays alerts generated when a telemetry or OEE/KPI value exceeds a specific threshold.</span></span>
* <span data-ttu-id="97030-158">Egy **Overall Equipment Efficiency** (Teljes eszközhatékonyság) panelt, amely a teljes vállalat vagy a megtekintett gyár/gyártósor/állomás OEE-értékeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="97030-158">An **Overall Equipment Efficiency** panel that shows the OEE values for the whole enterprise, or the factory/production line/station you are viewing.</span></span> <span data-ttu-id="97030-159">Az érték az állomásnézettől a vállalati szintig összesítve van.</span><span class="sxs-lookup"><span data-stu-id="97030-159">This value is aggregated from the station view to the enterprise level.</span></span> <span data-ttu-id="97030-160">Az OEE értéke és annak összetevői részletesebben is elemezhetők.</span><span class="sxs-lookup"><span data-stu-id="97030-160">The OEE figure and its constituent elements can be further analyzed.</span></span>
* <span data-ttu-id="97030-161">Egy **Key Performance Indicators** (Fő teljesítménymutatók) panelt, amely a teljes vállalat vagy a megtekintett gyár/gyártósor/állomás által előállított egységeket és felhasznált energiát mutatja.</span><span class="sxs-lookup"><span data-stu-id="97030-161">**Key Performance Indicators** panel that displays the number of units produced and energy used by the whole enterprise or the factory/production line/station you are viewing.</span></span> <span data-ttu-id="97030-162">Az értékek az állomásnézettől a vállalati szintig összesítve vannak.</span><span class="sxs-lookup"><span data-stu-id="97030-162">These values are aggregated from a station view to the enterprise level.</span></span>

## <a name="view-factories"></a><span data-ttu-id="97030-163">Üzemek megtekintése</span><span class="sxs-lookup"><span data-stu-id="97030-163">View factories</span></span>

<span data-ttu-id="97030-164">A *Factories* (Üzemek) panel a megoldás részét képező összes üzem földrajzi helyét, állapotát és aktuális termelési konfigurációját megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="97030-164">The *Factories* panel shows you the geographical location of all the factories in the solution, their status, and current production configuration.</span></span> <span data-ttu-id="97030-165">A helyek listájából átléphet a megoldáshierarchia más szintjeire.</span><span class="sxs-lookup"><span data-stu-id="97030-165">From the locations list, you can navigate to the other levels in the solution hierarchy.</span></span> <span data-ttu-id="97030-166">A lista sorai hivatkozások, amelyek az adott helyen található gyártósorok részletes adataira mutatnak,</span><span class="sxs-lookup"><span data-stu-id="97030-166">The rows in the list are hyperlinks that link details of the production lines at that location.</span></span> <span data-ttu-id="97030-167">amelyek segítenek feltárni a gyártósorok minden részletét, egészen az állomásszintű nézetig.</span><span class="sxs-lookup"><span data-stu-id="97030-167">It is then possible to drill into the production line details and down to the station level view.</span></span> <span data-ttu-id="97030-168">A listán szűrőt is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="97030-168">You can also apply a filter to the list.</span></span>

![Előre konfigurált csatlakoztatott gyár megoldás – gyárak][cf-img-factories] 

1. <span data-ttu-id="97030-170">A **Factory panel** a megoldás részét képező gyárak listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="97030-170">The **Factory panel** shows the factory list for this solution.</span></span>

2. <span data-ttu-id="97030-171">A gyárlistán először a kiépítési folyamat által létrehozott hat gyár látható.</span><span class="sxs-lookup"><span data-stu-id="97030-171">The factory list initially shows six factories created by the provisioning process.</span></span> <span data-ttu-id="97030-172">Hozzáadhat további szimulált eszközöket és fizikai eszközöket is a megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="97030-172">You can add additional simulated and physical devices to the solution.</span></span>

3. <span data-ttu-id="97030-173">Valamely gyár részleteinek megtekintéséhez kattintson az adott sorra a gyárlistában.</span><span class="sxs-lookup"><span data-stu-id="97030-173">To view the details of a factory, click the row in the factory list.</span></span>

4. <span data-ttu-id="97030-174">Valamely gyártósor részleteinek megtekintéséhez kattintson az adott sorra a listában.</span><span class="sxs-lookup"><span data-stu-id="97030-174">To view the details of a production line, click the row in the list.</span></span>

5. <span data-ttu-id="97030-175">Valamely gyártósor állomásához tartozó közzétett OPC UA-csomópontok megtekintéséhez kattintson a vonatkozó sorra a listában.</span><span class="sxs-lookup"><span data-stu-id="97030-175">To view the published OPC UA nodes of a station on the production line, click the row in the list.</span></span>

6. <span data-ttu-id="97030-176">Valamely állomás adott csomópontjára vonatkozó részletek megtekintéséhez kattintson az adott sorra a listában.</span><span class="sxs-lookup"><span data-stu-id="97030-176">To view details on a specific node in the station, click the row in the list.</span></span> <span data-ttu-id="97030-177">Ez a művelet elindítja a helyi panelt a Time Series Insights-vizualizációkkal.</span><span class="sxs-lookup"><span data-stu-id="97030-177">This action launches the context panel with Time Series Insights visualizations.</span></span> <span data-ttu-id="97030-178">A diagramokra kattintva részletesebb elemzéseket is végezhet a Time Series Insights Explorer környezetében.</span><span class="sxs-lookup"><span data-stu-id="97030-178">Click these graphs to do further analysis in the Time Series Insights explorer environment.</span></span>

## <a name="view-map"></a><span data-ttu-id="97030-179">Térkép megtekintése</span><span class="sxs-lookup"><span data-stu-id="97030-179">View map</span></span>

<span data-ttu-id="97030-180">Ha előfizetésével hozzá tud férni a Bing Maps API-hoz, a *Factories* (Üzemek) térkép a megoldás részét képező összes üzem földrajzi helyét és állapotát megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="97030-180">If your subscription has access to the Bing Maps API, the *Factories* map shows you the geographical location and status of all the factories in the solution.</span></span> <span data-ttu-id="97030-181">Az adott hely részleteit a térképen megjelenített helyekre kattintva tárhatja fel.</span><span class="sxs-lookup"><span data-stu-id="97030-181">To drill into the location details, click the locations displayed on the map.</span></span>

![Előre konfigurált csatlakoztatott gyár megoldás – térkép][cf-img-map]

## <a name="view-alerts"></a><span data-ttu-id="97030-183">Riasztások megtekintése</span><span class="sxs-lookup"><span data-stu-id="97030-183">View alerts</span></span>

<span data-ttu-id="97030-184">Az **Alert** (Riasztások) panel olyan riasztásokat jelenít meg, amelyek arra figyelmeztetnek, ha egy jelentett érték vagy egy kiszámított OEE/KPI meghaladja a hozzá beállított határértéket.</span><span class="sxs-lookup"><span data-stu-id="97030-184">The **Alert** panel shows you alerts generated due to a reported value or a calculated OEE/KPI value exceeding its configured threshold.</span></span> <span data-ttu-id="97030-185">A panel a hierarchia minden szintjére vonatkozó riasztásokat megjeleníti, az állomásszinttől egészen a globális nézetig.</span><span class="sxs-lookup"><span data-stu-id="97030-185">This panel displays alerts at each level of the hierarchy, from the station level view to the global view.</span></span> <span data-ttu-id="97030-186">A riasztások a riasztás leírását, dátumát és időpontját, helyét és előfordulásainak számát tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="97030-186">The alerts contain a description of the alert, date, time, location, and number of occurrences.</span></span> <span data-ttu-id="97030-187">A Time Series Insights-adatok segítségével mélyebb bepillantást nyerhet a riasztást kiváltó adatokba.</span><span class="sxs-lookup"><span data-stu-id="97030-187">You can gain insights in to the data that caused the alert using the Time Series Insights data.</span></span> <span data-ttu-id="97030-188">Ahol lehetséges, a riasztások Time Series Insights-adatait a rendszer vizualizálja.</span><span class="sxs-lookup"><span data-stu-id="97030-188">The Time Series Insights data is visualized in the alerts where applicable.</span></span> <span data-ttu-id="97030-189">Ha Ön rendszergazda, a riasztásokon a következő alapértelmezett műveleteket hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="97030-189">If you are an Administrator, you can take default actions on the alerts such as:</span></span>

* <span data-ttu-id="97030-190">Lezárhatja a riasztást.</span><span class="sxs-lookup"><span data-stu-id="97030-190">Close the alert.</span></span>
* <span data-ttu-id="97030-191">Nyugtázhatja a riasztást.</span><span class="sxs-lookup"><span data-stu-id="97030-191">Acknowledge the alert.</span></span>

<span data-ttu-id="97030-192">Igény szerint összetettebb műveleteket is végrehajthat.</span><span class="sxs-lookup"><span data-stu-id="97030-192">Optionally, you can take more complex actions.</span></span> <span data-ttu-id="97030-193">Például egy szerelvény nyomási OPC UA-csomópontja esetében a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="97030-193">For example, for the Pressure OPC UA Node of the Assembly you could:</span></span>

* <span data-ttu-id="97030-194">Támogatási információkat jeleníthet meg egy weboldalon egy új böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="97030-194">Show supporting information in a web page in a new browser window.</span></span>
* <span data-ttu-id="97030-195">A riasztás okának elhárítása érdekében meghívhat egy OPC UA-metódust az eszközön.</span><span class="sxs-lookup"><span data-stu-id="97030-195">Mitigate the cause of the alert by calling an OPC UA method on the device.</span></span>
* <span data-ttu-id="97030-196">Felfüggesztheti az alapértelmezett műveletek rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="97030-196">Suppress the availability of the default actions.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – riasztások][cf-img-alerts]

> [!NOTE]
> <span data-ttu-id="97030-198">Ezek a riasztások az előre konfigurált megoldás egy konfigurációs fájljában megadott szabályok alapján jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="97030-198">These alerts are generated by rules that are specified in a configuration file in the preconfigured solution.</span></span> <span data-ttu-id="97030-199">Ezek a szabályok riasztásokat hoznak létre, ha az OEE- vagy KPI-értékek, vagy az OPC UA-csomópont értékei meghaladják a beállított küszöbértékeket.</span><span class="sxs-lookup"><span data-stu-id="97030-199">These rules can generate alerts when the OEE or KPI figures or OPC UA Node values are exceeding their configured threshold.</span></span>

1. <span data-ttu-id="97030-200">Az **Alerts panel** (Riasztások) a jelen megoldásban létrehozott riasztásokat mutatja.</span><span class="sxs-lookup"><span data-stu-id="97030-200">The **Alerts panel** shows the alerts generated in this solution.</span></span>

2. <span data-ttu-id="97030-201">Egy riasztás részleteinek megtekintéséhez kattintson az adott riasztásra a riasztások panelén.</span><span class="sxs-lookup"><span data-stu-id="97030-201">To view the details of an alert, click the alert in the alerts panel.</span></span>

3. <span data-ttu-id="97030-202">A riasztás adatainak további elemzéséhez kattintson a diagramra a riasztások panelén a Time Series Insights Explorer környezetének megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="97030-202">To further analyze the alert data, click the graph in the alert panel to open the Time Series Insights explorer environment.</span></span>

4. <span data-ttu-id="97030-203">A riasztások kezeléséhez több művelet is elérhető a riasztások panelén.</span><span class="sxs-lookup"><span data-stu-id="97030-203">To address the alert, several actions are available in the alert panel.</span></span> <span data-ttu-id="97030-204">Válassza ki a megfelelő lehetőséget, és kattintson a műveletet végrehajtó parancsgombra.</span><span class="sxs-lookup"><span data-stu-id="97030-204">Choose the appropriate option for you and click the execute action command button.</span></span>

## <a name="view-overall-equipment-efficiency"></a><span data-ttu-id="97030-205">A teljes eszközhatékonyság megtekintése</span><span class="sxs-lookup"><span data-stu-id="97030-205">View overall equipment efficiency</span></span>

<span data-ttu-id="97030-206">Az OEE a gyártási folyamat hatékonyságát osztályozza a gyártáshoz kapcsolódó főbb működési paraméterek alapján.</span><span class="sxs-lookup"><span data-stu-id="97030-206">OEE rates the efficiency of the manufacturing process using a key production-related operational parameters.</span></span> <span data-ttu-id="97030-207">Az OEE egy iparági szabvány mérőszám, amely a rendelkezésre állás, a teljesítmény és a minőség besorolásainak szorzata: OEE = rendelkezésre állás x teljesítmény x minőség.</span><span class="sxs-lookup"><span data-stu-id="97030-207">OEE is an industry standard measure calculated by multiplying the availability rate, performance rate, and quality rate: OEE = availability x performance x quality.</span></span>

![Előre konfigurált csatlakoztatott gyár megoldás – OEE][cf-img-oee]

1. <span data-ttu-id="97030-209">A hierarchia valamely szintjére vonatkozó OEE megtekintéséhez lépjen az adott nézetre.</span><span class="sxs-lookup"><span data-stu-id="97030-209">To view OEE for any level in the hierarchy, navigate to the specific view you require.</span></span> <span data-ttu-id="97030-210">A nézet OEE mutatója megjelenik a panelen, azon elemekkel együtt, amelyek összesen kiteszik az OEE százalékos értékét.</span><span class="sxs-lookup"><span data-stu-id="97030-210">The OEE for that view displays in the panel along with each of the elements that make up the OEE percentage.</span></span>

2. <span data-ttu-id="97030-211">Az OEE mélyebb elemzéséhez a hierarchiaadatok valamelyik szintjén kattintson az OEE, a rendelkezésre állás, a teljesítmény vagy a minőség százalékos értékére.</span><span class="sxs-lookup"><span data-stu-id="97030-211">To further analyze the OEE for any level in the hierarchy data, click either the OEE, availability, performance, or quality percentage.</span></span> <span data-ttu-id="97030-212">Megjelenik egy Time Series Insights-vizualizációkat tartalmazó helyi panel, amelyek az elmúlt óra, az elmúlt 24 óra és az elmúlt 7 nap adatait mutatják.</span><span class="sxs-lookup"><span data-stu-id="97030-212">A context panel appears with Time Series Insights powered visualizations that shows data from the last hour, last 24 hours, and last 7 days.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – TSI-vizualizáció][cf-img-tsi-visualization]

3. <span data-ttu-id="97030-214">A riasztás adatainak további elemzéséhez kattintson a diagramra a riasztások panelén.</span><span class="sxs-lookup"><span data-stu-id="97030-214">To further analyze the alert data, click the graph in the alert panel.</span></span> <span data-ttu-id="97030-215">Ez a művelet megnyitja a Time Series Insights Explorer környezetet.</span><span class="sxs-lookup"><span data-stu-id="97030-215">This action opens the Time Series Insights explorer environment.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – TSI Explorer][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a><span data-ttu-id="97030-217">Fő teljesítménymutatók megtekintése</span><span class="sxs-lookup"><span data-stu-id="97030-217">View Key Performance Indicators</span></span>

<span data-ttu-id="97030-218">A megoldás két fő teljesítménymutatót biztosít: *egységek száma óránként* és *felhasznált energia (kWh)*.</span><span class="sxs-lookup"><span data-stu-id="97030-218">The solution provides two key performance indicators, *units per hour* and *energy used in kWh*.</span></span>

![Előre konfigurált csatlakoztatott gyár megoldás – KPI][cf-img-kpi]

1. <span data-ttu-id="97030-220">A hierarchia valamely szintjére vonatkozó óránkénti egységszám vagy felhasznált energia megtekintéséhez lépjen az adott nézetre.</span><span class="sxs-lookup"><span data-stu-id="97030-220">To view units per hour or energy used for any level in the hierarchy, navigate to the specific view you require.</span></span> <span data-ttu-id="97030-221">Az óránkénti egységszám és a felhasznált energia megjelenik a panelen.</span><span class="sxs-lookup"><span data-stu-id="97030-221">The units per hour and energy used display in the panel.</span></span>

2. <span data-ttu-id="97030-222">Az óránkénti egységszám vagy a felhasznált energia elemzéséhez a hierarchia valamely további szintjén kattintson a mutatóra a **Fő teljesítménymutatók** panelen.</span><span class="sxs-lookup"><span data-stu-id="97030-222">To analyze units per hour or energy used for any level in the hierarchy further, click the gauge in the **Key Performance Indicators** panel.</span></span> <span data-ttu-id="97030-223">Megjelenik egy Time Series Insights-vizualizációkat tartalmazó helyi panel, amelyek az elmúlt óra, az elmúlt 24 óra és az elmúlt 7 nap adatait mutatják.</span><span class="sxs-lookup"><span data-stu-id="97030-223">A context panel appears with Time Series Insights powered visualizations enabling you to view data from the last hour, the last 24 hours, and last 7 days.</span></span>

## <a name="scenario-review"></a><span data-ttu-id="97030-224">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="97030-224">Scenario review</span></span>

<span data-ttu-id="97030-225">Ebben a forgatókönyvben megfigyelte az üzemek OEE és KPI mutatóit az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="97030-225">In this scenario, you monitored your factories OEE and KPIs values, in the dashboard.</span></span> <span data-ttu-id="97030-226">Ezután a Time Series Insights használatával további információkhoz jutott, hogy az OEE-k és KPI-k telemetriaadatainak részletesebb feltárásával segíthesse elő a rendellenességek észlelését.</span><span class="sxs-lookup"><span data-stu-id="97030-226">You then used Time Series Insights to provide more information to help drill further into the telemetry data for OEE and KPIs to help with detecting anomalies.</span></span> <span data-ttu-id="97030-227">A riasztási panelen megtekintette az üzemekben jelentkező problémákat, és a rendelkezésre műveletekkel orvosolta a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="97030-227">You also used the alert panel to view issues with your factories and you used the actions available to you to resolve the alert.</span></span>

## <a name="other-features"></a><span data-ttu-id="97030-228">Egyéb jellemzők</span><span class="sxs-lookup"><span data-stu-id="97030-228">Other features</span></span>

<span data-ttu-id="97030-229">A következő szakaszokban a csatlakoztatott gyár megoldás néhány egyéb jellemzőjéről lesz szó, amelyek nem szerepeltek az előző forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="97030-229">The following sections describe some additional features of the connected factory solution that are not described in the previous scenario.</span></span>

## <a name="apply-filters"></a><span data-ttu-id="97030-230">Szűrők alkalmazása</span><span class="sxs-lookup"><span data-stu-id="97030-230">Apply filters</span></span>

1. <span data-ttu-id="97030-231">Kattintson a **sávnyílra** a gyárhelyek vagy a riasztások panelén elérhető szűrők listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="97030-231">Click the **chevron** to display a list of available filters in either the factory locations panel or the alerts panel.</span></span>

2. <span data-ttu-id="97030-232">Megjelenik a szűrők panelje.</span><span class="sxs-lookup"><span data-stu-id="97030-232">The filters panel is displayed for you.</span></span> 

    ![Előre konfigurált csatlakoztatott gyár megoldás – szűrők][cf-img-alert-filter]

3. <span data-ttu-id="97030-234">Válassza ki a kívánt szűrőt.</span><span class="sxs-lookup"><span data-stu-id="97030-234">Choose the filter that you require.</span></span> <span data-ttu-id="97030-235">A szűrőmezőkben szabad szöveget is megadhat.</span><span class="sxs-lookup"><span data-stu-id="97030-235">It is also possible to type free text into the filter fields.</span></span>

4. <span data-ttu-id="97030-236">A rendszer működésbe lépteti a szűrőt.</span><span class="sxs-lookup"><span data-stu-id="97030-236">The filter is then applied for you.</span></span> <span data-ttu-id="97030-237">A szűrő állapota az irányítópulton is megjeleníthető egy tölcsér ikonon keresztül, amely az üzemek és a riasztások tábláiban is elérhető.</span><span class="sxs-lookup"><span data-stu-id="97030-237">The filter state is also shown in the dashboard via a funnel that displays in the factories and alerts tables.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – szűrők][cf-img-alert-filter-funnel]

    > [!NOTE]
    > <span data-ttu-id="97030-239">Az aktív szűrők nincsenek hatással a megjelenített OEE és KPI mutatókra, csupán a listák tartalmát szűkítik.</span><span class="sxs-lookup"><span data-stu-id="97030-239">An active filter does not affect the displayed OEE and KPI values, it only filters the list contents.</span></span>

5. <span data-ttu-id="97030-240">A szűrő törléséhez kattintson a tölcsér ikonra, majd a szűrőre a szűrő helyi panelén.</span><span class="sxs-lookup"><span data-stu-id="97030-240">To clear a filter, click the funnel and click filter in the filter context panel.</span></span> <span data-ttu-id="97030-241">Ekkor az **All** (Összes) kifejezés jelenik meg az üzemek és a riasztások tábláiban.</span><span class="sxs-lookup"><span data-stu-id="97030-241">The text **All** is displayed in the factories and alerts tables.</span></span>

## <a name="browse-an-opc-ua-server"></a><span data-ttu-id="97030-242">OPC UA-kiszolgáló tallózása</span><span class="sxs-lookup"><span data-stu-id="97030-242">Browse an OPC UA server</span></span>

<span data-ttu-id="97030-243">Az előre konfigurált megoldás üzembe helyezésekor automatikusan sor kerül a szimulált OPC UA-kiszolgálók kiosztására, amelyek a megoldás böngészőjében tallózhatóak.</span><span class="sxs-lookup"><span data-stu-id="97030-243">When you deploy the preconfigured solution, you automatically provision simulated OPC UA servers that you can browse via the solution browser.</span></span> <span data-ttu-id="97030-244">Ezek a kiszolgálók *szimulált OPC UA-kiszolgálók*.</span><span class="sxs-lookup"><span data-stu-id="97030-244">These servers are *simulated OPC UA servers*.</span></span> <span data-ttu-id="97030-245">A szimulált kiszolgálók megkönnyítik az előre konfigurált megoldással történő kísérletezést, anélkül, hogy valódi, fizikai kiszolgálók üzembe helyezésére lenne szükség.</span><span class="sxs-lookup"><span data-stu-id="97030-245">Simulated servers make it easy for you to experiment with the preconfigured solution without the need to deploy real, physical servers.</span></span> <span data-ttu-id="97030-246">Valós OPC UA-kiszolgálóknak a megoldáshoz történő csatlakoztatásáról [az OPC UA-eszköz előre konfigurált csatlakoztatott gyár megoldáshoz történő csatlakoztatásával][lnk-connect-cf] foglalkozó oktatóanyagban olvashat.</span><span class="sxs-lookup"><span data-stu-id="97030-246">If you do want to connect a real OPC UA server to the solution, see the [Connect your OPC UA device to the connected factory preconfigured solution][lnk-connect-cf] tutorial.</span></span>

1. <span data-ttu-id="97030-247">Kattintson a **gyár ikonra** az irányítópult navigációs sávján.</span><span class="sxs-lookup"><span data-stu-id="97030-247">Click the **factory icon** in the dashboard navigation bar.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – kiszolgálótallózó][cf-img-server-browser]

2. <span data-ttu-id="97030-249">Válasszon egyet a kiszolgálók közül az előre konfigurált listában.</span><span class="sxs-lookup"><span data-stu-id="97030-249">Choose one of the servers from the preconfigured list.</span></span> <span data-ttu-id="97030-250">A lista az előre konfigurált megoldásban üzembe helyezett kiszolgálókat mutatja.</span><span class="sxs-lookup"><span data-stu-id="97030-250">This list shows the servers that are deployed for you in the preconfigured solution.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – kiválasztott kiszolgálók][cf-img-server-choice]

3. <span data-ttu-id="97030-252">Kattintson a **Connect** (Csatlakozás) gombra. Megjelenik egy biztonsági párbeszédablak.</span><span class="sxs-lookup"><span data-stu-id="97030-252">Click **Connect**, a security dialog displays.</span></span> <span data-ttu-id="97030-253">A szimuláció esetében biztonságosan kattinthat a **Proceed** (Folytatás) gombra.</span><span class="sxs-lookup"><span data-stu-id="97030-253">For the simulation, it is safe to click **Proceed**</span></span>

4. <span data-ttu-id="97030-254">Kattintson bármely csomópontra a kiszolgálófán a kibontásához.</span><span class="sxs-lookup"><span data-stu-id="97030-254">To expand any of the nodes in the server tree, click it.</span></span> <span data-ttu-id="97030-255">A telemetriaadatokat közzétevő csomópontokat egy pipa jelöli.</span><span class="sxs-lookup"><span data-stu-id="97030-255">Nodes that are publishing telemetry have a tick mark beside them.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – kiszolgálófa][cf-img-server-tree]

5. <span data-ttu-id="97030-257">Egy csomópont olvasásához, írásához, közzétételéhez vagy meghívásához kattintson jobb gombbal az adott elemre.</span><span class="sxs-lookup"><span data-stu-id="97030-257">Right-click an item to read, write, publish, or call that node.</span></span> <span data-ttu-id="97030-258">Az elérhető műveletek a jogosultságaitól és a csomópont attribútumaitól függnek.</span><span class="sxs-lookup"><span data-stu-id="97030-258">The actions available to you depend on your permissions and the attributes of the node.</span></span> <span data-ttu-id="97030-259">Az olvasási lehetőség egy helyi panelt jelenít meg, amelyen az adott csomópont értéke látható.</span><span class="sxs-lookup"><span data-stu-id="97030-259">The read option to displays a context panel showing the value of the specific node.</span></span> <span data-ttu-id="97030-260">Az írási lehetőség egy olyan helyi panelt jelenít meg, amelyen új értéket adhat meg.</span><span class="sxs-lookup"><span data-stu-id="97030-260">The write option displays a context panel where you can enter a new value.</span></span> <span data-ttu-id="97030-261">A hívási lehetőség egy csomópontot jelenít meg, amelyen megadhatja a hívás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="97030-261">The call option displays a node where you can enter the parameters for the call.</span></span>

## <a name="publish-a-node"></a><span data-ttu-id="97030-262">Csomópont közzététele</span><span class="sxs-lookup"><span data-stu-id="97030-262">Publish a node</span></span>

<span data-ttu-id="97030-263">Ha egy *szimulált OPC UA-kiszolgálóhoz* tallóz, új csomópontokat is közzétehet, ha szeretne.</span><span class="sxs-lookup"><span data-stu-id="97030-263">When you browse a *simulated OPC UA server*, you can also choose to publish new nodes.</span></span> <span data-ttu-id="97030-264">Ezeknek a csomópontoknak a telemetriáját is elemezheti a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="97030-264">You can analyze the telemetry from these nodes in the solution.</span></span> <span data-ttu-id="97030-265">Ezek a *szimulált OPC UA-kiszolgálók* megkönnyítik az előre konfigurált megoldással történő kísérletezést, anélkül, hogy valódi, fizikai eszközök üzembe helyezésére lenne szükség.</span><span class="sxs-lookup"><span data-stu-id="97030-265">These *simulated OPC UA servers* make it easy to experiment with the preconfigured solution without deploying real, physical devices.</span></span>

1. <span data-ttu-id="97030-266">Lépjen egy közzétenni kívánt csomópontra az OPC UA-kiszolgáló tallózási fáján.</span><span class="sxs-lookup"><span data-stu-id="97030-266">Browse to a node in the OPC UA server browser tree that you wish to publish.</span></span>

2. <span data-ttu-id="97030-267">Kattintson a jobb gombbal a csomópontra.</span><span class="sxs-lookup"><span data-stu-id="97030-267">Right-click the node.</span></span>

3. <span data-ttu-id="97030-268">Válassza a **Publish** (Közzététel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="97030-268">Choose **Publish**.</span></span>

    ![A csatlakoztatott gyár közzétesz egy csomópontot][cf-img-publish-node]

4. <span data-ttu-id="97030-270">Megjelenik egy helyi panel, amely tájékoztatja, hogy a közzététel sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="97030-270">A context panel appears which tells you that the publish has succeeded.</span></span> <span data-ttu-id="97030-271">A csomópont megjelenik az állomásszinten, mellette egy pipával.</span><span class="sxs-lookup"><span data-stu-id="97030-271">The node appears in the station level view with a check mark beside it.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – sikeres közzététel][cf-img-publish-success]

## <a name="command-and-control"></a><span data-ttu-id="97030-273">Parancs és vezérlés</span><span class="sxs-lookup"><span data-stu-id="97030-273">Command and control</span></span>

<span data-ttu-id="97030-274">A csatlakoztatott gyár segítségével közvetlenül a felhőből irányíthatja és felügyelheti ipari eszközeit.</span><span class="sxs-lookup"><span data-stu-id="97030-274">The connected factory allows you command and control your industry devices directly from the cloud.</span></span> <span data-ttu-id="97030-275">A szolgáltatással az eszközök által létrehozott riasztásokra is reagálhat,</span><span class="sxs-lookup"><span data-stu-id="97030-275">You can use this feature to respond to alerts generated by the device.</span></span> <span data-ttu-id="97030-276">például parancsokat küldhet a felhőből egy eszközre.</span><span class="sxs-lookup"><span data-stu-id="97030-276">For example, you could send a command to the device from the cloud.</span></span> <span data-ttu-id="97030-277">A rendelkezésre álló parancsokat az OPC UA-kiszolgáló tallózási fájának **StationCommands** csomópontján találja.</span><span class="sxs-lookup"><span data-stu-id="97030-277">You can find the available commands in the **StationCommands** node in the OPC UA servers browser tree.</span></span> <span data-ttu-id="97030-278">Ebben a forgatókönyvben megnyit egy nyomáskiegyenlítő szelepet egy müncheni gyártósor összeszerelő állomásán.</span><span class="sxs-lookup"><span data-stu-id="97030-278">In this scenario, you open a pressure release valve on the assembly station of a production line in Munich.</span></span> <span data-ttu-id="97030-279">Az irányítási és felügyeleti funkciók használatához **Rendszergazda** szerepkörrel kell rendelkeznie az előre konfigurált megoldás üzemelő példányán.</span><span class="sxs-lookup"><span data-stu-id="97030-279">To use the command and control functionality, you must be in the **Administrator** role for the preconfigured solution deployment.</span></span>

1. <span data-ttu-id="97030-280">Lépjen a **StationCommands** csomópontra az OPC UA-kiszolgáló tallózási fáján.</span><span class="sxs-lookup"><span data-stu-id="97030-280">Browse to the **StationCommands** node in the OPC UA server browser tree.</span></span>

2. <span data-ttu-id="97030-281">Válassza ki a használni kívánt parancsot.</span><span class="sxs-lookup"><span data-stu-id="97030-281">Choose the command that you wish use.</span></span>

3. <span data-ttu-id="97030-282">Kattintson a jobb gombbal a csomópontra.</span><span class="sxs-lookup"><span data-stu-id="97030-282">Right-click the node.</span></span>

4. <span data-ttu-id="97030-283">Válassza a **Call** (Hívás) parancsot.</span><span class="sxs-lookup"><span data-stu-id="97030-283">Choose **Call**.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – hívás parancs][cf-img-call-command]

5. <span data-ttu-id="97030-285">Megjelenik egy helyi panel, amely tájékoztatja a meghívni készült metódusról, valamint az esetleges vonatkozó paraméterek részleteiről.</span><span class="sxs-lookup"><span data-stu-id="97030-285">A context panel appears informing you which method you are about to call and any parameter details is applicable.</span></span>

6. <span data-ttu-id="97030-286">Válassza a **Call** (Hívás) parancsot.</span><span class="sxs-lookup"><span data-stu-id="97030-286">Choose **Call**.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – hívás helyi panelje][cf-img-call-context]

7. <span data-ttu-id="97030-288">A helyi panel frissül, és tájékoztatja róla, ha a metódus meghívása sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="97030-288">The context panel is updated to inform you that the method call succeeded.</span></span> <span data-ttu-id="97030-289">A hívás sikerességét ellenőrizheti a hívás eredményeképpen frissített nyomásmérő csomópont értékének leolvasásával.</span><span class="sxs-lookup"><span data-stu-id="97030-289">You can verify the call succeeded by reading the value of the pressure node that updated as a result of the call.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – sikeres hívás][cf-img-call-success]


## <a name="behind-the-scenes"></a><span data-ttu-id="97030-291">A színfalak mögött</span><span class="sxs-lookup"><span data-stu-id="97030-291">Behind the scenes</span></span>

<span data-ttu-id="97030-292">Előre konfigurált megoldás üzembe helyezésekor az üzembehelyezési folyamat több erőforrást hoz létre a kiválasztott Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="97030-292">When you deploy a preconfigured solution, the deployment process creates multiple resources in the Azure subscription you selected.</span></span> <span data-ttu-id="97030-293">Ezeket az erőforrásokat az Azure [Portalon][lnk-portal] tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="97030-293">You can view these resources in the Azure [portal][lnk-portal].</span></span> <span data-ttu-id="97030-294">Az üzembe helyezési folyamat létrehoz egy **erőforráscsoportot** az előre konfigurált megoldáshoz kiválasztott néven alapuló névvel:</span><span class="sxs-lookup"><span data-stu-id="97030-294">The deployment process creates a **resource group** with a name based on the name you choose for your preconfigured solution:</span></span>

![Előre konfigurált megoldás az Azure Portalon][img-cf-portal]

<span data-ttu-id="97030-296">Az egyes erőforrások beállításainak megtekintéséhez válassza ki az erőforrást az erőforráscsoport erőforráslistájában.</span><span class="sxs-lookup"><span data-stu-id="97030-296">You can view the settings of each resource by selecting it in the list of resources in the resource group.</span></span>

<span data-ttu-id="97030-297">Megtekintheti az előre konfigurált megoldás forráskódját is.</span><span class="sxs-lookup"><span data-stu-id="97030-297">You can also view the source code for the preconfigured solution.</span></span> <span data-ttu-id="97030-298">Az előre konfigurált csatlakoztatott gyár megoldás forráskódja az [azure-iot-connected-factory][lnk-cfgithub] GitHub-adattárban található:</span><span class="sxs-lookup"><span data-stu-id="97030-298">The connected factory preconfigured solution source code is in the [azure-iot-connected-factory][lnk-cfgithub] GitHub repository:</span></span>

<span data-ttu-id="97030-299">Amikor elkészült, törölheti az előre konfigurált megoldást az Azure-előfizetésből az [azureiotsuite.com][lnk-azureiotsuite] webhelyen.</span><span class="sxs-lookup"><span data-stu-id="97030-299">When you are done, you can delete the preconfigured solution from your Azure subscription on the [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="97030-300">Így könnyedén törölheti az előre konfigurált megoldás létrehozásakor megkapott összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="97030-300">This site enables you to easily delete all the resources that were provisioned when you created the preconfigured solution.</span></span>

> [!NOTE]
> <span data-ttu-id="97030-301">Ahhoz, hogy biztosan törölje az előre konfigurált megoldáshoz kapcsolódó összes elemet, törölje a megoldást az [azureiotsuite.com][lnk-azureiotsuite] webhelyről.</span><span class="sxs-lookup"><span data-stu-id="97030-301">To ensure that you delete everything related to the preconfigured solution, delete it on the [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="97030-302">Ne törölje az erőforráscsoportot a portálon.</span><span class="sxs-lookup"><span data-stu-id="97030-302">Do not delete the resource group in the portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97030-303">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97030-303">Next Steps</span></span>

<span data-ttu-id="97030-304">Most, hogy üzembe helyezett egy működő előre konfigurált megoldást, a következő cikkek elolvasásával folytathatja az ismerkedést az IoT Suite használatával:</span><span class="sxs-lookup"><span data-stu-id="97030-304">Now that you’ve deployed a working preconfigured solution, you can continue getting started with IoT Suite by reading the following articles:</span></span>

* <span data-ttu-id="97030-305">[Előre konfigurált csatlakoztatott gyár megoldás – bemutató][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="97030-305">[Connected factory preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="97030-306">[Az eszköz csatlakoztatása az előre konfigurált csatlakoztatott gyár megoldáshoz][lnk-connect-cf]</span><span class="sxs-lookup"><span data-stu-id="97030-306">[Connect your device to the Connected factory preconfigured solution][lnk-connect-cf]</span></span>
* <span data-ttu-id="97030-307">[Engedélyek az azureiotsuite.com webhelyen][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="97030-307">[Permissions on the azureiotsuite.com site][lnk-permissions]</span></span>

[img-cf-home]:media/iot-suite-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-suite-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-suite-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-suite-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-suite-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-suite-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-suite-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-suite-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-suite-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-suite-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-suite-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-suite-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-suite-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-suite-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-suite-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-suite-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-suite-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-suite-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-suite-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-suite-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-suite-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md
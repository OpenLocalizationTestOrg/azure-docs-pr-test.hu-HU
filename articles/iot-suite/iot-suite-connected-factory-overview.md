---
title: "az IoT Suite aaaAzure csatlakoztatott gyári áttekintése |} Microsoft Docs"
description: "Hello Azure IoT Suite leírása előre konfigurált gyári megoldás csatlakoztatva."
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
ms.openlocfilehash: 929b5ed41ef7d82c9b4480d02aa3f0db38931919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-connected-factory-preconfigured-solution"></a><span data-ttu-id="743c3-103">Ismerkedés a csatlakoztatott hello előre konfigurált gyári megoldás</span><span class="sxs-lookup"><span data-stu-id="743c3-103">Get started with hello connected factory preconfigured solution</span></span>

<span data-ttu-id="743c3-104">Az Azure IoT Suite [előre konfigurált megoldások] [ lnk-preconfigured-solutions] több Azure IoT szolgáltatások toodeliver végpontok közötti megoldások, amelyek megvalósítják az IoT elterjedt üzleti forgatókönyvek kombinálni.</span><span class="sxs-lookup"><span data-stu-id="743c3-104">Azure IoT Suite [preconfigured solutions][lnk-preconfigured-solutions] combine multiple Azure IoT services toodeliver end-to-end solutions that implement common IoT business scenarios.</span></span> <span data-ttu-id="743c3-105">Hello *csatlakoztatott gyári* előkonfigurált megoldás csatlakozik tooand figyelők az ipari eszközök.</span><span class="sxs-lookup"><span data-stu-id="743c3-105">hello *connected factory* preconfigured solution connects tooand monitors your industrial devices.</span></span> <span data-ttu-id="743c3-106">Használhatja a hello megoldás tooanalyze hello adatfolyamban az adatok az eszközök és a toodrive működési hatékonyság és a nyereségességre nézve.</span><span class="sxs-lookup"><span data-stu-id="743c3-106">You can use hello solution tooanalyze hello stream of data from your devices and toodrive operational productivity and profitability.</span></span>

<span data-ttu-id="743c3-107">Az oktatóanyag bemutatja, hogyan tooprovision hello csatlakoztatva gyári előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="743c3-107">This tutorial shows you how tooprovision hello connected factory preconfigured solution.</span></span> <span data-ttu-id="743c3-108">Azt is bemutatja, hogyan hello alapvető szolgáltatások előre konfigurált hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="743c3-108">It also walks you through hello basic features of hello preconfigured solution.</span></span> <span data-ttu-id="743c3-109">Érheti el ezeket a funkciókat számos hello megoldás *irányítópult* , előre konfigurált hello megoldás részeként telepíti:</span><span class="sxs-lookup"><span data-stu-id="743c3-109">You can access many of these features from hello solution *dashboard* that deploys as part of hello preconfigured solution:</span></span>

![Az előre konfigurált csatlakoztatott gyár megoldás irányítópultja][img-cf-home]

<span data-ttu-id="743c3-111">toocomplete ebben az oktatóanyagban aktív Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="743c3-111">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="743c3-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="743c3-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="743c3-113">További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="743c3-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
> 
> 

## <a name="provision-hello-solution"></a><span data-ttu-id="743c3-114">Kiépítés hello megoldás</span><span class="sxs-lookup"><span data-stu-id="743c3-114">Provision hello solution</span></span>

1. <span data-ttu-id="743c3-115">Jelentkezzen be Azure-fiók hitelesítő adataival tooazureiotsuite.com, és kattintson a "**+**" toocreate megoldást.</span><span class="sxs-lookup"><span data-stu-id="743c3-115">Log on tooazureiotsuite.com using your Azure account credentials, and click "**+**" toocreate a solution.</span></span>
2. <span data-ttu-id="743c3-116">Kattintson a **válasszon** a hello **csatlakoztatott gyári** csempére.</span><span class="sxs-lookup"><span data-stu-id="743c3-116">Click **Select** on hello **Connected factory** tile.</span></span>
3. <span data-ttu-id="743c3-117">Adja meg a **Megoldásnevet** az előre konfigurált csatlakoztatott gyár megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="743c3-117">Enter a **Solution name** for your connected factory preconfigured solution.</span></span>
4. <span data-ttu-id="743c3-118">Jelölje be hello **előfizetés** és **régió** kívánt toouse tooprovision hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="743c3-118">Select hello **Subscription** and **Region** you want toouse tooprovision hello solution.</span></span>
5. <span data-ttu-id="743c3-119">Kattintson a **megoldás létrehozása** toobegin hello létesítésének folyamatát kell használnia.</span><span class="sxs-lookup"><span data-stu-id="743c3-119">Click **Create Solution** toobegin hello provisioning process.</span></span> <span data-ttu-id="743c3-120">Ez a folyamat általában több percet toorun időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="743c3-120">This process typically takes several minutes toorun.</span></span>

### <a name="while-you-wait-for-hello-provisioning-process-toocomplete"></a><span data-ttu-id="743c3-121">A kiépítési folyamat toocomplete hello várakozás közben</span><span class="sxs-lookup"><span data-stu-id="743c3-121">While you wait for hello provisioning process toocomplete</span></span>

1. <span data-ttu-id="743c3-122">Kattintson a megoldás a hello csempe **kiépítési** állapotát.</span><span class="sxs-lookup"><span data-stu-id="743c3-122">Click hello tile for your solution with **Provisioning** status.</span></span>
2. <span data-ttu-id="743c3-123">Értesítés hello **állapotok kiépítés** , Azure-szolgáltatások vannak telepítve az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="743c3-123">Notice hello **Provisioning states** as Azure services are deployed in your Azure subscription.</span></span>
3. <span data-ttu-id="743c3-124">Miután kiépítése befejeződött, hello állapotmódosítások túl**készen**.</span><span class="sxs-lookup"><span data-stu-id="743c3-124">Once provisioning completes, hello status changes too**Ready**.</span></span>
4. <span data-ttu-id="743c3-125">A megoldás hello jobb oldali ablaktáblában hello csempe toosee hello Részletek gombra.</span><span class="sxs-lookup"><span data-stu-id="743c3-125">Click hello tile toosee hello details of your solution in hello right-hand pane.</span></span>

> [!NOTE]
> <span data-ttu-id="743c3-126">Ha hibát tapasztal előre konfigurált hello megoldás telepítésének, tekintse át a [hello azureiotsuite.com hely engedélyeinek] [ lnk-permissions] és hello [csatlakoztatott gyári gyakran ismételt kérdések](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="743c3-126">If you encounter issues deploying hello preconfigured solution, review [Permissions on hello azureiotsuite.com site][lnk-permissions] and hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span> <span data-ttu-id="743c3-127">Ha hello problémák továbbra is fennáll, hozzon létre szolgáltatásjegyet a hello [portal][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="743c3-127">If hello issues persist, create a service ticket on hello [portal][lnk-portal].</span></span>

<span data-ttu-id="743c3-128">Vannak-e részletek toosee teheti meg, amelyek nem jelennek meg a megoldáshoz?</span><span class="sxs-lookup"><span data-stu-id="743c3-128">Are there details you'd expect toosee that aren't listed for your solution?</span></span> <span data-ttu-id="743c3-129">A [felhasználói visszajelzési webhelyen](https://feedback.azure.com/forums/321918-azure-iot) elküldheti a szolgáltatásokkal kapcsolatos javaslatait.</span><span class="sxs-lookup"><span data-stu-id="743c3-129">Give us feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="743c3-130">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="743c3-130">Scenario overview</span></span>

<span data-ttu-id="743c3-131">Csatlakoztatott hello telepítésekor gyári előre konfigurált megoldás, azt az erőforrásokat, amelyek lehetővé teszik a toostep keresztül ipari forgatókönyve előre feltöltve.</span><span class="sxs-lookup"><span data-stu-id="743c3-131">When you deploy hello connected factory preconfigured solution, it is prepopulated with resources that enable you toostep through a common industrial scenario.</span></span> <span data-ttu-id="743c3-132">Ebben a forgatókönyvben számos előállítók csatlakoztatott toohello megoldás jelentés hello adatok értékek szükséges toocompute berendezések hatékonyságot (OEE) és fő teljesítménymutatók (KPI).</span><span class="sxs-lookup"><span data-stu-id="743c3-132">In this scenario, several factories connected toohello solution report hello data values required toocompute overall equipment efficiency (OEE) and key performance indicators (KPIs).</span></span> <span data-ttu-id="743c3-133">hello következő szakaszok bemutatják, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="743c3-133">hello following sections show you how to:</span></span>

* <span data-ttu-id="743c3-134">Gyárakra, gyártósorokra, állomásokra vonatkozó OEE- és KPI-értékek figyelése</span><span class="sxs-lookup"><span data-stu-id="743c3-134">Monitor factory, production lines, station OEE, and KPI values</span></span>
* <span data-ttu-id="743c3-135">Ezek az eszközök Azure idő adatsorozat Insights alapján generált hello telemetriai adatainak elemzése</span><span class="sxs-lookup"><span data-stu-id="743c3-135">Analyze hello telemetry data generated from these devices using Azure Time Series Insights</span></span>
* <span data-ttu-id="743c3-136">A riasztások toofix problémák működésre</span><span class="sxs-lookup"><span data-stu-id="743c3-136">Act on alerts toofix issues</span></span>

<span data-ttu-id="743c3-137">Ebben a forgatókönyvben alapfunkciója, hogy távolról is végrehajtható, ezek a műveletek hello megoldás irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="743c3-137">A key feature of this scenario is that you can perform all these actions remotely from hello solution dashboard.</span></span> <span data-ttu-id="743c3-138">Nem kell fizikai hozzáférés toohello eszközök.</span><span class="sxs-lookup"><span data-stu-id="743c3-138">You do not need physical access toohello devices.</span></span>

## <a name="view-hello-solution-dashboard"></a><span data-ttu-id="743c3-139">Nézet hello megoldás irányítópultja</span><span class="sxs-lookup"><span data-stu-id="743c3-139">View hello solution dashboard</span></span>

<span data-ttu-id="743c3-140">hello megoldás irányítópultja toomanage telepített hello megoldás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="743c3-140">hello solution dashboard enables you toomanage hello deployed solution.</span></span> <span data-ttu-id="743c3-141">Az irányítópult egy globális gyárkonfiguráció hierarchikus megjelenítése,</span><span class="sxs-lookup"><span data-stu-id="743c3-141">It is a hierarchical representation of a global factory configuration.</span></span> <span data-ttu-id="743c3-142">amelyen megtekintheti például az OEE-ket és KPI-ket, és új csomópontokat tehet közzé a telemetriához és műveleti riasztásokhoz.</span><span class="sxs-lookup"><span data-stu-id="743c3-142">For example, you can view OEE and KPIs, publish new nodes for telemetry and action alerts.</span></span>

1. <span data-ttu-id="743c3-143">Hello kiépítés befejeztével, miután az előkonfigurált megoldás hello csempe jelzi **készen áll a**, válassza ki **indítsa el** tooopen a csatlakoztatott gyári megoldás portál új lapon.</span><span class="sxs-lookup"><span data-stu-id="743c3-143">When hello provisioning is complete and hello tile for your preconfigured solution indicates **Ready**, choose **Launch** tooopen your connected factory solution portal in a new tab.</span></span>

    ![Indítsa el az előre konfigurált hello megoldás][img-launch-solution]

1. <span data-ttu-id="743c3-145">Alapértelmezés szerint hello megoldás portal mutatja hello *irányítópult*.</span><span class="sxs-lookup"><span data-stu-id="743c3-145">By default, hello solution portal shows hello *dashboard*.</span></span> <span data-ttu-id="743c3-146">toonavigate tooother területek hello portál hello menü hello bal oldalán található hello használata.</span><span class="sxs-lookup"><span data-stu-id="743c3-146">toonavigate tooother areas of hello portal, use hello menu on hello left-hand side of hello page.</span></span>

    ![Az előre konfigurált csatlakoztatott gyár megoldás irányítópultja][cf-img-menu]

<span data-ttu-id="743c3-148">hello irányítópult a következő információ hello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="743c3-148">hello dashboard displays hello following information:</span></span>

* <span data-ttu-id="743c3-149">A **gyári lista** hello állapot, a helyet és a jelenlegi üzemi konfiguráció hello megoldás megjelenítő panelen.</span><span class="sxs-lookup"><span data-stu-id="743c3-149">A **Factory list** panel that shows hello status, location, and current production configuration in hello solution.</span></span> <span data-ttu-id="743c3-150">Hello megoldás első futtatásakor számos szimulált eszköz.</span><span class="sxs-lookup"><span data-stu-id="743c3-150">When you first run hello solution, there are a number of simulated devices.</span></span> <span data-ttu-id="743c3-151">hello termelési sor szimuláció állnak három valós OPC EE kiszolgálók éles soronként, amelyek szimulált feladatokat végeznek megoszthatják az adatokat.</span><span class="sxs-lookup"><span data-stu-id="743c3-151">hello production line simulation is composed of three real OPC UA servers per production line that perform simulated tasks and share data.</span></span> <span data-ttu-id="743c3-152">OPC EE kapcsolatos további információkért lásd: hello [gyári gyakran ismételt kérdések csatlakoztatott](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="743c3-152">For more information about OPC UA, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>
* <span data-ttu-id="743c3-153">A **térkép** , hogy minden eszköz megjeleníti hello helye csatlakoztatva toohello megoldás.</span><span class="sxs-lookup"><span data-stu-id="743c3-153">A **map** that displays hello location of each device connected toohello solution.</span></span> <span data-ttu-id="743c3-154">hello megoldás hello a Bing térképek API tooplot információt használhatja hello térképen.</span><span class="sxs-lookup"><span data-stu-id="743c3-154">hello solution can use hello Bing Maps API tooplot information on hello map.</span></span> <span data-ttu-id="743c3-155">Ha az előfizetéshez engedélyezve van a Bing Maps Enterprise API használata, a rendszer automatikusan ezt használja.</span><span class="sxs-lookup"><span data-stu-id="743c3-155">If your subscription is enabled for Bing Maps Enterprise API, then this feature is used automatically.</span></span> <span data-ttu-id="743c3-156">Ha nem, olvassa el a hello [gyakran ismételt kérdések] [ lnk-faq] toolearn hogyan toomake hello térkép dinamikus.</span><span class="sxs-lookup"><span data-stu-id="743c3-156">If not, see hello [FAQ][lnk-faq] toolearn how toomake hello map dynamic.</span></span>
* <span data-ttu-id="743c3-157">Egy **Alerts** (Riasztások) panelt, amely riasztásokat jelenít meg, ha egy telemetria- vagy OEE/KPI-érték meghalad egy adott küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="743c3-157">An **Alerts** panel that displays alerts generated when a telemetry or OEE/KPI value exceeds a specific threshold.</span></span>
* <span data-ttu-id="743c3-158">Egy **berendezések hatékonyságot** panel, amely azt mutatja be a teljes vállalati hello vagy hello gyári/éles hello OEE értékeit sor/állomás, a rendszer azért jelenítette.</span><span class="sxs-lookup"><span data-stu-id="743c3-158">An **Overall Equipment Efficiency** panel that shows hello OEE values for hello whole enterprise, or hello factory/production line/station you are viewing.</span></span> <span data-ttu-id="743c3-159">Ez az érték a hello állomás nézet toohello vállalati szinten összesíti.</span><span class="sxs-lookup"><span data-stu-id="743c3-159">This value is aggregated from hello station view toohello enterprise level.</span></span> <span data-ttu-id="743c3-160">hello OEE. ábra és a bennük foglalt elemek további elemzése.</span><span class="sxs-lookup"><span data-stu-id="743c3-160">hello OEE figure and its constituent elements can be further analyzed.</span></span>
* <span data-ttu-id="743c3-161">**A fő teljesítménymutatók** panel, amely előállított egységek és a teljes vállalati hello vagy hello gyári/termelési sor által használt energia hello számát mutatja, /, állomás tekinti meg.</span><span class="sxs-lookup"><span data-stu-id="743c3-161">**Key Performance Indicators** panel that displays hello number of units produced and energy used by hello whole enterprise or hello factory/production line/station you are viewing.</span></span> <span data-ttu-id="743c3-162">Ezek az értékek egy állomás nézet toohello vállalati szint összesítése.</span><span class="sxs-lookup"><span data-stu-id="743c3-162">These values are aggregated from a station view toohello enterprise level.</span></span>

## <a name="view-factories"></a><span data-ttu-id="743c3-163">Üzemek megtekintése</span><span class="sxs-lookup"><span data-stu-id="743c3-163">View factories</span></span>

<span data-ttu-id="743c3-164">Hello *előállítók* panel mutat be, akkor hello összes hello előállítók hello megoldás, az állapot és az üzemi aktuális konfigurációs földrajzi elhelyezkedését.</span><span class="sxs-lookup"><span data-stu-id="743c3-164">hello *Factories* panel shows you hello geographical location of all hello factories in hello solution, their status, and current production configuration.</span></span> <span data-ttu-id="743c3-165">Hello helyek listából navigálhat toohello hello megoldás hierarchiában lévő többi szinten.</span><span class="sxs-lookup"><span data-stu-id="743c3-165">From hello locations list, you can navigate toohello other levels in hello solution hierarchy.</span></span> <span data-ttu-id="743c3-166">hello hello lista sorai hivatkozások, amelyek az adott helyhez hello éles sorok részleteit.</span><span class="sxs-lookup"><span data-stu-id="743c3-166">hello rows in hello list are hyperlinks that link details of hello production lines at that location.</span></span> <span data-ttu-id="743c3-167">Akkor majd lehetséges toodrill hello termelési sor részleteinek és lefelé toohello állomás szint nézetében.</span><span class="sxs-lookup"><span data-stu-id="743c3-167">It is then possible toodrill into hello production line details and down toohello station level view.</span></span> <span data-ttu-id="743c3-168">Szűrő toohello listáját is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="743c3-168">You can also apply a filter toohello list.</span></span>

![Előre konfigurált csatlakoztatott gyár megoldás – gyárak][cf-img-factories] 

1. <span data-ttu-id="743c3-170">Hello **gyári panel** mutat be hello gyári lista ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="743c3-170">hello **Factory panel** shows hello factory list for this solution.</span></span>

2. <span data-ttu-id="743c3-171">hello gyári kezdetben listája hat előállítók hozta létre hello létesítésének folyamatát kell használnia.</span><span class="sxs-lookup"><span data-stu-id="743c3-171">hello factory list initially shows six factories created by hello provisioning process.</span></span> <span data-ttu-id="743c3-172">Hozzáadhat további szimulált és a fizikai eszközök toohello megoldás.</span><span class="sxs-lookup"><span data-stu-id="743c3-172">You can add additional simulated and physical devices toohello solution.</span></span>

3. <span data-ttu-id="743c3-173">a gyári tooview hello részleteit kattintson hello sorra hello gyári listában.</span><span class="sxs-lookup"><span data-stu-id="743c3-173">tooview hello details of a factory, click hello row in hello factory list.</span></span>

4. <span data-ttu-id="743c3-174">egy éles sor tooview hello részleteinek hello lista hello sorára kattintson.</span><span class="sxs-lookup"><span data-stu-id="743c3-174">tooview hello details of a production line, click hello row in hello list.</span></span>

5. <span data-ttu-id="743c3-175">tooview hello közzétett hello éles sor állomás OPC EE csomópontjára, kattintson a hello listában hello sort.</span><span class="sxs-lookup"><span data-stu-id="743c3-175">tooview hello published OPC UA nodes of a station on hello production line, click hello row in hello list.</span></span>

6. <span data-ttu-id="743c3-176">tooview részletekért hello állomás, az adott csomóponton hello lista hello sorára kattintson.</span><span class="sxs-lookup"><span data-stu-id="743c3-176">tooview details on a specific node in hello station, click hello row in hello list.</span></span> <span data-ttu-id="743c3-177">Ez a művelet elindítása hello környezetben panel idő adatsorozat Insights megjelenítésekkel.</span><span class="sxs-lookup"><span data-stu-id="743c3-177">This action launches hello context panel with Time Series Insights visualizations.</span></span> <span data-ttu-id="743c3-178">Kattintson a diagramok toodo további elemzés hello idő adatsorozat Insights explorer környezetben.</span><span class="sxs-lookup"><span data-stu-id="743c3-178">Click these graphs toodo further analysis in hello Time Series Insights explorer environment.</span></span>

## <a name="view-map"></a><span data-ttu-id="743c3-179">Térkép megtekintése</span><span class="sxs-lookup"><span data-stu-id="743c3-179">View map</span></span>

<span data-ttu-id="743c3-180">Ha az előfizetés a Bing térképek API access toohello, hello *előállítók* térkép meg hello földrajzi hely és állapotát jeleníti meg az összes hello előállítók hello megoldásban.</span><span class="sxs-lookup"><span data-stu-id="743c3-180">If your subscription has access toohello Bing Maps API, hello *Factories* map shows you hello geographical location and status of all hello factories in hello solution.</span></span> <span data-ttu-id="743c3-181">hello hely részleteinek, toodrill hello térképen hello helyek elemre.</span><span class="sxs-lookup"><span data-stu-id="743c3-181">toodrill into hello location details, click hello locations displayed on hello map.</span></span>

![Előre konfigurált csatlakoztatott gyár megoldás – térkép][cf-img-map]

## <a name="view-alerts"></a><span data-ttu-id="743c3-183">Riasztások megtekintése</span><span class="sxs-lookup"><span data-stu-id="743c3-183">View alerts</span></span>

<span data-ttu-id="743c3-184">Hello **riasztási** panelen láthatók miatt előállított riasztások tooa jelentett értékét vagy számított OEE/KPI értéke meghaladja a beállított küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="743c3-184">hello **Alert** panel shows you alerts generated due tooa reported value or a calculated OEE/KPI value exceeding its configured threshold.</span></span> <span data-ttu-id="743c3-185">A panel hello felvételekor hello állomás szint nézetében toohello globális nézet egyes szintjein riasztásokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="743c3-185">This panel displays alerts at each level of hello hierarchy, from hello station level view toohello global view.</span></span> <span data-ttu-id="743c3-186">hello riasztások hello riasztást, dátum, idő, helyét és előfordulási leírását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="743c3-186">hello alerts contain a description of hello alert, date, time, location, and number of occurrences.</span></span> <span data-ttu-id="743c3-187">Akkor értékes következtetéseket vonhat hello idő adatsorozat Insights adatainak használatával hello riasztást generáló toohello adatokban.</span><span class="sxs-lookup"><span data-stu-id="743c3-187">You can gain insights in toohello data that caused hello alert using hello Time Series Insights data.</span></span> <span data-ttu-id="743c3-188">hello idő adatsorozat Insights adatok formájában jelenik meg a hello riasztásokat, ha alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="743c3-188">hello Time Series Insights data is visualized in hello alerts where applicable.</span></span> <span data-ttu-id="743c3-189">Ha Ön rendszergazda, alapértelmezett műveletek hajthatók végre hello riasztások többek között:</span><span class="sxs-lookup"><span data-stu-id="743c3-189">If you are an Administrator, you can take default actions on hello alerts such as:</span></span>

* <span data-ttu-id="743c3-190">Bezárás hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="743c3-190">Close hello alert.</span></span>
* <span data-ttu-id="743c3-191">Tudomásul hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="743c3-191">Acknowledge hello alert.</span></span>

<span data-ttu-id="743c3-192">Igény szerint összetettebb műveleteket is végrehajthat.</span><span class="sxs-lookup"><span data-stu-id="743c3-192">Optionally, you can take more complex actions.</span></span> <span data-ttu-id="743c3-193">Például a hello nyomás OPC EE munkaterület hello szerelvény, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="743c3-193">For example, for hello Pressure OPC UA Node of hello Assembly you could:</span></span>

* <span data-ttu-id="743c3-194">Támogatási információkat jeleníthet meg egy weboldalon egy új böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="743c3-194">Show supporting information in a web page in a new browser window.</span></span>
* <span data-ttu-id="743c3-195">Enyhíteni hello riasztás hello okait hello eszközön egy OPC EE metódus meghívásával.</span><span class="sxs-lookup"><span data-stu-id="743c3-195">Mitigate hello cause of hello alert by calling an OPC UA method on hello device.</span></span>
* <span data-ttu-id="743c3-196">Ne jelenjen meg többé hello alapértelmezett műveletek hello rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="743c3-196">Suppress hello availability of hello default actions.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – riasztások][cf-img-alerts]

> [!NOTE]
> <span data-ttu-id="743c3-198">Ezek a riasztások akkor jönnek létre által előre konfigurált hello megoldásban a konfigurációs fájlban megadott feltételeket.</span><span class="sxs-lookup"><span data-stu-id="743c3-198">These alerts are generated by rules that are specified in a configuration file in hello preconfigured solution.</span></span> <span data-ttu-id="743c3-199">Ezek a szabályok riasztást generál, ha hello OEE vagy KPI számokat vagy OPC EE csomópont érték túllépte a beállított küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="743c3-199">These rules can generate alerts when hello OEE or KPI figures or OPC UA Node values are exceeding their configured threshold.</span></span>

1. <span data-ttu-id="743c3-200">Hello **riasztások panel** mutat be hello figyelmeztetéseket, ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="743c3-200">hello **Alerts panel** shows hello alerts generated in this solution.</span></span>

2. <span data-ttu-id="743c3-201">tooview hello részleteit a riasztást, kattintson a riasztások panelen hello hello riasztásra.</span><span class="sxs-lookup"><span data-stu-id="743c3-201">tooview hello details of an alert, click hello alert in hello alerts panel.</span></span>

3. <span data-ttu-id="743c3-202">toofurther hello riasztási adatok elemzésére, kattintson a hello graph hello riasztási panel tooopen hello idő adatsorozat Insights explorer környezetben.</span><span class="sxs-lookup"><span data-stu-id="743c3-202">toofurther analyze hello alert data, click hello graph in hello alert panel tooopen hello Time Series Insights explorer environment.</span></span>

4. <span data-ttu-id="743c3-203">tooaddress hello riasztást, és számos műveletet hello riasztási panelen elérhető.</span><span class="sxs-lookup"><span data-stu-id="743c3-203">tooaddress hello alert, several actions are available in hello alert panel.</span></span> <span data-ttu-id="743c3-204">Válassza ki a hello megfelelő beállítást, és kattintson a hello Akciógomb parancs végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="743c3-204">Choose hello appropriate option for you and click hello execute action command button.</span></span>

## <a name="view-overall-equipment-efficiency"></a><span data-ttu-id="743c3-205">A teljes eszközhatékonyság megtekintése</span><span class="sxs-lookup"><span data-stu-id="743c3-205">View overall equipment efficiency</span></span>

<span data-ttu-id="743c3-206">OEE díjszabás hello hatékonyságát hello gyártási folyamat egy kulcs éles kapcsolatos működési paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="743c3-206">OEE rates hello efficiency of hello manufacturing process using a key production-related operational parameters.</span></span> <span data-ttu-id="743c3-207">OEE az iparági szabványos mérték szorzásával hello rendelkezésre állási sebessége, teljesítmény sebessége és minőségének: OEE = x x minőségi rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="743c3-207">OEE is an industry standard measure calculated by multiplying hello availability rate, performance rate, and quality rate: OEE = availability x performance x quality.</span></span>

![Előre konfigurált csatlakoztatott gyár megoldás – OEE][cf-img-oee]

1. <span data-ttu-id="743c3-209">tooview OEE bármely szint hello hierarchiában, keresse meg a toohello specifikus nézet van szüksége.</span><span class="sxs-lookup"><span data-stu-id="743c3-209">tooview OEE for any level in hello hierarchy, navigate toohello specific view you require.</span></span> <span data-ttu-id="743c3-210">hello panel hello OEE százalékos alkotó hello elemmel együtt hello OEE, ha a nézet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="743c3-210">hello OEE for that view displays in hello panel along with each of hello elements that make up hello OEE percentage.</span></span>

2. <span data-ttu-id="743c3-211">toofurther hello OEE bármely szint hello hierarchia adatok elemzése, kattintson hello OEE, rendelkezésre állási, teljesítmény vagy a minőségi százalékos aránya.</span><span class="sxs-lookup"><span data-stu-id="743c3-211">toofurther analyze hello OEE for any level in hello hierarchy data, click either hello OEE, availability, performance, or quality percentage.</span></span> <span data-ttu-id="743c3-212">A környezet panel jelenik meg, amelyen idő adatsorozat Insights energiaforrással rendelkező képi megjelenítéseket, amelyek az elmúlt egy óra utolsó 24 óra és az elmúlt 7 napban hello adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="743c3-212">A context panel appears with Time Series Insights powered visualizations that shows data from hello last hour, last 24 hours, and last 7 days.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – TSI-vizualizáció][cf-img-tsi-visualization]

3. <span data-ttu-id="743c3-214">toofurther hello riasztási adatok elemzésére, hello graph hello riasztási panelen kattintson.</span><span class="sxs-lookup"><span data-stu-id="743c3-214">toofurther analyze hello alert data, click hello graph in hello alert panel.</span></span> <span data-ttu-id="743c3-215">Ez a művelet megnyitja hello idő adatsorozat Insights explorer környezetben.</span><span class="sxs-lookup"><span data-stu-id="743c3-215">This action opens hello Time Series Insights explorer environment.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – TSI Explorer][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a><span data-ttu-id="743c3-217">Fő teljesítménymutatók megtekintése</span><span class="sxs-lookup"><span data-stu-id="743c3-217">View Key Performance Indicators</span></span>

<span data-ttu-id="743c3-218">hello megoldás két fő teljesítménymutatók, biztosít *óránként egységek* és *kWh-ban használt energia*.</span><span class="sxs-lookup"><span data-stu-id="743c3-218">hello solution provides two key performance indicators, *units per hour* and *energy used in kWh*.</span></span>

![Előre konfigurált csatlakoztatott gyár megoldás – KPI][cf-img-kpi]

1. <span data-ttu-id="743c3-220">tooview egység / óra vagy energiát minden szint hello hierarchiában, keresse meg a toohello specifikus nézet van szüksége.</span><span class="sxs-lookup"><span data-stu-id="743c3-220">tooview units per hour or energy used for any level in hello hierarchy, navigate toohello specific view you require.</span></span> <span data-ttu-id="743c3-221">hello egység / óra és energia hello panelen megjelenített használt.</span><span class="sxs-lookup"><span data-stu-id="743c3-221">hello units per hour and energy used display in hello panel.</span></span>

2. <span data-ttu-id="743c3-222">tooanalyze egység / óra vagy energiát minden szint hello hierarchia további, kattintson a hello hello mérőműszer **a fő teljesítménymutatók** panel.</span><span class="sxs-lookup"><span data-stu-id="743c3-222">tooanalyze units per hour or energy used for any level in hello hierarchy further, click hello gauge in hello **Key Performance Indicators** panel.</span></span> <span data-ttu-id="743c3-223">A környezet panel hello utolsó óra utolsó 24 órában, és a legutóbbi 7 nap hello tooview adatok így már kapcsolva idő adatsorozat Insights megjelenítésekkel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="743c3-223">A context panel appears with Time Series Insights powered visualizations enabling you tooview data from hello last hour, hello last 24 hours, and last 7 days.</span></span>

## <a name="scenario-review"></a><span data-ttu-id="743c3-224">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="743c3-224">Scenario review</span></span>

<span data-ttu-id="743c3-225">Ebben a forgatókönyvben a előállítók OEE és a KPI-k, szereplő értékek hello irányítópult figyeli.</span><span class="sxs-lookup"><span data-stu-id="743c3-225">In this scenario, you monitored your factories OEE and KPIs values, in hello dashboard.</span></span> <span data-ttu-id="743c3-226">Majd használt idő adatsorozat Insights tooprovide további információt toohelp részletek további hello telemetriai adatokat a rendellenességeinek észlelésekor OEE és a KPI-k toohelp.</span><span class="sxs-lookup"><span data-stu-id="743c3-226">You then used Time Series Insights tooprovide more information toohelp drill further into hello telemetry data for OEE and KPIs toohelp with detecting anomalies.</span></span> <span data-ttu-id="743c3-227">A előállítók hello riasztási panel tooview problémákat is használt, és hello műveletek elérhető tooyou tooresolve hello riasztás használta.</span><span class="sxs-lookup"><span data-stu-id="743c3-227">You also used hello alert panel tooview issues with your factories and you used hello actions available tooyou tooresolve hello alert.</span></span>

## <a name="other-features"></a><span data-ttu-id="743c3-228">Egyéb jellemzők</span><span class="sxs-lookup"><span data-stu-id="743c3-228">Other features</span></span>

<span data-ttu-id="743c3-229">a következő szakaszok hello csatlakoztatott hello gyári megoldás néhány további szolgáltatásokat, nem ismerteti a fentebbi szituáció hello ismertetik.</span><span class="sxs-lookup"><span data-stu-id="743c3-229">hello following sections describe some additional features of hello connected factory solution that are not described in hello previous scenario.</span></span>

## <a name="apply-filters"></a><span data-ttu-id="743c3-230">Szűrők alkalmazása</span><span class="sxs-lookup"><span data-stu-id="743c3-230">Apply filters</span></span>

1. <span data-ttu-id="743c3-231">Kattintson a hello **sávnyíl** toodisplay vagy hello gyári helyek vagy hello riasztások panelen rendelkezésre álló szűrők listája.</span><span class="sxs-lookup"><span data-stu-id="743c3-231">Click hello **chevron** toodisplay a list of available filters in either hello factory locations panel or hello alerts panel.</span></span>

2. <span data-ttu-id="743c3-232">az Ön hello szűrők panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="743c3-232">hello filters panel is displayed for you.</span></span> 

    ![Előre konfigurált csatlakoztatott gyár megoldás – szűrők][cf-img-alert-filter]

3. <span data-ttu-id="743c3-234">Válassza ki a szükséges hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="743c3-234">Choose hello filter that you require.</span></span> <span data-ttu-id="743c3-235">Akkor is lehetséges tootype szabad szöveg hello szűrő mezőkbe.</span><span class="sxs-lookup"><span data-stu-id="743c3-235">It is also possible tootype free text into hello filter fields.</span></span>

4. <span data-ttu-id="743c3-236">hello majd szűrőhöz meg.</span><span class="sxs-lookup"><span data-stu-id="743c3-236">hello filter is then applied for you.</span></span> <span data-ttu-id="743c3-237">hello irányítópulton keresztül a tölcsér hello előállítók jeleníti meg, és riasztást küld a táblák hello szűrő állapota is látható.</span><span class="sxs-lookup"><span data-stu-id="743c3-237">hello filter state is also shown in hello dashboard via a funnel that displays in hello factories and alerts tables.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – szűrők][cf-img-alert-filter-funnel]

    > [!NOTE]
    > <span data-ttu-id="743c3-239">Aktív szűrő nem befolyásolja a megjelenő hello OEE és KPI értékek, csak szűri hello tartalmának listázása.</span><span class="sxs-lookup"><span data-stu-id="743c3-239">An active filter does not affect hello displayed OEE and KPI values, it only filters hello list contents.</span></span>

5. <span data-ttu-id="743c3-240">tooclear szűrőt, kattintson a hello tölcsér, és kattintson a szűrő hello szűrő környezetben panelen.</span><span class="sxs-lookup"><span data-stu-id="743c3-240">tooclear a filter, click hello funnel and click filter in hello filter context panel.</span></span> <span data-ttu-id="743c3-241">szöveg hello **összes** hello előállítók jelenik meg, és riasztást küld a táblákat.</span><span class="sxs-lookup"><span data-stu-id="743c3-241">hello text **All** is displayed in hello factories and alerts tables.</span></span>

## <a name="browse-an-opc-ua-server"></a><span data-ttu-id="743c3-242">OPC UA-kiszolgáló tallózása</span><span class="sxs-lookup"><span data-stu-id="743c3-242">Browse an OPC UA server</span></span>

<span data-ttu-id="743c3-243">Előre konfigurált hello megoldás telepítésekor automatikusan létesítsen keresztül hello megoldás böngésző tallózással szimulált OPC EE-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="743c3-243">When you deploy hello preconfigured solution, you automatically provision simulated OPC UA servers that you can browse via hello solution browser.</span></span> <span data-ttu-id="743c3-244">Ezek a kiszolgálók *szimulált OPC UA-kiszolgálók*.</span><span class="sxs-lookup"><span data-stu-id="743c3-244">These servers are *simulated OPC UA servers*.</span></span> <span data-ttu-id="743c3-245">Szimulált kiszolgálók könnyítheti meg tooexperiment előre konfigurált hello megoldással hello kell toodeploy valódi, fizikai kiszolgálók nélkül.</span><span class="sxs-lookup"><span data-stu-id="743c3-245">Simulated servers make it easy for you tooexperiment with hello preconfigured solution without hello need toodeploy real, physical servers.</span></span> <span data-ttu-id="743c3-246">Ha szeretné, hogy valós OPC EE-kiszolgálói toohello megoldás tooconnect, tekintse meg a hello [kapcsolni a OPC EE eszköz csatlakoztatva toohello előre konfigurált gyári megoldást] [ lnk-connect-cf] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="743c3-246">If you do want tooconnect a real OPC UA server toohello solution, see hello [Connect your OPC UA device toohello connected factory preconfigured solution][lnk-connect-cf] tutorial.</span></span>

1. <span data-ttu-id="743c3-247">Kattintson a hello **gyári ikon** hello irányítópult navigációs sávban.</span><span class="sxs-lookup"><span data-stu-id="743c3-247">Click hello **factory icon** in hello dashboard navigation bar.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – kiszolgálótallózó][cf-img-server-browser]

2. <span data-ttu-id="743c3-249">Hello előre konfigurált listából válasszon hello kiszolgálók közül.</span><span class="sxs-lookup"><span data-stu-id="743c3-249">Choose one of hello servers from hello preconfigured list.</span></span> <span data-ttu-id="743c3-250">A lista mutatja meg az előre konfigurált hello megoldás telepített hello kiszolgálókat.</span><span class="sxs-lookup"><span data-stu-id="743c3-250">This list shows hello servers that are deployed for you in hello preconfigured solution.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – kiválasztott kiszolgálók][cf-img-server-choice]

3. <span data-ttu-id="743c3-252">Kattintson a **Connect** (Csatlakozás) gombra. Megjelenik egy biztonsági párbeszédablak.</span><span class="sxs-lookup"><span data-stu-id="743c3-252">Click **Connect**, a security dialog displays.</span></span> <span data-ttu-id="743c3-253">Hello szimuláció, biztonságos tooclick **folytatása**</span><span class="sxs-lookup"><span data-stu-id="743c3-253">For hello simulation, it is safe tooclick **Proceed**</span></span>

4. <span data-ttu-id="743c3-254">tooexpand bármelyik hello csomópontok hello kiszolgáló csomópontjára, kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="743c3-254">tooexpand any of hello nodes in hello server tree, click it.</span></span> <span data-ttu-id="743c3-255">A telemetriaadatokat közzétevő csomópontokat egy pipa jelöli.</span><span class="sxs-lookup"><span data-stu-id="743c3-255">Nodes that are publishing telemetry have a tick mark beside them.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – kiszolgálófa][cf-img-server-tree]

5. <span data-ttu-id="743c3-257">Kattintson a jobb gombbal egy cikk tooread, írása, közzététele és hívható meg, hogy a csomópont.</span><span class="sxs-lookup"><span data-stu-id="743c3-257">Right-click an item tooread, write, publish, or call that node.</span></span> <span data-ttu-id="743c3-258">hello műveletek elérhető tooyou az engedélyeit, és hello attribútumok hello csomópont függ.</span><span class="sxs-lookup"><span data-stu-id="743c3-258">hello actions available tooyou depend on your permissions and hello attributes of hello node.</span></span> <span data-ttu-id="743c3-259">hello olvasható beállítás toodisplays egy adott csomópont hello hello értéket megjelenítő környezetben panel.</span><span class="sxs-lookup"><span data-stu-id="743c3-259">hello read option toodisplays a context panel showing hello value of hello specific node.</span></span> <span data-ttu-id="743c3-260">hello írható beállítást jeleníti meg a környezetben panel, ahol új értéket adhat meg.</span><span class="sxs-lookup"><span data-stu-id="743c3-260">hello write option displays a context panel where you can enter a new value.</span></span> <span data-ttu-id="743c3-261">hello hívás lehetőséget választja, megnyílik egy csomópont be hello paraméterek hello hívásához.</span><span class="sxs-lookup"><span data-stu-id="743c3-261">hello call option displays a node where you can enter hello parameters for hello call.</span></span>

## <a name="publish-a-node"></a><span data-ttu-id="743c3-262">Csomópont közzététele</span><span class="sxs-lookup"><span data-stu-id="743c3-262">Publish a node</span></span>

<span data-ttu-id="743c3-263">Ha megnyitja egy *szimulált OPC EE-kiszolgáló*, dönthet úgy is toopublish új csomópontok.</span><span class="sxs-lookup"><span data-stu-id="743c3-263">When you browse a *simulated OPC UA server*, you can also choose toopublish new nodes.</span></span> <span data-ttu-id="743c3-264">Ezek a csomópontok hello megoldásban a hello telemetriai elemezheti.</span><span class="sxs-lookup"><span data-stu-id="743c3-264">You can analyze hello telemetry from these nodes in hello solution.</span></span> <span data-ttu-id="743c3-265">Ezek *OPC EE-kiszolgálók szimulált* könnyebben előre konfigurált hello megoldással egyszerűen tooexperiment valódi, fizikai eszközök telepítése nélkül.</span><span class="sxs-lookup"><span data-stu-id="743c3-265">These *simulated OPC UA servers* make it easy tooexperiment with hello preconfigured solution without deploying real, physical devices.</span></span>

1. <span data-ttu-id="743c3-266">Keresse meg, hogy kívánja-e toopublish OPC EE böngésző fa hello tooa csomópontja.</span><span class="sxs-lookup"><span data-stu-id="743c3-266">Browse tooa node in hello OPC UA server browser tree that you wish toopublish.</span></span>

2. <span data-ttu-id="743c3-267">Kattintson a jobb gombbal hello csomópont.</span><span class="sxs-lookup"><span data-stu-id="743c3-267">Right-click hello node.</span></span>

3. <span data-ttu-id="743c3-268">Válassza a **Publish** (Közzététel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="743c3-268">Choose **Publish**.</span></span>

    ![A csatlakoztatott gyár közzétesz egy csomópontot][cf-img-publish-node]

4. <span data-ttu-id="743c3-270">A környezet panel jelenik meg, amely közli, hogy hello közzététele sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="743c3-270">A context panel appears which tells you that hello publish has succeeded.</span></span> <span data-ttu-id="743c3-271">hello csomópont hello állomás szint nézetében mellette jelölve jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="743c3-271">hello node appears in hello station level view with a check mark beside it.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – sikeres közzététel][cf-img-publish-success]

## <a name="command-and-control"></a><span data-ttu-id="743c3-273">Parancs és vezérlés</span><span class="sxs-lookup"><span data-stu-id="743c3-273">Command and control</span></span>

<span data-ttu-id="743c3-274">hello csatlakoztatott gyári lehetővé teszi a parancsot, és az iparág eszközök közvetlenül a hello felhőből szabályozzák.</span><span class="sxs-lookup"><span data-stu-id="743c3-274">hello connected factory allows you command and control your industry devices directly from hello cloud.</span></span> <span data-ttu-id="743c3-275">Ez a szolgáltatás toorespond tooalerts hello eszköz állítja elő is használhatja.</span><span class="sxs-lookup"><span data-stu-id="743c3-275">You can use this feature toorespond tooalerts generated by hello device.</span></span> <span data-ttu-id="743c3-276">Például egy parancs toohello eszköz sikerült elküldeni a hello felhőből.</span><span class="sxs-lookup"><span data-stu-id="743c3-276">For example, you could send a command toohello device from hello cloud.</span></span> <span data-ttu-id="743c3-277">Hello elérhető parancsok az található hello **StationCommands** hello OPC EE kiszolgálók böngésző fa csomópontja.</span><span class="sxs-lookup"><span data-stu-id="743c3-277">You can find hello available commands in hello **StationCommands** node in hello OPC UA servers browser tree.</span></span> <span data-ttu-id="743c3-278">Ebben a forgatókönyvben egy éles sor München hello szerelvény állomáson nyomás kiadás szelep nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="743c3-278">In this scenario, you open a pressure release valve on hello assembly station of a production line in Munich.</span></span> <span data-ttu-id="743c3-279">toouse hello parancs funkciói, kell lennie a hello **rendszergazda** hello szerepkör előre konfigurált megoldás üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="743c3-279">toouse hello command and control functionality, you must be in hello **Administrator** role for hello preconfigured solution deployment.</span></span>

1. <span data-ttu-id="743c3-280">Keresse meg a toohello **StationCommands** hello OPC EE böngésző fa csomópontja.</span><span class="sxs-lookup"><span data-stu-id="743c3-280">Browse toohello **StationCommands** node in hello OPC UA server browser tree.</span></span>

2. <span data-ttu-id="743c3-281">Válassza ki, hogy kívánja-e használatban hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="743c3-281">Choose hello command that you wish use.</span></span>

3. <span data-ttu-id="743c3-282">Kattintson a jobb gombbal hello csomópont.</span><span class="sxs-lookup"><span data-stu-id="743c3-282">Right-click hello node.</span></span>

4. <span data-ttu-id="743c3-283">Válassza a **Call** (Hívás) parancsot.</span><span class="sxs-lookup"><span data-stu-id="743c3-283">Choose **Call**.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – hívás parancs][cf-img-call-command]

5. <span data-ttu-id="743c3-285">A környezet panel jelenik meg, tájékoztat módszer toocall, és minden paraméter adatát kell alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="743c3-285">A context panel appears informing you which method you are about toocall and any parameter details is applicable.</span></span>

6. <span data-ttu-id="743c3-286">Válassza a **Call** (Hívás) parancsot.</span><span class="sxs-lookup"><span data-stu-id="743c3-286">Choose **Call**.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – hívás helyi panelje][cf-img-call-context]

7. <span data-ttu-id="743c3-288">hello környezetben panel egy frissített tooinform, hogy a metódus hívása hello sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="743c3-288">hello context panel is updated tooinform you that hello method call succeeded.</span></span> <span data-ttu-id="743c3-289">Hello hívás sikeres hello nyomás csomópont hello hívása miatt frissített hello érték beolvasásával ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="743c3-289">You can verify hello call succeeded by reading hello value of hello pressure node that updated as a result of hello call.</span></span>

    ![Előre konfigurált csatlakoztatott gyár megoldás – sikeres hívás][cf-img-call-success]


## <a name="behind-hello-scenes"></a><span data-ttu-id="743c3-291">Hello háttérben</span><span class="sxs-lookup"><span data-stu-id="743c3-291">Behind hello scenes</span></span>

<span data-ttu-id="743c3-292">Amikor telepít egy előre beállított megoldás, hello központi telepítési folyamat több erőforrást hello kijelölt Azure-előfizetés hoz létre.</span><span class="sxs-lookup"><span data-stu-id="743c3-292">When you deploy a preconfigured solution, hello deployment process creates multiple resources in hello Azure subscription you selected.</span></span> <span data-ttu-id="743c3-293">Megtekintheti ezeket az erőforrásokat a hello Azure [portal][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="743c3-293">You can view these resources in hello Azure [portal][lnk-portal].</span></span> <span data-ttu-id="743c3-294">hello központi telepítési folyamat létrehoz egy **erőforráscsoport** előkonfigurált megoldást választja hello neve alapján néven:</span><span class="sxs-lookup"><span data-stu-id="743c3-294">hello deployment process creates a **resource group** with a name based on hello name you choose for your preconfigured solution:</span></span>

![Hello Azure-portálon az előkonfigurált megoldás][img-cf-portal]

<span data-ttu-id="743c3-296">Megtekintheti a hello-beállítások az egyes erőforrások hello az erőforrások listájához a hello erőforráscsoport lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="743c3-296">You can view hello settings of each resource by selecting it in hello list of resources in hello resource group.</span></span>

<span data-ttu-id="743c3-297">Előre konfigurált hello megoldás forráskódját hello is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="743c3-297">You can also view hello source code for hello preconfigured solution.</span></span> <span data-ttu-id="743c3-298">hello előre konfigurált csatlakoztatott gyári megoldás forráskódját van hello [azure iot-csatlakoztatott-gyári] [ lnk-cfgithub] GitHub-tárházban:</span><span class="sxs-lookup"><span data-stu-id="743c3-298">hello connected factory preconfigured solution source code is in hello [azure-iot-connected-factory][lnk-cfgithub] GitHub repository:</span></span>

<span data-ttu-id="743c3-299">Amikor elkészült, hogy törli előre konfigurált hello megoldás az Azure-előfizetéshez hello [azureiotsuite.com] [ lnk-azureiotsuite] hely.</span><span class="sxs-lookup"><span data-stu-id="743c3-299">When you are done, you can delete hello preconfigured solution from your Azure subscription on hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="743c3-300">Ez a hely összes hello előre konfigurált hello megoldás létrehozása után kiépített erőforrások tooeasily törlése lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="743c3-300">This site enables you tooeasily delete all hello resources that were provisioned when you created hello preconfigured solution.</span></span>

> [!NOTE]
> <span data-ttu-id="743c3-301">tooensure törlése minden kapcsolódó előre konfigurált toohello megoldás, törölje a hello [azureiotsuite.com] [ lnk-azureiotsuite] hely.</span><span class="sxs-lookup"><span data-stu-id="743c3-301">tooensure that you delete everything related toohello preconfigured solution, delete it on hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="743c3-302">Ne törölje az erőforráscsoportot hello hello portálon.</span><span class="sxs-lookup"><span data-stu-id="743c3-302">Do not delete hello resource group in hello portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="743c3-303">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="743c3-303">Next Steps</span></span>

<span data-ttu-id="743c3-304">Most, hogy egy előre konfigurált működő megoldást telepítése után, továbbra is a következő cikkek hello olvasásával Ismerkedés az IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="743c3-304">Now that you’ve deployed a working preconfigured solution, you can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="743c3-305">[Előre konfigurált csatlakoztatott gyár megoldás – bemutató][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="743c3-305">[Connected factory preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="743c3-306">[Csatlakozás a toohello előre konfigurált csatlakoztatott gyári megoldás][lnk-connect-cf]</span><span class="sxs-lookup"><span data-stu-id="743c3-306">[Connect your device toohello Connected factory preconfigured solution][lnk-connect-cf]</span></span>
* <span data-ttu-id="743c3-307">[Engedélyek hello azureiotsuite.com webhelyen][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="743c3-307">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>

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
---
title: "Élő ASP.NET-webapp figyelése az Azure Application Insights segítségével | Microsoft Docs"
description: "Megfigyelheti egy webhely teljesítményét annak ismételt üzembe helyezése nélkül. A helyszíni, valamint a virtuális gépeken, illetve az Azure-ban üzemeltetett ASP.NET-webappokhoz is használható."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: d07a0c81f89100c378456bbea8dca1c009cc8d77
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a><span data-ttu-id="49107-104">Webalkalmazások futásidejű kialakítása az Application Insights használatával</span><span class="sxs-lookup"><span data-stu-id="49107-104">Instrument web apps at runtime with Application Insights</span></span>


<span data-ttu-id="49107-105">Egy élő webalkalmazást a kód módosítása vagy ismételt telepítése nélkül is kialakíthat az Azure Application Insights használatával.</span><span class="sxs-lookup"><span data-stu-id="49107-105">You can instrument a live web app with Azure Application Insights, without having to modify or redeploy your code.</span></span> <span data-ttu-id="49107-106">Ha az alkalmazásokat egy helyszíni IIS-kiszolgáló működteti, telepítse az Állapotfigyelőt.</span><span class="sxs-lookup"><span data-stu-id="49107-106">If your apps are hosted by an on-premises IIS server, install Status Monitor.</span></span> <span data-ttu-id="49107-107">Ha Azure-webalkalmazások, illetve egy Azure VM-en futnak, az Azure vezérlőpultján bekapcsolhatja az Application Insights-figyelést.</span><span class="sxs-lookup"><span data-stu-id="49107-107">If they're Azure web apps or run in an Azure VM, you can switch on Application Insights monitoring from the Azure control panel.</span></span> <span data-ttu-id="49107-108">(Külön cikkek érhetők el az [élő J2EE-webalkalmazások](app-insights-java-live.md) és az [Azure Cloud Services](app-insights-cloudservices.md) kialakításáról.) Ehhez [Microsoft Azure](http://azure.com)-előfizetésre van szükség.</span><span class="sxs-lookup"><span data-stu-id="49107-108">(There are also separate articles about instrumenting [live J2EE web apps](app-insights-java-live.md) and [Azure Cloud Services](app-insights-cloudservices.md).) You need a [Microsoft Azure](http://azure.com) subscription.</span></span>

![mintadiagramok](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

<span data-ttu-id="49107-110">Háromféleképpen alkalmazhatja az Application Insights szolgáltatást a .NET-webalkalmazásokra:</span><span class="sxs-lookup"><span data-stu-id="49107-110">You have a choice of three routes to apply Application Insights to your .NET web applications:</span></span>

* <span data-ttu-id="49107-111">**Felépítési idő:** [Adja az Application Insights SDK-t][greenbrown] a webalkalmazás kódjához.</span><span class="sxs-lookup"><span data-stu-id="49107-111">**Build time:** [Add the Application Insights SDK][greenbrown] to your web app code.</span></span>
* <span data-ttu-id="49107-112">**Futási idő:** A webalkalmazását a kiszolgálón, az alábbiakban leírtak szerint, a kód újraépítése és újratelepítése nélkül alakíthatja ki.</span><span class="sxs-lookup"><span data-stu-id="49107-112">**Run time:** Instrument your web app on the server, as described below, without rebuilding and redeploying the code.</span></span>
* <span data-ttu-id="49107-113">**Mindkettő:** Az SDK-t beépítheti a webalkalmazás-kódba, és alkalmazhatja a futásidejű bővítményeket is.</span><span class="sxs-lookup"><span data-stu-id="49107-113">**Both:** Build the SDK into your web app code, and also apply the run-time extensions.</span></span> <span data-ttu-id="49107-114">Így mindkét lehetőség előnyeivel élhet.</span><span class="sxs-lookup"><span data-stu-id="49107-114">Get the best of both options.</span></span>

<span data-ttu-id="49107-115">Itt található egy összefoglaló az egyes módszerek eredményeiről:</span><span class="sxs-lookup"><span data-stu-id="49107-115">Here's a summary of what you get by each route:</span></span>

|  | <span data-ttu-id="49107-116">Felépítési idő</span><span class="sxs-lookup"><span data-stu-id="49107-116">Build time</span></span> | <span data-ttu-id="49107-117">Futási idő</span><span class="sxs-lookup"><span data-stu-id="49107-117">Run time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="49107-118">Kérések és kivételek</span><span class="sxs-lookup"><span data-stu-id="49107-118">Requests & exceptions</span></span> |<span data-ttu-id="49107-119">Igen</span><span class="sxs-lookup"><span data-stu-id="49107-119">Yes</span></span> |<span data-ttu-id="49107-120">Igen</span><span class="sxs-lookup"><span data-stu-id="49107-120">Yes</span></span> |
| [<span data-ttu-id="49107-121">Részletes kivételek</span><span class="sxs-lookup"><span data-stu-id="49107-121">More detailed exceptions</span></span>](app-insights-asp-net-exceptions.md) | |<span data-ttu-id="49107-122">Igen</span><span class="sxs-lookup"><span data-stu-id="49107-122">Yes</span></span> |
| [<span data-ttu-id="49107-123">Függőségek diagnosztikája</span><span class="sxs-lookup"><span data-stu-id="49107-123">Dependency diagnostics</span></span>](app-insights-asp-net-dependencies.md) |<span data-ttu-id="49107-124">.NET 4.6+ esetén, kevésbé részletesen</span><span class="sxs-lookup"><span data-stu-id="49107-124">On .NET 4.6+, but less detail</span></span> |<span data-ttu-id="49107-125">Igen, teljes részletesség: eredménykódok, SQL-parancsszöveg, HTTP-parancsok</span><span class="sxs-lookup"><span data-stu-id="49107-125">Yes, full detail: result codes, SQL command text, HTTP verb</span></span>|
| [<span data-ttu-id="49107-126">Rendszerteljesítmény-számlálók</span><span class="sxs-lookup"><span data-stu-id="49107-126">System performance counters</span></span>](app-insights-performance-counters.md) |<span data-ttu-id="49107-127">Igen</span><span class="sxs-lookup"><span data-stu-id="49107-127">Yes</span></span> |<span data-ttu-id="49107-128">Igen</span><span class="sxs-lookup"><span data-stu-id="49107-128">Yes</span></span> |
| <span data-ttu-id="49107-129">[API egyéni telemetriához][api]</span><span class="sxs-lookup"><span data-stu-id="49107-129">[API for custom telemetry][api]</span></span> |<span data-ttu-id="49107-130">Igen</span><span class="sxs-lookup"><span data-stu-id="49107-130">Yes</span></span> |<span data-ttu-id="49107-131">Nem</span><span class="sxs-lookup"><span data-stu-id="49107-131">No</span></span> |
| [<span data-ttu-id="49107-132">Nyomkövetési napló integrációja</span><span class="sxs-lookup"><span data-stu-id="49107-132">Trace log integration</span></span>](app-insights-asp-net-trace-logs.md) |<span data-ttu-id="49107-133">Igen</span><span class="sxs-lookup"><span data-stu-id="49107-133">Yes</span></span> |<span data-ttu-id="49107-134">Nem</span><span class="sxs-lookup"><span data-stu-id="49107-134">No</span></span> |
| [<span data-ttu-id="49107-135">Lapmegtekintések és felhasználói adatok</span><span class="sxs-lookup"><span data-stu-id="49107-135">Page view & user data</span></span>](app-insights-javascript.md) |<span data-ttu-id="49107-136">Igen</span><span class="sxs-lookup"><span data-stu-id="49107-136">Yes</span></span> |<span data-ttu-id="49107-137">Nem</span><span class="sxs-lookup"><span data-stu-id="49107-137">No</span></span> |
| <span data-ttu-id="49107-138">Szükség van a kód ismételt felépítésére</span><span class="sxs-lookup"><span data-stu-id="49107-138">Need to rebuild code</span></span> |<span data-ttu-id="49107-139">Igen</span><span class="sxs-lookup"><span data-stu-id="49107-139">Yes</span></span> | <span data-ttu-id="49107-140">Nem</span><span class="sxs-lookup"><span data-stu-id="49107-140">No</span></span> |


## <a name="monitor-a-live-azure-web-app"></a><span data-ttu-id="49107-141">Élő Azure-webapp figyelése</span><span class="sxs-lookup"><span data-stu-id="49107-141">Monitor a live Azure web app</span></span>

<span data-ttu-id="49107-142">Ha az alkalmazás Azure-webszolgáltatásként fut, a következőképpen kapcsolható be a figyelése:</span><span class="sxs-lookup"><span data-stu-id="49107-142">If your application is running as an Azure web service, here's how to switch on monitoring:</span></span>

* <span data-ttu-id="49107-143">Válassza az Application Insights szolgáltatást az alkalmazás vezérlőpultján az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="49107-143">Select Application Insights on the app's control panel in Azure.</span></span>

    ![Az Application Insights beállítása egy Azure-webapphoz](./media/app-insights-monitor-performance-live-website-now/azure-web-setup.png)
* <span data-ttu-id="49107-145">Amikor megnyílik az Application Insights összegző lapja, kattintson a lap alján lévő hivatkozásra a teljes Application Insights-erőforrás megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="49107-145">When the Application Insights summary page opens, click the link at the bottom to open the full Application Insights resource.</span></span>

    ![Az Application Insights elérése kattintással](./media/app-insights-monitor-performance-live-website-now/azure-web-view-more.png)

<span data-ttu-id="49107-147">[Felhőben és virtuális gépeken futó alkalmazások figyelése](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="49107-147">[Monitoring Cloud and VM apps](app-insights-azure.md).</span></span>

### <a name="enable-client-side-monitoring-in-azure"></a><span data-ttu-id="49107-148">Az ügyféloldali figyelés engedélyezése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="49107-148">Enable client-side monitoring in Azure</span></span>

<span data-ttu-id="49107-149">Ha engedélyezte az Application Insights szolgáltatást az Azure-ban, felveheti a lapmegtekintéseket és a felhasználók telemetriai adatait.</span><span class="sxs-lookup"><span data-stu-id="49107-149">If you have enabled Application Insights in Azure, you can add page view and user telemetry.</span></span>

1. <span data-ttu-id="49107-150">Válassza a Beállítások > Alkalmazásbeállítások lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="49107-150">Select Settings > Application Settings</span></span>
2.  <span data-ttu-id="49107-151">Az alkalmazásbeállításoknál adjon meg egy új kulcs-érték párt:</span><span class="sxs-lookup"><span data-stu-id="49107-151">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="49107-152">Kulcs: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="49107-152">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="49107-153">Érték: `true`</span><span class="sxs-lookup"><span data-stu-id="49107-153">Value: `true`</span></span>
3. <span data-ttu-id="49107-154">**Mentse** a beállításokat, és **indítsa újra** az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="49107-154">**Save** the settings and **Restart** your app.</span></span>

<span data-ttu-id="49107-155">Ezzel elhelyezte minden weblapon az Application Insights JavaScript SDK-t.</span><span class="sxs-lookup"><span data-stu-id="49107-155">The Application Insights JavaScript SDK is now injected into each web page.</span></span>

## <a name="monitor-a-live-iis-web-app"></a><span data-ttu-id="49107-156">Élő IIS-webapp figyelése</span><span class="sxs-lookup"><span data-stu-id="49107-156">Monitor a live IIS web app</span></span>

<span data-ttu-id="49107-157">Ha az alkalmazás egy IIS-kiszolgálón fut, engedélyezze az Application Insightst az Állapotfigyelő használatával.</span><span class="sxs-lookup"><span data-stu-id="49107-157">If your app is hosted on an IIS server, enable Application Insights by using Status Monitor.</span></span>

1. <span data-ttu-id="49107-158">Az IIS-webkiszolgálón jelentkezzen be rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="49107-158">On your IIS web server, sign in with administrator credentials.</span></span>
2. <span data-ttu-id="49107-159">Ha az Application Insights Állapotfigyelő még nincs telepítve, töltse le és futtassa az [Állapotfigyelő telepítőjét](http://go.microsoft.com/fwlink/?LinkId=506648) (vagy futtassa a [Webplatform-telepítőt](https://www.microsoft.com/web/downloads/platform.aspx), és keresse meg benne az Application Insights Állapotfigyelőt).</span><span class="sxs-lookup"><span data-stu-id="49107-159">If Application Insights Status Monitor is not already installed, download and run the [Status Monitor installer](http://go.microsoft.com/fwlink/?LinkId=506648) (or run [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) and search in it for Application Insights Status Monitor).</span></span>
3. <span data-ttu-id="49107-160">Az Állapotfigyelőben válassza ki a megfigyelni kívánt telepített webappot vagy webhelyet.</span><span class="sxs-lookup"><span data-stu-id="49107-160">In Status Monitor, select the installed web application or website that you want to monitor.</span></span> <span data-ttu-id="49107-161">Jelentkezzen be az Azure-beli hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="49107-161">Sign in with your Azure credentials.</span></span>

    <span data-ttu-id="49107-162">Konfigurálja az erőforrást, amelyben az eredményeket látni szeretné az Application Insights portálon.</span><span class="sxs-lookup"><span data-stu-id="49107-162">Configure the resource where you want to see the results in the Application Insights portal.</span></span> <span data-ttu-id="49107-163">(Általában az a legjobb megoldás, ha létrehoz egy új erőforrást.</span><span class="sxs-lookup"><span data-stu-id="49107-163">(Normally, it's best to create a new resource.</span></span> <span data-ttu-id="49107-164">Meglévő erőforrást akkor válasszon, ha már rendelkezik [webes tesztekkel][availability] vagy [ügyfélfigyeléssel][client] az alkalmazáshoz.)</span><span class="sxs-lookup"><span data-stu-id="49107-164">Select an existing resource if you already have [web tests][availability] or [client monitoring][client] for this app.)</span></span> 

    ![Válasszon egy alkalmazást és egy erőforrást.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. <span data-ttu-id="49107-166">Indítsa újra az IIS-t.</span><span class="sxs-lookup"><span data-stu-id="49107-166">Restart IIS.</span></span>

    ![Válassza az Újraindítás gombot a párbeszédpanel tetején.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    <span data-ttu-id="49107-168">A webszolgáltatása rövid időre megszakad.</span><span class="sxs-lookup"><span data-stu-id="49107-168">Your web service is interrupted for a short while.</span></span>

## <a name="customize-monitoring-options"></a><span data-ttu-id="49107-169">A megfigyelési beállítások testreszabása</span><span class="sxs-lookup"><span data-stu-id="49107-169">Customize monitoring options</span></span>

<span data-ttu-id="49107-170">Az Application Insights engedélyezése DLL-eket és az ApplicationInsights.config fájlt adja hozzá a webapphoz.</span><span class="sxs-lookup"><span data-stu-id="49107-170">Enabling Application Insights adds DLLs and ApplicationInsights.config to your web app.</span></span> <span data-ttu-id="49107-171">A [.config fájl szerkesztésével](app-insights-configuration-with-applicationinsights-config.md) bizonyos beállítások módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="49107-171">You can [edit the .config file](app-insights-configuration-with-applicationinsights-config.md) to change some of the options.</span></span>

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a><span data-ttu-id="49107-172">Az Application Insights ismételt engedélyezése az alkalmazás ismételt közzétételekor</span><span class="sxs-lookup"><span data-stu-id="49107-172">When you re-publish your app, re-enable Application Insights</span></span>

<span data-ttu-id="49107-173">Mielőtt újra közzéteszi az alkalmazást, fontolja [az Application Insights hozzáadását a kódhoz a Visual Studióban][greenbrown].</span><span class="sxs-lookup"><span data-stu-id="49107-173">Before you re-publish your app, consider [adding Application Insights to the code in Visual Studio][greenbrown].</span></span> <span data-ttu-id="49107-174">Ezzel részletesebb telemetriához jut, és egyéni telemetriát is írhat.</span><span class="sxs-lookup"><span data-stu-id="49107-174">You'll get more detailed telemetry and the ability to write custom telemetry.</span></span>

<span data-ttu-id="49107-175">Ha anélkül szeretné újra közzétenni az alkalmazást, hogy a kódhoz hozzáadná az Application Insightst, vegye figyelembe, hogy az üzembehelyezési folyamat törölheti a DLL-eket és az ApplicationInsights.config fájlt a közzétett webhelyről.</span><span class="sxs-lookup"><span data-stu-id="49107-175">If you want to re-publish without adding Application Insights to the code, be aware that the deployment process may delete the DLLs and ApplicationInsights.config from the published web site.</span></span> <span data-ttu-id="49107-176">Ezért:</span><span class="sxs-lookup"><span data-stu-id="49107-176">Therefore:</span></span>

1. <span data-ttu-id="49107-177">Ha szerkesztette az ApplicationInsights.config fájlt, készítsen róla egy másolatot, mielőtt újra közzéteszi az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="49107-177">If you edited ApplicationInsights.config, take a copy of it before you re-publish your app.</span></span>
2. <span data-ttu-id="49107-178">Tegye közzé újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="49107-178">Republish your app.</span></span>
3. <span data-ttu-id="49107-179">Engedélyezze újra az Application Insights-figyelést.</span><span class="sxs-lookup"><span data-stu-id="49107-179">Re-enable Application Insights monitoring.</span></span> <span data-ttu-id="49107-180">(Használja a megfelelő módszert: vagy az Azure-webapp vezérlőpultját, vagy egy IIS-gazdagép Állapotfigyelőjét.)</span><span class="sxs-lookup"><span data-stu-id="49107-180">(Use the appropriate method: either the Azure web app control panel, or the Status Monitor on an IIS host.)</span></span>
4. <span data-ttu-id="49107-181">Állítsa vissza a .config fájlon végrehajtott szerkesztéseket.</span><span class="sxs-lookup"><span data-stu-id="49107-181">Reinstate any edits you performed on the .config file.</span></span>


## <a name="troubleshooting-runtime-configuration-of-application-insights"></a><span data-ttu-id="49107-182">Az Application Insights futtatókörnyezet-konfigurációjának hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="49107-182">Troubleshooting runtime configuration of Application Insights</span></span>

### <a name="cant-connect-no-telemetry"></a><span data-ttu-id="49107-183">Nem tud csatlakozni?</span><span class="sxs-lookup"><span data-stu-id="49107-183">Can't connect?</span></span> <span data-ttu-id="49107-184">Nem működik a telemetria?</span><span class="sxs-lookup"><span data-stu-id="49107-184">No telemetry?</span></span>

* <span data-ttu-id="49107-185">Nyissa meg [a szükséges kimenő portokat](app-insights-ip-addresses.md#outgoing-ports) a kiszolgálója tűzfalán, hogy az Állapotfigyelő működhessen.</span><span class="sxs-lookup"><span data-stu-id="49107-185">Open [the necessary outgoing ports](app-insights-ip-addresses.md#outgoing-ports) in your server's firewall to allow Status Monitor to work.</span></span>

* <span data-ttu-id="49107-186">Nyissa meg az Állapotfigyelőt, és válassza ki az alkalmazását a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="49107-186">Open Status Monitor and select your application on left pane.</span></span> <span data-ttu-id="49107-187">Ellenőrizze, hogy vannak-e diagnosztikai üzenetek ehhez az alkalmazáshoz a „Konfigurációs értesítések” szakaszban:</span><span class="sxs-lookup"><span data-stu-id="49107-187">Check if there are any diagnostics messages for this application in the "Configuration notifications" section:</span></span>

  ![A Teljesítmény panel megnyitása a kérelem, a válaszidő, a függőség és egyéb adatok megtekintéséhez](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* <span data-ttu-id="49107-189">Ha a kiszolgálón „elégtelen engedélyekkel” kapcsolatos üzenet jelenik meg, próbálja meg a következőt:</span><span class="sxs-lookup"><span data-stu-id="49107-189">On the server, if you see a message about "insufficient permissions", try the following:</span></span>
  * <span data-ttu-id="49107-190">Az IIS-kezelőben válassza ki az alkalmazáskészletét, nyissa meg a **Speciális beállítások** elemet, és a **Folyamatmodell** területen jegyezze fel az identitást.</span><span class="sxs-lookup"><span data-stu-id="49107-190">In IIS Manager, select your application pool, open **Advanced Settings**, and under **Process Model** note the identity.</span></span>
  * <span data-ttu-id="49107-191">A Számítógép-kezelés vezérlőpulton adja ezt az identitást a Teljesítményfigyelő felhasználói csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="49107-191">In Computer management control panel, add this identity to the Performance Monitor Users group.</span></span>
* <span data-ttu-id="49107-192">Ha MMA/SCOM (Systems Center Operations Manager) van telepítve a kiszolgálón, néhány verzió esetében ütközés léphet fel.</span><span class="sxs-lookup"><span data-stu-id="49107-192">If you have MMA/SCOM (Systems Center Operations Manager) installed on your server, some versions can conflict.</span></span> <span data-ttu-id="49107-193">Távolítsa el az SCOM-ot és az Állapotfigyelőt is, és telepítse újra a legújabb verziókat.</span><span class="sxs-lookup"><span data-stu-id="49107-193">Uninstall both SCOM and Status Monitor, and re-install the latest versions.</span></span>
* <span data-ttu-id="49107-194">Lásd: [Hibaelhárítás][qna].</span><span class="sxs-lookup"><span data-stu-id="49107-194">See [Troubleshooting][qna].</span></span>

## <a name="system-requirements"></a><span data-ttu-id="49107-195">Rendszerkövetelmények</span><span class="sxs-lookup"><span data-stu-id="49107-195">System Requirements</span></span>
<span data-ttu-id="49107-196">Operációs rendszeri támogatás az Application Insights Állapotfigyelőhöz a kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="49107-196">OS support for Application Insights Status Monitor on Server:</span></span>

* <span data-ttu-id="49107-197">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="49107-197">Windows Server 2008</span></span>
* <span data-ttu-id="49107-198">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="49107-198">Windows Server 2008 R2</span></span>
* <span data-ttu-id="49107-199">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="49107-199">Windows Server 2012</span></span>
* <span data-ttu-id="49107-200">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="49107-200">Windows server 2012 R2</span></span>
* <span data-ttu-id="49107-201">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="49107-201">Windows Server 2016</span></span>

<span data-ttu-id="49107-202">a legújabb szervizcsomaggal és a .NET-keretrendszer 4.5-ös verziójával</span><span class="sxs-lookup"><span data-stu-id="49107-202">with latest SP and .NET Framework 4.5</span></span>

<span data-ttu-id="49107-203">Az ügyféloldalon: Windows 7, 8, 8.1 és 10, szintén a .NET-keretrendszer 4.5-ös verziójával</span><span class="sxs-lookup"><span data-stu-id="49107-203">On the client side: Windows 7, 8, 8.1 and 10, again with .NET Framework 4.5</span></span>

<span data-ttu-id="49107-204">IIS-támogatás: IIS 7, 7.5, 8, 8.5 (az IIS kötelező)</span><span class="sxs-lookup"><span data-stu-id="49107-204">IIS support is: IIS 7, 7.5, 8, 8.5 (IIS is required)</span></span>

## <a name="automation-with-powershell"></a><span data-ttu-id="49107-205">Automatizálás a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="49107-205">Automation with PowerShell</span></span>
<span data-ttu-id="49107-206">A PowerShell a saját IIS-kiszolgálón való használatával elindíthatja és leállíthatja a figyelést.</span><span class="sxs-lookup"><span data-stu-id="49107-206">You can start and stop monitoring by using PowerShell on your IIS server.</span></span>

<span data-ttu-id="49107-207">Először importálja az Application Insights-modult:</span><span class="sxs-lookup"><span data-stu-id="49107-207">First import the Application Insights module:</span></span>

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

<span data-ttu-id="49107-208">Derítse ki, melyik alkalmazások állnak megfigyelés alatt:</span><span class="sxs-lookup"><span data-stu-id="49107-208">Find out which apps are being monitored:</span></span>

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* <span data-ttu-id="49107-209">`-Name` (Választható) A webalkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="49107-209">`-Name` (Optional) The name of a web app.</span></span>
* <span data-ttu-id="49107-210">Megjeleníti mindegyik webalkalmazás (vagy elnevezett alkalmazás) Application Insights figyelési állapotát ezen az IIS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="49107-210">Displays the Application Insights monitoring status for each web app (or the named app) in this IIS server.</span></span>
* <span data-ttu-id="49107-211">Az `ApplicationInsightsApplication` elemet adja vissza mindegyik alkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="49107-211">Returns `ApplicationInsightsApplication` for each app:</span></span>

  * <span data-ttu-id="49107-212">`SdkState==EnabledAfterDeployment`: Az alkalmazás megfigyelés alatt áll, és a futási időben lett kialakítva, az Állapotfigyelő eszköz vagy a `Start-ApplicationInsightsMonitoring` által.</span><span class="sxs-lookup"><span data-stu-id="49107-212">`SdkState==EnabledAfterDeployment`: App is being monitored, and was instrumented at run time, either by the Status Monitor tool, or by `Start-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="49107-213">`SdkState==Disabled`: Az alkalmazás nincs kialakítva az Application Insights szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="49107-213">`SdkState==Disabled`: The app is not instrumented for Application Insights.</span></span> <span data-ttu-id="49107-214">Vagy soha nem lett kialakítva, vagy a futásidejű figyelés le van tiltva az Állapotfigyelő eszközzel vagy a `Stop-ApplicationInsightsMonitoring` eszközzel.</span><span class="sxs-lookup"><span data-stu-id="49107-214">Either it was never instrumented, or run-time monitoring was disabled with the Status Monitor tool or with `Stop-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="49107-215">`SdkState==EnabledByCodeInstrumentation`: Az alkalmazás kialakításakor az SDK a forráskódhoz lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="49107-215">`SdkState==EnabledByCodeInstrumentation`: The app was instrumented by adding the SDK to the source code.</span></span> <span data-ttu-id="49107-216">Az SDK-ja nem frissíthető és nem állítható le.</span><span class="sxs-lookup"><span data-stu-id="49107-216">Its SDK cannot be updated or stopped.</span></span>
  * <span data-ttu-id="49107-217">`SdkVersion` az alkalmazás figyeléséhez használt verziót jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="49107-217">`SdkVersion` shows the version in use for monitoring this app.</span></span>
  * <span data-ttu-id="49107-218">`LatestAvailableSdkVersion` a NuGet-katalógusban jelenleg elérhető verziót jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="49107-218">`LatestAvailableSdkVersion`shows the version currently available on the NuGet gallery.</span></span> <span data-ttu-id="49107-219">Ha az alkalmazást erre a verzióra szeretné frissíteni, használja a következőt: `Update-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="49107-219">To upgrade the app to this version, use `Update-ApplicationInsightsMonitoring`.</span></span>

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* <span data-ttu-id="49107-220">`-Name` Az alkalmazás neve az IIS-ben</span><span class="sxs-lookup"><span data-stu-id="49107-220">`-Name` The name of the app in IIS</span></span>
* <span data-ttu-id="49107-221">`-InstrumentationKey` Azon Application Insights-erőforrás kulcsa, ahol az eredményeket meg szeretné jeleníteni.</span><span class="sxs-lookup"><span data-stu-id="49107-221">`-InstrumentationKey` The ikey of the Application Insights resource where you want the results to be displayed.</span></span>
* <span data-ttu-id="49107-222">Ez a parancsmag csak olyan alkalmazásokra van hatással, amelyek még nincsenek kialakítva – vagyis amelyek esetében az SdkState==NotInstrumented.</span><span class="sxs-lookup"><span data-stu-id="49107-222">This cmdlet only affects apps that are not already instrumented - that is, SdkState==NotInstrumented.</span></span>

    <span data-ttu-id="49107-223">A parancsmag nincs hatással a már kialakított alkalmazásokra.</span><span class="sxs-lookup"><span data-stu-id="49107-223">The cmdlet does not affect an app that is already instrumented.</span></span> <span data-ttu-id="49107-224">Nem számít, hogy azok a beépítési időben az SDK a kódhoz adásával, vagy a futásidőben a parancsmag korábbi használatával lettek kialakítva.</span><span class="sxs-lookup"><span data-stu-id="49107-224">It does not matter whether the app was instrumented at build time by adding the SDK to the code, or at run time by a previous use of this cmdlet.</span></span>

    <span data-ttu-id="49107-225">Az alkalmazás kialakításához használt SDK-verzió a kiszolgálóra legutóbb letöltött verzió.</span><span class="sxs-lookup"><span data-stu-id="49107-225">The SDK version used to instrument the app is the version that was most recently downloaded to this server.</span></span>

    <span data-ttu-id="49107-226">A legújabb verzió letöltéséhez használja az Update-ApplicationInsightsVersion parancsot.</span><span class="sxs-lookup"><span data-stu-id="49107-226">To download the latest version, use Update-ApplicationInsightsVersion.</span></span>
* <span data-ttu-id="49107-227">Siker esetén az `ApplicationInsightsApplication` elemet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="49107-227">Returns `ApplicationInsightsApplication` on success.</span></span> <span data-ttu-id="49107-228">Sikertelenség esetén nyomkövetést naplóz a stderrben.</span><span class="sxs-lookup"><span data-stu-id="49107-228">If it fails, it logs a trace to stderr.</span></span>

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* <span data-ttu-id="49107-229">`-Name` Az alkalmazás neve az IIS-ben</span><span class="sxs-lookup"><span data-stu-id="49107-229">`-Name` The name of an app in IIS</span></span>
* <span data-ttu-id="49107-230">`-All` Leállítja minden alkalmazás megfigyelését ezen az IIS-kiszolgálón, amely esetében az `SdkState==EnabledAfterDeployment`</span><span class="sxs-lookup"><span data-stu-id="49107-230">`-All` Stops monitoring all apps in this IIS server for which `SdkState==EnabledAfterDeployment`</span></span>
* <span data-ttu-id="49107-231">Leállítja a megadott alkalmazások megfigyelését, és eltávolítja a kialakítást.</span><span class="sxs-lookup"><span data-stu-id="49107-231">Stops monitoring the specified apps and removes instrumentation.</span></span> <span data-ttu-id="49107-232">Csak olyan alkalmazásokhoz működik, amelyek futásidőben lettek kialakítva az Állapotfigyelés eszközzel vagy a Start-ApplicationInsightsApplication paranccsal.</span><span class="sxs-lookup"><span data-stu-id="49107-232">It only works for apps that have been instrumented at run-time using the Status Monitoring tool or Start-ApplicationInsightsApplication.</span></span> <span data-ttu-id="49107-233">(`SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="49107-233">(`SdkState==EnabledAfterDeployment`)</span></span>
* <span data-ttu-id="49107-234">Az ApplicationInsightsApplication elemet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="49107-234">Returns ApplicationInsightsApplication.</span></span>

<span data-ttu-id="49107-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span><span class="sxs-lookup"><span data-stu-id="49107-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span></span>

* <span data-ttu-id="49107-236">`-Name`: A webalkalmazás neve az IIS-ben.</span><span class="sxs-lookup"><span data-stu-id="49107-236">`-Name`: The name of a web app in IIS.</span></span>
* <span data-ttu-id="49107-237">`-InstrumentationKey`(Választható.) Ezzel módosíthatja az erőforrást, amelynek az alkalmazás telemetriája el lesz küldve.</span><span class="sxs-lookup"><span data-stu-id="49107-237">`-InstrumentationKey` (Optional.) Use this to change the resource to which the app's telemetry is sent.</span></span>
* <span data-ttu-id="49107-238">Ez a parancsmag:</span><span class="sxs-lookup"><span data-stu-id="49107-238">This cmdlet:</span></span>
  * <span data-ttu-id="49107-239">frissíti az elnevezett alkalmazást a gépre legutóbb letöltött SDK-verzióra.</span><span class="sxs-lookup"><span data-stu-id="49107-239">Upgrades the named app to the version of the SDK most recently downloaded to this machine.</span></span> <span data-ttu-id="49107-240">(Csak akkor működik, ha `SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="49107-240">(Only works if `SdkState==EnabledAfterDeployment`)</span></span>
  * <span data-ttu-id="49107-241">Ha megad egy kialakítási kulcsot, újrakonfigurálja az elnevezett alkalmazást, hogy telemetriát küldjön az erőforrásnak ezzel a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="49107-241">If you provide an instrumentation key, the named app is reconfigured to send telemetry to the resource with that key.</span></span> <span data-ttu-id="49107-242">(Akkor működik, ha `SdkState != Disabled`)</span><span class="sxs-lookup"><span data-stu-id="49107-242">(Works if `SdkState != Disabled`)</span></span>

`Update-ApplicationInsightsVersion`

* <span data-ttu-id="49107-243">letölti a legújabb Application Insights SDK-t a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="49107-243">Downloads the latest Application Insights SDK to the server.</span></span>

## <span data-ttu-id="49107-244"><a name="questions"></a>Kérdések az Állapotfigyelővel kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="49107-244"><a name="questions"></a>Questions about Status Monitor</span></span>

### <a name="what-is-status-monitor"></a><span data-ttu-id="49107-245">Mi az Állapotfigyelő?</span><span class="sxs-lookup"><span data-stu-id="49107-245">What is Status Monitor?</span></span>

<span data-ttu-id="49107-246">Egy asztali alkalmazás, amelyet az IIS-webkiszolgálón kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="49107-246">A desktop application that you install in your IIS web server.</span></span> <span data-ttu-id="49107-247">Segít a webalkalmazások kialakításában és konfigurálásában.</span><span class="sxs-lookup"><span data-stu-id="49107-247">It helps you instrument and configure web apps.</span></span> 

### <a name="when-do-i-use-status-monitor"></a><span data-ttu-id="49107-248">Mire használhatom az Állapotfigyelőt?</span><span class="sxs-lookup"><span data-stu-id="49107-248">When do I use Status Monitor?</span></span>

* <span data-ttu-id="49107-249">Bármely, az IIS-kiszolgálón futtatott, akár már futó webalkalmazások beállításához.</span><span class="sxs-lookup"><span data-stu-id="49107-249">To instrument any web app that is running on your IIS server - even if it is already running.</span></span>
* <span data-ttu-id="49107-250">További telemetria engedélyezéséhez olyan webalkalmazások számára, amelyeket [az Application Insights SDK-val állítottak össze](app-insights-asp-net.md) a fordítás során.</span><span class="sxs-lookup"><span data-stu-id="49107-250">To enable additional telemetry for web apps that have been [built with the Application Insights SDK](app-insights-asp-net.md) at compile time.</span></span> 

### <a name="can-i-close-it-after-it-runs"></a><span data-ttu-id="49107-251">A futtatás után bezárhatom?</span><span class="sxs-lookup"><span data-stu-id="49107-251">Can I close it after it runs?</span></span>

<span data-ttu-id="49107-252">Igen.</span><span class="sxs-lookup"><span data-stu-id="49107-252">Yes.</span></span> <span data-ttu-id="49107-253">Az alkalmazást a kiválasztott webhelyek beállításának befejezése után be lehet zárni.</span><span class="sxs-lookup"><span data-stu-id="49107-253">After it has instrumented the websites you select, you can close it.</span></span>

<span data-ttu-id="49107-254">Az alkalmazás önmagától nem gyűjt telemetriai adatokat,</span><span class="sxs-lookup"><span data-stu-id="49107-254">It doesn't collect telemetry by itself.</span></span> <span data-ttu-id="49107-255">csupán a webalkalmazásokat konfigurálja, és néhány engedélyt állít be.</span><span class="sxs-lookup"><span data-stu-id="49107-255">It just configures the web apps and sets some permissions.</span></span>

### <a name="what-does-status-monitor-do"></a><span data-ttu-id="49107-256">Milyen műveleteket hajt végre az Állapotfigyelő?</span><span class="sxs-lookup"><span data-stu-id="49107-256">What does Status Monitor do?</span></span>

<span data-ttu-id="49107-257">Ha kiválaszt egy webalkalmazást, amelyet az Állapotfigyelővel szeretne beállítani:</span><span class="sxs-lookup"><span data-stu-id="49107-257">When you select a web app for Status Monitor to instrument:</span></span>

* <span data-ttu-id="49107-258">Letölti és elhelyezi az Application Insights-szerelvényeket és a .config fájlt a webalkalmazás bináris mappájába.</span><span class="sxs-lookup"><span data-stu-id="49107-258">Downloads and places the Application Insights assemblies and .config file in the web app's binaries folder.</span></span>
* <span data-ttu-id="49107-259">Módosítja a `web.config` fájlt az Application Insights HTTP nyomkövetési moduljának hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="49107-259">Modifies `web.config` to add the Application Insights HTTP tracking module.</span></span>
* <span data-ttu-id="49107-260">A függőségi hívások összegyűjtéséhez engedélyezi a CLR-profilkészítést.</span><span class="sxs-lookup"><span data-stu-id="49107-260">Enables CLR profiling to collect dependency calls.</span></span>

### <a name="do-i-need-to-run-status-monitor-whenever-i-update-the-app"></a><span data-ttu-id="49107-261">Az alkalmazás frissítésekor minden alkalommal futtatnom kell az Állapotfigyelőt?</span><span class="sxs-lookup"><span data-stu-id="49107-261">Do I need to run Status Monitor whenever I update the app?</span></span>

<span data-ttu-id="49107-262">Nem, ha az ismételt üzembe helyezés növekményesen történik.</span><span class="sxs-lookup"><span data-stu-id="49107-262">Not if you redeploy incrementally.</span></span> 

<span data-ttu-id="49107-263">Ha a közzététel során a „Meglévő fájlok törlése” lehetőséget választja, az Application Insights konfigurálásához újból futtatnia kell az Állapotfigyelőt.</span><span class="sxs-lookup"><span data-stu-id="49107-263">If you select the 'delete existing files' option in the publish process, you would need to re-run Status Monitor to configure Application Insights.</span></span>

### <a name="what-telemetry-is-collected"></a><span data-ttu-id="49107-264">A rendszer milyen telemetriai adatokat gyűjt?</span><span class="sxs-lookup"><span data-stu-id="49107-264">What telemetry is collected?</span></span>

<span data-ttu-id="49107-265">Olyan alkalmazások esetén, amelyeket az Állapotfigyelővel kizárólag futásidőben állít be:</span><span class="sxs-lookup"><span data-stu-id="49107-265">For applications that you instrument only at run-time by using Status Monitor:</span></span>

* <span data-ttu-id="49107-266">HTTP-kérések</span><span class="sxs-lookup"><span data-stu-id="49107-266">HTTP requests</span></span>
* <span data-ttu-id="49107-267">Függőségi hívások</span><span class="sxs-lookup"><span data-stu-id="49107-267">Calls to dependencies</span></span>
* <span data-ttu-id="49107-268">Kivételek</span><span class="sxs-lookup"><span data-stu-id="49107-268">Exceptions</span></span>
* <span data-ttu-id="49107-269">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="49107-269">Performance counters</span></span>

<span data-ttu-id="49107-270">A fordítási során már kiépített alkalmazások esetén:</span><span class="sxs-lookup"><span data-stu-id="49107-270">For applications already instrumented at compile time:</span></span>

 * <span data-ttu-id="49107-271">Folyamatszámlálók.</span><span class="sxs-lookup"><span data-stu-id="49107-271">Process counters.</span></span>
 * <span data-ttu-id="49107-272">Függőségi hívások (.NET 4.5); függőségi hívásokban visszaadott értékek (.NET 4.6).</span><span class="sxs-lookup"><span data-stu-id="49107-272">Dependency calls (.NET 4.5); return values in dependency calls (.NET 4.6).</span></span>
 * <span data-ttu-id="49107-273">Kivételhíváslánc-értékek.</span><span class="sxs-lookup"><span data-stu-id="49107-273">Exception stack trace values.</span></span>

[<span data-ttu-id="49107-274">További információ</span><span class="sxs-lookup"><span data-stu-id="49107-274">Learn more</span></span>](http://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a><span data-ttu-id="49107-275">Videó</span><span class="sxs-lookup"><span data-stu-id="49107-275">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <span data-ttu-id="49107-276"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49107-276"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="49107-277">A telemetriai adatok megtekintése:</span><span class="sxs-lookup"><span data-stu-id="49107-277">View your telemetry:</span></span>

* <span data-ttu-id="49107-278">[A metrikák áttekintése](app-insights-metrics-explorer.md) a teljesítmény és a használat figyeléséhez</span><span class="sxs-lookup"><span data-stu-id="49107-278">[Explore metrics](app-insights-metrics-explorer.md) to monitor performance and usage</span></span>
* <span data-ttu-id="49107-279">[Események és naplók keresése][diagnostic] a problémák diagnosztizálásához</span><span class="sxs-lookup"><span data-stu-id="49107-279">[Search events and logs][diagnostic] to diagnose problems</span></span>
* <span data-ttu-id="49107-280">[Elemzések](app-insights-analytics.md) az összetettebb lekérdezésekhez</span><span class="sxs-lookup"><span data-stu-id="49107-280">[Analytics](app-insights-analytics.md) for more advanced queries</span></span>
* [<span data-ttu-id="49107-281">Irányítópultok létrehozása</span><span class="sxs-lookup"><span data-stu-id="49107-281">Create dashboards</span></span>](app-insights-dashboards.md)

<span data-ttu-id="49107-282">További telemetriai funkciók hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="49107-282">Add more telemetry:</span></span>

* <span data-ttu-id="49107-283">[Létrehozhat webes teszteket][availability] annak biztosításához, hogy a hely elérhető maradjon.</span><span class="sxs-lookup"><span data-stu-id="49107-283">[Create web tests][availability] to make sure your site stays live.</span></span>
* <span data-ttu-id="49107-284">[Webesügyfél-telemetriát adhat hozzá][usage], hogy lássa a weblapkód kivételeit, és nyomkövetési hívásokat szúrhasson be.</span><span class="sxs-lookup"><span data-stu-id="49107-284">[Add web client telemetry][usage] to see exceptions from web page code and to let you insert trace calls.</span></span>
* <span data-ttu-id="49107-285">[Application Insights SDK-t adhat a kódhoz][greenbrown], hogy nyomkövetési és naplóhíváskat szúrhasson be</span><span class="sxs-lookup"><span data-stu-id="49107-285">[Add Application Insights SDK to your code][greenbrown] so that you can insert trace and log calls</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-javascript.md

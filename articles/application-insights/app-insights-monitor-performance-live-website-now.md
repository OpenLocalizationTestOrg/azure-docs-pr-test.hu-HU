---
title: "aaaMonitor élő ASP.NET webalkalmazás az Azure Application insights szolgáltatással |} Microsoft Docs"
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
ms.openlocfilehash: 0d53f0a59974f40767fae681bafc4f358d1283a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a><span data-ttu-id="326cd-104">Webalkalmazások futásidejű kialakítása az Application Insights használatával</span><span class="sxs-lookup"><span data-stu-id="326cd-104">Instrument web apps at runtime with Application Insights</span></span>


<span data-ttu-id="326cd-105">Egy élő webalkalmazását az Azure Application insights szolgáltatással állíthatnak be anélkül, hogy toomodify, vagy telepítse újra a kódot.</span><span class="sxs-lookup"><span data-stu-id="326cd-105">You can instrument a live web app with Azure Application Insights, without having toomodify or redeploy your code.</span></span> <span data-ttu-id="326cd-106">Ha az alkalmazásokat egy helyszíni IIS-kiszolgáló működteti, telepítse az Állapotfigyelőt.</span><span class="sxs-lookup"><span data-stu-id="326cd-106">If your apps are hosted by an on-premises IIS server, install Status Monitor.</span></span> <span data-ttu-id="326cd-107">Azure-webalkalmazásokban vagy egy Azure virtuális gép fut, az Application Insights általi figyelés hello Azure Vezérlőpultról válthat.</span><span class="sxs-lookup"><span data-stu-id="326cd-107">If they're Azure web apps or run in an Azure VM, you can switch on Application Insights monitoring from hello Azure control panel.</span></span> <span data-ttu-id="326cd-108">(Külön cikkek érhetők el az [élő J2EE-webalkalmazások](app-insights-java-live.md) és az [Azure Cloud Services](app-insights-cloudservices.md) kialakításáról.) Ehhez [Microsoft Azure](http://azure.com)-előfizetésre van szükség.</span><span class="sxs-lookup"><span data-stu-id="326cd-108">(There are also separate articles about instrumenting [live J2EE web apps](app-insights-java-live.md) and [Azure Cloud Services](app-insights-cloudservices.md).) You need a [Microsoft Azure](http://azure.com) subscription.</span></span>

![mintadiagramok](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

<span data-ttu-id="326cd-110">Megválaszthatja három útvonalak tooapply Application Insights tooyour .NET webes alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="326cd-110">You have a choice of three routes tooapply Application Insights tooyour .NET web applications:</span></span>

* <span data-ttu-id="326cd-111">**Build idő:** [Application Insights SDK hozzáadása hello] [ greenbrown] tooyour webes mintaalkalmazás kódját.</span><span class="sxs-lookup"><span data-stu-id="326cd-111">**Build time:** [Add hello Application Insights SDK][greenbrown] tooyour web app code.</span></span>
* <span data-ttu-id="326cd-112">**Futási idő:** állíthatnak be a webalkalmazás hello kiszolgálón, az alább ismertetett, újraépítését, és újbóli hello kód nélkül.</span><span class="sxs-lookup"><span data-stu-id="326cd-112">**Run time:** Instrument your web app on hello server, as described below, without rebuilding and redeploying hello code.</span></span>
* <span data-ttu-id="326cd-113">**Mindkét:** hello SDK összeállítása a webes alkalmazás kódba, és hello futásidejű bővítmények verzióra is érvényes.</span><span class="sxs-lookup"><span data-stu-id="326cd-113">**Both:** Build hello SDK into your web app code, and also apply hello run-time extensions.</span></span> <span data-ttu-id="326cd-114">Hello mindkét lehetőség a leghatékonyabb beolvasása.</span><span class="sxs-lookup"><span data-stu-id="326cd-114">Get hello best of both options.</span></span>

<span data-ttu-id="326cd-115">Itt található egy összefoglaló az egyes módszerek eredményeiről:</span><span class="sxs-lookup"><span data-stu-id="326cd-115">Here's a summary of what you get by each route:</span></span>

|  | <span data-ttu-id="326cd-116">Felépítési idő</span><span class="sxs-lookup"><span data-stu-id="326cd-116">Build time</span></span> | <span data-ttu-id="326cd-117">Futási idő</span><span class="sxs-lookup"><span data-stu-id="326cd-117">Run time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="326cd-118">Kérések és kivételek</span><span class="sxs-lookup"><span data-stu-id="326cd-118">Requests & exceptions</span></span> |<span data-ttu-id="326cd-119">Igen</span><span class="sxs-lookup"><span data-stu-id="326cd-119">Yes</span></span> |<span data-ttu-id="326cd-120">Igen</span><span class="sxs-lookup"><span data-stu-id="326cd-120">Yes</span></span> |
| [<span data-ttu-id="326cd-121">Részletes kivételek</span><span class="sxs-lookup"><span data-stu-id="326cd-121">More detailed exceptions</span></span>](app-insights-asp-net-exceptions.md) | |<span data-ttu-id="326cd-122">Igen</span><span class="sxs-lookup"><span data-stu-id="326cd-122">Yes</span></span> |
| [<span data-ttu-id="326cd-123">Függőségek diagnosztikája</span><span class="sxs-lookup"><span data-stu-id="326cd-123">Dependency diagnostics</span></span>](app-insights-asp-net-dependencies.md) |<span data-ttu-id="326cd-124">.NET 4.6+ esetén, kevésbé részletesen</span><span class="sxs-lookup"><span data-stu-id="326cd-124">On .NET 4.6+, but less detail</span></span> |<span data-ttu-id="326cd-125">Igen, teljes részletesség: eredménykódok, SQL-parancsszöveg, HTTP-parancsok</span><span class="sxs-lookup"><span data-stu-id="326cd-125">Yes, full detail: result codes, SQL command text, HTTP verb</span></span>|
| [<span data-ttu-id="326cd-126">Rendszerteljesítmény-számlálók</span><span class="sxs-lookup"><span data-stu-id="326cd-126">System performance counters</span></span>](app-insights-performance-counters.md) |<span data-ttu-id="326cd-127">Igen</span><span class="sxs-lookup"><span data-stu-id="326cd-127">Yes</span></span> |<span data-ttu-id="326cd-128">Igen</span><span class="sxs-lookup"><span data-stu-id="326cd-128">Yes</span></span> |
| <span data-ttu-id="326cd-129">[API egyéni telemetriához][api]</span><span class="sxs-lookup"><span data-stu-id="326cd-129">[API for custom telemetry][api]</span></span> |<span data-ttu-id="326cd-130">Igen</span><span class="sxs-lookup"><span data-stu-id="326cd-130">Yes</span></span> |<span data-ttu-id="326cd-131">Nem</span><span class="sxs-lookup"><span data-stu-id="326cd-131">No</span></span> |
| [<span data-ttu-id="326cd-132">Nyomkövetési napló integrációja</span><span class="sxs-lookup"><span data-stu-id="326cd-132">Trace log integration</span></span>](app-insights-asp-net-trace-logs.md) |<span data-ttu-id="326cd-133">Igen</span><span class="sxs-lookup"><span data-stu-id="326cd-133">Yes</span></span> |<span data-ttu-id="326cd-134">Nem</span><span class="sxs-lookup"><span data-stu-id="326cd-134">No</span></span> |
| [<span data-ttu-id="326cd-135">Lapmegtekintések és felhasználói adatok</span><span class="sxs-lookup"><span data-stu-id="326cd-135">Page view & user data</span></span>](app-insights-javascript.md) |<span data-ttu-id="326cd-136">Igen</span><span class="sxs-lookup"><span data-stu-id="326cd-136">Yes</span></span> |<span data-ttu-id="326cd-137">Nem</span><span class="sxs-lookup"><span data-stu-id="326cd-137">No</span></span> |
| <span data-ttu-id="326cd-138">Toorebuild kód szükséges</span><span class="sxs-lookup"><span data-stu-id="326cd-138">Need toorebuild code</span></span> |<span data-ttu-id="326cd-139">Igen</span><span class="sxs-lookup"><span data-stu-id="326cd-139">Yes</span></span> | <span data-ttu-id="326cd-140">Nem</span><span class="sxs-lookup"><span data-stu-id="326cd-140">No</span></span> |


## <a name="monitor-a-live-azure-web-app"></a><span data-ttu-id="326cd-141">Élő Azure-webapp figyelése</span><span class="sxs-lookup"><span data-stu-id="326cd-141">Monitor a live Azure web app</span></span>

<span data-ttu-id="326cd-142">Ha az alkalmazás fut-e egy Azure webes szolgáltatás, itt, hogyan tooswitch figyeléséről:</span><span class="sxs-lookup"><span data-stu-id="326cd-142">If your application is running as an Azure web service, here's how tooswitch on monitoring:</span></span>

* <span data-ttu-id="326cd-143">Válassza ki az Application Insights Vezérlőpultján hello alkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="326cd-143">Select Application Insights on hello app's control panel in Azure.</span></span>

    ![Az Application Insights beállítása egy Azure-webapphoz](./media/app-insights-monitor-performance-live-website-now/azure-web-setup.png)
* <span data-ttu-id="326cd-145">Hello Application Insights összefoglaló lap megnyitása után kattintson a hello hivatkozásra, hello alsó tooopen hello teljes Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="326cd-145">When hello Application Insights summary page opens, click hello link at hello bottom tooopen hello full Application Insights resource.</span></span>

    ![Kattintson a tooApplication Insights](./media/app-insights-monitor-performance-live-website-now/azure-web-view-more.png)

<span data-ttu-id="326cd-147">[Felhőben és virtuális gépeken futó alkalmazások figyelése](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="326cd-147">[Monitoring Cloud and VM apps](app-insights-azure.md).</span></span>

### <a name="enable-client-side-monitoring-in-azure"></a><span data-ttu-id="326cd-148">Az ügyféloldali figyelés engedélyezése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="326cd-148">Enable client-side monitoring in Azure</span></span>

<span data-ttu-id="326cd-149">Ha engedélyezte az Application Insights szolgáltatást az Azure-ban, felveheti a lapmegtekintéseket és a felhasználók telemetriai adatait.</span><span class="sxs-lookup"><span data-stu-id="326cd-149">If you have enabled Application Insights in Azure, you can add page view and user telemetry.</span></span>

1. <span data-ttu-id="326cd-150">Válassza a Beállítások > Alkalmazásbeállítások lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="326cd-150">Select Settings > Application Settings</span></span>
2.  <span data-ttu-id="326cd-151">Az alkalmazásbeállításoknál adjon meg egy új kulcs-érték párt:</span><span class="sxs-lookup"><span data-stu-id="326cd-151">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="326cd-152">Kulcs: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="326cd-152">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="326cd-153">Érték: `true`</span><span class="sxs-lookup"><span data-stu-id="326cd-153">Value: `true`</span></span>
3. <span data-ttu-id="326cd-154">**Mentés** beállítások hello és **indítsa újra a** az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="326cd-154">**Save** hello settings and **Restart** your app.</span></span>

<span data-ttu-id="326cd-155">Application Insights JavaScript SDK hello van most be a nézetmodellbe, minden weblapon.</span><span class="sxs-lookup"><span data-stu-id="326cd-155">hello Application Insights JavaScript SDK is now injected into each web page.</span></span>

## <a name="monitor-a-live-iis-web-app"></a><span data-ttu-id="326cd-156">Élő IIS-webapp figyelése</span><span class="sxs-lookup"><span data-stu-id="326cd-156">Monitor a live IIS web app</span></span>

<span data-ttu-id="326cd-157">Ha az alkalmazás egy IIS-kiszolgálón fut, engedélyezze az Application Insightst az Állapotfigyelő használatával.</span><span class="sxs-lookup"><span data-stu-id="326cd-157">If your app is hosted on an IIS server, enable Application Insights by using Status Monitor.</span></span>

1. <span data-ttu-id="326cd-158">Az IIS-webkiszolgálón jelentkezzen be rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="326cd-158">On your IIS web server, sign in with administrator credentials.</span></span>
2. <span data-ttu-id="326cd-159">Ha az Application Insights Állapotmonitor nincs telepítve, töltse le, és futtassa a hello [állapotfigyelő telepítő](http://go.microsoft.com/fwlink/?LinkId=506648) (vagy futtassa [Webplatform-telepítő](https://www.microsoft.com/web/downloads/platform.aspx) és a keresési azt az Application Insights állapotot A figyelő).</span><span class="sxs-lookup"><span data-stu-id="326cd-159">If Application Insights Status Monitor is not already installed, download and run hello [Status Monitor installer](http://go.microsoft.com/fwlink/?LinkId=506648) (or run [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) and search in it for Application Insights Status Monitor).</span></span>
3. <span data-ttu-id="326cd-160">Állapotmonitorban válassza ki a telepített hello webes alkalmazás vagy webhely, amelyet az toomonitor.</span><span class="sxs-lookup"><span data-stu-id="326cd-160">In Status Monitor, select hello installed web application or website that you want toomonitor.</span></span> <span data-ttu-id="326cd-161">Jelentkezzen be az Azure-beli hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="326cd-161">Sign in with your Azure credentials.</span></span>

    <span data-ttu-id="326cd-162">Konfigurálja a kívánt toosee hello eredmények hello Application Insights portál hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="326cd-162">Configure hello resource where you want toosee hello results in hello Application Insights portal.</span></span> <span data-ttu-id="326cd-163">(Általában, akkor ajánlott toocreate egy új erőforrást.</span><span class="sxs-lookup"><span data-stu-id="326cd-163">(Normally, it's best toocreate a new resource.</span></span> <span data-ttu-id="326cd-164">Meglévő erőforrást akkor válasszon, ha már rendelkezik [webes tesztekkel][availability] vagy [ügyfélfigyeléssel][client] az alkalmazáshoz.)</span><span class="sxs-lookup"><span data-stu-id="326cd-164">Select an existing resource if you already have [web tests][availability] or [client monitoring][client] for this app.)</span></span> 

    ![Válasszon egy alkalmazást és egy erőforrást.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. <span data-ttu-id="326cd-166">Indítsa újra az IIS-t.</span><span class="sxs-lookup"><span data-stu-id="326cd-166">Restart IIS.</span></span>

    ![Válassza ki az újraindítás hello párbeszédpanel hello tetején.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    <span data-ttu-id="326cd-168">A webszolgáltatása rövid időre megszakad.</span><span class="sxs-lookup"><span data-stu-id="326cd-168">Your web service is interrupted for a short while.</span></span>

## <a name="customize-monitoring-options"></a><span data-ttu-id="326cd-169">A megfigyelési beállítások testreszabása</span><span class="sxs-lookup"><span data-stu-id="326cd-169">Customize monitoring options</span></span>

<span data-ttu-id="326cd-170">Az Application Insights engedélyezése hozzáadja a dll-EK és ApplicationInsights.config tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="326cd-170">Enabling Application Insights adds DLLs and ApplicationInsights.config tooyour web app.</span></span> <span data-ttu-id="326cd-171">Is [hello .config fájl szerkesztésével](app-insights-configuration-with-applicationinsights-config.md) toochange hello-beállítások egy része.</span><span class="sxs-lookup"><span data-stu-id="326cd-171">You can [edit hello .config file](app-insights-configuration-with-applicationinsights-config.md) toochange some of hello options.</span></span>

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a><span data-ttu-id="326cd-172">Az Application Insights ismételt engedélyezése az alkalmazás ismételt közzétételekor</span><span class="sxs-lookup"><span data-stu-id="326cd-172">When you re-publish your app, re-enable Application Insights</span></span>

<span data-ttu-id="326cd-173">Mielőtt újra közzé az alkalmazást, érdemes lehet [a Visual Studio Application Insights toohello kód felvétele][greenbrown].</span><span class="sxs-lookup"><span data-stu-id="326cd-173">Before you re-publish your app, consider [adding Application Insights toohello code in Visual Studio][greenbrown].</span></span> <span data-ttu-id="326cd-174">Részletes telemetria és hello képességét toowrite egyéni telemetriai adatokat fog kapni.</span><span class="sxs-lookup"><span data-stu-id="326cd-174">You'll get more detailed telemetry and hello ability toowrite custom telemetry.</span></span>

<span data-ttu-id="326cd-175">Ha azt szeretné, hogy toore-közzététele az Application Insights toohello kód hozzáadása nélkül, vegye figyelembe, hogy hello telepítési folyamatának törölhetik a hello dll-EK és hello az ApplicationInsights.config webhelyen közzétett.</span><span class="sxs-lookup"><span data-stu-id="326cd-175">If you want toore-publish without adding Application Insights toohello code, be aware that hello deployment process may delete hello DLLs and ApplicationInsights.config from hello published web site.</span></span> <span data-ttu-id="326cd-176">Ezért:</span><span class="sxs-lookup"><span data-stu-id="326cd-176">Therefore:</span></span>

1. <span data-ttu-id="326cd-177">Ha szerkesztette az ApplicationInsights.config fájlt, készítsen róla egy másolatot, mielőtt újra közzéteszi az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="326cd-177">If you edited ApplicationInsights.config, take a copy of it before you re-publish your app.</span></span>
2. <span data-ttu-id="326cd-178">Tegye közzé újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="326cd-178">Republish your app.</span></span>
3. <span data-ttu-id="326cd-179">Engedélyezze újra az Application Insights-figyelést.</span><span class="sxs-lookup"><span data-stu-id="326cd-179">Re-enable Application Insights monitoring.</span></span> <span data-ttu-id="326cd-180">(Használjon megfelelő módszert hello: hello Azure web app Vezérlőpult, vagy egy IIS állomáson állapotfigyelő hello.)</span><span class="sxs-lookup"><span data-stu-id="326cd-180">(Use hello appropriate method: either hello Azure web app control panel, or hello Status Monitor on an IIS host.)</span></span>
4. <span data-ttu-id="326cd-181">Végzett módosításokat végzett hello .config fájl visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="326cd-181">Reinstate any edits you performed on hello .config file.</span></span>


## <a name="troubleshooting-runtime-configuration-of-application-insights"></a><span data-ttu-id="326cd-182">Az Application Insights futtatókörnyezet-konfigurációjának hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="326cd-182">Troubleshooting runtime configuration of Application Insights</span></span>

### <a name="cant-connect-no-telemetry"></a><span data-ttu-id="326cd-183">Nem tud csatlakozni?</span><span class="sxs-lookup"><span data-stu-id="326cd-183">Can't connect?</span></span> <span data-ttu-id="326cd-184">Nem működik a telemetria?</span><span class="sxs-lookup"><span data-stu-id="326cd-184">No telemetry?</span></span>

* <span data-ttu-id="326cd-185">Nyissa meg [szükséges kimenő portok hello](app-insights-ip-addresses.md#outgoing-ports) az a kiszolgáló tűzfal tooallow állapotfigyelő toowork.</span><span class="sxs-lookup"><span data-stu-id="326cd-185">Open [hello necessary outgoing ports](app-insights-ip-addresses.md#outgoing-ports) in your server's firewall tooallow Status Monitor toowork.</span></span>

* <span data-ttu-id="326cd-186">Nyissa meg az Állapotfigyelőt, és válassza ki az alkalmazását a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="326cd-186">Open Status Monitor and select your application on left pane.</span></span> <span data-ttu-id="326cd-187">Ellenőrizze, hogy van-e az alkalmazás a hello "Konfigurációs értesítések" szakaszhoz diagnosztikai üzeneteket:</span><span class="sxs-lookup"><span data-stu-id="326cd-187">Check if there are any diagnostics messages for this application in hello "Configuration notifications" section:</span></span>

  ![Nyissa meg a hello teljesítmény panel toosee kérelem, válaszideje, függőség és egyéb adatok](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* <span data-ttu-id="326cd-189">Hello kiszolgálón Ha "nincs megfelelő engedélye" kapcsolatos egy üzenet jelenik meg, próbálkozzon hello következő:</span><span class="sxs-lookup"><span data-stu-id="326cd-189">On hello server, if you see a message about "insufficient permissions", try hello following:</span></span>
  * <span data-ttu-id="326cd-190">Az IIS-kezelőben válassza ki az alkalmazáskészletet, nyissa meg a **speciális beállítások**, majd a **folyamatmodell** jegyezze fel hello identitását.</span><span class="sxs-lookup"><span data-stu-id="326cd-190">In IIS Manager, select your application pool, open **Advanced Settings**, and under **Process Model** note hello identity.</span></span>
  * <span data-ttu-id="326cd-191">A számítógép-felügyeleti Vezérlőpult adja hozzá az identitás toohello Teljesítményfigyelő felhasználók csoportjába.</span><span class="sxs-lookup"><span data-stu-id="326cd-191">In Computer management control panel, add this identity toohello Performance Monitor Users group.</span></span>
* <span data-ttu-id="326cd-192">Ha MMA/SCOM (Systems Center Operations Manager) van telepítve a kiszolgálón, néhány verzió esetében ütközés léphet fel.</span><span class="sxs-lookup"><span data-stu-id="326cd-192">If you have MMA/SCOM (Systems Center Operations Manager) installed on your server, some versions can conflict.</span></span> <span data-ttu-id="326cd-193">SCOM és a állapotfigyelő el, majd telepítse újra a hello legújabb verziói.</span><span class="sxs-lookup"><span data-stu-id="326cd-193">Uninstall both SCOM and Status Monitor, and re-install hello latest versions.</span></span>
* <span data-ttu-id="326cd-194">Lásd: [Hibaelhárítás][qna].</span><span class="sxs-lookup"><span data-stu-id="326cd-194">See [Troubleshooting][qna].</span></span>

## <a name="system-requirements"></a><span data-ttu-id="326cd-195">Rendszerkövetelmények</span><span class="sxs-lookup"><span data-stu-id="326cd-195">System Requirements</span></span>
<span data-ttu-id="326cd-196">Operációs rendszeri támogatás az Application Insights Állapotfigyelőhöz a kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="326cd-196">OS support for Application Insights Status Monitor on Server:</span></span>

* <span data-ttu-id="326cd-197">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="326cd-197">Windows Server 2008</span></span>
* <span data-ttu-id="326cd-198">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="326cd-198">Windows Server 2008 R2</span></span>
* <span data-ttu-id="326cd-199">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="326cd-199">Windows Server 2012</span></span>
* <span data-ttu-id="326cd-200">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="326cd-200">Windows server 2012 R2</span></span>
* <span data-ttu-id="326cd-201">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="326cd-201">Windows Server 2016</span></span>

<span data-ttu-id="326cd-202">a legújabb szervizcsomaggal és a .NET-keretrendszer 4.5-ös verziójával</span><span class="sxs-lookup"><span data-stu-id="326cd-202">with latest SP and .NET Framework 4.5</span></span>

<span data-ttu-id="326cd-203">Hello ügyféloldalon: Windows 7, 8, 8.1 és 10, újra a .NET-keretrendszer 4.5</span><span class="sxs-lookup"><span data-stu-id="326cd-203">On hello client side: Windows 7, 8, 8.1 and 10, again with .NET Framework 4.5</span></span>

<span data-ttu-id="326cd-204">IIS-támogatás: IIS 7, 7.5, 8, 8.5 (az IIS kötelező)</span><span class="sxs-lookup"><span data-stu-id="326cd-204">IIS support is: IIS 7, 7.5, 8, 8.5 (IIS is required)</span></span>

## <a name="automation-with-powershell"></a><span data-ttu-id="326cd-205">Automatizálás a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="326cd-205">Automation with PowerShell</span></span>
<span data-ttu-id="326cd-206">A PowerShell a saját IIS-kiszolgálón való használatával elindíthatja és leállíthatja a figyelést.</span><span class="sxs-lookup"><span data-stu-id="326cd-206">You can start and stop monitoring by using PowerShell on your IIS server.</span></span>

<span data-ttu-id="326cd-207">Először importálni hello Application Insights modult:</span><span class="sxs-lookup"><span data-stu-id="326cd-207">First import hello Application Insights module:</span></span>

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

<span data-ttu-id="326cd-208">Derítse ki, melyik alkalmazások állnak megfigyelés alatt:</span><span class="sxs-lookup"><span data-stu-id="326cd-208">Find out which apps are being monitored:</span></span>

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* <span data-ttu-id="326cd-209">`-Name`A webes alkalmazás neve (nem kötelező) hello.</span><span class="sxs-lookup"><span data-stu-id="326cd-209">`-Name` (Optional) hello name of a web app.</span></span>
* <span data-ttu-id="326cd-210">Jeleníti meg az IIS-kiszolgálót az Application Insights figyelési állapotát az egyes web app (vagy alkalmazás nevű hello) hello.</span><span class="sxs-lookup"><span data-stu-id="326cd-210">Displays hello Application Insights monitoring status for each web app (or hello named app) in this IIS server.</span></span>
* <span data-ttu-id="326cd-211">Az `ApplicationInsightsApplication` elemet adja vissza mindegyik alkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="326cd-211">Returns `ApplicationInsightsApplication` for each app:</span></span>

  * <span data-ttu-id="326cd-212">`SdkState==EnabledAfterDeployment`: Figyelt alkalmazás és a futási időben lett tagolva, vagy hello állapotfigyelő eszköz, vagy `Start-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="326cd-212">`SdkState==EnabledAfterDeployment`: App is being monitored, and was instrumented at run time, either by hello Status Monitor tool, or by `Start-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="326cd-213">`SdkState==Disabled`: hello app nem tagolva az Application insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="326cd-213">`SdkState==Disabled`: hello app is not instrumented for Application Insights.</span></span> <span data-ttu-id="326cd-214">Soha ne tagolva volt, vagy futásidejű figyelés le lett tiltva, hello állapotfigyelő eszközzel vagy `Stop-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="326cd-214">Either it was never instrumented, or run-time monitoring was disabled with hello Status Monitor tool or with `Stop-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="326cd-215">`SdkState==EnabledByCodeInstrumentation`: hello alkalmazás lett tagolva hello SDK toohello forráskód hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="326cd-215">`SdkState==EnabledByCodeInstrumentation`: hello app was instrumented by adding hello SDK toohello source code.</span></span> <span data-ttu-id="326cd-216">Az SDK-ja nem frissíthető és nem állítható le.</span><span class="sxs-lookup"><span data-stu-id="326cd-216">Its SDK cannot be updated or stopped.</span></span>
  * <span data-ttu-id="326cd-217">`SdkVersion`Ez az alkalmazás figyeléséhez használja hello verzióját jelzi.</span><span class="sxs-lookup"><span data-stu-id="326cd-217">`SdkVersion` shows hello version in use for monitoring this app.</span></span>
  * <span data-ttu-id="326cd-218">`LatestAvailableSdkVersion`jelenleg elérhető hello verzió látható hello NuGet gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="326cd-218">`LatestAvailableSdkVersion`shows hello version currently available on hello NuGet gallery.</span></span> <span data-ttu-id="326cd-219">tooupgrade hello toothis Alkalmazásverzió, használjon `Update-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="326cd-219">tooupgrade hello app toothis version, use `Update-ApplicationInsightsMonitoring`.</span></span>

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* <span data-ttu-id="326cd-220">`-Name`az IIS-ben hello alkalmazás hello nevét</span><span class="sxs-lookup"><span data-stu-id="326cd-220">`-Name` hello name of hello app in IIS</span></span>
* <span data-ttu-id="326cd-221">`-InstrumentationKey`az Application Insights-erőforrás hello eredmények toobe jelenik meg, ahová hello ikey hello.</span><span class="sxs-lookup"><span data-stu-id="326cd-221">`-InstrumentationKey` hello ikey of hello Application Insights resource where you want hello results toobe displayed.</span></span>
* <span data-ttu-id="326cd-222">Ez a parancsmag csak olyan alkalmazásokra van hatással, amelyek még nincsenek kialakítva – vagyis amelyek esetében az SdkState==NotInstrumented.</span><span class="sxs-lookup"><span data-stu-id="326cd-222">This cmdlet only affects apps that are not already instrumented - that is, SdkState==NotInstrumented.</span></span>

    <span data-ttu-id="326cd-223">hello parancsmag nem befolyásolja az alkalmazások, amelyek már tagolva.</span><span class="sxs-lookup"><span data-stu-id="326cd-223">hello cmdlet does not affect an app that is already instrumented.</span></span> <span data-ttu-id="326cd-224">Nem számít, hello app build időpontban, hello SDK toohello kódrészletet tagolva, és a futási időt egy előző Ez a parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="326cd-224">It does not matter whether hello app was instrumented at build time by adding hello SDK toohello code, or at run time by a previous use of this cmdlet.</span></span>

    <span data-ttu-id="326cd-225">hello SDK használt verzió tooinstrument hello alkalmazás áll a legutóbb hello verzió letöltött toothis kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="326cd-225">hello SDK version used tooinstrument hello app is hello version that was most recently downloaded toothis server.</span></span>

    <span data-ttu-id="326cd-226">toodownload hello legújabb verziójára, használja a frissítés-ApplicationInsightsVersion.</span><span class="sxs-lookup"><span data-stu-id="326cd-226">toodownload hello latest version, use Update-ApplicationInsightsVersion.</span></span>
* <span data-ttu-id="326cd-227">Siker esetén az `ApplicationInsightsApplication` elemet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="326cd-227">Returns `ApplicationInsightsApplication` on success.</span></span> <span data-ttu-id="326cd-228">Ha nem sikerül, egy nyomkövetési toostderr naplózza.</span><span class="sxs-lookup"><span data-stu-id="326cd-228">If it fails, it logs a trace toostderr.</span></span>

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* <span data-ttu-id="326cd-229">`-Name`az IIS alkalmazás hello neve</span><span class="sxs-lookup"><span data-stu-id="326cd-229">`-Name` hello name of an app in IIS</span></span>
* <span data-ttu-id="326cd-230">`-All` Leállítja minden alkalmazás megfigyelését ezen az IIS-kiszolgálón, amely esetében az `SdkState==EnabledAfterDeployment`</span><span class="sxs-lookup"><span data-stu-id="326cd-230">`-All` Stops monitoring all apps in this IIS server for which `SdkState==EnabledAfterDeployment`</span></span>
* <span data-ttu-id="326cd-231">Leállítja a figyelést hello adott alkalmazást, és eltávolítja a instrumentation.</span><span class="sxs-lookup"><span data-stu-id="326cd-231">Stops monitoring hello specified apps and removes instrumentation.</span></span> <span data-ttu-id="326cd-232">Az alkalmazásokat, amelyek rendelkeznek lettek tagolva futásidejű az eszköz állapotának monitorozása vagy a Start-ApplicationInsightsApplication hello csak működik.</span><span class="sxs-lookup"><span data-stu-id="326cd-232">It only works for apps that have been instrumented at run-time using hello Status Monitoring tool or Start-ApplicationInsightsApplication.</span></span> <span data-ttu-id="326cd-233">(`SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="326cd-233">(`SdkState==EnabledAfterDeployment`)</span></span>
* <span data-ttu-id="326cd-234">Az ApplicationInsightsApplication elemet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="326cd-234">Returns ApplicationInsightsApplication.</span></span>

<span data-ttu-id="326cd-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span><span class="sxs-lookup"><span data-stu-id="326cd-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span></span>

* <span data-ttu-id="326cd-236">`-Name`: az IIS-ben a webes alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="326cd-236">`-Name`: hello name of a web app in IIS.</span></span>
* <span data-ttu-id="326cd-237">`-InstrumentationKey`(Választható.) Használja a toochange hello erőforrás toowhich hello alkalmazás telemetriai zajlik.</span><span class="sxs-lookup"><span data-stu-id="326cd-237">`-InstrumentationKey` (Optional.) Use this toochange hello resource toowhich hello app's telemetry is sent.</span></span>
* <span data-ttu-id="326cd-238">Ez a parancsmag:</span><span class="sxs-lookup"><span data-stu-id="326cd-238">This cmdlet:</span></span>
  * <span data-ttu-id="326cd-239">Frissítések hello nevű alkalmazás toohello verziója hello SDK legutóbb letöltött toothis gép.</span><span class="sxs-lookup"><span data-stu-id="326cd-239">Upgrades hello named app toohello version of hello SDK most recently downloaded toothis machine.</span></span> <span data-ttu-id="326cd-240">(Csak akkor működik, ha `SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="326cd-240">(Only works if `SdkState==EnabledAfterDeployment`)</span></span>
  * <span data-ttu-id="326cd-241">Ha megad egy rendszerállapot-kulcsot, nevű alkalmazás hello újra konfigurált toosend telemetriai toohello erőforrás ezzel a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="326cd-241">If you provide an instrumentation key, hello named app is reconfigured toosend telemetry toohello resource with that key.</span></span> <span data-ttu-id="326cd-242">(Akkor működik, ha `SdkState != Disabled`)</span><span class="sxs-lookup"><span data-stu-id="326cd-242">(Works if `SdkState != Disabled`)</span></span>

`Update-ApplicationInsightsVersion`

* <span data-ttu-id="326cd-243">Letölti a legújabb Application Insights SDK toohello server hello.</span><span class="sxs-lookup"><span data-stu-id="326cd-243">Downloads hello latest Application Insights SDK toohello server.</span></span>

## <span data-ttu-id="326cd-244"><a name="questions"></a>Kérdések az Állapotfigyelővel kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="326cd-244"><a name="questions"></a>Questions about Status Monitor</span></span>

### <a name="what-is-status-monitor"></a><span data-ttu-id="326cd-245">Mi az Állapotfigyelő?</span><span class="sxs-lookup"><span data-stu-id="326cd-245">What is Status Monitor?</span></span>

<span data-ttu-id="326cd-246">Egy asztali alkalmazás, amelyet az IIS-webkiszolgálón kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="326cd-246">A desktop application that you install in your IIS web server.</span></span> <span data-ttu-id="326cd-247">Segít a webalkalmazások kialakításában és konfigurálásában.</span><span class="sxs-lookup"><span data-stu-id="326cd-247">It helps you instrument and configure web apps.</span></span> 

### <a name="when-do-i-use-status-monitor"></a><span data-ttu-id="326cd-248">Mire használhatom az Állapotfigyelőt?</span><span class="sxs-lookup"><span data-stu-id="326cd-248">When do I use Status Monitor?</span></span>

* <span data-ttu-id="326cd-249">a webalkalmazás, hogy fut az IIS-kiszolgáló - még akkor is, ha már fut. tooinstrument.</span><span class="sxs-lookup"><span data-stu-id="326cd-249">tooinstrument any web app that is running on your IIS server - even if it is already running.</span></span>
* <span data-ttu-id="326cd-250">tooenable további telemetriai adatokat is a web Apps [hello Application Insights SDK-val készült](app-insights-asp-net.md) fordítás során.</span><span class="sxs-lookup"><span data-stu-id="326cd-250">tooenable additional telemetry for web apps that have been [built with hello Application Insights SDK](app-insights-asp-net.md) at compile time.</span></span> 

### <a name="can-i-close-it-after-it-runs"></a><span data-ttu-id="326cd-251">A futtatás után bezárhatom?</span><span class="sxs-lookup"><span data-stu-id="326cd-251">Can I close it after it runs?</span></span>

<span data-ttu-id="326cd-252">Igen.</span><span class="sxs-lookup"><span data-stu-id="326cd-252">Yes.</span></span> <span data-ttu-id="326cd-253">Ha az rendelkezik tagolva hello webhelyek választja, bezárhatja azt.</span><span class="sxs-lookup"><span data-stu-id="326cd-253">After it has instrumented hello websites you select, you can close it.</span></span>

<span data-ttu-id="326cd-254">Az alkalmazás önmagától nem gyűjt telemetriai adatokat,</span><span class="sxs-lookup"><span data-stu-id="326cd-254">It doesn't collect telemetry by itself.</span></span> <span data-ttu-id="326cd-255">Ebben az esetben hello webalkalmazások konfigurálja, és néhány engedélyeket állít be.</span><span class="sxs-lookup"><span data-stu-id="326cd-255">It just configures hello web apps and sets some permissions.</span></span>

### <a name="what-does-status-monitor-do"></a><span data-ttu-id="326cd-256">Milyen műveleteket hajt végre az Állapotfigyelő?</span><span class="sxs-lookup"><span data-stu-id="326cd-256">What does Status Monitor do?</span></span>

<span data-ttu-id="326cd-257">Ha bejelöli az állapotfigyelő tooinstrument webalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="326cd-257">When you select a web app for Status Monitor tooinstrument:</span></span>

* <span data-ttu-id="326cd-258">Letölti és hello Application Insights szerelvények és .config fájl helyezi hello webes alkalmazás bináris fájljainak mappáját.</span><span class="sxs-lookup"><span data-stu-id="326cd-258">Downloads and places hello Application Insights assemblies and .config file in hello web app's binaries folder.</span></span>
* <span data-ttu-id="326cd-259">Módosítja a `web.config` tooadd hello Application Insights HTTP követési modul.</span><span class="sxs-lookup"><span data-stu-id="326cd-259">Modifies `web.config` tooadd hello Application Insights HTTP tracking module.</span></span>
* <span data-ttu-id="326cd-260">Lehetővé teszi, hogy a CLR-profilkészítés toocollect függőségi hívások esetében.</span><span class="sxs-lookup"><span data-stu-id="326cd-260">Enables CLR profiling toocollect dependency calls.</span></span>

### <a name="do-i-need-toorun-status-monitor-whenever-i-update-hello-app"></a><span data-ttu-id="326cd-261">Állapotfigyelő toorun amikor hello alkalmazást frissíteni kell?</span><span class="sxs-lookup"><span data-stu-id="326cd-261">Do I need toorun Status Monitor whenever I update hello app?</span></span>

<span data-ttu-id="326cd-262">Nem, ha az ismételt üzembe helyezés növekményesen történik.</span><span class="sxs-lookup"><span data-stu-id="326cd-262">Not if you redeploy incrementally.</span></span> 

<span data-ttu-id="326cd-263">Ha hello "törölje a meglévő fájlokat" lehetőséget választja a hello folyamat közzétételéhez toore futtatási állapotfigyelő tooconfigure Application Insights kell.</span><span class="sxs-lookup"><span data-stu-id="326cd-263">If you select hello 'delete existing files' option in hello publish process, you would need toore-run Status Monitor tooconfigure Application Insights.</span></span>

### <a name="what-telemetry-is-collected"></a><span data-ttu-id="326cd-264">A rendszer milyen telemetriai adatokat gyűjt?</span><span class="sxs-lookup"><span data-stu-id="326cd-264">What telemetry is collected?</span></span>

<span data-ttu-id="326cd-265">Olyan alkalmazások esetén, amelyeket az Állapotfigyelővel kizárólag futásidőben állít be:</span><span class="sxs-lookup"><span data-stu-id="326cd-265">For applications that you instrument only at run-time by using Status Monitor:</span></span>

* <span data-ttu-id="326cd-266">HTTP-kérések</span><span class="sxs-lookup"><span data-stu-id="326cd-266">HTTP requests</span></span>
* <span data-ttu-id="326cd-267">Meghívja a toodependencies</span><span class="sxs-lookup"><span data-stu-id="326cd-267">Calls toodependencies</span></span>
* <span data-ttu-id="326cd-268">Kivételek</span><span class="sxs-lookup"><span data-stu-id="326cd-268">Exceptions</span></span>
* <span data-ttu-id="326cd-269">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="326cd-269">Performance counters</span></span>

<span data-ttu-id="326cd-270">A fordítási során már kiépített alkalmazások esetén:</span><span class="sxs-lookup"><span data-stu-id="326cd-270">For applications already instrumented at compile time:</span></span>

 * <span data-ttu-id="326cd-271">Folyamatszámlálók.</span><span class="sxs-lookup"><span data-stu-id="326cd-271">Process counters.</span></span>
 * <span data-ttu-id="326cd-272">Függőségi hívások (.NET 4.5); függőségi hívásokban visszaadott értékek (.NET 4.6).</span><span class="sxs-lookup"><span data-stu-id="326cd-272">Dependency calls (.NET 4.5); return values in dependency calls (.NET 4.6).</span></span>
 * <span data-ttu-id="326cd-273">Kivételhíváslánc-értékek.</span><span class="sxs-lookup"><span data-stu-id="326cd-273">Exception stack trace values.</span></span>

[<span data-ttu-id="326cd-274">További információ</span><span class="sxs-lookup"><span data-stu-id="326cd-274">Learn more</span></span>](http://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a><span data-ttu-id="326cd-275">Videó</span><span class="sxs-lookup"><span data-stu-id="326cd-275">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <span data-ttu-id="326cd-276"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="326cd-276"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="326cd-277">A telemetriai adatok megtekintése:</span><span class="sxs-lookup"><span data-stu-id="326cd-277">View your telemetry:</span></span>

* <span data-ttu-id="326cd-278">[Megismerkedhet a metrikák](app-insights-metrics-explorer.md) toomonitor teljesítmény- és használati</span><span class="sxs-lookup"><span data-stu-id="326cd-278">[Explore metrics](app-insights-metrics-explorer.md) toomonitor performance and usage</span></span>
* <span data-ttu-id="326cd-279">[Keresést az események és a naplók] [ diagnostic] toodiagnose problémák</span><span class="sxs-lookup"><span data-stu-id="326cd-279">[Search events and logs][diagnostic] toodiagnose problems</span></span>
* <span data-ttu-id="326cd-280">[Elemzések](app-insights-analytics.md) az összetettebb lekérdezésekhez</span><span class="sxs-lookup"><span data-stu-id="326cd-280">[Analytics](app-insights-analytics.md) for more advanced queries</span></span>
* [<span data-ttu-id="326cd-281">Irányítópultok létrehozása</span><span class="sxs-lookup"><span data-stu-id="326cd-281">Create dashboards</span></span>](app-insights-dashboards.md)

<span data-ttu-id="326cd-282">További telemetriai funkciók hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="326cd-282">Add more telemetry:</span></span>

* <span data-ttu-id="326cd-283">[Webteszt létrehozása] [ availability] toomake meg arról, hogy a hely élő marad.</span><span class="sxs-lookup"><span data-stu-id="326cd-283">[Create web tests][availability] toomake sure your site stays live.</span></span>
* <span data-ttu-id="326cd-284">[Adja hozzá a webes ügyfél telemetriai] [ usage] weblap kódot és beszúrása toolet toosee kivételei nyomkövetési hívások.</span><span class="sxs-lookup"><span data-stu-id="326cd-284">[Add web client telemetry][usage] toosee exceptions from web page code and toolet you insert trace calls.</span></span>
* <span data-ttu-id="326cd-285">[Application Insights SDK tooyour kódot] [ greenbrown] , hogy helyezze be a nyomkövetést, és jelentkezzen hívások</span><span class="sxs-lookup"><span data-stu-id="326cd-285">[Add Application Insights SDK tooyour code][greenbrown] so that you can insert trace and log calls</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-javascript.md

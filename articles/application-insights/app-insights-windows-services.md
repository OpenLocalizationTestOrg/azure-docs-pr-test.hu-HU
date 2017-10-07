---
title: "aaaAzure Application Insights for Windows server és a feldolgozói szerepkörök |} Microsoft Docs"
description: "Adja hozzá manuálisan a hello Application Insights SDK tooyour ASP.NET alkalmazások tooanalyze használatát, rendelkezésre állását és teljesítményét."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 64643ef637195d10f87fc6020a77169bca66c1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manually-configure-application-insights-for-net-applications"></a><span data-ttu-id="d4b18-103">Az Application Insights manuális beállítása a .NET-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="d4b18-103">Manually configure Application Insights for .NET applications</span></span>

<span data-ttu-id="d4b18-104">Konfigurálható [Application Insights](app-insights-overview.md) toomonitor számos alkalmazások vagy alkalmazás-szerepkörök, összetevők és mikroszolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d4b18-104">You can configure [Application Insights](app-insights-overview.md) toomonitor a wide variety of applications or application roles, components, or microservices.</span></span> <span data-ttu-id="d4b18-105">A webalkalmazásokhoz és -szolgáltatásokhoz a Visual Studio [egylépéses konfigurációs lehetőséget ](app-insights-asp-net.md) biztosít.</span><span class="sxs-lookup"><span data-stu-id="d4b18-105">For web apps and services, Visual Studio offers [one-step configuration](app-insights-asp-net.md).</span></span> <span data-ttu-id="d4b18-106">Más típusú .NET-alkalmazásokhoz, például a háttérkiszolgálói szerepkörökhöz vagy az asztali alkalmazásokhoz beállíthatja manuálisan az Application Insightsot.</span><span class="sxs-lookup"><span data-stu-id="d4b18-106">For other types of .NET application, such as backend server roles or desktop applications, you can configure Application Insights manually.</span></span>

![Példa teljesítményfigyelő diagramokra](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a><span data-ttu-id="d4b18-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="d4b18-108">Before you start</span></span>

<span data-ttu-id="d4b18-109">A következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="d4b18-109">You need:</span></span>

* <span data-ttu-id="d4b18-110">Előfizetés túl[Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="d4b18-110">A subscription too[Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="d4b18-111">Ha a csapat vagy szervezet Azure-előfizetéssel, hello tulajdonosa adhat hozzá, tooit, használja a [Microsoft-fiók](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="d4b18-111">If your team or organization has an Azure subscription, hello owner can add you tooit, using your [Microsoft account](http://live.com).</span></span>
* <span data-ttu-id="d4b18-112">Visual Studio 2013 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d4b18-112">Visual Studio 2013 or later.</span></span>

## <span data-ttu-id="d4b18-113"><a name="add"></a>1. Application Insights-erőforrások választása</span><span class="sxs-lookup"><span data-stu-id="d4b18-113"><a name="add"></a>1. Choose an Application Insights resource</span></span>

<span data-ttu-id="d4b18-114">hello "resource", ahol az adatok gyűjtése és hello Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d4b18-114">hello 'resource' is where your data is collected and displayed in hello Azure portal.</span></span> <span data-ttu-id="d4b18-115">E szüksége toodecide toocreate egy új, vagy egy meglévő fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="d4b18-115">You need toodecide whether toocreate a new one, or share an existing one.</span></span>

### <a name="part-of-a-larger-app-use-existing-resource"></a><span data-ttu-id="d4b18-116">Egy nagyobb alkalmazás része: létező erőforrás használata</span><span class="sxs-lookup"><span data-stu-id="d4b18-116">Part of a larger app: Use existing resource</span></span>

<span data-ttu-id="d4b18-117">Ha a webes alkalmazás több összetevői – például egy előtér-webalkalmazást és egy vagy több háttér-szolgáltatásaihoz - vannak, akkor a telemetriai adatokat küldjön az összes hello összetevők toohello ugyanazt az erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d4b18-117">If your web application has several components - for example, a front-end web app and one or more back-end services - then you should send telemetry from all hello components toohello same resource.</span></span> <span data-ttu-id="d4b18-118">Ezzel lehetővé teszi egy önálló alkalmazás térképen toobe, és könnyebben lehetséges tootrace egy összetevő tooanother kérése.</span><span class="sxs-lookup"><span data-stu-id="d4b18-118">This will enable them toobe displayed on a single Application Map, and make it possible tootrace a request from one component tooanother.</span></span>

<span data-ttu-id="d4b18-119">Tehát ha most már figyelés az alkalmazás más összetevői, majd csak használata hello azonos erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d4b18-119">So, if you're already monitoring other components of this app, then just use hello same resource.</span></span>

<span data-ttu-id="d4b18-120">Nyissa meg a hello erőforrás hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d4b18-120">Open hello resource in hello [Azure portal](https://portal.azure.com/).</span></span> 

### <a name="self-contained-app-create-a-new-resource"></a><span data-ttu-id="d4b18-121">Önálló alkalmazás: Új erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d4b18-121">Self-contained app: Create a new resource</span></span>

<span data-ttu-id="d4b18-122">Ha új alkalmazás hello független tooother alkalmazások, azt a saját erőforrás kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="d4b18-122">If hello new app is unrelated tooother applications, then it should have its own resource.</span></span>

<span data-ttu-id="d4b18-123">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/), és hozzon létre egy új Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d4b18-123">Sign in toohello [Azure portal](https://portal.azure.com/), and create a new Application Insights resource.</span></span> <span data-ttu-id="d4b18-124">Válassza ki az ASP.NET hello alkalmazás típusként.</span><span class="sxs-lookup"><span data-stu-id="d4b18-124">Choose ASP.NET as hello application type.</span></span>

![Kattintson az Új, majd az Application Insights lehetőségre](./media/app-insights-windows-services/01-new-asp.png)

<span data-ttu-id="d4b18-126">hello választott alkalmazástípus hello erőforrás paneleken tartalmának hello alapértelmezett beállítása.</span><span class="sxs-lookup"><span data-stu-id="d4b18-126">hello choice of application type sets hello default content of hello resource blades.</span></span>

## <a name="2-copy-hello-instrumentation-key"></a><span data-ttu-id="d4b18-127">2. Hello Instrumentation kulcs másolása</span><span class="sxs-lookup"><span data-stu-id="d4b18-127">2. Copy hello Instrumentation Key</span></span>
<span data-ttu-id="d4b18-128">hello kulcs hello erőforrás azonosítja.</span><span class="sxs-lookup"><span data-stu-id="d4b18-128">hello key identifies hello resource.</span></span> <span data-ttu-id="d4b18-129">Lesz telepíti, amint az SDK-val hello rendelés toodirect adatok toohello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d4b18-129">You'll install it soon in hello SDK, in order toodirect data toohello resource.</span></span>

![Kattintson a Tulajdonságok parancsra, válassza ki a hello kulcs, használja a ctrl + C](./media/app-insights-windows-services/02-props-asp.png)

## <span data-ttu-id="d4b18-131"><a name="sdk"></a>3. Az alkalmazás hello Application Insights csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="d4b18-131"><a name="sdk"></a>3. Install hello Application Insights package in your application</span></span>
<span data-ttu-id="d4b18-132">Telepítése és konfigurálása hello Application Insights csomagot dolgozik hello platform függ.</span><span class="sxs-lookup"><span data-stu-id="d4b18-132">Installing and configuring hello Application Insights package varies depending on hello platform you're working on.</span></span> 

1. <span data-ttu-id="d4b18-133">A Visual Studióban kattintson a jobb gombbal a projektjére, és válassza a **Manage NuGet Packages (NuGet-csomagok kezelése)** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d4b18-133">In Visual Studio, right-click your project and choose **Manage Nuget Packages**.</span></span>
   
    ![Kattintson a jobb gombbal a hello projektet, és válassza ki a Nuget-csomagok kezelése](./media/app-insights-windows-services/03-nuget.png)
2. <span data-ttu-id="d4b18-135">Telepítse az Application Insights csomagot hello Windows server-alkalmazások esetén "Microsoft.ApplicationInsights.WindowsServer."</span><span class="sxs-lookup"><span data-stu-id="d4b18-135">Install hello Application Insights package for Windows server apps, "Microsoft.ApplicationInsights.WindowsServer."</span></span>
   
    ![Az „Application Insights” kifejezés keresése](./media/app-insights-windows-services/04-ai-nuget.png)
   
    <span data-ttu-id="d4b18-137">*Melyik verzió?*</span><span class="sxs-lookup"><span data-stu-id="d4b18-137">*Which version?*</span></span>

    <span data-ttu-id="d4b18-138">Ellenőrizze **közé tartoznak az előzetes** Ha azt szeretné, tootry a legújabb funkciókat.</span><span class="sxs-lookup"><span data-stu-id="d4b18-138">Check **Include prerelease** if you want tootry our latest features.</span></span> <span data-ttu-id="d4b18-139">hello dokumentumokat és blogok vegye figyelembe, hogy szükséges-e előzetes verzióját.</span><span class="sxs-lookup"><span data-stu-id="d4b18-139">hello relevant documents or blogs note whether you need a prerelease version.</span></span>
    
    <span data-ttu-id="d4b18-140">*Használhatok más csomagokat is?*</span><span class="sxs-lookup"><span data-stu-id="d4b18-140">*Can I use other packages?*</span></span>
   
    <span data-ttu-id="d4b18-141">Igen.</span><span class="sxs-lookup"><span data-stu-id="d4b18-141">Yes.</span></span> <span data-ttu-id="d4b18-142">Ha azt szeretné csak toouse hello API toosend saját telemetriai, válassza a "Microsoft.ApplicationInsights".</span><span class="sxs-lookup"><span data-stu-id="d4b18-142">Choose "Microsoft.ApplicationInsights" if you only want toouse hello API toosend your own telemetry.</span></span> <span data-ttu-id="d4b18-143">hello Windows Server csomag hello API mellett egyéb csomagok, például a teljesítményszámlálók gyűjteményét a és a függőségi figyelő egy száma is.</span><span class="sxs-lookup"><span data-stu-id="d4b18-143">hello Windows Server package includes hello API plus a number of other packages such as performance counter collection and dependency monitoring.</span></span> 

### <a name="tooupgrade-toofuture-package-versions"></a><span data-ttu-id="d4b18-144">tooupgrade toofuture alkalmazáscsomag-verziók</span><span class="sxs-lookup"><span data-stu-id="d4b18-144">tooupgrade toofuture package versions</span></span>
<span data-ttu-id="d4b18-145">Azt a kiadási hello SDK idő tootime az új verzióját.</span><span class="sxs-lookup"><span data-stu-id="d4b18-145">We release a new version of hello SDK from time tootime.</span></span>

<span data-ttu-id="d4b18-146">tooupgrade tooa [hello csomag új kiadási](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), nyissa meg újra a NuGet-Csomagkezelőt, és a telepített csomagok szűrésére.</span><span class="sxs-lookup"><span data-stu-id="d4b18-146">tooupgrade tooa [new release of hello package](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), open NuGet package manager again and filter on installed packages.</span></span> <span data-ttu-id="d4b18-147">Jelölje ki a **Microsoft.ApplicationInsights.WindowsServer** lehetőséget, és válassza az **Upgrade** (Frissítés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d4b18-147">Select **Microsoft.ApplicationInsights.WindowsServer** and choose **Upgrade**.</span></span>

<span data-ttu-id="d4b18-148">Ha végzett a testreszabások tooApplicationInsights.config, egy példányának mentése, frissítése, és ezt követően a változtatások egyesítése hello új verzió előtt.</span><span class="sxs-lookup"><span data-stu-id="d4b18-148">If you made any customizations tooApplicationInsights.config, save a copy of it before you upgrade, and afterwards merge your changes into hello new version.</span></span>

## <a name="4-send-telemetry"></a><span data-ttu-id="d4b18-149">4. Telemetria küldése</span><span class="sxs-lookup"><span data-stu-id="d4b18-149">4. Send telemetry</span></span>
<span data-ttu-id="d4b18-150">**Ha csak hello API csomag telepítése:**</span><span class="sxs-lookup"><span data-stu-id="d4b18-150">**If you installed only hello API package:**</span></span>

* <span data-ttu-id="d4b18-151">Beállíthat hello instrumentation kulcs a kódban, például `main()`:</span><span class="sxs-lookup"><span data-stu-id="d4b18-151">Set hello instrumentation key in code, for example in `main()`:</span></span> 
  
    <span data-ttu-id="d4b18-152">`TelemetryConfiguration.Active.InstrumentationKey = "`*az Ön kulcsa*`";`</span><span class="sxs-lookup"><span data-stu-id="d4b18-152">`TelemetryConfiguration.Active.InstrumentationKey = "` *your key* `";`</span></span> 
* <span data-ttu-id="d4b18-153">[Saját API-jával hello telemetriai írási](app-insights-api-custom-events-metrics.md#ikey).</span><span class="sxs-lookup"><span data-stu-id="d4b18-153">[Write your own telemetry using hello API](app-insights-api-custom-events-metrics.md#ikey).</span></span>

<span data-ttu-id="d4b18-154">**Ha telepítette a többi Application Insights csomagot** tetszés szerint használhatja hello .config fájl tooset hello instrumentation kulcs:</span><span class="sxs-lookup"><span data-stu-id="d4b18-154">**If you installed other Application Insights packages,** you can, if you prefer, use hello .config file tooset hello instrumentation key:</span></span>

* <span data-ttu-id="d4b18-155">Szerkessze az ApplicationInsights.config (amely hello NuGet telepítése felvették).</span><span class="sxs-lookup"><span data-stu-id="d4b18-155">Edit ApplicationInsights.config (which was added by hello NuGet install).</span></span> <span data-ttu-id="d4b18-156">Szúrja be a záró címke hello előtt:</span><span class="sxs-lookup"><span data-stu-id="d4b18-156">Insert this just before hello closing tag:</span></span>
  
    <span data-ttu-id="d4b18-157">`<InstrumentationKey>`*hello instrumentation kulcs másolt*`</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="d4b18-157">`<InstrumentationKey>` *hello instrumentation key you copied* `</InstrumentationKey>`</span></span>
* <span data-ttu-id="d4b18-158">Győződjön meg arról, hogy túl van-e beállítva a hello tulajdonságait a Solution Explorer ApplicationInsights.config**Build művelet = tartalom másolása tooOutput Directory másolási =**.</span><span class="sxs-lookup"><span data-stu-id="d4b18-158">Make sure that hello properties of ApplicationInsights.config in Solution Explorer are set too**Build Action = Content, Copy tooOutput Directory = Copy**.</span></span>

<span data-ttu-id="d4b18-159">Ha azt szeretné, hogy túl hasznos tooset hello instrumentation kulcs kódban[kapcsoló hello kulcs eltérő konfigurációk](app-insights-separate-resources.md).</span><span class="sxs-lookup"><span data-stu-id="d4b18-159">It's useful tooset hello instrumentation key in code if you want too[switch hello key for different build configurations](app-insights-separate-resources.md).</span></span> <span data-ttu-id="d4b18-160">Hello kulcs kód állítja be, ha nincs tooset legyen hello `.config` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d4b18-160">If you set hello key in code, you don't have tooset it in hello `.config` file.</span></span>

## <span data-ttu-id="d4b18-161"><a name="run"></a> A projekt futtatása</span><span class="sxs-lookup"><span data-stu-id="d4b18-161"><a name="run"></a> Run your project</span></span>
<span data-ttu-id="d4b18-162">Használjon hello **F5** toorun az alkalmazás, és próbálja ki: különböző nyílt lapok toogenerate néhány telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="d4b18-162">Use hello **F5** toorun your application and try it out: open different pages toogenerate some telemetry.</span></span>

<span data-ttu-id="d4b18-163">A Visual Studio látni fogja, az elküldött hello események száma.</span><span class="sxs-lookup"><span data-stu-id="d4b18-163">In Visual Studio, you'll see a count of hello events that have been sent.</span></span>

![Események száma a Visual Studióban](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <span data-ttu-id="d4b18-165"><a name="monitor"></a> A telemetriai adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="d4b18-165"><a name="monitor"></a> View your telemetry</span></span>
<span data-ttu-id="d4b18-166">Térjen vissza a toohello [Azure-portálon](https://portal.azure.com/) , és keresse meg a tooyour Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d4b18-166">Return toohello [Azure portal](https://portal.azure.com/) and browse tooyour Application Insights resource.</span></span>

<span data-ttu-id="d4b18-167">Keresse meg hello áttekintő diagramok adatokat.</span><span class="sxs-lookup"><span data-stu-id="d4b18-167">Look for data in hello Overview charts.</span></span> <span data-ttu-id="d4b18-168">Először csak egy vagy két pontot lát.</span><span class="sxs-lookup"><span data-stu-id="d4b18-168">At first, you'll just see one or two points.</span></span> <span data-ttu-id="d4b18-169">Példa:</span><span class="sxs-lookup"><span data-stu-id="d4b18-169">For example:</span></span>

![Kattintson a toomore adatok](./media/app-insights-windows-services/12-first-perf.png)

<span data-ttu-id="d4b18-171">Kattintson a diagram toosee keresztül metrikák részletes.</span><span class="sxs-lookup"><span data-stu-id="d4b18-171">Click through any chart toosee more detailed metrics.</span></span> [<span data-ttu-id="d4b18-172">További információk a metrikákról.</span><span class="sxs-lookup"><span data-stu-id="d4b18-172">Learn more about metrics.</span></span>](app-insights-web-monitor-performance.md)

### <a name="no-data"></a><span data-ttu-id="d4b18-173">Nincs adat?</span><span class="sxs-lookup"><span data-stu-id="d4b18-173">No data?</span></span>
* <span data-ttu-id="d4b18-174">Hello alkalmazást, a különböző oldalakhoz megnyitása, hogy néhány telemetriai generál használni.</span><span class="sxs-lookup"><span data-stu-id="d4b18-174">Use hello application, opening different pages so that it generates some telemetry.</span></span>
* <span data-ttu-id="d4b18-175">Nyissa meg hello [keresési](app-insights-diagnostic-search.md) csempe, toosee események.</span><span class="sxs-lookup"><span data-stu-id="d4b18-175">Open hello [Search](app-insights-diagnostic-search.md) tile, toosee individual events.</span></span> <span data-ttu-id="d4b18-176">Egyes esetekben szükséges események közben hosszabb egy kis tooget hello metrikák-feldolgozási folyamaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="d4b18-176">Sometimes it takes events a little while longer tooget through hello metrics pipeline.</span></span>
* <span data-ttu-id="d4b18-177">Várjon néhány másodpercet, és kattintson a **Frissítés** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="d4b18-177">Wait a few seconds and click **Refresh**.</span></span> <span data-ttu-id="d4b18-178">Diagramok rendszeresen frissítse magát, de is frissítheti manuálisan Ha eredménykészletre várakozik egyes adatok tooshow.</span><span class="sxs-lookup"><span data-stu-id="d4b18-178">Charts refresh themselves periodically, but you can refresh manually if you're waiting for some data tooshow up.</span></span>
* <span data-ttu-id="d4b18-179">Lásd: [Hibaelhárítás](app-insights-troubleshoot-faq.md).</span><span class="sxs-lookup"><span data-stu-id="d4b18-179">See [Troubleshooting](app-insights-troubleshoot-faq.md).</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="d4b18-180">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="d4b18-180">Publish your app</span></span>
<span data-ttu-id="d4b18-181">Most már telepítheti az alkalmazáskiszolgáló tooyour, vagy tooAzure és figyelési hello adat gyűlik össze.</span><span class="sxs-lookup"><span data-stu-id="d4b18-181">Now deploy your application tooyour server or tooAzure and watch hello data accumulate.</span></span>

![Visual Studio toopublish az alkalmazás használata](./media/app-insights-windows-services/15-publish.png)

<span data-ttu-id="d4b18-183">Ha hibakeresési módban futtatja, telemetriai végezhető hello-feldolgozási folyamaton keresztül, hogy másodpercen belül szereplő adatokat kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="d4b18-183">When you run in debug mode, telemetry is expedited through hello pipeline, so that you should see data appearing within seconds.</span></span> <span data-ttu-id="d4b18-184">Ha Kiadás konfigurációban telepíti az alkalmazását, az adatok lassabban gyűlnek.</span><span class="sxs-lookup"><span data-stu-id="d4b18-184">When you deploy your app in Release configuration, data accumulates more slowly.</span></span>

### <a name="no-data-after-you-publish-tooyour-server"></a><span data-ttu-id="d4b18-185">Nincs adat tooyour server közzététele után?</span><span class="sxs-lookup"><span data-stu-id="d4b18-185">No data after you publish tooyour server?</span></span>
<span data-ttu-id="d4b18-186">Nyissa meg a portokat a kimenő forgalom számára a kiszolgáló tűzfalán.</span><span class="sxs-lookup"><span data-stu-id="d4b18-186">Open ports for outgoing traffic in your server's firewall.</span></span> <span data-ttu-id="d4b18-187">Lásd: [ezen a lapon](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) szükséges címek hello listája</span><span class="sxs-lookup"><span data-stu-id="d4b18-187">See [this page](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) for hello list of required addresses</span></span> 

### <a name="trouble-on-your-build-server"></a><span data-ttu-id="d4b18-188">Probléma adódott a lemezképfájl-kiszolgálóján?</span><span class="sxs-lookup"><span data-stu-id="d4b18-188">Trouble on your build server?</span></span>
<span data-ttu-id="d4b18-189">Tekintse meg [ezt a Hibaelhárítási cikket](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).</span><span class="sxs-lookup"><span data-stu-id="d4b18-189">Please see [this Troubleshooting item](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).</span></span>

> [!NOTE]
> <span data-ttu-id="d4b18-190">Az alkalmazás nagy mennyiségű telemetriai adatokat állít elő, ha hello adaptív mintavételi modul automatikusan toohello portal reprezentatív része események küldése által küldött hello kötet csökkenti.</span><span class="sxs-lookup"><span data-stu-id="d4b18-190">If your app generates a lot of telemetry, hello adaptive sampling module will automatically reduce hello volume that is sent toohello portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="d4b18-191">Azonban események, amelyek kapcsolódó toohello kérésben lesz kiválasztva vagy nincs kijelölve csoportosan, hogy a kapcsolódó események közti léphet.</span><span class="sxs-lookup"><span data-stu-id="d4b18-191">However, events that are related toohello same request will be selected or deselected as a group, so that you can navigate between related events.</span></span> 
> <span data-ttu-id="d4b18-192">[Ismerkedés a mintavételezéssel](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="d4b18-192">[Learn about sampling](app-insights-sampling.md).</span></span>
> 
> 

## <a name="video"></a><span data-ttu-id="d4b18-193">Videó</span><span class="sxs-lookup"><span data-stu-id="d4b18-193">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="d4b18-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d4b18-194">Next steps</span></span>
* <span data-ttu-id="d4b18-195">[Adja hozzá a további telemetriai](app-insights-asp-net-more.md) tooget hello 360 fok nézet az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d4b18-195">[Add more telemetry](app-insights-asp-net-more.md) tooget hello full 360-degree view of your application.</span></span>

